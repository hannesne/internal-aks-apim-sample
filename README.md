# internal-aks-apim-sample
This sample contains a shell script that will provision an internal AKS cluster that runs my [Echo function](https://github.com/hannesne/echofunction). The service is exposed through an API Management instance deployed into the same vnet, with a public IP. IP Filtering is configured on API Management. An NSG limits access to the kubernetes cluster to the subnet in which API Management is deployed.

## Instructions
1. open deployscript.sh
2. Edit the values at the start. You will need to provide a unique values for the api management instance. 
3. Open a terminal, and ensure that kubectl and the azure CLI is installed.
4. Login to azure using az login.
5. Run the script. 
6. Fix any issues you encounter and create a pull request. ;)

## Azure Functions
The echo function is deployed to the AKS cluster using a pre-built [docker image](https://cloud.docker.com/u/hannesn/repository/docker/hannesn/echo). The source code is provided in this repo for reference only. You can build and deploy your own image from it using the azure functions cli. Two yaml files are used to perform the deployment to the cluster via kubectl. These yaml files were intialially generated using the functions cli. They were edited to change the loadbalancer to an internal load balancer, and parameterize the allowed source ip addresses for the load balancer.

Sending an HTTP request to the Echo function wil return an empty HTTP 204 response. The HTTP result code that it returns can be overriden using a query string parameter named `result`. It will also return a message defined in a query string parameter `message`, but API Management overrides that querystring parameter to contain _Much success!_ This should be displayed at the end of running the script, if everything worked fine.

## References
* https://docs.microsoft.com/en-us/azure/api-management/api-management-using-with-vnet
* https://docs.microsoft.com/en-us/azure/aks/configure-kubenet#overview-of-kubenet-networking-with-your-own-subnet
* https://docs.microsoft.com/en-us/azure/azure-functions/functions-kubernetes-keda
* https://docs.microsoft.com/en-us/azure/aks/internal-lb
* https://kubernetes.io/docs/tasks/access-application-cluster/configure-cloud-provider-firewall/#restrict-access-for-loadbalancer-service
