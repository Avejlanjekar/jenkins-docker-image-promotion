pipeline{
    agent any

    environment{
        DEV_DOCKER_IMAGE= 'avejlanjekar45/dev-docker-image'
        QA_DOCKER_IMAGE= 'avejlanjekar45/qa-docker-image'
        STAGE_DOCKER_IMAGE= 'avejlanjekar45/stage-docker-image'

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
                        "${DEV_DOCKER_IMAGE}:${GIT_COMMIT}"
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