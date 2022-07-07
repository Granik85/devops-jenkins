pipeline {

    agent any

    tools {
        // Note: this should match with the tool name configured in your jenkins instance (JENKINS_URL/configureTools/)
        maven "Maven 3.6.3"
    }

    environment {
        // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "127.0.0.1:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "repository-jenkins-snp"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus-credentials"

    }

    stages {
        stage("clone code") {
            steps {
                script {
                    //println ${JAVA_HOME}
                    // Let's clone the source
                    git credentialsId: "Github-SSH", branch: "${params.ESB_GIT_BRANCH_NAME}", url: "${params.git_repo}";
                }
            }
        }

        stage("mvn build") {
            steps {
                script {
                    def java_home = tool params.JAVA_VERSION_FOR_BUILD
                    env.JAVA_HOME = "${java_home}"
                    env.PATH = "${java_home}bin:${env.PATH}"
                    // If you are using Windows then you should use "bat" step
                    // Since unit testing is out of the scope we skip them
                    sh "java -version"
                    //sh "mvn package -DskipTests=true"
                    sh "mvn clean deploy -DskipTests=true"
                }
            }
        }


    }
}
