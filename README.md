# connect_eks

This is a helper tool for EKS cluster management.
You can use kubectl, awscli2, ekscli and helm easily.

## How to use

Set your AWS credentials to environment values.

```
ex)
export AWS_ACCESS_KEY_ID=xxxxx
export AWS_SECRET_ACCESS_KEY=xxxxx
export AWS_SESSION_TOKEN=xxxx
```

Start this tool.

```
./connect_eks
```

A docker image will be built at first time.

### update the target cluster or the namespace.

```
update_config -c CLUSTER_NAME -n NAME_SPACE
```

Now you can use below example commands.

```
kubectl get ns
kubectl get pod
kubectl get svc
kubectl get pv
kubectl get ingress
.
.
eksctl get cluster
.
.
aws ec2 describe-instances
.
.
helm list
.
.


```

- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [eksctl](https://eksctl.io/)
- [awscli2](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-chap-welcome.html)
- [helm](https://helm.sh/)


Let's enjoy kubernetes!

