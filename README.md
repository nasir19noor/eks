### Install Kubectl
sudo curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.24.7/2022-10-31/bin/linux/amd64/kubectl

sudo chmod +x ./kubectl

sudo mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin

sudo echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc

source /root/.bashrc

kubectl version --short --client

### Install Eksctl
sudo curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin

export PATH=$PATH:/usr/local/bin

sudo echo 'export PATH=$PATH:/usr/local/bin' >> /root/.bashrc

source /root/.bashrc

eksctl version

### Upgrade AWS CLI
sudo yum remove awscli -y

sudo curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

sudo unzip awscliv2.zip

sudo ./aws/install

sudo ln -s /usr/local/bin/aws /bin/aws

aws --version

### AWS IAM Authenticator
curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.5.9/aws-iam-authenticator_0.5.9_linux_amd64

chmod +x ./aws-iam-authenticator

mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin

echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc

aws-iam-authenticator version

### Create EKS Cluster
eksctl create cluster --name nginxcluster --nodes-min 1 --nodes-max 2 --alb-ingress-access --region ap-southeast-1 --node-type t2.medium --ssh-public-key cc-nasir

kubectl get nodes

### Install Deplyment and Service
git clone https://github.com/nasir19noor/eks.git

cd eks

kubectl apply -f nginx.yaml

kubectl get pods

kubectl apply -f nginx-service.yaml

kubectl get svc -o wide

### Delete Resources

kubectl delete service nginx

kubectl delete deploy nginx

eksctl delete cluster --name nginxcluster --region ap-southeast-1 --wait

