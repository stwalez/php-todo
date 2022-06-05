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

      stage('Building the features branch image') { 
              when { branch pattern: "^jenkins-feature*", comparator: "REGEXP"}
              environment {
                branch_name = 'features'
              }
              steps { 
                  script { 
                      sh 'echo building the image'
                      dockerImage = docker.build registry + ":$branch_name-$BUILD_NUMBER" 
                  }
              } 
          }
      stage('Building the jenkins main branch image') { 
              when { branch pattern: "^jenkins-main*", comparator: "REGEXP"}
              environment {
                branch_name = 'main'
              }
              steps { 
                  script { 
                      sh 'echo building the image'
                      dockerImage = docker.build registry + ":$branch_name-$BUILD_NUMBER" 
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
              sh "docker rmi $registry:$branch_name-$BUILD_NUMBER" 
          }
      } 
    }
}