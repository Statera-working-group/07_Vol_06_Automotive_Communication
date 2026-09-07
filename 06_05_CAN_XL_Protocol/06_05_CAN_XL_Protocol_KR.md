**Volume 06 Automotive Communication**

# Chapter 5. CAN XL Protocol

## 5.1 CAN XL Overview 10Mbps

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

CAN XL은 Controller Area Network(CAN) 계열의 차세대 발전 기술로, Classical CAN과 CAN FD를 널리 사용하게 만든 결정적 중재(Deterministic Arbitration)와 고장 처리(Fault Handling) 원칙을 유지하면서 훨씬 높은 대역폭(Bandwidth)을 제공하도록 개발되었다. CAN XL은 약 10 Mbps 이상의 통신 속도를 목표로 하며, 모든 기능을 즉시 Ethernet으로 이전하지 않고도 더 큰 소프트웨어, 진단 데이터, 제어 정보, 서비스 지향 데이터(Service-Oriented Data)를 효율적으로 전송할 수 있도록 설계되었다.

CAN XL의 아키텍처는 CAN FD와 Automotive Ethernet 사이의 성능 격차를 메우기 위해 설계되었다. CAN FD는 페이로드 크기와 데이터 구간 속도를 크게 향상시켰지만, 현대의 차량, 로봇, 산업 장비는 소프트웨어 이미지, 상세 진단 정보, 고해상도 상태 데이터, 보안 정보, 게이트웨이 트래픽 등을 지속적으로 교환하고 있다. CAN XL은 이러한 요구를 충족하기 위해 훨씬 큰 페이로드, 더욱 유연한 프로토콜 필드, 향상된 물리 계층 성능을 제공한다.

CAN XL은 우선순위 기반 버스 접근(Priority-Based Bus Access) 방식을 그대로 유지한다. 따라서 더 작은 식별자(Identifier)를 가진 메시지는 우선적으로 중재에서 승리할 수 있으며, 낮은 우선순위 메시지를 손상시키지 않는다. 이를 통해 중요한 제어 메시지의 예측 가능한 지연 시간(Latency)을 유지할 수 있다. 스위칭과 트래픽 관리(Traffic Shaping)가 필요한 Ethernet과 달리 CAN XL은 CAN 고유의 분산 중재(Distributed Arbitration) 구조를 그대로 계승한다.

CAN XL 프레임은 중재 구간(Arbitration Phase)과 고속 데이터 구간(High-Speed Data Phase)으로 구성된다. 중재는 일반적으로 안정적인 명목 비트 속도(Nominal Bit Rate)에서 수행되어 모든 노드가 신뢰성 있게 참여할 수 있도록 한다. 이후 버스 사용권을 획득한 송신 노드는 더 높은 데이터 속도로 전환한다. 이러한 원리는 CAN FD의 비트 전환(Bit Rate Switching, BRS)과 유사하지만, CAN XL은 더욱 발전된 프레임 구조와 물리 계층 동작 방식을 채택하고 있다.

일반적으로 언급되는 약 10 Mbps의 통신 속도는 하나의 고정된 규격이 아니라 현실적인 목표 성능을 의미한다. 실제 운용 속도는 케이블 길이, 네트워크 토폴로지(Network Topology), 트랜시버(Transceiver) 특성, 발진기(Oscillator) 정확도, 커넥터 품질, 전자파 환경 등에 의해 결정된다. 짧고 잘 설계된 네트워크는 더 높은 성능을 달성할 수 있지만, 긴 배선이나 전기적으로 열악한 환경에서는 안정적인 통신을 위해 데이터 속도를 낮추어야 할 수도 있다.

CAN XL의 가장 중요한 개선 사항 가운데 하나는 크게 증가한 페이로드 용량이다. Classical CAN은 최대 8바이트(Byte), CAN FD는 최대 64바이트를 지원하는 반면 CAN XL은 약 2 KB 수준의 페이로드를 하나의 프레임에서 지원한다. 이를 통해 소프트웨어, 진단 정보, 설정 데이터(Configuration Data), 구조화된 메시지를 수많은 작은 프레임으로 나누지 않고 훨씬 적은 프로토콜 오버헤드(Protocol Overhead)로 전송할 수 있다.

이처럼 큰 페이로드는 네트워크 애플리케이션 설계 방식을 변화시킨다. 기존에는 전송 계층(Transport Layer)을 통해 수십 또는 수백 개의 프레임으로 분할해야 했던 데이터가 이제는 하나 또는 몇 개의 CAN XL 프레임으로 전송될 수 있다. 그 결과 프로세서 부하, 응답 처리(Acknowledgement Handling), 버퍼 관리(Buffer Management), 재전송(Retransmission) 복잡성이 감소하며, 펌웨어 배포(Firmware Distribution), 캘리브레이션(Calibration), 이벤트 로그(Event Log), 서비스 지향 통신(Service-Oriented Communication)의 효율이 크게 향상된다.

그러나 큰 페이로드를 항상 최대 크기로 사용하는 것은 바람직하지 않다. 우선순위가 낮은 매우 긴 프레임은 상당한 시간 동안 버스를 점유하여 이후의 메시지를 지연시킬 수 있다. 따라서 실시간 제어 데이터는 적절한 크기의 프레임과 높은 우선순위 식별자를 사용해야 한다. 대용량 페이로드는 소프트웨어 업데이트, 진단, 대량 데이터 전송(Bulk Transfer), 집계 데이터(Aggregated Data)와 같이 긴 전송 간격을 허용하는 작업에 가장 효과적이다.

CAN XL은 상위 계층(Higher Layer)의 데이터를 더욱 효율적으로 식별하고 라우팅(Routing)할 수 있도록 새로운 프로토콜 필드를 제공한다. 이러한 필드는 게이트웨이와 수신 애플리케이션이 단순히 응용 프로그램 규칙에 의존하지 않고도 다양한 페이로드 유형을 구분할 수 있도록 지원한다. 그 결과 기존의 신호 기반 통신(Signal-Based Communication)뿐 아니라 데이터 구조, 명령, 이벤트, 네트워크 서비스를 동시에 지원하는 서비스 지향 아키텍처(Service-Oriented Architecture)도 구현할 수 있다.

CAN XL은 이전 세대 CAN보다 다양한 상위 계층 기술을 더욱 자연스럽게 수용하도록 설계되었다. Ethernet 스타일의 페이로드(Ethernet-Style Payload), 인터넷 프로토콜(Internet Protocol, IP) 트래픽, 서비스 지향 데이터(Service-Oriented Data)는 CAN XL의 적응 메커니즘(Adaptation Mechanism)을 통해 전달될 수 있다. 이것이 CAN XL을 Ethernet과 동일하게 만드는 것은 아니지만, 제한된 임베디드 제어기와 고성능 컴퓨팅 시스템 사이의 통신 모델을 더욱 일관성 있게 설계할 수 있도록 한다.

CAN XL은 존 기반(Zonal) 및 중앙 집중형(Centralized) 전자 아키텍처에서도 CAN 계열 네트워크의 역할을 더욱 강화한다. 현대 시스템은 소수의 고성능 중앙 컴퓨터와 다수의 입출력 장치, 액추에이터, 센서를 분산 배치하는 구조를 사용한다. CAN XL은 이러한 존 장치(Zonal Device)를 CAN FD보다 높은 대역폭으로 연결하면서도 저렴한 공유 배선과 결정적인 통신 특성을 유지할 수 있다.

차량에서는 CAN XL이 도메인 제어기(Domain Controller), 존 게이트웨이(Zonal Gateway), 차체 전장(Body Electronics), 파워트레인(Powertrain), 에너지 시스템(Energy System), 섀시 기능, 지능형 센서를 연결하는 데 사용될 수 있다. Automotive Ethernet은 카메라, 레이더, 인포테인먼트(Infotainment), 대용량 컴퓨팅을 담당하고, CAN XL은 CAN FD와 Ethernet 사이에서 중간 수준의 데이터 요구사항을 효과적으로 처리하는 역할을 수행한다.

자율이동로봇(Autonomous Mobile Robot, AMR)도 이러한 중간 계층 네트워크의 장점을 크게 활용할 수 있다. 복잡한 AMR에는 다수의 구동 제어기, 조향 모듈, 안전 제어기, 배터리 시스템, 지능형 전력 분배 장치, 매니퓰레이터(Manipulator), 다양한 페이로드 인터페이스가 포함된다. CAN XL은 동작 및 안전 관련 메시지의 우선순위를 유지하면서도 더욱 상세한 액추에이터 피드백, 에너지 정보, 설정 데이터, 로그, 소프트웨어 업데이트를 전달할 수 있다.

중량물 운반 AMR(Heavy-Duty AMR)은 특히 높은 통신 요구사항을 가진다. 여러 개의 구동 모터, 사륜 조향(Four-Wheel Steering), 제동 시스템, 인양 장치(Lifting Mechanism), 대용량 배터리, 임무 장비(Mission Equipment)가 동시에 동작하면서 많은 상태 정보를 생성한다. CAN XL은 모든 임베디드 장치가 Ethernet을 사용할 필요 없이 이러한 상태 정보를 더욱 효율적으로 교환할 수 있도록 한다.

약 10 Mbps 수준의 통신 속도를 구현하기 위해서는 물리 계층(Physical Layer)이 매우 중요하다. 고속 통신을 위해서는 낮은 전파 지연(Propagation Delay), 정확한 에지 제어(Edge Control), 우수한 지연 대칭성(Delay Symmetry), 뛰어난 EMC 성능을 갖춘 트랜시버가 필요하다. 또한 하네스(Harness)는 일정한 차동 임피던스(Differential Impedance), 짧은 분기선(Stub), 안정적인 커넥터 전이, 적절한 종단 저항(Termination)을 유지해야 한다. CAN FD에서 정상적으로 동작하는 네트워크가 반드시 CAN XL에서도 동일한 속도로 동작하는 것은 아니다.

CAN XL 물리 계층은 중재 이후의 고속 신호 전송을 개선하기 위한 새로운 동작 방식을 도입한다. 중재 구간에서는 기존의 우성(Dominant) 및 열성(Recessive) 신호 방식을 유지하지만, 고속 데이터 구간에서는 향상된 트랜시버 동작을 통해 더욱 강력하고 안정적인 신호를 생성할 수 있다. 이를 통해 기존 오픈 드레인(Open-Drain) 방식이 가진 한계를 줄이고 더욱 우수한 타이밍 특성을 제공한다.

물리 계층은 호환성과 성능 요구사항에 따라 다양한 운용 모드를 사용할 수 있다. 일부 구현은 기존 CAN FD 배선과의 호환성을 우선하고, 다른 구현은 CAN XL 전용 트랜시버 기능을 활용하여 더 높은 속도를 달성할 수 있다. 선택한 운용 방식은 호환성, 네트워크 길이, 통신 속도, EMC, 그리고 다양한 CAN 세대의 혼합 운용 가능성에 직접적인 영향을 준다.

하위 호환성(Backward Compatibility)은 신중하게 이해해야 한다. CAN XL은 CAN 계열의 설계 철학을 유지하지만, Classical CAN이나 CAN FD 컨트롤러는 CAN XL 프레임을 자동으로 해석할 수 없다. 따라서 혼합 세대 네트워크는 적절한 컨트롤러, 선택적 운용 모드, 별도의 채널 또는 게이트웨이를 필요로 한다. 즉, CAN XL로의 전환은 단순한 트랜시버 교체가 아니라 전체 시스템 아키텍처를 고려한 설계가 필요하다.

게이트웨이(Gateway)는 Classical CAN, CAN FD, CAN XL, Ethernet 네트워크를 분리하면서 필요한 정보만 상호 전달할 수 있다. 이를 통해 기존 시스템을 그대로 유지하면서 새로운 고대역폭 기능을 단계적으로 도입할 수 있다. 게이트웨이는 식별자 변환(Identifier Translation), 페이로드 변환(Payload Conversion), 라우팅, 버퍼링(Buffering), 보안 검사(Security Checking), 속도 변환(Rate Adaptation)을 수행하며, 저속 네트워크가 고속 트래픽에 의해 영향을 받지 않도록 해야 한다.

데이터 속도가 10 Mbps에 가까워질수록 비트 타이밍(Bit Timing)은 더욱 까다로워진다. 발진기 허용 오차(Oscillator Tolerance), 동기화 점프 폭(Synchronization Jump Width), 샘플링 지점(Sampling Point), 트랜시버 지연, 케이블 전파 시간을 모두 최악 조건(Worst Case) 기준으로 계산해야 한다. 500 kbps에서는 문제가 없던 작은 타이밍 오차도 고속에서는 치명적인 통신 오류를 유발할 수 있다.

전파 지연은 컨트롤러 출력, 트랜시버 드라이버, 케이블, 커넥터, 수신 트랜시버, 컨트롤러 입력까지 모두 포함한다. 데이터 속도가 높아질수록 동일한 나노초(nanosecond) 단위의 지연도 전체 비트 시간에서 차지하는 비율이 커진다. 따라서 긴 하네스와 분산된 노드가 존재하는 경우에는 하나의 긴 CAN XL 버스 대신 여러 개의 지역(Local) CAN XL 세그먼트(Segment)를 게이트웨이로 연결하는 것이 더욱 효율적일 수 있다.

네트워크 토폴로지는 가능한 한 선형 백본(Linear Backbone) 구조를 유지해야 한다. 스타 연결(Star Connection)이나 긴 분기선은 신호 반사를 발생시켜 이후 신호와 중첩될 수 있다. 커넥터 체인, 사용하지 않는 하네스 연장, 서비스 어댑터(Service Adapter), 선택 사양 모듈도 임피던스를 변화시킨다. 데이터 속도가 높을수록 전기 아키텍처와 하네스 설계 규칙을 더욱 엄격하게 적용해야 한다.

종단 저항은 선택한 물리 계층 방식과 트랜시버 규격에 맞추어 설계되어야 한다. 기존 고속 CAN은 일반적으로 양 끝에 120Ω 종단 저항을 사용하지만, CAN XL은 물리 계층 방식에 따라 추가적인 요구사항이 존재할 수 있다. 따라서 Classical CAN용으로 최적화된 종단 회로가 CAN XL에서도 최적이라고 가정해서는 안 되며, 시뮬레이션과 실제 측정을 통해 검증해야 한다.

전자파 적합성(Electromagnetic Compatibility, EMC)은 중요한 설계 과제이다. 빠른 에지는 더 많은 고주파 성분을 포함하므로 전도 및 방사 노이즈를 증가시킬 수 있다. 연선 품질, 차폐(Shielding), 공통 모드 초크(Common-Mode Choke), 분할 종단(Split Termination), 접지, 커넥터 위치, PCB 레이아웃을 함께 고려해야 한다. 에지 형상 제어(Edge Shaping)는 노이즈를 줄일 수 있지만 지나친 필터링은 타이밍 여유를 감소시키고 고속 신호를 왜곡할 수 있다.

CAN XL 역시 차동 통신(Differential Communication)을 사용하므로 공통 모드 전압(Common-Mode Voltage)과 접지 오프셋(Ground Offset)을 고려해야 한다. 차량과 로봇은 대전류 모터, 배터리, 컨택터, 인버터, DC-DC 컨버터를 포함하므로 접지 전위차가 쉽게 발생한다. 트랜시버는 충분한 공통 모드 허용 범위를 가져야 하며, 시스템 접지는 구동 전류와 충전 전류가 통신 접지를 통해 흐르지 않도록 설계해야 한다. 필요하면 전원 영역 간 절연(Isolation)도 적용할 수 있다.

보호 부품(Protection Component)은 전기적 보호와 고속 성능을 동시에 만족해야 한다. TVS 다이오드(Transient Voltage Suppressor), 공통 모드 초크, 필터, 직렬 소자는 정전기 방전(Electrostatic Discharge), 서지(Surge), EMI로부터 시스템을 보호한다. 그러나 이러한 부품의 기생 정전용량(Parasitic Capacitance)과 인덕턴스(Inductance)는 CAN XL 파형을 저하시킬 수 있으므로 실제 부하와 대역폭을 고려하여 선정해야 한다.

CAN XL은 소프트웨어 업데이트 성능을 크게 향상시킬 것으로 기대된다. 임베디드 제어기는 대용량 펌웨어, 보안 패치(Security Patch), 캘리브레이션 데이터셋(Calibration Dataset), 설정 파일(Configuration File)을 지속적으로 관리해야 한다. Classical CAN에서는 매우 긴 시간이 필요하고 CAN FD도 대규모 시스템에서는 한계가 있을 수 있다. CAN XL은 더 높은 속도와 큰 프레임을 이용하여 기존 CAN 기반 인프라를 유지하면서 업데이트 시간을 크게 줄일 수 있다.

진단 기능도 확장된 페이로드의 이점을 얻는다. 하나의 제어기는 상세한 고장 스냅샷(Fault Snapshot), 동작 이력(Operating History), 내부 측정값, 소프트웨어 버전, 카운터, 추적 기록(Trace Record)을 적은 수의 프레임으로 보고할 수 있다. 서비스 장비는 더 적은 트랜잭션으로 더 많은 정보를 획득할 수 있으며, 이는 문제 해결, 예지 정비(Predictive Maintenance), 원격 지원(Remote Support)을 더욱 효율적으로 만든다.

프레임 크기와 네트워크 연결성이 증가함에 따라 보안(Security)은 더욱 중요해진다. 기존 CAN 중재 방식은 암호화(Encryption), 인증(Authentication), 접근 제어(Access Control)를 자체적으로 제공하지 않는다. 따라서 CAN XL 시스템은 애플리케이션 계층 인증, 보안 진단(Secure Diagnostics), 최신성 카운터(Freshness Counter), 보호된 펌웨어 업데이트, 보안 부팅(Secure Boot), 게이트웨이 기반 필터링을 함께 사용해야 한다. 큰 페이로드는 보안 메타데이터(Security Metadata)를 포함하기 쉽지만, 보안은 전체 시스템 아키텍처 차원에서 구현되어야 한다.

대역폭이 증가하더라도 네트워크 부하(Network Load) 분석은 여전히 중요하다. 엔지니어는 프레임 전송 시간, 중재 지연, 오류 처리, 재전송, 페이로드 크기, 순간적인 버스트 트래픽(Burst Traffic)을 모두 고려해야 한다. 대용량 소프트웨어나 진단 전송이 실시간 제어를 방해해서는 안 된다. 이를 위해 트래픽 관리(Traffic Shaping), 메시지 우선순위, 업데이트 시간 창(Update Window), 대역폭 예약(Bandwidth Reservation), 게이트웨이 속도 제어가 필요할 수 있다.

CAN XL은 통신 클래스를 명확하게 구분하면 실시간 제어와 대용량 데이터 전송을 동시에 지원할 수 있다. 동작 명령, 제동 정보, 액추에이터 한계값, 안전 상태는 높은 우선순위의 짧은 프레임을 사용해야 한다. 모니터링 데이터는 중간 우선순위의 주기적 프레임을 사용하고, 로그, 캘리브레이션 블록, 업데이트는 낮은 우선순위를 사용할 수 있다. 이러한 구조적인 스케줄링은 대용량 데이터와 실시간 제어가 동일한 네트워크를 공유하도록 한다.

오류 검출(Error Detection)과 오류 제한(Error Confinement)은 여전히 CAN 계열의 가장 큰 장점이다. 컨트롤러는 송수신 비트를 감시하고, 검사 값을 확인하며, 오류 정보를 생성하고, 반복적으로 오류를 발생시키는 노드를 격리한다. CAN XL은 프로토콜 기능을 확장하면서도 이러한 기본 철학을 유지한다. 시스템 소프트웨어는 추가적으로 메시지 타임아웃(Time-out), 시퀀스 연속성(Sequence Continuity), 데이터 최신성(Data Freshness), 노드 가용성(Node Availability)을 감시해야 한다.

시험은 프로토콜, 타이밍, 전기적 특성, 환경, 상호운용성(Interoperability)을 모두 포함해야 한다. 엔지니어는 차동 전압, 공통 모드 변화, 전파 지연, 링잉, 오버슈트(Overshoot), 에지 대칭성, 샘플링 여유를 측정해야 한다. 프로토콜 분석기는 중재, 프레임 오류, 재전송, 버스 부하, 오류 복구를 감시해야 한다. 시험은 최소 및 최대 케이블 길이, 다양한 제조사의 트랜시버, 극한 온도, 전원 변화 조건에서 수행되어야 한다.

전자파 시험은 실제 운용 환경을 재현해야 한다. 모터, 인버터, 충전기, 릴레이, 무선 장비, 고전류 부하를 모두 동작시킨 상태에서 CAN XL 통신을 최대 속도로 시험해야 한다. 실험실에서는 정상적으로 동작하던 네트워크도 실제 전력 전자장치 근처에서는 실패할 수 있으므로 전체 시스템 검증이 필수적이다.

새로운 프로토콜이 초기 도입되는 시기에는 상호운용성 검증이 특히 중요하다. 서로 다른 제조사의 컨트롤러와 트랜시버는 타이밍 여유나 에지 특성이 다를 수 있다. 따라서 동일한 부품만 사용하는 시험이 아니라 최악 조건 조합(Worst-Case Combination)을 이용한 검증이 필요하다. 표준 적합성(Standard Conformance)은 중요하지만 실제 현장에서의 상호운용성 시험도 반드시 수행해야 한다.

실내형 AMR에서는 짧은 하네스와 잘 제어된 패키징 덕분에 CAN XL이 높은 성능으로 동작할 수 있다. 네트워크는 상세한 모터 피드백, 배터리 정보, 리프트 상태, 진단, 빠른 펌웨어 업데이트를 지원할 수 있다. Ethernet은 인지와 내비게이션을 담당하고, CAN XL은 분산된 전기·기계 장치를 연결하는 임베디드 제어 및 서비스 백본 역할을 수행한다.

야외형 또는 중량물 운반 로봇에서는 긴 배선, 큰 접지 전위차, 강한 전자파 환경, 많은 커넥터 때문에 실제 운용 가능한 속도가 제한될 수 있다. 그러나 최고 이론 속도를 사용하지 않더라도 CAN XL의 큰 페이로드와 향상된 프로토콜 효율성은 여전히 큰 장점을 제공한다. 최고 속도에서 불안정하게 동작하는 것보다 검증된 낮은 속도에서 안정적으로 동작하는 것이 훨씬 중요하다.

CAN XL은 CAN FD나 Ethernet을 모두 대체하는 범용 기술이 아니다. CAN FD는 중간 수준의 데이터 요구사항을 가진 제어 네트워크에 여전히 경제적이며, Ethernet은 초고대역폭 데이터와 스위치 기반 통신에 적합하다. CAN XL은 결정적 중재, 공유 버스의 단순성, 큰 페이로드, 약 10 Mbps 수준의 성능이 가장 적합한 중간 영역을 담당한다.

CAN XL의 도입은 CAN 계열의 연속적인 발전이며 완전히 새로운 아키텍처로의 전환은 아니다. CAN XL은 식별자, 중재, 분산 제어, 오류 제한과 같은 기존 CAN의 핵심 개념을 유지하면서 소프트웨어 정의 시스템(Software-Defined System)과 데이터 중심 임베디드 시스템(Data-Intensive Embedded System)에 필요한 용량을 추가한다. 따라서 CAN 경험이 풍부한 조직은 개발 위험을 줄이면서 더욱 발전된 시스템으로 자연스럽게 전환할 수 있다.

적절하게 설계된 CAN XL은 미래의 차량, 산업 자동화 시스템, 자율 로봇 플랫폼을 위한 확장 가능한 통신 백본을 제공할 수 있다. 결정적인 버스 접근, 수 KB 수준의 페이로드, 높은 데이터 속도, 게이트웨이 호환성, 향상된 프로토콜 유연성을 결합함으로써 검증된 CAN 설계 원칙을 유지하면서도 더욱 풍부한 정보를 교환할 수 있다. 그러나 약 10 Mbps 수준의 안정적인 성능을 확보하기 위해서는 엄격한 물리 계층 설계, 정확한 타이밍 분석, 체계적인 트래픽 설계, 보안, 그리고 철저한 검증이 반드시 함께 수행되어야 한다.

## 5.2 CAN XL Frame and PHY

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

CAN XL 프레임(Frame) 구조와 물리 계층(Physical Layer, PHY) 기술은 CAN FD가 등장한 이후 CAN 계열에서 이루어진 가장 중요한 기술적 발전이다. CAN FD가 주로 페이로드(Payload) 크기와 통신 속도를 향상시켰다면, CAN XL은 약 10 Mbps 수준의 통신을 지원할 수 있는 더욱 발전된 프로토콜 구조와 새로운 물리 계층 개념을 도입하였다. 이러한 발전은 소프트웨어 정의 차량(Software-Defined Vehicle), 산업 자동화 시스템, 자율이동로봇(Autonomous Mobile Robot, AMR), 분산 임베디드 컴퓨팅 플랫폼에서 증가하는 통신 요구사항을 충족시키면서도 수십 년 동안 CAN 네트워크가 유지해 온 결정적인 통신 철학을 그대로 계승하도록 설계되었다.

CAN XL 프레임은 훨씬 큰 데이터 전송을 지원하면서도 높은 통신 신뢰성을 유지하도록 설계되었다. 이전 세대와 마찬가지로 모든 통신은 어떤 노드가 버스 사용권을 획득할지를 결정하는 중재(Arbitration) 과정으로 시작된다. 중재가 완료되면 송신 컨트롤러는 고속 데이터 전송 구간으로 전환하여 Classical CAN이나 CAN FD보다 훨씬 큰 페이로드를 전송할 수 있다. 이러한 중재 구간과 고속 데이터 전송 구간의 분리는 공유 버스의 결정성을 유지하면서도 대역폭을 효율적으로 활용하도록 한다.

프레임 구조는 순차적으로 구성되는 통신 필드(Field)의 개념을 유지하면서 각 필드의 기능을 크게 확장하였다. 각 필드는 동기화(Synchronization), 식별(Identification), 제어(Control), 페이로드 관리, 무결성 검증(Integrity Verification), 프레임 종료(Frame Completion)를 담당한다. CAN XL은 단순히 CAN FD의 필드를 확장한 것이 아니라 일부 프로토콜 구조를 재구성하여 확장성(Scalability), 미래 호환성(Future Extensibility), 그리고 점점 복잡해지는 소프트웨어 아키텍처와의 적합성을 높였다. 이를 통해 기존의 신호 기반 통신과 서비스 지향 통신(Service-Oriented Communication)을 하나의 프로토콜에서 동시에 지원할 수 있다.

프레임 전송은 시작 프레임(Start of Frame, SOF)으로 시작되며, 이를 통해 모든 노드는 내부 통신 상태 머신(State Machine)을 동기화한다. 데이터 속도가 높아질수록 작은 타이밍 오차도 빠르게 누적되므로 정확한 동기화는 매우 중요하다. 모든 컨트롤러는 버스를 지속적으로 감시하면서 관측된 신호 에지(Edge)에 자신의 내부 클록을 맞춘다. 이러한 동기화 과정은 안정적인 중재와 이후의 고속 데이터 전송을 위한 기반이 된다.

중재 필드(Arbitration Field)는 CAN의 가장 큰 장점 가운데 하나인 비파괴 비트 단위 중재(Nondestructive Bitwise Arbitration)를 그대로 유지한다. 여러 컨트롤러가 동시에 송신을 시도하더라도 가장 높은 우선순위의 식별자만 계속 송신을 수행하고, 나머지 컨트롤러는 프레임을 손상시키지 않은 채 즉시 수신 모드로 전환된다. 이 방식은 네트워크 부하와 관계없이 결정적인 버스 접근을 보장한다. 따라서 중요한 실시간 제어 메시지는 전체 네트워크 부하가 증가하더라도 예측 가능한 통신 지연 시간을 유지할 수 있다.

스위치 기반 통신이 중앙 스케줄링(Centralized Scheduling)이나 QoS(Quality of Service)를 필요로 하는 것과 달리, CAN XL은 공유 버스에서 직접 우선순위를 결정한다. 모든 컨트롤러는 중재 과정에서 자신이 송신한 비트와 실제 버스의 비트를 동시에 비교한다. 자신이 열성(Recessive)을 송신했는데 버스에서 우성(Dominant)이 감지되면, 더 높은 우선순위의 노드가 존재한다는 의미이므로 즉시 송신을 중단하고 현재 프레임이 종료될 때까지 대기한다. 이러한 분산 중재 방식은 CAN 계열을 대표하는 핵심 기술 가운데 하나이다.

중재 이후의 제어 필드(Control Field)는 정상적인 수신을 위해 필요한 다양한 통신 파라미터를 정의한다. 여기에는 프레임 해석 정보, 페이로드 특성, 프로토콜 설정, 기타 송수신기가 공유해야 하는 통신 속성이 포함된다. CAN XL은 이전 세대보다 훨씬 다양한 통신 방식을 지원하므로 제어 필드는 나머지 프레임을 어떻게 처리해야 하는지를 설명하는 더 많은 정보를 포함한다.

CAN XL의 가장 큰 발전 가운데 하나는 매우 큰 페이로드를 지원한다는 점이다. CAN XL은 약 2 KB 수준의 최대 페이로드를 지원하며, 이는 Classical CAN과 CAN FD에 비해 매우 큰 증가이다. 이러한 용량 증가는 대용량 소프트웨어 이미지, 구조화된 진단 데이터, 설정 데이터베이스(Configuration Database), 캘리브레이션 파라미터(Calibration Parameter), 서비스 지향 메시지를 훨씬 적은 프로토콜 오버헤드로 전송할 수 있도록 한다.

대용량 페이로드는 네트워크 효율을 근본적으로 변화시킨다. Classical CAN에서는 대용량 진단 데이터나 펌웨어를 전송하기 위해 수백 개의 프레임이 필요했으며, CAN FD는 64바이트 페이로드를 통해 이를 크게 줄였다. CAN XL은 훨씬 큰 애플리케이션 데이터를 하나의 프레임에 담을 수 있으므로 프레임 오버헤드를 더욱 감소시킨다. 결과적으로 실제 데이터 전송 효율이 향상되고, 컨트롤러 인터럽트(Interrupt) 횟수와 프로토콜 처리 부하도 감소한다.

매우 큰 페이로드를 지원하더라도 효율적인 네트워크 설계를 위해서는 적절한 메시지 크기를 선택해야 한다. 실시간 제어 메시지는 버스 점유 시간을 줄이기 위해 여전히 짧게 유지하는 것이 바람직하다. 반대로 대용량 CAN XL 프레임은 펌웨어 배포(Firmware Distribution), 이벤트 로그(Event Logging), 엔지니어링 진단(Engineering Diagnostics), 장비 설정(Machine Configuration), 생산 시험(Production Testing), 소프트웨어 유지보수와 같은 용도에 적합하다. 실시간 제어와 대용량 데이터 전송을 분리하는 것은 여전히 중요한 설계 원칙이다.

CAN XL 프로토콜은 기존 식별자만으로는 부족했던 페이로드 식별 기능을 강화하였다. 추가적인 프로토콜 정보는 수신 소프트웨어와 게이트웨이가 다양한 통신 서비스를 더욱 효율적으로 구분할 수 있도록 한다. 이러한 유연성은 임베디드 제어기가 단순한 수치 신호뿐 아니라 구조화된 데이터 객체(Data Object), 서비스 요청(Service Request), 이벤트(Event), 애플리케이션 세션(Application Session)을 교환하는 현대적인 분산 소프트웨어 아키텍처를 지원한다.

CAN XL은 소프트웨어 정의 시스템(Software-Defined System)을 고려하여 설계되었다. 고정된 신호 정의에만 의존하지 않고 보다 동적인 통신 모델을 지원한다. 서비스 지향 아키텍처(Service-Oriented Architecture)는 애플리케이션이 서비스를 탐색하고(Discovery), 구조화된 정보를 교환하며, 표준화된 인터페이스를 통해 통신하도록 한다. CAN XL은 이러한 현대적인 소프트웨어 개발 방식을 지원하기 위한 충분한 프로토콜 유연성을 제공한다.

프레임 무결성(Frame Integrity)은 CAN XL 설계의 가장 중요한 목표 중 하나이다. 모든 프레임에는 수신기가 데이터 손상 여부를 확인할 수 있는 다양한 검증 메커니즘이 포함된다. 강력한 오류 검출(Error Detection)은 잡음, 타이밍 교란, 신호 왜곡, 전자파 간섭(Electromagnetic Interference, EMI)으로부터 통신을 보호한다. 오류가 발생하면 네트워크는 즉시 재전송을 수행하며, CAN 계열의 오류 제한(Error Confinement) 메커니즘을 통해 고장을 관리한다.

오류 검출은 중앙 제어기가 아닌 모든 노드에 분산되어 수행된다. 각 노드는 자신이 수신한 데이터를 독립적으로 검증하는 동시에 자신이 송신한 신호도 감시한다. 이러한 분산 감시는 통신 오류를 매우 빠르게 발견할 수 있으며, 특정 하드웨어의 고장이 감지되지 않은 채 남아 있는 상황을 방지한다. 이러한 높은 고장 허용성(Fault Tolerance)은 CAN 계열이 다른 산업용 네트워크와 차별화되는 중요한 특징이다.

프레임이 정상적으로 수신되면 수신 노드는 응답(Acknowledgement, ACK)을 통해 최소 하나 이상의 노드가 데이터를 성공적으로 수신했음을 송신기에 알려준다. 응답이 존재하지 않으면 통신이 실패했거나 유효한 수신 노드가 존재하지 않는다는 의미이다. 따라서 송신기는 별도의 애플리케이션 복구 절차 없이도 프로토콜 규칙에 따라 자동으로 재전송을 수행할 수 있다.

프레임 종료(End of Frame, EOF)는 통신의 종료를 정의하며 버스를 다시 유휴 상태(Idle State)로 복귀시킨다. 이후 인터프레임 간격(Interframe Space)이 지나면 모든 컨트롤러는 다음 중재를 준비한다. 이러한 종료 과정은 단순해 보이지만 고속 통신에서는 매우 작은 동기화 오차도 다음 프레임에 영향을 줄 수 있으므로 정확한 타이밍 관리가 필요하다.

논리적인 프레임 구조가 프로토콜의 동작을 정의한다면, 물리 계층(Physical Layer, PHY)은 실제로 약 10 Mbps 수준의 통신이 가능한지를 결정한다. 물리 계층은 차동 신호(Differential Signaling), 전기적 전압 레벨, 트랜시버 동작, 케이블 특성, 종단 방식, 커넥터 요구사항, 전파 지연 한계, EMC 요구사항을 정의한다. 성공적인 CAN XL 구현은 프로토콜 설계뿐 아니라 물리 계층 설계에도 동일한 수준의 중요성을 가진다.

CAN XL은 차동 통신 방식을 계속 사용한다. 차동 신호는 외부 전자파 간섭에 대한 뛰어난 내성을 제공한다. 두 개의 통신선은 서로 반대 방향의 전압 변화를 전달하며, 외부 잡음은 두 선에 거의 동일하게 유입된다. 수신기는 이러한 공통 모드(Common-Mode) 잡음을 제거하고 실제 통신 신호만을 검출할 수 있다. 이 원리는 데이터 속도가 크게 증가한 환경에서도 여전히 매우 효과적이다.

CAN XL의 물리 계층은 기존 오픈 드레인(Open-Drain) 방식보다 더 높은 속도를 지원하기 위해 향상된 트랜시버 동작을 도입하였다. 중재 구간에서는 기존 CAN과 동일한 우성 및 열성 신호를 사용하여 우선순위 기반 중재를 수행한다. 그러나 중재가 종료되면 트랜시버는 더욱 최적화된 신호 생성 방식을 사용하여 고속 데이터 전송을 수행한다. 이러한 하이브리드 방식은 기존 CAN 철학을 유지하면서도 훨씬 높은 물리 계층 성능을 제공한다.

고속 통신에서는 신호 에지(Edge) 품질이 매우 중요하다. 상승 에지(Rising Edge)와 하강 에지(Falling Edge)는 충분히 빠르면서도 오버슈트(Overshoot), 언더슈트(Undershoot), 링잉(Ringing), EMI를 최소화해야 한다. 따라서 트랜시버 출력단은 구동 강도(Drive Strength), 슬루율(Slew Rate), 출력 임피던스(Output Impedance), 스위칭 대칭성(Switching Symmetry)을 정밀하게 최적화해야 한다. 1 Mbps에서는 문제가 되지 않던 작은 파형 왜곡도 10 Mbps에서는 심각한 통신 오류의 원인이 될 수 있다.

전파 지연(Propagation Delay)은 고속 통신에서 가장 중요한 제한 요소 가운데 하나이다. 전체 지연에는 컨트롤러 출력, 트랜시버 스위칭, 케이블 전파 시간, 커넥터, 수신기 응답, 컨트롤러 입력 동기화가 모두 포함된다. 데이터 속도가 증가할수록 동일한 지연 시간이 전체 비트 시간에서 차지하는 비율이 급격히 증가한다. 따라서 네트워크 설계자는 모든 통신 경로에 대해 최악 조건의 전파 지연을 분석해야 한다.

발진기(Oscillator)의 정확도는 통신 속도가 증가할수록 더욱 중요해진다. 모든 컨트롤러는 자체 클록을 이용하여 통신 타이밍을 생성한다. 발진기 간의 작은 주파수 차이도 시간이 지나면서 누적된다. 고속 통신에서는 허용 가능한 타이밍 오차가 매우 작기 때문에 제조 편차, 온도 변화, 노화(Aging), 전원 전압 변화까지 고려한 고정밀 발진기가 요구된다.

케이블 품질 역시 CAN XL 성능에 큰 영향을 미친다. 연선(Twisted Pair)은 일정한 차동 임피던스를 유지하면서 외부 잡음의 영향을 최소화하는 가장 적합한 전송 매체이다. 도체 간격, 절연 특성, 케이블 형상, 제조 품질은 모두 안정적인 신호 전달에 영향을 준다. 데이터 속도와 신호 에지가 빨라질수록 고품질 케이블의 중요성은 더욱 커진다.

네트워크 토폴로지(Network Topology)는 물리 계층의 신뢰성에 직접적인 영향을 준다. 선형 백본(Linear Backbone)과 짧은 스텁(Stub)이 가장 권장되는 구조이다. 긴 스텁, 스타 연결(Star Connection), 사용하지 않는 케이블 연장, 과도한 커넥터 체인은 신호 반사를 발생시켜 이후 신호를 방해할 수 있다. 따라서 약 10 Mbps 수준의 통신에서는 배선 구조를 더욱 엄격하게 설계해야 한다.

종단 저항(Termination)은 통신선 내부의 반사를 억제하기 위해 반드시 필요하다. 네트워크 전체의 차동 임피던스가 일정하게 유지되어야 하며, 버스 양 끝에서는 신호 에너지를 흡수해야 한다. 기존 고속 CAN에서는 일반적으로 양 끝에 각각 120Ω 종단 저항을 사용하지만, CAN XL에서는 물리 계층 구현 방식과 운용 모드에 따라 추가적인 권장 사항이 적용될 수 있다.

커넥터 성능도 고속 통신에서는 더욱 중요해진다. 모든 커넥터는 임피던스 변화, 접촉 저항(Contact Resistance), 기생 정전용량(Parasitic Capacitance), 기계적 오차를 발생시킨다. 품질이 낮은 커넥터는 진동, 부식, 오염, 마모에 의해 국부적인 반사나 간헐적인 통신 오류를 유발할 수 있다. 따라서 자동차와 산업용 CAN XL 시스템은 고주파 차동 통신 전용 커넥터를 사용하는 것이 바람직하다.

PCB 레이아웃도 신호 무결성(Signal Integrity)에 직접적인 영향을 준다. 차동 신호선은 일정한 간격을 유지하며 함께 배선되어야 하고, 길이 차이를 최소화해야 한다. 고전류 스위칭 회로는 통신선과 충분히 분리해야 하며, 접지면(Ground Plane)은 안정적인 리턴 전류(Return Current)를 제공해야 한다. 보호 회로는 커넥터 가까이에 배치하고 디커플링 커패시터(Decoupling Capacitor)는 트랜시버 전원 핀 근처에 배치하는 것이 바람직하다.

전자파 적합성(EMC)은 CAN XL에서 가장 어려운 설계 과제 가운데 하나이다. 빠른 스위칭 에지는 높은 주파수 성분을 생성하여 전도 및 방사 노이즈를 증가시킨다. 따라서 케이블 배선, 차폐(Shielding), 접지 구조, 커넥터 위치, PCB 설계, 필터(Filter), 공통 모드 초크(Common-Mode Choke), 트랜시버 출력 특성을 모두 함께 최적화해야 한다. EMC 성능은 하나의 기술만으로 해결되는 것이 아니라 전체 시스템 설계를 통해 확보된다.

차동 통신을 사용하더라도 접지(Grounding)는 여전히 중요하다. 대형 산업 장비, 전기차, 자율이동로봇은 대용량 모터, 배터리, 인버터, 충전기, 전원 변환기를 포함하므로 큰 접지 전위차(Ground Potential Difference)가 발생할 수 있다. 트랜시버는 충분한 공통 모드 전압 허용 범위를 가져야 하며, 시스템 접지는 구동 전류가 통신 접지를 통과하지 않도록 설계해야 한다. 독립적인 전원 영역 사이에는 갈바닉 절연(Galvanic Isolation)을 적용하는 것도 효과적이다.

보호 회로(Protection Circuit)는 다양한 전기적 이상 상황으로부터 통신 하드웨어를 보호한다. 정전기 방전(Electrostatic Discharge, ESD), 서지(Surge), 로드 덤프(Load Dump), 역극성(Reverse Polarity), 단락(Short Circuit)을 모두 고려해야 한다. 보호 부품은 높은 보호 성능과 함께 낮은 기생 정전용량을 유지하여 고속 통신 성능을 저하시키지 않아야 한다. 이를 위해 적절한 부품 선정과 PCB 설계가 필요하다.

물리 계층은 전체 운용 환경에서도 안정적으로 동작해야 한다. 온도 변화는 발진기 주파수, 트랜시버 지연, 케이블 저항, 종단 저항 특성을 변화시킨다. 진동은 커넥터 접촉 상태에 영향을 주며, 습기와 오염은 장기적인 신뢰성을 저하시킬 수 있다. 따라서 CAN XL 하드웨어는 실제 사용 환경과 동일한 조건에서 충분한 검증을 수행해야 한다.

시스템이 복잡해질수록 네트워크 분할(Network Segmentation)의 중요성이 증가한다. 하나의 거대한 고속 네트워크 대신 여러 개의 CAN XL 영역(Domain)을 게이트웨이로 연결하면 각 네트워크의 케이블 길이를 줄이고 전파 지연을 감소시킬 수 있다. 이러한 구조는 확장성을 높이고 타이밍 검증과 고장 분석도 더욱 쉽게 만든다.

게이트웨이(Gateway)는 혼합 네트워크에서 매우 중요한 역할을 수행한다. Classical CAN, CAN FD, CAN XL, Automotive Ethernet, 산업용 Ethernet, 무선 통신을 연결하면서 프로토콜 변환, 라우팅, 필터링, 버퍼링, 보안 검사, 대역폭 관리 기능을 수행한다. 적절한 게이트웨이 설계는 대용량 진단 데이터나 소프트웨어 전송이 실시간 제어 통신을 방해하지 않도록 한다.

CAN XL의 물리 계층 검증은 단순히 프로토콜이 동작하는지만 확인해서는 충분하지 않다. 차동 전압, 신호 대칭성, 전파 지연, 상승 및 하강 시간, 링잉, 오버슈트, 언더슈트, 공통 모드 전압, 아이 다이어그램(Eye Diagram), 타이밍 여유(Timing Margin), EMC, 수신 감도를 모두 측정해야 한다. 오실로스코프(Oscilloscope), 차동 프로브(Differential Probe), 프로토콜 분석기, 벡터 네트워크 분석기(Vector Network Analyzer), EMC 시험 장비를 함께 활용해야 전체 네트워크 성능을 정확히 평가할 수 있다.

프로토콜 검증은 물리 계층 시험을 보완한다. 중재 동작, 프레임 해석, 오류 복구, 재전송, ACK 처리, 오류 제한, 다양한 제조사의 컨트롤러 간 상호운용성(Interoperability)을 모두 확인해야 한다. 성공적인 구현은 프로토콜의 논리적 정확성과 물리 계층의 전기적 무결성을 동시에 만족해야 한다. 프로토콜이 올바르게 구현되더라도 물리 계층의 타이밍이나 신호 품질이 기준을 만족하지 못하면 통신은 실패할 수 있다.

자율이동로봇(AMR)은 CAN XL 프레임과 물리 계층이 통합적으로 활용되는 대표적인 사례이다. 고성능 AMR은 구동 제어기, 조향 모듈, 배터리 시스템, 지능형 전력 분배 장치, 서스펜션 제어기, 리프트 장치, 환경 센서, 상위 제어 컴퓨터를 차량 전체에 분산 배치한다. CAN XL은 동작 제어와 안전 감시에 필요한 결정성을 유지하면서도 풍부한 진단 정보, 소프트웨어 업데이트, 설정 관리, 상세한 액추에이터 피드백을 충분한 대역폭으로 제공할 수 있다.

향후 임베디드 시스템은 CAN XL과 Automotive Ethernet을 함께 사용하는 방향으로 발전할 가능성이 높다. Ethernet은 카메라, LiDAR, 레이더(Radar), 인공지능(AI), 클라우드 연결, 초고대역폭 데이터를 담당하고, CAN XL은 CAN FD보다 훨씬 높은 대역폭을 제공하면서도 분산 임베디드 제어를 위한 결정적인 통신을 담당한다. 이러한 두 기술은 서로 경쟁하는 것이 아니라 상호 보완적인 관계이며, 차세대 차량, 산업 자동화 장비, 자율 로봇 시스템을 위한 유연하고 확장 가능한 통신 아키텍처를 구현하는 핵심 기술이 될 것이다.

## 5.3 CAN XL vs CAN FD Comparison

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

CAN XL과 CAN FD는 모두 동일한 Controller Area Network(CAN) 계열에 속하며, 결정적 중재(Deterministic Arbitration), 분산 제어(Distributed Control), 차동 통신(Differential Signaling), 강력한 오류 검출(Error Detection), 공유 버스를 이용한 높은 신뢰성 통신과 같은 기본 원리를 공유한다. 그러나 두 기술은 서로 다른 세대의 임베디드 네트워크 요구사항을 해결하기 위해 설계되었다. CAN FD는 Classical CAN의 한계를 극복하기 위해 페이로드와 통신 속도를 향상시켰으며, CAN XL은 소프트웨어 정의 시스템(Software-Defined System), 서비스 지향 아키텍처(Service-Oriented Architecture), 그리고 데이터 중심(Data-Intensive) 임베디드 시스템의 요구사항을 지원하면서도 CAN 고유의 결정적인 통신 특성을 유지하도록 개발되었다.

Classical CAN에서 CAN FD를 거쳐 CAN XL로 이어지는 발전 과정은 기존 기술을 완전히 대체하는 것이 아니라 네트워크 성능을 단계적으로 확장한 과정이다. Classical CAN은 최대 8바이트(Byte)의 페이로드와 약 1 Mbps 수준의 통신 속도를 기반으로 실시간 제어에 최적화되어 있었다. CAN FD는 비트 속도 전환(Bit Rate Switching, BRS)을 통해 데이터 구간 속도를 높이고 최대 64바이트의 페이로드를 지원하였다. CAN XL은 이를 더욱 발전시켜 약 2 KB 수준의 페이로드, 약 10 Mbps 수준의 통신 속도, 향상된 프로토콜 유연성, 그리고 현대적인 분산 소프트웨어 아키텍처를 지원하는 기능을 제공한다.

CAN FD와 CAN XL의 가장 큰 차이점 가운데 하나는 페이로드 용량이다. CAN FD는 최대 페이로드를 8바이트에서 64바이트로 증가시켜 진단 데이터(Diagnostic Data), 캘리브레이션 데이터(Calibration Data), 펌웨어 업데이트(Firmware Update)의 효율을 크게 향상시켰다. CAN XL은 이를 다시 약 2 KB 수준까지 확대하여 하나의 프레임으로 훨씬 더 많은 애플리케이션 데이터를 전송할 수 있도록 한다. 이로 인해 프로토콜 오버헤드(Protocol Overhead)가 감소하고, 컨트롤러 인터럽트(Interrupt) 발생 횟수가 줄어들며, 실제 데이터 전송 효율이 크게 향상된다.

대용량 페이로드는 통신 효율을 높이지만 애플리케이션 설계에서는 신중하게 사용해야 한다. CAN FD에서도 불필요하게 큰 프레임은 버스를 오래 점유하여 다른 메시지의 전송을 지연시킬 수 있었다. CAN XL은 훨씬 더 큰 페이로드를 지원하기 때문에 메시지 분할(Message Segmentation)과 통신 스케줄링(Scheduling)의 중요성이 더욱 커진다. 실시간 제어 데이터는 여전히 짧게 유지하고, 소프트웨어 배포, 이벤트 로그(Event Log), 엔지니어링 진단, 설정 관리(Configuration Management), 대용량 데이터 전송(Bulk Data Transfer)에는 CAN XL의 큰 프레임을 활용하는 것이 바람직하다.

통신 속도 역시 두 기술을 구분하는 중요한 요소이다. CAN FD는 일반적으로 기존 명목 비트 속도(Nominal Bit Rate)에서 중재를 수행한 후 Bit Rate Switching을 이용하여 데이터 구간만 수 Mbps 수준까지 가속한다. CAN XL은 향상된 프로토콜 구조와 새로운 물리 계층(Physical Layer)을 통해 약 10 Mbps 수준까지 통신 성능을 확장하였다. 이러한 발전은 더 높은 대역폭이 요구되는 미래 시스템을 위한 기반이 된다.

두 기술 모두 CAN의 핵심인 비파괴 비트 단위 중재(Nondestructive Bitwise Arbitration)를 그대로 유지한다. 여러 노드가 동시에 송신을 시도하더라도 가장 높은 우선순위의 식별자(Identifier)를 가진 노드가 즉시 버스를 획득하며, 다른 노드는 송신을 중단하고 수신기로 전환한다. 이러한 방식은 전체 네트워크 부하와 관계없이 안전 관련 시스템(Safety-Critical System)과 실시간 제어 시스템에 예측 가능한 통신 지연(Latency)을 제공한다.

중재 방식은 동일하지만, 중재 이후의 프로토콜 유연성은 크게 달라진다. CAN FD는 주로 고정된 식별자와 신호 기반(Signal-Oriented) 통신을 중심으로 설계되었다. 반면 CAN XL은 구조화된 데이터(Structured Data), 서비스 지향 통신(Service-Oriented Communication), 애플리케이션 세션(Application Session), 게이트웨이 라우팅(Gateway Routing) 등 미래의 소프트웨어 정의 시스템을 위한 다양한 통신 모델을 보다 효과적으로 지원한다.

프레임 구조(Frame Structure) 역시 상당한 차이를 가진다. CAN FD는 Classical CAN 프레임을 기반으로 제어 정보를 확장하고 Bit Rate Switching 기능을 추가한 구조를 사용한다. 반면 CAN XL은 훨씬 큰 페이로드와 다양한 프로토콜 정보를 지원하기 위해 프레임 일부를 재구성하였다. 이러한 구조는 향후 프로토콜 확장과 고급 통신 서비스를 지원하면서도 CAN 특유의 결정성을 유지하도록 설계되었다.

물리 계층(Physical Layer)도 두 기술의 중요한 차이점이다. CAN FD는 기존 고속 CAN 트랜시버(High-Speed CAN Transceiver)를 개선하여 데이터 구간에서 더 빠른 속도를 지원한다. CAN XL은 약 10 Mbps 수준의 통신을 위해 완전히 향상된 물리 계층 구조를 도입하였다. 중재 과정에서는 기존 CAN과 동일한 동작 방식을 유지하지만, 데이터 전송 구간에서는 보다 최적화된 신호 생성 기술을 사용하여 고속 통신을 구현한다.

전파 지연(Propagation Delay)의 중요성도 크게 증가한다. CAN FD에서도 케이블 길이, 트랜시버 지연, 발진기(Oscillator) 오차, 네트워크 토폴로지(Network Topology)를 고려해야 하지만, CAN XL은 비트 시간이 더욱 짧기 때문에 이러한 요소들이 훨씬 더 큰 영향을 미친다. 따라서 컨트롤러, 트랜시버, 케이블, 커넥터, 그리고 환경 조건까지 포함한 최악 조건(Worst Case) 타이밍 분석이 반드시 필요하다.

발진기의 성능 요구사항도 더욱 엄격해진다. CAN FD에서는 주기적인 재동기화(Resynchronization)를 통해 일정 수준의 오차를 허용할 수 있었지만, CAN XL은 동일한 타이밍 오차가 전체 비트 시간에서 차지하는 비율이 훨씬 커진다. 따라서 제조 편차, 온도 변화, 전원 변동, 장기 노화(Aging)를 모두 고려한 더욱 정밀한 발진기가 요구된다.

케이블 품질 역시 CAN XL에서 더욱 중요해진다. 두 기술 모두 차동 연선(Twisted Pair)을 사용하여 우수한 전자파 내성을 확보하지만, CAN XL은 훨씬 높은 통신 주파수를 사용하므로 임피던스 변화(Impedance Discontinuity), 커넥터 품질, 케이블 형상, 스텁(Stub)에 의한 신호 반사의 영향을 더욱 크게 받는다. 따라서 CAN FD에서는 문제가 없던 배선 구조라도 CAN XL에서는 개선이 필요할 수 있다.

네트워크 토폴로지의 기본 원칙은 동일하지만, CAN XL에서는 더욱 엄격한 설계가 요구된다. 두 기술 모두 선형 버스(Linear Bus)와 짧은 스텁 구조를 권장하지만, CAN XL은 스텁 길이, 커넥터 개수, 사용하지 않는 케이블, 임피던스 변화 등에 대해 훨씬 작은 허용 오차를 가진다. 통신 속도가 높아질수록 허용 가능한 타이밍 여유가 줄어들기 때문이다.

종단 저항(Termination)의 역할도 여전히 매우 중요하다. 적절한 임피던스 매칭(Impedance Matching)은 신호 반사를 방지하여 안정적인 통신을 가능하게 한다. CAN FD는 일반적으로 버스 양 끝에 120Ω 종단 저항을 사용하지만, CAN XL은 사용하는 물리 계층과 트랜시버 구조에 따라 추가적인 권장 사항이 적용될 수 있다. 따라서 기존 방식만을 그대로 사용하는 것이 아니라 실제 신호 품질을 측정하여 검증해야 한다.

전자파 적합성(Electromagnetic Compatibility, EMC)은 두 기술 모두 중요한 요소이지만 CAN XL에서는 더욱 까다롭다. 빠른 신호 에지는 더 많은 고주파 성분을 생성하여 전도 및 방사 노이즈를 증가시킨다. 따라서 차폐(Shielding), 접지(Grounding), 케이블 배선, 공통 모드 초크(Common-Mode Choke), PCB 레이아웃(Layout), 커넥터, 트랜시버 설계를 모두 함께 최적화해야 한다. CAN XL은 기존 CAN FD보다 더욱 체계적인 EMC 설계를 요구한다.

오류 검출(Error Detection)은 두 기술이 공유하는 가장 중요한 공통점 가운데 하나이다. 두 프로토콜 모두 송수신 비트를 지속적으로 감시하고 다양한 오류 검사를 통해 프레임의 무결성을 확인한다. 오류가 발생하면 자동으로 재전송을 수행하며, 반복적으로 오류를 발생시키는 노드는 CAN의 오류 제한(Error Confinement) 메커니즘을 통해 네트워크에서 스스로 격리된다. 이러한 강력한 오류 관리 방식은 CAN 계열의 가장 큰 장점으로 유지되고 있다.

ACK(Acknowledgement) 메커니즘도 두 기술 모두 동일하게 유지된다. 송신기는 최소 하나 이상의 수신기가 프레임을 정상적으로 수신했는지를 확인한다. ACK가 존재하지 않으면 통신 실패 또는 활성 수신기가 없음을 의미하며, 애플리케이션 수준의 별도 복구 절차 없이 자동 재전송을 수행한다. 이러한 자동 복구 기능은 다른 산업용 통신 방식보다 임베디드 소프트웨어를 훨씬 단순하게 만들어 준다.

프로토콜 오버헤드는 두 기술 사이에서 상당한 차이를 가진다. CAN FD는 중간 크기의 메시지에는 매우 효율적이지만, 대용량 데이터를 전송하려면 여전히 여러 개의 프레임으로 분할해야 한다. CAN XL은 약 2 KB 수준의 페이로드를 제공하므로 프레임 분할 횟수를 크게 줄일 수 있다. 따라서 소프트웨어 업데이트, 캘리브레이션 데이터, 엔지니어링 로그, 진단 정보, 구조화된 데이터 교환에서 훨씬 높은 대역폭 효율을 제공한다.

애플리케이션 소프트웨어 구조도 CAN XL에서 크게 발전하였다. CAN FD는 일반적으로 고정된 데이터베이스(Database)에 정의된 신호를 중심으로 통신한다. CAN XL은 구조화된 정보, 서비스 지향 통신, 애플리케이션 라우팅(Application Routing), 향후 프로토콜 확장을 지원할 수 있도록 설계되었다. 이러한 특성은 소프트웨어 정의 차량과 분산 로봇 플랫폼에 더욱 적합하다.

게이트웨이(Gateway)의 역할도 더욱 중요해진다. 미래의 시스템은 Classical CAN, CAN FD, CAN XL, Automotive Ethernet, 산업용 Ethernet, 무선 통신을 동시에 사용할 가능성이 높다. 게이트웨이는 프로토콜 변환, 페이로드 변환, 라우팅(Routing), 버퍼링(Buffering), 대역폭 관리(Bandwidth Management), 보안 검사(Security Inspection)를 수행하여 각 네트워크가 가장 적합한 역할을 수행하도록 한다.

CAN FD에서 CAN XL로의 전환은 대부분 점진적으로 이루어질 것이다. 현재의 많은 산업용 제어기, 차체 전장(Body Electronics), 배터리 관리 시스템(Battery Management System), 모터 드라이브(Motor Drive), 자동화 장비는 CAN FD만으로도 충분한 성능을 제공한다. CAN XL은 대용량 데이터, 소프트웨어 확장성, 서비스 지향 통신이 필요한 새로운 시스템에서 우선적으로 적용될 가능성이 높다. 따라서 향후 상당 기간 동안 두 기술은 동일한 시스템 안에서 함께 사용될 것이다.

CAN XL은 더 많은 프로토콜 기능, 높은 통신 속도, 엄격한 물리 계층 요구사항, 큰 메모리 공간, 복잡한 소프트웨어 구조, 그리고 더욱 철저한 검증 절차를 요구하므로 개발 난이도가 다소 증가한다. 그러나 이러한 추가적인 개발 노력은 훨씬 높은 통신 성능과 장기적인 시스템 확장성이라는 이점을 제공한다.

검증 절차 역시 CAN XL에서 더욱 강화된다. 기존 프로토콜 시험 외에도 신호 무결성(Signal Integrity), 타이밍 분석(Timing Analysis), 아이 다이어그램(Eye Diagram), EMC 시험, 상호운용성(Interoperability), 환경 시험(Environmental Qualification)을 모두 수행해야 한다. 통신 속도가 높아질수록 전기적인 허용 오차가 줄어들기 때문에 물리 계층 검증은 프로토콜 검증만큼 중요한 요소가 된다.

비용 측면에서도 두 기술은 차이를 가진다. CAN FD는 기존 인프라를 활용할 수 있으며 중간 수준의 데이터 통신이 필요한 대부분의 분산 제어 시스템에서 매우 경제적인 솔루션이다. CAN XL은 새로운 컨트롤러, 고성능 트랜시버, 고품질 케이블, 보다 철저한 검증이 필요할 수 있다. 그러나 대용량 데이터를 처리하는 시스템에서는 프레임 수 감소, 프로토콜 단순화, 전송 효율 향상을 통해 전체 시스템 복잡도를 오히려 줄일 수도 있다.

자율이동로봇(Autonomous Mobile Robot, AMR)은 두 기술의 차이를 가장 잘 보여주는 사례이다. 비교적 단순한 실내 운반 로봇은 구동 제어, 배터리 정보, 안전 신호, 센서 데이터 정도만 처리하므로 CAN FD만으로도 충분하다. 반면 대형 야외 로봇, 건설 장비, 광산 차량, 농업용 기계, 지능형 물류 시스템은 대용량 진단 정보, 소프트웨어 업데이트, 구조화된 설정 데이터베이스, 예지 정비(Predictive Maintenance), 서비스 지향 통신을 지속적으로 처리해야 하므로 CAN XL의 높은 대역폭과 큰 페이로드가 큰 장점을 제공한다.

산업 자동화 시스템에서도 유사한 변화가 나타난다. 기존 PLC(Programmable Logic Controller), 분산 입출력(Distributed I/O), 서보 드라이브(Servo Drive), 센서 네트워크는 CAN FD만으로도 충분한 경우가 많다. 그러나 디지털 트윈(Digital Twin), 머신러닝(Machine Learning), 클라우드 진단(Cloud Diagnostics), 예지 정비, 엣지 컴퓨팅(Edge Computing), 협동 로봇(Collaborative Robot), 소프트웨어 정의 제조 시스템은 훨씬 풍부한 정보 교환이 필요하며, CAN XL은 이러한 미래형 산업 환경에 더욱 적합한 통신 성능을 제공한다.

소프트웨어 정의 차량(Software-Defined Vehicle)은 CAN XL 개발의 가장 큰 배경 가운데 하나이다. 현대 차량은 중앙 집중형 컴퓨팅(Centralized Computing), OTA(Over-the-Air) 소프트웨어 업데이트, 서비스 지향 통신, 사이버 보안(Cybersecurity), 클라우드 연결, 지속적인 소프트웨어 기능 확장을 요구한다. CAN FD는 기존 임베디드 제어를 계속 담당하고, CAN XL은 미래 전자 아키텍처를 위한 더 높은 대역폭과 유연한 프로토콜을 제공하면서도 CAN의 결정적 통신 철학을 유지한다.

CAN XL을 CAN FD의 단순한 대체 기술로 보는 것은 적절하지 않다. 두 기술은 동일한 CAN 계열에서 서로 다른 역할을 담당하는 상호 보완적인 기술이다. CAN FD는 중간 수준의 대역폭이 필요한 결정적 제어 시스템에 매우 적합하며, CAN XL은 대용량 구조화 데이터, 높은 통신 성능, 소프트웨어 정의 기능, 미래 임베디드 아키텍처를 지원한다. 따라서 어떤 기술이 더 우수한가보다는 요구되는 대역폭(Bandwidth), 애플리케이션 복잡도(Application Complexity), 시스템 확장성(Scalability), 장기 유지보수 전략(Maintenance Strategy), 그리고 전체 시스템 아키텍처(System Architecture)에 따라 적절한 기술을 선택하는 것이 가장 중요하다.

## 5.4 CAN XL Automotive Roadmap

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

CAN XL은 Controller Area Network(CAN) 계열의 세 번째 주요 세대로, 차세대 자동차 전기·전자(Electrical and Electronic, E/E) 아키텍처(Architecture)의 통신 요구사항을 충족하기 위해 설계되었다. Classical CAN이 결정적(Deterministic) 실시간 제어를 위해 최적화되었고 CAN FD가 페이로드(Payload) 용량과 통신 속도를 확장하였다면, CAN XL은 훨씬 더 큰 페이로드, 높은 대역폭(Bandwidth), 그리고 향상된 프로토콜 유연성(Protocol Flexibility)을 제공한다. CAN XL은 기존 CAN 기술을 대체하는 것이 아니라 장기적인 발전 로드맵(Roadmap)의 일부로서 서로 다른 CAN 세대가 애플리케이션(Application)의 요구사항에 따라 함께 공존하도록 설계되었다. 이러한 점진적인 발전 전략은 기존 시스템에 대한 투자를 보호하면서 새로운 통신 기능을 점진적으로 도입할 수 있도록 지원한다.

자동차 산업은 지난 수십 년 동안 근본적인 변화를 겪고 있다. 기존 차량은 상대적으로 작은 제어 메시지(Message)를 교환하는 다수의 독립적인 전자제어장치(Electronic Control Unit, ECU)로 구성되어 있었다. 그러나 현대 차량은 지속적인 소프트웨어 업데이트(Software Update), 첨단 운전자 지원 시스템(Advanced Driver Assistance System, ADAS), 중앙 집중형 컴퓨팅(Centralized Computing), 클라우드 연결(Cloud Connectivity), 인공지능(Artificial Intelligence, AI), 그리고 점점 복잡해지는 센서 처리(Sensor Processing)를 요구한다. 이러한 변화는 실시간 성능과 기능 안전(Functional Safety)을 유지하면서도 훨씬 더 높은 통신 대역폭을 필요로 하며, CAN XL은 이러한 새로운 요구사항을 충족하기 위해 개발되었다.

Classical CAN에서 CAN FD, 그리고 CAN XL로 이어지는 발전 과정은 단순히 통신 속도를 높인 것이 아니라 소프트웨어 아키텍처(Software Architecture)의 진화를 반영한다. Classical CAN은 분산된 ECU 사이에서 개별 제어 신호(Control Signal)를 전달하는 역할에 집중하였다. CAN FD는 Bit Rate Switching(BRS)을 통해 더 큰 진단 데이터(Diagnostic Data)와 펌웨어 업데이트(Firmware Update)를 효율적으로 처리할 수 있도록 개선되었다. CAN XL은 이를 더욱 발전시켜 구조화된 애플리케이션 데이터(Structured Application Data), 서비스 지향 통신(Service-Oriented Communication), 그리고 향후 프로토콜 확장(Future Protocol Extension)을 지원하는 기반을 제공한다.

CAN XL 개발의 가장 중요한 배경 가운데 하나는 소프트웨어 정의 차량(Software-Defined Vehicle, SDV)으로의 전환이다. 기존 차량에서는 기능이 하드웨어(Hardware)에 의해 결정되었지만, SDV에서는 새로운 기능을 지속적으로 소프트웨어를 통해 추가하거나 개선할 수 있다. OTA(Over-the-Air) 소프트웨어 업데이트, 기능 활성화(Feature Activation), 사이버 보안(Cybersecurity) 개선, 예지 정비(Predictive Maintenance), 그리고 클라우드 기반 서비스는 과거보다 훨씬 많은 데이터를 지속적으로 교환해야 한다. CAN XL은 이러한 대용량 데이터 교환을 지원하면서도 CAN 고유의 결정적 중재(Deterministic Arbitration)를 유지한다.

중앙 집중형 컴퓨팅(Centralized Computing) 아키텍처 역시 CAN XL 채택을 촉진하는 중요한 요소이다. 기존 차량은 100개 이상의 ECU가 여러 개의 CAN 버스를 통해 연결되는 구조를 사용하는 경우가 많았다. 최근에는 다수의 소형 ECU를 소수의 고성능 도메인 컨트롤러(Domain Controller)와 중앙 컴퓨팅 장치(Central Computing Unit)로 통합하는 방향으로 발전하고 있다. 이러한 변화는 여러 컴퓨팅 도메인(Computing Domain) 사이에서 대용량 데이터를 안정적으로 교환할 수 있는 새로운 통신 인프라를 요구하며, CAN XL은 이러한 요구사항을 충족하도록 설계되었다.

차량 네트워크는 기능 중심(Function-Oriented) 구조에서 영역 중심(Zonal Architecture) 구조로도 변화하고 있다. 영역 중심 아키텍처에서는 차량의 각 물리적 영역(Physical Zone)에 위치한 센서와 액추에이터(Actuator)의 정보를 지역 컨트롤러(Local Controller)가 수집한 후 중앙 컴퓨팅 시스템으로 전달한다. 이러한 구조는 배선(Wiring)을 단순화하고 통신 효율을 향상시킨다. CAN XL은 집계된 센서 데이터(Aggregated Sensor Data)를 보다 효율적으로 전달할 수 있는 충분한 페이로드를 제공하면서도 실시간 제어를 위한 결정성을 유지한다.

자동차 통신 로드맵은 하나의 프로토콜만 사용하는 방향이 아니라 여러 네트워크 기술을 함께 사용하는 방향으로 발전하고 있다. 저속 장치는 LIN(Local Interconnect Network)을 사용하고, 기존 제어 시스템은 Classical CAN을 계속 활용한다. CAN FD는 중간 수준의 대역폭이 필요한 제어, 진단(Diagnostics), 배터리 관리 시스템(Battery Management System, BMS)에 적합하다. CAN XL은 구조화된 데이터와 서비스 지향 통신을 담당하며, Automotive Ethernet은 카메라(Camera), 중앙 컴퓨팅, 멀티미디어(Multimedia)와 같은 초고속 데이터 전송을 담당한다. 각각의 기술은 차량 전체 네트워크에서 자신에게 가장 적합한 역할을 수행한다.

CAN XL은 여러 개의 CAN FD 프레임(Frame)으로 나누어 전송해야 했던 대용량 구조화 데이터를 처리하는 데 특히 유리하다. 소프트웨어 배포(Software Deployment), 엔지니어링 진단(Engineering Diagnostics), 캘리브레이션 데이터베이스(Calibration Database), 설정 관리(Configuration Management), 예지 정비 정보, 서비스 지향 데이터 교환은 모두 CAN XL의 큰 페이로드를 효과적으로 활용할 수 있다. 프레임 분할(Frame Segmentation)이 감소하면 프로토콜 오버헤드(Protocol Overhead)가 줄어들고, 프로세서 인터럽트(Processor Interrupt) 발생 횟수가 감소하며, 애플리케이션 소프트웨어도 더욱 단순하게 구현할 수 있다.

게이트웨이(Gateway)의 역할 역시 미래 자동차에서 크게 확대되고 있다. 과거에는 단순히 서로 다른 버스(Bus) 사이에서 메시지를 전달하는 역할을 수행하였다면, 미래의 게이트웨이는 프로토콜 변환(Protocol Conversion), 애플리케이션 라우팅(Application Routing), 대역폭 관리(Bandwidth Management), 보안 검사(Security Inspection), 데이터 집계(Data Aggregation), 네트워크 가상화(Network Virtualization), 서비스 관리(Service Management) 등을 수행한다. CAN XL은 구조화된 애플리케이션 정보를 효율적으로 전달함으로써 이러한 지능형 게이트웨이 기능을 지원한다.

사이버 보안(Cybersecurity) 역시 CAN XL 로드맵에서 중요한 요소이다. 미래 차량은 안전한 OTA 업데이트, 인증(Authentication), 보호된 진단, 보안 부팅(Secure Boot), 그리고 지속적인 취약점 관리(Vulnerability Management)를 필요로 한다. CAN XL 자체가 보안 기능을 제공하는 것은 아니지만, 인증서(Certificate), 암호화 데이터(Encrypted Data), 인증 정보(Authentication Information), 소프트웨어 패키지(Software Package), 보안 메타데이터(Security Metadata)와 같은 대용량 정보를 효율적으로 전달할 수 있도록 지원한다.

자율주행(Autonomous Driving) 기술의 발전 역시 CAN XL 도입을 가속화하는 요인이다. 첨단 운전자 지원 시스템(ADAS), 자율주행 제어기(Autonomous Driving Controller), 센서 융합(Sensor Fusion), 고정밀 지도(High-Definition Map), 인공지능(AI) 프로세서는 지속적으로 많은 양의 구조화된 데이터를 교환한다. 고해상도 센서 데이터는 일반적으로 Automotive Ethernet을 통해 전송되지만, 진단 정보, 설정 동기화(Configuration Synchronization), 상태 모니터링(Health Monitoring), 장애 보고(Fault Reporting), 서브시스템 협조(Sub-System Coordination)와 같은 기능은 CAN XL을 통해 효율적으로 처리될 수 있다.

자동차와 클라우드(Cloud) 간의 연계 역시 지속적으로 강화되고 있다. 차량은 플릿 관리(Fleet Management), 예지 정비, 디지털 트윈(Digital Twin), 원격 진단(Remote Diagnostics), 소프트웨어 생명주기 관리(Software Lifecycle Management)를 위해 지속적으로 데이터를 클라우드와 교환한다. CAN XL은 차량 내부에서 이러한 구조화된 데이터를 효율적으로 전달하여 상위 시스템이 무선 통신(Wireless Communication)을 통해 외부와 연계할 수 있도록 지원한다.

전기차(Electric Vehicle)의 발전 역시 CAN XL의 필요성을 높이고 있다. 배터리 관리 시스템(BMS), 충전 제어기(Charging Controller), 열관리 시스템(Thermal Management System), 전력전자(Power Electronics), 에너지 최적화(Energy Optimization), 고전압 안전(High-Voltage Safety)은 과거보다 훨씬 더 많은 진단 정보와 운영 데이터를 지속적으로 교환한다. 실시간 제어 신호는 여전히 CAN FD를 활용할 수 있지만, 엔지니어링 데이터, 장기 운용 이력(Historical Performance Data), 캘리브레이션 데이터, 유지보수 정보는 CAN XL의 대용량 페이로드를 활용하는 것이 더욱 효율적이다.

CAN XL은 기존 CAN 시스템을 한 번에 대체하기보다는 점진적으로 도입될 것으로 예상된다. 자동차 개발 주기는 매우 길며 제조사들은 이미 CAN 및 CAN FD 기반 인프라에 막대한 투자를 수행하였다. 따라서 향후 상당 기간 동안 Classical CAN, CAN FD, CAN XL, Automotive Ethernet, LIN이 동시에 사용되는 혼합 네트워크(Hybrid Network)가 일반적인 구조가 될 가능성이 높다. 게이트웨이는 서로 다른 네트워크 간의 상호운용성(Interoperability)을 제공하면서 새로운 기술이 기존 시스템과 함께 동작하도록 지원한다.

반도체(Semiconductor)의 발전 역시 CAN XL 로드맵의 핵심 요소이다. 새로운 CAN XL 컨트롤러(Controller), 고성능 트랜시버(Transceiver), 향상된 마이크로컨트롤러(Microcontroller), 개선된 물리 계층(Physical Layer), 그리고 새로운 네트워크 분석 도구(Network Analysis Tool)가 점차 상용화되고 있다. 다양한 반도체 제조사들이 표준 기반 제품을 출시함에 따라 개발 비용은 점차 감소하고 상호운용성도 지속적으로 향상될 것으로 기대된다.

물리 계층(Physical Layer)의 발전 역시 매우 중요하다. 더 높은 통신 속도는 신호 무결성(Signal Integrity), 케이블 임피던스(Cable Impedance), 커넥터 품질(Connector Quality), 전파 지연(Propagation Delay), 전자파 적합성(Electromagnetic Compatibility, EMC), 그리고 네트워크 토폴로지(Network Topology)에 대해 더욱 엄격한 설계를 요구한다. 차량 제조사는 타이밍 분석(Timing Analysis), 신호 무결성 시뮬레이션(Signal Integrity Simulation), 아이 다이어그램(Eye Diagram) 분석, 최악 조건(Worst Case) 환경 시험 등을 통해 통신 품질을 충분히 검증해야 한다.

CAN XL 로드맵은 승용차뿐만 아니라 상용차(Commercial Vehicle)와 지능형 이동체(Intelligent Mobility Platform)로도 확대되고 있다. 대형 트럭(Truck), 버스(Bus), 건설 장비(Construction Equipment), 농업 기계(Agricultural Machinery), 광산 차량(Mining Vehicle), 자율 배송 로봇(Autonomous Delivery Robot), 산업용 이동 로봇(Industrial Mobile Robot) 역시 SDV와 유사한 소프트웨어 중심 구조를 요구하고 있다. 이러한 시스템은 결정적인 통신 특성을 유지하면서도 대용량 진단 데이터, 소프트웨어 배포, 설정 데이터베이스, 플릿 관리 정보를 효율적으로 처리할 수 있는 CAN XL의 장점을 활용할 수 있다.

자율이동로봇(Autonomous Mobile Robot, AMR)은 자동차 통신 기술이 산업용 로봇으로 확장되는 대표적인 사례이다. 비교적 단순한 실내 물류 로봇은 모터 제어(Motor Control), 배터리 관리(Battery Management), 안전 신호(Safety Signal) 등을 위해 CAN FD만으로도 충분한 성능을 제공한다. 그러나 야외 자율주행 차량, 대형 산업용 로봇, 광산 장비, 농업 기계, 검사 로봇은 대용량 진단 로그(Diagnostic Log), 소프트웨어 배포, 설정 데이터베이스, 클라우드 동기화(Cloud Synchronization), 고급 플릿 관리(Fleet Management)를 지속적으로 수행해야 하므로 CAN XL이 더욱 적합한 통신 플랫폼이 된다.

미래 자동차 통신 아키텍처는 경쟁 관계가 아니라 상호 보완적인 여러 기술의 조합으로 발전할 것이다. LIN은 단순한 차체 전장(Body Electronics)을 담당하고, CAN FD는 결정적인 중간 대역폭 제어를 담당하며, CAN XL은 구조화된 대용량 임베디드 통신을 담당한다. Automotive Ethernet은 초고속 센서와 중앙 컴퓨팅을 연결하는 역할을 수행한다. 이러한 계층적 통신 구조(Hierarchical Communication Architecture)는 성능, 확장성(Scalability), 신뢰성(Reliability), 비용 효율성(Cost Efficiency)을 모두 확보하면서 소프트웨어 정의 차량과 미래 지능형 모빌리티 시스템을 지원하는 핵심 기반이 될 것이다. CAN XL의 자동차 로드맵은 단순한 통신 속도 향상이 아니라 더욱 지능적이고 서비스 지향적(Service-Oriented)이며, 안전하고 지속적으로 진화하는 미래 차량 전자 아키텍처를 실현하기 위한 핵심 기술 발전 방향을 의미한다.

## 5.5 CAN XL Robotics Prospect

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 통신 아키텍처(Robotics Communication Architecture)는 자율 시스템(Autonomous System)이 더욱 지능화되고, 상호 연결되며, 소프트웨어 중심으로 발전함에 따라 빠르게 진화하고 있다. 기존 산업용 로봇은 주로 필드버스(Fieldbus)나 기존 CAN 네트워크를 이용하여 모터 제어기(Motor Controller), 센서(Sensor), 프로그래머블 로직 컨트롤러(Programmable Logic Controller, PLC) 사이에서 결정적인 제어 신호(Control Signal)를 교환하였다. 그러나 현대의 자율이동로봇(Autonomous Mobile Robot, AMR), 휴머노이드(Humanoid), 사족보행 로봇(Quadruped), 협동 로봇(Collaborative Robot), 농업 기계(Agricultural Machine), 건설 장비(Construction Equipment), 자율 광산 차량(Autonomous Mining Vehicle)은 인공지능(AI), 클라우드 서비스(Cloud Service), 진단(Diagnostics), 플릿 관리(Fleet Management), 그리고 다양한 인지(Perception) 데이터를 지속적으로 처리해야 한다. 이러한 변화는 기존 CAN 네트워크의 한계를 넘어서는 통신 성능을 요구하면서도 실시간 결정성(Deterministic Real-Time Control)은 그대로 유지해야 한다. CAN XL은 이러한 미래 로봇 통신 요구사항을 충족하기 위해 기존 CAN 생태계를 자연스럽게 확장하는 진화형 기술이다.

로봇 통신의 발전 과정은 자동차 전자 시스템의 발전 과정과 매우 유사하다. 초기 로봇은 비교적 단순한 센서 피드백(Sensor Feedback)과 중앙 제어기(Central Controller)를 기반으로 반복적인 동작을 수행하였다. 그러나 자율성이 향상되면서 다수의 카메라(Camera), LiDAR, 레이더(Radar), GNSS 수신기, 관성측정장치(Inertial Measurement Unit, IMU), 힘 센서(Force Sensor), 환경 센서(Environmental Sensor), 그리고 분산 컴퓨팅 플랫폼(Distributed Computing Platform)이 통합되기 시작하였다. 이에 따라 통신 네트워크는 단순한 액추에이터 명령(Actuator Command) 전달에서 벗어나 분산 처리(Distributed Processing), 소프트웨어 서비스(Software Service), 그리고 상위 수준의 애플리케이션 조정(Application Coordination)을 지원하는 방향으로 발전하였다. CAN XL은 이러한 변화를 지원하면서도 CAN의 결정적 중재(Deterministic Arbitration)를 그대로 유지하는 중요한 통신 기술이다.

현대 로봇은 하나의 통신 프로토콜만 사용하는 것이 아니라 다양한 통신 기술을 함께 사용하는 계층형 구조(Hierarchical Architecture)를 채택하고 있다. 실시간 모터 제어는 EtherCAT이나 CAN FD가 담당하고, 센서 시간 동기화(Sensor Synchronization)는 Precision Time Protocol(PTP)을 사용하며, 대용량 인지 데이터는 Gigabit Ethernet을 통해 전송되고, 클라우드 통신은 Wi-Fi, 5G, 위성 통신(Satellite Communication) 등을 활용한다. CAN XL은 이러한 계층 구조에서 CAN FD보다 큰 구조화된 데이터(Structured Data)를 처리하면서도 전체 시스템을 Ethernet으로 구성하지 않아도 되는 중간 수준의 결정적 통신 계층으로 매우 적합하다.

CAN XL이 로봇 분야에서 가지는 가장 큰 장점 가운데 하나는 크게 향상된 페이로드(Payload) 용량이다. CAN FD는 최대 64바이트(Byte)의 데이터를 지원하므로 대용량 엔지니어링 데이터는 여러 프레임(Frame)으로 나누어 전송해야 했다. CAN XL은 약 2 KB 수준의 페이로드를 지원하여 캘리브레이션 데이터베이스(Calibration Database), 설정 파일(Configuration File), 진단 기록(Diagnostic Record), 소프트웨어 패키지(Software Package), 파라미터(Parameter), 유지보수 로그(Maintenance Log), 그리고 구조화된 운용 데이터를 훨씬 효율적으로 전송할 수 있다. 큰 프레임은 프로토콜 오버헤드(Protocol Overhead)를 줄이고, 프로세서 인터럽트(Processor Interrupt)를 감소시키며, 전체 통신 효율을 향상시킨다.

미래 로봇은 소프트웨어 정의 로보틱스(Software-Defined Robotics)의 개념을 기반으로 발전할 것으로 예상된다. 제조 시점에 기능이 고정되는 것이 아니라 운영 기간 동안 새로운 자율 기능(Autonomous Function), 인공지능 모델(AI Model), 사이버 보안(Cybersecurity) 업데이트, 성능 개선 등이 지속적으로 소프트웨어를 통해 추가된다. 이러한 지속적인 소프트웨어 관리(Software Lifecycle Management)는 대용량 소프트웨어를 안정적으로 전송할 수 있는 통신 네트워크를 필요로 하며, CAN XL은 기존 임베디드 제어(Embedded Control)와 호환성을 유지하면서 이러한 지속적 배포(Continuous Deployment)를 지원한다.

인공지능(AI)의 도입은 로봇 내부 통신 요구사항을 더욱 증가시키고 있다. 엣지 AI 컴퓨터(Edge AI Computer)는 인지 결과(Perception Result), 위치 추정(Localization), 의미 지도(Semantic Map), 장애물 분류(Obstacle Classification), 환경 모델(Environment Model), 임무 정보(Mission Update), 시스템 상태(System Status)를 하위 제어기와 지속적으로 교환한다. 원시 센서 데이터(Raw Sensor Data)는 일반적으로 고속 Ethernet을 이용하지만, AI 추론 결과(Inference Result), 모델 관리(Model Management), 설정 정보(Configuration), 상태 모니터링(Health Monitoring) 등은 CAN XL을 통해 효율적으로 전달될 수 있다. 이를 통해 각 통신 기술은 자신에게 가장 적합한 역할을 수행할 수 있다.

플릿 관리(Fleet Management)는 CAN XL이 활용될 수 있는 또 다른 중요한 분야이다. 현대의 물류창고(Warehouse), 제조 공장(Manufacturing Facility), 항만(Port), 공항(Airport), 병원(Hospital), 광산(Mining Site)은 수십에서 수백 대의 자율 로봇을 동시에 운영하고 있다. 각 로봇은 임무(Mission), 경로 계획(Navigation Parameter), 진단 정보(Diagnostics), 소프트웨어 버전(Software Version), 배터리 상태(Battery Status), 자원 할당(Resource Allocation) 등을 지속적으로 관리해야 한다. CAN XL은 로봇 내부 제어기 간에 이러한 구조화된 데이터를 효율적으로 전달하면서 상위 네트워크와의 연계도 지원한다.

산업용 이동 로봇(Industrial Mobile Robot)은 운영 기간 동안 더욱 정교한 진단 기능을 요구한다. 과거에는 단순한 오류 코드(Error Code)와 기본 상태 정보만 제공하였다면, 미래 로봇은 시스템 건강 상태(System Health), 환경 운용 이력(Environmental History), 예지 정비 정보(Predictive Maintenance Indicator), 캘리브레이션 이력(Calibration History), 액추에이터 성능 변화(Actuator Performance Trend), 소프트웨어 구성(Software Inventory), 사이버 보안 상태(Cybersecurity Status), 이벤트 로그(Event Log)를 지속적으로 저장하고 분석하게 된다. 이러한 구조화된 진단 데이터는 기존 CAN보다 CAN XL에서 훨씬 효율적으로 처리될 수 있다.

설정 관리(Configuration Management)의 중요성도 지속적으로 증가하고 있다. 현대 로봇은 센서, 매니퓰레이터(Manipulator), 배터리 팩(Battery Pack), 검사 장비(Inspection Equipment), 통신 장치(Communication Device), 다양한 페이로드(Payload)를 필요에 따라 교체하거나 추가할 수 있도록 설계된다. 설정 데이터베이스(Configuration Database)를 여러 제어기에 효율적으로 배포하면 초기 설치(Commissioning), 현장 업그레이드(Field Upgrade), 유지보수(Maintenance), 그리고 제품 맞춤화(Customization)가 훨씬 쉬워진다. CAN XL은 큰 설정 데이터를 적은 수의 프레임으로 전송하여 시스템 초기화와 재구성(Reconfiguration)의 복잡성을 줄여준다.

디지털 트윈(Digital Twin)은 CAN XL이 큰 장점을 제공하는 또 다른 미래 기술이다. 디지털 트윈은 실제 로봇의 상태를 지속적으로 반영하는 가상 시스템(Virtual Representation)이다. 제어기는 운용 데이터(Operational Data), 캘리브레이션 값(Calibration Value), 유지보수 이력(Maintenance History), 환경 조건(Environmental Condition), 부품 상태(Component Health), 소프트웨어 버전 등을 지속적으로 상위 컴퓨팅 시스템으로 전달한다. CAN XL은 이러한 구조화된 데이터를 효율적으로 전달하여 디지털 트윈의 실시간 동기화를 지원한다.

예지 정비(Predictive Maintenance)는 장기간 축적된 대량의 운용 데이터를 기반으로 동작한다. 모터 온도(Motor Temperature), 진동(Vibration), 배터리 열화(Battery Degradation), 액추에이터 토크(Torque), 바퀴 미끄러짐(Wheel Slip), 자율주행 성능(Navigation Performance), 환경 조건(Environmental Exposure), 통신 품질(Communication Quality), 제어기 부하(Controller Utilization) 등은 모두 유지보수 계획을 최적화하는 데 활용된다. 고주파 센서 데이터는 로컬(Local)에서 처리하더라도 통계 정보와 장기 운용 데이터는 CAN XL을 통해 효율적으로 전달할 수 있다.

로봇 사이버 보안(Cybersecurity)의 중요성도 지속적으로 증가하고 있다. 미래 로봇은 인증된 소프트웨어 배포(Authenticated Software Deployment), 보안 부팅(Secure Boot), 암호화된 설정 관리(Encrypted Configuration), 인증서(Certificate) 배포, 접근 제어(Access Control), 침입 탐지(Intrusion Detection), 지속적인 취약점 관리(Vulnerability Management)를 수행해야 한다. CAN XL이 직접 보안 기능을 제공하는 것은 아니지만, 인증 정보(Authentication Information), 암호화 데이터(Encrypted Data), 보안 메타데이터(Security Metadata)를 효율적으로 전달할 수 있도록 지원한다.

사람과 함께 작업하는 협동 로봇(Collaborative Robot)은 특히 높은 통신 신뢰성을 요구한다. 분산된 안전 제어기(Safety Controller), 인지 시스템(Perception System), 경로 계획기(Motion Planner), 액추에이터 네트워크(Actuator Network)는 기능 안전(Functional Safety)을 위해 예측 가능한 통신 지연(Latency)과 강력한 오류 검출(Error Detection)이 필요하다. CAN XL은 기존 CAN의 비파괴 비트 단위 중재(Nondestructive Bitwise Arbitration)를 그대로 유지하면서도 고급 안전 진단(Safety Diagnostics), 이벤트 기록(Event Logging), 시스템 관리(System Management)를 위한 충분한 통신 용량을 제공한다.

건설 장비, 광산 차량, 농업 기계, 산림 장비(Forestry Equipment), 군수 물류 플랫폼(Military Logistics Platform), 공항 서비스 차량(Airport Service Vehicle), 항만 자동화 장비(Port Automation Equipment)와 같은 중장비 자율 시스템 역시 CAN XL의 중요한 적용 대상이다. 이러한 시스템은 유압(Hydraulic System), 동력계(Powertrain), 에너지 관리(Energy Management), 환경 인지(Environmental Sensing), 자율주행(Autonomous Navigation), 작업 장치(Payload Control), 예지 정비 등 방대한 데이터를 지속적으로 생성한다. CAN XL은 이러한 구조화된 엔지니어링 데이터를 충분한 대역폭으로 처리하면서도 기존 CAN의 강인성(Robustness)을 그대로 유지한다.

휴머노이드(Humanoid)는 현재 개발되고 있는 로봇 가운데 가장 복잡한 통신 구조를 가진 시스템 중 하나이다. 수십 개 이상의 액추에이터, 힘 센서, 촉각 센서(Tactile Sensor), 스테레오 카메라(Stereo Camera), 깊이 카메라(Depth Camera), LiDAR, 마이크(Microphone), 배터리 관리 시스템(BMS), 분산 모터 제어기, 엣지 AI 컴퓨터, 중앙 계획 시스템(Central Planning System)이 하나의 플랫폼에서 함께 동작한다. 대용량 인지 데이터는 Ethernet을 사용하지만, 설정 데이터베이스, 소프트웨어 배포, 진단 정보, 동기화 데이터, 구조화된 제어기 간 통신은 CAN XL을 활용하는 것이 매우 효율적이다.

사족보행 로봇(Quadruped) 역시 원격 제어 연구 플랫폼에서 자율 검사(Autonomous Inspection), 보안(Security), 산업 유지보수(Industrial Maintenance), 재난 대응(Disaster Response) 시스템으로 발전하면서 통신 구조가 점점 복잡해지고 있다. 보행 제어(Locomotion Control), 인지(Perception), 환경 인식(Environmental Awareness), 임무 관리(Mission Management), 상태 모니터링(Health Monitoring), 페이로드 통합(Payload Integration), 클라우드 연계(Cloud Synchronization)는 실시간 제어와 구조화된 데이터를 동시에 처리해야 한다. CAN XL은 기존 실시간 제어 네트워크를 보완하면서 이러한 요구사항을 효과적으로 지원한다.

게이트웨이(Gateway)는 다양한 통신 기술을 통합하는 핵심 요소가 된다. 미래 로봇은 CAN FD, CAN XL, EtherCAT, Automotive Ethernet, Gigabit Ethernet, USB, PCI Express(PCIe), Wi-Fi, Bluetooth, GNSS 보정 신호(GNSS Correction), 클라우드 통신을 동시에 사용할 가능성이 높다. 지능형 게이트웨이는 프로토콜 변환(Protocol Translation), 데이터 집계(Data Aggregation), 대역폭 최적화(Bandwidth Optimization), 사이버 보안 검사(Security Inspection), 서비스 라우팅(Service Routing), 품질 관리(Quality of Service, QoS), 네트워크 분리(Network Isolation)를 수행한다. CAN XL은 임베디드 제어 영역과 상위 컴퓨팅 시스템 사이에서 구조화된 데이터를 효율적으로 전달하는 중요한 통신 계층이 된다.

미래 로봇 표준(Robotics Standard)은 특정 제조사의 독자적인 통신 방식보다 상호운용성(Interoperability), 모듈화(Modularity), 서비스 지향 통신(Service-Oriented Communication), 그리고 소프트웨어 이식성(Software Portability)을 더욱 중요하게 고려하게 될 것이다. CAN XL은 구조화된 애플리케이션 데이터, 유연한 프로토콜 확장성(Protocol Scalability), 우수한 확장성(Scalability), 다양한 네트워크와의 공존(Coexistence)을 지원하므로 이러한 미래 표준과 매우 잘 부합한다. CAN XL은 기존 산업용 통신을 대체하기보다 이를 보완하면서 더욱 지능적인 로봇 시스템을 구현하는 기반 기술이 될 것이다.

로봇 분야에서 CAN XL의 도입은 점진적으로 이루어질 가능성이 높다. 기존 산업용 로봇, 서보 시스템(Servo System), 배터리 관리 시스템(BMS), 안전 제어기(Safety Controller), 임베디드 제어기(Embedded Controller)는 앞으로도 상당 기간 CAN FD를 계속 사용할 것이다. 반면 대용량 구조화 데이터, 고급 진단, 소프트웨어 생명주기 관리(Software Lifecycle Management), 예지 정비, AI 통합, 플릿 지능(Fleet Intelligence), 소프트웨어 정의 로봇(Software-Defined Robotics)을 요구하는 시스템부터 CAN XL이 우선 적용될 것으로 예상된다. 따라서 CAN, CAN FD, CAN XL, EtherCAT, Ethernet이 함께 사용되는 혼합 통신 구조가 차세대 자율 로봇의 일반적인 형태가 될 가능성이 매우 높다.

CAN XL의 장기적인 전망은 단순한 통신 속도 향상에 그치지 않는다. 이는 로보틱스(Robotics), 인공지능(AI), 클라우드 컴퓨팅(Cloud Computing), 디지털 트윈(Digital Twin), 사이버 보안(Cybersecurity), 예지 정비(Predictive Maintenance), 그리고 소프트웨어 정의 시스템 엔지니어링(Software-Defined System Engineering)을 하나로 연결하는 핵심 기반 기술이다. 미래의 자율 시스템은 지속적인 소프트웨어 업데이트와 서비스 확장을 통해 끊임없이 진화하게 되며, 이를 위해서는 결정적 통신과 대용량 데이터 전송을 동시에 지원하는 네트워크가 필수적이다. CAN XL은 CAN 계열이 가진 높은 신뢰성(Reliability), 강인성(Robustness), 예측 가능성(Predictability)을 그대로 유지하면서 미래 로봇 생태계가 요구하는 유연성과 확장성을 제공하는 가장 현실적인 진화 방향 가운데 하나로 평가된다.
