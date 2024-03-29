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


ansible-playbook check_httpd_status.yml


Replace `your_target_server` with the actual hostname or IP address of the server you want to check.

The playbook will connect to the target server(s), run the command to check the HTTPD status, and then display the status in the output.

Make sure you have SSH access and appropriate permissions to run commands on the target server(s) using Ansible

# Issue: fatal: [remote@172.31.47.58]: FAILED! => {"changed": true, "cmd": "systemctl is-active httpd", "delta": "0:00:00.042619", "end": "2023-09-15 09:58:25.305827", "msg": "non-zero return code", "rc": 3, "start": "2023-09-15 09:58:25.263208", "stderr": "", "stderr_lines": [], "stdout": "unknown", "stdout_lines": ["unknown"]}

##  The "unknown" status returned by the `systemctl is-active httpd` command usually means that the Apache HTTP server (`httpd`) is not installed on the target server or that it's not recognized as a valid service by `systemctl`. Here are some troubleshooting steps to address this issue:

1. Ensure Apache HTTP Server (`httpd`) is installed:
   - Make sure that Apache HTTP Server is installed on the target server. You can do this manually or by using Ansible to install it. For example, you can use Ansible's package module to install Apache HTTP Server if it's not already installed.

   - name: Install Apache HTTP Server
     hosts: your_target_server
     tasks:
       - name: Install Apache HTTP Server
         package:
           name: httpd
           state: present


   Replace `your_target_server` with your actual server information.

2. Confirm the service name:
   - Double-check that the service name for Apache HTTP Server is `httpd`. This may vary depending on your Linux distribution. For example, on some systems, it might be `apache2` or `httpd.service`. You can use the `systemctl list-units --type=service` command to list all available services.

3. Ensure proper privileges:
   - Make sure that the user running the Ansible playbook has the necessary privileges to run the `systemctl` command. It might require elevated privileges, so ensure that you are running the playbook with the appropriate user or using `become` in your playbook to escalate privileges.

   ```yaml
   - name: Check HTTPD Status
     hosts: your_target_server
     become: yes
     tasks:
       - name: Check HTTPD Status
         command: systemctl is-active httpd
         register: httpd_status
       - name: Display HTTPD Status
         debug:
           var: httpd_status.stdout
   ```

4. Check for alternative service names:
   - As mentioned earlier, the service name for Apache HTTP Server may vary depending on the Linux distribution. Try running `systemctl list-units --type=service` on the target server to identify the correct service name.

5. Verify service status:
   - After confirming the correct service name, you can use the following Ansible playbook task to check the status of the service:

   ```yaml
   - name: Check HTTPD Status
     hosts: your_target_server
     tasks:
       - name: Check HTTPD Status
         service:
           name: httpd   # Replace with the correct service name if needed
           state: status
           enabled: yes
         register: httpd_status
       - name: Display HTTPD Status
         debug:
           var: httpd_status
   ```

Make sure to adapt these steps to your specific server configuration and distribution. The key is to ensure that the correct service name is used and that Ansible has the necessary privileges to query the service status.
