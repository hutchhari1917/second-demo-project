pipeline {
    agent any
    environment{
        PATH = "/opt/apache-maven-3.8.3/bin:$PATH"
		<!-- maven path , maven should install in jenkins server -->
    }
    stages {
        stage ('Git Clone') {
            steps {
                 git 'https://github.com/hutchhari1917/war-demo-project.git'
            }
        }
        stage ('Build maven-war') {
            steps {
                 sh "mvn clean install"
            }
        }
        stage ('War file to Nexus') {
            steps {
                 nexusArtifactUploader artifacts: [
                    [ artifactId: 'second-demo-project', 
                      classifier: '', 
                      file: 'target/second-demo-project.war',
                      type: 'war'
                    ]
                ], 
                      credentialsId: 'nexus',
                      groupId: 'com.democompany', 
                      nexusUrl: '3.16.23.70:8081', 
                      nexusVersion: 'nexus3',
                      protocol: 'http',
                      repository: 'war-demo-project', 
                      version: '1.0.0'
            }
        }
        stage ('Deploy to Tomcat') {
            steps {
                 sshagent(['sshkey']) {
               sh "scp -o StrictHostKeyChecking=no target/second-demo-project.war ec2-user@18.224.33.235:/opt/apache-tomcat-8.5.72/webapps"
               
				 }
            }
        }
    }
}
