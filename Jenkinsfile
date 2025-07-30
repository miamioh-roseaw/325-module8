pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg"
        ANSIBLE_SSH_EXECUTABLE = 'ssh'
    }
    stages {
        stage('Run Ansible Playbook') {
            steps {
                echo '[INFO] Running Ansible Playbook...'
                sh '''
                    ansible-playbook playbook.yaml -i hosts \
                      -e ansible_user="${ANSIBLE_USER}" \
                      -e ansible_password="${ANSIBLE_PASSWORD}"
                '''
            }
        }
    }

    post {
        always {
            echo '✅ Jenkins pipeline finished.'
        }
        failure {
            echo '❌ Jenkins pipeline failed.'
        }
    }
}
