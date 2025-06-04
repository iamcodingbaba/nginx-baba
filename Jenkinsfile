pipeline {
    agent any

    environment {
        PLAYBOOK_FILE = "/home/ubuntu/ansible/playbook.yaml"
        INVENTORY_FILE = "/home/ubuntu/ansible/dynamic_aws_ec2.yaml"
    }

    stages {
        stage('Download Playbook') {
            steps {
                echo "Downloading azure-nginx file directly to ${PLAYBOOK_FILE}..."
                sh """
                    curl -o ${PLAYBOOK_FILE} https://raw.githubusercontent.com/iamcodingbaba/nginx-baba/main/azure-nginx
                """
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo "Executing Ansible playbook..."
                sh """
                    # Assuming Jenkins user can run sudo without password for ansible-playbook or you configure sudoers accordingly
                    sudo ansible-playbook -i ${INVENTORY_FILE} ${PLAYBOOK_FILE}
                """
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
