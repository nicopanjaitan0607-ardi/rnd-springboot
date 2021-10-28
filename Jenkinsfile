// Based on:
// https://raw.githubusercontent.com/redhat-cop/container-pipelines/master/basic-spring-boot/Jenkinsfile

library identifier: "pipeline-library@v1.5",
retriever: modernSCM(
  [
    $class: "GitSCMSource",
    remote: "https://github.com/redhat-cop/pipeline-library.git"
  ]
)

// The name you want to give your Spring Boot application
// Each resource related to your app will be given this name
appSourceUrl = "https://github/.com/monodot/hello.java.git"
appSourceRef = "develop"


appName = "hello-java-spring-boot"

pipeline {
    // Use the 'maven' Jenkins agent image which is provided with OpenShift 
    agent { label "maven" }
    stages {
        stage("Checkout") {
            steps {
                sh "mkdir ${appFolder}"
                dir(appFolder) {
                    git url: "${appSourceUrl}", branch: "${appSourceRef}"
                }
            }
        }

        stage("Get Version from POM"){            
            steps {
                script {
                    dir(appFolder) {
                        tag = readMavenPom().getVersion()
                    }
                }
            }

        }
        stage("Docker Build") {
            steps {
                // This uploads your application's source code and performs a binary build in OpenShift
                // This is a step defined in the shared library (see the top for the URL)
                // (Or you could invoke this step using 'oc' commands!)
                dir(appFolder) {
                binaryBuild(buildConfigName: appName, buildFromPath: ".")
            }
        }
    }

        // You could extend the pipeline by tagging the image,
        // or deploying it to a production environment, etc......
    }
}
