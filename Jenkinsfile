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
            
            #when{ expression {params.choice == 'both'|'build'}}

            steps {  
                   

                    sh 'docker build . -t node-app:$BUILD_NUMBER'
            }

        } 

        stage('Building our image') { 
            #when{ expression {params.choice == 'both' | params.choice == 'build'}}
            steps { 

                script { 

                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 

                }

            } 

        }

        stage('Deploy our image') { 
        #when{ expression {params.choice == 'both' | params.choice == 'build'}} 
            steps { 

                script { 

                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                }
            } 
        }
    }    
        
        stage('Cleaning up') { 
            #when{ expression {params.choice == 'both'|'build'}}
            steps { 

                sh "docker rmi $registry:$BUILD_NUMBER" 
                sh 'docker run -d -p 3000:3000  node-app:$BUILD_NUMBER '
            }
        }
            
        stage('Deploy  Application') {
            #when{ expression {params.choice == 'both' | params.choice == 'Deploy'}}  
                
            steps { 
                sh "cd Deployment"
                sh  "kubectl apply -f deploy.yaml"
                sh  "kubectl apply -f service.yaml"
            }
        } 
    }
}    
     
