pipeline {
    agent {
      label 'linux'
    }
    tools {
      maven 'mvn_3.8.1'
    }

    stages {
        
        stage ('Checkout Java Code'){
            steps{
                git branch: 'master', credentialsId: 'GITHUB-CREDS', url: 'https://github.com/kul-samples/java_sample_webapp.git'
            }
        }
        stage('Check Docker Version') {
            steps {
                 sh 'docker version'
             }
        }
        stage('Build Package') {
          steps {
            sh 'mvn clean package'
          }
          post {
            always {
                archive 'target/devops.war'
                }
            }
        }
        
        stage('create docker image') {
            steps {
                 sh '''docker image ls 
                    docker image build .  -f Dockerfile -t 912silver/devops1:latest
                    docker image ls'''
             }
        }
        stage('push docker image') {
            steps {
                sh 'docker push 912silver/devops1:latest'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('How are you?') {
            steps {
                echo 'How are you?'
            }
        }
        stage ('SAST'){
          parallel{
            stage ('sonar-scan'){
              steps {
                echo 'Running Sonar Scan'
              }
            }
            stage ('synk-scan'){
              steps {
                echo 'Synk scan'
              }
            }
            stage ('DC'){
              steps {
                echo 'Running Dependency Check'
              }
            }
          }
        }
    }
}
