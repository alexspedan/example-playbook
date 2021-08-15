pipeline {
    agent {
        node {
        label 'ansible_docker'
        }
    }
    stages {
        stege('Build') {
            steps {
                //Get some code from GitHub repository
                git 'https://github.com/alexspedan/example-playbook.git'
                sh ansible-vault decrypt secret --vault-password-file vault_pass
                sh mkdir ~/.ssh/ && mv ./secret ~/.ssh/id_rsa && chmod 600 ~/.ssh/id_rsa
                sh 'eval `ssh-agent -s` && ssh-add ~/.ssh/id_rsa && eval `ssh -T git@github.com -o StrictHostKeyChecking=no`'
                sh ansible-galaxy install -r requirements.yml -p roles
                ansible-playbook inventory: 'inventory/prod.yml', playbook: site.yml

            }
        }
    }
}
