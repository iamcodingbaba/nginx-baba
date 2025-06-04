pipeline {
    agent any

    environment {
        PLAYBOOK_FILE = "/home/ubuntu/ansible/aws.yaml"
        INVENTORY_FILE = "/home/ubuntu/ansible/dynamic_aws_ec2.yaml"
    }

    stages {
        stage('Download Playbook') {
            steps {
                echo "Downloading azure-nginx file directly to ${PLAYBOOK_FILE}..."
                sh """
                    curl -sSL -H "Cache-Control: no-cache" -o /home/ubuntu/ansible/aws.yaml https://raw.githubusercontent.com/iamcodingbaba/nginx-baba/main/azure-nginx
                    curl -s https://api.github.com/repos/iamcodingbaba/nginx-baba/commits/main | grep sha
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
