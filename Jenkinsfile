properties([
    parameters([
        choice(
            name: 'PIPELINE_ACTION',
            choices: ['ping', 'validate', 'deploy_mq'],
            description: 'Choose automation action'
        ),
        choice(
            name: 'TARGET_ENV',
            choices: ['mq_nonprod', 'mq_prod'],
            description: 'Choose target environment group'
        ),
        string(
            name: 'TARGET_LIMIT',
            defaultValue: '',
            description: 'Optional host override, for example mqnode1. Leave blank to use TARGET_ENV.'
        )
    ])
])

pipeline {
    agent any

    environment {
        EFFECTIVE_TARGET = "${params.TARGET_LIMIT?.trim() ? params.TARGET_LIMIT.trim() : params.TARGET_ENV}"
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
                    echo "PIPELINE_ACTION=${PIPELINE_ACTION}"
                    echo "TARGET_ENV=${TARGET_ENV}"
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
                sh 'ansible "${EFFECTIVE_TARGET}" -m ping'
            }
        }

        stage('Validate Targets') {
            when {
                expression { params.PIPELINE_ACTION == 'validate' }
            }
            steps {
                sh 'ansible-playbook playbooks/validate_targets.yml --limit "${EFFECTIVE_TARGET}"'
            }
        }

        stage('🚨 Approval for PROD') {
            when {
                expression {
                    params.PIPELINE_ACTION == 'deploy_mq' &&
                    params.TARGET_ENV == 'mq_prod'
                }
            }
            steps {
                input message: "Deploying to PROD. Approve to continue?", ok: "Deploy"
            }
        }

        stage('Deploy MQ Objects') {
            when {
                expression { params.PIPELINE_ACTION == 'deploy_mq' }
            }
            steps {
                sh 'ansible-playbook playbooks/deploy_mq_objects.yml --limit "${EFFECTIVE_TARGET}"'
            }
        }
    }
}
