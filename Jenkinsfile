pipeline {
    agent any
    
    environment{
        IMAGE="leonelfeukouo/talys_app:v_${env.BUILD_NUMBER}"
        DOCKERHUB_CREDENTIALS = credentials('LeonelDockerHub')
    }
    
    tools {
      maven 'maven'
  }
  
    stages {
        stage('CLONE avec Git') {
            steps {
                git branch:'main', url:'https://github.com/LeonelFeukouo/Gestion_java.git'
            }
        }

        stage('BUILD avec Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        // stage('TEST avec Sonarqube') {
        //   steps {
        //     withSonarQubeEnv(installationName: 'sq1') { 
        //       sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
        //     }
        //   }
        // }

        // stage('TEST avec SonarQube') {
        //     steps {
        //      withSonarQubeEnv('sq1') { 
        //      sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.0.2155:sonar'
        //    }
        //  }
        // }
    
        // stage('RELEASE avec Dockerfile') {
        //     steps {
        //         sh 'docker build -t ${IMAGE} .'
        //     }
        // }

         stage('Build') {
            steps {
                // Étapes pour la construction de votre application ou conteneur
                
                // Exemple de construction de l'image Docker
                script {
                    def dockerImage = docker.build("${IMAGE}")
                }
                
                // Autres étapes de construction, si nécessaire
            }
        }
        
        stage('Push Image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push ${IMAGE}'
                sh 'docker logout'
            }
        }
        stage('Remove Container') {
            steps {
                script {
                    def containerName = 'app' // Remplacez par le nom de votre conteneur
                    
                    def containerId = sh(script: "docker ps -aq -f name=${containerName}", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker rm -f ${containerName}"
                        echo "Container '${containerName}' has been removed."
                    } else {
                        echo "No container found with the name '${containerName}'."
                    }
                }
            }
        }
        
        // stage('DEPLOYMENT: Lancement du conteneur') {
        //     steps {
        //         script {
        //             def containerId = sh(returnStdout: true, script: "docker ps -q -f name=app").trim()
        //             if (containerId) {
        //                 echo "Ancien conteneur détecté : $containerId"
        //                 sh "docker kill $containerId"
        //                 sh "docker rm $containerId"
        //                 sh 'docker run -d --name app -p 8081:8080 ${IMAGE}'
        //             } else {
        //                 echo "Aucun ancien conteneur trouvé"
        //                 sh 'docker run -d --name app -p 8081:8080 ${IMAGE}'
        //             }
        //         }
                
        //     }
        // }
        
        stage('Suppression de l\'image') {
            steps {
                sh 'docker rmi -f ${IMAGE}'
            }
        }

   

   
   
    }
  }
