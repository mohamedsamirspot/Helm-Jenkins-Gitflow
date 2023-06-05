pipeline {
    agent { label 'my-slave-label-onWhich-i-will-choose-to-run-the-pipeline-on-it' }
    stages {
        stage('build') {
            steps {
                echo 'build'
                script {
                    withCredentials([usernamePassword(credentialsId: 'my-docker-hub-cred', usernameVariable: 'MY_USERNAME', passwordVariable: 'MY_PASSWORD')]) {
                        sh '''
                            docker login -u ${MY_USERNAME} -p ${MY_PASSWORD}
                            docker build -t mohamedsamirebrahim/carrepair${BRANCH_NAME}:v${BUILD_NUMBER} .
                            docker push mohamedsamirebrahim/carrepair${BRANCH_NAME}:v${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                    withCredentials([file(credentialsId: 'kubeconfig-for-my-slave', variable: 'MY_KUBECONFIG')]) {
                        sh '''
                            # Check if release already exists
                            if helm status "${BRANCH_NAME}" --kubeconfig "${MY_KUBECONFIG}" >/dev/null 2>&1; then
                                helm upgrade "${BRANCH_NAME}" ./Deployment -f ./Deployment/${BRANCH_NAME}values.yaml --set image.repository=mohamedsamirebrahim/carrepair${BRANCH_NAME},image.tag=${BUILD_NUMBER} --kubeconfig "${MY_KUBECONFIG}"
                            else
                                helm install "${BRANCH_NAME}" ./Deployment -f ./Deployment/${BRANCH_NAME}values.yaml --set image.repository=mohamedsamirebrahim/carrepair${BRANCH_NAME},image.tag=${BUILD_NUMBER} --kubeconfig "${MY_KUBECONFIG}"
                            fi
                        '''
                    }
                }
            }
        }
    }
}