# 🧪 Demo: Reproducing Failure Scenarios

---

## 🔹 Prerequisites

- Kubernetes cluster (Minikube recommended)
- `kubectl` configured
- Accomplish AI installed
- Skill loaded (`k8s-sre-agent`)

---

# 🚨 Scenario 1: Broken Image

## Apply faulty deployment

```bash
kubectl apply -f examples/broken-nginx.yaml
```

## Expected Result

- Pod enters `ImagePullBackOff` state

## Agent Will

- Detect failure
- Generate `SRE_Incident_Report.md`
- Propose fix
- Wait for approval
- Patch deployment
- Verify Running state

---

# 🚨 Scenario 2: OOMKilled Pod

## Apply memory-stressed pod

```bash
kubectl apply -f examples/oom-demo.yaml
```

## Expected Result

- Pod crashes due to memory limit
- Exit Code `137`
- Status shows `OOMKilled`

## Agent Will

- Identify `OOMKilled`
- Propose memory adjustment
- Wait for approval
- Apply fix
- Verify pod health

---

# 🤖 How to Run with Accomplish AI

1. Open Accomplish AI
2. Go to **Skills**
3. Upload `SKILL.md`
4. Start chat with:

```bash
/k8s-sre-agent
```

The agent will:

- Scan the Minikube cluster
- Check all namespaces, deployments, and pods
- Propose solutions
- Generate `k8s_incident_report.md`
- Wait for approval
- Apply fix
- Verify cluster health