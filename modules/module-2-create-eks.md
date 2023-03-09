# Module 2 - Create an EKS cluster and Connect it to Calico Cloud

> **Note**: During this workshop, we'll set up some environment variables. If you're terminal session restarts, you may need to reload these variables. You can use that via the following command: <p>
```bash
source ~/labVars.env
```

For EKS, the Calico OSS can be used as a CNI, or you can use the AWS VPC networking as CNI and have Calico only as a plugin for the security policies. 

We will use the second approach during this workshop. Please, find below an example of how to create a two nodes cluster with a small footprint, but feel free to create your EKS cluster with the parameters you prefer. Remember to include the region if different than the default on your account.

Set your variables accordingly.

```bash 
export CLUSTERNAME=$RANDOM-tigera-workshop
export REGION=ca-central-1
```

Save the variables in the `~/labVars.env` file, and create the EKS cluster.

```bash
echo "# Start Lab Params" > ~/labVars.env
echo export CLUSTERNAME=$CLUSTERNAME >> ~/labVars.env
echo export REGION=$REGION >> ~/labVars.env
#
eksctl create cluster --name $CLUSTERNAME --version 1.24  --region $REGION --node-type m5.xlarge
```

- View EKS cluster.

  Once cluster is created you can list it using eksctl.
  
  ```bash
  eksctl get cluster $CLUSTERNAME --region $REGION
  ```

- Install the EBS driver for the EKS cluster.

  ```bash
  kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.16"
  ```
  
  Check driver pods status.
  
  ```bash
  kubectl get pods -n kube-system -l app.kubernetes.io/name=aws-ebs-csi-driver
  ```

- Test access to EKS cluster with kubectl.

  Once the EKS cluster is provisioned with the eksctl tool, the kubeconfig file would be placed into `~/.kube/config` path. The kubectl CLI looks for kubeconfig at `~/.kube/config` path or into KUBECONFIG environment variable.

  ```bash
  # Verify kubeconfig file path.
  ls ~/.kube/config
  # Test cluster connection.
  kubectl get nodes
  ```

--- 

[:arrow_right: Module 3 - Connect the AWS EKS cluster to Calico Cloud](/modules/module-3-connect-calicocloud.md)  <br>

[:arrow_left: Module 1 - Getting Started](/modules/module-1-getting-started.md)    
[:leftwards_arrow_with_hook: Back to Main](/README.md)  