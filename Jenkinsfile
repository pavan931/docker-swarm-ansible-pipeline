pipeline{
    agent any
    environment{
        AWS_REGION='us-west-1'
    }
    stages{
        stage('clone'){
            steps{
                git url:'https://github.com/pavan931/docker-swarm-ansible-pipeline.git',branch:'main',
                credentialsId:'git-creds'
            }
        }
        stage('Run ansible Playbook'){
            steps{
                withCredentials([sshUserPrivateKey(credentialsId:'ec2-ssh',keyFileVariable:'KEY')]){
                    sh '''
                    ansible-playbook -i ansible/inventory/hosts.ini ansible/playbook.yml --private-key=$KEY -e "ansible_ssh_common_args='-o StrictHostKeyChecking=no'"
                    '''
                }
            }
        }
    }
    post{
        success{
            echo 'Swarm setup and services deployed successfully!'
        }
        failure{
            echo 'Deployment failed!'
        }
    }
}
