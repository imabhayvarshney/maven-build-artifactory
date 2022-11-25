pipeline{
    agent any
    tools{
        maven 'MAVEN_HOME'
    }

    environment {
        CI = true
        ARTIFACTORY_ACCESS_TOKEN = credentials('az-container-artifactory-token')
    }

    stages{
        stage ("maven build"){
            steps{
                sh 'mvn clean package'
            }
        }

        stage ("upload to artifactory"){
            steps {
              script { 
                 def server = Artifactory.server 'jfrog-artifactory'
                 def uploadSpec = """{
                    "files": [{
                       "pattern": "/var/jenkins_home/workspace/ci-maven-artifactory/target/restro.war",
                       "target": "restro-mvn-local/"
                    }]
                 }"""

                 server.upload(uploadSpec) 
               }
            }

        }
    }
}
