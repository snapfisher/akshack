# Challenge 4: Coach's Guide

[< Previous Challenge](./03-k8sintro.md) - **[Home](README.md)** - [Next Challenge >](./05-scaling.md)

## Notes & Guidance

- If students have not or could not deploy their containers to the Azure Container Registry, we have staged the FabMedical apps on Docker Hub at these locations:
  - **API app:** whatthehackmsft/content-api
  - **Web app:** whatthehackmsft/content-web


- Students will be provided with two sample template yaml files one for a Service resource and one for a Deployment resource. A good recommendation would be to create two copies of each (one set for content-api and one set for content-web)
- Students will need to find extra settings to add to their template YAML files. They should make use of the Kubernetes docs in addition to whatever else they can find on the web.
	- **NOTE:** Fully fleshed out YAML files are available in the Coach Solutions folder for Challenge 4.
	- **NOTE:** If you observe students using **foo** and **bar** unchanged in the template files, you should recommend they consider more relevant names such as **web**, **content-web**, **api**, or **content-api**.
- In the Deployment YAML for **content-api**, they’ll need these settings in the spec section:	
	- containers.resources.requests.cpu: **0.5 (or 500m)**
	- containers.resources.requests.memory: **128Mi**
    - containers.ports.containerPort: **3001**
- In the deployment YAML for **content-web**, they will need these settings in the spec section:
	- containers.resources.requests.cpu: **0.5 (or 500m)**
	- containers.resources.requests.memory: **128Mi**
	- containers.ports.containerPort: **3000** 
	- containers.env.name: **CONTENT_API_URL**
	- containers.env.value: **http://content-api:3001**
		- **NOTE:** The value **content-api** in the URL must be whatever was used as the name of the service during deployment. 
- In the Service YAML for **content-web**, they need to figure out that the type should be changed to “LoadBalancer”.
- Coaches should be familiar with common kubectl commands in order to help the students troublshoot:
	- `kubectl get nodes`
	- `kubectl get pods`
	- `kubectl describe pod <pod-name>`
	- `kubectl get services`
	- `kubectl get deployments`
	- `kubectl delete deployment <deployment-name>`
	- `kubectl delete pod <pod-name>`
  	- `kubectl delete service <service-name>`
	- `kubectl apply -f <yaml-file>`
- When the service is deployed it will take some time for an External IP to be assigned.
	- Issue the following kubectl command and look in the **EXTERNAL-IP** column
		- `kubectl get services`
	- You will see `<pending>` if the IP hasn’t yet been assigned.
- To verify that the API app is correctly deployed the students need to:
	- Figure out the name of the pod the API app was deployed to, eg: 	
    	- `content-api-23aceed`
	- Then use a kubectl command like this to get a bash shell:
		- `kubectl exec -it content-api-23aceed -- /bin/bash`
	- To verify the API app is working you need to curl the /speakers endpoint:
		- `curl http://localhost:3001/speakers`
    - They should see a huge JSON document printed to the screen.

