node('jnlp') {
   def app

    stage('Clone Repo') {
	    // Checkout SCM
         checkout scm    
    }    
  
    stage('Build Docker Image') {
      // build docker image
      app = docker.build("python-flask-buildcontroller")
    }

    stage('Publish Tests Results'){
      echo " Will be integrated with Testing platforms in Phase 2"
    }
    
   
    stage('Push image') {
        /* Finally, we'll push the image to ECR on latest tag. */
        sh "eval \$(aws ecr get-login --no-include-email --region us-west-1 | sed 's|https://||')"       
        docker.withRegistry('https://490747939488.dkr.ecr.us-west-1.amazonaws.com/python-flask-buildcontroller:latest', 'ecr:us-west-1:ecr-credentials') {
            docker.image('python-flask-buildcontroller').push('latest')
    }
    }

    stage('Helm Push Stage') {
        /* Finally, we'll push the image to Kubernetes Cluster. */
        sh "eval \$(aws ecr get-login --no-include-email --region us-west-1 | sed 's|https://||')"
        sh 'kubectl run python-flask-buildcontroller --image=490747939488.dkr.ecr.us-west-1.amazonaws.com/python-flask-buildcontroller:latest --port=80'
        sh 'kubectl expose deployment python-flask-buildcontroller --type LoadBalancer'
} 
    
    }