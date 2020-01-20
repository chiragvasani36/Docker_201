pipeline {
	
	environment {
    registryCredential = 'dockerhub'
   
    }
	
    agent any
    stages {
        stage('git checkout') {
            steps {
               git 'https://github.com/chiragvasani36/Docker_201.git'
            }
        }
        
       
        
		stage('packaging') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('container creation using docker compose') {
            steps {
                script {
                    sh 'docker-compose up -d --force-recreate --build'
                    }
                }	
            }
        
        stage('docker tag and push') {
            steps 
            {
                
                sh 'docker tag docker201_frontend chiragvasani36/docker201_frontend:$BUILD_NUMBER'
                script {
                    docker.withRegistry( '', registryCredential ) {
                    
                    sh 'docker push chiragvasani36/docker201_frontend:$BUILD_NUMBER' 
                    }
                }
            }
	    }
		
		
			   
        
    }
    
    
}
