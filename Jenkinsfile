pipeline {
    agent any
         options { 
    buildDiscarder(logRotator(numToKeepStr: '10')) 
    timestamps()
    }
    parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'main', description: '')
    string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: '') 
    booleanParam(name: 'DEBUG_BUILD', defaultValue: true, description: '')
  }
    stages{
      stage('dynamic build name'){
        steps{
            script {
                currentBuild.displayName = "#${BUILD_NUMBER} -petclinic"
                currentBuild.description = "build for petclinic"
            }
        }
    }
    stages {
      when{
            expression { BRANCH_NAME ==~ /(main)/ }
        }
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
        stage("deploy to DEV"){
         when {
                expression { DEPLOY_ENV ==~ /(dev)/ }
        }
        steps{
        echo "deployment done in DEV server"
        }
    }
    stage("deploy to PROD"){
        when {
                expression { DEPLOY_ENV ==~ /(prod)/ }
        }
        steps{
        echo "deployment done in PROD server"
        }
    }
   }
 }
}
