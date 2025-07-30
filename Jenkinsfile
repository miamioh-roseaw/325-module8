pipeline {
  agent any

  environment {
    INVENTORY = 'hosts'
    PLAYBOOK = 'playbook.yaml'
  }

  stages {

    stage('Run Ansible Playbook') {
      environment {
        CISCO_CREDS = credentials('cisco-ssh-creds')
      }
      steps {
        sh '''
          cd ansible-automation
          echo "[INFO] Running Ansible Playbook..."
          ansible-playbook $PLAYBOOK -i $INVENTORY \
            -e "ansible_user=${CISCO_CREDS_USR} ansible_password=${CISCO_CREDS_PSW}"
        '''
      }
    }
  }
}
