# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #
# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #

# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #
# Section2_coreConcepts

* Recap - pods with YML

pod-definition.yml 

apiVersion:
kind:
metadata:

spec:

---

Version -   v1, apps/v1

Kind -      Pod, Service, ReplicaSet, Deployment 

metadata:                   dictionary
    name: myapp-pod 
    labels:
        app: myapp
        type: frontend

spec:
    containers:             list / array
        - name:             - indicates item in the list
          image:   

> kubectl create -f pod-definition.yml 

> kubectl get pods 

> kubectl describe pod myapp-pod


# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #
- 27. Solution - Deployments Optional

kubectl create deployment --help 

kubectl create deployment httpd-frontend --image=httpd:2.4-alpine --replicas=3
kubectl get deploy 

- 29. Recap - Namespaces 

--namespace=dev

kubectl get pods --namespace=dev

kubectl get ns 
kubectl get pods --all-namespaces

kubectl get service -n=marketing 


# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #

- 31. Solution. Namespaces

kubectl get namespaces
or
kubectl get ns 

kubectl get pods --namespace=research
or
kubectl get pods -n=research

kubectl get services --namespace=dev
or
kubectl get svc -n=dev

db-service.dev.svc.cluster.local

- 32. Certification Tip

--dry-run
--dry-run=client 
-o yaml

Generate POD Manifest YAML file (don't create it [--dry-run])
kubectl run nginx --image=nginx --dry-run=client -o yaml

- 33. Imperative commands 

kubectl run redis --image=redis:alpine --dry-run=client -oyaml > redis-pod.yaml 

- 34. Solution

kubectl create service clusterip --help

# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #
# Section_3_configuration

- 37. Define, Build, and Modify container images 

FROM
RUN 
COPY 
ENTRYPOINT 

docker images 

- 39. Docker - commands and arguments 

docker run ubuntu sleep 5

ENTRYPOINT 
is the command instruction 

FROM        Ubuntu 
ENTRYPOINT  ["sleep"]
CMD         ["5"]       <-- default 

docker run ubuntu-sleeper 10
                          ^
                          entryPoint instr




# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #
# Section_4_

- 01. a subTitle 

- 01. a subTitle 

# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #






