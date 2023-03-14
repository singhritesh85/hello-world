pipeline{
    agent {
        node{
            label "Slave-1"
            customWorkspace "/home/jenkins/newdir"
        }
    }
    environment{
        JAVA_HOME="/usr/lib/jvm/java-11-openjdk-11.0.18.0.10-1.el7_9.x86_64"
        PATH="$PATH:$JAVA_HOME/bin:/opt/apache-maven/bin"    //:$HOME/bin"  // $HOME/bin used for aws-iam-authenticator in eks
    }
    parameters {
        string(name: 'COMMIT_ID', defaultValue: '', description: 'Provide the commit Id for the build')
    }
    stages{
        stage("Clone-Code"){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '${COMMIT_ID}']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-cred', url: 'https://github.com/singhritesh85/hello-world.git']]])
            }
        }
        stage("Build"){
            steps{
                sh 'mvn clean install'
                sh 'scp -rv webapp docker-user@35.199.60.111:~/'
                sh 'scp -rv Dockerfile docker-user@35.199.60.111:~/'
                sh 'scp -rv docker-compose.yaml docker-user@35.199.60.111:~/'
            }
        }
        stage("Deployment"){
            steps{
                sh '''
                       ssh docker-user@35.199.60.111 << -EOT
                       docker-compose -p newproject build --no-cache
                       docker tag myimage:1.06 027330342406.dkr.ecr.us-east-2.amazonaws.com/newecr:latest-prod
                       eval $(aws ecr get-login --no-include-email --region us-east-2)
                       docker push 027330342406.dkr.ecr.us-east-2.amazonaws.com/newecr:latest-prod
                       docker-compose -p newproject down
                       docker-compose -p newproject up -d
                       docker ps
                       exit
                       EOT
                '''
            }
        }
    }
}
