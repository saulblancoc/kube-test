# How to get your first app on kubernetes

## Steps are:
  - Set up your kubernetes dependencies.  
    - Install docker engine from Ubuntu's repos.  
      ```
      sudo apt install docker.io
      ```  
    - Install minikube, you can chose your prefered way of installation. I did it directly from the package.  
      ```
      curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_arm64.deb  
      sudo dpkg -i minikube_latest_arm64.deb  
      ```
    - Install kubectl 
      ```
      sudo apt install kubectl
      ```
    - Install an ingress controller with helm.
      - Helm installation
      ```
      wget https://get.helm.sh/helm-v3.11.1-linux-arm64.tar.gz
      tar -xzvf  helm-v3.11.1-linux-arm64.tar.gz
      dpkg -i helm
      curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
      chmod 700 get_helm.sh
      ./get_helm.sh
      ```
      - I went for Apache APISIX ingress controller. Careful with helm, as it installs packages without architectures checks. If you are in ARM as I was, this wont work for you and you will need to build an ARM64 architecture ingress controller image.   
      ```
      helm repo add apisix https://charts.apiseven.com
      helm repo add bitnami https://charts.bitnami.com/bitnami
      helm repo update
      helm install apisix apisix/apisix   --set gateway.type=NodePort   --set ingress-controller.enabled=true   --create-namespace   --namespace ingress-apisix   --set ingress-controller.config.apisix.serviceNamespace=ingress-apisix   --set ingress-controller.config.apisix.adminAPIVersion=v3
      ```
  - Build the image of the app you want to run.
    - You will need the application and a Dockerfile. In my case, I have gone with a very simple php that prints the date and hostname of its host and then saved it as index.php.
      
