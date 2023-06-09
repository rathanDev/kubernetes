
# Setup

    kubectl version
    
    minikube version

    minikube start --driver=docker

    minikube status

    kubectl cluster-info

    kubectl get nodes

    minikube docker-env

# Prepare docker image

    docker images
        rathanDev/plan-finder  latest

# Deployment

    kubectl get deployments
        kubectl delete deployment plan-finder-k8s
    
    kubectl create deployment plan-finder-k8s --image=rathandev/plan-finder:latest --port=8080
    
    kubectl get deployments
    
    kubectl describe deployment plan-finder-k8s
    
    kubectl get pods
    
    kubectl logs <podName>
    
    kubectl expose deployment plan-finder-k8s --type=NodePort
    
    kubectl get service
    
    minikube service plan-finder-k8s --url
        http://127.0.0.1:57653/plan

# Clean up

    kubectl get service
    kubectl delete service plan-finder-k8s

    kubectl get deployments
    kubectl delete deployment plan-finder-k8s

    minikube stop 

    minikube delete 



