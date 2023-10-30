# webapp-helm-chart
This repository contains a Helm chart for deploying our web application on Kubernetes. The Helm chart simplifies the deployment process, making it easy to configure and manage our web application in various Kubernetes clusters.
# Contents
1.Values Configuration: Inside the Helm chart, you will find a values.yaml file that allows you to configure various aspects of the deployment. This file contains default values and descriptions for each configuration option, making it easy to customize the deployment according to your needs.

2. Templates: The templates directory within the Helm chart contains the template files for Kubernetes resources. These templates are used to generate the actual Kubernetes manifests during the deployment process.

3. Charts: If the Helm chart has dependencies on other charts, you'll find them in the charts directory. These subcharts are included to simplify the management of complex deployments.
