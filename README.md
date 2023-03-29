# VeradigmTakeHomeTest
 
The main.tf is the terraform configuration to make the eks cluster "my-eks-cluster". 

## How Terraform provisions the EKS cluster:
Terraform is an infrastructure-as-code tool that allows users to declaratively deploy and manage cloud resources. Terraform has an EKS module that makes it easier to set up and manage the Kubernetes control plane and worker nodes when building an Amazon Elastic Kubernetes Service (EKS) cluster.

A number of elements that make up the EKS module combine to form an EKS cluster include:
1. Amazon EKS Control Plane: Terraform sets up the Kubernetes control plane, which consists of the worker nodes that run containerized apps and the master nodes that regulate the cluster's overall state.
2. VPC: You can launch resources in a virtual network from a Virtual Private Cloud (VPC), which is a logically isolated area of the AWS cloud. The EKS cluster can communicate with other AWS services using the VPC that Terraform constructs.
3. Subnets: To enable various levels of access to the resources in the EKS cluster, Terraform constructs public and private subnets within the VPC.
4. Security Groups: Terraform creates security groups that define the rules for inbound and outbound traffic to the resources in the EKS cluster.
5. Node Groups: Terraform creates worker nodes within the EKS cluster by launching EC2 instances and configuring them to connect to the EKS control plane. These nodes are organized into groups that provide scalability and resiliency for the cluster.
6.IAM Roles: With Terraform, IAM roles and policies are created, allowing worker nodes to access the required AWS resources including S3 buckets, CloudWatch logs, and ECR repositories.

I just provided the configuration for a simple cluster so I only included 1 and 2. 

## How to verify the eks cluster exists
You can use the Terraform aws eks cluster data source to see if an Amazon Elastic Kubernetes Service (EKS) cluster is present. You can retrieve information about an existing EKS cluster, such as its name, endpoint, security group, and subnets, using the aws eks cluster data source.

Here is an example of Terraform code that determines whether an EKS cluster with a particular name already exists:
data "aws_eks_cluster" "my_cluster" {
  name = "my-eks-cluster"
}

resource "null_resource" "verify_eks_cluster" {
  # This will trigger an error if the EKS cluster doesn't exist
  # because the `aws_eks_cluster` data source will fail to fetch
  # the details of the non-existent EKS cluster.
  
This code snippet retrieves information on the EKS cluster with the name "my-eks-cluster" using the aws eks cluster data source. The data.aws eks cluster.my cluster object will include information on the EKS cluster if it already exists, and the null resource.verify eks cluster resource will be successfully created. The creation of the null resource.verify eks cluster resource will fail if the EKS cluster doesn't exist, and the data.aws eks cluster.my cluster object will be empty.

Note that using the aws eks cluster data source to retrieve the EKS cluster's details requires that the proper Amazon credentials be setup and set up in your Terraform configuration.
  depends_on = [data.aws_eks_cluster.my_cluster]
}

