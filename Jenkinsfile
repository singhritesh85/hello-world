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
    parameters{
        string(name: 'COMMIT_ID', defaultValue: '', description: 'Provide the Commit Id')
        string(name: 'TAG_NUMBER', defaultValue: '', description: 'Provide the Docker-Image-TagNumber')
    }
    stages{
        stage("Clone-Code"){
            steps{
                cleanWs()
                checkout([$class: 'GitSCM', branches: [[name: '${COMMIT_ID}']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-credentials', url: 'https://github.com/singhritesh85/hello-world.git']]])
            }
        }
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
