### Step 4: Create a new AKS cluster

1. Create a new AKS cluster and attach the Grafana instance to it


### The Culprit: POSIX Path Conversion

When Git Bash sees a string starting with `/subscriptions/...`, it assumes it’s a Windows file path and often prepends `C:\Program Files\Git...` to it before sending it to the Azure CLI. This breaks the specific regex pattern the `aks-preview` extension is looking for.

---

### The Fix: Disable Path Conversion

You can fix this by setting an environment variable specifically for this command to tell Git Bash **"don't touch my paths."**

Try running the command with `MSYS_NO_PATHCONV=1` prefixed to it:

```bash
MSYS_NO_PATHCONV=1 az aks create \
  --name ${AKS_CLUSTER_NAME} \
  --resource-group ${RG_NAME} \
  --node-count 1 \
  --enable-managed-identity \
  --enable-azure-monitor-metrics \
  --enable-cost-analysis \
  --grafana-resource-id "${GRAFANA_RESOURCE_ID}" \
  --azure-monitor-workspace-resource-id "$(az monitor account show --name azmon-aks-labs --resource-group rg-advanced-mon --query id -o tsv)" \
  --tier Standard

```
---
---

### Create a dashboard in Grafana to visualize the new metric

Navigate to your Manage Grafana Instance, click on `Dashboards`->`AKS-Labs`->`API Server`

Click on Top Right Corner `edit` -> `Add` and then `Visualization`