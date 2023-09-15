# Tomcat
## Learn Apache Tomcat Deep Dive

### To check the status of the Apache HTTP server (httpd) using an Ansible playbook, you can use the Ansible "command" or "shell" module to run the appropriate command on the target server. Here's a simple example of how to do this:

1. Create an Ansible playbook, e.g., `check_httpd_status.yml`.

- name: Check HTTPD Status
  hosts: your_target_server
  tasks:
    - name: Check HTTPD Status
      shell: systemctl is-active httpd
      register: httpd_status
    - name: Display HTTPD Status
      debug:
        var: httpd_status.stdout


In this playbook:

- `hosts` should be set to the target server(s) where you want to check the HTTPD status.

- The `shell` module is used to run the command `systemctl is-active httpd`, which checks the status of the HTTPD service using `systemctl`.

- The `register` directive is used to capture the output of the command in a variable named `httpd_status`.

- The `debug` module is used to display the result of the `systemctl is-active httpd` command, which is stored in the `httpd_status.stdout` variable.

2. Run the playbook using the `ansible-playbook` command:

```bash
ansible-playbook check_httpd_status.yml
```

Replace `your_target_server` with the actual hostname or IP address of the server you want to check.

The playbook will connect to the target server(s), run the command to check the HTTPD status, and then display the status in the output.

Make sure you have SSH access and appropriate permissions to run commands on the target server(s) using Ansible.
