# Install Visual Studio Code Server (code-server) on Ubuntu 22.04 (LTS)

### Introduction

This guide provides step-by-step instructions to install and configure **Visual Studio Code Server (code-server)** on an Ubuntu 22.04 (LTS) server. `code-server` allows you to run Visual Studio Code on a remote server and access it through your web browser.

### Prerequisites

- Ubuntu 22.04 (LTS) server
- Root or sudo access
- Basic knowledge of the command line

### Steps

#### 1. Update System

First, update your system to the latest version.

```bash
sudo apt update
sudo apt upgrade -y
```
#### 2. Install Dependencies

Install essential packages required for the installation.
```bash
sudo apt install -y curl wget gnupg
```

#### 3. Install Node.js (Optional)

If Node.js is not already installed or you need a specific version, install it with the following commands.
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

```
#### 4. Install code-server
  - Download and Install
    Install code-server using the installation script.
    ```bash
    curl -fsSL https://code-server.dev/install.sh | sh
    ```
    Alternatively, you can download a specific version from the [code-server GitHub releases page](https://github.com/coder/code-server/releases) and follow the manual installation instructions.
  - Verify Installation
    Ensure 'code-server' is installed correctly by checking its version.
    ```bash
    code-server --version
    ```
#### 5. Configure code-server
- Create Systemd Service File
  Create a systemd service file for code-server to manage it as a service.
  ```bash
  sudo nano /etc/systemd/system/code-server.service
  ```
  Paste the following configuration into the file:
  ```bash
  [Unit]
  Description=code-server
  Documentation=https://github.com/coder/code-server
  After=network.target

  [Service]
  Type=simple
  User=your-username
  ExecStart=/usr/bin/code-server --config /home/your-username/.config/code-server/config.yaml
  Restart=on-failure
  KillMode=process

  [Install]
  WantedBy=multi-user.target
  ```
  Replace your-username with your actual username.
- Reload Systemd and Start code-server
  Reload systemd and start the code-server service.
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable code-server
  sudo systemctl start code-server
  ```
#### 6. Configure Password
- Edit Configuration
  Edit the code-server configuration file to set your password and other settings.
  ```bash
  nano ~/.config/code-server/config.yaml
  ```
  Add or update the following lines to set the password:
  ```bash
  bind-addr: 0.0.0.0:8080
  auth: password
  password: your-password
  ```
  Replace your-password with your desired password.
- Restart code-server
  Restart the service to apply the new configuration.
  ```bash
  sudo systemctl restart code-server
  ```
#### 7. Access code-server
Open your web browser and navigate to:
```bash
http://your-ip-address:8080
```
Log in using the password you configured.


### Troubleshooting
If you encounter issues or need help, refer to the code-server documentation or check the troubleshooting guide.

