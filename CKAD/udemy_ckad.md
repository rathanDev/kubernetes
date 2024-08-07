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

kubectl run redis --image=redis:alpine --dry-run=client -o yaml > redis-pod.yaml 

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

- 41. Quick note on editing pod and deployment

* For Pod
kubectl edit pod <podName>

kubectl get pod webapp -o yaml > my-new-pod.yaml
vi my-new-pod.yaml
kubectl delete pod webapp
kubectl create -f my-new-pod.yaml

* For Deployment
kubectl edit deployment my-deployment

k replace --force -f /tmp/kubectk-edit-222.yaml

- 43. Commands and Arguments

spec:
  containers:
  - command:
    - sleep
    - "4800"
    image: ubuntu
    name: ubuntu

- 44. Environment variables

- 45. ConfigMaps 
create config map
inject config map into pod

config-map.yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    APP_COLOR: blue
    APP_MODE: prod 

kubectl create configmap -f cm.yaml

kubectl describe configMaps 

- 47. Solution ConfigMaps 

kubectl get pods
k get pod webapp-color -o yaml > pod.yaml
vi pod.yaml #makeChanges 

- 48. Secrets 
Create secret 
Inject into pod

apiVersion:
kind:
metadata:
    name: app-secret
data:
    DB_Host: mysql
    DB_User: <root>
    DB_Password: <password>

kubectl describe secrets 
k get secret app-secret -o yaml

* secrets are not encrypted, just encoded 

secrets stored in ETCD are not encrypted

Encrypting data at rest 


- 53. Encrypting Secret data at Rest 

k get secret my-secret -o yaml 
echo ""sdagagxzc" | base64 --decode

etcdctl     <- et cd cuttle utility

- 56. Security contexts

whoami

k exec ubuntu-sleeper -- whoami 

spec:
    securityContext:
        runAsUser: 1010

k delete pod ubuntu-sleeper --force

- 58. Service Account 

UserAccount vs ServiceAccount 

- 64. Taints and tolerations

k run bee --image=nginx --dry-run=client -o yaml

k run bee --image=nginx --dry-run=client -o yaml > bee.yaml

add toleration section

k get pods -o wide 

k taint node controlplane node-role.kubernetes.io/control-plane:NoSchedule-
                                                                          ^ to remove

- 67. Node Selectors

spec:
    nodeSelector: large

Node Affinity and Anti Affinity

affinity:
    nodeAffinity:
        ...


- 75. Multi container pods

Ambassador
Adapter
Sidecar

SideCar - logging agent

adapter - converting logs to a common format 

Ambassador - directing the logs to right db

k -n elastic-stack exec -it app -- cat /log/app.log


- 99. Practice - Multi container pods

k run yellow --image=redis --dry-run=client -o yaml

k -n elatic-stack logs kibana

k logs app -n elastic-stack  <tab>

k -n es exec -it app -- cat /app/log


- 78. Init Containers
https://kubernetes.io/docs/concepts/workloads/pods/init-containers/


- 81. Readiness and Liveness Probe

Observability 

POD conditions:
PodScheduled
Initialized
ContainersReady
Ready

readinessProbe:
    httpGet:
        path: /api/ready
        port: 8080
    initialDelaySeconds: 10
    periodSeconds: 5

- 82. Liveness Probes

livenessProbe:
    httpGet:
        path: /api/healthy
        port: 8080


- 85. Container logging

k logs simple-node simple-app

- 88. Monitor and Debug Applications

node level matrix 

Metrix server   -   CKAD 
Prometheus 
Elastic stack
Datadog
Dynatrace

Kubelet 
cAdvisor 

> k top node

git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git

k create -f .

k get services --all-namespaces

k top node
vs
k top pod 

- 91. Labels, Selectors and Annotations

Labels and selectors 
are standard method to group things together 

k get pods --selector env=dev 

k get pods --selector bu=finance

k get all --selector env=prod

k get all --selector --no-headers | wc -l

wc - word count 

k get all --selector env=prod,bu=finance,tier=frontend

- 94. Rolling Updates & Rollback in Deployments 

Recreate vs RollingUpdate

deployment-def.yaml

image: nginx:1.7.1 <--- upgraded

> k apply -f dep-def.yaml
* it'll do rolling update

k rollout undo deploymnent/myapp-deployment

k create -f dep.yaml
k get deployments
k apply -f dep.yaml
k set image dep/myapp-dep nginx=nginx:1.9.1
k rollout status dep/myapp-dep
k rollout history dep/myapp-dep
k rollout undo dep/myapp-dep

k get pods -n some-namespace

k create ingress -h             # help

- 95. Updating a deployment 
- 96. Demo deployments 

--record 
    ^
    add to change cause 

- 99. Deployments - Blue Green 

- 100. Deployments - Canary

Route traffic to both versions
Route a small percentage of traffic to V2

myapp-primary.yaml
spec:
    containers:
    - name:
      image: 2

myapp-canary.yaml
spec:
    containers:
    - name:
      image: 2

k scale deploy frontend --replicas=2

- 103. Jobs
kind: Job
Types of workloads 

restartPolicy: Never  or  OnFailure

- 104. CronJobs
kind: CronJob


# ----- ----- ----- # ----- ----- ----- # 
# Section_7_Services_and_Networking

- 107. Services

NodePort
ClusterIP
LoadBalancer

Services - NodePort

- 108. Services - Cluster IP


- 111. Ingress Networking 

k8s version 1.20+

Article 
https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/28046958#announcements

k get pods -A    
            ^
            all namespaces



- 149. Solution cluster roles 

k create clusterrolebinding michelle-storage-admin --user=michelle --clusterrole=storage-admin

k describe clusterrolebinding michelle-storage-admin

k --as michelle get storageclasses 


- 150. Admission Controllers 

Securing kubernetes 

Kubectl     ->  Kube ApiServer  ->  create pod 
                authentication 
                authorization 

Authorization - RBAC 

Admission controller - for advanced security measures 
    don't want the images to come from docker public repository (only from your private repo)
    

- 154. Validating and Mutating Admission controllers 

Mutates first then validate 

k -n webhook-demo create secret tls webhook-server-tls \
    --cert ..   \
    --key ..

- 155. API Versions 

- 157. API Deprecations 

single API can support multiple versions 

API deprecation policy 
    rule 1
    rule 2


- 162. Custom Controllers

# ----- ----- ----- # ----- ----- ----- # 
# Section_10_HelmFundamentals

- 164. Helm introduction 

package manager 

- 165. Install Helm


- 168. Helm concepts 


# ----- ----- ----- # ----- ----- ----- # 
# Section_12_CertificationTips 

- 172. Time management 




- 109. A sub tititle
- 109. A sub tititle






# ----- ----- ----- # ----- ----- ----- # 
# Section_10_HelmFundamentals


- 120. A sub tititle
- 121. A sub tititle



# ----- ----- ----- # ----- ----- ----- # ----- ----- ----- #






