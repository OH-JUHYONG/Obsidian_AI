### [SSH](https://velog.io/@717lumos/Git-GitHub-%EC%82%AC%EC%9A%A9%EB%B2%952-SSH-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D%EA%B3%BC-Git-Clone-Private-%EC%A0%80%EC%9E%A5%EC%86%8C ) 

- Secure Shell[^0]
- Private key와 Public key 한 쌍을 이용해 컴퓨터 인증
![[Pasted image 20231020002223.png]]
	![[Pasted image 20231020002249.png]]
	- 외부에서 접속시 따로 포트 설정이 필요


### [Virtual Environment](https://teddylee777.github.io/python/anaconda-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95-%ED%8C%81-%EA%B0%95%EC%A2%8C/)

- SSH를 통해 원격 접속을 하게되면 해당 GPU의 계정을 생성
- GPU에는 공통으로 적용되는 각 library가 있음
	- 각 계정에서 모델을 돌릴때 모델에 맞는 library가 있는데 매번 실행할때마다 해당 모델에 맞는 library version에 맞게 설정하기도 번거롭고 사용하는 계정은 root 권한이 없기 때문에 version 변경이 불가 
	- 해당 문제를 해결하기 위해 **Anaconda Virtual Environment**[^1] 사용
		1.  [Linux Anaconda version](https://www.anaconda.com/download#downloads) 설치  - 64-Bit(x86) Installer
		2. SSH에 접속 후 해당 계정 경로에다가 설치 파일 옮김  
		3. 터미널 실행 후 *bash Anaconda3-2023.09-0-Linux-x86_64.sh*
		4. 설치 후 *conda* 입력 후 제대로 설치돼었는지 확인
		5. conda를 찾을 수 없다고 뜨면 *export PATH="/home/사용자이름/anaconda3/bin:$PATH"*  입력

##### 가상환경 생성, 제거 및 활성화
1.  _conda create -n (가상환경이름) (설치하려는 패키지 버전 나열)
2. _conda env remove -n (가상환경이름)_
3.  _source activate (가상환경이름)_  
	- Interpreter를 해당 가상환경으로 바꿔줌

##### 가상환경 목록 출력 및 설치된 패키지 확인
- _conda env list_
- _conda list_


### [GPU](https://velog.io/@whattsup_kim/gpu%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0)

- _nvidia-smi_ 를 통해 GPU 환경 확인
- ==GPU 환경에 맞는 패키지 버전 설치가 제일 중요==
##### CUDA
- **Computer Unified Device Architecture**
- GPU 컴퓨팅에서 **컴파일러 역할** 수행

##### cuDNN
- cuda Deep Neural network Library
- Deep Neural network를 위한 GPU 가속화 라이브러리 기초 요소로 Convolution, pooling, Normarization, Activation과 같은 것들을 빠르게 실행할 수 있도록 하는 라이브러리



### [tmux](https://velog.io/@ur-luella/tmux-%EC%82%AC%EC%9A%A9%EB%B2%95)
- 새로운 세션 생성
	- _tmux new -s (session name)_ 
-  세션 다시 시작, 불러오기
	- _tmux attach -t (session name)














[^0]: SSH
	- https://velog.io/@717lumos/Git-GitHub-%EC%82%AC%EC%9A%A9%EB%B2%952-SSH-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D%EA%B3%BC-Git-Clone-Private-%EC%A0%80%EC%9E%A5%EC%86%8C 


