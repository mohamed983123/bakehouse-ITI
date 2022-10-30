pipeline { 

    environment { 

        registry = "morshed983/morshed983" 

        registryCredential = 'docker-hub'


    }
    agent { label 'avg3' }
    
    stages { 

        stage('build my img') { 

            steps { 

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



    }



}
