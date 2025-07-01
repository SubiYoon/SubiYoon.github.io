---
tags:
  - OS
  - Linux
  - Cli
---
## 포트 확인
포트를 모를경우 탐색
```bash
netstat -tnlp
```

포트를 알고 있고 PID를 확인하려는 경우

```bash
netstat -tnlp | grep ${PORT_NUM}
```

## 포트 번호로 죽이기
```bash
fuser -k ${PORT_NUM}/tcp
```

## PID(Process Id)로 죽이기
```bash
kill -9 46009
```