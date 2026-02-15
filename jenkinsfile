pipeline {
    agent any
    tools {
        nodejs 'NodeJS 18' //Replace NodeJS 18 with the name configured in Global Tool Configuration
    }
    stages {
           stage('codecheckout')
        {
         steps  
          {
            git branch:'deploy' , url:'https://github.com/MYPROJECTS555/React-js-node_web_application.git'
          } 
        }
        stage('Node.js Build') {
            steps {
                sh 'node -v'
                sh 'npm install'
                sh 'npm run build'
            }
        }
       stage('docker-build') 
        {
            steps 
            {
             sh ' docker build -t nameisvikas/node:1 .'         // (cat /etc/group) , (usermod -aG jenkins docker) ,visudo command =(jenkins  ALL=(ALL:ALL) NOPASSWD: /usr/bin/docker) this is giving the permission to docker and jenkins                                                                                                                                                   
            }                        
        }
      stage('trivy-docker-image-scanner') 
        {
            steps 
            {
               sh 'trivy image --format table -o trivy_report.html nameisvikas/node:1 '                                                                                
            }                        
        }
       stage('container-creation') 
        {
            steps 
            {
              sh 'docker run -it -d --name node_container -p 3000:3000 nameisvikas/node:1'     // automaticall conatiner is created                                                                         
            }                        
        }
        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                        }
                    }
        }
        stage('Pushing image to repository'){
            steps{
                sh 'docker push nameisvikas/node:1'
            }
        }
          stage('message')
                {
            steps
                  {
               echo 'pipeline is running successfully'
            }
        }
    }
}
