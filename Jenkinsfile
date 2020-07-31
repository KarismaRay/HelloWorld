pipeline
{
   agent any
   stages
   { 
       stage('scm checkout')
       {
           steps {
               git branch: 'master', url: 'https://github.com/KarismaRay/HelloWorld.git'
                 }
        }
       stage('please test code')
       { 
          steps {
           withMaven(jdk: 'locakjdk-1.8', maven: 'localmaven') 
		    {
            sh 'mvn test'
            }
                }
    
       }
        stage('please build code')
       { 
	     steps {
           withMaven(jdk: 'locakjdk-1.8', maven: 'localmaven') 
		    {
             sh 'mvn package'
            }
               }

       }
      
	    stage ('dockerize:Build the  image') 
	   {
	  
          sh 'docker build -t kray992/HelloWorld_app:1.0.0 .'
	   }

        stage ('Push Docker image to DockerHub') 
		{
		 steps{
               withCredentials([string(credentialsId: 'dockerhubaccount', variable: 'dockerhubaccount')]) 
			   {
                sh "docker login -u kray992 -p ${dockerhubaccount}"
               }
               sh 'docker push kray992/HelloWorld_app:1.0.0'
	          }
        }

        stage ('Deploy to Dev') 
		{
		
           def dockerRun = 'docker run -d -p 9000:8080 --name my-tomcat-app kray992/HelloWorld_app:1.0.0'
           sshagent(['deploy-to-dev-docker']) 
		   {
           sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.46.1 ${dockerRun}"
           }
        }
   } 
}
