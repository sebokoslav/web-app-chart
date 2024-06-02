This is a test Helm chart for a simple web app with a load balancer. 

The repo has its own self-hosted Helm chart repository at https://sebokoslav.github.io/web-app-chart/.
It's got a CD/CI that automatically builds the Helm chart and deploys it to the repo. 

# How to use this repo
Add the Helm chart repo locally:

```
helm repo add web-app-chart-repo https://sebokoslav.github.io/web-app-chart/
```

Install the chart:
```
helm install web-app-chart web-app-chart-repo/web-app-chart
```