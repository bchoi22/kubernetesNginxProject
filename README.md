# kubernetesNginxProject

This project is a work-in-progress.  Although the 'kubectl get deployments' command works (it shows the nginx deployments in the kubernetes cluster and in a running state), the problem is that I'm still having an issue connecting the ALB to the private instances housing my eks cluster.  The ALB, although deployed, is not connecting to my target groups.  I believe the issue is that the NAT is not allocating the proper public ip for the private one.  One of the ways to address this is, I believe, is to break everything out to include defining the route tables.

To see the deployments for both search-api and graph-api, execute the following lines:

1. Go into a root directory where you'd like to emplace the archive.zip file.  Then:

2. brew install awscli (make sure to install package manager, homebrew, if you're running into errors)

3. aws configure
AWS Access Key ID [None]: YOUR_AWS_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_AWS_SECRET_ACCESS_KEY
Default region name [None]: YOUR_AWS_REGION
Default output format [None]: json

4. brew install aws-iam-authenticator

5. brew install kubernetes-cli

6. mkdir myeksproject

7. Download and install myProject files into the directory myeksproject

8. cd learn-terraform-provision-eks-cluster

9. terraform init

10. terraform apply (in case you run into any host access errors, run this: terraform state rm module.eks.kubernetes_config_map.aws_auth.  Then try again).

11. aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)
Do this to configure kubectl with your credentials

12. cd eks-nginx

13. terraform init

14. terraform apply (in case you run into kubeconfig issues regarding not finding the host, run this: terraform state rm module.eks.kubernetes_config_map.aws_auth.  Then try again).

15. kubectl get deployments
You should see,
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
search-api   2/2     2            2           15s
graph-api    2/2     2            2           15s

16. Still need to connect the ALB to NATs within the public subnets properly so that the search.altana.ai and graph.altana.ai hostnames can be reached.  TBD.
