pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }
    tools {
        maven 'Maven-3.9'
    }
    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging..'
                sh 'mvn clean package'
            }
        }
        stage('Copying jar file') {
            steps {
                echo 'Copying jar file..'
                sh 'mv target/*.jar .'
            }
        }
        stage('cleanup') {
            steps {
                sh 'docker system prune -a --volumes --force --filter "label=campaign-demo-server" || true'
            }
        }
        stage('build image') {
            steps {
                sh 'docker build -t lindsaysamantha/campaign-demo:v1 --label campaign-demo-server .'
            }
        }
        stage('run container') {
            steps {
                sh 'docker run -d --name campaign-demo-server --label campaign-demo-server -p 5000:5000 lindsaysamantha/campaign-demo:v1'
            }
        }
    }
}
