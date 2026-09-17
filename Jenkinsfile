pipeline{
    agent any

    environment{
        Docker_image= 'avejlanjekar45/jenkins-docker-promotion'
    }

    stages{
        stage("checkout"){
            steps{
                checkout scm
            }
        }

        stage("Build docker image"){
            steps{
                script{
                    def app= docker.build(
                        "${Docker_image}:${GIT_COMMIT}"
                    )
                }
            }
        }
    }
}