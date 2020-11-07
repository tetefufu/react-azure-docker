# react-azure-docker

```
react-azure-docker

todo:
? what does docker prune do?
? why doesn't docker prune remove my images

to start docker on mac bootcamp - restart in macos (hold option key) then go to system pref, restart in windows 
docker run -it --rm -p 3001:3000 sample:dev
docker image ls
docker image prune // remove unsued images 
docker build -f Dockerfile.prod -t sample:prod
docker run --it --rm -p1337:80 sample:prod



get and test locally

git clone https://github.com/tetefufu/react-azure-docker.git
docker build -f Dockerfile.prod -t react-azure-docker-img:prod .
docker run -it --rm -p 1338:80 react-azure-docker-img:prod
docker tag react-azure-docker-img:prod reactazuredockeracr.azurecr.io/samples/react-azure-docker-img
	
create azure and push to azure
	
az login
az group create --location eastus --name react-azure-docker-rg
az acr create --name reactazuredockeracr --resource-group react-azure-docker-rg --sku standard --admin-enabled true
az acr login --name reactazuredockeracr
docker push reactazuredockeracr.azurecr.io/samples/react-azure-docker-img
az appservice plan create --name react-azure-docker-sp --resource-group react-azure-docker-rg --location eastus
az webapp create --name react-azure-docker-wa --resource-group react-azure-docker-rg --plan react-azure-docker-sp --deployment-container-image-name reactazuredockeracr.azurecr.io/samples/react-azure-docker-img:latest
az webapp identity assign --resource-group react-azure-docker-rg --name react-azure-docker-wa --query principalId --output tsv
az account show --query id
az role assignment create --assignee xxxxxxxxxxxxx --scope /subscriptions/xxxxxxxxxxxxxxxxxxxxxx/resourceGroups/react-azure-docker-rg/providers/Microsoft.ContainerRegistry/registries/reactazuredockeracr --role "AcrPull"
az webapp config container set --name react-azure-docker-wa --resource-group react-azure-docker-rg --docker-custom-image-name reactazuredockeracr.azurecr.io/samples/react-azure-docker-img:latest --docker-registry-server-url https://reactazuredockeracr.azurecr.io

view
https://react-azure-docker-wa.azurewebsites.net

clean up
docker image rm reactazuredockeracr.azurecr.io/samples/react-azure-docker-img
az group delete --name react-azure-docker-rg

notes 
az group list --output table
az acr build -f Dockerfile.prod --registry reactazuredockeracr --image webimage .
az acr delete --name reactazuredockerAcr
```
