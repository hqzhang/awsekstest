# EKS Getting Started Guide Configuration
Terraform v0.12.25
This is the full configuration from https://www.terraform.io/docs/providers/aws/guides/eks-getting-started.html

See that guide for additional information.

NOTE: This full configuration utilizes the [Terraform http provider](https://www.terraform.io/docs/providers/http/index.html) to call out to icanhazip.com to determine your local workstation external IP for easily configuring EC2 Security Group access to the Kubernetes master servers. Feel free to replace this as necessary.
1.terraform init
2.terfaform validate
3.terraform plan -auto-approve
4.terrafrom apply -auto-approve
output:
0) git clone https://github.com/terraform-providers/terraform-provider-aws.git
   Cd terraform-provider-aws/examples/eks-getting-started
   Terraform plan
   Terraform apply
Outputs:
config_map_aws_auth = ....

1)create config and list nodes  
  i) aws eks list-clusters
  "clusters": [ "terraform-eks-demo"]

  ii) aws eks update-kubeconfig --name terraform-eks-demo
  /Users/hongqizhang/.kube/config

2)  list nodes
  Kubectl get nodes
  NAME                                      STATUS   ROLES    AGE     VERSION
  ip-10-0-0-56.us-west-2.compute.internal   Ready    <none>   3h30m   v1.16.8-eks-e16311
  ip-10-0-1-31.us-west-2.compute.internal   Ready    <none>   3h30m   v1.16.8-eks-e16311

3)install metric server 
  wget -O v0.3.6.tar.gz https://codeload.github.com/kubernetes-sigs/metrics-server/tar.gz/v0.3.6 && tar -xzf v0.3.6.tar.gz
  kubectl apply -f metrics-server-0.3.6/deploy/1.8+/

4)install dashboard
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml

5)get token
  kubectl apply -f https://raw.githubusercontent.com/hashicorp/learn-terraform-provision-eks-cluster/master/kubernetes-dashboard-admin.rbac.yaml
  kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep service-controller-token | awk '{print $1}')
  
  output:
  Token:......

6)kubectl proxy

7)login UI with token
  http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/



1)list eks
  aws eks list-clusters
  "clusters": [ "terraform-eks-demo"]

2)create config
  aws eks update-kubeconfig --name terraform-eks-demo
  /Users/hongqizhang/.kube/config

3)install metric server 
  wget -O v0.3.6.tar.gz https://codeload.github.com/kubernetes-sigs/metrics-server/tar.gz/v0.3.6 && tar -xzf v0.3.6.tar.gz
$ kubectl apply -f metrics-server-0.3.6/deploy/1.8+/

4)install dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml

5)kubectl proxy

6)brawler UI
http://127.0.0.1:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

7)auth login
kubectl apply -f https://raw.githubusercontent.com/hashicorp/learn-terraform-provision-eks-cluster/master/kubernetes-dashboard-admin.rbac.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep service-controller-token | awk '{print $1}')

