# Module 3: MLOps and Real World Scenarios: Get Hands-On

## Part 0 - Checking the Pre-requisites

### Register for GCP Free Trial

Register for GCP Free Trial through which $300 Credits for a 90 days period can be availed.
Additionally, the GCP Free Tier can be leveraged.
Check out the [official documentation](https://cloud.google.com/free) for more details.

**Notes:**

- Use GCP Cloud Shell for executing the rest of the terminal commands, as outlined in this section.
- **It is highly recommended to destroy the complete GCP setup post the workshop to avoid unnecessary charges/cost.** Check out [Deleting GCP Projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects#shutting_down_projects) for more details.
- Stay on top of the Billing Reports and charges for the GCP Organization. Use [GCP Pricing Calculator](https://cloud.google.com/products/calculator) to estimate costs before using and GCP service.

### Create a Docker Hub Account

Check out the [official documentation](https://docs.docker.com/docker-id/) for creating the Docker Hub account.

### GKE Cluster Creation

```bash
export PROJECT_ID="<gcp-project-id>"
export GKE_CLUSTER_NAME="cost-optimized-gke-mlrun-cluster-00"

gcloud services enable container.googleapis.com --project "$PROJECT_ID"

gcloud beta container --project "$PROJECT_ID" clusters create "cost-optimized-cluster-2-clone-1" --zone "us-central1-c" --no-enable-basic-auth --cluster-version "1.27.2-gke.1200" --release-channel "regular" --machine-type "e2-standard-8" --image-type "COS_CONTAINERD" --disk-type "pd-balanced" --disk-size "100" --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --max-pods-per-node "110" --num-nodes "3" --logging=SYSTEM,WORKLOAD --monitoring=SYSTEM --enable-ip-alias --network "projects/$PROJECT_ID/global/networks/default" --subnetwork "projects/$PROJECT_ID/regions/us-central1/subnetworks/default" --no-enable-intra-node-visibility --default-max-pods-per-node "110" --enable-autoscaling --min-nodes "0" --max-nodes "5" --location-policy "BALANCED" --security-posture=standard --workload-vulnerability-scanning=disabled --no-enable-master-authorized-networks --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --enable-autoprovisioning --min-cpu 8 --max-cpu 16 --min-memory 32 --max-memory 64 --autoprovisioning-scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --enable-autoprovisioning-autorepair --enable-autoprovisioning-autoupgrade --autoprovisioning-max-surge-upgrade 1 --autoprovisioning-max-unavailable-upgrade 0 --autoscaling-profile optimize-utilization --enable-managed-prometheus --enable-vertical-pod-autoscaling --enable-shielded-nodes --node-locations "us-central1-c" && gcloud beta container --project "$PROJECT_ID" node-pools create "nap-e2-standard-2-1fxej3oy" --cluster "$GKE_CLUSTER_NAME" --zone "us-central1-c" --machine-type "e2-standard-2" --image-type "COS_CONTAINERD" --disk-type "pd-balanced" --disk-size "100" --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --enable-autoscaling --min-nodes "0" --max-nodes "1000" --location-policy "BALANCED" --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --max-pods-per-node "110" --node-locations "us-central1-c"
```

### Connect to the GKE Cluster

```bash
gcloud container clusters get-credentials $GKE_CLUSTER_NAME --zone us-central1-c --project $PROJECT_ID
```

### Create Kubernetes Namespace

```bash
kubectl create namespace mlrun
```

### Configure DockerHub Credentials

```bash
kubectl --namespace mlrun create secret docker-registry registry-credentials
--docker-server https://index.docker.io/v1/
--docker-username <dockerhub-username>
--docker-password <dockerhub-password>
--docker-email <dockerhub-emailid>
```

### Create a Storage Class

Save the following configuration in a file named `local-path.yaml`.

```yaml
allowVolumeExpansion: true
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  annotations:
    components.gke.io/layer: addon
  labels:
    addonmanager.kubernetes.io/mode: EnsureExists
    k8s-app: gcp-compute-persistent-disk-csi-driver
  name: local-path
parameters:
  type: pd-balanced
provisioner: pd.csi.storage.gke.io
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

Apply the Storage Class configuration, as shown below.

```bash
kubectl apply -f local-path.yaml
```

### Helm 3 Installation and Configuration

- Helm 3 can be installed by following the instructions on the [official documentation](https://helm.sh/docs/intro/install/).
- Add MLRun Helm Repository.

```bash
helm repo add mlrun https://mlrun.github.io/ce

helm repo update
```

- Install the MLRun Helm Chart.
  **Note:** Get the Kubernetes Worker Node's External IP by using the following command.

```bash
kubectl get nodes -o wide
```

Use this Kubernetes Worker Node External IP while installing the Helm Chart.

```bash
helm --namespace mlrun \
    install mlrun-ce \
    --wait \
    --timeout 960s \
    --set global.registry.url=index.docker.io/<dockerhub-registry-name> \
    --set global.registry.secretName=registry-credentials \
    --set global.externalHostAddress=<kubernetes-worker-node-external-ip> \
    --set nuclio.dashboard.containerBuilderKind=docker \
    --debug \
    mlrun/mlrun-ce
```

### Install Redis Helm Chart (this should be installed only during the NLP workshop)

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami

helm --namespace mlrun install redis bitnami/redis --version 17.14.5

# Note: The password for Redis can be fetched using the following command.
export REDIS_PASSWORD=$(kubectl get secret -n mlrun redis -o jsonpath="{.data.redis-password}" | base64 -d)

# Note: The `redis_url` can be built using the following format.
# redis://:<redis_password>@redis-master.mlrun.svc.cluster.local:6379
```

## Disclaimer

The workshop relies on using GCP Free Trial $300 Credits for demonstrations. It is highly recommended to stay on top of the cost and charges being incurred, and the credits being used/left. The attendees are encouraged to destroy the complete workshop related GCP setup after the workshop is concluded, to avoid incurring any additional unnecessary costs. The speaker nor any of the organizations / professional bodies / institutes the speaker is part of / associate with will be responsible for a situation where the attendee forgets to delete the resources created during the workshop and incurs any cost afterwards. The attendees understand that they will be solely responsible for the billing associated to GCP workloads, if any appears. The workshop is just 7-8 hours long and only needs one small sized GKE Cluster (only one 8 Core and 16 GB Worker Node is required; some small Persistent Volumes will be created as well).
