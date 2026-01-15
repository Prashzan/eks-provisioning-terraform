### Automate provisioning of eks cluster using terraform

#### We are using existing ready modules for creating vpc for eks cluster

Best practice is to create 1 private and 1 public subnet in each AZ (availability zone)

```
tags = {
   "kubernetes.io/cluster/myapp-eks-cluster"="shared"
   }
```
   
#### Tags are for human consumption to have more information and also for referencing components from other components (programatically), basically in eks cluster when we create the control plane, one of the processes is cloud controller manager, comes from aws, orchestrates connecting to vpc, subnets, worker nodes

#### k8 cloud controller manager needs to know which resources in our account it should talk to to.. it should know which vpc should be used in cluster, or which subnet

#### All the resources that aws provisioned that has an external IP address must be deployed in public subnet not in private subnet.

#### To Run the project

#### To see what is being applied and to see if its valid
``` terraform plan ```

#### To apply changes
``` terraform apply --auto-approve ```

#### To connect to cluster using kubectl (awscli, kubectl and aws-iam-authenticator needs to be installed before running this command
```
    aws eks update-kubeconfig --name myapp-eks-cluster --region ca-central-1
```

#### We can apply simple nginx deployment just to test into cluster
``` kubectl apply -f nginx-config.yaml ```

#### to get the pods and services
``` kubectl get pod ```

``` kubectl get svc ```

Some Notes

#### We have workloads on private subnet for security reasons cuz it is not exposed to internet.

#### -- Public subnet are for external resources, aws load balancer, external connectivity

#### -- pod-identity-agent which allows pods to use iam rules from aws without storing credentials

