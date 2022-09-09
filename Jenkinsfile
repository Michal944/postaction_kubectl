pipeline {
    agent {
        label 'develop'
    }
    environment{
        KUBECTL_VERSION="v1.25.0"
        MASTERNODE = "172.168.8.1"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
            sh "curl -LO https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl"
            sh "chmod +x ./kubectl"
            withCredentials([usernamePassword(credentialsId: 'k8snodes', usernameVariable: 'USER', passwordVariable: 'PASSWORD')]){                        
                    sh "sshpass -p ${PASSWORD} scp -o StrictHostKeyChecking=no ${USER}@${MASTERNODE}:/root/.kube/config ./config.yaml"              
                }
            sh "sed -i \"s+https://127.0.0.1+cp.k8s\" -i config.yaml"
            sh "./kubectl --kubeconfig=\"config.yaml\" get nodes -o wide"
 
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
