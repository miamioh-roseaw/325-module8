pipeline {
  agent any

  environment {
    INVENTORY = 'hosts'
    PLAYBOOK = 'playbook.yaml'
    PATH = "${HOME}/.local/bin:${env.PATH}"
    ANSIBLE_CONFIG = 'ansible.cfg'
    ANSIBLE_HOST_KEY_CHECKING = 'False'
    LIBSSH2KNOWNHOSTS = '/dev/null' // disables strict host checking with libssh
  }

  stages {

    stage('Install Ansible and pylibssh') {
      steps {
        sh '''
          # Ensure pip is installed
          if ! command -v pip3 > /dev/null; then
              echo "[INFO] pip3 not found. Installing..."
              wget https://bootstrap.pypa.io/get-pip.py -O get-pip.py
              python3 get-pip.py --user
          fi

          # Install Ansible and pylibssh
          pip3 install --user ansible ansible-pylibssh
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
            -e "ansible_user=${CISCO_CREDS_USR} ansible_password=${CISCO_CREDS_PSW}" -vvv
        '''
      }
    }
  }
}
