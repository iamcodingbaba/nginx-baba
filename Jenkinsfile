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

        stage('Update Playbook on System') {
            steps {
                echo "Updating ${PLAYBOOK_FILE} with content from azure-nginx..."
                sh """
                    if [ -f "${PLAYBOOK_FILE}" ]; then
                        echo "Existing playbook found. Merging updates..."
                        grep -Fxv -f "${PLAYBOOK_FILE}" azure-nginx > /tmp/temp_diff || true
                        if [ -s /tmp/temp_diff ]; then
                            sudo bash -c 'cat /tmp/temp_diff >> "${PLAYBOOK_FILE}"'
                            echo "New content appended to ${PLAYBOOK_FILE}."
                        else
                            echo "No new content to add to the playbook."
                        fi
                        rm -f /tmp/temp_diff
                    else
                        echo "No existing playbook. Creating new one..."
                        sudo cp azure-nginx "${PLAYBOOK_FILE}"
                    fi
                """
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
