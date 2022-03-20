import groovy.json.JsonSlurper

node {
    ws('netcore') {
        stage('SCM') {
            git branch: 'master', url: 'https://github.com/Stywar/ms-dotnet-5Marzo22'          
        }
        stage('Build') {
            dotnet_build();
        }
        stage('Docker') {
              //bat(script: 'docker login --username %UsernameDockerHub% --password %PasswordDocker%', returnStdout: true);
              bat(script: 'docker build -t antony0618/servicenet5 .', returnStdout: true);
              bat(script: 'docker push antony0618/servicenet5', returnStdout: true);            
        }
        stage('Deploy Dev') {
              bat(script: 'az login --service-principal --username 60445a5a-4c7d-404e-96a0-0d5c83c4978f --password PI-197rOGFr9LkKq0uE3uiKqd-qdBdJi9_  --tenant 74343d69-5375-4c7a-8cc9-08986488c964', returnStdout: true);
              bat(script: 'az account set --subscription "Developer"', returnStdout: true);
              bat(script: 'az container restart --name micro5testservice --resource-group aforo255Devops', returnStdout: true);  
        }
        stage('Deploy Prod') {
             bat(script: 'az aks get-credentials --resource-group aforo255Devops --name k8s-cluster-aforo255fv & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
             bat(script: 'kubectl config use-context k8s-cluster-aforo255fv --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
             bat(script: 'Kubectl delete --all pods --kubeconfig=%KUBE_PATH_CONFIG% & kubectl apply -f k8s.yml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
        }        
    }
}

def dotnet_build() {
    bat(script: 'dotnet restore', returnStdout: true);
    bat(script: 'dotnet build', returnStdout: true);
    bat(script: 'dotnet test', returnStdout: true);
}
