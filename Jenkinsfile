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
  stage("Deploy") {
    acsDeploy azureCredentialsId: 'azsvcprincipal',
                  resourceGroupName: resourceGroup,
                  containerService: "${aks} | AKS",
                  configFilePaths: 'src/php.yaml',
                  enableConfigSubstitution: true
   }

   def verifyEnvironment = { service ->
           sh """
             endpoint_ip="\$(kubectl --kubeconfig=kubeconfig get services '${service}' --output json | jq -r '.status.loadBalancer.ingress[0].ip')"
             count=0
             while true; do
                 count=\$(expr \$count + 1)
                 if curl -m 10 "http://\$endpoint_ip"; then
                     break;
                 fi
                 if [ "\$count" -gt 30 ]; then
                     echo 'Timeout while waiting for the ${service} endpoint to be ready'
                     exit 1
                 fi
                 echo "${service} endpoint is not ready, wait 10 seconds..."
                 sleep 10
             done
           """
       }

   stage('Verify Deployment') {
       // verify the production environment is working properly
       verifyEnvironment('linux-php')
   }
}
