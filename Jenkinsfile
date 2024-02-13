pipeline{
    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'adminadmin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '192.168.56.102'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        /** #NEXUS_VERSION = "nexus3"
        #NEXUS_PROTOCOL = "http"
        #NEXUS_URL = "172.31.86.22:8081"
        #NEXUS_REPOSITORY = "vprofile-release"
	    #NEXUS_REPOGRP_ID    = "vprofile-grp-repo"
        #NEXUS_CREDENTIAL_ID = "nexuslogin"
        #ARTVERSION = "${env.BUILD_ID}" **/
    }

    stages{
        stage('Build'){
            steps{
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

	stage('INTEGRATION TEST'){
            steps {
                sh 'mvn -s settings.xml verify -DskipUnitTests'
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
    }
}