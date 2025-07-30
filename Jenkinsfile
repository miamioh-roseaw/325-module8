pipeline {
  agent any

  environment {
    INVENTORY = 'hosts'
    PLAYBOOK = 'playbook.yaml'
    PATH = "${HOME}/.local/bin:${env.PATH}"
    ANSIBLE_CONFIG = 'ansible.cfg'
    ANSIBLE_HOST_KEY_CHECKING = 'False'
  }

  stages {

    stage('Install Ansible') {
      steps {
        sh '''
          # Ensure pip is installed
          if ! command -v pip3 > /dev/null; then
              echo "[INFO] pip3 not found. Installing..."
              wget https://bootstrap.pypa.io/get-pip.py -O get-pip.py
              python3 get-pip.py --user
          fi

          # Install Ansible if not available
          if ! command -v ansible-playbook > /dev/null; then
              echo "[INFO] Ansible not found. Installing..."
              pip3 install --user ansible
          else
              echo "[INFO] Ansible is already installed."
          fi
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

          export ANSIBLE_HOST_KEY_CHECKING=$ANSIBLE_HOST_KEY_CHECKING

          ansible-playbook $PLAYBOOK -i $INVENTORY \
            -e "ansible_user=${CISCO_CREDS_USR} ansible_password=${CISCO_CREDS_PSW}"
        '''
      }
    }
  }
}
