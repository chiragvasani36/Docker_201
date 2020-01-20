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
        
        stage ('code compile')
        {
			steps{
			sh 'mvn compile'
			}
		}
		
		stage('Sonarqube analysis')
		{
			steps{
				withSonarQubeEnv('sonar')
				{
					sh 'mvn sonar:sonar'
				}
			}	
		}	
	
		stage('Quality Gate')
		{
			steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
		}}
		stage('packaging') {
            steps {
                sh 'mvn clean install'
            }
        }
        
		stage('Archive Package in jfrog') 
		{
			steps{
			    script{
				def server= Artifactory.server 'artifactory'
				def uploadSpec= """{
                    "files": [{
                    "pattern": "target/*.war",
                    "target": "Jenkins-integration"}]
                }"""
                    server.upload (uploadSpec)}
			}
		}
		
        stage('container creation using docker compose') {
            steps {
                script {
                    sh 'docker-compose up -d --force-recreate --build'
                    }
                }	
            }
        
        stage('docker build and push') {
            steps 
            {
                //sh 'docker build -t chiragvasani36/gol_image:$BUILD_NUMBER ./gol/'
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
