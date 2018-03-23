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
      logstashSend failBuild: true, maxLines: 1000
	    
    }
    
   
    stage('Push image') {
        /* Finally, we'll push the image to ECR on latest tag. */
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          sh 'docker login -u $USERNAME -p $PASSWORD'
          sh 'docker tag python-flask-buildcontroller:latest anandvelusamy/python-flask-buildcontroller:latest'
          sh 'docker push anandvelusamy/python-flask-buildcontroller:latest'
          sh 'python --version'
          sh 'kubectl run python-flask-buildcontroller --image=anandvelusamy/python-flask-buildcontroller --port=80'
          sh 'kubectl expose deployment python-flask-buildcontroller --type=LoadBalancer' 
          sh 'sleep 20s'
          sh 'kubectl describe deployments python-flask-buildcontroller'
          sh 'kubectl describe svc python-flask-buildcontroller' 
    }
    }   
  }
