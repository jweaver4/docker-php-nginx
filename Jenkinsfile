node {
    def app = ''
    def scannerHome = "/var/lib/jenkins/sonar-scanner-4.0.0.1744-linux"
  /*  EMAIL_TO = 'jay.weaver@usda.gov' */

 /* try { */
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("pe-201642-agent.puppetdebug.vlan:5000/jayweaver/php_nginx:${env.BUILD_NUMBER}", '--no-cache --pull .')
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image. */
        //sh 'echo "Tests passed"'
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }

    stage('Push image to DTR') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
       docker.withRegistry('http://pe-201642-agent.puppetdebug.vlan:5000', 'portus_registry') {
          app.push("${env.BUILD_NUMBER}")
       }
    }

    stage('Remove existing container') {
        docker.withServer('tcp://pe-201642-agent:2375') {
          sh 'docker ps -f name=php_nginx -q | xargs --no-run-if-empty docker container stop'
          sh 'docker container ls -a -fname=php_nginx -q | xargs -r docker container rm'
        }
    }

    stage('Deploy new container') {
  	    docker.withServer('tcp://pe-201642-agent:2375') {
			       docker.image("pe-201642-agent.puppetdebug.vlan:5000/jayweaver/php_nginx:${env.BUILD_NUMBER}").run("--name php_nginx -p 8080:8080")
        }
  	}

    stage('Prune Docker Images') {
        sh 'docker system prune -af'
    }
        currentBuild.result = 'SUCCESS'
  }
/*  catch (err) {
    currentBuild.result = 'FAILURE'
  }
  finally {
    mail to: EMAIL_TO,
         from: 'jenkins.fs@usda.gov',
         subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
         body: "${env.BUILD_URL} has result ${currentBuild.result}"
  }*/
}
