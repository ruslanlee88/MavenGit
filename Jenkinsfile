pipeline {
	agent any
	stages {
		stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "artifactory.version:  6.15.0",
                    url: "http://localhost:9000/artifactory",
                    username: "admin",
					password: "password"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "artifactory.version:  6.15.0",
                    releaseRepo: "repository-key-local",
                    snapshotRepo: "repository-key-ss-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "artifactory.version:  6.15.0",
                    //releaseRepo: "libs-release",
                    //snapshotRepo: "libs-snapshot"
					releaseRepo: "repository-key-local",
                    snapshotRepo: "repository-key-ss-local"

                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: "MAVEN_TOOL", // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    //deployerId: "MAVEN_DEPLOYER",
                   //resolverId: "MAVEN_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "artifactory.version:  6.15.0"
                )
            }
        }
	}
	/***
	  post {
		success {
			mail to: "XXXXX@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed."
		}
		failure {
			mail to: "XXXXX@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Boo, we failed."
		}
	 }
	***/
}