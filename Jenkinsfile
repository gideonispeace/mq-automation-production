pipeline {
    agent any

    parameters {
        choice(
            name: 'PIPELINE_ACTION',
            choices: ['ping', 'validate', 'deploy_mq'],
            description: 'Choose which automation action to run'
        )
    }

    stages {
        stage('Verify Repo') {
            steps {
                sh '''
                    echo "Repo pulled successfully"
                    pwd
                    ls -la
                '''
            }
        }

        stage('Check Ansible') {
            steps {
                sh '''
                    ansible --version
                '''
            }
        }

        stage('Inspect Workspace') {
            steps {
                sh '''
                    echo "Current working directory:"
                    pwd

                    echo ""
                    echo "Workspace file tree:"
                    find . -maxdepth 4 -type f | sort

                    echo ""
                    echo "Inventory contents:"
                    cat inventory/hosts.ini
                '''
            }
        }

        stage('Ping Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'ping' }
            }
            steps {
                sh '''
                    ansible mq_targets -m ping
                '''
            }
        }

        stage('Validate Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'validate' }
            }
            steps {
                sh '''
                    ansible-playbook playbooks/validate_targets.yml
                '''
            }
        }

        stage('Deploy MQ Objects') {
            when {
                expression { params.PIPELINE_ACTION == 'deploy_mq' }
            }
            steps {
                sh '''
                    ansible-playbook playbooks/deploy_mq_objects.yml
                '''
            }
        }
    }
}
