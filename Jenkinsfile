pipeline {
    agent any

    stages {
        stage('Verify Repo') {
            steps {
                sh '''
                    echo "Repo pulled successfully"
                    echo "Current directory:"
                    pwd
                    echo "Top-level contents:"
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
                    echo "Git tree for current HEAD:"
                    git ls-tree -r --name-only HEAD | sort

                    echo ""
                    echo "Playbooks directory details:"
                    ls -la playbooks || true

                    echo ""
                    echo "Ping file details:"
                    ls -l playbooks/ping.yml || true

                    echo ""
                    echo "Ping file contents:"
                    cat playbooks/ping.yml || true
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
