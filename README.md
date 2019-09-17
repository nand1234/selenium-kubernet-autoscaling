# prerequiste:
- kubernetes is insallted

# how to install matrix server
- clone the project on local driver

- command to install matrix server
-- kubectl apply -f deploy/1.8+/

- Command to check matrix server is installed successfully
-- kubectl top node

# Command to create selenium chrorme deployment
- kubectl apply -f <path of the yml>\chrome-deployment.yml

- command to scale deployment
-- kubectl apply -f <<path of yml>>\scale.yml

- command to check deployment scale is working
-- kubectl describe hpa <<auto scaler name>>

# install kubernet dashboard
1. kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
2. enbale the port : Kubectl proxy
3. generate secrate key. run below code in powershell
   $TOKEN=((kubectl -n kube-system describe secret default | Select-String "token:") -split " +")[1]
    kubectl config set-credentials docker-for-desktop --token="${TOKEN}"
    
# addiotional commands:
- kubectl top node
- kubectl get pods
- kubectl get hpa
- kubectl delete all --all
- kubectl describe hpa nand-auto-scaler
- kubectl delete hpa nand-auto-scaler
- kubectl get deployments -o wide
- kubectl delete deployments selenium-node-chrome
- kubectl delete hpa NAME-OF-HPA
- kubectl get hpa -n testing-ns

# how to install  Prometheus

prerequisite : install choco 

1. choco install kubernetes-helm
2. helm init --service-account tiller --history-max 200

Create a service account:
- kubectl create serviceaccount --namespace kube-system tiller

Bind the new service account to the cluster-admin role. This will give tiller admin access to the entire cluster:
- kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller

Deploy tiller and add the line serviceAccount: tiller to spec.template.spec:
- kubectl patch deploy --namespace kube-system tiller-deploy -p "{\"spec\":{\"template\":{\"spec\":{\"serviceAccount\":\"tiller\"}}}}"
  
 # Now we are ready to install the Prometheus operator:
 This will create a separate namespace monitoring and will install the Prometheus operator in that namespace. It will also install Grafana, Kube-state-metrics and the Prometheus alert manager in the same namespace.
 - helm install --name prom-operator stable/prometheus-operator --namespace monitoring
 
 Once the Prometheus operator is installed we can forward the Prometheus server pod to a port on our local machine
 - kubectl port-forward -n monitoring  prometheus-prom-operator-prometheus-o-prometheus-0 9090:9090
 
 Access the Prometheus dashboard by navigating to http://localhost:9090
 
 # delete all promothe monitoring
- kubectl delete crd prometheuses.monitoring.coreos.com
- kubectl delete crd prometheusrules.monitoring.coreos.com
- kubectl delete crd servicemonitors.monitoring.coreos.com
- kubectl delete crd podmonitors.monitoring.coreos.com
- kubectl delete crd alertmanagers.monitoring.coreos.com

 
 
 
