pipeline{
    agent any

    environment{
        DOCKER_IMAGE= 'avejlanjekar45/jenkins-docker-promotion'
        REGISTRY_URL= 'https://registry.hub.docker.com'
        CREDENTIALS= 'dockerhub-credentials'
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
                        "${DOCKER_IMAGE}:${GIT_COMMIT}"
                    )

                    docker.withRegistry(
                        "${REGISTRY_URL}",
                        "${CREDENTIALS}"
                    )

                    {
                        app.push()
                    }
                }
            }
        }
    }
}