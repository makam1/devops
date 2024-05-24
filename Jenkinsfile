pipeline {
    agent any
    stages {
        stage('Build Artefact') {
            steps {
               sh "mvn clean package -DskipTest=true"
            }
        }
        stage('Lancer les Tests') {
            steps {
               sh "mvn test"
            }
        }
        stage('Build and Push Image') {
            steps {
                withDockerRegistry([credentialsId: "docker-credential", url: ""]) {
                    sh "docker build -t makam1/demo-boot:$BUILD_NUMBER ."
                    sh "docker push makam1/demo-boot:$BUILD_NUMBER"
                }
            }
        }

        stage('Deploy to dev') {
            when { expression { false } }
            steps {
                sh 'docker stop demo-boot || true'
                sh 'docker rm demo-boot || true'
                sh 'docker run -p 8180:8180 -d --name demo-boot demo-boot'
            }
        }
    }
}
