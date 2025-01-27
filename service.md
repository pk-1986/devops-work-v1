What is kubernetesserivce and when it is needed?
•	In kuberenetes cluster each pod get its own ip address
•	Note : Pods in kubernetes are Ephemeral which means those are tend to get destroyed frequently
•	Eventually when pod gets recreated it will be assigned with new ip address
•	Hence, it doesn't make sense to use pod ip address as primary
•	Else we will end up in adjusting the ip address everytime when pod get recreated
•	The solution for this problem would be services
•	Through help of services we can have a static ip address which stays even when pod dies
•	Hence services are good solution to loose coupling the components of cluster
How service works?
•	As we know pods will be assigned randomly in worker nodes
•	Also, pods ip addresses will be assigned from Node's ip range
•	Eventually we can see the ip addresses using kubectl get pos -o wide
 
•	Now this ip address cannot be used as primary as because pods are ephemeral
•	Hence service will be an virtual abstraction which will be taking traffic from outside and routing to pods
 
Questions would raise in mind?
•	Now next question would be
o	how pods are identified by service?
	This happens with help of selector
	End pods are identifed via selector
	Note: all labels of pods must matched with service selector if incase some labels are not matching that pods will not be registered with service
o	which port to forward?
	This happens with help of argument called targetPort
Notes
•	Once after service is created, this object can be monitored with help of command kubectl get endpoints
•	Kuberenets will use this to track of which pods are the members of service
•	Since this is dynamic, while pod get recreated it automatically updates the endpoint services

Different service types
•	Cluster IP service
•	NodePort service
•	LoadBalancer service

Cluster IP service
•	This is the default type of service
•	while we create a service without specifing type, by default it may consider cluster IP service
•	however in our manifest file <cluster.yaml> we are specifing type as ClusterIP
•	apply the file - kubectly apply -f cluster.yaml
•	kubectl commands
o	kubectl get service
o	kubectl get svc
o	kubectl get service <service> -o yaml
o	kubectl get endpoints
o	kubectl get pod -o wide
Test scenarios
•	add pods by increasing replicas & see if pods are getting auto added by service using endpoint command
•	decrease the pod count and check if it is removed from endpoints
Accessing your application via cluster IP
•	login into pod and add your own contents in html file kubectl exec -it <pod-name> --bash
•	Then do curl from machine with : to check if you can view your html contents. curl <cluster-ip>:<port-number>
NodePort service
•	Nodeport allows you to establish the service to your virtual machine IP address
•	chekout the manifest file <nodeport.yaml>
•	fields to be noticed
o	type:
o	nodeport:
•	Note : By default, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
•	apply the file - kubectly apply -f nodeport.yaml
•	kubectl commands
o	kubectl get service
o	kubectl get svc
o	kubectl get service <service> -o yaml
o	kubectl get endpoints
o	kubectl get pod -o wide
Test scenarios
•	add pods by increasing replicas & see if pods are getting auto added by service using endpoint command
•	decrease the pod count and check if it is removed from endpoints
Accessing your application via node IP
•	login into pod and add your own contents in html file kubectl exec -it <pod-name> --bash
•	Then do curl from machine with : to check if you can view your html contents. curl <node-ip>:<port-number>
•	From browser you can access the same using :
•	Note :
o	local virtual machine does not have external IP
o	hence no ip found in EXTERNAL-IP section from kubectl get svc command
o	if your machine has public IP address, it would be visible over here

Load balance service
•	Load balance allows you to establish the service to common load balancer IP address
•	chekout the manifest file <lb.yaml>
•	fields to be noticed
o	type:
o	nodeport:
•	Note : By default, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
•	apply the file - kubectly apply -f nodeport.yaml
•	kubectl commands
o	kubectl get service
o	kubectl get svc
o	kubectl get service <service> -o yaml
o	kubectl get endpoints
o	kubectl get pod -o wide
Test scenarios
•	add pods by increasing replicas & see if pods are getting auto added by service using endpoint command
•	decrease the pod count and check if it is removed from endpoints
Accessing your application via load balancer IP
•	login into pod and add your own contents in html file kubectl exec -it <pod-name> --bash
•	Then do curl from machine with : to check if you can view your html contents. curl <lb-ip>:<port-number>
•	From browser you can access the same using :
•	Note :
o	local virtual machine does not create load balancer hence it might not have external IP
o	hence no EXTERNAL-IP section from kubectl get svc command will be in pending state
o	if your create this in public cloud it might automatically create lb for itself and assign external IP
•	Cluster IP
pods can be accessible from different nodes via cluster IP
pods cannot be accessible from outside
Only internal access is possible
Node port
pods can be accessible from different nodes via common port
every node ip address returns same result from common port
pods can be accessible from outside via node ip and port
Load Balancer
pods can be accessible from different nodes via common port
but common port are accessible only via load balancer ip
pods can be accessible from outside via loadbalanceip

