# Helm Chart Template Properties Documentation (YAML Props)

This document explains every YAML property (key) used in the chart templates, what it is, and why it is needed. Use this as a reference to understand and maintain the chart.

---

## deployment.yaml
- **apiVersion**: Specifies the Kubernetes API version for the resource.
- **kind**: The type of Kubernetes resource (here, Deployment).
- **metadata**: Metadata about the resource (name, labels, annotations).
  - **name**: Unique name for the Deployment.
  - **labels**: Key-value pairs for identifying and grouping resources.
  - **annotations**: Key-value metadata for storing arbitrary non-identifying information.
- **spec**: Specification of the desired behavior of the Deployment.
  - **replicas**: Number of pod replicas to run.
  - **selector**: How to identify pods managed by this Deployment.
    - **matchLabels**: Labels that must match for pods to be managed.
  - **template**: Template for the pods.
    - **metadata**: Metadata for the pods (labels).
    - **spec**: Pod specification.
      - **serviceAccountName**: ServiceAccount to use for the pod.
      - **containers**: List of containers in the pod.
        - **name**: Name of the container.
        - **image**: Docker image to use.
        - **ports**: Ports to expose from the container.
          - **containerPort**: Port number the container listens on.
        - **resources**: Resource requests and limits for the container.
          - **requests**: Minimum resources guaranteed.
          - **limits**: Maximum resources allowed.
        - **livenessProbe**: Checks if the container is running.
          - **httpGet**: HTTP endpoint to check.
            - **path**: URL path to check.
            - **port**: Port to check.
          - **initialDelaySeconds**: Delay before first probe.
          - **periodSeconds**: How often to probe.
          - **timeoutSeconds**: Probe timeout.
          - **failureThreshold**: Number of failures before restart.
        - **readinessProbe**: Checks if the container is ready for traffic.
          - (same structure as livenessProbe)

---

## service.yaml
- **apiVersion**: Kubernetes API version for Service.
- **kind**: Type of resource (Service).
- **metadata**: Metadata for the Service.
  - **name**: Name of the Service.
  - **labels**: Labels for grouping/identification.
- **spec**: Service specification.
  - **type**: How the Service is exposed (e.g., LoadBalancer).
  - **selector**: Labels to select target pods.
  - **ports**: List of ports exposed by the Service.
    - **protocol**: Protocol used (TCP/UDP).
    - **port**: Port exposed by the Service.
    - **targetPort**: Port on the pod to forward to.

---

## ingress.yaml
- **apiVersion**: API version for Ingress.
- **kind**: Type of resource (Ingress).
- **metadata**: Metadata for the Ingress.
  - **name**: Name of the Ingress.
  - **labels**: Labels for grouping/identification.
  - **annotations**: Extra metadata for controllers/cert-manager, etc.
- **spec**: Ingress specification.
  - **ingressClassName**: Which Ingress controller to use.
  - **rules**: List of routing rules.
    - **host**: Hostname to match.
    - **http**: HTTP rules.
      - **paths**: List of path rules.
        - **path**: URL path to match.
        - **pathType**: How to match the path (Prefix/Exact).
        - **backend**: Where to send matching traffic.
          - **service**: Service backend.
            - **name**: Name of the Service.
            - **port**: Service port.
              - **number**: Port number.

---

## serviceaccount.yaml
- **apiVersion**: API version for ServiceAccount.
- **kind**: Type of resource (ServiceAccount).
- **metadata**: Metadata for the ServiceAccount.
  - **name**: Name of the ServiceAccount.
  - **labels**: Labels for grouping/identification.
  - **annotations**: Extra metadata for identity/IAM.

---

## role.yaml
- **apiVersion**: API version for Role.
- **kind**: Type of resource (Role).
- **metadata**: Metadata for the Role.
  - **name**: Name of the Role.
  - **labels**: Labels for grouping/identification.
- **rules**: List of permission rules that define what actions this Role allows. Each rule specifies a set of resources, the API group they belong to, and the allowed actions (verbs).
  - **apiGroups**: Specifies which Kubernetes API group the rule applies to. The empty string ("") means the core API group (where ConfigMaps and Secrets live). Other values could be "apps", "batch", etc.
  - **resources**: The types of Kubernetes resources this rule applies to (e.g., "configmaps", "secrets").
  - **verbs**: The allowed actions on those resources (e.g., "get", "list", "create", "update").

---

## rolebinding.yaml
- **apiVersion**: API version for RoleBinding.
- **kind**: Type of resource (RoleBinding).
- **metadata**: Metadata for the RoleBinding.
  - **name**: Name of the RoleBinding.
  - **labels**: Labels for grouping/identification.
- **roleRef**: Reference to the Role being bound.
  - **apiGroup**: API group of the Role.
  - **kind**: Kind of the referenced Role.
  - **name**: Name of the referenced Role.
- **subjects**: List of subjects to bind the Role to.
  - **kind**: Type of subject (ServiceAccount).
  - **name**: Name of the subject.
  - **namespace**: Namespace of the subject.

---

If you add new YAML properties to the templates, update this file to keep documentation in sync.
