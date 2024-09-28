
# Create EKS Cluster and Use it with AWS CLI

## Prerequisites

1. **Install AWS CLI**  
   Make sure the AWS CLI is installed. You can download and install it using the command below:
   
   ```bash
   curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   sudo installer -pkg AWSCLIV2.pkg -target /
   ```

   Verify the installation:
   ```bash
   aws --version
   ```

2. **Install `kubectl`**  
   `kubectl` is needed to interact with the Kubernetes cluster. Install it by running:

   ```bash
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x ./kubectl
   sudo mv ./kubectl /usr/local/bin/kubectl
   ```

   Verify the installation:
   ```bash
   kubectl version --client
   ```

3. **Install `eksctl`**  
   `eksctl` simplifies EKS cluster creation. You can install it by running:

   ```bash
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   ```

   Verify the installation:
   ```bash
   eksctl version
   ```

4. **Configure AWS CLI**  
   Configure your AWS CLI with appropriate credentials:

   ```bash
   aws configure
   ```

   Provide:
   - AWS Access Key
   - AWS Secret Access Key
   - Default region name (e.g., `us-west-2`)
   - Default output format (e.g., `json`)

---

## Step 1: Create an EKS Cluster

### 1.1 Create a Cluster Using `eksctl`

The simplest way to create a basic EKS cluster is using the `eksctl` tool. Run the following command:

```bash
eksctl create cluster \
  --name my-eks-cluster \
  --region us-west-2 \
  --nodegroup-name linux-nodes \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed
```

**Explanation**:
- `--name`: The name of the cluster.
- `--region`: AWS region where the cluster is created.
- `--nodegroup-name`: The name of the node group (EC2 instances running the Kubernetes pods).
- `--node-type`: The EC2 instance type for the worker nodes.
- `--nodes`: The number of nodes initially.
- `--nodes-min`: Minimum number of nodes in the autoscaling group.
- `--nodes-max`: Maximum number of nodes in the autoscaling group.
- `--managed`: Use AWS-managed nodes.

The process will take around 10-15 minutes. You will get confirmation once the cluster is ready.

### 1.2 Check the Cluster and Nodes

After the cluster is created, verify its status:

```bash
eksctl get cluster --region us-west-2
```

You can also describe the nodes:

```bash
kubectl get nodes
```

---

## Step 2: Configure `kubectl` to Use the EKS Cluster

To interact with the EKS cluster via `kubectl`, you need to configure it to use the right cluster context.

### 2.1 Update `kubeconfig`

Run the following command to update your `kubeconfig` file with the new EKS cluster:

```bash
aws eks --region us-west-2 update-kubeconfig --name my-eks-cluster
```

This command:
- Retrieves the cluster configuration.
- Updates your local `kubeconfig` file.
- Sets the context to use the EKS cluster.

### 2.2 Verify `kubectl` Configuration

You can now verify that `kubectl` is configured to use the EKS cluster:

```bash
kubectl config get-contexts
```

The context for your cluster should be listed.

---

## Step 3: Deploy a Sample Application

### 3.1 Create a Kubernetes Deployment

Create a simple NGINX deployment:

```bash
kubectl create deployment nginx --image=nginx
```

### 3.2 Expose the NGINX Deployment

Expose the deployment using a LoadBalancer service:

```bash
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

This will provision an external AWS Elastic Load Balancer (ELB) to expose your application.

### 3.3 Check Service Status

To check the status of the service and retrieve the external IP address:

```bash
kubectl get services
```

Wait until the external IP (or DNS) is assigned. Once available, you can access the NGINX server via the ELB URL.

---

## Step 4: Manage the EKS Cluster and Nodes

### 4.1 Scale the Node Group

To scale the number of nodes in the EKS cluster:

```bash
eksctl scale nodegroup \
  --cluster my-eks-cluster \
  --name linux-nodes \
  --nodes 4 \
  --region us-west-2
```

### 4.2 Upgrade the Cluster

To upgrade your EKS cluster to a newer Kubernetes version:

```bash
eksctl upgrade cluster --name my-eks-cluster --region us-west-2 --approve
```

---

## Step 5: Clean Up

Once you’re done with the cluster and don’t need it anymore, delete it to avoid costs:

```bash
eksctl delete cluster --name my-eks-cluster --region us-west-2
```

This will remove the EKS cluster, associated resources, and managed node groups.

---

## Additional Tips

- **IAM Roles**: Ensure that the AWS user or role you’re using has the required permissions to create and manage EKS clusters, including creating VPCs, Load Balancers, EC2 instances, and associated networking resources.
- **Networking**: By default, `eksctl` sets up a new VPC and subnets. If you want to use an existing VPC, you can provide custom VPC and subnet details.
- **Cluster Security**: AWS IAM roles and policies are crucial for cluster and pod security. You may need to assign IAM roles to your worker nodes and set up role-based access control (RBAC) for `kubectl`.
