+++
title = "Using your CF instance"
weight = "3"
+++


The ability to connect with the Cloud Foundry API endpoint is the best validation of the installation. You can do this by setting the endpoint of your local cf CLI as follows

```
cf api --skip-ssl-validation $IP.xip.io
```

This step will allow you to verify if the cf installation is functional. Next, log in to the instance to put the instance to some real use.

```
cf login
```

Create an org for your use and create spaces within them. Set the new space as your “target” to which you will deploy your applications.

```
cf create-org SampleOrg
cf create-space -o SampleOrg dev
cf target -o SampleOrg -s dev
```

The simplest app you can push is the Node.js app that is included within the cf-for-k8s directory. Use this command:

```
cf push test-node-app -p tests/smoke/assets/test-node-app
```

This will push a test application to the Kubernetes clusters. Use the URL endpoint generated and open the application in your browser.

## Conclusion

With this installation, you can help developers in your org realize the benefits of the simplified ‘cf push’ process while deploying to modern infrastructure. The cf-for-k8s community has got positive feedback for this abstraction layer over Kubernetes. There are several ongoing projects that aim to improve this offering.
