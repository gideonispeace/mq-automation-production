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

    environment {
        EFFECTIVE_TARGET = "${params.TARGET_LIMIT?.trim() ? params.TARGET_LIMIT.trim() : 'mqnode1'}"
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
                    echo "PIPELINE_ACTION=${PIPELINE_ACTION}"
                    echo "TARGET_LIMIT=${TARGET_LIMIT}"
                    echo "EFFECTIVE_TARGET=${EFFECTIVE_TARGET}"
                '''
            }
        }

        stage('Ping Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'ping' }
            }
            steps {
                sh '''
                    ansible "${EFFECTIVE_TARGET}" -m ping
                '''
            }
        }

        stage('Validate Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'validate' }
            }
            steps {
                sh '''
                    ansible-playbook playbooks/validate_targets.yml --limit "${EFFECTIVE_TARGET}"
                '''
            }
        }

        stage('Deploy MQ Objects') {
            when {
                expression { params.PIPELINE_ACTION == 'deploy_mq' }
            }
            steps {
                sh '''
                    ansible-playbook playbooks/deploy_mq_objects.yml --limit "${EFFECTIVE_TARGET}"
                '''
            }
        }
    }
}
