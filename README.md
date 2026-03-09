Project Structure

Argocd/       
├── parent.yaml          
├── app/                     
│   ├── deployment.yaml                
│   ├── deployment.yaml       
│       └── service.yaml
└── README.md



parent.yaml – The main Argo CD Application manifest that points to the reppos subfolder containing the individual apps.

reppos/flask-app – Contains Kubernetes manifests for Flask.

reppos/nginx-app – Contains Kubernetes manifests for Nginx.



--------
Prerequisites

Make sure you have the following installed:

Minikube

kubectl

Argo CD CLI

Git (for repo access)


-------
**Step 1: Start Minikube**
'minikube start --driver=docker'

Check cluster status:
'kubectl get nodes'

--------
**Step 2: Install Argo CD on Minikube**
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Check Argo CD pods:

kubectl get pods -n argocd

------
**Step 3: Expose Argo CD Server**
kubectl port-forward svc/argocd-server -n argocd 8080:443

Now access Argo CD UI at: https://localhost:8080

Default admin password:

kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d

------
**Step 4: Deploy Applications via Argo CD**

Apply the parent application YAML:

kubectl apply -f parent.yaml

Argo CD will sync the subfolder apps (flask-app and nginx-app) automatically.

Check deployment status:

kubectl get all

-----
**Step 5: Access Applications
Flask App**
kubectl port-forward svc/flask-app-service 5000:5000

Visit: http://localhost:5000

Nginx App
kubectl port-forward svc/nginx-app-service 8081:80

Visit: http://localhost:8081

----------------------------------------------------------------------------------------------
**Notes**

Ensure each app has its own Deployment and Service YAML.

The parent.yaml acts as the main Argo CD application referencing the app subfolders.

You can add more apps by creating additional subfolders under appp and updating parent.yaml.



**OUTPUT**

<img width="1912" height="1137" alt="image" src="https://github.com/user-attachments/assets/ea1d54d6-4fbe-48a8-ab0d-208c75610b26" />




