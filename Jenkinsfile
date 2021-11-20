pipeline {
    agent any
    environment {
        PATH = "/usr/share/maven/bin:$PATH"
    }

    stages {
        stage("clone code") {
            steps {
                git branch: 'master',
                    url: 'https://github.com/ValaxyTech/hello-world.git'
            }
        }
        stage("build code") {
            steps {
                sh "mvn clean install"
            }
        }
        /*
        
        stage("deploy") {
            steps {
                sh "cp -rf webapp/target/webapp.war /opt/tomcat/webapps "
            }
        }
        */
        stage("deploy") {
            steps {
                //sh "cp -rf webapp/target/webapp.war /opt/tomcat/webapps "
                sh "ls -la webapp/target"
                sh "scp -P 22220 -r webapp/target/webapp.war  root@127.0.0.1:/opt/tomcat/webapps"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
