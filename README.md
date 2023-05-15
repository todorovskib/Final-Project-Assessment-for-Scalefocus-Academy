# Final-Project-Assessment-for-Scalefocus-Academy
Deploy a WordPress on Kubernetes (using Minicube) with Helm and automation with Jenkins.
Prerequisites:

1. Install the necessary tools: Minicube, Helm and Jenkins.

Minikube:	

Firstly we go to the website https://minikube.sigs.k8s.io/docs/start/ where we follow the instruction on how to download and install Minicube
We execute the following commands in PowerShell 
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' –UseBasicParsing
And then
$oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(';') -inotcontains 'C:\minikube'){ `
  [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}
After that I use the command minikube start in cmd to see if we have properly installed minikube and that it is working. Since this command has yielded and error: Exiting due to HOST_VIRT_UNAVAILABLE: Failed to start host: creating host: create: precreate: This computer doesn't have VT-X/AMD-v enabled. Enabling it in the BIOS is mandatory.
We have found a workaround with using the command 
 ![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/ce030124-4c8d-4922-bf78-4afe9b044339)

As we can see we have started minikube.

![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/ef793a46-e82a-4b9f-96bd-d1738bbf84bc)

 

Helm:
Next we will install Helm. This will be done by following the instruction on the website https://helm.sh/docs/intro/install/ . I install it by using the command choco install kubernetes-helm since I have Chocolatey.
 ![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/c8e87545-b439-4a7b-b923-e21fb86362ae)

Jenkins:
And finally as the last prerequisite we will download Jenkins for windows. We will do so by following the instructions on the website: https://www.jenkins.io/doc/book/installing/windows/ 
After some installation of Security policies , updating java services  and clearing ports we have installed Jenkins as a service  
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/433435ea-aa17-4364-a048-43db78bb8fc6)
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/6adf4c08-7ea2-4e36-8ba9-16bdd9fe9423)

2. Separate repo in your GitHub Profile named: Final Project Assessment for Scalefocus Academy
Requirement for the Project Assessment:
 
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/27add985-fb45-4679-b69d-8b3e46ed8cf6)








1. Download Helm chart for WordPress. ( Bitnami chart: https://github.com/bitnami/charts/tree/main/bitnami/wordpress )
 ![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/4af32e5a-693c-4bd1-afb4-494db7fd91f8)

I have also downloaded it locally 
 ![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/33ea94ce-3bed-4e05-af88-90a70006349d)

2. In values.yaml, you need to change line 543 from type: LoadBalancer to type: ClusterIP ( Hint: there
will be one more problem when deploying. Resolve it. )


Then we change the type in line 534 to ClusterIP
 
 ![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/b7afbbce-15e2-4e9a-9080-cbba9a46964c)

Then we run the kubectl get services command and we can see that the Type is ClusterIP
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/85fc87ee-26b6-497c-a617-c98aa1a9a54e)

Now we set the local port with the following command 

![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/9f2b1020-7e95-4d31-a963-79456478f5a8)

and we can see that we have access to our wordpress page 

![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/b24c5ada-1298-47a0-a3e1-80a95b3e7ffd)


3. Create a Jenkins pipeline that checks if wp namespace exists, if it doesn’t then it creates one.

We create a Jenkinsfile and we deploy it on a pipeline in order to execute the commands as followed 
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/bc6e1d3b-4b14-47c5-92f7-cb95009a8800)
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/db57211a-df63-4876-8395-bde45f1999ae)

or alternativly 
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/a97ba763-c874-46d6-8260-dc8257183c31)


Checks if WordPress exists, if it doesn’t then it installs the chart.
4. Name the Helm Deployment as: final-project-wp-scalefocus.
We have saved the jenkins piple configuration as shown before.  Once we have saved the piple we are redirected to the projects page where we will build the solution. When cliclikg build now , it will beging a sequence of steps do be done.
 It will check first iw the wp namespace exists by running the kubectl get namespace wp command. 
 If the namespace doesn't exist, it will create it using the kubectl create namespace wp command.
 Next, the pipeline will check if the WordPress deployment with the name final-project-wp-scalefocus exists by running the helm list -n wp command.
 If the deployment doesn't exist, it will install the Helm chart using the helm install final-project-wp-scalefocus bitnami/wordpress --namespace wp -f path/to/modified/values.yaml command. 

6. Deploy the helm chart using the Jenkins pipeline.

7. Load the home page of the WordPress to see the final result.
![image](https://github.com/todorovskib/Final-Project-Assessment-for-Scalefocus-Academy/assets/118693625/70a9e4d9-ae2f-408e-9290-0f88e83917ff)
BONUS POINTS:
- Instead of using Minicube, consider using a different Kubernetes flavor of your choice, like k3s, k8s,
microK8s for bonus points during the grading. 
- While I have not done this in a different Kubernetes flavor ( have had many compatability problem with the regurlar one , and for sake of time will not experiment ) I understand and will provide why and when we should use the different flavours.
- In general the deployment part with kubectl and Helm is generally similar between all of the flavours that are mentioned above. The main differance would be slight varioations in command syntax or configuration options.
   - IN terms of cluster setup, Minikube handles the setup of a single-node cluster automatically, whereas other flavors like k3s, k8s, and microK8s may require additional steps to set up a multi-node cluster or customize the cluster configuration.
   - In terms of consumtion , Minikube and other flavors have varying resource requirements and footprints. Minikube tends to have a smaller resource footprint since it's optimized for local development, while other flavors may have different optimizations or trade-offs based on their target use cases
   - IN terms of features, Vanilla Kubernetes (k8s) offers the complete set of features and functionalities of Kubernetes, making it highly flexible and scalable. On the other hand, flavors like Minikube, k3s, and microK8s may have certain features or components optimized or pre-configured for specific use cases. For example, k3s emphasizes lightweight and resource-constrained environments, while microK8s provides a compact and fast way to run Kubernetes on local machines.
Overall, while the core concepts and deployment workflows of Kubernetes remain consistent across different flavors, there can be differences in installation, cluster setup, resource consumption, feature sets, and management tools

