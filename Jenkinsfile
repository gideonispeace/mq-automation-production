// force refresh
pipeline {
    agent any

    stages {
        stage('Verify Repo') {
            steps {
                sh '''
                    echo "Repo pulled successfully"
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

        stage('Run Ansible Playbook') {
            steps {
                sh '''
                    ansible-playbook playbooks/ping.yml
                '''
            }
        }
    }
}
