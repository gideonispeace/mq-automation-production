pipeline {
    agent any

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
                    find . -maxdepth 3 -type f | sort

                    echo ""
                    echo "Inventory contents:"
                    cat inventory/hosts.ini

                    echo ""
                    echo "Playbook contents:"
                    cat playbooks/ping.yml
                '''
            }
        }

        stage('Test SSH Connectivity') {
            steps {
                sh '''
                    ansible mq_targets -m ping
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
