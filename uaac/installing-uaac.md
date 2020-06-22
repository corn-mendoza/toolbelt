# Installing cf-uaac for Tanzu Application Service (TAS - fomerly PAS) and Tanzu Kubernetes Grid Internal (TKGI - fomerly PKS) for Windows Users

### Overview
TAS and TKGI utilizes the internal UAC for user authorization and access.To complete configuration of UAC, the following steps will need to be completed. Existing users can be given access by adding the user to the security groups below.

Example:
- read.fortunes

`$ uaac member add read.fortunes {userid}`


#### Setting up UAAC in Windows 10 
To complete the setup of security for applications running in Tanzu Application Services or Tanzu Kubernetes Grid Internal, the cf-uaac program needs to be installed to access security features needed for user access control, including CredHub. 

##### Installing cf-uaac for WSL:

`$ sudo apt-get update`

`$ sudo apt install build-essential`

`$ sudo apt install ruby-full`

`$ sudo gem install cf-uaac`

##### Installing cf-uaac for Windows Command or Powershell Use:

Pre-Req: Install Ruby with MSYS2 and open a new Powershell or Command Prompt to refresh PATH

 `> gem install eventmachine --platform ruby`

 `> gem install cf-uaac`

#### Usage Example: Configuring Application Security
To complete security configuration, use the cf-uaac command in a Windows Command Prompt or a bash shell to execute the following to enable the role "read.fortunes". You will need to have access to the UAA Admin Credentials to complete these tasks.

`$ uaac target uaa.sys.yourdomain.com --skip-ssl-validation`

`$ uaac token client get admin -s {admin password}`

`$ uaac group add read.fortunes`

`$ uaac user add {username} -p {password} --emails {email address}`

`$ uaac member add read.fortunes {username}`

`$ uaac client add myFortunes --authorized_grant_types authorization_code,refresh_token --authorities uaa.resource --redirect_uri http://fortuneui*.apps.yourdomain.com/signin-cloudfoundry --autoapprove cloud_controller.read,cloud_controller_service_permissions.read,openid,read.fortunes --secret mySecret`

`$ uaac client update myFortunes --scope read.fortunes,openid,cloud_controller.read,cloud_controller_service_permissions.read`
