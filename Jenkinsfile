#!/usr/bin/env groovy
node {
def app = ''

  stage('Clone repository') {
      checkout scm
  }

  stage('Test image') {
     scannerHome = tool 'Ssonarqube';
      withSonarQubeEnv('sonarqube') {
        /*  sh "${scannerHome}/bin/sonar-scanner"*/
        sh "env"
      }
      timeout(time: 10, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
      }
  }

/*  stage("Build & Push to Azure Storage") {
  *   sh 'az cloud set --name AzureUSGovernment'

  *   withCredentials([azureServicePrincipal(credentialsId: 'azsvcprincipal',
  *                                       subscriptionIdVariable: 'SUBS_ID',
  *                                       clientIdVariable: 'CLIENT_ID',
  *                                       clientSecretVariable: 'CLIENT_SECRET',
  *                                       tenantIdVariable: 'TENANT_ID')]) {
  *       sh 'az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET -t $TENANT_ID'
  *       sh 'az acr login --name ttregistry'

  *       app = docker.build("ttregistry.azurecr.us/linux/php:${env.BUILD_NUMBER}")
  *       app.push("${env.BUILD_NUMBER}")
  *   }
   } */
}
