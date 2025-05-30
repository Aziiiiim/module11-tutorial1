# Reflection - Tutorial 1 

## 1. Application logs before and after exposing as a Service

Before exposing the application as a Service, the logs only showed startup messages from the container, such as the initialization of the HTTP and UDP servers.

After exposing the Service and accessing the URL in the browser multiple times, I noticed that new log entries appeared each time the page was refreshed. This shows that each access to the application generates a new log message, confirming that the service is functioning correctly and is accessible from outside the cluster.

## 2. Difference between kubectl get and kubectl get -n kube-system

The kubectl get command without any options shows resources from the default namespace, which is where my hello-node application was deployed.

On the other hand, the command with -n kube-system displays resources from the system namespace, where Kubernetes runs its internal components like metrics-server and coredns. That’s why the services and pods I created did not appear when using the -n kube-system option. This helped me understand that namespaces in Kubernetes are used to organize and isolate different sets of resources within the cluster.

