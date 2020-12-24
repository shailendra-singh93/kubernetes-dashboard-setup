# kubernetes-dashboard-setup
This repo explains how to setup k8s dashboard after cluster setup

#Below are the steps mentioned to setup kubernetes dashboard

*If you have individual centos server machine setup with vnc to access GUI of kubernetes dashboard, then first create a ssh link to localhost since k8s can only be access at localhost or 127.0.0.1

On GUI server terminal, execute (# ssh -L localhost:8001:127.0.0.1:8001 <user>@<Kubernetes_master_IP>)
  
# Now from kubernetes terminal create pods, service account, admin account, read only account etc. as below

Execute  # kubectl create -f dashboard.yaml
         # kubectl create -f dashboard-user-admin.yaml
         # kubectl create -f dashboard-user-read-only.yaml
         
# Now, get tokens for accessing Kubernetes dashboard during login screen:

# admin user
kubectl get secret -n kubernetes-dashboard $(kubectl get serviceaccount admin-user -n kubernetes-dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode

# Read-only user
kubectl get secret -n kubernetes-dashboard $(kubectl get serviceaccount read-only-user -n kubernetes-dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode

# From kubernetes terminal start dashboard on setup proxy as below
kubectl proxy

# Go to the GUI server and open search engine like chrome, mozila .. etc and hit below URL to access Kubernetes-Dashboard.
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
