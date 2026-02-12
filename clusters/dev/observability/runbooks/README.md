# Ops Runbooks (Dev)

## KubeNodeNotReady
**What it means:** A node is NotReady for >5m (cluster may be degraded).

**Check**
- `kubectl get nodes -o wide`
- `kubectl describe node <node>`
- `kubectl -n kube-system get pods -o wide`

**Common causes**
- Disk full
- Network/iptables issues
- kubelet/containerd problems

**Fix**
- Check disk: `df -h`
- Restart k3s: `sudo systemctl restart k3s`
- Check logs: `sudo journalctl -u k3s -n 200 --no-pager`

---

## KubePodCrashLooping
**Check**
- `kubectl -n <ns> get pod <pod> -o wide`
- `kubectl -n <ns> describe pod <pod>`
- `kubectl -n <ns> logs <pod> --all-containers --tail=200`

**Fix**
- Roll back / fix env vars / config / image
- If it’s your app: bump resources, fix config, redeploy

---

## KubeDeploymentReplicasMismatch
**Check**
- `kubectl -n <ns> get deploy <deploy>`
- `kubectl -n <ns> describe deploy <deploy>`
- `kubectl -n <ns> get rs,pod -l app=<label> -o wide`

---

## PrometheusTargetMissing
**Check**
- Prometheus UI → Status → Targets
- Confirm service endpoints:
  - `kubectl -n <ns> get svc,endpoints`

**Fix**
- If ServiceMonitor/PodMonitor: ensure labels match
- If app down: restart deployment/pod
