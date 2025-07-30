pipeline {
  agent any

  environment {
    INVENTORY = 'hosts'
    PLAYBOOK = 'playbook.yaml'
    PATH = "${HOME}/.local/bin:${env.PATH}"
    ANSIBLE_CONFIG = 'ansible.cfg'
    ANSIBLE_HOST_KEY_CHECKING = 'False'
    ANSIBLE_SSH_TYPE = 'paramiko'
  }

  stages {

    stage('Install Ansible and Required Collections') {
      steps {
        sh '''
          # Ensure pip is installed
          if ! command -v pip3 > /dev/null; then
              echo "[INFO] pip3 not found. Installing..."
              wget https://bootstrap.pypa.io/get-pip.py -O get-pip.py
              python3 get-pip.py --user
          fi

          # Install Ansible
          if ! command -v ansible-playbook > /dev/null; then
              echo "[INFO] Installing Ansible..."
              pip3 install --user ansible
          else
              echo "[INFO] Ansible already installed."
          fi

          # Install required collection for ios_banner module
          ansible-galaxy collection install cisco.ios
        '''
      }
    }

    stage('Run Ansible Playbook') {
      environment {
        CISCO_CREDS = credentials('cisco-ssh-creds')
      }
      steps {
        sh '''
          echo "[INFO] Running Ansible Playbook..."
          ansible-playbook $PLAYBOOK -i $INVENTORY \
            -e "ansible_user=${CISCO_CREDS_USR} \
                ansible_password=${CISCO_CREDS_PSW} \
                ansible_become_password=${CISCO_CREDS_PSW}"
        '''
      }
    }
  }
}
