pipeline {
    agent any

    environment {
        PLAYBOOK_FILE = "/home/ubuntu/ansible/playbook_bansible_nginx.yaml"
        INVENTORY_FILE = "/home/ubuntu/ansible/inventory_azure.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning GitHub repository..."
                git branch: 'main', url: 'https://github.com/iamcodingbaba/nginx-baba.git'
            }
        }

    stage('Overwrite Playbook with azure-nginx') {
        steps {
            echo "Overwriting ${PLAYBOOK_FILE} with content from azure-nginx..."
            sh '''
                sudo cp azure-nginx "${PLAYBOOK_FILE}"
            '''
        }
    }


        stage('Run Ansible Playbook') {
            steps {
                echo "Executing Ansible playbook..."
                sh "ansible-playbook -i ${INVENTORY_FILE} ${PLAYBOOK_FILE}"
            }
        }
    }

    post {
        failure {
            echo 'Deployment failed. Please check the logs.'
        }
        success {
            echo 'Deployment completed successfully!'
        }
    }
}
