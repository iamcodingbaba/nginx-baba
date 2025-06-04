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
                    curl -sSL -H "Cache-Control: no-cache" -o ${PLAYBOOK_FILE} https://raw.githubusercontent.com/iamcodingbaba/nginx-baba/main/azure-nginx
                    curl -s https://api.github.com/repos/iamcodingbaba/nginx-baba/commits/main | grep sha
                """
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                echo "Executing Ansible playbook..."
                sh """
                    sudo ansible-playbook -i ${INVENTORY_FILE} ${PLAYBOOK_FILE}
                """
            }
        }

        stage('Restart NGINX') {
            steps {
                echo "Restarting NGINX service..."
                sh """
                    sudo /bin/systemctl restart nginx
                    echo "NGINX restarted."
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
