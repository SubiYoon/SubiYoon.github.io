---
tags:
  - FrameWork
  - Kuberntes
  - Issue
---
### 환경
* k8s version 1.30 -> k8s version 1.28로 다운그레이드한 상황
* ubuntu 22.04
* calico
* cri-o

### 문제
설치한 쿠버네트스를 다운그레이드하는 과정에서 `kubeadm init`을 한후 다음 명령어 실행시 `coredns`의 상태가 `containerCreating`에서 `Running`으로 변경되지 않는 문제 발생

### 원인 분석
```bash
# 시스템로그에서 error만 검색해서 찾기
journalctl -b | grep error

# 특정 pod의 log정보를 가져오기
kubectl -n kube-system describe po coredns-xxxx
```

```bash
can't find the container with id ~~~
```

대충 컨테이너를 찾을 수 없다는 이야기...

\[참고]
> [https://www.bytefusion.de/2021/01/08/openshift-okd-causes-image-layer-not-known-problems/](https://www.bytefusion.de/2021/01/08/openshift-okd-causes-image-layer-not-known-problems/)

cri-o 이미지가 손상되어있는 걸로 확인 된다.
다운그레이드하는 과정에서 기존의 정보를 삭제하기 위해 `kubeadm reset`과정에서 손상되었으려나?? 라는 추측을 해본다.

### 해결 방법
이미지를 새로 삭제하고 `kubelet`과 `cri-o`를 재시작해준다.

```bash
sudo systemctl stop kubelet.service
sudo systemctl stop crio.service
sudo rm -r /var/lib/containers/ # 삭제 불가능이라는 말이 나오면 -rf로 사용하자
sudo systemctl start crio.service
sudo systemctl start kubelet.service

# 아래 명령어가 정상 작동한다면 해결된 것이다.
sudo crictl images
```