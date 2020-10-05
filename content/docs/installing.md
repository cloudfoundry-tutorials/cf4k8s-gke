+++
title = "Installing cf-for-k8s"
weight = "3"
+++


Now we get into the actual installation process.

- Clone the cf-for-k8s repo. 

  ```
  git clone https://github.com/cloudfoundry/cf-for-k8s.git
  ```

- Switch into the cloned directory.
  
  ```
  cd cf-for-k8s
  ```

- Create a directory to use as a temporary/swap space for the YAML files. Assign a local variable named TMP_DIR the path to this directory. We will be using this directory to store the YAML files that we generate and use for the final installation.

  ```
  mkdir -p ~/tempdir
  TMP_DIR=~/tempdir
  ```

- A YAML file containing the deployment manifest is needed. This will contain the details of the endpoint, our container registry credentials, and static ip among other things. You can generate this by yourself or make use of the auto-generate script that is included in the cf-for-k8s repo. This is a temporary measure meant for illustrative purposes only.

  The command used to create the YAML file is:

  ```
  ./hack/generate-values.sh -d ${IP}.xip.io > $TMP_DIR/cf-values.yml
  ```

  Don’t be alarmed when you see this message:

  ```
  WARNING: The hack scripts are intended for the development of cf-for-k8s. They are not officially supported product bits. Their interface and behavior may change at any time without notice.
  ```
  
Autogen Values & Set Static IP
Two important parts of the cf-for-k8s install process — we set the static IP on the server, insert that static ip as…
asciinema.org
Step 5:
A couple of additions have to be made to the cf-yalues.yml file that you created.
1. Add credentials that can help connect to your Container Registry. This could be Docker Hub or any private one.
app_registry:
    hostname: https://index.docker.io/v1/ 
    repository_prefix: “<my_username>” 
    username: “<my_username>” 
    password: “<my_password>”
2. Append the static IP address we generated before to the cf-values.yml file. This is to help assign the ingress gateway of the Kubernetes installation.
echo “istio_static_ip: \”$IP\”” >> $TMP_DIR/cf-values.yml
Step 6:
Next, use the ytt tool to generate the final template to be used for the installation.
ytt -f config -f $TMP_DIR/cf-values.yml > $TMP_DIR/cf-for-k8s-rendered.yml
Step 7:
Finally, use the kapp command to install cf-for-k8s on your Kubernetes cluster.
kapp deploy -a cf -f $TMP_DIR/cf-for-k8s-rendered.yml -y
cf-for-k8s Install
Screencast capturing the entire installation process for cf-for-k8s.
asciinema.org
Wait for the installation to complete. It takes an estimated 8–10 minutes to complete.