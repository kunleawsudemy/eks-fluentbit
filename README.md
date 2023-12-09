# Distribited Tracing using fluentbit


### Tutorial
![IMAGE ALT TEXT](http://img.youtube.com/vi/33VEu9Kqvno/0.jpg)

Logging in Kubernetes | Fluent Bit | Observability

### Installation

Fluent Bit is deployed as a DaemonSet, so that it will be available on every node of your Kubernetes cluster. To get started run the following commands to create the namespace, service account and role setup:

Prerequisites:
- Create an S3 Bucket: 
    S3 Bucket requires the following Permission in your node role:
    
```
{
	"Version": "2012-10-17",
	"Statement": [{
		"Effect": "Allow",
		"Action": [
			"s3:PutObject"
		],
		"Resource": "*"
	}]
}
```
**1.** Create Namespace
```bash
kubectl create ns logging
```

**2.** We will first setup the required role, rolebinding, namespace and service-account/
For Kubernetes v1.22+
```bash
kubectl apply -f install/v1.22
```

**3.** After setting up the required kubernetes objects, we can now install the fluentbit(daemonsets) and it's configuration (configmap).
```bash
kubectl apply -f ./fluentbit/
```


TROUBLESHOOTING Fluent Bit

Check fluent bit config
    https://docs.fluentbit.io/manual/administration/configuring-fluent-bit/classic-mode/configuration-file

Check Fluent Bit daemonset is running
```
kubectl get daemonsets -n <fluentbit-namespace>
```

If FluentBit is not sending logs, restart fluentbit daemonset by deleting fluentbit pod (This will recreate the daemonset)
```
kubectl delete pod fluent-bit-4dnc2 -n logging
```

References:

**I.** Fluentbit: https://docs.fluentbit.io/manual/administration/configuring-fluent-bit/classic-mode/configuration-file

**II.** S3-Plugin: https://docs.fluentbit.io/manual/pipeline/outputs/s3

**III.** Fluentbit Helm Chart: https://artifacthub.io/packages/helm/fluent/fluent-bit/0.15.15

Troubleshooting Fluent Bit: 

**IV.** https://kube-logging.dev/docs/operation/troubleshooting/fluentbit/

**V.** https://calyptia.com/blog/troubleshooting-your-fluent-bit-configuration-with-calyptia-cloud