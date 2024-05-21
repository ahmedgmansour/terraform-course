# Setting up AWS EKS (Hosted Kubernetes)

See https://www.terraform.io/docs/providers/aws/guides/eks-getting-started.html for full guide


## Download kubectl
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.23.6/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
https://gist.github.com/Zheaoli/335bba0ad0e49a214c61cbaaa1b20306
https://github.com/aws/aws-cli/issues/6920
https://github.com/kubernetes-sigs/aws-iam-authenticator/releases
https://stackoverflow.com/questions/71318743/kubectl-versions-error-exec-plugin-is-configured-to-use-api-version-client-auth

## Download the aws-iam-authenticator
```
wget https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.6.11/aws-iam-authenticator_0.6.11_darwin_amd64
chmod +x aws-iam-authenticator_0.6.11_darwin_amd64
sudo mv aws-iam-authenticator_0.6.11_darwin_amd64 /usr/local/bin/heptio-authenticator-aws
```

## Modify providers.tf

Choose your region. EKS is not available in every region, use the Region Table to check whether your region is supported: https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/

Make changes in providers.tf accordingly (region, optionally profile)

## Terraform apply
```
terraform init
terraform apply
```

## Configure kubectl
```
terraform output kubeconfig # save output in ~/.kube/config
aws eks --region <region> update-kubeconfig --name terraform-eks-demo
```

## Configure config-map-auth-aws
```
terraform output config-map-aws-auth # save output in config-map-aws-auth.yaml
kubectl apply -f config-map-aws-auth.yaml
```

## See nodes coming up
```
kubectl get nodes
```

## Destroy
Make sure all the resources created by Kubernetes are removed (LoadBalancers, Security groups), and issue:
```
terraform destroy
```
