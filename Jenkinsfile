pipeline {
  agent any

  environment {
    ANSIBLE_FORCE_COLOR = 'true'
    ANSIBLE_HOST_KEY_CHECKING = 'False'
    ANSIBLE_SSH_EXECUTABLE = 'ssh'
  }

  stages {
    stage('Install Ansible & Cisco Collection') {
      steps {
        sh '''
          # Install pip if missing
          if ! command -v pip3 > /dev/null; then
            wget https://bootstrap.pypa.io/get-pip.py -O get-pip.py
            python3 get-pip.py --user
          fi

          # Install Ansible and collection
          pip3 install --user ansible
          ~/.local/bin/ansible-galaxy collection install cisco.ios
        '''
      }
    }

    stage('Run Ansible Playbook') {
      steps {
        sh '''
          echo "[INFO] Running Ansible Playbook..."
          ~/.local/bin/ansible-playbook \
            -i hosts \
            set_banner.yml
        '''
      }
    }
  }

  post {
    always {
      echo '[INFO] Job complete.'
    }
  }
}
