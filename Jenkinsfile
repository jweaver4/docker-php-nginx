node {
    def servicePrincipalId = 'd890b33f-0a30-47c9-bac2-86b895de310a'
    def resourceGroup = 'TT-Jay-SandBox'
    def aks = 'TT-AKSCluster'
    def dockerRegistry = 'ttregistry.azurecr.us'
    def tenant = '24de1de9-7a5a-4c23-8a59-1f4bb77b12b9'
    def pass = '2ef18dfa-3337-455c-b034-6291e65e0f6b'
    def username = 'http://ttAKSClusterServicePrincipal'

    def app = ''
    def scannerHome = "/var/lib/jenkins/sonar-scanner-4.0.0.1744-linux"

    stage('Azure Login') {
      sh 'az cloud set --name AzureUSGovernment'

      withCredentials([usernamePassword(credentialsId: servicePrincipalId, passwordVariable: pass, usernameVariable: username'), string(credentialsId: tenant, variable: 'tenantId')]){

<<<<<<< HEAD
        sh 'az login --service-principal -u servicePrincipalId -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID'
      }
    }

    /* stage('Clone repository') {
     *    checkout scm
    }*/

    /* stage('Build image') {
     *  app = docker.build("pe-201642-agent.puppetdebug.vlan:5000/jayweaver/php_nginx:${env.BUILD_NUMBER}", '--no-cache --pull .')
    } */

    /* stage('Test image') {
     *  withSonarQubeEnv('sonarqube') {
     *      sh "${scannerHome}/bin/sonar-scanner"
     *   }
     *  timeout(time: 10, unit: 'MINUTES') {
     *     waitForQualityGate abortPipeline: true
    *   }
    } */

    /* stage('Push image to DTR') {
    *    /* Finally, we'll push the image with two tags:
    *     * First, the incremental build number from Jenkins
    *     * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
    *   docker.withRegistry('ttregistry.azurecr.us') {
    *      app.push("${env.BUILD_NUMBER}")
    *   }
    } */
=======
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

    }

    stage('Build container image & push to DTR') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
       docker.withRegistry('http://pe-201642-agent.puppetdebug.vlan:5000', 'portus_registry') {
            app = docker.build("pe-201642-agent.puppetdebug.vlan:5000/jayweaver/php_nginx:${env.BUILD_NUMBER}", '--no-cache --pull .')
            app.push("${env.BUILD_NUMBER}")
       }
    }
>>>>>>> 2168a59c9bb0296271acdbc6c6d1de594db717be

    /* stage('Remove existing container') {
      *  docker.withServer('tcp://pe-201642-agent:2375') {
      *          sh 'docker ps -f name=php_nginx -q | xargs --no-run-if-empty docker container stop'
      *          sh 'docker container ls -a -fname=php_nginx -q | xargs -r docker container rm'
      *        }
      * }

<<<<<<< HEAD
    *stage('Deploy new container') {
  	*    docker.withServer('tcp://pe-201642-agent:2375') {
		*	       docker.image("pe-201642-agent.puppetdebug.vlan:5000/jayweaver/php_nginx:${env.BUILD_NUMBER}").run("--name php_nginx -p 8080:8080")
    *    }
  	* }

    * stage('Prune Docker Images') {
    *    sh 'docker system prune -af'
    * }
        currentBuild.result = 'SUCCESS' */
=======
    stage('Deploy new container') {
  	    docker.withServer('tcp://pe-201642-agent:2375') {
			       docker.image("pe-201642-agent.puppetdebug.vlan:5000/jayweaver/php_nginx:${env.BUILD_NUMBER}").run("--name php_nginx -p 8080:8080")
        }
  	}
>>>>>>> 2168a59c9bb0296271acdbc6c6d1de594db717be
}
