
# Kubernetes Commands

## Basic Commands

1. **View Cluster Information**
   ```bash
   kubectl cluster-info
   ```

2. **View Configuration Details**
   ```bash
   kubectl config view
   ```

3. **Get Available Contexts**
   ```bash
   kubectl config get-contexts
   ```

4. **Set a Specific Context**
   ```bash
   kubectl config use-context <context-name>
   ```

5. **Check Nodes in the Cluster**
   ```bash
   kubectl get nodes
   ```

6. **Get Detailed Node Information**
   ```bash
   kubectl describe node <node-name>
   ```

---

## Working with Pods

1. **List All Pods**
   ```bash
   kubectl get pods
   ```

2. **List Pods in a Specific Namespace**
   ```bash
   kubectl get pods --namespace <namespace>
   ```

3. **Get Detailed Information on a Pod**
   ```bash
   kubectl describe pod <pod-name>
   ```

4. **Stream Pod Logs**
   ```bash
   kubectl logs <pod-name>
   ```

5. **Stream Logs from a Specific Container in a Pod**
   ```bash
   kubectl logs <pod-name> -c <container-name>
   ```

6. **Execute a Command Inside a Running Pod**
   ```bash
   kubectl exec -it <pod-name> -- /bin/bash
   ```

7. **Delete a Pod**
   ```bash
   kubectl delete pod <pod-name>
   ```

---

## Working with Deployments

1. **List Deployments**
   ```bash
   kubectl get deployments
   ```

2. **Create a Deployment**
   ```bash
   kubectl create deployment <deployment-name> --image=<image-name>
   ```

3. **Scale a Deployment**
   ```bash
   kubectl scale deployment <deployment-name> --replicas=<number-of-replicas>
   ```

4. **Update an Image in a Deployment**
   ```bash
   kubectl set image deployment/<deployment-name> <container-name>=<new-image>
   ```

5. **View the Status of a Deployment**
   ```bash
   kubectl rollout status deployment/<deployment-name>
   ```

6. **Rollback to a Previous Deployment Revision**
   ```bash
   kubectl rollout undo deployment/<deployment-name>
   ```

7. **Delete a Deployment**
   ```bash
   kubectl delete deployment <deployment-name>
   ```

---

## Working with Services

1. **List All Services**
   ```bash
   kubectl get services
   ```

2. **Get Service Details**
   ```bash
   kubectl describe service <service-name>
   ```

3. **Expose a Deployment as a Service**
   ```bash
   kubectl expose deployment <deployment-name> --type=LoadBalancer --port=80 --target-port=8080
   ```

4. **Delete a Service**
   ```bash
   kubectl delete service <service-name>
   ```

---

## Working with Namespaces

1. **List All Namespaces**
   ```bash
   kubectl get namespaces
   ```

2. **Create a New Namespace**
   ```bash
   kubectl create namespace <namespace-name>
   ```

3. **Delete a Namespace**
   ```bash
   kubectl delete namespace <namespace-name>
   ```

---

## Working with ConfigMaps and Secrets

1. **Create a ConfigMap**
   ```bash
   kubectl create configmap <configmap-name> --from-literal=<key>=<value>
   ```

2. **View ConfigMaps**
   ```bash
   kubectl get configmaps
   ```

3. **Create a Secret**
   ```bash
   kubectl create secret generic <secret-name> --from-literal=<key>=<value>
   ```

4. **View Secrets**
   ```bash
   kubectl get secrets
   ```

5. **Delete a Secret**
   ```bash
   kubectl delete secret <secret-name>
   ```

---

## Managing Jobs and CronJobs

1. **Create a Job**
   ```bash
   kubectl create job <job-name> --image=<image-name>
   ```

2. **Create a CronJob**
   ```bash
   kubectl create cronjob <cronjob-name> --schedule="*/1 * * * *" --image=<image-name>
   ```

3. **Get List of Jobs**
   ```bash
   kubectl get jobs
   ```

4. **Get List of CronJobs**
   ```bash
   kubectl get cronjobs
   ```

5. **Delete a Job or CronJob**
   ```bash
   kubectl delete job <job-name>
   kubectl delete cronjob <cronjob-name>
   ```

---

## Working with Persistent Volumes

1. **List Persistent Volumes**
   ```bash
   kubectl get pv
   ```

2. **Create a Persistent Volume Claim**
   ```bash
   kubectl create -f <pvc-definition.yaml>
   ```

3. **Check the Status of Persistent Volumes**
   ```bash
   kubectl describe pv <pv-name>
   ```

4. **Delete a Persistent Volume**
   ```bash
   kubectl delete pv <pv-name>
   ```

---

## Managing Roles and RBAC

1. **Create a Role**
   ```bash
   kubectl create role <role-name> --verb=get --verb=list --resource=pods
   ```

2. **Create a RoleBinding**
   ```bash
   kubectl create rolebinding <rolebinding-name> --role=<role-name> --user=<user-name> --namespace=<namespace>
   ```

3. **List Roles and RoleBindings**
   ```bash
   kubectl get roles
   kubectl get rolebindings
   ```

