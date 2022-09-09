pipeline {
    agent { 
        node {
        label params.Node
        }
    }
    parameters{
        choice(name: "Node", choices: ["master", "develop"], description: "Node to be used for the building and deploying")
    }
    environment{
        KUBECTL_VERSION = "v1.25.0"
    }

    stages {        
        stage('Prepare Tools') {
            steps {              
                echo '--------------Preparing tools...--------------'           
            }
        }
        stage('Build') {
            steps {
            sh "curl -LO https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl"
            sh "chmod +x ./kubectl"
            withCredentials([usernamePassword(credentialsId: 'k8snodes', usernameVariable: 'USER', passwordVariable: 'PASSWORD')]){                        
                    sh "sshpass -p ${PASSWORD} scp -o StrictHostKeyChecking=no ${USER}@${MASTERNODE}:/root/.kube/config ./config.yaml"              
                }
            sh "sed -i \"s+https://127.0.0.1+cp.k8s\" config.yaml"
            sh "./kubectl --kubeconfig=\"config.yaml\" get nodes -o wide"
            }
        }
    post {
        always {
            cleanWs()
        }
    }
}
