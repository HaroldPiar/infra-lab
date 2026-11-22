## Environment

- Windows + Docker Desktop (WSL2 backend) + Ubuntu on WSL2
- VS Code with the WSL extension
- Work inside the WSL filesystem (not under `C:\`) for better performance
- Main diagnostic tool: [`nicolaka/netshoot`](https://github.com/nicolaka/netshoot)

## Resulst: 

- Docker is running with WSL2 as Backend with Ubuntu 
cat /etc/os-release : 
    PRETTY_NAME="Ubuntu 24.04.3 LTS"
    NAME="Ubuntu"
    VERSION_ID="24.04"
    VERSION="24.04.3 LTS (Noble Numbat)"
    VERSION_CODENAME=noble
    ID=ubuntu
    ID_LIKE=debian
    HOME_URL="https://www.ubuntu.com/"
    SUPPORT_URL="https://help.ubuntu.com/"
    BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
    PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
    UBUNTU_CODENAME=noble
    LOGO=ubuntu-logo

- Terminal in VSC is a Command Line with WSL

- The repo is cloned in ~, on WSL

- We build the docker compose file for netshooting tools 