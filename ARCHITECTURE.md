                        ┌──────────────────────────────┐
                        │        Kubernetes Cluster     │
                        │  (All Namespaces & Workloads) │
                        └──────────────┬───────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────────┐
                        │        Audit Phase            │
                        │  kubectl get pods -A         │
                        │  Identify non-Running pods   │
                        └──────────────┬───────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────────┐
                        │     Investigation Phase       │
                        │  kubectl describe pod         │
                        │  kubectl logs --tail=50       │
                        │  Extract exit codes & events  │
                        └──────────────┬───────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────────┐
                        │   Accomplish AI Reasoning     │
                        │  Root Cause Analysis          │
                        │  Fix Recommendation           │
                        └──────────────┬───────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────────┐
                        │    Reporting Phase            │
                        │  Generate                     │
                        │  SRE_Incident_Report.md       │
                        └──────────────┬───────────────┘
                                       │
                                       ▼
                        ┌──────────────────────────────┐
                        │  Human-in-the-Loop Approval   │
                        │  "Do you want to apply fix?"  │
                        └──────────────┬───────────────┘
                                       │
                          Yes         │         No
                           │          │
                           ▼          ▼
                ┌────────────────┐   End Process
                │ Remediation    │
                │ kubectl patch  │
                │ set image      │
                └────────┬───────┘
                         │
                         ▼
                ┌────────────────┐
                │ Verification   │
                │ Wait 10 sec    │
                │ Final Health   │
                │ Check          │
                └────────────────┘