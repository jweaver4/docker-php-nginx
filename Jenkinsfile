node {

  stage('Clone repository') {
      checkout scm
  }

  stage("Pushing to Azure Storage") {
     sh 'az cloud set --name AzureUSGovernment'

     withCredentials([azureServicePrincipal('azsvcprincipal')]) {
        sh 'az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET -t $TENANT_ID'
        }
     }
}
