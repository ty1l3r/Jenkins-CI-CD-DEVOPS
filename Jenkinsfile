pipeline {
    environment {
        DOCKER_ID = 'your-docker-id'
        DOCKER_TAG = "your-docker-tag"
        DOCKER_IMAGE_MOVIE = 'movie-fastapi'
        DOCKER_IMAGE_CAST = 'cast-fastapi'
        DOCKER_PASS = credentials('your-docker-pass')
        KUBECONFIG = credentials('config')
    }
    agent any

    stages {
        stage('Docker Build for movie-fastapi') {
            steps {
                script {
                    sh '''
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE_MOVIE:$DOCKER_TAG -f build/movie-service/Dockerfile .
                    docker login -u $DOCKER_ID -p $DOCKER_PASS
                    docker push $DOCKER_ID/$DOCKER_IMAGE_MOVIE:${DOCKER_TAG}
                    sleep 6
                    '''
                }
            }
        }

        stage('Docker Build for cast-fastapi') {
            steps {
                script {
                    sh '''
                    docker build -t $DOCKER_ID/$DOCKER_IMAGE_CAST:$DOCKER_TAG -f build/cast-service/Dockerfile .
                    docker login -u $DOCKER_ID -p $DOCKER_PASS
                    docker push $DOCKER_ID/$DOCKER_IMAGE_CAST:${DOCKER_TAG}
                    sleep 6
                    '''
                }
            }
        }

        stage('Deployment to Dev') {
            environment {
                VALUES_FILE = 'charts/chart-dev/values.yaml'
                VALUES_SECRET_FILE = 'charts/chart-dev/values-secret.yaml'
                NAMESPACE = 'dev'
                CHART_NAME = 'chart-dev'
                CHART_DIR = 'charts/chart-dev'
            }
            steps {
                script {
                    // Installation du Chart Dev
                    sh '''
                    helm uninstall ${CHART_NAME} -n ${NAMESPACE}
                    rm -Rf .kube
                    mkdir .kube
                    cp $KUBECONFIG .kube/config
                    cp ${VALUES_FILE} values.yml
                    cp ${VALUES_SECRET_FILE} values-secret.yml
                    sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                    helm upgrade --install ${CHART_NAME} ${CHART_DIR} \
                    --values=values.yml \
                    --values=values-secret.yml \
                    --namespace ${NAMESPACE} \
                    --wait \
                    --set fastapi_movie.tag=${DOCKER_TAG} \
                    --set fastapi_cast.tag=${DOCKER_TAG}
                    '''
                }
            }
        }

        stage('Deployment to qa') {
            environment {
                VALUES_FILE = 'charts/chart-qa/values.yaml'
                VALUES_SECRET_FILE = 'charts/chart-qa/values-secret.yaml'
                NAMESPACE = 'qa'
                CHART_NAME = 'chart-qa'
                CHART_DIR = 'charts/chart-qa'
            }
            steps {
                script {
                    // Installation du Chart QA
                    sh '''
                    helm uninstall ${CHART_NAME} -n ${NAMESPACE}
                    rm -Rf .kube
                    mkdir .kube
                    cp $KUBECONFIG .kube/config
                    cp ${VALUES_FILE} values.yml
                    cp ${VALUES_SECRET_FILE} values-secret.yml
                    sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                    helm upgrade --install ${CHART_NAME} ${CHART_DIR} \
                    --values=values.yml \
                    --values=values-secret.yml \
                    --namespace ${NAMESPACE} \
                    --wait \
                    --set fastapi_movie.tag=${DOCKER_TAG} \
                    --set fastapi_cast.tag=${DOCKER_TAG}
                    '''
                }
            }
        }

        stage('Deployment to staging') {
            environment {
                VALUES_FILE = 'charts/chart-staging/values.yaml'
                VALUES_SECRET_FILE = 'charts/chart-staging/values-secret.yaml'
                NAMESPACE = 'staging'
                CHART_NAME = 'chart-staging'
                CHART_DIR = 'charts/chart-staging'
            }
            steps {
                script {
                    // Installation du Chart Staging
                    sh '''
                    helm uninstall ${CHART_NAME} -n ${NAMESPACE}
                    rm -Rf .kube
                    mkdir .kube
                    cp $KUBECONFIG .kube/config
                    cp ${VALUES_FILE} values.yml
                    cp ${VALUES_SECRET_FILE} values-secret.yml
                    sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                    helm upgrade --install ${CHART_NAME} ${CHART_DIR} \
                    --values=values.yml \
                    --values=values-secret.yml \
                    --namespace ${NAMESPACE} \
                    --wait \
                    --set fastapi_movie.tag=${DOCKER_TAG} \
                    --set fastapi_cast.tag=${DOCKER_TAG}
                    '''
                }
            }
        }

        stage('Deploiement en prod') {
            environment {
                VALUES_FILE = 'charts/chart-prod/values.yaml'
                VALUES_SECRET_FILE = 'charts/chart-prod/values-secret.yaml'
                NAMESPACE = 'prod'
                CHART_NAME = 'chart-prod'
                CHART_DIR = 'charts/chart-prod'
            }
            steps {
                script {
                    // Demander une confirmation avant de déployer en production
                    timeout(time: 1, unit: 'HOURS') {
                        def userInput = input(
                            message: 'Êtes-vous sûr de vouloir déployer en production ?',
                            ok: 'Déployer',
                            parameters: [
                                [$class: 'BooleanParameterDefinition', name: 'confirm', defaultValue: false]
                            ]
                        )
                        if (userInput == false) {
                            error('Déploiement en production annulé.')
                        } else {
                            // Installation du Chart Prod
                            sh '''
                            rm -Rf .kube
                            mkdir .kube
                            cp $KUBECONFIG .kube/config
                            cp ${VALUES_FILE} values.yml
                            cp ${VALUES_SECRET_FILE} values-secret.yml
                            sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml
                            helm upgrade --install ${CHART_NAME} ${CHART_DIR} \
                            --values=values.yml \
                            --values=values-secret.yml \
                            --namespace ${NAMESPACE} \
                            --wait \
                            --set fastapi_movie.tag=${DOCKER_TAG} \
                            --set fastapi_cast.tag=${DOCKER_TAG}
                            '''
                        }
                    }
                }
            }
        }
    }
}
