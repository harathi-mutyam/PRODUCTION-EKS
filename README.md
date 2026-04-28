**Changes were made to the EKS project on 27/04/2026.**

**For EKS-Project**

Steps to Clone and Run the Project

**1. Create a Local Folder**

Create a folder in your local directory named production-eks.

**2. Clone the Repository**

Open VS Code (or Git Bash) and clone the repository:

git clone https://github.com/harathi-mutyam/PRODUCTION-EKS.git

**3. After Cloning the Repository**

Make the following changes:

Update the region in dev.tfvars based on the location where you want to create your infrastructure (such as EKS, VPC, etc.).

Example:

region = "us-east-1"

Ensure the region in backend.tf matches the region where your Terraform state storage (for example, an S3 bucket) is hosted.

**4. Navigate to the Terraform Directory**

Always run Terraform commands from the folder where main.tf exists:

cd eks-project/terraform/eks

**5. Verify Terraform Installation**

terraform version

**6. Initialize Terraform and validate it**

terraform init

terraform validate

**7. Plan the Infrastructure**

terraform plan -var-file="dev.tfvars"

**8. Apply the Changes**

terraform apply -var-file="dev.tfvars"

**changes were made on 28/4/2026**



Removed duplicate OIDC-related resources from eks.tf since they already exist in iam.tf as eks_oidc.

The following OIDC provider block was commented out to prevent duplication:

/* resource "aws_iam_openid_connect_provider" "eks-oidc" {

  client_id_list  = ["sts.amazonaws.com"]
  
  thumbprint_list = [data.tls_certificate.eks-certificate.certificates[0].sha1_fingerprint]
  
  url             = data.tls_certificate.eks-certificate.url
  
} */

Also removed the duplicate TLS certificate data source in iam.tf to keep a single source of truth:

/* data "tls_certificate" "eks-certificate" {

  url = aws_eks_cluster.eks[0].identity[0].oidc[0].issuer
  
} */

This ensures the OIDC configuration is defined only once in iam.tf, avoiding conflicts and duplication.






