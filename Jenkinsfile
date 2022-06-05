pipeline {

    environment { 
        registry = "stwalez/php-todo" 
        registryCredential = 'dockerhub-id' 
        dockerImage = '' 
    }
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }

        }

    stage('Checkout SCM') {
      steps {
            git branch: 'jenkins-main', url: 'https://github.com/stwalez/php-todo.git'
      }
    }

    stage('Building our image') { 
            steps { 
                script { 
                    sh 'echo building the image'
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
            }
        } 
    }
}