**Volume 06 Automotive Communication**

# Chapter 10. FlexRay

## 10.01. FlexRay Protocol Overview

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

플렉스레이(FlexRay)는 기존 CAN 네트워크보다 높은 대역폭(Bandwidth), 예측 가능한 타이밍(Predictable Timing), 결함 허용성(Fault Tolerance)이 필요한 분산 전자 제어 시스템(Distributed Electronic Control System)을 위해 개발된 결정론적 고속 통신 프로토콜(Deterministic High-Speed Communication Protocol)이다. 주로 섀시 제어(Chassis Control), 파워트레인 협조 제어(Powertrain Coordination), 엑스 바이 와이어(X-by-Wire)와 같은 첨단 자동차 응용을 위해 설계되었다. 시간 트리거 통신(Time-Triggered Communication)과 선택적인 이벤트 트리거 전송(Event-Triggered Transmission)을 결합하여 안전 중요 통신과 일반 제어 통신을 하나의 네트워크에서 함께 처리할 수 있다.

플렉스레이(FlexRay)의 핵심적인 특징은 결정론적 통신 동작(Deterministic Communication Behavior)이다. CAN의 중재(Arbitration) 방식처럼 노드(Node)가 지속적으로 버스 접근 권한을 경쟁하는 대신, 플렉스레이는 동기화된 통신 사이클(Communication Cycle)에 따라 통신을 구성한다. 각 전자 제어 장치(Electronic Control Unit, ECU)는 공통 전역 시간 기준(Global Time Reference)을 따르며, 메시지를 미리 정의된 시간 위치에서 전송한다. 이를 통해 제한된 지연 시간(Bounded Latency)과 높은 수준의 예측 가능한 메시지 전달을 제공한다.

플렉스레이 통신 사이클(FlexRay Communication Cycle)은 기본적으로 정적 세그먼트(Static Segment)와 동적 세그먼트(Dynamic Segment)로 구분된다. 정적 세그먼트는 특정 통신 노드에 할당된 미리 정의된 시간 슬롯(Time Slot)을 사용하여 엄격한 결정론적 전송을 제공한다. 동적 세그먼트는 미니슬롯(Minislot) 기반의 접근 방식을 통해 더욱 유연하게 대역폭을 활용한다. 또한 심볼 전송(Symbol Transmission)과 네트워크 유휴 시간(Network Idle Time)을 위한 영역이 포함되어 동기화와 다양한 통신을 하나의 반복적인 구조 안에서 조정한다.

정적 세그먼트(Static Segment)에서는 설정된 각 메시지가 통신 스케줄(Communication Schedule) 내의 사전에 결정된 슬롯을 사용한다. 노드는 자신에게 할당된 슬롯이 도달했을 때만 데이터를 전송하기 때문에 정상적으로 설정된 노드 사이에서는 충돌이 발생하지 않는다. 이러한 특성은 조향 명령(Steering Command), 제동 정보(Braking Information), 차량 동역학 데이터(Vehicle Dynamics Data), 액추에이터 목표값(Actuator Target), 동기화된 센서 정보처럼 시간 변동을 엄격하게 제한해야 하는 신호에 특히 적합하다.

동적 세그먼트(Dynamic Segment)는 실제 전송 요구에 따라 통신 자원을 할당할 수 있도록 하여 정적 스케줄링(Static Scheduling)을 보완한다. 고정된 전체 슬롯 대신 미니슬롯(Minislot)을 사용하여 설정된 동적 프레임(Dynamic Frame)이 필요한 시점에 전송될 수 있도록 한다. 따라서 안전 중요 제어 신호처럼 엄격한 주기적 타이밍이 필요하지 않은 정보에 보다 높은 유연성을 제공한다. 결과적으로 플렉스레이는 결정론적 주기 통신과 상대적으로 유연한 이벤트 지향 통신(Event-Oriented Communication)을 하나의 동기화된 프로토콜에서 결합할 수 있다.

플렉스레이(FlexRay)는 통신 채널(Communication Channel)당 최대 10 Mbit/s의 데이터 전송률(Data Rate)을 지원하여 클래식 CAN(Classical CAN)에 비해 상당히 높은 통신 성능을 제공한다. 더욱 중요한 특징은 일반적으로 채널 A(Channel A)와 채널 B(Channel B)라고 하는 두 개의 독립적인 물리 통신 채널(Physical Communication Channel)을 중심으로 설계되었다는 점이다. 두 채널은 결함 허용성을 향상시키기 위한 이중화(Redundancy)에 사용하거나 통신 용량을 증가시키기 위해 독립적으로 사용할 수 있다.

이중 채널 동작(Dual-Channel Operation)은 특히 결함 허용 아키텍처(Fault-Tolerant Architecture)에서 중요하다. 동일한 안전 중요 정보(Safety-Critical Information)를 두 채널을 통해 동시에 전송하면 하나의 물리적 통신 경로가 손상되거나 장애를 받아도 통신을 지속할 수 있다. 반대로 서로 다른 메시지를 두 채널에 분배하면 유효 네트워크 용량(Effective Network Capacity)을 증가시킬 수 있다. 따라서 설계자는 동일한 프로토콜과 동기화 구조를 유지하면서 시스템 요구에 따라 이중화와 대역폭 사이의 균형을 조정할 수 있다.

플렉스레이 네트워크(FlexRay Network)는 수동 버스 구조(Passive Bus Structure), 능동 스타 구성(Active Star Configuration), 그리고 이들을 결합한 다양한 물리 토폴로지(Physical Topology)를 사용할 수 있다. 능동 스타(Active Star)는 연결된 각 분기 사이에서 신호를 수신하고 재분배하는 전자 회로를 포함하여 네트워크 영역 간의 격리(Isolation)를 향상시킨다. 따라서 패키징(Packaging), 배선(Wiring), 결함 격리(Fault Containment), 가용성(Availability) 요구에 따라 하이브리드 구조(Hybrid Architecture)를 구성할 수 있다.

시간 동기화(Time Synchronization)는 플렉스레이의 가장 중요한 기술적 기반 중 하나이다. 선택된 노드들은 동기화 정보를 제공하여 분산된 제어기(Distributed Controller)가 충분히 일관된 전역 통신 시간(Global Communication Time)을 유지하도록 한다. 각 노드의 로컬 발진기(Local Oscillator)는 필연적으로 클록 오프셋(Clock Offset)과 드리프트(Drift)를 발생시키므로 프로토콜은 주기적으로 동기화 보정(Synchronization Correction)을 수행한다. 이를 통해 모든 노드가 통신 사이클의 경계와 할당된 전송 슬롯을 동일하게 인식할 수 있다.

플렉스레이 프레임(FlexRay Frame)은 애플리케이션 페이로드(Application Payload)와 함께 프로토콜 제어 정보(Protocol Control Information) 및 오류 검출 정보(Error Detection Information)를 전달한다. 프레임 구조(Frame Structure)는 헤더(Header), 페이로드(Payload), 순환 중복 검사(Cyclic Redundancy Check, CRC)를 포함하는 트레일러(Trailer)로 구성된다. 헤더는 프레임 식별자(Frame Identifier)와 페이로드 설정 등의 정보를 제공하며, CRC 메커니즘은 전송 중 데이터 손상을 검출하여 잘못된 정보가 애플리케이션에서 사용되는 것을 방지한다.

플렉스레이는 전원 인가 또는 네트워크 장애 이후 결정론적 통신을 설정하는 데 필요한 시작 및 동기화 절차(Startup and Synchronization Procedure)도 포함한다. 특정 노드는 시작 및 동기화 기능(Startup and Synchronization Capability)을 수행하도록 설정될 수 있으며, 이를 통해 네트워크가 통신 스케줄과 전역 시간 기준을 확립한다. 분산 실시간 시스템(Distributed Real-Time System)은 초기화 과정에서도 임의적인 메시지 타이밍에 의존할 수 없기 때문에 시작 동작은 단순한 구현 기능이 아니라 통신 아키텍처의 중요한 부분이다.

플렉스레이는 시스템 통합(System Integration) 과정에서 설정되는 구성 파라미터(Configuration Parameter)를 통해 통신 스케줄링과 애플리케이션 동작을 분리한다. 엔지니어는 정상 운용 전에 통신 사이클, 슬롯 할당(Slot Assignment), 프레임 식별자, 페이로드 길이, 채널 사용 방식, 타이밍 파라미터(Timing Parameter)를 정의한다. 따라서 플렉스레이 엔지니어링은 체계적인 시스템 수준 계획을 필요로 하며, 메시지 할당 변경이 여러 ECU의 타이밍에 영향을 줄 수 있기 때문에 통신 스케줄 설계가 전체 전기전자 아키텍처(E/E Architecture)의 중요한 요소가 된다.

CAN과 비교할 때 플렉스레이는 우선순위 기반 중재(Priority-Based Arbitration)보다 보장된 시간적 동작(Guaranteed Temporal Behavior)을 더욱 중요하게 다룬다. CAN은 높은 신뢰성과 단순성을 제공하지만 전송 지연 시간은 버스 부하(Bus Load)와 메시지 우선순위에 따라 달라진다. 반면 플렉스레이의 정적 스케줄링은 중요한 프레임이 설정된 각 통신 사이클의 알려진 위치에서 전송 기회를 확보하도록 보장할 수 있다. 이러한 특성으로 인해 플렉스레이는 여러 액추에이터와 센서를 긴밀하게 협조 제어해야 하는 분산 제어 기능에 적합했다.

플렉스레이는 기계적 또는 유압식 연결을 전자 센싱(Electronic Sensing), 연산(Computation), 통신(Communication), 액추에이션(Actuation)으로 보완하거나 대체하는 엑스 바이 와이어(X-by-Wire) 개념과 밀접하게 연관되었다. 조향(Steering), 제동(Braking), 서스펜션(Suspension), 차량 동역학 협조 제어(Coordinated Vehicle Dynamics Control)는 여러 제어기 사이의 신뢰성 높은 제어 정보 교환을 필요로 한다. 결정론적 스케줄링, 이중 채널, 시간 동기화, 결함 허용 네트워크 구조는 이러한 안전 관련 분산 시스템에 적합한 아키텍처 특성을 제공하였다.

플렉스레이의 결정론적 특성은 분산 제어 루프(Distributed Control Loop)의 구현에도 유리하다. 여러 제어기가 센서 상태, 계산된 명령, 액추에이터 피드백(Actuator Feedback)을 교환하는 경우 네트워크 지연의 변화는 제어 안정성과 응답 성능에 영향을 줄 수 있다. 시간 트리거 스케줄(Time-Triggered Schedule)을 사용하면 센싱(Sensing), 연산, 전송, 액추에이션을 알려진 시간 경계에 맞추어 조정할 수 있다. 따라서 통신을 예측하기 어려운 외부 전송 수단이 아니라 제어 시스템 타이밍 설계(Control-System Timing Design)의 일부로 포함할 수 있다.

플렉스레이는 차량 네트워크(Vehicle Network)의 전체적인 발전 과정 안에서 이해할 필요가 있다. 첨부된 구조에서는 자동차 이더넷(Automotive Ethernet) 이후에 플렉스레이가 배치되어 있으며, 이후에는 MOST의 역사적 발전을 다룬다. 또한 플렉스레이의 후속 내용은 시간 트리거 통신(Time-Triggered Communication), 엑스 바이 와이어 적용(X-by-Wire Application), CAN FD와의 비교, 그리고 플렉스레이의 유산과 쇠퇴(Legacy and Decline)를 다룬다. 이는 플렉스레이가 실제 자동차 통신 기술인 동시에 결정론적 분산 통신을 이해하기 위한 중요한 참조 아키텍처임을 보여준다.

로보틱스(Robotics)와 피지컬 AI(Physical AI) 시스템에서도 플렉스레이가 실제 통신 기술로 선택되지 않더라도 그 개념은 여전히 중요한 의미를 가진다. 현대 로봇에는 분산 센서(Distributed Sensor), 모터 제어기(Motor Controller), 안전 제어기(Safety Controller), 임베디드 컴퓨터(Embedded Computer), 고성능 컴퓨팅 노드(High-Performance Computing Node)가 존재하며 각각 서로 다른 지연 시간과 대역폭을 요구한다. 플렉스레이는 결정론적 스케줄링, 동기화된 클록, 통신 이중화, 결함 격리, 정교한 네트워크 타이밍 설계가 분산된 물리 구성요소의 안정적인 협조 제어를 어떻게 지원할 수 있는지를 보여준다.

현대의 자동차 이더넷(Automotive Ethernet)과 시간 민감형 네트워킹(Time-Sensitive Networking, TSN)은 훨씬 큰 데이터 흐름을 지원하면서 유사한 실시간 통신 요구를 처리하고 있으며, CAN FD와 CAN XL 역시 CAN 계열의 통신 능력을 확장하고 있다. 따라서 플렉스레이는 통신 기술 발전 과정에서 중요한 과도기적 위치를 차지한다. 결정론적 네트워킹을 분산 차량 제어 아키텍처에 직접 적용할 수 있음을 보여주는 동시에 엄격한 스케줄 설정과 전용 네트워크 기술에서 발생하는 비용과 복잡성도 보여주었다.

플렉스레이를 이해하면 보다 일반적인 실시간 통신 원리(Real-Time Communication Principle)를 이해하기 위한 기반을 마련할 수 있다. 정적 세그먼트와 동적 세그먼트는 결정론적 자원 예약(Deterministic Resource Reservation)과 유연한 대역폭 활용 사이의 절충 관계를 보여주며, 이중 채널은 통신 이중화(Communication Redundancy)를 설명한다. 전역 시간 모델(Global Time Model)은 분산 동기화(Distributed Synchronization)를 보여주고, 다양한 토폴로지는 물리 아키텍처가 결함 격리에 미치는 영향을 보여준다. 이러한 개념은 안전 중요 자동차 네트워크뿐 아니라 산업 시스템, 자율 로봇, 분산 피지컬 AI 플랫폼(Distributed Physical AI Platform)을 설계하는 데에도 직접적으로 활용될 수 있다.

## 10.02. Time Triggered Communication

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

시간 트리거 통신(Time-Triggered Communication)은 비동기 이벤트(Asynchronous Event)에 의해 주로 전송이 시작되는 방식이 아니라, 사전에 결정된 특정 시점에 메시지를 전송하는 네트워킹 방식이다. 플렉스레이(FlexRay)에서 이 원리는 통신 매체(Communication Medium)에 대한 결정론적 접근(Deterministic Access)을 제공하며, 분산 전자 제어 장치(Distributed Electronic Control Unit)가 정확하게 정의된 스케줄에 따라 중요 정보를 교환할 수 있도록 한다. 예측 가능한 타이밍은 통신 지연 시간이 제한되어야 하고 실시간 제어 기능과 긴밀하게 연계되어야 하는 시스템에서 특히 중요하다.

기본적인 개념은 네트워크 동작을 반복되는 통신 사이클(Communication Cycle)로 나누고 참여 노드(Participating Node)에 특정한 전송 기회(Transmission Opportunity)를 할당하는 것이다. 각 노드는 자신이 언제 전송할 수 있는지와 다른 노드의 메시지가 언제 도착할지를 알고 있다. 이러한 전송 기회가 정상 동작 이전에 설정되므로 올바르게 구성된 노드는 예정된 전송 과정에서 통신 매체를 차지하기 위해 경쟁할 필요가 없다. 따라서 네트워크 타이밍(Network Timing)은 순간적인 트래픽 상황의 결과가 아니라 설계된 시스템 특성이 된다.

이러한 아키텍처에서는 공유된 시간 개념(Shared Time)이 필수적이다. 각 제어기는 로컬 클록(Local Clock)을 유지하지만 발진기(Oscillator)의 허용오차로 인해 개별 클록은 서로 다른 속도로 드리프트(Drift)할 수 있다. 따라서 플렉스레이는 분산 클록 동기화(Distributed Clock Synchronization)를 적용하여 노드들이 충분히 일관된 전역 시간 기준(Global Time Reference)을 유지하도록 한다. 주기적인 동기화 보정(Synchronization Correction)을 통해 모든 노드가 통신 사이클 경계와 예정된 전송 위치를 동일하게 해석할 수 있다.

플렉스레이 통신 사이클(FlexRay Communication Cycle)은 시간 트리거 동작을 위한 시간적 프레임워크(Temporal Framework)를 제공한다. 통신 사이클은 정적 세그먼트(Static Segment), 동적 세그먼트(Dynamic Segment), 심볼 윈도(Symbol Window), 네트워크 유휴 시간(Network Idle Time)으로 구성된다. 이 가운데 정적 세그먼트는 미리 정의된 통신 슬롯으로 구성되기 때문에 엄격한 시간 트리거 통신과 가장 직접적으로 관련된다. 전체 사이클은 지속적으로 반복되며 제어기가 통신, 연산, 센싱, 액추에이션을 조정할 수 있는 주기적인 시간축을 형성한다.

정적 세그먼트(Static Segment) 내부에서는 통신 자원이 미리 정의된 위치와 지속 시간을 갖는 슬롯(Slot)으로 분할된다. 특정 슬롯에 연결된 프레임(Frame)은 통신 사이클에서 해당 슬롯이 도달했을 때 전송될 수 있다. 슬롯 할당(Slot Assignment)은 메시지 중재(Message Arbitration)를 통해 동적으로 결정되는 것이 아니라 네트워크 스케줄의 일부로 사전에 설정된다. 따라서 주기적으로 스케줄된 메시지의 최대 대기 시간(Maximum Waiting Time)을 시스템이 실제로 동작하기 전에 통신 설정을 기반으로 분석할 수 있다.

이 방식은 클래식 CAN(Classical CAN)과 같은 우선순위 기반 통신(Priority-Based Communication)과 근본적으로 다르다. CAN에서는 여러 노드가 동시에 전송을 시도할 수 있으며 이후 식별자 기반 중재(Identifier-Based Arbitration)를 통해 어떤 프레임이 버스를 사용할지 결정된다. 높은 우선순위 메시지는 유리한 접근 권한을 얻지만 지연 시간은 경쟁 트래픽에 따라 달라질 수 있다. 반면 시간 트리거 정적 스케줄에서는 접근 시간이 이미 할당되어 있으므로 정상적으로 스케줄된 전송 간의 중재가 제거되고 훨씬 예측 가능한 시간적 동작을 제공한다.

결정론적 통신(Deterministic Communication)이 모든 메시지를 동일한 주파수로 전송해야 한다는 것을 의미하지는 않는다. 서로 다른 신호는 각각의 갱신 요구사항(Update Requirement)에 따라 다른 통신 기회를 할당받을 수 있다. 빠른 제어 정보는 모든 통신 사이클마다 전송할 수 있으며, 상대적으로 느린 상태 정보는 선택된 사이클에서만 전송할 수 있다. 따라서 플렉스레이 스케줄링(FlexRay Scheduling)은 중요한 메시지의 결정론적 전송 기회를 유지하면서 애플리케이션의 타이밍 요구에 따라 주기적 통신을 구성할 수 있다.

통신 스케줄(Communication Schedule)의 개념은 단순한 버스 접근보다 넓은 의미를 가진다. 분산 제어 시스템(Distributed Control System)에서는 센서 정보가 언제 샘플링되고, 데이터가 언제 전송되며, 제어기가 언제 알고리즘을 실행하고, 액추에이터 명령이 언제 적용되는지를 조정할 수 있다. 이를 통해 예측 가능한 센싱-연산-통신-액추에이션(Sense-Compute-Communicate-Actuate) 순서를 구성할 수 있다. 시간 관계를 사전에 알 수 있기 때문에 제어 루프 지연(Control-Loop Latency)과 지터(Jitter)를 체계적으로 분석할 수 있다.

시간 트리거 통신은 타이밍 변화가 안정성이나 성능에 영향을 미칠 수 있는 분산 제어 루프(Distributed Control Loop)에 특히 유용하다. 조향(Steering), 제동(Braking), 서스펜션(Suspension), 차량 동역학 제어기(Vehicle Dynamics Controller)는 고정된 간격으로 상태와 명령을 반복적으로 교환할 수 있다. 통신 타이밍이 크게 변하면 수신 제어기는 추가적인 불확실성을 처리해야 한다. 결정론적 스케줄은 이러한 불확실성을 줄이고 제어 알고리즘과 통신 아키텍처를 알려진 시간 관계를 기준으로 설계할 수 있게 한다.

플렉스레이는 정적 세그먼트와 동적 세그먼트를 통해 엄격한 시간 트리거 통신과 보다 유연한 통신을 결합한다. 정적 세그먼트는 주기적 트래픽을 위해 결정론적 슬롯을 예약하는 반면, 동적 세그먼트는 보다 높은 유연성이 필요한 정보에 대해 미니슬롯 기반 접근(Minislot-Based Access)을 사용한다. 이러한 하이브리드 구조(Hybrid Structure)는 모든 네트워크 메시지에 영구적으로 대역폭을 예약할 필요가 없다는 점을 반영한다. 안전 중요 제어 정보에는 보장된 통신 기회를 제공하면서 상대적으로 덜 결정론적인 트래픽은 동적으로 할당된 영역을 사용할 수 있다.

대역폭 예약(Bandwidth Reservation)은 정적 스케줄링의 중요한 결과이다. 특정 통신 기능에 슬롯이 할당되면 애플리케이션에서 새롭게 전송할 정보가 거의 없는 경우에도 설정된 스케줄에 따라 해당 전송 기회가 존재한다. 이는 시간적 보장(Temporal Guarantee)을 제공하지만 완전히 이벤트 기반으로 동작하는 통신보다 대역폭 활용 효율(Bandwidth Utilization Efficiency)이 낮아질 수 있다. 따라서 시간 트리거 아키텍처는 예측 가능성, 분석 가능성, 결정론적 동작을 확보하는 대신 일부 유연성과 대역폭 효율을 절충한다.

따라서 스케줄 설계(Schedule Design)는 중요한 시스템 엔지니어링(System Engineering) 활동이 된다. 엔지니어는 통신 사이클 파라미터, 슬롯 할당, 메시지 주기(Message Period), 페이로드 요구사항(Payload Requirement), 채널 할당(Channel Assignment), 분산 제어 기능 사이의 관계를 결정해야 한다. 잘못 설계된 스케줄은 대역폭을 낭비하거나 불필요한 지연을 만들 수 있지만 최적화된 스케줄은 충분한 통신 용량과 결정론적 특성을 동시에 제공할 수 있다. 따라서 네트워크 설정은 ECU 소프트웨어 타이밍 및 전체 시스템 아키텍처와 함께 개발되어야 한다.

시간 트리거 통신은 참여 노드들이 네트워크 타이밍에 대한 공통된 이해를 확립하기 전에는 정상적으로 동작할 수 없기 때문에 시작 과정(Startup)에서도 중요한 과제가 존재한다. 플렉스레이는 적절하게 설정된 노드들이 통신 사이클과 전역 시간을 설정하는 데 참여할 수 있도록 시작 및 동기화 메커니즘(Startup and Synchronization Mechanism)을 제공한다. 동기화가 완료된 이후 정상적인 스케줄 통신이 시작되며, 시스템 동작 중에는 이러한 동기화 상태를 지속적으로 유지해야 한다.

이중 채널 통신(Dual-Channel Communication)은 시간 트리거 아키텍처를 더욱 강화할 수 있다. 플렉스레이는 각각 최대 10 Mbit/s의 통신을 지원하는 채널 A(Channel A)와 채널 B(Channel B)를 제공한다. 결함 허용성(Fault Tolerance)이 필요한 경우 안전 중요 프레임을 두 채널에 중복 전송하거나, 추가 통신 용량이 더 중요한 경우 트래픽을 두 채널에 분산할 수 있다. 따라서 결정론적 스케줄링과 물리적 통신 이중화(Physical Communication Redundancy)를 결합하여 신뢰성 높은 분산 제어를 구현할 수 있다.

통신 동작이 시간적으로 제한되어 있으면 결함 격리(Fault Containment)도 보다 체계적으로 분석할 수 있다. 정상적으로 동작하는 노드는 네트워크를 임의로 점유하는 것이 아니라 설정된 통신 기회에 따라 전송해야 한다. 따라서 시스템 설계자는 예상되는 시간적 동작(Expected Temporal Behavior)을 정의하고 이로부터 벗어난 동작을 식별할 수 있다. 오류 검출(Error Detection), 이중 통신 경로, 동기화된 동작, 적절한 네트워크 토폴로지(Network Topology)와 함께 사용하면 구조적인 결함 허용 통신 아키텍처를 구성할 수 있다.

시간 트리거 통신은 특히 엑스 바이 와이어(X-by-Wire) 시스템 개발 과정에서 중요한 의미를 갖게 되었다. 전자식 조향(Electronic Steering), 제동, 서스펜션 및 관련 차량 제어 기능은 센서, 제어기, 액추에이터 사이의 신뢰성 높은 협조를 필요로 한다. 기존의 기계적 연결을 분산 전자 시스템으로 대체하거나 보완하면 통신 타이밍에 대한 의존성이 증가한다. 따라서 알려진 전송 기회, 제한된 지연 시간(Bounded Latency), 동기화된 제어기, 이중 채널은 안전 관련 분산 차량 기능에 중요한 특성을 제공한다.

동일한 원리는 자동차 시스템을 넘어 다양한 분야에 적용될 수 있다. 산업 자동화(Industrial Automation), 모바일 로봇(Mobile Robot), 자율주행 차량(Autonomous Vehicle), 피지컬 AI 플랫폼(Physical AI Platform)은 점점 더 많은 분산 센서, 임베디드 제어기, 모터 드라이브(Motor Drive), 안전 시스템, 고성능 컴퓨팅 노드를 포함하고 있다. 실제 사용하는 통신 기술은 플렉스레이와 다를 수 있지만 여러 물리적 구성요소가 실시간 제약 조건 아래에서 협조 동작해야 하는 경우 예측 가능한 스케줄링의 중요성은 동일하다.

로보틱스(Robotics)에서는 결정론적 저수준 제어 트래픽(Deterministic Low-Level Control Traffic)과 고대역폭 연산 트래픽(High-Bandwidth Computational Traffic)을 구분하는 것이 유용하다. 모터 명령, 액추에이터 상태, 비상 정보, 동기화된 제어 신호에는 제한된 타이밍이 필요한 반면 카메라, 라이다(LiDAR) 포인트 클라우드, 지도, AI 표현(AI Representation)은 훨씬 높은 대역폭을 필요로 한다. 이는 현대 로봇 시스템이 모든 정보를 동일한 타이밍 특성을 가진 하나의 네트워크로 처리하기보다 여러 통신 기술을 함께 사용하는 이유를 보여준다.

현대의 자동차 이더넷(Automotive Ethernet)과 시간 민감형 네트워킹(Time-Sensitive Networking, TSN)의 결합은 이더넷 호환성과 훨씬 높은 대역폭을 유지하면서 결정론적 통신을 구현하는 또 다른 접근 방법을 제공한다. CAN FD와 CAN XL 역시 기존 CAN 아키텍처의 기능을 확장한다. 이러한 기술은 플렉스레이와 구조적으로 다르지만 핵심적인 엔지니어링 문제는 동일하다. 서로 다른 우선순위와 대역폭 요구를 가진 트래픽이 통신 인프라를 공유하는 상황에서도 중요한 데이터가 허용 가능한 시간 안에 목적지에 도달해야 한다.

따라서 시간 트리거 통신은 단순히 플렉스레이의 한 가지 메커니즘이 아니라 보다 광범위한 아키텍처 원리(Architectural Principle)로 이해해야 한다. 핵심 개념은 전역 시간(Global Time), 주기적 통신 사이클, 미리 정의된 전송 기회, 제한된 지연 시간, 제어된 지터(Controlled Jitter), 분산 실행의 협조(Coordinated Distributed Execution)이다. 플렉스레이는 이러한 개념을 명확하게 구현한 사례로서 통신 타이밍을 실시간 사이버 물리 시스템(Real-Time Cyber-Physical System)의 명시적인 설계 요소로 만드는 방법을 이해하는 데 유용하다.

피지컬 AI 시스템에서는 인지(Perception), 추론(Reasoning), 제어(Control), 액추에이션이 서로 다른 컴퓨팅 장치에 분산되면서 이러한 원리가 더욱 중요해진다. 상위 수준 AI 의사결정은 상대적으로 낮은 주파수로 동작할 수 있지만 임베디드 제어기는 초당 수백 또는 수천 회의 액추에이터 제어 루프를 실행할 수 있다. 결정론적 통신 경계(Deterministic Communication Boundary)는 이러한 서로 다른 시간 영역(Temporal Domain)을 분리하면서 안전 중요 명령과 상태 정보가 예측 가능하고 동기화된 형태로 전달되도록 지원할 수 있다.

궁극적으로 시간 트리거 통신은 네트워크 접근을 불확실한 실행 시점의 경쟁에서 계획된 시간 자원(Planned Temporal Resource)으로 변화시킨다. 통신 대역폭, 전송 기회, 제어 실행, 동기화, 이중화를 하나의 시스템 아키텍처 안에서 함께 설계할 수 있다. 이것이 플렉스레이의 시간 트리거 설계가 제공하는 핵심적인 교훈이며, 현대의 결정론적 이더넷(Deterministic Ethernet), 안전 네트워크(Safety Network), 자율 시스템, 분산 로보틱스, 그리고 미래 피지컬 AI 통신 아키텍처에도 계속 적용될 수 있는 중요한 원리이다.

## 10.03. FlexRay X-by-Wire Application

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

플렉스레이(FlexRay)는 기존의 기계적 및 유압식 연결을 전자 센싱(Electronic Sensing), 통신(Communication), 연산(Computation), 액추에이션(Actuation)으로 대체하거나 보완하는 첨단 엑스 바이 와이어(X-by-Wire) 시스템을 지원하기 위한 목적으로 개발되었다. 스티어 바이 와이어(Steer-by-Wire), 브레이크 바이 와이어(Brake-by-Wire), 전자식 섀시 협조 제어(Electronically Coordinated Chassis Control)와 같은 응용에서는 액추에이터 동작이 분산 전자 제어 장치(Distributed Electronic Control Unit) 사이에서 교환되는 명령에 직접 의존할 수 있기 때문에 예측 가능한 통신 타이밍이 필요하다.

기존의 기계식 제어 시스템(Conventional Mechanical Control System)에서는 운전자의 입력이 샤프트(Shaft), 유압(Hydraulic Pressure), 케이블(Cable) 또는 기타 기계적 메커니즘을 통해 물리적으로 전달될 수 있다. 엑스 바이 와이어는 이러한 관계를 변화시켜 입력을 전기 신호(Electrical Signal)로 변환하고, 제어기가 이를 해석한 후 액추에이터로 전달하도록 한다. 따라서 통신 네트워크가 기능적 제어 경로(Functional Control Path)의 일부가 되며 지연 시간(Latency), 동기화(Synchronization), 이중화(Redundancy), 오류 검출(Error Detection), 가용성(Availability)이 중요한 시스템 수준의 설계 요소가 된다.

스티어 바이 와이어(Steer-by-Wire)는 대표적인 사례이다. 조향 입력 센서(Steering Input Sensor)가 운전자가 요구하는 조향 동작을 측정하면 전자 제어기는 필요한 실제 차륜 조향각(Road-Wheel Angle)을 계산하여 조향 액추에이터(Steering Actuator)에 명령한다. 차량 속도, 요 레이트(Yaw Rate), 휠 속도(Wheel Speed), 안정성 제어 상태(Stability-Control State) 등의 추가 정보도 명령 계산에 영향을 줄 수 있다. 센싱, 연산, 액추에이션이 조향 제어 루프 전체에서 협조되도록 통신 시스템은 이러한 신호를 예측 가능한 방식으로 전달해야 한다.

브레이크 바이 와이어(Brake-by-Wire)도 유사한 요구사항을 가진다. 제동 명령이 전자적으로 생성되어 제동 제어기와 휠 액추에이터(Wheel Actuator) 사이에 분산될 수 있기 때문이다. 차량 동역학 기능(Vehicle Dynamics Function)은 휠 슬립(Wheel Slip), 요 동작(Yaw Behavior), 안정성 목표에 따라 각 휠의 제동 명령을 개별적으로 수정할 수 있다. 통신 지연과 과도한 지터(Jitter)는 협조 제동 성능에 영향을 줄 수 있으므로 여러 제어기가 긴밀하게 결합된 제동 기능에 참여하는 경우 결정론적 전송 기회(Deterministic Transmission Opportunity)가 중요하다.

서스펜션 및 섀시 제어 시스템(Suspension and Chassis-Control System)도 동기화된 분산 통신(Synchronized Distributed Communication)의 이점을 활용할 수 있다. 전자식 서스펜션 제어기는 휠 움직임, 차체 가속도, 조향 상태, 차량 속도 및 기타 동역학 정보를 처리하여 감쇠력(Damping)이나 능동 서스펜션 힘(Active Suspension Force)을 조절할 수 있다. 조향, 제동, 서스펜션 제어기가 동기화된 정보를 공유하면 차량 수준 제어 알고리즘이 각 섀시 서브시스템을 독립적으로 처리하는 대신 여러 액추에이터를 통합적으로 협조 제어할 수 있다.

플렉스레이의 정적 세그먼트(Static Segment)는 안전 관련 제어 메시지에 미리 정의된 전송 슬롯(Predefined Transmission Slot)을 할당할 수 있기 때문에 이러한 응용에 특히 적합하다. 조향 명령, 제동 요청, 액추에이터 상태, 차량 동역학 신호는 설정된 통신 사이클에서 알려진 전송 기회를 확보할 수 있다. 경쟁 기반 통신(Contention-Based Communication)과 달리 스케줄된 전송은 네트워크 트래픽 변화에 따른 불확실성을 줄이며 엔지니어가 통신 지연을 분산 제어 시스템의 타이밍 예산(Timing Budget)에 포함하여 분석할 수 있도록 한다.

통신 사이클(Communication Cycle)은 센싱과 액추에이션 사이의 협조도 지원한다. 센서 측정값을 알려진 시점에 획득하고 할당된 슬롯에서 전송한 후 제어기가 이를 처리하고, 다시 정의된 통신 기회를 통해 액추에이터 명령을 전송할 수 있다. 이를 통해 센싱-통신-연산-통신-액추에이션(Sense-Communicate-Compute-Communicate-Actuate)의 구조화된 순서를 만들 수 있다. 이러한 시간적 구성은 개별 네트워크 메시지만 독립적으로 분석하는 대신 종단 간 응답 시간(End-to-End Response Time)을 평가하는 데 도움이 된다.

이러한 협조가 여러 ECU에 걸쳐 이루어지는 경우 전역 시간 동기화(Global Time Synchronization)가 필수적이다. 각 제어기는 자체 발진기(Oscillator)를 가지고 있으며 독립적인 클록은 자연스럽게 오프셋(Offset)과 드리프트(Drift)를 발생시킨다. 플렉스레이의 동기화 메커니즘은 참여 노드 전체에 충분히 일관된 전역 통신 시간(Global Communication Time)을 유지한다. 따라서 제어기는 로컬 소프트웨어 실행과 통신 이벤트를 공통 사이클 경계와 연계하여 분산 엑스 바이 와이어 기능의 시간적 일관성을 향상시킬 수 있다.

이중화(Redundancy)는 엑스 바이 와이어 아키텍처를 위한 플렉스레이의 또 다른 중요한 특성이다. 채널 A(Channel A)와 채널 B(Channel B)는 각각 최대 10 Mbit/s로 동작할 수 있는 두 개의 물리적 통신 경로를 제공한다. 통신 이중화가 필요한 경우 중요한 정보를 두 채널을 통해 전송할 수 있다. 배선, 커넥터(Connector), 트랜시버(Transceiver) 또는 기타 통신 장애로 하나의 경로를 사용할 수 없게 되면 시스템 아키텍처는 결함 허용 전략(Fault-Tolerance Strategy)의 일부로 이중화된 경로를 활용할 수 있다.

그러나 이중 채널 통신(Dual-Channel Communication)만으로 엑스 바이 와이어 시스템의 안전성이 보장되는 것은 아니다. 기능 안전(Functional Safety)은 센서, 전원 공급 장치(Power Supply), 프로세서(Processor), 통신 경로, 액추에이터, 진단(Diagnostics), 모니터링(Monitoring), 결함 처리(Fault Handling), 성능 저하 운전 전략(Degraded Operating Strategy)을 포함하는 전체 아키텍처에 의해 결정된다. 플렉스레이는 결정론적이고 이중화된 통신 기능을 제공하지만 요구되는 가용성과 결함 대응을 달성하려면 적절한 시스템 수준 안전 메커니즘과 결합되어야 한다.

네트워크 토폴로지(Network Topology) 역시 결함 격리(Fault Containment) 목표를 지원할 수 있다. 플렉스레이는 수동 버스(Passive Bus), 능동 스타(Active Star), 하이브리드(Hybrid) 구성을 지원한다. 능동 스타는 통신 분기를 전기적으로 격리하여 특정 물리적 결함이 다른 영역으로 전파되는 것을 제한할 수 있다. 따라서 설계자는 신뢰성 높은 분산 엑스 바이 와이어 시스템을 구성할 때 통신 토폴로지를 이중 채널, ECU 분할(ECU Partitioning), 전원 분배(Power Distribution), 배선 경로, 커넥터 아키텍처와 함께 고려할 수 있다.

예측 가능한 통신은 가변적인 네트워크 지연이 제어 루프 내부의 또 다른 불확실성으로 작용하기 때문에 폐루프 제어(Closed-Loop Control)에 특히 유용하다. 센서 정보와 액추에이터 명령이 크게 변화하는 지연 시간으로 도착하면 제어기 설계에서 추가적인 시간 변동을 고려해야 한다. 스케줄 기반 플렉스레이 통신은 제한된 전송 기회(Bounded Transmission Opportunity)를 제공하여 이러한 불확실성을 줄이고 제어 엔지니어가 제어기 주기와 네트워크 스케줄을 보다 체계적으로 조정할 수 있도록 한다.

엑스 바이 와이어 아키텍처는 메시지의 성공적인 전달만큼 정보 최신성(Information Freshness)이 중요한 이유도 보여준다. 제어 메시지가 정상적으로 수신되더라도 너무 늦게 도착하면 현재의 물리적 상태를 더 이상 정확하게 나타내지 못할 수 있다. 결정론적 스케줄은 정보가 언제 사용 가능해야 하는지에 대한 기대 시간을 설정하는 데 도움을 준다. 애플리케이션 소프트웨어는 타이밍 감시(Timing Supervision), 시퀀스 모니터링(Sequence Monitoring), 유효성 검사(Validity Check), 결함 처리를 결합하여 누락되거나 지연되거나 일관성이 없는 정보를 검출할 수 있다.

분산 엑스 바이 와이어 기능은 전원이 인가된 직후부터 동기화된 통신이 존재한다고 가정할 수 없기 때문에 시작 동작(Startup Behavior)도 특별히 고려해야 한다. 플렉스레이는 통신 사이클과 공통 시간 기준을 확립하는 시작 및 동기화 절차(Startup and Synchronization Procedure)를 포함한다. 시스템 설계자는 정상적인 폐루프 동작이 허용되기 전에 이러한 네트워크 상태를 ECU 초기화(ECU Initialization), 센서 준비 상태(Sensor Readiness), 액추에이터 활성화 조건(Actuator Enable Condition), 진단 및 안전 상태 관리(Safety-State Management)와 조정해야 한다.

결정론적 스케줄(Deterministic Schedule)은 제어 요구사항에 따라 설계되어야 한다. 빠른 신호는 모든 통신 사이클에서 전송되어야 할 수 있지만 상대적으로 느린 진단 또는 상태 정보는 더 낮은 빈도의 전송 기회를 사용할 수 있다. 따라서 엔지니어는 제어 루프 주파수(Control-Loop Frequency), 허용 가능한 지연 시간, 페이로드 크기(Payload Size), 이중화 요구사항, 사용 가능한 대역폭을 고려하여 슬롯과 메시지 주기를 할당한다. 이에 따라 통신 설계는 독립적인 배선 문제가 아니라 기능 아키텍처(Functional Architecture)와 긴밀하게 연결된다.

플렉스레이의 결정론적 스케줄링과 유연한 통신의 조합은 엑스 바이 와이어 네트워크에서 여러 종류의 트래픽을 처리할 때 유용하다. 중요한 주기적 명령은 정적 세그먼트(Static Segment)를 사용할 수 있으며, 시간 요구가 상대적으로 낮은 정보는 동적 세그먼트(Dynamic Segment)를 사용할 수 있다. 이러한 분리를 통해 제어 기능에는 예측 가능한 통신 자원을 예약하면서 다른 통신 요구에는 일정한 유연성을 제공할 수 있지만 비효율적인 대역폭 할당을 방지하기 위해 신중한 스케줄 엔지니어링(Schedule Engineering)이 필요하다.

차량 수준 협조 제어(Vehicle-Level Coordination)는 이러한 아키텍처의 보다 광범위한 가치를 보여준다. 조향, 제동, 서스펜션, 파워트레인(Powertrain), 차량 동역학 제어기는 운전자의 요구와 환경 조건에 대한 통합적인 반응을 생성하기 위해 정보를 교환할 수 있다. 동기화된 네트워크는 이러한 분산 기능에 공통된 시간적 프레임워크(Temporal Framework)를 제공한다. 따라서 차량을 독립적으로 동작하는 여러 전자 서브시스템의 집합이 아니라 통합된 메카트로닉 제어 시스템(Integrated Mechatronic Control System)으로 다룰 수 있다.

이러한 개념은 플렉스레이 자체를 사용하지 않는 자율주행 차량(Autonomous Vehicle)과 모바일 로봇(Mobile Robot)에도 적용될 수 있다. 로봇에는 인지와 계획(Perception and Planning)을 수행하는 엣지 컴퓨터(Edge Computer), 빠른 제어 루프를 실행하는 임베디드 제어기(Embedded Controller), 액추에이터를 제어하는 모터 드라이브(Motor Drive), 움직임을 독립적으로 감시하는 안전 제어기(Safety Controller)가 존재할 수 있다. 여기에서도 동일한 아키텍처 문제가 발생하며 상위 수준의 의사결정은 궁극적으로 신뢰성 있고 적시에 검증 가능한 물리적 액추에이터 명령으로 변환되어야 한다.

이러한 로봇 시스템에서는 고대역폭 인지 트래픽(High-Bandwidth Perception Traffic)과 결정론적 액추에이터 트래픽(Deterministic Actuator Traffic)이 서로 다른 통신 요구사항을 가지는 경우가 많다. 카메라 영상, 라이다(LiDAR) 포인트 클라우드, 지도, AI 표현(AI Representation)은 이더넷 수준의 높은 대역폭이 적합하지만 모터 명령, 조향 상태, 제동 명령, 비상 신호, 액추에이터 피드백은 예측 가능한 지연 시간이 중요하다. 따라서 엑스 바이 와이어 경험은 타이밍, 대역폭, 안전 요구사항에 따라 통신 영역을 분리하는 데 유용한 원칙을 제공한다.

현대의 자동차 이더넷(Automotive Ethernet)과 시간 민감형 네트워킹(Time-Sensitive Networking, TSN)은 훨씬 높은 대역폭과 이더넷 기반 컴퓨팅 플랫폼과의 통합을 지원하면서 결정론적 통신 기능을 제공하고 있다. CAN FD와 CAN XL 역시 CAN 기반 아키텍처를 확장한다. 따라서 플렉스레이는 새로운 통신 아키텍처가 주류가 되기 이전에 분산 안전 관련 차량 제어를 지원하기 위해 결정론적 네트워킹이 어떻게 도입되었는지를 이해할 수 있는 중요한 역사적·공학적 기준이 된다.

피지컬 AI(Physical AI)의 관점에서 플렉스레이 엑스 바이 와이어 응용이 제공하는 더 중요한 교훈은 지능적 의사결정(Intelligent Decision Making)을 물리적 실행 타이밍(Physical Execution Timing)과 분리해서 생각할 수 없다는 것이다. AI 시스템이 환경을 인지하고 정교한 움직임을 결정하더라도 이러한 결정은 결국 임베디드 제어 및 통신 계층을 거쳐 조향 시스템, 모터, 브레이크, 관절(Joint), 기타 액추에이터로 전달된다. 지능과 물리적 행동 사이에서는 예측 가능한 타이밍, 동기화, 이중화, 진단, 결함 격리가 계속해서 필수적인 요소가 된다.

따라서 플렉스레이 엑스 바이 와이어 아키텍처는 통신이 단순한 데이터 전송(Data Transport)에서 분산 제어(Distributed Control)의 핵심 구성요소로 변화하는 과정을 보여준다. 동기화된 시간, 결정론적 스케줄링, 이중 채널, 구조화된 토폴로지, 협조된 ECU 실행(Coordinated ECU Execution)을 결합함으로써 네트워크는 물리적 제어의 타이밍 아키텍처에 직접 참여할 수 있다. 이러한 원리는 현대 자율주행 차량, 로보틱스, 안전 중요 시스템(Safety-Critical Machine), 분산 피지컬 AI 시스템에서도 여전히 중요한 의미를 가진다.

## 10.04. FlexRay vs CAN FD Comparison

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

플렉스레이(FlexRay)와 CAN FD는 클래식 CAN(Classical CAN)을 넘어 통신 성능을 향상시키기 위한 서로 다른 두 가지 접근 방식을 나타낸다. 플렉스레이는 분산된 안전 관련 제어를 위한 결정론적 시간 트리거 통신(Deterministic Time-Triggered Communication)을 중심으로 설계되었으며, CAN FD는 데이터 페이로드(Data Payload)를 확장하고 데이터 구간(Data Phase)의 전송 속도를 높이는 방식으로 기존 CAN 아키텍처를 발전시켰다. 따라서 두 기술의 차이는 단순한 대역폭뿐만 아니라 매체 접근(Media Access), 타이밍 동작, 복잡성, 비용, 시스템 아키텍처까지 포함한다.

플렉스레이는 채널(Channel)당 최대 10 Mbit/s의 공칭 데이터 전송률(Nominal Data Rate)을 제공하며 두 개의 독립적인 통신 채널을 지원한다. CAN FD는 공칭 비트 전송률(Nominal Bit Rate)에서 CAN 방식의 중재를 유지하면서 비트 전송률 전환(Bit Rate Switching)을 통해 프레임의 데이터 부분을 더 높은 비트 전송률로 동작시킬 수 있다. 또한 CAN FD는 클래식 CAN의 최대 8바이트였던 페이로드를 최대 64바이트까지 확장하여 더 많은 제어 또는 진단 데이터를 전송할 때 효율성을 향상시킨다.

가장 근본적인 아키텍처 차이는 통신 매체에 대한 접근 방식을 어떻게 구성하는가에 있다. 플렉스레이는 통신을 정적 세그먼트(Static Segment)와 동적 세그먼트(Dynamic Segment)를 포함하는 반복적인 통신 사이클(Communication Cycle)로 구분한다. 정적 세그먼트에서는 메시지에 미리 결정된 시간 슬롯(Time Slot)을 할당하여 결정론적 전송 기회를 제공한다. 반면 CAN FD는 CAN으로부터 계승된 식별자 기반 중재(Identifier-Based Arbitration)를 사용하여 버스가 사용 가능한 상황에서 노드가 전송을 시작하며, 동시에 전송을 시도하면 높은 우선순위 식별자가 접근 권한을 획득한다.

이러한 차이는 지연 시간(Latency) 특성에 직접적인 영향을 준다. 적절하게 구성된 플렉스레이의 정적 스케줄(Static Schedule)에서는 특정 프레임이 언제 통신 기회를 얻는지 알 수 있으므로 최대 통신 지연을 높은 수준으로 예측할 수 있다. CAN FD의 우선순위 기반 중재 규칙 자체는 명확하게 정의되어 있지만 실제 메시지 지연 시간은 현재 버스 트래픽, 메시지 우선순위, 프레임 길이, 경쟁 전송에 따라 달라진다. 따라서 플렉스레이는 스케줄 기반의 시간적 예측 가능성(Temporal Predictability) 측면에서 더 강한 특성을 제공한다.

CAN FD는 클래식 CAN과의 점진적인 발전 관계(Evolutionary Relationship)에서 중요한 실용적 장점을 얻는다. 기존 CAN 엔지니어링 개념, 트랜시버(Transceiver) 기술, 소프트웨어 아키텍처, 진단 방식, 개발 경험을 네트워크의 기본 철학을 완전히 재설계하지 않고 CAN FD로 확장할 수 있는 경우가 많다. 반면 플렉스레이는 전역 동기화(Global Synchronization), 통신 사이클 설정, 정적 슬롯 할당, 시작 동작(Startup Behavior), 보다 광범위한 시스템 수준 스케줄 엔지니어링을 포함하는 전문화된 통신 아키텍처를 필요로 한다.

페이로드 용량(Payload Capacity) 역시 두 기술을 구분하는 중요한 요소이다. 플렉스레이 프레임은 클래식 CAN보다 상당히 많은 애플리케이션 데이터를 전송할 수 있으며, CAN FD는 CAN의 페이로드를 프레임당 최대 64바이트까지 명확하게 확장한다. 더 큰 페이로드를 사용하면 애플리케이션 정보를 여러 개의 짧은 CAN 프레임으로 나누면서 발생하는 오버헤드(Overhead)를 줄일 수 있다. 따라서 CAN FD는 CAN 네트워크의 기본적인 분산 및 우선순위 중심 동작을 유지하면서 더 높은 데이터 용량이 필요한 경우 특히 매력적인 기술이 되었다.

플렉스레이의 이중 채널 아키텍처(Dual-Channel Architecture)는 또 다른 중요한 차이를 제공한다. 채널 A(Channel A)와 채널 B(Channel B)를 이중화하여 중요한 정보를 두 개의 통신 경로로 전달하거나 서로 다른 트래픽을 각각의 채널에 분배하여 통신 용량을 증가시킬 수 있다. CAN FD 네트워크는 일반적으로 하나의 논리적 버스 세그먼트(Logical Bus Segment)를 기반으로 하지만 여러 CAN 네트워크, 게이트웨이(Gateway), 이중화된 제어기 또는 기타 아키텍처 메커니즘을 사용하여 시스템 수준에서 이중화를 구현할 수 있다.

시간 동기화(Time Synchronization)는 플렉스레이의 동작에 훨씬 더 깊게 통합되어 있다. 참여 노드는 통신 시간에 대한 공통된 이해를 유지하여 통신 사이클 경계와 할당된 전송 슬롯을 식별할 수 있다. 클록 오프셋(Clock Offset)과 드리프트(Drift)는 동기화 메커니즘을 통해 지속적으로 관리된다. CAN FD는 정상적인 버스 중재를 위해 이러한 형태의 전역 동기화 시간 트리거 스케줄을 요구하지 않으므로 기본적인 네트워크 설정은 더 단순하지만 본질적인 시간적 협조 수준은 플렉스레이와 다르다.

따라서 플렉스레이는 통신 타이밍 자체가 제어 설계의 일부를 구성하는 분산 제어 아키텍처(Distributed Control Architecture)에 자연스럽게 적합하다. 센서 샘플링(Sensor Sampling), 메시지 전송, 제어기 실행, 액추에이터 명령을 알려진 통신 사이클을 중심으로 조정할 수 있다. 반면 CAN FD는 모든 참여 ECU가 전역적으로 설정된 정적 통신 스케줄 내에서 동작할 필요 없이 메시지 우선순위와 이벤트 발생에 따라 효율적으로 정보를 교환해야 하는 경우에 적합한 경우가 많다.

네트워크 활용률(Network Utilization)은 중요한 절충 관계를 보여준다. 플렉스레이의 정적 슬롯은 해당 애플리케이션이 항상 새로운 정보를 전송할 필요가 없는 경우에도 통신 기회를 예약한다. 이러한 예약은 예측 가능한 지연 시간을 제공하지만 일부 대역폭이 활용되지 않을 수 있다. CAN FD는 실제 메시지 요구에 따라 통신이 이루어지기 때문에 이벤트 기반 트래픽(Event-Driven Traffic)에서 효율적인 활용이 가능하지만 버스 부하가 증가하면 낮은 우선순위 프레임이 추가적인 지연을 경험할 가능성도 증가한다.

플렉스레이의 동적 세그먼트(Dynamic Segment)는 미니슬롯 기반 접근(Minislot-Based Access)을 사용하여 완전히 정적인 스케줄링에서 발생하는 일부 경직성을 줄인다. 따라서 플렉스레이는 고정된 시분할 네트워크만을 의미하는 것이 아니라 매우 결정론적인 정적 통신과 보다 유연한 동적 트래픽을 결합한다. 그러나 전체적인 아키텍처는 여전히 동기화된 통신 사이클을 중심으로 구성된다. CAN FD는 개념적으로 기존의 이벤트 트리거 CAN 통신(Event-Triggered CAN Communication)에 더 가까우며 일반적으로 스케줄 중심의 통합 작업이 적게 필요하다.

결함 허용성(Fault Tolerance) 역시 구현 철학에서 차이가 있다. 플렉스레이는 이중 통신 채널, 동기화된 동작, 구조화된 시작 절차, 결함 격리(Fault Containment)를 지원할 수 있는 능동 스타(Active Star)를 포함한 토폴로지 옵션 등의 특성을 고려하여 개발되었다. CAN FD는 CAN에서 사용되는 강력한 오류 검출(Error Detection), 결함 제한(Fault Confinement), 재전송(Retransmission), 차동 물리 계층(Differential Physical Layer)의 개념을 계승한다. 두 기술 모두 신뢰성 높은 시스템을 지원할 수 있지만 최종적인 안전성은 통신 프로토콜 하나가 아니라 전체 전자 아키텍처에 의해 결정된다.

물리적 토폴로지(Physical Topology)와 배선은 아키텍처 비용에도 영향을 미친다. 플렉스레이는 버스(Bus), 능동 스타, 하이브리드(Hybrid) 구성을 사용할 수 있으며 이중 채널을 구현하면 추가적인 배선, 트랜시버, 커넥터(Connector), 네트워크 구성요소가 필요할 수 있다. CAN FD는 일반적으로 CAN과 연관된 비교적 단순한 선형 버스 토폴로지(Linear Bus Topology)를 유지한다. 결정론적인 이중 채널 통신이 필요하지 않은 응용에서는 CAN FD의 낮은 아키텍처 복잡성이 비용과 패키징 측면에서 중요한 장점이 될 수 있다.

개발 과정(Development Process)도 이에 따라 달라진다. 플렉스레이에서는 엔지니어가 사이클 타이밍, 슬롯 할당, 프레임 식별자, 통신 주기, 채널 사용, 동기화 파라미터, 시작 설정을 정의해야 한다. 하나의 통신 기능 변경이 전체 스케줄에 영향을 줄 수도 있다. CAN FD 네트워크 엔지니어링 역시 비트 타이밍(Bit Timing), 버스 부하 분석(Bus-Load Analysis), 식별자 할당, 페이로드 설계, 물리 계층 검증, 지연 시간 분석이 필요하지만 메시지를 추가할 때 일반적으로 전역 시간 슬롯 스케줄 전체를 다시 구성할 필요는 없다.

엑스 바이 와이어(X-by-Wire) 및 긴밀하게 협조되는 섀시 기능(Chassis Function)에서는 플렉스레이가 역사적으로 강력한 장점을 제공하였다. 조향, 제동, 서스펜션, 차량 동역학 제어기는 예측 가능한 시간 구간에서 정보를 교환할 수 있었으며 이중 채널은 이중화를 위한 아키텍처 옵션을 제공하였다. CAN FD 역시 많은 실시간 제어 기능을 지원할 수 있지만 설계자는 미리 정의된 정적 전송 슬롯에 의존하는 대신 중재 및 버스 부하 조건에서의 최악 지연 시간(Worst-Case Latency)을 검증해야 한다.

진단(Diagnostics), 차체 제어(Body Control), 파워트레인 통신(Powertrain Communication), 액추에이터 네트워크, 일반적인 ECU 상호작용에서는 CAN FD가 성능과 엔지니어링 단순성 사이에서 좋은 균형을 제공한다. 더 큰 페이로드와 빠른 데이터 구간은 플렉스레이와 관련된 전체적인 스케줄링 및 동기화 프레임워크를 도입하지 않고도 클래식 CAN보다 상당한 성능 향상을 제공한다. 이러한 점진적 호환성(Evolutionary Compatibility)은 차량 제조사가 불필요한 아키텍처 복잡성을 증가시키지 않으면서 네트워크 성능을 향상시키는 데 중요한 요소가 되었다.

이러한 비교는 로보틱스(Robotics)와 자율이동로봇(Autonomous Mobile Robot, AMR)에서도 특히 의미가 있다. 모터 제어기(Motor Controller), 조향 시스템, 배터리 관리 시스템(Battery Management System), 안전 제어기(Safety Controller), 임베디드 장치(Embedded Device)는 비교적 작은 크기의 제어 메시지를 빈번하게 교환하며 이러한 경우 CAN FD가 실용적인 선택이 될 수 있다. 여러 액추에이터가 엄격하게 동기화된 실행을 요구한다면 플렉스레이와 유사한 결정론적 아키텍처가 개념적으로 유용하지만 추가적인 스케줄링, 하드웨어, 통합 복잡성이 실제 타이밍 및 안전 요구사항에 의해 정당화되어야 한다.

플렉스레이와 CAN FD 어느 것도 모든 종류의 데이터를 위한 고대역폭 네트워크를 대체하기 위한 기술은 아니다. 카메라, 고해상도 라이다(LiDAR), 레이더 데이터 스트림(Radar Stream), 지도, AI 표현(AI Representation)은 일반적인 제어 네트워크의 요구 수준을 훨씬 초과하는 대역폭을 필요로 할 수 있다. 따라서 현대 차량과 피지컬 AI(Physical AI) 시스템은 임베디드 제어에는 CAN 계열 네트워크를 사용하고 고대역폭 통신에는 자동차 이더넷(Automotive Ethernet)을 사용하는 방식으로 여러 네트워크를 결합하며, 게이트웨이 또는 도메인 제어기(Domain Controller)가 이들 네트워크 영역 사이의 정보를 조정하는 경우가 증가하고 있다.

시간 민감형 네트워킹(Time-Sensitive Networking, TSN)이 결합된 자동차 이더넷은 이더넷 수준의 대역폭과 함께 제한된 지연 시간(Bounded Latency), 트래픽 스케줄링(Traffic Scheduling), 동기화, 결정론적 통신을 위한 메커니즘을 제공함으로써 이러한 비교 구도를 더욱 변화시키고 있다. 이는 현대의 중앙 집중형 및 존 기반 컴퓨팅 아키텍처(Centralized and Zonal Computing Architecture)를 지원하면서 과거 전용 결정론적 네트워크가 필요했던 여러 요구사항을 해결한다. 결과적으로 플렉스레이의 아키텍처적 역할은 감소했지만 결정론적 통신 원리는 여전히 기술적으로 중요하다.

CAN FD는 이와 다른 발전 경로를 따랐으며 이미 광범위하게 사용되는 CAN 생태계(Ecosystem)를 확장한다는 점에서 지속적인 가치를 갖는다. CAN의 기본적인 중재 철학을 대체하는 대신 프레임당 전송되는 데이터의 양을 증가시키고 지원되는 구간에서 프레임 전송을 가속한다. 따라서 CAN FD는 점진적인 네트워크 진화(Incremental Network Evolution)를 의미하는 반면, 플렉스레이는 전역적으로 동기화되고 스케줄된 분산 통신을 향한 보다 근본적인 아키텍처 변화를 의미한다.

시스템 엔지니어링(System Engineering)의 관점에서 두 기술의 선택을 단순한 최대 비트 전송률 비교로 축소해서는 안 된다. 플렉스레이는 결정론적 타이밍, 동기화된 실행, 설정 가능한 이중화(Configurable Redundancy), 예측 가능한 분산 제어를 강조한다. CAN FD는 CAN 개념과의 호환성, 유연한 우선순위 기반 통신, 증가된 페이로드 용량, 향상된 처리량(Throughput), 비교적 단순한 구축을 강조한다. 적절한 아키텍처는 타이밍 보장, 안전 목표, 네트워크 부하, 비용, 복잡성, 수명주기 요구사항(Lifecycle Requirement)에 따라 결정되어야 한다.

피지컬 AI의 관점에서 이러한 비교는 보다 광범위한 아키텍처 원리를 보여준다. 상위 수준 지능(High-Level Intelligence), 임베디드 제어(Embedded Control), 물리적 액추에이션(Physical Actuation)은 서로 다른 시간 척도(Time Scale)에서 동작하며 서로 다른 통신 요구사항을 생성한다. 일부 정보는 이벤트 기반 우선순위 접근(Event-Driven Priority Access)이 적합하지만 안전 중요 제어에는 제한되고 동기화된 타이밍이 필요할 수 있다. 따라서 플렉스레이와 CAN FD를 이해하는 것은 단순한 대역폭이 아니라 물리 시스템의 시간적 동작(Temporal Behavior)을 기준으로 적절한 통신 메커니즘을 선택하는 데 도움이 된다.

## 10.05. FlexRay Legacy and Decline

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

플렉스레이(FlexRay)는 결정론적(Deterministic), 동기화(Synchronized), 결함 허용(Fault-Tolerant) 통신이 분산 전자 제어(Distributed Electronic Control)를 어떻게 지원할 수 있는지를 보여주었다는 점에서 자동차 네트워킹(Automotive Networking)의 역사에서 중요한 위치를 차지한다. 플렉스레이는 특히 조향, 제동, 서스펜션, 차량 동역학을 포함하는 협조 섀시 제어(Coordinated Chassis Control)와 엑스 바이 와이어(X-by-Wire)처럼 기존 CAN이 충분히 대응하기 어려운 타이밍과 신뢰성 요구를 가진 응용을 위해 개발되었다.

플렉스레이의 기술적 중요성은 통신 타이밍(Communication Timing)을 설계 가능한 시스템 자원(System Resource)으로 다루었다는 데 있다. 플렉스레이는 트래픽을 정적 세그먼트(Static Segment)와 동적 세그먼트(Dynamic Segment)를 포함하는 반복적인 통신 사이클(Communication Cycle)로 구성하여 중요한 메시지에 미리 정의된 전송 기회를 제공하였다. 전역 시간 동기화(Global Time Synchronization)는 분산 ECU를 스케줄에 맞추어 동작하게 하였으며, 결정론적 슬롯은 제어 시스템의 타이밍 분석에 직접 포함할 수 있는 예측 가능한 지연 시간을 제공하였다.

이 프로토콜은 통신 이중화(Communication Redundancy)에도 강한 아키텍처적 중점을 두었다. 일반적으로 채널 A(Channel A)와 채널 B(Channel B)라고 하는 두 개의 독립적인 채널을 통해 안전 관련 정보를 중복 전송하거나 각각의 채널을 추가 대역폭 확보에 사용할 수 있었다. 버스(Bus), 능동 스타(Active Star), 하이브리드(Hybrid) 토폴로지와 결합함으로써 통신 가용성(Communication Availability), 물리적 결함 격리(Physical Fault Isolation), 신뢰성 높은 분산 제어를 명시적으로 고려하는 네트워크를 구성할 수 있었다.

이러한 기능은 자동차 전자 시스템이 점점 더 분산화되던 시기에 플렉스레이를 기술적으로 매력적인 선택으로 만들었다. 전자식 조향(Electronic Steering), 제동, 서스펜션, 파워트레인(Powertrain), 안정성 제어(Stability Control)는 여러 제어기 사이에서 더욱 빈번한 협조를 필요로 했다. 플렉스레이는 채널당 최대 10 Mbit/s의 통신 성능과 결정론적 스케줄링(Deterministic Scheduling)을 제공하여 여러 ECU 사이의 예측 가능한 실시간 상호작용을 위해 특별히 최적화된 통신 아키텍처를 제공하였다.

그러나 플렉스레이에 결정론적 동작을 제공했던 바로 그 특성들이 엔지니어링 복잡성(Engineering Complexity)을 증가시키는 원인이 되기도 했다. 정적 통신 슬롯은 동작 전에 계획되어야 했으며 통신 사이클을 세밀하게 설정하고 참여 노드들을 동기화해야 했다. 따라서 엔지니어는 메시지 주기, 슬롯 할당(Slot Assignment), 페이로드(Payload), 채널, ECU 실행 타이밍, 시작 동작(Startup Behavior), 네트워크 설정을 전체 시스템 아키텍처의 상호 연결된 요소로 함께 조정해야 했다.

이러한 스케줄 중심 접근(Schedule-Oriented Approach)은 이후의 시스템 변경을 더욱 어렵게 만들 수 있었다. 메시지를 추가하거나 메시지 주기를 변경하는 작업이 항상 국부적인 수정만으로 끝나는 것은 아니었다. 통신 자원이 전역적으로 설정된 스케줄을 중심으로 구성되어 있기 때문에 여러 ECU에 걸쳐 슬롯 할당, 타이밍 관계, 대역폭 분배를 다시 검토해야 할 수 있었다. 이는 메시지를 우선순위와 사용 가능한 버스 용량에 따라 더욱 유연하게 추가할 수 있는 통신 아키텍처보다 높은 통합 작업 부담을 발생시켰다.

예약된 대역폭(Reserved Bandwidth) 역시 하나의 절충 요소였다. 정적 슬롯은 관련 기능에 새로운 정보가 거의 없는 경우에도 통신 스케줄의 일부로 유지된다. 이러한 자원 예약은 예측 가능한 전송 기회를 보장한다는 점에서 가치가 있지만 대역폭 활용 효율(Bandwidth Utilization Efficiency)을 감소시킬 수 있다. 특히 메시지가 엄격한 주기성을 갖지 않고 불규칙하게 발생하는 기능에서는 이벤트 중심 통신(Event-Oriented Communication)이 네트워크 용량을 더욱 동적으로 활용할 수 있다.

CAN FD는 기존 CAN의 통신 철학을 유지하면서 성능을 향상시켰기 때문에 많은 응용에서 실용적인 대안으로 등장하였다. CAN FD는 최대 페이로드를 64바이트까지 확장하고 비트 전송률 전환(Bit Rate Switching)을 통해 데이터 구간(Data Phase)을 더욱 빠르게 전송할 수 있도록 하였다. 따라서 엔지니어는 식별자 기반 중재(Identifier-Based Arbitration), 기존 CAN 엔지니어링 방식, 익숙한 진단 개념, 광범위한 제어기와 개발 도구 생태계를 유지하면서 데이터 전송 효율을 크게 향상시킬 수 있었다.

CAN FD와의 이러한 차이는 엄격한 시간 트리거 통신(Time-Triggered Communication)이 필요하지 않은 기능에서 플렉스레이의 매력이 감소한 이유 중 하나를 보여준다. 시스템이 CAN FD를 이용하여 최악 조건의 지연 시간(Worst-Case Latency), 신뢰성, 대역폭 요구사항을 만족할 수 있다면 플렉스레이의 추가적인 스케줄링 및 동기화 복잡성을 정당화하기 어려울 수 있다. 따라서 CAN FD는 완전히 다른 통신 아키텍처를 도입하는 대신 많은 임베디드 제어 네트워크(Embedded Control Network)를 점진적으로 발전시킬 수 있는 경로를 제공하였다.

대역폭 요구의 반대편에서는 카메라, 레이더(Radar), 고해상도 센서, 인포테인먼트 시스템(Infotainment System), 중앙 집중형 컴퓨팅(Centralized Computing), 첨단 운전자 보조 시스템(Advanced Driver-Assistance System)이 훨씬 많은 데이터를 생성하면서 자동차 이더넷(Automotive Ethernet)의 중요성이 증가하였다. 이더넷 기술은 수백 메가비트에서 기가비트급 통신으로 확장할 수 있는 경로를 제공하였으며, 상대적으로 낮은 대역폭을 가진 특수 목적 제어 네트워크는 증가하는 차량 데이터를 전달하는 데 점차 적합하지 않게 되었다.

시간 민감형 네트워킹(Time-Sensitive Networking, TSN)은 결정론적 통신과 관련된 메커니즘을 제공하면서 이더넷의 역할을 더욱 강화하였다. 시간 동기화(Time Synchronization), 트래픽 스케줄링(Traffic Scheduling), 우선순위화(Prioritization), 제한된 지연 시간(Bounded Latency), 트래픽 셰이핑(Traffic Shaping)을 통해 이더넷 네트워크는 과거 특수한 실시간 프로토콜이 필요했던 요구사항을 처리할 수 있게 되었다. 이에 따라 별도의 플렉스레이 영역을 유지하는 대신 고대역폭과 결정론적 통신을 공통 이더넷 기반 아키텍처에서 결합할 수 있는 경로가 형성되었다.

차량 전기전자 아키텍처(Vehicle Electrical and Electronic Architecture) 자체도 변화하였다. 기존 차량은 여러 특수 버스를 통해 연결된 다수의 분산 ECU를 사용했지만 새로운 설계에서는 도메인 제어기(Domain Controller), 중앙 컴퓨팅 플랫폼(Central Computing Platform), 존 아키텍처(Zonal Architecture)의 사용이 증가하고 있다. 컴퓨팅 기능이 통합되면서 통신 요구사항은 고용량 백본 네트워크(High-Capacity Backbone Network)와 보다 단순한 로컬 네트워크의 조합으로 이동하고 있다. 이러한 아키텍처 변화는 많은 분산 제어 ECU를 연결하기 위한 특수한 전역 스케줄 버스의 필요성을 감소시킨다.

비용과 패키징(Packaging) 역시 이러한 전환을 강화하였다. 이중 채널 플렉스레이 구현에는 추가적인 트랜시버(Transceiver), 배선, 커넥터(Connector), 네트워크 인터페이스(Network Interface), 경우에 따라 능동 스타 구성요소가 필요할 수 있다. 이러한 구성요소는 공간을 차지하고 와이어 하니스(Wire Harness)의 복잡성을 증가시키며 시스템 비용에 영향을 준다. 다른 네트워크가 더 적은 특수 구성요소로 기능 요구사항을 만족할 수 있다면 차량 제조사는 통신 아키텍처를 단순화하고 하드웨어 종류를 줄이려는 강한 동기를 갖게 된다.

소프트웨어와 툴체인 복잡성(Toolchain Complexity)도 프로토콜의 장기적인 생존에 영향을 미친다. 통신 기술은 물리 계층 하드웨어뿐만 아니라 설정 도구(Configuration Tool), 미들웨어(Middleware), 진단 시스템, 시험 장비, 개발 프로세스, 공급업체 전문성, 장기적인 부품 공급에 의해 지원되어야 한다. 널리 사용되는 생태계를 기반으로 하는 기술은 엔지니어링 지식과 인프라를 다양한 응용 및 여러 제품 세대에서 재사용할 수 있기 때문에 상당한 경제적 이점을 확보할 수 있다.

따라서 플렉스레이의 쇠퇴(Decline)를 결정론적 통신이 불필요해졌다는 의미로 해석해서는 안 된다. 오히려 플렉스레이가 해결하고자 했던 많은 문제는 현대 차량 엔지니어링에서도 여전히 핵심적인 과제로 남아 있다. 예측 가능한 지연 시간, 클록 동기화(Clock Synchronization), 트래픽 스케줄링, 이중화, 결함 격리(Fault Containment), 협조된 분산 실행(Coordinated Distributed Execution)은 계속 중요하다. 변화한 것은 이러한 특성을 구현할 수 있는 대체 기술의 등장과 전체적인 기술 환경이다.

플렉스레이의 유산(Legacy)은 시간 트리거 시스템(Time-Triggered System)을 둘러싼 엔지니어링 방법론에서 특히 분명하게 나타난다. 플렉스레이는 센서 획득(Sensor Acquisition), 메시지 전송, 연산, 액추에이터 명령이 알려진 시간 관계를 따르도록 통신 스케줄과 제어 실행을 함께 설계할 수 있음을 보여주었다. 이러한 접근은 네트워크를 독립적으로 동작하는 소프트웨어 구성요소 사이의 단순한 투명 통신 채널로 보는 대신 종단 간 타이밍(End-to-End Timing)을 분석하도록 엔지니어링 관점을 확장하였다.

이 프로토콜은 결정론(Determinism)과 유연성(Flexibility) 사이의 절충 관계에 대해서도 중요한 교훈을 제공한다. 자원을 사전에 예약하면 최악 조건의 타이밍을 더욱 쉽게 보장할 수 있지만 경직된 할당은 적응성과 자원 활용 효율을 감소시킬 수 있다. 이벤트 기반 통신(Event-Driven Communication)은 더 높은 유연성을 제공하지만 지연 시간이 트래픽 조건에 더욱 의존하게 된다. 현대 실시간 네트워킹 역시 우선순위 메커니즘, 스케줄된 트래픽, 트래픽 셰이핑, 자원 예약(Resource Reservation)을 통해 본질적으로 동일한 절충 문제를 해결하고 있다.

로보틱스(Robotics)와 자율이동로봇(Autonomous Mobile Robot, AMR)에서도 이러한 역사는 여전히 의미가 있다. 임베디드 모터 제어기(Embedded Motor Controller), 안전 장치, 조향 시스템, 배터리 제어기, 센서, 엣지 컴퓨터(Edge Computer), AI 프로세서는 매우 서로 다른 주파수에서 동작하며 다양한 종류의 트래픽을 생성한다. 하나의 통신 기술이 모든 영역에 최적일 가능성은 낮다. CAN FD는 비교적 작은 임베디드 제어 트래픽을 처리하고, 이더넷(Ethernet)은 인지 데이터와 고성능 컴퓨터 사이의 통신을 담당할 수 있다.

피지컬 AI(Physical AI) 시스템에서는 이러한 구분이 더욱 중요해진다. AI 인지(Perception)와 추론(Reasoning)은 초당 수십 회 수준으로 동작할 수 있지만 임베디드 모션 제어기(Embedded Motion Controller)는 초당 수백 또는 수천 회의 제어 주기를 실행할 수 있다. 카메라와 라이다(LiDAR) 데이터는 높은 대역폭을 요구하지만 액추에이터 명령과 안전 상태(Safety State)는 예측 가능한 지연 시간을 필요로 한다. 플렉스레이의 유산은 최대 처리량(Maximum Throughput)만이 아니라 타이밍 등급(Timing Class), 제어 중요도(Control Criticality), 대역폭, 결함 요구사항에 따라 네트워크 아키텍처를 설계해야 하는 이유를 보여준다.

플렉스레이에서 CAN FD와 자동차 이더넷으로 이어지는 역사적 변화는 기술적으로 정교한 프로토콜이 특정 한 가지 측면에서 뛰어난 성능을 제공한다는 이유만으로 지속적으로 생존하는 것은 아니라는 사실도 보여준다. 성공적인 통신 아키텍처는 성능뿐만 아니라 비용, 확장성(Scalability), 설정 작업, 호환성, 부품 가용성(Component Availability), 소프트웨어 생태계, 정비성(Serviceability), 미래 아키텍처 방향 사이에서 균형을 이루어야 한다. 다른 기술이 시스템 요구사항을 더욱 경제적으로 만족할 수 있다면 과도한 기능은 불필요한 복잡성이 될 수 있다.

따라서 새로운 아키텍처에서 플렉스레이의 역할이 감소하더라도 교육적 참조 모델(Educational Reference Model)로서의 가치는 여전히 크다. 정적 및 동적 통신 세그먼트는 결정론적 자원 할당과 유연한 자원 할당의 차이를 보여주며, 동기화 메커니즘은 분산 전역 시간(Distributed Global Time)을 설명한다. 이중 채널은 통신 이중화를 보여주고 다양한 토폴로지 옵션은 결함 격리의 개념을 보여준다. 이러한 원리는 하나의 자동차 프로토콜을 넘어 실시간 사이버 물리 시스템(Real-Time Cyber-Physical System) 전반에 적용된다.

보다 광범위한 자동차 통신 구조에서 플렉스레이는 중요한 기술적 성취인 동시에 과도기적 아키텍처(Transitional Architecture)를 의미한다. CAN FD는 기존 임베디드 제어 네트워킹의 점진적인 확장을 보여주며, 자동차 이더넷과 TSN은 확장 가능한 고대역폭 결정론적 인프라(Scalable High-Bandwidth Deterministic Infrastructure)를 향한 변화를 나타낸다. 이러한 기술 사이에서 플렉스레이를 학습하면 차량 통신이 수많은 특수 목적 버스에서 점점 더 통합된 네트워크 아키텍처로 발전한 이유를 이해할 수 있다.

플렉스레이가 남긴 핵심적인 교훈은 실패했다는 것이 아니라 플렉스레이가 해결했던 요구사항이 새로운 아키텍처로 이동했다는 것이다. 결정론적 타이밍, 동기화, 이중화, 협조된 실행은 통신이 물리적 제어에 직접 참여하는 모든 시스템에서 여전히 필수적이다. 따라서 플렉스레이의 유산은 미래의 지배적인 네트워크 자체라기보다 차량, 로보틱스, 피지컬 AI 시스템에서 센싱(Sensing), 연산(Computation), 제어(Control), 액추에이션 사이의 신뢰성 높은 통신을 설계하기 위한 명확한 엔지니어링 모델로 계속 남아 있다.
