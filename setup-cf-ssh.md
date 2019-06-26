# Setting up cf ssh for Azure Cloud Foundry

1. After logging into the Azure CLI in a Linux shell, execute the following commands (substitute your location and resource group appropriately):

  `$ export LOCATION="westus"`

  `$ export RESOURCE_GROUP="AZURE-RESOURCE-GRP"`

  `$ az network lb create --name pcf-ssh-lb \
  
  --resource-group $RESOURCE_GROUP --location $LOCATION \
  
  --backend-pool-name pcf-ssh-lb-be-pool --frontend-ip-name pcf-ssh-lb-fe-ip \
  
  --public-ip-address pcf-ssh-lb-ip --public-ip-address-allocation Static \
  
  --sku Standard`


  `$ az network lb rule create --lb-name pcf-ssh-lb \
  
  --name diego-ssh --resource-group $RESOURCE_GROUP \
  
  --protocol Tcp --frontend-port 2222 \
  
  --backend-port 2222 --frontend-ip-name pcf-ssh-lb-fe-ip \
  
  --backend-pool-name pcf-ssh-lb-be-pool`


  `$ az network lb probe create --lb-name pcf-ssh-lb \
  
  --name tcp2222 --resource-group $RESOURCE_GROUP \
  
  --protocol Tcp --port 2222`

2. Update control resource in the PAS tile with the load balancer pcf-ssh-lb

3. Find control VM and add firewall rule to allow inbound connections on 2222 in Azure

4. Update DNS entry for ssh.sys._yourdomain.com_

