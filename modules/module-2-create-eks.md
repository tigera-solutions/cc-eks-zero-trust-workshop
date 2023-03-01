# Module 2 - Create an EKS cluster and Connect it to Calico Cloud

For EKS, the Calico OSS can be used as a CNI, or you can make use of the AWS VPC networking as CNI and have Calico only as a plugin for the security policies. 

We will use the second approach during this workshop. Please, find below an example on how to create a two nodes cluster with an small footprint, but feel free to create your EKS cluster with the parameters you prefer. Do not forget to include the region if different than the default on your account.

```bash
echo "# Start Lab Params" > ~/labVars.env
UNIQUE_SUFFIX=$USER$RANDOM
# Remove Underscores and Dashes (Not Allowed in EKS Names)
UNIQUE_SUFFIX="${UNIQUE_SUFFIX//_}"
UNIQUE_SUFFIX="${UNIQUE_SUFFIX//-}"
# Check Unique Suffix Value (Should be No Underscores or Dashes)
echo $UNIQUE_SUFFIX
export CLUSTERNAME=$UNIQUE_SUFFIX-tigera-workshop
export REGION=ca-central-1
# 
echo "# Start Lab Params" > ~/labVars.env
echo export CLUSTERNAME=$CLUSTERNAME >> ~/labVars.env
echo export REGION=$REGION >> ~/labVars.env
#
eksctl create cluster --name $CLUSTERNAME --version 1.24  --region $REGION --node-type m5.xlarge
```

- View EKS cluster.

  Once cluster is created you can list it using eksctl.
  
  ```bash
  eksctl get cluster tigera-workshop --region $REGION
  ```

- Install the EBS driver for the EKS cluster

  ```bash
  kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.15"
  ```
  
  Check driver pods status
  
  ```bash
  kubectl get pods -n kube-system -l app.kubernetes.io/name=aws-ebs-csi-driver
  ```

- Test access to EKS cluster with kubectl

  Once the EKS cluster is provisioned with eksctl tool, the kubeconfig file would be placed into ~/.kube/config path. The kubectl CLI looks for kubeconfig at ~/.kube/config path or into KUBECONFIG env var.

  ```bash
  # verify kubeconfig file path
  ls ~/.kube/config
  # test cluster connection
  kubectl get nodes
  ```

--- 

[:arrow_right: Module 3 - Connect the AWS EKS cluster to Calico Cloud](/modules/module-3-connect-calicocloud.md)  <br>

[:arrow_left: Module 1 - Getting Started](/modules/module-1-getting-started.md)    
[:leftwards_arrow_with_hook: Back to Main](/README.md)  