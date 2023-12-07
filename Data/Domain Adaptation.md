cf[^0]
### Domain-shift
- 학습 데이터(**source**)와 테스트 데이터(**target**)의 ==distribution의 차이==를 의미
- 학습 데이터와 테스트 데이터의 Domain-shift가 일어나면 학습 데이터에서는 잘 동작하지만 테스트 데이터에서는 잘 동작하지 않는 문제가 발생

### Traditional Domain Adaptation
#### Metric Learning
- 거리 기반으로 Domain adaptation 진행
- Source domain과 Target domain이 같은 클래스면 가깝게, 다른 클래스면 멀게 만듬
- 직관적이고 확실하지만 이를 위해서는 Source와 Target 모두 label이 필요

#### Subspace Representation
- Source와 Target은 다른 subspace에 놓여져 있고 이 둘의 subspace를 alignment하면 문제를 해결할 수 있다고 생각
- Source subspace와 Target subspace 사이의 가능한 subspace를 sampling 혹은 Integration하여 Alignment하는 방식

#### Matching Distribution
- Domain Shift로 인한 Distribution 차이를 직접적으로 해결하는데 초





[^0]: https://lhw0772.medium.com/study-da-domain-adaptation-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-%EA%B8%B0%EB%B3%B8%ED%8E%B8-4af4ab63f871
	- 