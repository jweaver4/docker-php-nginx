node {

  stage('Clone repository') {
      checkout scm
  }

  stage("Pushing to Azure Storage") {
     az cloud set --name AzureUSGovernment

     withCredentials([azureServicePrincipal('azsvcprincipal')]) {
        sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
        }
     }
}
