# jenkins-Gitflow
Here is a simple gitflow to deploy static website on 4 different environemt (dev, test, preprod and prod) from 4 different github branches.

Using minikube kubernates cluster for deployment with helm and jenkins to build the ci/cd multibranch pipeline on some jenkins slave pods.

On each pipeline jenkins file, i build and push docker image from the current branch to dockerhub to a specific dockerhub image with the name of the branch to then run the manifest kubernates deployment file from the slave pod to deploy the app to the required environment.
![z (1)](https://github.com/mohamedsamirspot/jenkins-Gitflow/assets/71722372/bf76b16e-fb25-4274-aeb9-4b137067e488)

