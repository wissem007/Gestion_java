pipeline {
    agent any

    
 
    stages {
        stage('CLONE') {
            steps {
                
                git branch: 'main', url: 'https://github.com/wissem007/Gestion_java.git'
            }
        }

        stage('Build avec Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

    }
}
