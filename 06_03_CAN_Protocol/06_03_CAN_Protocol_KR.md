**Volume 06 Automotive Communication**

# Chapter 3. CAN Protocol

## 3.1 CAN 2.0A 2.0B Frame Structure

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

제어기 영역 네트워크(Controller Area Network, CAN)는 자동차 시스템 내부에 분산되어 있는 전자제어장치(Electronic Control Units, ECUs) 간의 신뢰성 높은 실시간 통신(Real-Time Communication)을 제공하기 위해 개발되었다. CAN이 도입되기 이전에는 개별 전자 모듈(Electronic Modules)들이 점대점 배선(Point-to-Point Wiring)으로 직접 연결되었으며, 전자 기능이 증가할수록 배선 복잡성(Wiring Complexity), 차량 중량(Vehicle Weight), 제조 비용(Manufacturing Cost)이 증가하고 유지보수성(Maintainability)은 저하되었다. CAN은 여러 제어기가 하나의 공유 통신 버스(Shared Communication Bus)를 통해 효율적으로 정보를 교환하면서 메시지 중재(Message Arbitration)를 이용한 결정적인 버스 접근(Deterministic Bus Access)을 유지함으로써 이러한 문제를 해결하였다. CAN 2.0 규격(Specification)은 현대 자동차 통신의 기반이 되었으며 이후 산업 자동화(Industrial Automation), 로봇공학(Robotics), 의료 장비(Medical Equipment), 항공우주(Aerospace) 및 다양한 임베디드 제어 시스템(Embedded Control Systems)으로 확대 적용되었다.

CAN 2.0 규격은 CAN 2.0A와 CAN 2.0B라는 두 가지 호환 가능한 프레임 형식(Frame Formats)을 정의한다. CAN 2.0A는 11비트 표준 식별자(Standard Identifier)를 사용하며, CAN 2.0B는 식별자 길이를 29비트로 확장하여 사용 가능한 메시지 주소 공간(Address Space)을 크게 증가시켰다. 두 형식 모두 동일한 물리 계층(Physical Communication Principles), 중재 메커니즘(Arbitration Mechanism), 오류 검출(Error Detection Strategy) 및 전체 프레임 구조(Frame Organization)를 공유한다. 확장 형식을 지원하는 장치는 일반적으로 두 가지 프레임 형식 모두와 통신할 수 있으므로 제조사는 시스템 설계 목표(System Design Objectives)에 따라 호환성(Compatibility), 네트워크 복잡성(Network Complexity) 및 주소 지정 요구사항(Addressing Requirements)의 균형을 선택할 수 있다.

CAN 프레임(CAN Frame)은 정보를 전달하는 동시에 동기화(Synchronization), 중재(Arbitration), 제어(Control), 오류 검출(Error Detection), 확인(Acknowledgement) 및 프레임 구분(Frame Separation)을 함께 수행하도록 설계되어 있다. 이러한 기능들을 별도의 제어 메시지(Control Messages)로 처리하는 통신 프로토콜과 달리, CAN은 모든 필수 통신 기능을 각각의 전송 프레임 안에 직접 포함한다. 그 결과 모든 메시지는 네트워크에 연결된 모든 노드(Node)가 동일한 방식으로 해석할 수 있는 하나의 완전한 통신 트랜잭션(Self-Contained Communication Transaction)이 된다.

통신은 항상 프레임 시작(Start of Frame, SOF) 필드(Field)에서 시작된다. 프레임 시작은 단 하나의 지배 비트(Dominant Bit)로 구성되며 버스에 연결된 모든 노드를 동기화한다. 모든 제어기는 지속적으로 버스 활동(Bus Activity)을 감시하고 있으므로 이 지배 상태로의 전환을 감지하면 나머지 프레임을 해석하기 전에 내부 타이밍(Internal Timing)을 정렬할 수 있다. 정확한 동기화는 CAN 네트워크가 수십 kbps에서 1Mbps에 이르는 다양한 통신 속도(Communication Speeds)와 다양한 케이블 길이(Cable Lengths)에서도 안정적인 통신을 유지하기 위해 매우 중요하다.

프레임 시작 직후에는 중재 필드(Arbitration Field)가 위치하며, 이 필드는 메시지의 식별(Message Identity)과 전송 우선순위(Transmission Priority)를 결정한다. 표준 CAN 2.0A 프레임에서는 이 필드가 11비트 식별자와 원격 전송 요청 비트(Remote Transmission Request, RTR)를 포함한다. 확장 CAN 2.0B 프레임에서는 추가 제어 비트(Control Bits)와 18개의 추가 식별자 비트가 포함되어 전체 식별자가 29비트로 확장된다. 이 식별자는 송신자(Source)나 수신자(Destination)의 주소(Address)를 의미하지 않으며, 전송되는 정보의 의미와 우선순위를 나타낸다. 이를 통해 여러 제어기는 자신이 담당하는 기능과 관련된 메시지만 선택적으로 처리할 수 있다.

중재 메커니즘(Arbitration Mechanism)은 CAN 통신의 가장 혁신적인 특징 중 하나이다. 여러 노드가 유휴 버스(Idle Bus)를 감지한 후 동시에 전송을 시작할 수 있다. 중재 과정에서 각 송신기는 자신이 전송한 비트와 실제 버스에서 관측한 비트를 지속적으로 비교한다. 지배 비트(Dominant Bit)는 항상 열성 비트(Recessive Bit)를 덮어쓰기 때문에 숫자가 가장 작은 식별자(Numerically Lowest Identifier)를 가진 메시지가 중재에서 승리하며 현재 진행 중인 통신은 손상되지 않는다. 중재에서 패배한 노드는 즉시 전송을 중단하고 현재 프레임이 완료된 후 자동으로 재전송을 시도한다.

중재 필드 다음에는 제어 필드(Control Field)가 위치하며 전송되는 프레임의 특성을 정의한다. 이 필드는 데이터 길이 코드(Data Length Code, DLC)와 함께 표준 프레임(Standard Frame)인지 확장 프레임(Extended Frame)인지를 구분하는 제어 비트를 포함한다. 데이터 길이 코드는 데이터 필드(Data Field)에 포함되는 애플리케이션 데이터(Application Data)의 바이트 수(Bytes)를 나타낸다. 클래식 CAN(Classical CAN)은 0바이트부터 8바이트까지의 페이로드(Payload)를 지원하며, 이는 많은 실시간 제어 애플리케이션에서 충분한 데이터 용량을 제공하면서 통신 지연시간(Communication Latency)을 최소화한다.

데이터 필드(Data Field)는 전자제어장치(Electronic Control Units) 간에 실제 애플리케이션 정보를 전달하는 부분이다. 애플리케이션에 따라 이 데이터는 센서 측정값(Sensor Measurements), 액추에이터 명령(Actuator Commands), 진단 정보(Diagnostic Information), 시스템 상태(System Status), 제어 파라미터(Control Parameters), 교정값(Calibration Values) 또는 동기화 신호(Synchronization Signals)를 포함할 수 있다. CAN은 페이로드를 애플리케이션과 독립적인 데이터(Application-Independent Data)로 취급하기 때문에 동일한 통신 메커니즘을 수정 없이 다양한 제어 시스템에서 사용할 수 있다.

데이터 필드 뒤에는 순환 중복 검사(Cyclic Redundancy Check, CRC) 필드가 위치하며 전송 무결성(Transmission Integrity)을 검증한다. 송신기는 프레임 내 보호 대상 비트(Protected Bits)를 기반으로 CRC 값을 계산하여 메시지 끝에 추가한다. 모든 수신 노드는 동일한 계산을 독립적으로 수행하고 계산 결과를 수신한 CRC 값과 비교한다. 값이 일치하지 않으면 전송 오류(Transmission Errors)가 발생한 것으로 판단하여 해당 프레임을 폐기하고 필요 시 자동으로 재전송한다. 이러한 분산 검증(Distributed Verification) 방식은 전기적 잡음(Electrically Noisy Environments)이 존재하는 환경에서도 매우 높은 통신 신뢰성을 제공한다.

확인 필드(Acknowledgement Field)는 하나 이상의 수신기가 전송된 프레임을 정상적으로 수신했음을 즉시 확인하는 기능을 수행한다. 이 구간에서 송신기는 열성 비트(Recessive Bit)를 전송하고, 정상적으로 수신한 모든 노드는 이를 지배 비트(Dominant Bit)로 덮어쓴다. 만약 지배 확인 비트(Dominant Acknowledgement)가 감지되지 않으면 송신기는 어떤 수신기도 메시지를 정상적으로 수신하지 못했다고 판단하고 자동으로 재전송을 예약한다. 이 메커니즘은 복잡한 상위 계층 확인 절차(Higher-Layer Confirmation Procedures) 없이도 통신 실패를 신속하게 감지할 수 있도록 한다.

프레임 종료(End of Frame, EOF)는 여러 개의 열성 비트(Recessive Bits)를 사용하여 메시지 전송의 종료를 표시한다. 이 비트들은 모든 제어기가 현재 통신이 종료되었음을 인식하고 다음 프레임이 시작되기 전에 준비할 수 있도록 한다. 프레임 종료 이후에는 프레임 간 간격(Interframe Space)이 존재하며, 연속적인 메시지 사이에 필요한 준비 시간을 제공함으로써 지속적인 통신 환경에서도 질서 있는 버스 동작(Orderly Bus Operation)을 유지한다.

CAN 프레임 구조(Frame Structure)의 가장 큰 특징 중 하나는 메시지가 물리적인 장치 주소(Physical Device Addresses)가 아니라 데이터의 내용(Content)에 의해 식별된다는 점이다. 모든 제어기는 모든 전송 프레임을 수신하고, 해당 식별자가 자신의 수신 필터(Acceptance Filters)와 일치하는지를 독립적으로 판단한다. 필요하지 않은 메시지는 각 제어기에서 자체적으로 폐기되며 전체 네트워크 통신에는 아무런 영향을 주지 않는다. 이러한 브로드캐스트 통신 모델(Broadcast Communication Model)은 기존 송신기를 수정하지 않고도 새로운 장치를 추가할 수 있으므로 분산 제어 아키텍처(Distributed Control Architectures)를 크게 단순화한다.

CAN 2.0A는 11비트 식별자를 사용하는 표준 프레임 형식(Standard Frame Format)을 사용한다. 이 방식은 총 2,048개의 식별자를 지원하며 현재도 자동차 바디 전자장치(Automotive Body Electronics), 산업용 제어기(Industrial Controllers), 임베디드 장비(Embedded Equipment) 및 로봇 서브시스템(Robotic Subsystems)에서 널리 사용되고 있다. 비교적 짧은 중재 필드(Arbitration Field)는 매 메시지마다 전송되는 식별자 비트 수를 줄여 통신 효율(Communication Efficiency)을 향상시킨다. 주소 공간 요구사항이 크지 않은 시스템에서는 대역폭 활용도(Bandwidth Utilization)를 극대화하기 위해 표준 형식을 선호하는 경우가 많다.

CAN 2.0B는 식별자 길이를 29비트로 확장하여 5억 3천6백만 개 이상의 고유 식별자(Unique Identifiers)를 지원한다. 이처럼 확장된 주소 공간은 수많은 기능 모듈(Functional Modules), 게이트웨이(Gateways), 진단 서비스(Diagnostic Services) 및 통신 도메인(Communication Domains)을 포함하는 대규모 분산 시스템(Large Distributed Systems)에 적합하다. 대형 상용차(Heavy Commercial Vehicles), 농업 기계(Agricultural Machinery), 산업 자동화 시스템(Industrial Automation Systems), 선박 전자장치(Marine Electronics) 및 고급 로봇 플랫폼(Advanced Robotic Platforms)은 이러한 확장 CAN 통신(Extended CAN Communication)의 장점을 적극 활용한다.

두 프레임 형식은 동일한 네트워크에서 함께 사용할 수 있지만, 중재 과정은 항상 결정적인 통신(Deterministic Communication)을 유지한다. 동일한 기본 식별자(Base Identifier)를 공유하는 경우에는 식별자 확장 비트(Identifier Extension Bit)의 중재 규칙에 의해 표준 식별자(Standard Identifier)가 확장 식별자(Extended Identifier)보다 우선순위를 가진다. 따라서 시스템 설계자는 어떤 프레임 형식을 사용하더라도 예측 가능한 통신 우선순위(Communication Priorities)를 유지할 수 있도록 식별자 값을 신중하게 할당해야 한다.

원격 프레임(Remote Frames)은 CAN 2.0에서 정의된 또 하나의 통신 기능이다. 원격 프레임은 데이터를 포함하지 않으며 동일한 식별자를 가진 데이터 프레임(Data Frame)의 전송을 요청하는 역할을 수행한다. 과거에는 하나의 제어기가 별도의 프로토콜 명령 없이 다른 제어기에 최신 정보를 요청하는 데 사용되었다. 그러나 현대 자동차 시스템에서는 주기적 전송 스케줄(Periodic Transmission Schedules)이나 상위 계층 통신 프로토콜(Higher-Layer Communication Protocols)을 선호하기 때문에 원격 프레임은 현재 거의 사용되지 않는다.

오류 프레임(Error Frames)은 CAN의 높은 신뢰성을 보장하는 핵심 요소이다. 제어기가 비트 모니터링(Bit Monitoring), CRC 검증(CRC Verification), 프레임 형식 검사(Frame Format Checking), 확인 비트 감시(Acknowledgement Monitoring) 또는 비트 스터핑(Bit Stuffing) 검사를 통해 통신 오류를 감지하면 즉시 오류 프레임을 전송한다. 이 과정은 현재 프레임을 의도적으로 무효화하여 모든 노드가 오류 발생을 인식하도록 한다. 이후 원래의 송신기는 프로토콜이 정의한 자동 오류 복구 절차(Automatic Error Recovery Procedures)에 따라 다시 메시지를 전송하여 전체 네트워크의 데이터 일관성(Data Consistency)을 유지한다.

비트 스터핑(Bit Stuffing)은 통신 신뢰성을 더욱 향상시키는 중요한 기법이다. CAN은 동기화를 유지하기 위해 동일한 극성(Polarity)의 비트가 연속으로 다섯 개 나타나면 자동으로 반대 극성의 비트를 하나 삽입한다. 모든 수신기는 실제 메시지를 처리하기 전에 이러한 삽입 비트를 제거한다. 만약 비트 스터핑 규칙이 위반되면 즉시 통신 오류로 판단되므로 추가적인 오류 검출 기능(Error Detection Mechanism)과 동시에 분산 제어기 간의 클록 동기화(Clock Synchronization)도 유지할 수 있다.

프레임 효율(Frame Efficiency)은 애플리케이션 페이로드(Application Payload)뿐만 아니라 여러 요소에 의해 결정된다. 중재 비트(Arbitration Bits), 제어 정보(Control Information), CRC, 확인 필드(Acknowledgement), 비트 스터핑(Bit Stuffing) 및 프레임 간 간격(Interframe Space)은 모두 통신 오버헤드(Communication Overhead)를 구성한다. 따라서 엔지니어는 실시간 통신 스케줄(Real-Time Communication Schedules)을 설계할 때 페이로드 효율(Payload Efficiency)과 전체 버스 사용률(Bus Utilization)을 함께 고려해야 한다. 메시지 전송 주기(Message Frequency), 데이터 그룹화(Payload Grouping), 식별자 할당(Identifier Allocation) 및 네트워크 부하(Network Loading)를 최적화하면 불필요한 대역폭 소비를 줄이면서 예측 가능한 지연시간(Predictable Latency)을 유지할 수 있다.

프레임 식별자(Frame Identifiers)는 네트워크 전체의 통신 우선순위 계층(Communication Priority Hierarchy)을 결정한다. 제동 시스템(Braking Systems), 조향 제어(Steering Control), 모터 제어(Motor Control), 비상 기능(Emergency Functions) 및 동기화(Synchronization)와 관련된 메시지는 일반적으로 더 작은 숫자의 식별자를 사용하여 중재에서 우선권을 갖도록 한다. 반대로 진단(Diagnostics), 구성 변경(Configuration Updates), 환경 모니터링(Environmental Monitoring) 및 유지보수 상태(Maintenance Status)와 같은 덜 긴급한 정보는 상대적으로 낮은 우선순위를 갖는 식별자가 할당된다. 적절한 식별자 설계는 높은 통신 부하에서도 시스템 응답성(System Responsiveness)에 직접적인 영향을 미친다.

로봇 시스템에서는 CAN 프레임 구조(Frame Structure)를 이용하여 분산형 모터 제어기(Distributed Motor Controllers), 배터리 관리 시스템(Battery Management Systems), 관성 센서(Inertial Sensors), 안전 제어기(Safety Controllers), 유압 모듈(Hydraulic Modules), 조향 시스템(Steering Systems), 환경 모니터링 장치(Environmental Monitoring Devices) 및 상위 제어기(Supervisory Controllers)가 서로 통신한다. 모든 서브시스템은 애플리케이션 목적과 관계없이 동일한 표준 프레임 구조(Standardized Frame Organization)를 사용하여 정보를 교환한다. 이러한 공통 통신 모델(Common Communication Model)은 소프트웨어 개발, 하드웨어 통합, 시험(Test), 진단(Diagnostics) 및 장기 유지보수(Long-Term Maintenance)를 단순화하며 서로 다른 제조사의 장치도 하나의 통합된 로봇 통신 아키텍처(Unified Robotic Communication Architecture) 안에서 함께 사용할 수 있도록 한다.

CAN 2.0A에서 CAN 2.0B로의 발전은 통신 프로토콜이 기존 호환성(Backward Compatibility)과 결정적인 동작(Deterministic Operation)을 유지하면서도 점점 더 복잡한 분산 시스템을 지원하도록 어떻게 확장되는지를 잘 보여준다. 이후 등장한 CAN FD와 같은 기술은 데이터 용량(Payload Capacity)과 통신 성능(Communication Performance)을 더욱 향상시켰지만, CAN 2.0이 확립한 프레임 구조(Frame Organization)는 동기화(Synchronization), 중재(Arbitration), 오류 검출(Error Detection), 확인(Acknowledgement) 및 신뢰성 높은 실시간 통신(Reliable Real-Time Communication)의 핵심 원리를 정의하고 있으며, 이러한 원리는 오늘날의 자동차 및 로봇 제어 시스템에서도 여전히 가장 중요한 기반 기술로 활용되고 있다.

## 3.2 Bit Timing and Synchronization

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

비트 타이밍(Bit Timing)은 제어기 영역 네트워크(Controller Area Network, CAN) 통신에서 가장 기본적인 개념 가운데 하나이며, 공유 통신 버스(Shared Communication Bus)에서 송신 노드(Transmitting Node)와 수신 노드(Receiving Node)가 각각의 비트(Bit)를 어떻게 해석하는지를 결정한다. 각 장치가 독립적으로 동작하는 비동기 통신(Asynchronous Communication)과 달리, CAN은 모든 제어기(Controller)가 메시지 전송(Message Transmission) 동안 거의 동일한 타이밍(Timing)을 유지해야 한다. 정확한 동기화(Synchronization)는 발진기(Oscillator) 주파수, 신호 전파 지연(Signal Propagation Delay), 케이블 길이(Cable Length), 환경 조건(Environmental Conditions)의 차이가 존재하더라도 모든 전자제어장치(Electronic Control Units, ECUs)가 동일한 비트 경계(Bit Boundaries)를 인식하도록 보장한다. 이러한 정확한 타이밍 유지 능력은 전기적 잡음(Electrical Noise)이 많은 환경에서도 CAN 네트워크가 매우 높은 신뢰성을 갖는 결정적 통신(Deterministic Communication)을 실현하도록 한다.

모든 CAN 제어기(CAN Controller)는 수정 발진기(Crystal Oscillator) 또는 기타 타이밍 소스(Timing Source)에 의해 생성되는 내부 클록(Internal Clock)을 가지고 있다. 이러한 발진기는 매우 높은 정확도를 제공하지만, 어느 두 장치도 완전히 동일한 주파수를 생성하지는 않는다. 제조 공차(Manufacturing Tolerances), 온도 변화(Temperature Variation), 전원 전압 변동(Supply Voltage Fluctuations), 부품 노화(Component Aging)는 제어기 간에 작은 주파수 차이를 발생시킨다. 효과적인 동기화 메커니즘(Synchronization Mechanism)이 없다면 이러한 작은 타이밍 오차는 점차 누적되어 결국 수신기가 전송된 비트를 잘못 샘플링(Sampling)하게 된다. CAN은 통신 중 적절한 동기화 이벤트(Synchronization Events)가 발생할 때마다 내부 타이밍을 지속적으로 조정함으로써 이러한 문제를 해결한다.

CAN 통신 타이밍의 가장 작은 단위는 시간 양자(Time Quantum, TQ)이다. 시간 양자는 CAN 제어기가 각각의 통신 비트(Communication Bit)를 구성하기 위해 사용하는 최소 프로그래밍 가능한 시간 간격(Minimum Programmable Time Interval)을 의미한다. CAN은 하나의 비트를 고정된 물리적 시간으로 정의하지 않고 여러 개의 시간 양자로 분할한다. 이를 통해 엔지니어는 네트워크 길이(Network Length), 통신 속도(Communication Speed), 발진기 주파수(Oscillator Frequency) 및 하드웨어 특성(Hardware Characteristics)에 맞추어 비트 타이밍을 구성할 수 있다. 이러한 유연한 타이밍 모델은 기본 프로토콜을 변경하지 않고도 다양한 임베디드 시스템(Embedded Systems)에서 안정적인 CAN 통신을 가능하게 한다.

각각의 CAN 비트는 전체 비트 기간(Bit Period)을 구성하는 여러 개의 논리적 타이밍 구간(Logical Timing Segments)으로 이루어진다. 이러한 구간은 동기화 구간(Synchronization Segment), 전파 구간(Propagation Segment), 위상 구간 1(Phase Segment 1), 위상 구간 2(Phase Segment 2)로 구성된다. 각 구간은 동기화 조정(Synchronization Adjustment), 전파 지연 보상(Propagation Delay Compensation), 샘플링 정확도(Sampling Accuracy)를 하나의 비트 구간 안에서 동시에 수행하도록 설계되어 있다. 이러한 구간을 적절히 설정하는 것이 다양한 물리적 설치 환경에서 CAN 네트워크가 안정적으로 동작할 수 있는지를 결정한다.

동기화 구간(Synchronization Segment)은 항상 하나의 시간 양자(Time Quantum)를 차지하며 모든 통신 비트의 시작을 나타낸다. 이 구간은 버스에서 신호 전환(Signal Transition)이 발생할 때 모든 참여 노드를 동기화한다. CAN 통신은 지배 비트(Dominant Bit)와 열성 비트(Recessive Bit) 사이의 전환을 감지하여 동작하므로 이러한 에지(Edge)는 통신 전반에 걸쳐 자연스러운 동기화 기준(Synchronization Opportunities)을 제공한다. 모든 제어기는 이러한 신호 변화를 지속적으로 감시하며 필요할 경우 내부 타이밍을 조정하여 송신 노드와의 동기화를 유지한다.

동기화 구간 다음에는 전파 구간(Propagation Segment)이 위치하며 네트워크 전체에서 발생하는 물리적 신호 지연(Physical Signal Delays)을 보상한다. 전기 신호(Electrical Signals)는 통신 케이블(Communication Cables), 커넥터(Connectors), 트랜시버(Transceivers), 인쇄회로기판 배선(Printed Circuit Board Traces)을 따라 이동하는 데 실제 시간이 필요하다. 네트워크 길이가 증가할수록 전파 지연도 함께 증가한다. 전파 구간은 수신기가 비트를 샘플링하기 전에 전송된 신호가 모든 노드에 도달할 수 있도록 충분한 시간을 제공한다. 특히 대규모 산업용 네트워크(Industrial Networks), 상용차(Commercial Vehicles) 및 긴 통신 케이블을 사용하는 분산형 로봇 플랫폼(Distributed Robotic Platforms)에서는 적절한 설정이 매우 중요하다.

위상 구간 1(Phase Segment 1)은 샘플링 지점(Sampling Point) 이전에 추가적인 타이밍 유연성을 제공한다. 이 구간에서는 통신 중인 제어기 간의 클록 차이(Clock Differences)를 보상하기 위해 필요 시 현재 비트 길이를 연장하는 동기화 조정이 수행될 수 있다. 이러한 조정 과정을 재동기화(Resynchronization)라고 하며, 통신을 중단하지 않고도 모든 수신기가 점진적으로 타이밍 오차를 수정할 수 있도록 한다. 위상 구간 1을 조정할 수 있는 기능은 실제 운용 환경에서 CAN 통신이 매우 높은 견고성(Robustness)을 유지하는 중요한 이유 가운데 하나이다.

위상 구간 2(Phase Segment 2)는 샘플링 지점 이후에 위치하며 통신 비트의 나머지 부분을 구성한다. 이 구간 역시 위상 구간 1과 마찬가지로 동기화 조정에 참여할 수 있지만, 주요 역할은 다음 비트가 시작되기 전까지 충분한 타이밍 여유(Timing Margin)를 확보하는 것이다. 두 개의 위상 구간은 함께 발진기 드리프트(Oscillator Drift)를 보상하면서도 수천 개의 연속 비트가 전송되는 긴 통신 과정에서도 정확한 동기화를 유지하도록 지원한다.

샘플링 지점(Sampling Point)은 모든 수신 제어기가 현재 버스의 상태(Bus Level)를 지배 비트인지 열성 비트인지 판단하는 정확한 순간이다. 일반적으로 이 시점은 전체 비트 길이의 약 70%에서 85% 사이에 위치하지만, 정확한 위치는 네트워크 구성(Network Configuration)에 따라 달라진다. 적절한 샘플링 지점을 선택하면 전파 지연 보상과 타이밍 여유를 균형 있게 확보할 수 있어 다양한 케이블 길이와 운용 환경에서도 안정적인 통신이 가능해진다.

발진기 허용오차(Oscillator Tolerance)는 허용 가능한 통신 성능에 직접적인 영향을 미친다. 모든 제어기의 내부 발진기는 기준 주파수(Nominal Frequency)에서 약간씩 벗어나며, 이러한 누적된 주파수 차이가 동기화 이벤트 사이에서 타이밍 오차가 얼마나 빠르게 증가하는지를 결정한다. 높은 통신 속도나 긴 케이블을 사용하는 네트워크는 누적 오차를 허용할 수 있는 여유가 적기 때문에 더욱 정밀한 발진기를 필요로 한다. 따라서 적절한 발진기 규격을 선택하는 것은 CAN 하드웨어 설계에서 매우 중요한 요소이다.

강제 동기화(Hard Synchronization)는 버스가 이전까지 유휴 상태(Idle State)였을 때 제어기가 프레임 시작(Start of Frame, SOF)의 지배 에지(Dominant Edge)를 감지하면서 발생한다. 이 과정에서 모든 수신 제어기는 내부 비트 타이밍을 즉시 초기화하여 새로운 프레임이 완전히 동기화된 상태에서 시작되도록 한다. 강제 동기화는 전체 통신 프레임의 기준 타이밍을 설정하며 이후 수행되는 모든 재동기화의 기준이 된다.

재동기화(Resynchronization)는 통신이 시작된 이후에도 지속적으로 동기화를 유지하는 과정이다. 예상보다 조금 빠르거나 늦게 신호 전환이 발생하면 제어기는 위상 오차(Phase Error)를 계산하여 내부 타이밍을 자동으로 조정한다. 통신을 다시 시작하는 대신 적절한 위상 구간을 사전에 정의된 범위 안에서 늘리거나 줄이는 방식으로 보정이 이루어진다. 이러한 작은 조정은 메시지 전송 과정 전체에서 사용자에게 보이지 않는 형태로 지속적으로 수행되며, 발진기 오차가 존재하더라도 안정적인 통신을 가능하게 한다.

재동기화 과정에서 허용되는 최대 조정량은 동기화 점프 폭(Synchronization Jump Width, SJW)에 의해 결정된다. 이 설정값은 각각의 동기화 이벤트 이후 적용할 수 있는 최대 타이밍 보정 범위를 제한한다. 조정 폭을 제한함으로써 과도한 타이밍 변화로 인해 통신이 불안정해지는 것을 방지하면서도 정상적인 발진기 드리프트를 보상할 수 있는 충분한 유연성을 확보할 수 있다. 엔지니어는 네트워크 속도(Network Speed), 발진기 품질(Oscillator Quality), 예상 운용 환경에 따라 적절한 SJW 값을 선택한다.

비트 타이밍(Bit Timing)을 구성할 때에는 여러 설계 조건(Design Constraints)을 동시에 고려해야 한다. 높은 통신 속도는 하나의 비트 길이를 짧게 만들므로 전파 지연 보상과 동기화 조정을 수행할 수 있는 시간이 줄어든다. 긴 통신 케이블은 전파 지연을 증가시키므로 더 큰 타이밍 여유를 필요로 한다. 발진기 정확도(Oscillator Accuracy)는 허용 가능한 동기화 오차를 결정하며, 네트워크 토폴로지(Network Topology)는 전체 신호 분포(Signal Distribution)에 영향을 준다. 따라서 성공적인 CAN 시스템 설계는 개별 파라미터를 독립적으로 조정하는 것이 아니라 모든 타이밍 요소를 함께 최적화하는 과정이다.

통신 속도(Communication Speed)는 비트 타이밍 구성에 큰 영향을 미친다. 수십 kbps 수준의 저속 CAN 네트워크는 충분한 타이밍 여유를 제공하므로 긴 통신 거리와 비교적 저렴한 발진기를 사용할 수 있다. 반면 1Mbps에 가까운 고속 네트워크에서는 하나의 비트가 매우 짧은 시간만 지속되므로 훨씬 더 정밀한 타이밍이 요구된다. 따라서 고속 자동차 제어 시스템은 일반적으로 짧은 통신 케이블과 정밀한 하드웨어를 함께 사용한다.

신호 전파 지연(Signal Propagation Delay)은 여러 요소가 합쳐져 발생한다. 전기 신호는 통신 케이블, 트랜시버 회로(Transceiver Circuits), 커넥터 인터페이스(Connector Interfaces), 인쇄회로기판(Printed Circuit Boards), 내부 제어기 논리(Internal Controller Logic)를 통과하면서 각각 작은 지연을 발생시킨다. 이러한 지연이 모두 합쳐져 전체 네트워크의 전파 시간을 결정한다. 엔지니어는 네트워크 설계 과정에서 이러한 지연을 계산하여 전파 구간이 충분한 타이밍 여유를 제공하도록 설정한다.

버스 길이(Bus Length)와 통신 속도는 서로 반비례 관계(Inverse Relationship)를 가진다. 통신 속도가 증가할수록 전파 지연이 하나의 비트에서 차지하는 비율이 커지므로 사용할 수 있는 최대 케이블 길이는 감소한다. 반대로 통신 속도를 낮추면 더 긴 통신 거리를 확보할 수 있다. 따라서 자동차 바디 전자장치(Automotive Body Electronics), 산업 자동화 시스템(Industrial Automation Systems), 이동형 로봇(Mobile Robots)은 각각의 물리적 네트워크 크기와 성능 요구사항에 맞는 통신 속도를 선택한다.

적절한 비트 타이밍은 통신 신뢰성(Communication Reliability), 지연시간(Latency), 오류율(Error Rate), 전자파 적합성(Electromagnetic Compatibility, EMC)에 직접적인 영향을 미친다. 타이밍이 잘못 설정되면 CRC 오류(CRC Errors), 확인 오류(Acknowledgement Failures), 중재 실패(Arbitration Loss), 프레임 오류(Frame Errors) 또는 전체 통신 불안정(Communication Instability)이 발생할 수 있다. 실제로 소프트웨어 결함으로 오인되는 많은 통신 문제는 잘못 구성된 타이밍 파라미터에서 비롯된다. 따라서 오실로스코프(Oscilloscope), CAN 분석기(CAN Analyzer), 프로토콜 모니터링 장비(Protocol Monitoring Equipment)를 이용한 철저한 검증은 네트워크 구축과 문제 해결 과정에서 필수적인 절차이다.

현대의 CAN 제어기는 비트 타이밍을 자동으로 계산하는 다양한 설정 도구(Configuration Tools)를 제공한다. 엔지니어는 발진기 주파수, 원하는 통신 속도 및 하드웨어 특성을 입력하면 소프트웨어가 적절한 타이밍 파라미터를 자동으로 추천한다. 이러한 도구는 설정 과정을 크게 단순화하지만, 숙련된 엔지니어는 샘플링 지점, 전파 여유, 발진기 허용오차 및 동기화 성능을 직접 검증하여 예상되는 모든 운용 환경에서도 안정적으로 동작하는지를 확인한다.

로봇 시스템에서는 정확한 비트 타이밍이 모터 제어기(Motor Controllers), 배터리 관리 시스템(Battery Management Systems), 안전 제어기(Safety Controllers), 관성측정장치(Inertial Measurement Units, IMUs), 조향 모듈(Steering Modules), 유압 제어기(Hydraulic Controllers), 환경 센서(Environmental Sensors), 상위 제어 컴퓨터(Supervisory Computers) 간의 안정적인 통신을 보장한다. 이러한 분산 장치들은 진동(Vibration), 온도 변화(Temperature Variation), 전기적 잡음(Electrical Noise), 동적인 기계 부하(Dynamic Mechanical Loading) 환경에서 동작하는 경우가 많다. 적절한 동기화는 모든 서브시스템이 결정적인 실시간 정보(Deterministic Real-Time Information)를 교환하도록 하여 협조 제어(Coordinated Motion), 기능 안전(Functional Safety) 및 안정적인 자율 운용(Stable Autonomous Operation)을 가능하게 한다.

CAN이 정의한 비트 타이밍 구조(Bit Timing Architecture)는 완벽하지 않은 발진기를 사용하는 독립적인 제어기들이 하나의 공유 통신 매체(Shared Communication Media)를 통해 매우 높은 신뢰성으로 통신할 수 있도록 해 주는 가장 뛰어난 기술적 성과 가운데 하나이다. 시간 양자(Time Quantum), 동기화 구간(Synchronization Segments), 샘플링 지점 제어(Sampling Point Control), 전파 지연 보상(Propagation Delay Compensation), 재동기화(Resynchronization), 정교한 타이밍 조정(Timing Adjustments)을 통해 CAN은 수십 년 동안 자동차 및 산업용 임베디드 시스템에서 사용되어 온 결정적인 통신 성능을 제공해 왔으며, 오늘날에도 CAN FD와 같은 최신 통신 기술의 핵심 기반으로 계속 활용되고 있다.

## 3.3 Error Detection and Handling

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

신뢰성 있는 통신(Reliable Communication)은 제어기 영역 네트워크(Controller Area Network, CAN) 프로토콜이 처음부터 추구해 온 가장 중요한 설계 목표 가운데 하나이다. 자동차 제어 시스템(Automotive Control Systems)은 제동(Braking), 조향(Steering), 파워트레인(Powertrain), 배터리 관리(Battery Management) 및 다양한 차량 기능(Vehicle Functions)에 직접적인 영향을 미치는 안전 필수 정보(Safety-Critical Information)를 지속적으로 교환한다. 하나의 손상된 메시지(Corrupted Message)라도 잘못된 제어 판단을 유발하여 심각한 결과를 초래할 수 있다. CAN은 통신 오류를 상위 소프트웨어 계층(Higher Software Layers)에 의존하여 검출하는 대신, 포괄적인 오류 검출(Error Detection) 및 오류 복구(Error Recovery) 메커니즘을 프로토콜 자체에 내장하였다. 모든 노드는 통신 무결성(Communication Integrity)을 유지하는 과정에 능동적으로 참여하며, 이러한 구조는 CAN을 임베디드 시스템(Embedded Systems)에서 가장 높은 내고장성(Fault Tolerance)을 제공하는 필드버스(Fieldbus) 기술 가운데 하나로 만들어 주었다.

CAN은 전자기 간섭(Electromagnetic Interference), 전기적 잡음(Electrical Noise), 전압 변동(Voltage Fluctuations), 케이블 손상(Cable Damage), 커넥터 불량(Faulty Connectors), 트랜시버 고장(Defective Transceivers), 발진기 오차(Oscillator Inaccuracies), 하드웨어 고장(Hardware Failures) 등으로 인해 통신 오류가 발생할 수 있다고 가정한다. 모든 외란을 완전히 제거하려고 하기보다는 통신 품질(Communication Quality)을 지속적으로 감시하고 전송 무결성(Transmission Integrity)이 손상되는 즉시 이상 동작(Abnormal Behavior)을 검출한다. 오류가 감지되면 현재 통신은 즉시 종료되고, 모든 참여 노드에 오류가 통보되며, 애플리케이션 소프트웨어의 개입 없이 자동 재전송(Automatic Retransmission)이 수행된다.

CAN의 가장 중요한 특징 가운데 하나는 모든 송신 제어기(Transmitting Controller)와 수신 제어기(Receiving Controller)가 통신 버스(Communication Bus)를 지속적으로 감시한다는 점이다. 송신 노드는 단순히 데이터를 전송하는 것에 그치지 않고, 실제 버스 상태(Bus Level)를 동시에 관찰하여 자신이 전송한 비트가 물리적 네트워크(Physical Network)에 정확하게 반영되는지를 확인한다. 수신기 역시 프레임 형식(Frame Format), 타이밍 일관성(Timing Consistency), 순환 중복 검사(Cyclic Redundancy Check, CRC), 확인(Acknowledgement) 동작 및 프로토콜 규칙(Protocol Rules)을 독립적으로 검증한다. 이러한 분산 감시 구조(Distributed Monitoring Architecture)는 통신 신뢰성이 특정 제어기나 중앙 감독 장치(Centralized Supervisory Device)에 의존하지 않도록 한다.

CAN은 메시지 전송 과정 전체에서 동시에 동작하는 여러 개의 독립적인 오류 검출 메커니즘(Independent Error Detection Mechanisms)을 사용한다. 이러한 메커니즘에는 비트 모니터링(Bit Monitoring), 비트 스터핑 검증(Bit Stuffing Verification), CRC 검증(Cyclic Redundancy Check Verification), 프레임 검사(Frame Check), 확인 검사(Acknowledgement Check)가 포함된다. 각각의 메커니즘은 서로 다른 관점에서 통신 무결성을 평가하며 다양한 오류 형태(Failure Modes)에 대해 중복된 보호 기능(Redundant Protection)을 제공한다. 여러 기법이 동시에 동작하기 때문에 검출되지 않는 통신 오류의 발생 확률은 극히 낮으며, 이는 CAN 기반 제어 시스템의 뛰어난 신뢰성을 실현하는 중요한 요소가 된다.

비트 모니터링(Bit Monitoring)은 가장 단순하면서도 매우 효과적인 오류 검출 기법 가운데 하나이다. 전송 과정에서 모든 송신 제어기는 자신이 전송한 각각의 비트와 실제 통신 버스에서 관측되는 전기적 신호(Electrical Level)를 비교한다. 정상적인 경우 두 값은 항상 동일해야 한다. 송신기가 열성 비트(Recessive Bit)를 전송했는데 지배 비트(Dominant Bit)를 감지하거나, 중재 규칙(Arbitration Rules)을 제외한 상황에서 지배 비트를 전송했는데 예상치 못한 열성 비트를 감지하면 즉시 비트 오류(Bit Error)가 선언된다. 이러한 지속적인 자기 검증(Self-Verification)은 단 하나의 비트 전송만으로도 오류를 검출할 수 있도록 한다.

비트 스터핑 검증(Bit Stuffing Verification)은 또 하나의 독립적인 통신 보호 계층(Communication Protection Layer)을 제공한다. CAN은 분산 제어기 간의 동기화를 유지하기 위해 동일한 극성의 비트가 연속 다섯 개 나타날 경우 자동으로 반대 극성의 비트를 하나 삽입한다. 모든 수신기는 실제 메시지를 처리하기 전에 이러한 삽입 비트를 제거한다. 만약 스터핑이 이루어져야 하는 위치에서 동일한 극성의 비트가 다섯 개를 초과하여 연속으로 나타나면 모든 제어기는 즉시 스터프 오류(Stuff Error)를 감지한다. 비트 스터핑은 거의 모든 프레임에서 사용되므로 이 메커니즘은 네트워크 동작 전반에 걸쳐 동기화와 전송 무결성을 지속적으로 검증한다.

순환 중복 검사(Cyclic Redundancy Check, CRC)는 수학적 오류 검출 알고리즘(Mathematical Error Detection Algorithms)을 이용하여 데이터 손상을 검출한다. 송신기는 전송 전에 프레임의 보호 대상 영역(Protected Portions)을 이용하여 CRC 값을 계산하고 이를 메시지에 추가한다. 모든 수신기는 동일한 계산을 독립적으로 수행하여 자신의 계산 결과와 수신된 CRC 값을 비교한다. 계산 결과가 일치하지 않으면 즉시 CRC 오류(CRC Error)가 선언된다. 이 기법은 임의의 비트 손상(Random Bit Corruption)을 매우 높은 확률로 검출하며 디지털 통신 시스템(Digital Communication Systems)에서 가장 강력한 오류 검출 방법 가운데 하나로 널리 사용된다.

프레임 검사(Frame Check)는 모든 통신 프레임이 프로토콜에서 정의한 구조를 정확하게 따르는지를 검증한다. CAN은 프레임 시작(Start of Frame), 중재 필드(Arbitration Field), 제어 필드(Control Field), 데이터 필드(Data Field), CRC 필드(CRC Field), 확인 필드(Acknowledgement Field), 프레임 종료(End of Frame), 프레임 간 간격(Interframe Space)의 위치와 형식을 엄격하게 규정한다. 모든 수신 제어기는 이러한 필드가 올바른 순서와 형식으로 나타나는지를 지속적으로 확인한다. 프레임 구조가 규격과 다를 경우 즉시 형식 오류(Form Error)가 발생하며 잘못된 구조의 메시지가 네트워크 전체로 전달되는 것을 방지한다.

확인 검사(Acknowledgement Check)는 전송된 메시지가 통신 버스에 연결된 최소 하나 이상의 노드에 의해 성공적으로 수신되었는지를 확인한다. 확인 슬롯(Acknowledgement Slot)에서 송신기는 열성 비트를 전송하며 하나 이상의 수신기가 이를 지배 비트로 덮어쓰기를 기대한다. 만약 어떠한 확인 비트도 감지되지 않으면 송신기는 어떤 수신기도 메시지를 정상적으로 수신하지 못한 것으로 판단한다. 메시지 내용 자체에는 문제가 없더라도 확인이 이루어지지 않으면 신뢰성 있는 정보 전달을 보장하기 위해 자동 재전송이 수행된다.

어떤 제어기라도 앞에서 설명한 감시 메커니즘을 통해 오류를 감지하면 즉시 오류 프레임(Error Frame)을 생성한다. 일반적인 데이터 프레임(Data Frame)과 달리 오류 프레임은 의도적으로 정상적인 통신 규칙을 위반하여 모든 연결된 제어기가 현재 전송이 유효하지 않음을 즉시 인식하도록 만든다. 이러한 의도적인 통신 중단은 손상된 정보가 어떤 수신기에도 받아들여지지 않도록 하며, 모든 제어기가 동일한 통신 상태를 유지하도록 보장한다.

오류 프레임(Error Frame)은 주로 오류 플래그(Error Flag)와 오류 구분자(Error Delimiter)로 구성된다. 오류 플래그 구간에서는 하나 이상의 지배 비트가 의도적으로 현재 통신을 방해하여 손상된 프레임이 수신되기 전에 폐기되도록 만든다. 이어지는 오류 구분자는 정상적인 통신 상태를 복구하여 네트워크가 자동 재전송을 준비할 수 있도록 한다. 이러한 체계적인 복구 절차는 CAN 네트워크가 일시적인 통신 장애(Transient Communication Disturbances)로부터 매우 빠르게 회복할 수 있도록 지원한다.

오류 프레임이 손상된 통신을 종료하면 원래의 송신 제어기는 통신 버스가 다시 사용 가능한 상태가 되는 즉시 자동으로 재전송을 수행한다. 일반적으로 애플리케이션 소프트웨어(Application Software)는 별도로 재전송을 요청할 필요가 없다. 프로토콜 자체가 통신 환경이 허용되는 한 신뢰성 있는 메시지 전달을 보장하기 때문이다. 자동 재전송은 소프트웨어의 복잡성을 크게 줄이면서도 일시적인 통신 장애로 인해 정보가 영구적으로 손실되는 것을 방지한다.

자동 재전송은 신뢰성을 향상시키지만, 동시에 영구적으로 고장난 제어기가 네트워크를 계속 방해하는 상황도 방지해야 한다. 이를 위해 모든 CAN 제어기는 송신 오류 카운터(Transmit Error Counter, TEC)와 수신 오류 카운터(Receive Error Counter, REC)라는 두 개의 내부 오류 카운터(Error Counters)를 유지한다. 이러한 카운터는 오류가 발생할 때 증가하고 성공적인 통신이 이루어질 때 점진적으로 감소한다. 프로토콜은 개별 오류가 아니라 장기간 누적된 통신 상태(Long-Term Communication Quality)를 기준으로 제어기의 상태를 평가한다.

오류 카운터(Error Counters)의 값은 각각의 제어기가 어떤 동작 상태(Operational State)에 있는지를 결정한다. 초기에는 모든 제어기가 오류 활성 상태(Error Active State)에서 시작하며 정상적인 통신 기능을 수행한다. 오류가 누적되어 미리 정의된 임계값(Thresholds)을 초과하면 오류 수동 상태(Error Passive State)로 전환된다. 이후에도 오류가 계속 증가하면 최종적으로 버스 오프 상태(Bus Off State)에 진입하며 스스로 네트워크에서 완전히 분리되어 전체 시스템의 통신을 보호한다.

오류 활성 상태(Error Active State)의 제어기는 모든 통신에 정상적으로 참여하며 오류가 발생하면 능동 오류 플래그(Active Error Flag)를 생성할 수 있다. 능동 오류 플래그는 지배 비트를 포함하므로 현재 통신을 즉시 중단시키며 손상된 메시지가 수신되지 않도록 적극적으로 네트워크를 보호한다. 정상적인 CAN 네트워크에서는 대부분의 제어기가 오류 활성 상태를 유지한다.

오류 수동 상태(Error Passive State)는 제어기가 지속적인 통신 문제를 경험하고 있지만 아직 완전히 고장난 것은 아니라는 것을 의미한다. 이 상태에서는 지배 비트를 사용하는 능동 오류 플래그 대신 통신에 미치는 영향이 적은 수동 오류 플래그(Passive Error Flag)를 생성한다. 제어기는 계속해서 메시지 교환에 참여하지만 네트워크 전체에 미치는 영향은 줄어든다. 이러한 중간 단계는 완전한 네트워크 분리 이전에 추가적인 복구 기회를 제공한다.

버스 오프 상태(Bus Off State)는 고장난 하드웨어를 격리하기 위한 CAN의 최종 보호 메커니즘이다. 제어기가 버스 오프 상태에 진입하면 즉시 네트워크 통신에서 분리되며 더 이상 메시지를 송신하거나 확인하지 않는다. 이러한 격리는 고장난 장치가 정상적인 다른 제어기의 통신을 지속적으로 방해하는 것을 방지한다. 버스 오프 상태에서 복구하려면 일반적으로 소프트웨어의 명시적인 복구 절차 또는 프로토콜이 정의한 복귀 조건을 만족해야 한다.

오류 활성(Error Active)에서 오류 수동(Error Passive), 그리고 버스 오프(Bus Off)로 이어지는 점진적인 상태 전환은 CAN이 가진 가장 뛰어난 신뢰성 기술 가운데 하나이다. 프로토콜은 통신 오류를 단순한 개별 사건으로 취급하지 않고 장기간의 통신 품질을 평가하여 제어기의 동작을 점진적으로 조정한다. 정상적인 제어기는 계속 통신을 수행하는 반면, 문제가 있는 장치는 점차 스스로 네트워크에서 제외된다. 이러한 분산형 결함 격리 전략(Distributed Fault Confinement Strategy)은 일부 하드웨어에 장애가 발생하더라도 시스템 대부분이 정상적으로 계속 동작하도록 만든다.

결함 격리(Fault Confinement)는 전체 시스템의 가용성(System Availability)을 크게 향상시킨다. 하나의 고장난 제어기가 무한정 통신 대역폭(Communication Bandwidth)을 점유할 수 없으며, 반복적인 오류가 발생하면 결국 버스 오프 상태로 진입하게 된다. 반면 정상적인 제어기들은 거의 영향을 받지 않고 계속해서 메시지를 교환할 수 있다. 이러한 특성은 자동차 시스템, 산업 자동화, 자율주행 로봇 플랫폼과 같이 일부 장치에 장애가 발생하더라도 전체 시스템의 동작을 유지해야 하는 환경에서 매우 중요한 의미를 가진다.

오류 검출 메커니즘(Error Detection Mechanisms)은 진단 기능(Diagnostic Capabilities)도 함께 제공한다. 유지보수 엔지니어는 오류 카운터, 오류 프레임 발생 빈도(Error Frame Frequency), CRC 오류, 확인 오류(Acknowledgement Errors), 스터핑 오류(Stuffing Violations), 버스 오프 이벤트(Bus Off Events)를 분석하여 시스템이 완전히 고장 나기 전에 통신 품질 저하를 조기에 발견할 수 있다. 현대의 진단 소프트웨어(Diagnostic Software)는 이러한 이벤트를 기록하여 노후된 케이블, 손상된 커넥터, 불안정한 전원장치, 고장 징후가 있는 전자 모듈을 사전에 교체할 수 있도록 지원한다.

로봇 시스템에서는 신뢰성 있는 오류 처리(Error Handling)가 필수적이다. 분산형 모터 제어기(Distributed Motor Controllers), 배터리 관리 시스템(Battery Management Systems), 조향 제어기(Steering Controllers), 안전 프로세서(Safety Processors), 관성 센서(Inertial Sensors), 환경 센서(Environmental Sensors), 상위 제어 컴퓨터(Supervisory Computers)는 지속적인 실시간 통신(Real-Time Communication)에 의존한다. 고출력 모터, 스위칭 컨버터(Switching Converters), 유압 액추에이터(Hydraulic Actuators), 산업 설비에서 발생하는 전자기 간섭은 일시적으로 통신을 방해할 수 있다. CAN의 오류 검출 및 자동 복구 기능은 이러한 외란이 로봇의 위험한 동작이나 임무 실패(Mission Failure)로 이어질 가능성을 크게 줄여 준다.

다수의 독립적인 오류 검출 기법(Multiple Independent Error Detection Techniques), 즉각적인 오류 통보(Immediate Error Notification), 자동 재전송(Automatic Retransmission), 분산형 결함 격리(Distributed Fault Confinement), 적응형 제어기 상태(Adaptive Controller States), 하드웨어 격리(Hardware Isolation)의 조합은 CAN을 현대 임베디드 시스템에서 가장 신뢰성이 높은 통신 프로토콜 가운데 하나로 자리 잡게 만들었다. 수십 년 동안 수많은 자동차와 산업 시스템에서 사용되어 왔음에도 불구하고 CAN의 포괄적인 오류 검출 및 처리(Error Detection and Handling) 구조는 오늘날에도 자동차 전자 시스템(Automotive Electronics), 로봇공학(Robotics), 산업 자동화(Industrial Automation), 그리고 CAN FD 네트워크에서 안전하고 결정적이며 높은 신뢰성을 갖는 통신의 핵심 기반으로 계속 활용되고 있다.

## 3.4 Bus Load Calculation

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

통신 대역폭(Communication Bandwidth)을 효율적으로 활용하는 것은 제어기 영역 네트워크(Controller Area Network, CAN) 시스템 설계에서 가장 중요한 목표 가운데 하나이다. 공유 통신 버스(Shared Communication Bus)를 통해 전송되는 모든 메시지는 사용 가능한 통신 시간의 일부를 차지하며, 과도한 통신 트래픽(Traffic)은 지연시간(Latency)을 증가시키고 결정성(Determinism)을 저하시킬 뿐 아니라 높은 우선순위 메시지(High-Priority Messages)의 전달을 지연시키고 궁극적으로 시스템 전체의 성능을 저하시킬 수 있다. 버스 부하 계산(Bus Load Calculation)은 물리적인 시스템을 구현하기 전에 통신 용량을 정량적으로 평가할 수 있는 방법을 제공하며, 구축 이후가 아니라 설계 단계에서 네트워크를 최적화할 수 있도록 한다.

버스 부하(Bus Load)는 현재 전송되는 메시지가 사용 가능한 전체 통신 대역폭 가운데 얼마나 많은 비율을 점유하고 있는지를 나타내는 백분율(Percentage)이다. 예를 들어 버스 부하가 50%인 네트워크는 전체 통신 시간의 절반 정도를 프레임(Frame) 전송에 사용하며, 나머지 절반은 추가적인 통신, 재전송(Retransmissions), 진단 메시지(Diagnostic Messages), 예기치 않은 이벤트(Unexpected Events)를 처리하기 위한 여유로 남게 된다. CAN은 고정 스케줄(Fixed Scheduling)이 아니라 우선순위 기반 중재(Priority-Based Arbitration)를 사용하므로 버스 부하는 통신 지연시간, 중재 지연(Arbitration Delay), 메시지 응답시간(Message Response Time)에 직접적인 영향을 미친다.

버스 부하를 계산하기 위해서는 통신 속도(Communication Speed)와 메시지 전송 시간(Message Transmission Duration)을 모두 이해해야 한다. 각각의 CAN 메시지는 전송되는 비트(Bit)의 개수와 설정된 비트 속도(Bit Rate)에 따라 일정한 시간 동안 통신 매체(Communication Medium)를 점유한다. 여러 메시지가 주기적인 스케줄(Periodic Schedules)에 따라 반복적으로 전송될 경우, 전체 전송 시간을 관측 구간(Observation Interval)으로 나누면 평균 버스 사용률(Average Bus Utilization)을 계산할 수 있다. 계산 자체는 단순해 보이지만 실제 전송 비트 수에는 여러 프로토콜 기능이 영향을 미친다.

CAN 프레임(Frame)의 전체 길이는 프레임 시작(Start of Frame), 중재 필드(Arbitration Field), 제어 필드(Control Field), 데이터 필드(Data Field), 순환 중복 검사(Cyclic Redundancy Check, CRC), 확인 필드(Acknowledgement Field), 프레임 종료(End of Frame), 프레임 간 간격(Interframe Space), 그리고 비트 스터핑(Bit Stuffing)과 같은 다양한 요소에 의해 결정된다. 동일한 애플리케이션 데이터(Application Data)를 포함하는 두 개의 메시지라도 식별자 길이(Identifier Length)나 데이터 길이(Data Length)가 다르면 실제 전송 시간은 달라질 수 있다. 따라서 엔지니어는 애플리케이션 데이터만이 아니라 프로토콜 오버헤드(Protocol Overhead) 전체를 함께 고려하여 네트워크 대역폭을 계산한다.

데이터 길이 코드(Data Length Code, DLC)는 전송 시간에 큰 영향을 미친다. 페이로드(Payload)가 커질수록 더 많은 비트를 전송해야 하기 때문이다. 클래식 CAN(Classical CAN)은 0바이트에서 8바이트까지의 페이로드를 지원하며, 데이터가 많을수록 자연스럽게 전송 시간도 증가한다. 그러나 프로토콜 오버헤드는 데이터 길이와 관계없이 거의 일정하게 유지된다. 따라서 짧은 메시지는 프로토콜 오버헤드가 차지하는 비율이 상대적으로 커지므로 통신 효율(Communication Efficiency)이 낮아진다. 관련된 정보를 하나의 메시지로 묶어 전송하면 전체 대역폭 활용도를 향상시킬 수 있는 경우가 많다.

통신 속도는 특정 프레임(Frame)을 전송하는 데 필요한 시간을 직접 결정한다. 예를 들어 1Mbps에서는 하나의 비트가 약 1마이크로초(Microsecond)를 차지한다. 동일한 프레임을 500kbps에서 전송하면 두 배의 시간이 필요하며, 250kbps에서는 다시 두 배의 시간이 소요된다. 따라서 동일한 메시지 트래픽이라도 통신 속도가 낮을수록 버스를 점유하는 시간이 증가하며, 새로운 통신을 위해 사용할 수 있는 잔여 대역폭(Remaining Bandwidth)은 감소한다.

자동차 및 산업용 CAN 네트워크에서는 대부분 주기적 메시지(Periodic Messages)가 전체 통신의 대부분을 차지한다. 센서 측정값(Sensor Measurements), 액추에이터 명령(Actuator Commands), 배터리 정보(Battery Information), 조향 데이터(Steering Data), 휠 속도(Wheel Speeds), 모터 상태(Motor Status), 시스템 상태(System Health Information)는 일반적으로 일정한 주기(Update Interval)에 따라 반복적으로 전송된다. 따라서 버스 부하 계산은 각각의 주기적 메시지에 대해 전송 주기(Transmission Period)와 전송 시간을 계산한 후, 1초와 같은 대표적인 관측 구간에서 모든 반복 통신을 합산하는 방식으로 시작된다.

일반적인 계산 과정에서는 하나의 메시지가 가지는 전체 비트 수에 전송 빈도(Transmission Frequency)를 곱하고 이를 설정된 통신 속도로 나눈다. 동일한 계산을 모든 메시지에 대해 수행하면 네트워크 전체에 요구되는 총 통신량(Communication Demand)을 구할 수 있다. 누적된 전송 시간을 전체 관측 시간으로 나누면 평균 버스 사용률(Average Bus Utilization)을 백분율로 표현할 수 있다. 대부분의 소프트웨어 도구는 이러한 계산을 자동으로 수행하지만, 정확한 네트워크 설계를 위해서는 계산 원리를 이해하는 것이 매우 중요하다.

비트 스터핑(Bit Stuffing)은 실제 전송되는 비트 수가 데이터 내용에 따라 달라지므로 계산을 더욱 복잡하게 만든다. CAN은 동일한 극성의 비트가 다섯 개 연속될 경우 자동으로 보완 비트(Complementary Bit)를 삽입하여 프레임 길이를 약간 증가시킨다. 스터핑은 실제 데이터 패턴(Data Patterns)에 따라 달라지므로 모든 메시지의 정확한 길이를 계산하기보다는 평균적인 스터핑 오버헤드(Average Stuffing Overhead)를 추정하는 경우가 많다. 보수적인 네트워크 설계에서는 충분한 통신 여유를 확보하기 위해 실제보다 약간 긴 프레임 길이를 가정하는 경우도 있다.

버스 부하 계산에는 통신 오류로 인해 발생하는 재전송(Retransmissions)도 함께 고려해야 한다. 정상적인 환경에서는 재전송이 거의 발생하지 않으므로 추가적인 대역폭 소비는 매우 작다. 그러나 전기적 잡음(Electrically Noisy Environments), 손상된 케이블(Damaged Cables), 불량 트랜시버(Defective Transceivers), 불안정한 전원장치(Unstable Power Supplies)에서는 일시적으로 재전송 횟수가 증가할 수 있다. 따라서 엔지니어는 정상적인 통신량 외에도 일정한 추가 대역폭을 확보하여 재전송이 중요한 실시간 메시지를 지연시키지 않도록 설계한다.

우선순위 기반 중재(Priority-Based Arbitration)는 평균 버스 사용률이 충분히 낮더라도 실제 통신 지연에 영향을 미친다. 작은 식별자 값을 가지는 높은 우선순위 메시지는 대부분의 중재에서 승리하므로 거의 즉시 전송된다. 반면 낮은 우선순위 메시지는 높은 우선순위 통신이 집중될 경우 상당한 대기시간(Waiting Time)을 경험할 수 있다. 따라서 버스 부하 분석은 평균 사용률뿐 아니라 각각의 중요 메시지(Critical Communication Message)에 대한 최악 응답시간(Worst-Case Response Time)도 함께 평가해야 한다.

최악 조건 분석(Worst-Case Analysis)은 특히 안전 관련 시스템(Safety-Related Systems)에서 매우 중요하다. 제동 명령(Braking Commands), 조향 정보(Steering Information), 비상 정지 신호(Emergency Shutdown Signals), 충돌 회피 메시지(Collision Avoidance Messages), 기능 안전 통신(Functional Safety Communications)은 일반적으로 밀리초(Milliseconds) 단위의 엄격한 시간 제약을 가진다. 엔지니어는 최대 통신 부하(Maximum Network Traffic)가 발생하는 상황에서도 이러한 메시지가 항상 마감 시간(Deadlines)을 만족하는지를 계산해야 한다. 이를 위해서는 버스 부하 계산과 함께 중재 시간(Arbitration Timing) 및 최악 스케줄링(Worst-Case Scheduling) 분석이 함께 수행되어야 한다.

평균 버스 부하만으로는 네트워크 성능을 완전히 설명할 수 없다. 실제 통신량은 시간에 따라 계속 변화하기 때문이다. 차량 시동(Vehicle Startup), 진단 세션(Diagnostic Sessions), 펌웨어 업데이트(Firmware Updates), 장애 복구(Fault Recovery), 비상 상황(Emergency Events), 보정 절차(Calibration Procedures), 생산 시험(Manufacturing Tests)에서는 평상시보다 훨씬 많은 메시지가 발생할 수 있다. 따라서 네트워크 설계에서는 정상 운용(Steady-State Operation)뿐 아니라 이러한 일시적인 최대 부하(Transient Peak Traffic Conditions)도 함께 고려해야 한다.

통신 대역폭 계획(Communication Bandwidth Planning)에는 일반적으로 향후 시스템 확장을 위한 안전 여유(Safety Margins)가 포함된다. 새로운 센서(Sensors), 제어 모듈(Control Modules), 소프트웨어 기능(Software Features), 진단 서비스(Diagnostic Services), 사이버보안 기능(Cybersecurity Functions), 자율주행 기능(Autonomous Driving Capabilities)은 제품 출시 이후에도 계속 추가될 수 있다. 초기 단계에서 적절한 수준의 버스 사용률을 유지하도록 설계하면 향후 기능을 추가하더라도 하드웨어를 변경하지 않고 시스템을 확장할 수 있다.

절대적인 기준은 없지만 많은 자동차 및 산업용 애플리케이션에서는 정상 운용 시 평균 버스 사용률을 약 50\~70% 이하로 유지하도록 설계한다. 이러한 범위는 중재 지연, 재전송, 진단 통신, 예상하지 못한 상황을 처리하기 위한 충분한 통신 여유를 제공하면서도 결정적인 실시간 성능(Deterministic Real-Time Performance)을 유지할 수 있도록 한다. 지속적으로 최대 사용률에 가까운 네트워크는 통신 장애와 향후 기능 확장에 더욱 민감해질 수 있다.

버스 부하 계산은 적절한 통신 속도를 선택하는 과정에도 활용된다. 통신 속도를 높이면 모든 메시지의 전송 시간이 짧아지므로 평균 버스 사용률은 감소한다. 그러나 높은 속도는 허용 가능한 최대 케이블 길이를 줄이고 물리 계층 설계(Physical Network Design)에 대한 요구사항을 더욱 엄격하게 만든다. 따라서 엔지니어는 통신 대역폭, 네트워크 규모(Network Dimensions), 전자파 적합성(Electromagnetic Compatibility, EMC), 하드웨어 비용(Hardware Cost), 신뢰성(Reliability)을 종합적으로 고려하여 최적의 통신 속도를 결정한다.

현대의 소프트웨어 개발 도구는 시스템 개발 과정에서 버스 부하를 그래픽(Graphical) 형태로 시각화하는 기능을 제공한다. 프로토콜 분석기(Protocol Analyzers), 네트워크 시뮬레이션(Network Simulation) 도구, 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시스템, 통신 모니터링 소프트웨어는 실시간 버스 사용률과 함께 메시지 빈도(Message Frequency), 식별자 분포(Identifier Distribution), 지연시간(Latency), 중재 통계(Arbitration Statistics), 통신 오류(Communication Errors)를 함께 표시한다. 이러한 측정 결과는 이론적인 계산을 실제 시스템에서 검증하는 데 활용된다.

하드웨어가 아직 준비되지 않은 단계에서는 네트워크 시뮬레이션(Network Simulation)이 매우 중요한 역할을 수행한다. 엔지니어는 예상되는 모든 메시지의 전송 주기, 우선순위(Priorities), 페이로드 길이(Payload Lengths), 통신 속도를 포함한 가상 네트워크 모델(Virtual Communication Models)을 구축한다. 시뮬레이션은 평균 사용률, 최대 지연시간(Maximum Latency), 중재 충돌(Arbitration Conflicts), 응답시간(Response Time), 병목현상(Communication Bottlenecks)을 물리적인 구현 이전에 예측할 수 있다. 이러한 사전 분석은 개발 비용을 절감하고 최종 시스템의 통신 성능을 향상시키는 데 중요한 역할을 한다.

로봇 시스템에서는 버스 부하 계산이 분산형 모터 제어기(Distributed Motor Controllers), 조향 모듈(Steering Modules), 배터리 관리 시스템(Battery Management Systems), 관성측정장치(Inertial Measurement Units, IMUs), 안전 제어기(Safety Controllers), 환경 센서(Environmental Sensors), 유압 액추에이터(Hydraulic Actuators), 매니퓰레이터(Manipulators), 상위 제어 컴퓨터(Supervisory Computers) 사이의 결정적인 협조 제어(Coordinated Control)를 지원한다. 자율주행 로봇은 인지(Perception), 내비게이션(Navigation), 모션 제어(Motion Control), 진단(Diagnostics), 안전 감시(Safety Monitoring)를 동시에 수행해야 하므로 예측 가능한 통신 타이밍이 필수적이다. 적절한 대역폭 계획은 센서 데이터가 집중되거나 비상 상황이 발생하는 경우에도 핵심 제어 메시지가 항상 안정적으로 전송되도록 보장한다.

현대의 통신 아키텍처(Communication Architectures)는 하나의 CAN 버스에 모든 기능을 연결하기보다는 여러 개의 독립적인 CAN 네트워크를 사용하는 경우가 많다. 파워트레인 제어(Powertrain Control), 차체 전장(Body Electronics), 섀시 시스템(Chassis Systems), 배터리 관리(Battery Management), 진단(Diagnostics), 로봇 서브시스템(Robotic Subsystems)은 각각 독립적인 통신 채널(Communication Channels)에서 동작하고 지능형 게이트웨이(Intelligent Gateways)를 통해 서로 연결될 수 있다. 이러한 구조는 각각의 버스 부하를 줄이고 결함 격리(Fault Isolation)를 향상시키며 확장성(Scalability)을 높이고, 각 기능 영역에 적합한 통신 속도를 독립적으로 최적화할 수 있도록 한다.

정확한 버스 부하 계산(Bus Load Calculation)은 단순한 대역폭 계산을 넘어서는 중요한 설계 활동이다. 이는 결정적인 통신(Deterministic Communication), 예측 가능한 지연시간(Predictable Latency), 기능 안전(Functional Safety), 네트워크 확장성(Network Scalability), 장기적인 시스템 신뢰성(Long-Term System Reliability)을 실현하기 위한 분석적 기반이 된다. 프로토콜 오버헤드, 통신 주기(Communication Frequency), 중재 동작(Arbitration Behavior), 재전송 여유(Retransmission Margin), 향후 확장 용량(Future Expansion Capacity), 최악 운용 조건(Worst-Case Operating Conditions)을 종합적으로 분석함으로써 엔지니어는 현대 자동차, 산업 자동화 장비, 자율주행 로봇 플랫폼이 전체 수명 주기 동안 안정적인 실시간 성능을 유지할 수 있는 CAN 네트워크를 설계할 수 있다.

## 3.5 CAN DBC Signal Design

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

제어기 영역 네트워크 데이터베이스(Controller Area Network Database), 일반적으로 DBC 파일(DBC File)이라고 불리는 파일은 CAN 네트워크에서 애플리케이션 데이터(Application Data)가 어떻게 인코딩(Encoding)되고 디코딩(Decoding)되는지를 정의하는 표준 통신 규격(Standardized Communication Specification)이다. DBC 파일은 전기적 특성(Electrical Characteristics)이나 통신 타이밍(Communication Timing)을 설명하는 것이 아니라, 모든 CAN 메시지 내부에서 각 신호(Signal)의 의미, 위치, 스케일링(Scaling), 단위(Unit), 해석 방법을 정의한다. 이는 소프트웨어 개발자, 하드웨어 엔지니어, 진단 도구(Diagnostic Tools), 시뮬레이션 환경(Simulation Environments), 시험 시스템(Test Systems)이 함께 사용하는 공통 언어(Common Language) 역할을 수행하며, 제품의 전체 수명 주기(Product Lifecycle) 동안 모든 장치가 동일한 방식으로 통신을 해석하도록 보장한다.

CAN 프레임(Frame)은 단순히 이진 비트(Binary Bits)의 연속만을 전송할 뿐, 그 데이터의 물리적 의미를 이해하지는 못한다. DBC 파일은 이러한 원시 비트(Raw Bits)를 의미 있는 공학 값(Engineering Values)으로 변환하기 위해 하나의 메시지(Message) 안에 개별 신호(Signal)를 정의한다. 차량 속도(Vehicle Speed), 배터리 전압(Battery Voltage), 조향각(Steering Angle), 모터 온도(Motor Temperature), 휠 토크(Wheel Torque), 유압(Hydraulic Pressure), 로봇 위치(Robot Position), 시스템 상태(System Status)는 모두 CAN 프레임 내부의 특정 비트 위치(Bit Positions)에 배치된 신호로 존재한다. DBC 규격이 없다면 동일한 이진 데이터라도 전자제어장치(Electronic Control Units, ECUs)마다 서로 다르게 해석될 수 있으며, 이는 통신 오류와 예측 불가능한 시스템 동작을 초래할 수 있다.

DBC 파일은 일반적으로 메시지 정의(Message Definitions), 신호 정의(Signal Definitions), 노드 정보(Node Information), 전송 속성(Transmission Attributes), 값 테이블(Value Tables), 주석(Comments), 스케일링 매개변수(Scaling Parameters), 단위(Units), 멀티플렉싱 정보(Multiplexing Information), 그리고 네트워크 동작을 설명하는 다양한 메타데이터(Metadata)를 포함한다. 각각의 CAN 메시지는 고유한 식별자(Identifier), 데이터 길이(Data Length), 송신 노드(Transmitter Node), 그리고 해당 메시지에 포함된 신호들의 집합으로 정의된다. 각 신호는 비트 위치(Bit Location), 비트 길이(Bit Length), 바이트 순서(Byte Order), 부호(Sign), 변환 공식(Conversion Formula), 물리 단위(Physical Unit), 최소값(Minimum Value), 최대값(Maximum Value), 수신 노드(Receiving Nodes)를 함께 정의한다.

메시지 설계(Message Design)는 먼저 통신 목적(Communication Purpose)을 정의한 후 식별자와 페이로드(Payload) 위치를 할당하는 과정으로 시작된다. 엔지니어는 어떤 서브시스템(Subsystem)이 통신을 필요로 하는지, 어떤 정보를 교환해야 하는지, 얼마나 자주 갱신(Update)되어야 하는지, 어떤 전자제어장치가 데이터를 송신할 것인지를 먼저 결정한다. 기능 요구사항(Functional Requirements)을 충분히 이해한 이후에야 효율적인 메시지 구조(Message Structure)를 설계할 수 있다. 잘 구성된 메시지 정의는 향후 소프트웨어 개발, 진단, 유지보수, 시스템 확장을 더욱 단순하게 만들어 준다.

신호 설계(Signal Design)는 제한된 통신 대역폭 안에서 물리량(Physical Quantities)을 가장 효율적으로 표현하는 데 초점을 맞춘다. 각각의 신호는 요구되는 측정 해상도(Measurement Resolution)를 만족하는 데 필요한 최소한의 비트(Bit)만을 사용한다. 예를 들어 0.1도(One-Tenth Degree) 단위의 정밀도가 필요한 조향각은 단순한 온·오프 상태 표시(On-Off Status Indicator)보다 더 긴 비트 길이가 필요할 수 있다. 따라서 엔지니어는 측정 정확도(Measurement Accuracy), 통신 대역폭(Communication Bandwidth), 계산 단순성(Computational Simplicity), 향후 확장성(Future Scalability)을 균형 있게 고려하여 신호 크기를 결정한다.

비트 위치(Bit Position)는 DBC 신호 정의에서 가장 중요한 요소 가운데 하나이다. 각각의 신호는 지정된 시작 비트(Start Bit)에서 시작하여 CAN 페이로드 내부의 연속된 비트를 차지한다. 신호를 배치할 때에는 서로 겹치지 않도록 하면서도 전체 페이로드를 최대한 효율적으로 활용해야 한다. 비트를 체계적으로 배치하면 사용되지 않는 공간(Unused Space)을 최소화하고 필요한 메시지 수를 줄이며 네트워크 전체의 통신 효율을 높일 수 있다. 또한 관련 신호를 논리적으로 함께 배치하면 소프트웨어 구현과 디버깅(Debugging)도 더욱 쉬워진다.

바이트 순서(Byte Order)는 여러 바이트(Multi-Byte)로 구성된 신호가 CAN 페이로드 안에 어떻게 저장되는지를 정의한다. DBC 파일은 모토로라 빅엔디언(Motorola Big-Endian)과 인텔 리틀엔디언(Intel Little-Endian)을 모두 지원한다. 제조사가 다른 프로세서(Processor)는 수치 데이터를 서로 다른 방식으로 저장할 수 있으므로, 바이트 순서를 명확하게 정의하면 송신 장치와 수신 장치가 사용하는 프로세서 구조와 관계없이 동일한 수치 값을 재구성할 수 있다. 잘못된 바이트 순서 설정은 시스템 통합(System Integration) 과정에서 가장 자주 발생하는 통신 문제 가운데 하나이다.

신호 길이(Signal Length)는 표현 가능한 수치 범위(Numerical Range)와 해상도(Resolution)를 직접 결정한다. 긴 신호는 넓은 측정 범위나 높은 정밀도를 지원하지만 더 많은 통신 대역폭을 사용한다. 설계자는 예상되는 운용 조건(Operating Conditions), 센서 정확도(Sensor Accuracy), 제어 요구사항(Control Requirements)을 종합적으로 고려하여 적절한 신호 길이를 선택한다. 지나치게 많은 비트를 할당하면 대역폭이 낭비되고, 반대로 너무 적게 할당하면 향후 시스템 성능이 제한될 수 있다.

부호(Sign)와 무부호(Unsigned) 표현은 신호가 음수 값을 가질 수 있는지를 결정한다. 조향각(Steering Angle), 모터 전류(Motor Current), 가속도(Acceleration), 각속도(Angular Velocity), 위치 오차(Position Error)와 같은 물리량은 양수와 음수 모두 존재하므로 일반적으로 부호 있는 표현(Signed Representation)을 사용한다. 반면 배터리 전압(Battery Voltage), 잔여 에너지(Remaining Energy), 휠 속도 크기(Wheel Speed Magnitude), 동작 시간(Operating Time)과 같은 값은 음수의 의미가 없으므로 대부분 무부호 표현(Unsigned Representation)을 사용한다.

스케일링 계수(Scaling Factors)는 저장된 이진 값을 실제 공학 단위의 물리량으로 변환하는 역할을 수행한다. CAN 네트워크는 일반적으로 부동소수점(Floating-Point) 값을 직접 전송하지 않고, 작은 정수(Integer) 값과 DBC 파일에 정의된 스케일링 계수를 함께 사용한다. 저장된 정수에 스케일 계수를 곱하고 오프셋(Offset)을 적용하면 실제 물리량을 계산할 수 있다. 이러한 방식은 통신 대역폭을 절약하고 임베디드 소프트웨어(Embedded Software)를 단순화하며 결정적인 계산 성능(Deterministic Computational Performance)을 유지하면서도 높은 측정 정밀도를 제공한다.

오프셋 값(Offset Values)은 신호 길이를 증가시키지 않으면서 수치 범위를 이동시키는 역할을 한다. 온도 센서(Temperature Sensors)는 대표적인 예로, 실제 온도는 음수일 수 있지만 전송되는 정수 값은 항상 양수일 수 있다. 적절한 오프셋을 적용하면 제한된 비트 안에서도 전체 운용 온도 범위를 효율적으로 표현할 수 있다. 스케일링 계수와 오프셋을 함께 사용하면 저장된 이진 값과 실제 공학 값 사이를 유연하게 변환할 수 있다.

공학 단위(Engineering Units)는 모든 신호에 물리적인 의미를 부여한다. 속도는 km/h 또는 m/s, 토크(Torque)는 N·m(Newton-Meters), 온도는 ℃(Degrees Celsius), 압력은 kPa(Kilopascals), 전압은 V(Volts), 전류는 A(Amperes)와 같은 단위를 사용한다. 명확하게 정의된 단위는 개발 팀 간의 혼동을 방지하며, 시뮬레이션 도구, 진단 소프트웨어, 데이터 로깅(Logging) 시스템, 시각화 플랫폼(Visualization Platforms)이 모두 동일한 물리량을 일관되게 표시하도록 보장한다.

최소값(Minimum Value)과 최대값(Maximum Value)은 신호의 정상 운용 범위를 정의한다. 이러한 범위는 센서 고장(Sensor Failures), 통신 오류(Communication Corruption), 소프트웨어 결함(Software Defects), 하드웨어 고장(Hardware Malfunctions)으로 인해 발생하는 비정상적인 데이터를 검출하는 데 활용된다. 많은 검증 로직(Validation Logic)은 제어 알고리즘(Control Algorithms)이 데이터를 사용하기 전에 DBC 파일에 정의된 허용 범위와 비교하여 데이터의 유효성을 확인한다.

열거형 값 테이블(Enumerated Value Tables)은 이산적인 시스템 상태(Discrete Operating States)를 이해하기 쉽게 표현한다. 예를 들어 0, 1, 2, 3과 같은 숫자 대신 Off, Standby, Ready, Active, Charging, Emergency, Fault와 같은 의미 있는 상태 이름을 자동으로 표시한다. 이러한 기호적 정의(Symbolic Definitions)는 진단(Diagnostics), 유지보수(Maintenance), 소프트웨어 디버깅, 통신 분석 과정에서 추상적인 숫자 대신 실제 시스템 상태를 즉시 이해할 수 있도록 도와준다.

멀티플렉싱(Multiplexing)은 하나의 CAN 메시지가 운용 조건에 따라 여러 종류의 신호 집합을 공유할 수 있도록 하는 기술이다. 하나의 신호는 멀티플렉서 선택기(Multiplexer Selector) 역할을 수행하고, 다른 신호들은 그 값에 따라 활성화된다. 이러한 방식은 서로 다른 데이터 집합이 동일한 페이로드 위치를 공유하도록 하여 전체 메시지 개수를 증가시키지 않고도 통신 대역폭 활용도를 크게 향상시킨다. 멀티플렉싱은 선택적인 기능(Optional Subsystems)이나 특정 운용 모드에서만 사용하는 데이터를 전송할 때 특히 효과적이다.

노드 정의(Node Definitions)는 네트워크 전체의 통신 책임을 명확하게 정의한다. 각각의 메시지는 송신 노드와 수신 노드를 지정하여 분산된 전자제어장치 간의 전체 통신 구조를 쉽게 이해할 수 있도록 한다. 명확한 책임 구분은 소프트웨어 통합, 네트워크 검증, 역할 분담, 향후 유지보수를 더욱 효율적으로 수행할 수 있도록 지원한다.

DBC 파일에 포함된 주석(Comments)은 신호의 목적, 운용 가정(Operating Assumptions), 보정 방법(Calibration Methods), 설계 의도(Engineering Rationale), 구현 참고사항(Implementation Notes)을 설명하는 중요한 문서 역할을 한다. 주석 자체는 통신 동작에 영향을 주지는 않지만, 개발 조직이 변경되더라도 설계 지식을 보존하여 장기적인 유지보수성을 크게 향상시킨다. 잘 작성된 DBC 파일은 제품 전체 수명 주기 동안 핵심적인 참조 문서가 되는 경우가 많다.

버전 관리(Version Management)는 DBC 유지관리에서 매우 중요한 요소이다. 제품에 새로운 센서, 제어 기능, 안전 기능, 진단 기능, 자율주행 기능이 추가되면서 통신 규격도 지속적으로 변화한다. 엄격한 버전 관리는 서로 다른 소프트웨어 버전이 동일한 메시지를 다르게 해석하는 문제를 방지한다. 모든 변경 사항은 생산 시스템에 적용되기 전에 추적 가능해야 하며, 문서화(Documented), 검토(Reviewed), 검증(Validated) 과정을 거쳐야 한다.

현대의 자동차 및 산업용 개발 환경에서는 DBC 파일로부터 소프트웨어를 자동 생성(Automatic Generation)하는 기능을 널리 활용한다. 통신 드라이버(Communication Drivers), 애플리케이션 인터페이스(Application Interfaces), 신호 인코딩 함수(Encoding Functions), 디코딩 라이브러리(Decoding Libraries), 진단 데이터베이스(Diagnostic Databases), 시뮬레이션 모델(Simulation Models), 시험 스크립트(Test Scripts), 시각화 도구(Visualization Tools)를 모두 하나의 DBC 규격으로부터 일관성 있게 생성할 수 있다. 이러한 자동 생성은 수작업 코딩 오류를 줄이고 소프트웨어, 시험, 문서, 시스템 통합 사이의 일관성을 유지하는 데 큰 도움이 된다.

네트워크 시뮬레이션(Network Simulation) 환경에서도 DBC 파일은 중요한 역할을 수행한다. 하드웨어가 준비되기 전에도 엔지니어는 DBC 정의를 기반으로 가상의 CAN 메시지를 생성하여 애플리케이션 소프트웨어, 진단 시스템, 제어 알고리즘, 모니터링 도구를 실제 전자제어장치 없이 개발하고 검증할 수 있다. 이러한 모델 기반 개발(Model-Based Development) 방식은 개발 기간을 단축시키면서 통신 신뢰성을 향상시킨다.

DBC 파일은 시스템 검증(System Verification) 과정에서도 핵심적인 역할을 수행한다. 자동 시험 프레임워크(Automated Testing Frameworks)는 실제 전송되는 메시지를 DBC 규격과 비교하여 식별자 사용, 신호 위치, 스케일링 정확도, 타이밍, 공학 단위, 허용 범위, 디코딩 정확성을 검증한다. 이러한 지속적인 검증은 소프트웨어가 여러 차례 업데이트되는 과정에서도 통신 호환성(Communication Compatibility)을 안정적으로 유지하도록 보장한다.

자율주행 로봇에서는 DBC 신호 설계가 기존 자동차 통신을 넘어 분산형 지능(Distributed Intelligence)을 연결하는 핵심 역할을 수행한다. 모션 제어기(Motion Controllers), 배터리 관리 시스템(Battery Management Systems), 센서 융합 프로세서(Sensor Fusion Processors), 인지 컴퓨터(Perception Computers), 안전 제어기(Safety Controllers), 매니퓰레이터(Manipulators), 위치추정 모듈(Localization Modules), 플릿 관리 인터페이스(Fleet Management Interfaces), 인간-기계 인터페이스(Human-Machine Interaction Systems)는 모두 DBC 기반의 신호를 이용하여 정보를 교환한다. 잘 설계된 신호 정의는 결정적인 실시간 정보 교환을 가능하게 하며, 진단, 예지보전(Predictive Maintenance), 소프트웨어 업데이트, 미래 기능 확장을 위해 통신 구조를 변경하지 않고도 시스템을 지속적으로 발전시킬 수 있도록 한다.

효율적인 CAN DBC 신호 설계(CAN DBC Signal Design)는 통신 효율(Communication Efficiency), 소프트웨어 일관성(Software Consistency), 시스템 확장성(System Scalability), 장기 유지보수성(Long-Term Maintainability)을 하나의 통합된 통신 규격(Unified Communication Specification)으로 구현하는 과정이다. 메시지 구조, 신호 배치, 스케일링 방식, 공학적 의미, 검증 범위, 문서화, 버전 관리를 체계적으로 정의함으로써 엔지니어는 현대 자동차, 산업 자동화 시스템, 자율주행 로봇 플랫폼이 전체 수명 주기 동안 정확하고 신뢰성 있는 정보를 안정적으로 교환할 수 있는 견고한 CAN 네트워크 기반을 구축할 수 있다.
