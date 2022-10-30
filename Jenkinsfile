pipeline {
 agent { label 'agent' }
 
    
 parameters {
  choice choices: ['Deploy', 'Build', 'both'], name: 'choice'
}

    environment { 
        registry = "morshed983/morshed983" 
        registryCredential = 'docker-hub'
    }
    stages { 
         
   
        stage('build my img') {
            
            

            steps {  
                   sh 'groupadd docker'
                   sh 'usermod -aG docker ${USER}'
                   

                    sh 'docker build . -t node-app:$BUILD_NUMBER'
            }

        } 

        stage('Building our image') { 
            
            steps { 

                script { 

                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 

                }

            } 

        }

        stage('Deploy our image') { 
        
            steps { 

                script { 

                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                }
            } 
        }
    }    
        
        stage('Cleaning up') { 
            
            steps { 

                sh "docker rmi $registry:$BUILD_NUMBER" 
                sh 'docker run -d -p 3000:3000  node-app:$BUILD_NUMBER '
            }
        }
            
        stage('Deploy  Application') {
             
                
            steps { 
                sh "cd Deployment"
                sh  "kubectl apply -f deploy.yaml"
                sh  "kubectl apply -f service.yaml"
            }
        } 
    }
}    
     
