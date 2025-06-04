pipeline {
    agent any

    environment {
        PLAYBOOK_FILE = "playbook_bansible_nginx.yaml"
        INVENTORY_FILE = "inventory_azure.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Cloning GitHub repository..."
                git branch: 'main', url: 'https://github.com/iamcodingbaba/nginx-baba.git'
            }
        }

        stage('Prepare Playbook') {
            steps {
                echo "Copying content to ${PLAYBOOK_FILE}..."
                sh 'cp azure-nginx ${PLAYBOOK_FILE}'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo "Executing Ansible playbook..."
		sh "cd ansible"
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
