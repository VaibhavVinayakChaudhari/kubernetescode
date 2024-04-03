pipeline {
    agent any
    
    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/VaibhavVinayakChaudhari/kubernetescode'        
            }
        }
        
        stage('Docker build') {
            steps {
                sh 'docker build -t vaibhav1405/cicd:$BUILD_NUMBER .' 
            }
        }
        
        stage('Docker push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh 'docker push vaibhav1405/cicd:$BUILD_NUMBER'
                    }
                }
            }
        }
        
        stage('Modify Deployment') {
            steps {
                script {
                    sh "sed -i 's|image: docker.io/httpd|image: vaibhav1405/cicd:$BUILD_NUMBER|' deployment.yaml"
                }
            }
        }
      
        stage('Deployment') {
            steps {
                script {
                    
                    sh 'kubectl apply --kubeconfig=/tmp/myconfig -f deployment.yaml'
                    sh 'kubectl apply --kubeconfig=/tmp/myconfig -f svc.yaml'
                    }
            }
        }
    }
}
