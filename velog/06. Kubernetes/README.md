---
tags:
  - FrameWork
  - Kubernetes
  - Command
---
# Kubernetes
## Kubernetes Join token 재생성
```bash
kubeadm token create --print-join-command
```

## kubenetes에서 사용가능한 오브젝트 리스트
```bash
kubectl api-resources
```

## 모든 Node 보기
```bash
kubectl get nodes -A
```

## 모든 Pod 보기
```bash
kubectl get Pods -A
```

## 모든 Pod에 대한 간략한 정보 보기
```bash
kubectl get all -o wide
```

## .yaml 파일 적용시키기
```bash
kubectl apply -f ${file.yaml}

# example: calico 적용
kubectl apply -f calico.yaml
```

## 리소스의 속성을 직접 변경하기
```bash
kubectl edit ${object_type} ${resource_name}

# example: rook's cluster edit
kubectl edit cephcluster -n rook-ceph rook-ceph
```

## Pod 삭제하기
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

# 설정파일로 삭제하기
kubectl delete -f ${file.yaml}

# 모든 리소스 삭제
kubectl delete deployment,pod,rs --all
```

## Ready,SchedulingDisabled 상태 수정
```bash
# 노드 상태 확인 
$ kubectl get nodes
NAME                       STATUS                     ROLES                  AGE    VERSION
k8s-master-1               Ready                      control-plane,master   189d   v1.21.11
k8s-master-2               Ready                      control-plane,master   189d   v1.21.11
k8s-master-3               Ready                      control-plane,master   189d   v1.21.11
k8s-worker-1               Ready                      <none>                 189d   v1.21.11
k8s-worker-2               Ready                      <none>                 189d   v1.21.11
k8s-worker-3               Ready,SchedulingDisabled   <none>                 189d   v1.21.11
k8s-worker-4               Ready                      <none>                 181d   v1.21.11
            
$ # 노드 복원
kubectl uncordon [노드명]
```