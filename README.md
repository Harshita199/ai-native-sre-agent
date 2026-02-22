# 🚀 AI-Native Kubernetes SRE Agent

## 🏆 Hackathon: Automate Me If You Can

An autonomous, AI-powered Site Reliability Engineering (SRE) agent built using **Accomplish AI**, designed to perform cluster-wide diagnostics, root-cause analysis, incident reporting, and safe, controlled remediation — all fully automated.

---

# 🎯 Problem Statement

During a real-world production outage, engineers commonly face issues such as:

- Pods entering `CrashLoopBackOff`
- `ImagePullBackOff` due to invalid or inaccessible images
- Containers repeatedly restarting due to `OOMKilled`
- Slow or stressful manual debugging using commands like:

```bash
kubectl get pods -A
kubectl describe <pod> -n <namespace>
kubectl logs <pod> -n <namespace>
```

Resulting actions (patching deployments, fixing images, updating limits) are reactive, manual, and error-prone.

---

# 🤖 Solution: Autonomous SRE Agent

The **AI-Native SRE Agent** solves this by:

- Automatically auditing all namespaces
- Identifying failed or unhealthy workloads
- Investigating pods using logs + describe
- Performing root-cause analysis
- Generating a detailed incident report
- Proposing safe remediation steps
- Waiting for explicit user approval
- Applying the fix declaratively
- Verifying system recovery

Everything is driven by a declarative `SKILL.md` file.

---

# 🧠 How It Works

The agent follows a structured **5-phase lifecycle**:

---

## 1️⃣ Audit Phase

Scans all namespaces for pods not in `Running` state.

---

## 2️⃣ Investigation Phase

Runs:

```bash
kubectl describe <pod> -n <namespace>
kubectl logs <pod> -n <namespace> --tail=50
```

Extracts:

- Exit codes (`137 → OOMKilled`, `1 → App Error`)
- Event messages
- Image pull failures

---

## 3️⃣ Reporting Phase

Generates a Markdown report:

```
k8s_incident_report.md
```

Includes:

- Namespace
- Pod Name
- Root Cause
- Logs Summary
- Proposed Fix
- Impact Explanation

---

## 4️⃣ Remediation Phase

- Proposes fix and explains impact
- Waits for explicit user approval before applying changes

---

## 5️⃣ Verification Phase

After fix:

- Waits 10 seconds
- Performs final health check
- Confirms pod is `Running`

---

# 📄 Example Incident Report

See:

```
k8s_incident_report.md
```

---

# 🏗 Safety & Governance

- Human-in-the-loop approval required
- No destructive actions without confirmation
- Transparent incident documentation
- Post-remediation validation

---

# 🏗 Architecture

```
Observe → Analyze → Report → Confirm → Fix → Verify
```

Powered by:

- Kubernetes CLI
- Accomplish AI
- Declarative Skill-based Execution

---

# 🎥 Demo

Hackathon Demo:

https://www.youtube.com/watch?v=WB9y_1Q3_zU