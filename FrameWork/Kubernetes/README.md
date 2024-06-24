---
tags:
  - FrameWork
  - Kubernetes
  - Command
---
### Kubernetes Join token 재생성
```bash
kubeadm token create --print-join-command
```

### 모든 Node 보기
```bash
kubectl get nodes -A
```

### 모든 Pod 보기
```bash
kubectl get Pods -A
```

### 모든 Pod에 대한 간략한 정보 보기
```bash
kubectl get all -o wide
```

### .yaml 파일 적용시키기
```bash
kubectl apply -f ${file.yaml}
```

### Pod 삭제하기
```bash
# 일반 삭제
kubectl delete pod ${pod-name}

# 강제 삭제
kubectl delete pod ${pod-name} --grace-period=0 --force

# terminating 상태에서 멈춤 시 강제 삭제
kubectl patch pvc ${pod-name} -p '{"metadata":{"finalizers":null}}'

# 조건으로 검색된 복수의 항목 삭제
kubectl get pods -A |grep -i ${search-name} | awk '{print $2}' | xargs kubectl delete pod

# 모든 항목이 안될 때는 그냥 kubelet을 재시동하자
sudo systemctl restart kubelet.service
```