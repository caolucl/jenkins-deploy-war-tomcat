pipeline {
    agent any
    environment {
        PATH = "/usr/share/maven/bin:$PATH"
        SSH_CONFIG_NAME = "localhost"
    }

    stages {
        stage("clone code") {
            steps {
                git branch: 'master',
                    url: 'https://github.com/caolucl/jenkins-deploy-war-tomcat.git'
            }
        }
        stage("build code") {
            /*
            steps {
                sh "mvn clean install"
                
            }
            */
            steps {
                script {
                    try {
                    if(isUnix()){
                          sh 'mvn clean package'
                    }else{
                       bat 'mvn clean package'
                      }
                    }
                    catch(err) {
                        throw err
                    }
                }    
            }
        }
        /*
        
        stage("deploy") { //using copy on the same server
            steps {
                sh "cp -rf webapp/target/webapp.war /opt/tomcat/webapps "
            }
        }
        */
        /*
        stage("deploy") { //copy id_rsa.pub to the .ssh root and using command ssh
            steps {
                //sh "cp -rf webapp/target/webapp.war /opt/tomcat/webapps "
                sh "ls -la webapp/target"
                sh "scp -P 22220 -r webapp/target/webapp.war  root@127.0.0.1:/opt/tomcat/webapps"
            }
        }
        */
         stage('SSH transfer') { //using scp
			steps {
				script {
				sshPublisher(
			   continueOnError: false, failOnError: true,
			   publishers: [
				sshPublisherDesc(
				 configName: "${env.SSH_CONFIG_NAME}",
				 //configName: "localhost",
				 verbose: true,
				 transfers: [
				  sshTransfer(
				   sourceFiles: "webapp/target/webapp.war",
				   removePrefix: "webapp/target/",
				   remoteDirectory: "/opt/tomcat/webapps",
				   execCommand: "ls -la /opt/tomcat/webapps "
				  )
				 ])
			   ])
			 }
			}
		}
    }

    post {
        always {
            cleanWs()
        }
    }
}
