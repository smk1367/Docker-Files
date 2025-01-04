### README for the Dockerfile

#### Purpose
This Dockerfile creates a custom Ubuntu 20.04-based container for testing and deploying automation tools like Ansible with Kerberos authentication and WinRM support. It is configured to include an SSH server and essential packages for network utilities and Python development.

---

#### Features
1. **Base Image**: Ubuntu 20.04
2. **Installed Tools**:
   - GCC, Python development tools, and Kerberos libraries.
   - Python 3 and pip3 for package management.
   - `pywinrm` for Windows Remote Management (WinRM) support.
   - Ansible for IT automation.
   - SSH server for remote access.
   - Networking tools like `ping`.
3. **User Setup**:
   - A user `test` with sudo privileges and password authentication enabled.
4. **SSH Server**:
   - Configured to run on port 22 for remote management.

---

#### Build Instructions
1. Save the Dockerfile in a directory.
2. Build the Docker image:
   ```bash
   docker build -t ansible-automation:latest .
   ```

---

#### Run Instructions
1. Run a container from the built image:
   ```bash
   docker run -d -p 2222:22 ansible-automation:latest
   ```
   - Exposes SSH on port 2222 (host) -> 22 (container).

2. SSH into the container:
   ```bash
   ssh test@localhost -p 2222
   ```
   Password: `test`

---

#### Usage
- **Ansible Testing**:
  - Use this container to test Ansible playbooks with WinRM or Kerberos authentication.
  - Install additional Ansible collections or modules as needed.
- **Automation Development**:
  - Develop and debug scripts or applications requiring Python, Ansible, or Kerberos.

---

#### Notes
- Modify the Dockerfile to include additional packages if required.
- Ensure proper Kerberos configuration (`/etc/krb5.conf`) for Kerberos-based operations.
- Replace default credentials (`test:test`) for production use.
