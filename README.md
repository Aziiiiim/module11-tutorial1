# Reflection - Part 1 

## 1. Application logs before and after exposing as a Service

Before exposing the application as a Service, the logs only showed startup messages from the container, such as the initialization of the HTTP and UDP servers.

After exposing the Service and accessing the URL in the browser multiple times, I noticed that new log entries appeared each time the page was refreshed. This shows that each access to the application generates a new log message, confirming that the service is functioning correctly and is accessible from outside the cluster.

## 2. Difference between kubectl get and kubectl get -n kube-system

The kubectl get command without any options shows resources from the default namespace, which is where my hello-node application was deployed.

On the other hand, the command with -n kube-system displays resources from the system namespace, where Kubernetes runs its internal components like metrics-server and coredns. That’s why the services and pods I created did not appear when using the -n kube-system option. This helped me understand that namespaces in Kubernetes are used to organize and isolate different sets of resources within the cluster.

# Reflection - Part 2 

## 1. What is the difference between Rolling Update and Recreate deployment strategy?

The Rolling Update strategy replaces application pods gradually. New pods are created one by one while the old ones are terminated, ensuring that the application remains available during the update process. This minimizes downtime and is the default strategy in Kubernetes.

The Recreate strategy, in contrast, terminates all existing pods before deploying new ones. This causes application downtime during the update because no pods are running temporarily. However, it may be useful when the app cannot support multiple versions running simultaneously.

## 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.

I modified the deployment YAML by adding a strategy section and setting the type to Recreate. After applying the file with kubectl apply -f, I noticed that Kubernetes first terminated all existing pods, and only after that did it start the new pods. The application was briefly unavailable during the process, confirming the expected behavior of the Recreate strategy.

## 3. Prepare different manifest files for executing Recreate deployment strategy.

I created a separate deployment file called deployment-recreate.yaml. This file is based on the original deployment manifest but includes the following strategy section:

```yaml
strategy:
  type: Recreate
```

Using this file allows me to test and compare both strategies — Rolling Update and Recreate — under similar conditions with minimal changes.

## 4. What do you think are the benefits of using Kubernetes manifest files?

Kubernetes manifest files bring consistency and reproducibility to the deployment process. Instead of manually running multiple commands, I can define the desired state of the system declaratively in YAML files and apply them with a single command. This approach also integrates well with version control systems like Git, making collaboration easier and reducing the risk of human error. When I used kubectl apply -f, the setup was faster, easier to repeat, and easier to debug compared to executing each command manually.

