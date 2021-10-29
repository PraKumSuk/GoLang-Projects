Go App

Includes a very basic service, that responses with a message for every request it receives.

Port configured : 3000

Pre-requisites :
Docker
Kubernetes
Go

---

Instructions to install kubernetes and deploy this application in your local :-

Download Install kubectl cmd line clien
https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

Set up environment variable for kubectl

Open powershell and verify installation
kubectl config current-context

kubectl get nodes

kubectl get pods

//to list all pods, namespaces
kubectl get pods,svc --all-namespaces -o wide

----------------------------------

To setup k8s dashboard refer : (NOTE : UI Yaml settings in below url does not WORK………..USE the ONE GIVEN BELOW)
                (https://www.replex.io/blog/how-to-install-access-and-add-heapster-metrics-to-the-kubernetes-dashboard)

//Use below UI dashboard settings url as one mentioned above dont work
//from power shell run below
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml

//to start k8s dashboard
kubectl proxy

//from browser open local k8s dashboard
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login


----

Create user and use token method to authenticate

    //creating a user named dashboard-admin-sa
    kubectl create serviceaccount dashboard-admin-sa

    //to give cluster admin roles or binding
    kubectl create clusterrolebinding dashboard-admin-sa --clusterrole=cluster-admin --serviceaccount=default:dashboard-admin-sa

    //to verify
    kubectl get secrets

    //to get token, obtain name from above cmd out put
    kubectl describe secret dashboard-admin-sa-token-tz5zf

    //Copy token obtained above and paste in the browser using the token auth type


//Now open browser and continue to login using the above token
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login


Deploy your application :-

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

//to build image optional
docker build -t praveespk/go-hello C:\SpkWorkspace\Go\go-hello

//to start k8s cluster
kubectl proxy

//cd to ur project root folder
kubectl apply -f deployment.yml

kubectl apply -f service.yml

//verify
kubectl get services

//to delete
kubectl delete -n default deployment go-hello
