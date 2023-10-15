---
tags:
  - MTU
  - MSS
---
#MTU #MSS

# MSS(Maximum Sagment Size)
- 이론상 Sagment의 최대 크기는 68Kb
-  TCP 기반의 정보로, TCP데이터 길이만을 의미
- MSS는 TCP의 전송단위인 Sagment의 최대 사이즈를 결정하게 된다.
- MSS는 TCP 연결을 위한 3-way handshake 과정 중에 결정 된다.
	- TCP연결을 위한 초기 Sagment를 전송 시에 옵션 값에 MSS를 같이 보내 이를 결정한다.

# MTU(Maximum Transmission Unit)
- IP기반의 정보로 IP헤더, TCP헤더, TCP데이터를 모두 포함하는 길이
- 네트워크의 하드웨어에서 설정한 데이터 전송 제한의 최대 크기이다. 즉, 매체에 따라 값이 다르다.

# MSS와 MTU의 관계
- MSS는 MTU에 의해 값이 결정되게 되는데, 통상 MTU는 MSS보다 사이즈가 커야 한다.
- TCP 연결 성능을 향상시키려면 패킷 크기를 `MTU` 미만으로 유지해야 한다. 
	- 패킷 손실률은 패킷 크기에 따라 다르기 때문입니다. 패킷 크기가 클수록 패킷 손실 가능성이 높습니다.   
- `MTU`는 1500바이트(이더넷)와 576바이트(전화 접속) 범위여야 한다.