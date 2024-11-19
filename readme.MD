# Ingress controller

eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster expense-1 \
    --approve

---(policys)
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.2.1/docs/install/iam_policy.json

----
aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json


---

eksctl create iamserviceaccount \
--cluster=expense-1 \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::277083512436:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve

---

helm repo add eks https://aws.github.io/eks-charts

---

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=expense-1 --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller

