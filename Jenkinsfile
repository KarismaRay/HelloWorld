pipeline
{
   agent any
   stages
   { 
       stage('scm checkout')
       {
           steps {
               git branch: 'master', url: 'https://github.com/prakashk0301/maven-project'
                 }
       }

       stage('please compile code')
       { steps {
           withMaven(jdk: 'locakjdk-1.8', maven: 'localmaven') {
            sh 'mvn compile'
}
       }

       }
      
           stage('please test code')
       { steps {
           withMaven(jdk: 'locakjdk-1.8', maven: 'localmaven') {
            sh 'mvn test'
}
       }

       }
        stage('please build code')
       { steps {
           withMaven(jdk: 'locakjdk-1.8', maven: 'localmaven') {
            sh 'mvn package'
}
       }

       }
      
      stage('deploy to tomcat')
       {
         steps
          {
            sshagent(['3d7efb47-ebdb-41fc-ba84-64adcad73b85']) {
            sh 'scp -o StrictHostKeyChecking=no */target/webapp.war ec2-user@172.31.9.195:/var/lib/tomcat/webapps'
}
}
}
   } 
}
