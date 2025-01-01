# GitLab Setup with Docker

**GitLab** provides Git repository management, code reviews, issue tracking, wikis, and more. It also includes GitLab CI, a powerful tool for continuous integration and deployment.

---

## Docker Compose Configuration

### File: `docker-compose.yml`
```yaml
gitlab:
  image: gitlab/gitlab-ce
  hostname: git.example.com
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'https://git.example.com'
      gitlab_rails['gitlab_shell_ssh_port'] = 2222
  ports:
    - "443:443"
    - "2222:22"
  volumes:
    - ./gitlab/config:/etc/gitlab
    - ./gitlab/logs:/var/log/gitlab
    - ./gitlab/data:/var/opt/gitlab
  restart: always
```

---

## TLS Certificates

Place your TLS certificate and key files into the `./gitlab/config/ssl/` directory:

- `git.example.com.crt`
- `git.example.com.key`

---

## SSH Configuration

If port 22 is already in use, update your host's SSH configuration to allow GitLab to use port 2222:

1. Open the SSH configuration file:
   ```bash
   vi /etc/ssh/sshd_config
   ```
2. Add the following lines:
   ```
   Port 22
   Port 2222
   ```
3. Restart the SSH service:
   ```bash
   systemctl restart ssh
   ```
4. Test the connection:
   ```bash
   ssh -p 2222 localhost
   ```

---

## Start GitLab

1. Create necessary directories for GitLab:
   ```bash
   mkdir -p ~/fig/gitlab/gitlab/config/ssh
   cd ~/fig/gitlab/gitlab/config/ssh
   ```
2. Generate self-signed TLS certificates:
   ```bash
   openssl req -newkey rsa:4096 -nodes -sha256 -x509 -days 365 \
   -keyout git.example.com.key \
   -out git.example.com.crt
   ```
3. Start GitLab using Docker Compose:
   ```bash
   docker-compose up -d
   ```

---

## Access GitLab

Open the following URL in your web browser:

- **URL:** [https://git.example.com](https://git.example.com)
- **Username:** `root`
- **Password:** `5iveL!fe`

---

## Backup Volumes

To back up your GitLab volumes, run the following command:

```bash
docker run --rm \
  --volumes-from gitlab_gitlab_1 \
  -v $PWD:/tmp \
  alpine \
  tar czf /tmp/gitlab.tgz /etc/gitlab /var/opt/gitlab /var/log/gitlab
```

To verify the backup file, list its contents:

```bash
tar tzf gitlab.tgz
```

---
