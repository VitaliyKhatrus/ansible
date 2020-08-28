### Before you start

In this case we will use CloudFormation stacks (include nested) to create infrastructure for our k8s cluster and pass variables from CloudFormation `Outputs` into ansible.

For this purpose we have to create `s3 bucket`

``` $ aws --region us-west-2 s3api create-bucket --bucket eks-clfn-us-west-2 --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2```

Pack the `roles/cloudformation/files/eks-root.json` to the AWS S3 and save a resulted file to the `roles/cloudformation/files/` as `packed-eks-stacks.json`

``` 

cd roles/cloudformation/files/

aws --region us-west-2 cloudformation package --template-file eks-root.json --output-template ./packed-eks-stacks.json --s3-bucket eks-clfn-us-west-2 --use-json

```

Please, edit file `group_vars/all.yml` and specify some variables like `env, region, vpc_cidr_block, k8s_version`

also specify parameters for the worker nodes here `roles/eksctl/templates/nodes.yaml`

### To create kubernetes cluster use command
```ansible-playbook eks-cluster.yml ```

`eks-cluster.yml` include two roles:

- cloudformation
- eksctl

which can be used separately

### To delete cluster use command

``` ansible-playbook delete-cluster.yaml ```
