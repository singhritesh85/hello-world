pipeline {
    agent {
        node{
            label "Slave-1"
            customWorkspace "/home/jenkins/newdir"
        }
    }
    tools{
        jdk "JAVA"
        maven "Maven3"
    }
    stages{
        //stage("Clone-Code"){
            //steps{
                //cleanWs()
            //}
        //}
        stage("Build Code"){
            steps{
                sh 'mvn clean install'
            }
        }
        stage("Deployment"){
            steps{
                sh 'scp -rv webapp/target/webapp.war "tomcat-prod"@"18.118.155.156":/opt/apache-tomcat/webapps/'
            }
        }
    }
}
