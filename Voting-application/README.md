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







