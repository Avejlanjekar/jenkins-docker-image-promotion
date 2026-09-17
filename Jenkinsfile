pipeline{
    agent any

    environment{
        DEV_DOCKER_IMAGE= 'avejlanjekar45/dev-docker-image'
        QA_DOCKER_IMAGE= 'avejlanjekar45/qa-docker-image'
        STAGE_DOCKER_IMAGE= 'avejlanjekar45/stage-docker-image'
        PROD_DOCKER_IMAGE= 'avejlanjekar45/prod-docker-image'


        REGISTRY_URL= 'https://registry.hub.docker.com'
        DEV_DH_CREDENTIALS= 'dev-dockerhub-credentials'
        QA_DH_CREDENTIALS= 'qa-dockerhub-credentials'
        STAGE_DH_CREDENTIALS= 'stage-dockerhub-credentials'
        PROD_DH_CREDENTIALS= 'prod-dockerhub-credentials'
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
                        "${DEV_DH_CREDENTIALS}"
                    )

                    {
                        app.push()
                    }
                }
            }
        }

        stage('promoto dev to qa'){
            steps{
                script{
                    docker.withRegistry(
                        "${REGISTRY_URL}",
                        "${DEV_DH_CREDENTIALS}"
                    ){
                        docker.image("${DEV_DOCKER_IMAGE}:${GIT_COMMIT}").pull()
                    }

                    sh "docker tag ${DEV_DOCKER_IMAGE}:${GIT_COMMIT} ${QA_DOCKER_IMAGE}:${GIT_COMMIT}"

                    docker.withRegistry("${REGISTRY_URL}","${QA_DH_CREDENTIALS}"){
                        docker.image("${QA_DOCKER_IMAGE}:${GIT_COMMIT}").push()
                    }
                }
            }
        }

        stage('promote QA to stage'){
            steps{
                script{
                    docker.withRegistry("${REGISTRY_URL}","${QA_DH_CREDENTIALS}"){
                        docker.image("${QA_DOCKER_IMAGE}:${GIT_COMMIT}").pull()
                    }

                    sh "docker tag ${QA_DOCKER_IMAGE}:${GIT_COMMIT} ${STAGE_DOCKER_IMAGE}:${GIT_COMMIT}"

                    docker.withRegistry("${REGISTRY_URL}","${STAGE_DH_CREDENTIALS}"){
                        docker.image("${STAGE_DOCKER_IMAGE}:${GIT_COMMIT}").push()
                    }
                }
            }
        }

        stage('Promote stage to Prod'){
            steps{
                script{
                    docker.withRegistry("${REGISTRY_URL}","${STAGE_DH_CREDENTIALS}"){
                        docker.image("${STAGE_DOCKER_IMAGE}:${GIT_COMMIT}").pull()
                    }

                    sh "docker tag ${STAGE_DOCKER_IMAGE}:${GIT_COMMIT} ${PROD_DOCKER_IMAGE}:${GIT_COMMIT}"

                    docker.withRegistry("${REGISTRY_URL}","${PROD_DH_CREDENTIALS}"){
                        docker.image("${PROD_DOCKER_IMAGE}:${GIT_COMMIT}").push()
                    }
                }
            }
        }
    }
}