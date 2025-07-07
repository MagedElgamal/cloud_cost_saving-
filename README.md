---
- name: Check unhealthy load balancers and send to Slack
  hosts: localhost
  gather_facts: no
  tasks:
    - name: List global backend services
      shell: >
        gcloud compute backend-services list --global --format="value(name)"
      register: backend_services
      environment:
        GOOGLE_APPLICATION_CREDENTIALS: "{{ gcp_terraform_sa }}"

    - name: Check health status of each backend service
      shell: |
        backend_type=$(gcloud compute backend-services describe {{ item }} --global --format="value(backends[0].group)")
        if [[ $backend_type == *"serverless"* ]]; then
          echo "Skipping serverless backend: {{ item }}"
        else
          status=$(gcloud compute backend-services get-health {{ item }} --global --format="value(status.healthStatus[0].healthState)")
          if [[ $? -ne 0 ]]; then
            echo "Error checking {{ item }}"
            exit 1
          fi
          if [[ $status != "HEALTHY" ]]; then
            echo "| Global LB     | {{ item }}        | N/A          | $status |"
          fi
        fi
      with_items: "{{ backend_services.stdout_lines }}"
      register: health_statuses
      failed_when: health_statuses.rc != 0
      ignore_errors: true
      environment:
        GOOGLE_APPLICATION_CREDENTIALS: "{{ gcp_terraform_sa }}"

    - name: Send unhealthy backend services to Slack
      slack:
        token: "{{ slack_token }}"
        msg: |
          Unhealthy Load Balancers:
          -------------------------------------------------------------------------
          | Load Balancer | Backend Service | Region        | Status        |
          -------------------------------------------------------------------------
          {% for item in health_statuses.results %}
          {% if item.stdout %}
          {{ item.stdout }}
          {% endif %}
          {% endfor %}
      when: health_statuses.results | selectattr('stdout') | list | length > 0
