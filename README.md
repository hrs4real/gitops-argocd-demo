# GitOps with Argo CD and Argo Rollouts
This repository contains the setup for a GitOps pipeline using Argo CD for continuous deployment and Argo Rollouts for advanced deployment strategies within a Kubernetes environment.
## Table of Contents
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Usage](#usage)
- [Clean Up](#clean-up)
- [Conclusion](#conclusion)
## Prerequisites
Before you begin, ensure you have the following installed:
- Kubernetes cluster (e.g., Minikube, kind, or a cloud provider's Kubernetes service)
- Docker
- Git
- Argo CD
- Argo Rollouts
## Setup
1. **GitHub Repository**: Create a GitHub repository to host your Kubernetes manifest files.

![1](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/35f0183c-bd9f-4b5b-a892-ff8685f40c49)

2. **Demo Flask Web App**: Write a simple Flask web app for demonstration purposes.

![2](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/cb21eee5-69fd-40f2-b634-9eb099c6ad96)

3. **Dockerfile**: Write a Dockerfile for the Flask app.

![3](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/00f9e7e9-03a9-4f5b-a43f-69c458ae6c33)

4. **Build and Push Docker Image**: Build the Docker image (e.g., `demo-project:v1`) and push it to Docker Hub. Repeat for `v2`.

![4](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/820ba61a-b983-431f-82e1-ed7fdca6f93b)

![5](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/79207123-d76f-4452-a28f-9b79d969fe64)

![6](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/0800f557-9299-4dcb-8dca-8662b5f4e38d)


5. **Add Manifest Files**: Add `deployment.yml` and `service.yml` manifest files to your GitHub repository.

![7](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/b6974de0-8224-4677-bf76-58e98729df85)

![8](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/5ddee1ee-fc53-4de2-bf0d-ab4ccc525cfb)


6. **Install Argo CD**:

    - **Single Node Installation**: Install Argo CD on a single node Kubernetes cluster.<br>
      `kubectl create namespace argocd` <br>
      `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`

   ![9](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/9a4cc0d1-5fc7-4d81-b47f-982d3afe8869)

   ![10](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/b74a9dd4-dd87-40a8-bd06-d9941644f07e)

    - **Setup Argo CD UI Dashboard**: To access the Argo CD UI dashboard, you need to expose the Argo CD server service. Edit the `argocd-server` service to change the type from `ClusterIP` to `NodePort`:<br>
    `kubectl edit svc argocd-server -n argocd` **OR**<br>
    `kubectl port-forward svc/argocd-server -n argocd`<br>

    ![11](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/6178ca00-6a75-4ddd-b16a-f4f47fedd388)

    ![12](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/61ae4c84-f134-4e93-823d-f4d2d392fa29)

   ![13](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/2f817b2c-bff7-44fe-886e-44e862293b6d)

   - **Retrieve Login Password**: The default username for Argo CD is admin. You can retrieve the password by running the following command:<br>
   `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`

  ![14](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/08756d51-b608-4b92-9bf7-4933106ac753)

  ![15](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/70eed16b-e79a-4826-86ec-becb62afc7e5)


7. **Installing Argo Rollouts**: Install the Argo Rollouts controller in your Kubernetes cluster by applying the following manifest:<br>
`kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-rollouts/stable/manifests/install.yaml`

![16](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/99966c41-cfa1-4a5b-b5cf-a1a0cde2d1cd)

![17](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/ce1c385e-e522-4237-b76f-f3234e65d4c9)

## Usage
1. **Setting Up GitHub Repo**: Add your GitHub repository to the Argo CD dashboard in the default project. Click on the "Settings" icon in the top menu, select "Repositories", and then click on "Connect repo using HTTPS".

![18](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/b2760de7-1f46-4cfb-b2c6-47a21ba7c834)

![19](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/09caf333-f052-41dd-81f5-f278cfb8c90d)

2. **Creating Application**: Click on the "New Application" button in the Argo CD dashboard to create a new application. Enter the details for your application, including the repository URL, target revision (e.g., HEAD), and path to the Kubernetes manifests (e.g., /).

![20](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/69ea3408-f46c-4857-a21c-358803e9c0b7)

![21](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/8f83cde3-0f41-484b-83c9-540e7268f275)

3. **Deploying Application**:  Argo CD will automatically deploy your application based on the manifests in your GitHub repository. You can monitor the deployment progress in the Argo CD dashboard.

![22](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/aa39c326-5f15-407c-b962-3a97387f7c91)

4. Now **updating the replica=4** in deployment.ym file.

![23](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/ff2ef8c4-b7eb-437a-a971-4f804ba4eb60)

5. **Monitoring the update in ArgoCD dashboard**

![24](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/aaf7679c-1360-4d93-affc-718ece83aac2)

## Clean Up
To clean up the resources created during this setup, follow these steps:
1. Delete the application using the Argo CD dashboard.

![25](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/3cc3cb5e-1505-4835-8885-917429e97cb4)

![26](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/0d3e9399-9097-4ab4-a3dd-b169080e87c4)

2. Uninstall Argo CD and Argo Rollouts from your Kubernetes cluster.

![27](https://github.com/hrs4real/gitops-argocd-demo/assets/92949812/edb2f63c-1093-4a91-94f2-2f5860318614)

## Conclusion
In conclusion, this documentation has provided a comprehensive guide to setting up a GitOps pipeline using Argo CD and Argo Rollouts. By following the steps outlined here, you can automate the deployment and management of your applications in a Kubernetes environment, leveraging the power of Git for version control and Argo CD for continuous deployment. 

GitOps offers a robust and efficient approach to managing Kubernetes clusters, allowing you to easily track changes, rollback to previous versions, and implement advanced deployment strategies like canary releases. With Argo CD and Argo Rollouts, you can enhance the reliability and scalability of your applications, making them more resilient to failures and changes.

We hope this documentation has been helpful in getting you started with GitOps using Argo CD and Argo Rollouts. Feel free to explore further and adapt these practices to suit your specific requirements and workflows.
