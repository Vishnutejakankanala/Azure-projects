# CI Implimentation project using Azure Devops
Open Azure Devops and Create a project to get started project name [voting-app]
Go to repos and import a resistory, import. clone URL< https://github.com/Vishnutejakankanala/Azure-projects/Voting-app>
go to branches, set to main as a default branch. 

Create a pipeline
```
Clickon pipeline option create pipeline.
azure repo git -> voting-app -> Build and push docker@2. 
free trail -> Continue -> Container registry [vishnucicd]. 
 Review your azure-pipeline-vote.yaml.
Then add push stage -> command push.
save and run.

Go to project settings -> agent pool -> add pool -> azure agent -> create.
azure agent -> agent -> new agent -> Linux machine.
```

Open Azure Account (To create agent for azure devops)
```
Create resource group [azurecicd] -> create.
Azure Container register name [vishnucicd].
Create Virtual name [azure agent].
Create virtual machine with pemkey login to VM.
```
In Virtual Machine.
```
sudo apt update (To update vm).
mkdir myagent & cd myagent.
wget <url> ( url is in download at agent in azure devops).
tar zxvf <path> (To export tar file)
./config.sh (to config azure with VM).
enter the server url <https://dev.azure.com/{your organisation name(Voshnu09)}>
press enter for PAT [enter].
Enter Personal Access Token no (In azure devops -> user settings -> PATS.
enter agent pools [azure agent]
enter agent name [azureagent]
enter work folder {press enter}
sudo apt istall docker.io
sudo username -aG docker azure user
sudo systemctl restart docker
log out and login to VM again
./run.sh
```

open pipeline and run the job and see result.
Create pipeline for result (azure-pipeline-result.yaml).
Do changes in yaml file and run the job.
Create new pipeline for worker (azure-pipeline-worker.yaml).
Do Changes in yaml file and run job.
Hence, CI part is Completed.


# CD Implimentation using AZure Pipeline - ArgoCD
Create Kubernetes service in Azure Portal. 
```
Create -> create k8s cluster -> Resource group
Cluster present config [DEV/Test] -> k8s cluster name [azuredevops]
Region [south india]
Node Pool -> click on [agent pool] -> minimum node -> count [1] -> maximum count [2] -> max pods per node [30]
Enble public ip [/] -> update -> [/] agent pool -> review&Create -> Create.
```

workflow of CICD Pipeline
```
CI {REpo -> Build[Docker Image] -> Push[ACR] -> Update[Repo(k8s-manifests)]}  CD {Gitops[k8s-manifests / ArgoCD] -> K8S[azure k8s cluster]}
```

Login to the k8s cluster
```
az aks get-credentials --name azuredevops --overwrite-existing --resource-group azurecicd (To configure in CMD)
kubectl get pods (to get pods)
kubectl get nodes ( to get nodes)
```

Install ArgoCD Using CMD, refer argoCD doc
```
Qick start (execute commands)
kubectl get pods (to get pods)
```

Configure ArgoCD
```
kubectl get secrets -n argocd
kubectl edit secrets <argocd-initial-admin.secrets -n argocd>
To get password : <copy it = >
echo <password> | base64 --decode (you will get argocd password)
kubectl get svc -n argocd ( To get services)
kubectl edit svc argocd-sever -n argocd ( To edit type Cluster ip to Node port)
kubectl get nodes -o wide (To show all details)
copy External ip and http port <Ip address:portno>
To allow acess to webside goto azure portal.
search -> VMSS -> Click on agentpool -> instance -> aks agent pool -> Network settings -> Create port rules -> inbound port rule.
wait for some time and try again argoCD will open.
```

Login To argoCD and Connect to azure devops
```
User name <admin> and password <argocd-admin-secrets>
Go to azure devops and crete Personal Acess Token.
In argocd -> settings -> Repositories -> connect repo -> Choose your connection via HTTPS -> connect repo [git] ->
project [default] -> repo url [clone in devops repo].
in url replace <organization name with PAT no -> Connect.
```

Create application in argoCD 
```
Application -> Create[+] -> app name[voteapp-server]
project name [default] -> sync policy [AAutomatic]
source <repo url> -> path [k8s-specification]
destination -> Cluster url [https://kubernetes.default.svc]
Name space [default] -> Create. 
```

Add Update stage in azure Pipeline
```
Go to azure repo add folder -> Scripts. create file [update-k8s-manifests.sh]
update-k8s-manifests.sh script copy from GitHub
Go to pipeline add new or edit azure-pipeline-vote.yaml by addind update stage
save and run the script. It will run Stage, Build and Update.
make sure agent is online (VM). [./run.sh].
```

Command to Create ACR Image Pull Secrets
```
kubectl create secret docker-registry <secret-name> \
    --namespace <namespace> \
    --docker-server=<container-registry-name>.azurecr.io \
    --docker-username=<service-principal-ID> \
    --docker-password=<service-principal-password>

<secret-name> - acr-secreate   {in Secret acrsecreate will create}
<namespace> - default
<container-registry-name> - vishnucicd
<service-principal-ID> -> In ACR -> settings -> Accesskey -> [/]admin user -> you will get user name and password 
<service-principal-password>
```

In Azure Devops 
```
k8s-specifications/vote-deployment.yaml -> add image pool secrets.
image pull secrets:
-name: acr-secrets

vote/app.py (change cats vs dogs) commit it
```

To reach Vote app in Web-server
```
kubectl get svc (To get vote port no 31000)
kubectl get node -o wide (To get External ip address)
To allow acess to webside goto azure portal.
search -> VMSS -> Click on agentpool -> instance -> aks agent pool -> Network settings
-> Create port rules -> inbound port rule -> 31000 and save
Go to new tab <ipaddress:port no> to reach to voting application

Same way go to result and do the same thing, you will get result
```

Refer to https://youtu.be/aAjH9wqtx9o?si=rwJrmFJs37PSM4yl and https://youtu.be/HyTIsLZWkZg?si=s5-mI4-Jcl84dQhZ 
