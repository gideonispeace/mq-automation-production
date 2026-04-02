pipeline {
    agent any

    parameters {
        choice(
            name: 'PIPELINE_ACTION',
            choices: ['ping', 'validate', 'deploy_mq'],
            description: 'Choose automation action'
        )
        string(
            name: 'TARGET_LIMIT',
            defaultValue: 'mqnode1',
            description: 'Host or group to target, for example mqnode1 or mq_targets'
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
                sh '''
                    ansible "${TARGET_LIMIT}" -m ping
                '''
            }
        }

        stage('Validate Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'validate' }
            }
            steps {
                sh '''
                    ansible-playbook playbooks/validate_targets.yml --limit "${TARGET_LIMIT}"
                '''
            }
        }

        stage('Deploy MQ Objects') {
            when {
                expression { params.PIPELINE_ACTION == 'deploy_mq' }
            }
            steps {
                sh '''
                    ansible-playbook playbooks/deploy_mq_objects.yml --limit "${TARGET_LIMIT}"
                '''
            }
        }
    }
}
