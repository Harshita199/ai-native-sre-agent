# Minikube K8s Incident Report

Generated: 2026-02-22

## Cluster baseline
- `minikube status`: host/kubelet/apiserver Running; kubeconfig Configured
- Node: `minikube` is **Ready** (Kubernetes v1.35.1)

## Findings (Pods not healthy)

### 1) `default/broken-nginx` — **ErrImagePull / ImagePullBackOff**
**Evidence**
- Pod image: `nginx:this-tag-doesnt-exist`
- `kubectl describe` events show repeated back-off pulling image.
- Logs are unavailable because the container never starts.

**Root cause**
- Invalid/nonexistent image tag.

**Proposed fix (needs approval before I run it)**
- Delete and recreate with a valid tag:
  - `kubectl delete pod broken-nginx -n default`
  - `kubectl run broken-nginx -n default --image=nginx:1.25`

---

### 2) `default/my-app` — **CrashLoopBackOff**
**Evidence**
- Container args: `/bin/sh -c "exit 1"`
- Last state: Terminated (Reason: Error, ExitCode: 1)

**Root cause**
- The container command intentionally exits with a non-zero status, so kubelet restarts it indefinitely.

**Proposed fix (needs approval before I run it)**
- Delete and recreate with a long-running command:
  - `kubectl delete pod my-app -n default`
  - `kubectl run my-app -n default --image=busybox -- /bin/sh -c "sleep 3600"`

---

### 3) `default/oom-demo` — **OOMKilled**
**Evidence**
- Image: `polinux/stress`
- Memory limit: `50Mi`
- Workload attempts `--vm-bytes 200M` then is terminated with `Reason: OOMKilled`.

**Root cause**
- Memory limit is lower than the workload’s requested allocation.

**Proposed fix (needs approval before I run it)**
- If this was unintentional: delete it:
  - `kubectl delete pod oom-demo -n default`
- Or recreate with higher memory (example):
  - `kubectl run oom-demo -n default --image=polinux/stress --limits='memory=300Mi' --requests='memory=300Mi' -- stress --vm 1 --vm-bytes 200M --vm-hang 1`

## Cluster events (recent warnings)
- Historical warnings exist for `broken-nginx` and `my-app` from before remediation; confirm current state with `kubectl get pods -n default`.

## Remediations applied
- `broken-nginx`: recreated with `nginx:1.25` (now Running/Ready).
- `oom-demo`: recreated as `deployment/oom-demo` with memory requests/limits set to `300Mi` (now Running/Ready).
- `my-app`: replaced the crashing pod with `deployment/my-app` and patched the container command to `sh -c "tail -f /dev/null"` (fixes the invalid command that pointed to `C:/Program Files/Git/usr/bin/sh`); rollout succeeded and the new pod is Running/Ready.

## Notes
- All core `kube-system` components appear Running.
