Kubernetes extension for VSTS 
===

Enable Kubernetes extension for VSTS. Kubernetes endpoint for kubectl config and kubectl apply build task.
Mainly aim for using Linux Hosted Agent(preview).


Currently, stable version tasks are version 0.x.x.or 1.x.x.  2.0.0 or later is preview. 


[GitHub: KubernetesTask](https://github.com/TsuyoshiUshio/KubernetesTask)

![Header](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/Header.png)

# 1 How to use this

## 1.1. Create an endopint 

### Choose Kubernetes endpoint

Choose Kubernetes endpoint.

![Kubernetes Endpoint](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/endpoing01.png)

### Set your endpoint 

Kubernetes Connection pops up. Then fill the box.

![Kubernetes Connection](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/endpoint02.png)

* Connection name: Endpoint name (Anything is OK)
* Server URL : K8s Cluster URL for memo (Not used for the task until now)
* kubeconfig : Copy & paste your config file 

You can get config file from your k8s master node `.kube/config`. 
See [Microsoft Azure Container Service Engine - Kubernetes Walkthrough](https://docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough)
Then copy the file content and paste on kubeconfig column.

**NOTE**  

If you copy config file into Kubeconfig, the build log of VSTS might show you the contents.
This happens when you copy multiline. 
Kubernetes tasks support base64 encoding for Kubeconfig column. If you want to avoid this problem,
you can convert your config file into base64 string. You can find the tool for converting config file. `tools/convert.ts`

**Usage**

your config file should be `LF` not `CRLF` if you want to use on Linux hosted build.

```
tsc -p .
node tools/convert.js {filename}
```

You can find {filename}_new file which include base64 encoding string.


## 1.2. Store and link kubectl command link with VSTS private repository

If you use ver 2.0, you don't need this operation. However, this operation could fasten your pipeline without downloading a kubectl binary.

Link your repo which has kubecntl command. 

![Link Artifact](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/linkaritifact.png)

Please `chmod +x kubectl` before adding kubectl to your repo.

![VSTS Private Repository](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/repo01.png)

## 1.3. Setup your kubectlapply task

Then you can use the endpoint, specify the kubectl command and YAML file 
for deployment. Internally, it calls `kubectl apply` command. 

![kubectlapply Task](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/apply.png)

If you want to change the YAML file dynamically, you can use [Replace tokens](https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens) on the VSTS Marketplace.

You can see the `downloadVersion` textbox. If you don't specify `KubectlBinary`, this task automatically download the latest
kubebinary. If you want to specify the version, fill the `downloadVersion`, e.g. `v1.5.2`.

## 1.4. Setup your kubectlgeneral task

You can use every kubectl command you want. Use kubectlgeneral task.
You can specify a lot of arguments separated with space or new line. 

![kubectlgeneral Task](https://raw.githubusercontent.com/TsuyoshiUshio/KubernetesTask/master/docs/images/general.png)

You can see the `downloadVersion` textbox. If you don't specify `KubectlBinary`, this task automatically download the latest
kubebinary. If you want to specify the version, fill the `downloadVersion`, e.g. `v1.5.2`.

# 2. Project GitHub

Any contribution is welcome!

[TsuyoshiUshio/KubernetesTask](https://github.com/TsuyoshiUshio/KubernetesTask)

