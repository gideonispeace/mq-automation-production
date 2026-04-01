pipeline {
    agent any

    parameters {
        choice(
            name: 'PIPELINE_ACTION',
            choices: ['ping', 'validate', 'deploy_mq'],
            description: 'Choose automation action'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Repo') {
            steps {
                sh '''
                    echo "Workspace:"
                    pwd
                    ls -la
                '''
            }
        }

        stage('Ping Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'ping' }
            }
            steps {
                sh 'ansible mq_targets -m ping'
            }
        }

        stage('Validate Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'validate' }
            }
            steps {
                sh 'ansible-playbook playbooks/validate_targets.yml'
            }
        }

        stage('Deploy MQ Objects') {
            when {
                expression { params.PIPELINE_ACTION == 'deploy_mq' }
            }
            steps {
                sh 'ansible-playbook playbooks/deploy_mq_objects.yml'
            }
        }
    }
}
