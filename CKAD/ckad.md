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

# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #

# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #






