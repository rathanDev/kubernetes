# Setup

    minikube start --driver=docker

    minikube status

    kubectl cluster-info

    kubectl get nodes

    minikube docker-env

# Deployment

    kubectl create deployment plan-finder-k8s --image=rathandev/plan-finder --dry-run=client -o=yaml > deployment.yaml

    echo --- >> deployment.yaml

    kubectl create service clusterip plan-finder-k8s --tcp=8080:8080 --dry-run=client -o=yaml >> deployment.yaml

    kubectl apply -f deployment.yaml 

# Expose service using SSH tunnel

    kubectl port-forward svc/plan-finder-k8s 8080:8080

# Test 
    
    http://localhost:8080/plan
    