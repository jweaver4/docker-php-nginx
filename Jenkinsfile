#!/usr/bin/env groovy
node {
def app = ''

def servicePrincipalId = 'd890b33f-0a30-47c9-bac2-86b895de310a'
def resourceGroup = 'TT-Jay-SandBox'
def aks = 'TT-AKSCluster'

  stage('Clone repository') {
      checkout scm
  }

  stage('Test image') {
     scannerHome = tool 'sonarqube';
      withSonarQubeEnv('sonarqube') {
        sh "${scannerHome}/bin/sonar-scanner"
      }
      timeout(time: 10, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
      }
  }

  stage("Build & Push to Azure Storage") {
     sh 'az cloud set --name AzureUSGovernment'

     withCredentials([azureServicePrincipal(credentialsId: 'azsvcprincipal',
                                         subscriptionIdVariable: 'SUBS_ID',
                                         clientIdVariable: 'CLIENT_ID',
                                         clientSecretVariable: 'CLIENT_SECRET',
                                         tenantIdVariable: 'TENANT_ID')]) {
         sh 'az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET -t $TENANT_ID'
         sh 'az acr login --name ttregistry'

        /* app = docker.build("ttregistry.azurecr.us/linux/php:${env.BUILD_NUMBER}")
         app.push("${env.BUILD_NUMBER}") */
     }
   }
  stage("Push Container to Kubernetes") {
    acsDeploy azureCredentialsId: servicePrincipalId,
                  resourceGroupName: resourceGroup,
                  containerService: "${aks} | AKS",
                  configFilePaths: 'src/php.yml',
                  enableConfigSubstitution: true
     sh 'kubectl apply -f php.yaml'
   }
}
