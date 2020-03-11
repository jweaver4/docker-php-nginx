de {

  stage('Clone repository') {
      checkout scm
  }

  stage("Pushing to Azure Storage") {
     az cloud set --name AzureUSGovernment

     withCredentials([azureServicePrincipal(credentialsId: 'azsvcprincipal',
                                   subscriptionIdVariable: 'SUBS_ID',
                                   clientIdVariable: 'CLIENT_ID',
                                   clientSecretVariable: 'CLIENT_SECRET',
                                   tenantIdVariable: 'TENANT_ID')]) {
        sh 'az login --service-principal -u $CLIENT_ID -p $CLIENT_SECRET -t $TENANT_ID'
        }
     }
}
