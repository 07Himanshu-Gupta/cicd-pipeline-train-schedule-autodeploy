pipeline {
    agent any
    environment {
        //be sure to replace "bhavukm" with your own Docker Hub username
        //DOCKER_IMAGE_NAME = "ajaygaharana1/train-schedule"
        DOCKER_IMAGE_NAME = "himanshugupta171020/train-schedule"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'                      
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
               
        }
        }
        stage('Build Docker Image') {
            //when {
            //    branch 'master'
            //}
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            //when {
              //  branch 'master'
            //}
            steps {
                sh script: 'docker login -u himanshugupta171020 -p Gh@17102077' 
                script {
                    //withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://hub.docker.com/r/ajaygaharana1') {
                    //withDockerRegistry(credentialsId: 'DOCKER_HUB_LOGIN', url: 'https://hub.docker.com/r/ajaygaharana1/project2') {
                                        
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        
        stage('Deploy to K8s') {
		when {
              branch 'master'
            }
  	   steps {
    		
    		sh 'kubectl apply -f train-schedule-kube.yml'
	   }
	   post { 
              always { 
                cleanWs() 
	      }
	   }
	}   
      
    }       
}
