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
                sh '''
		echo "Updating ${PLAYBOOK_FILE} with changes from azure-nginx..."
		
		if [ -f "${PLAYBOOK_FILE}" ]; then
		  grep -Fxv -f "${PLAYBOOK_FILE}" azure-nginx >> "${PLAYBOOK_FILE}"
		else
  		  echo "No existing ${PLAYBOOK_FILE}. Creating a new one."
		  cp azure-nginx "${PLAYBOOK_FILE}"
		fi
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
