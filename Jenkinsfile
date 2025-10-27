pipeline {
    agent any
        options {
        timeout(time: 1, unit: 'HOURS') 
        timestamps()
        }
    stages {
        stage('git clone') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkins-github-ssh-key', url: 'git@github.com:gayathrimallisetti118-sys/spring-petclinic.git']])
            }
         }
         stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
         }
         stage('junit artifacts') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
         }
        stage('archive artifacts') {
            steps {
               archiveArtifacts artifacts: 'target/*.jar'
           }
        }  
        stage('deploy to tomcat'){
         steps {
             sh 'sudo cp -r "/var/lib/jenkins/workspace/pipelines/jenkins-maven method/target/"* /home/mallisetti/tomcat/webapps/'
            }
        }
    }
}
