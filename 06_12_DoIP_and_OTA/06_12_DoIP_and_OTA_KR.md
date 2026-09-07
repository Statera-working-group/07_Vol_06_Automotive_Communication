**Volume 06 Automotive Communication**

# Chapter 12. DoIP and OTA

## 12.1 ISO 13400 DoIP Architecture

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

ISO 13400 DoIP(Diagnostics over Internet Protocol) 아키텍처는 현대 자동차 진단 통신 분야에서 가장 중요한 발전 중 하나이다. 차량 전자 아키텍처(Electronic Architecture)가 기존의 분산형 ECU(Electronic Control Unit) 중심 구조에서 도메인 기반 아키텍처(Domain Architecture) 및 존 아키텍처(Zonal Architecture)로 발전하고, 고속 Automotive Ethernet 네트워크가 도입되면서 기존의 CAN 기반 진단 통신은 점차 한계를 드러내기 시작하였다. 소프트웨어 정의 차량(SDV, Software Defined Vehicle), 자율주행 플랫폼(Autonomous Driving Platform), 전기차(EV, Electric Vehicle), 산업용 자율이동로봇(AMR, Autonomous Mobile Robot), 지능형 운송 시스템(Intelligent Transportation System), 그리고 피지컬 AI(Physical AI) 플랫폼은 훨씬 더 빠르고 확장 가능하며 유연한 진단 통신 기술을 요구하고 있다. ISO 13400 DoIP는 이러한 요구를 충족하기 위해 UDS(Unified Diagnostic Services)를 인터넷 프로토콜(IP, Internet Protocol)과 Ethernet 기반 네트워크 위에서 동작하도록 정의한 표준이다. 이는 기존 차량 진단 방식에서 네트워크 중심 진단(Network-Oriented Diagnostics)으로 전환되는 중요한 기술적 변화라고 할 수 있다.

과거의 자동차 진단 시스템은 K-Line과 CAN 기반 통신 기술에 의존하였다. 이러한 기술은 ECU 수가 적고 소프트웨어 규모가 작았던 시절에는 충분히 효과적이었다. 그러나 현대 차량은 100개 이상의 ECU, 고성능 도메인 컨트롤러(Domain Controller), 중앙 컴퓨팅 플랫폼(Central Computing Platform), 첨단 운전자 보조 시스템(ADAS, Advanced Driver Assistance System), 배터리 관리 시스템(BMS, Battery Management System), 인포테인먼트 시스템(Infotainment System), 텔레매틱스 모듈(Telematics Module), 자율주행 컴퓨터 등을 포함한다. 차량 내 소프트웨어 규모 역시 수 MB에서 수십\~수백 GB 수준으로 증가하였다. 이러한 환경에서는 CAN 기반 진단 통신이 병목(Bottleneck)이 되기 시작하였다. DoIP는 Ethernet과 IP 기술을 활용하여 이러한 한계를 극복하고자 개발되었다.

DoIP의 기본 목적은 진단 데이터를 Ethernet 및 IP 네트워크를 통해 전달하는 것이다. 기존의 전용 진단 버스 대신, DoIP는 서비스 진단기(Service Tester), 정비 장비(Service Tool), 엔지니어링 워크스테이션(Engineering Workstation), 제조 설비(Manufacturing System), 원격 진단 플랫폼(Remote Diagnostic Platform)이 표준 네트워크 기술을 이용하여 ECU와 통신할 수 있도록 지원한다. 이를 통해 진단 통신은 클라우드 서비스, OTA(Over-The-Air) 업데이트, 플릿 관리(Fleet Management), 중앙 컴퓨팅 시스템과 자연스럽게 통합될 수 있다.

DoIP 시스템은 크게 외부 진단기(Diagnostic Tester), 차량 Ethernet 네트워크, DoIP 게이트웨이(DoIP Gateway), ECU 내부의 진단 서버(Diagnostic Server), 라우팅 시스템(Routing Mechanism), 전송 프로토콜(Transport Protocol), 네트워크 검색 서비스(Network Discovery Service), 진단 애플리케이션(Diagnostic Application)으로 구성된다. 이러한 구성 요소들은 외부 진단 장비와 차량 내부 ECU 간의 안정적인 통신 경로를 형성한다.

외부 진단기는 DoIP 아키텍처의 클라이언트(Client) 역할을 수행한다. 이는 정비소의 서비스 장비, 생산 공장의 검사 시스템, 개발용 엔지니어링 도구, 자동화 시험 장비, 플릿 관리 서버, 클라우드 기반 진단 시스템 등이 될 수 있다. 진단기는 네트워크 연결을 설정한 후 차량 내에서 제공되는 진단 서비스를 탐색하고 ECU와 통신을 시작한다. DoIP의 가장 큰 특징 중 하나는 기존 전용 인터페이스 대신 일반 Ethernet 기술을 활용할 수 있다는 점이다.

차량 내부에서는 DoIP 게이트웨이가 핵심 역할을 수행한다. 게이트웨이는 Ethernet 기반 진단 네트워크와 차량 내부의 다양한 통신 네트워크 사이를 연결한다. 현대 차량에는 CAN, CAN FD, CAN XL, LIN, FlexRay, Automotive Ethernet 등 여러 통신 기술이 공존한다. DoIP 게이트웨이는 외부 진단기의 요청을 적절한 ECU로 전달하고, ECU의 응답을 다시 진단기로 전달한다. 이를 통해 진단 도구는 내부 네트워크 구조를 몰라도 ECU에 접근할 수 있다.

DoIP 아키텍처에서 중요한 기능 중 하나는 차량 검색(Vehicle Discovery)이다. 진단을 수행하기 전에 진단기는 네트워크 상에서 DoIP를 지원하는 차량을 탐색해야 한다. ISO 13400은 이를 위한 검색 메커니즘을 정의한다. 검색 과정에서 차량 식별 정보(Vehicle Identification Information), 네트워크 주소(Network Address), 지원 서비스(Supported Service), 프로토콜 버전(Protocol Version), 통신 기능(Communication Capability) 등이 제공된다. 이는 IT 네트워크의 장치 검색(Device Discovery) 방식과 유사하다.

차량 검색 이후에는 연결 설정(Connection Establishment)이 이루어진다. 진단기는 DoIP 게이트웨이와 TCP(Transmission Control Protocol)를 사용하여 안정적인 연결을 수립한다. TCP는 데이터의 순서 보장(Order Guarantee)과 신뢰성(Reliability)을 제공하므로 진단 통신에 적합하다. UDP(User Datagram Protocol)는 일부 검색 및 알림 기능에 사용될 수 있다. 이러한 표준 네트워크 프로토콜의 활용은 기존 IT 기술과의 통합을 용이하게 한다.

연결이 수립된 후에는 라우팅 활성화(Routing Activation)가 수행된다. 이는 게이트웨이가 어떤 ECU로 진단 요청을 전달할 수 있는지 결정하는 과정이다. 라우팅 활성화는 단순한 경로 설정 기능뿐 아니라 보안(Security) 기능도 수행한다. 게이트웨이는 요청이 적절한 권한을 가진 사용자로부터 발생했는지 확인하고, 승인된 경우에만 통신 경로를 활성화한다.

DoIP의 핵심 진단 계층은 여전히 ISO 14229에서 정의한 UDS를 사용한다. DoIP는 UDS를 대체하는 것이 아니라 UDS 메시지를 Ethernet 기반 네트워크로 전송하는 역할을 수행한다. 따라서 Read Data By Identifier, Write Data By Identifier, Diagnostic Session Control, Security Access, Routine Control, ECU Reset, Download Request, Upload Request, DTC(Diagnostic Trouble Code) 관리와 같은 서비스는 그대로 유지된다. 차이점은 CAN 프레임 대신 Ethernet 패킷으로 전달된다는 점이다.

DoIP의 가장 큰 장점은 대역폭(Bandwidth)의 획기적인 향상이다. 일반 CAN 네트워크는 500 kbps에서 1 Mbps 정도의 속도를 제공하며, CAN FD도 수 Mbps 수준이다. 반면 Automotive Ethernet은 100 Mbps, 1 Gbps 이상의 속도를 지원한다. 이러한 대역폭 증가는 진단 데이터 전송 시간을 크게 줄인다. 과거에는 ECU 소프트웨어 다운로드에 수 시간이 걸렸다면, DoIP 환경에서는 수 분 내에 완료할 수 있다.

DoIP는 OTA 업데이트 시스템의 핵심 기반 기술이기도 하다. SDV 환경에서는 새로운 기능 추가, 보안 패치(Security Patch), 버그 수정(Bug Fix), 성능 개선을 위해 소프트웨어를 원격으로 배포해야 한다. 대용량 소프트웨어 패키지를 효율적으로 전송하기 위해서는 고속 네트워크가 필요하며, DoIP는 이를 위한 이상적인 기반을 제공한다.

사이버보안(Cybersecurity)은 DoIP 환경에서 매우 중요한 요소이다. Ethernet 네트워크는 기존 CAN 네트워크보다 훨씬 높은 연결성을 제공하기 때문에 강력한 보안 메커니즘이 필요하다. 현대 DoIP 시스템은 인증(Authentication), 암호화(Encryption), 인증서 관리(Certificate Management), 방화벽(Firewall), 침입 탐지 시스템(IDS, Intrusion Detection System), 접근 제어(Access Control)를 포함하는 경우가 많다.

특히 UDS의 Security Access 서비스는 DoIP 환경에서 더욱 중요해진다. 소프트웨어 업데이트, 파라미터 변경, 캘리브레이션(Calibration), 액추에이터 제어(Actuator Control), 안전 관련 설정 변경은 반드시 인증과 권한 검증을 거쳐야 한다. 이를 통해 비인가 접근(Unauthorized Access)을 방지할 수 있다.

차량 아키텍처가 중앙집중형 구조로 발전하면서 DoIP의 중요성은 더욱 증가하고 있다. 과거에는 각 ECU가 독립적으로 기능을 수행했지만, 현재는 중앙 컴퓨팅 플랫폼이 다수의 기능을 통합하고 있다. Ethernet은 이러한 시스템을 연결하는 주요 백본(Backbone) 역할을 수행하며, DoIP는 자연스럽게 이 구조와 통합된다.

제조 환경에서도 DoIP는 큰 이점을 제공한다. 생산 라인에서는 ECU 설정(Configuration), 소프트웨어 다운로드, 기능 검증(Function Validation), 품질 검사(Quality Inspection)가 반복적으로 수행된다. Ethernet 기반 진단은 검사 시간을 단축하고 생산 효율성을 향상시킨다.

개발 환경에서도 DoIP는 중요한 역할을 한다. 캘리브레이션 엔지니어(Calibration Engineer), 소프트웨어 개발자(Software Developer), 통합 엔지니어(System Integration Engineer), 검증 엔지니어(Validation Engineer)는 대량의 데이터를 빠르게 수집하고 분석해야 한다. DoIP는 데이터 수집, 디버깅(Debugging), 소프트웨어 배포를 더욱 효율적으로 수행할 수 있게 한다.

DoIP의 활용 범위는 승용차를 넘어 산업용 로봇, AMR, 농업 기계(Agricultural Machinery), 광산 장비(Mining Equipment), 건설 장비(Construction Equipment), 철도 시스템(Railway System), 방위 산업 플랫폼(Defense Platform), 피지컬 AI 시스템으로 확대되고 있다. 이들 역시 Ethernet 기반 네트워크를 사용하며, 진단과 유지보수의 중요성이 증가하고 있기 때문이다.

클라우드 기반 진단(Cloud-Based Diagnostics)은 또 다른 중요한 응용 분야이다. 현대 플릿(Fleet)은 원격 상태 모니터링(Remote Health Monitoring), 예지 정비(Predictive Maintenance), 고장 분석(Fault Diagnosis), 소프트웨어 관리(Software Management)를 필요로 한다. DoIP는 차량과 클라우드 플랫폼 간의 표준화된 진단 통신 경로를 제공한다.

TSN(Time Sensitive Networking)과 같은 최신 Ethernet 기술도 DoIP와의 통합 가능성을 높이고 있다. TSN은 결정적인 통신(Deterministic Communication)을 제공하며, 향후 진단 시스템과 실시간 네트워크 관리 기능을 더욱 긴밀하게 통합할 수 있게 한다.

DoIP 시스템을 구현하기 위해서는 철저한 검증 및 검증(Verification and Validation)이 필요하다. 차량 검색, 연결 설정, 라우팅 활성화, UDS 전송, 보안 기능, 장애 복구(Fault Recovery), 공급업체 간 상호운용성(Interoperability) 등을 확인해야 한다.

향후 차량과 로봇이 SDV, 중앙집중형 컴퓨팅, 자율주행, 클라우드 연결, 피지컬 AI 플랫폼으로 발전함에 따라 DoIP는 더욱 중요한 기술이 될 것이다. 미래에는 사이버보안 프레임워크(Cybersecurity Framework), OTA 플랫폼, 디지털 트윈(Digital Twin), 예지 정비 시스템, AI 기반 진단 시스템과 더욱 긴밀하게 통합될 것으로 예상된다.

결론적으로 ISO 13400 DoIP 아키텍처는 Ethernet 시대를 위한 차세대 진단 통신 기술이다. UDS의 강력한 진단 기능과 Ethernet/IP 네트워크의 높은 대역폭, 확장성, 유연성을 결합함으로써 복잡한 전자 시스템에서도 효율적인 진단 환경을 제공한다. DoIP는 SDV, 자율주행 플랫폼, 지능형 로봇, 산업 자동화 시스템, 그리고 미래 피지컬 AI 인프라를 지원하는 핵심 진단 기술로 자리잡고 있으며, 차세대 연결형 시스템(Connected System)의 기반 기술로서 그 중요성이 계속 증가할 것이다.

## 12.2 DoIP Routing Activation

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

DoIP 라우팅 활성화(DoIP Routing Activation)는 ISO 13400 Diagnostics over Internet Protocol 아키텍처에서 정의된 가장 중요한 기능 중 하나이다. Ethernet 기반 통신은 고속 진단 통신을 위한 물리 계층(Physical Layer)과 네트워크 인프라(Network Infrastructure)를 제공하지만, 실제로 진단 통신을 허용할 것인지, 어떤 경로를 통해 내부 ECU와 통신할 것인지를 결정하는 것은 Routing Activation의 역할이다. 즉, Routing Activation은 외부 진단기(Diagnostic Tester)와 차량 내부 ECU(Electronic Control Unit) 간의 안전하고 통제된 진단 세션(Diagnostic Session)을 설정하는 게이트웨이 인증 및 승인 절차라고 볼 수 있다. Routing Activation이 수행되지 않으면 DoIP 통신은 차량 검색(Vehicle Discovery)과 TCP 연결(Connection Establishment) 이후 단계로 진행될 수 없다.

차량 아키텍처가 기존의 독립적인 ECU 중심 구조에서 소프트웨어 정의 차량(SDV, Software Defined Vehicle), 자율주행 플랫폼(Autonomous Driving Platform), 중앙집중형 컴퓨팅(Centralized Computing), 클라우드 기반 모빌리티 시스템(Cloud-Connected Mobility System)으로 발전하면서 Routing Activation의 중요성은 더욱 커지고 있다. 현대 차량은 파워트레인(Powertrain), 섀시(Chassis), 바디 전장(Body Electronics), 배터리 관리 시스템(BMS, Battery Management System), 인포테인먼트 시스템(Infotainment System), 텔레매틱스(Telematics), 첨단 운전자 보조 시스템(ADAS), 자율주행 컴퓨터(Autonomous Driving Controller), 사이버보안 모듈(Cybersecurity Module) 등 수십 개에서 수백 개의 ECU를 포함하고 있다. 이러한 시스템에 대한 무제한 접근은 기능안전(Function Safety)과 사이버보안(Cybersecurity) 측면에서 매우 큰 위험을 초래할 수 있다. 따라서 Routing Activation은 승인된 경로를 통해서만 진단 통신이 이루어지도록 보장하는 핵심 메커니즘이다.

개념적으로 Routing Activation은 IT 네트워크의 인증(Authentication) 및 세션 생성(Session Establishment) 절차와 유사하다. 외부 진단 장비가 내부 ECU와 통신하기 전에 DoIP 게이트웨이는 진단 장비의 신원, 권한, 통신 목적, 그리고 접근 가능한 네트워크 경로를 검증해야 한다. 이러한 검증이 완료된 이후에만 실제 진단 메시지가 차량 내부로 전달될 수 있다.

Routing Activation 절차는 일반적으로 두 가지 사전 단계 이후에 수행된다. 첫 번째 단계는 차량 검색(Vehicle Discovery)이다. 진단기는 네트워크 상에서 DoIP 기능을 지원하는 차량을 탐색하고 식별한다. 두 번째 단계는 TCP 연결 수립(TCP Connection Establishment)이다. 진단기와 DoIP 게이트웨이 간에 안정적인 통신 채널이 형성되면, 진단기는 Routing Activation Request 메시지를 전송하여 라우팅 활성화를 요청한다.

Routing Activation Request에는 게이트웨이가 통신 요청을 평가하기 위해 필요한 다양한 정보가 포함된다. 여기에는 진단기의 논리 주소(Logical Address), 활성화 유형(Activation Type), 진단 클라이언트 식별 정보(Client Identification), 요청된 라우팅 기능(Routing Capability), 프로토콜 버전 정보(Protocol Version Information), 인증 관련 정보(Authentication Parameter) 등이 포함될 수 있다. 게이트웨이는 이 정보를 바탕으로 내부 보안 정책(Security Policy)과 라우팅 규칙(Routing Rule)을 적용하여 요청을 평가한다. 모든 조건이 만족되면 Routing Activation Response를 반환하여 세션을 활성화한다. 반대로 권한 부족, 설정 오류, 시스템 상태 문제 등이 존재할 경우 오류 응답(Error Response)을 반환한다.

Routing Activation의 가장 중요한 기능 중 하나는 논리 주소(Logical Address) 관리이다. DoIP 환경에서는 모든 진단 대상 ECU와 진단 장비가 고유한 논리 주소를 가진다. 이 논리 주소는 실제 물리 네트워크 구조를 추상화하는 역할을 한다. 게이트웨이는 논리 주소와 실제 ECU 간의 매핑(Mapping)을 유지하며, CAN, CAN FD, CAN XL, LIN, FlexRay, Automotive Ethernet 등의 내부 네트워크와 연결된 ECU로 진단 요청을 전달한다. Routing Activation은 진단기와 대상 ECU 간의 논리적 통신 경로를 설정하는 과정이라고 볼 수 있다.

활성화 유형(Activation Type)은 Routing Activation 과정에서 중요한 의미를 가진다. 제조 공정(Manufacturing Operation), 개발 환경(Engineering Development), 서비스 정비(Service Diagnostics), 원격 진단(Remote Diagnostics), 소프트웨어 프로그래밍(Software Programming), 유지보수(Maintenance) 등 각기 다른 상황에 따라 허용되는 기능과 접근 범위가 달라질 수 있다. 예를 들어 생산 공장에서는 ECU 프로그래밍이 허용될 수 있지만, 일반 서비스 센터에서는 제한될 수 있다. Routing Activation은 이러한 차이를 반영하여 상황별 접근 제어를 수행한다.

사이버보안은 Routing Activation 설계의 핵심 요소이다. 현대 차량은 셀룰러 통신(Cellular Communication), Wi-Fi, Bluetooth, V2X(Vehicle-to-Everything), 클라우드 서비스 등을 통해 외부 네트워크와 연결된다. 이러한 연결성은 공격 표면(Attack Surface)을 증가시키며, 적절한 접근 제어가 없다면 내부 ECU가 외부 공격에 노출될 수 있다. Routing Activation은 진단 통신이 시작되기 전에 접근 권한을 확인함으로써 보안 게이트(Security Gateway)의 역할을 수행한다.

인증(Authentication) 기능은 Routing Activation과 밀접하게 연관된다. 시스템에 따라 게이트웨이는 디지털 인증서(Digital Certificate), 암호화 키(Cryptographic Key), 디지털 서명(Digital Signature), 챌린지-응답 프로토콜(Challenge-Response Protocol), 하드웨어 보안 모듈(HSM, Hardware Security Module) 등을 이용하여 진단 장비의 신원을 검증할 수 있다. 이를 통해 승인되지 않은 장비의 접근을 차단할 수 있다.

인증 이후에는 권한 부여(Authorization)가 수행된다. 인증된 사용자라고 해서 모든 기능을 사용할 수 있는 것은 아니다. 예를 들어 정비용 진단기는 차량 상태 조회는 가능하지만 보안 설정 변경은 제한될 수 있다. 제조 설비는 ECU 프로그래밍이 가능하지만 서비스 모드에서는 허용되지 않을 수 있다. 개발용 장비는 실험적인 기능에 접근할 수 있지만 일반 서비스 장비는 접근할 수 없다. Routing Activation은 이러한 권한 차이를 정책 기반 접근 제어(Policy-Based Access Control)를 통해 관리한다.

라우팅 테이블(Routing Table)은 Routing Activation의 핵심 요소이다. 게이트웨이는 라우팅 테이블을 참조하여 어떤 ECU에 접근이 허용되는지 결정한다. 요청된 활성화 유형과 진단기의 권한 수준에 따라 허용된 경로만 활성화된다. 이를 통해 불필요한 네트워크 노출을 최소화하면서도 필요한 진단 기능을 제공할 수 있다.

현대의 존 아키텍처(Zonal Architecture)에서는 Routing Activation의 중요성이 더욱 증가한다. 기존 차량은 비교적 독립적인 도메인(Domain) 구조를 사용했지만, 존 아키텍처에서는 네트워크 통합이 더욱 강화된다. 이러한 환경에서 잘못된 라우팅 설정은 차량 전체에 대한 과도한 접근 권한을 제공할 수 있다. Routing Activation은 특정 존(Zone) 또는 특정 기능 영역(Function Domain)에 대해서만 접근을 허용함으로써 이러한 위험을 줄인다.

Routing Activation은 UDS(Unified Diagnostic Services)와도 밀접하게 연관되어 있다. Diagnostic Session Control, Security Access, ECU Reset, Routine Control, Read Data By Identifier, Write Data By Identifier, Request Download, Request Upload 등의 UDS 서비스는 차량 동작에 직접적인 영향을 줄 수 있다. 따라서 Routing Activation은 이러한 기능이 실행되기 전에 적절한 접근 권한이 검증되도록 보장한다.

소프트웨어 프로그래밍(Software Programming)은 Routing Activation이 특히 중요한 분야이다. 현대 ECU는 서비스 센터, 제조 공정, OTA 시스템을 통해 지속적으로 소프트웨어 업데이트를 수행한다. 펌웨어(Firmware) 다운로드와 플래싱(Flashing)은 매우 민감한 작업이므로 게이트웨이는 해당 세션이 승인된 경우에만 프로그래밍 경로를 활성화한다.

원격 진단(Remote Diagnostics)은 Routing Activation의 중요성을 더욱 높인다. 플릿 관리(Fleet Management), 예지 정비(Predictive Maintenance), 원격 기술 지원(Remote Technical Support) 시스템은 네트워크 기반 진단 통신을 필요로 한다. 물리적으로 차량에 연결된 진단기보다 보안 위험이 더 크기 때문에, Routing Activation은 원격 접근 경로를 엄격하게 통제해야 한다.

세션 관리(Session Management)는 Routing Activation 이후에도 지속적으로 수행된다. 게이트웨이는 진단기 정보, 권한 수준, 활성화 유형, 통신 통계, 보안 상태, 타임아웃 설정 등을 관리한다. 이를 통해 활성 세션을 모니터링하고 이상 행위를 탐지할 수 있다.

타임아웃 관리(Timeout Management)는 보안성과 자원 효율성을 동시에 향상시킨다. 진단 세션이 종료된 이후에도 연결이 무한정 유지되면 보안 위험이 증가한다. 따라서 게이트웨이는 일정 시간 동안 활동이 없을 경우 세션을 자동 종료하여 통신 경로를 비활성화한다.

오류 처리(Error Handling) 역시 Routing Activation의 중요한 부분이다. 잘못된 논리 주소, 지원되지 않는 활성화 유형, 인증 실패, 자원 부족, 네트워크 오류, 프로토콜 버전 불일치, 게이트웨이 내부 장애 등이 발생할 수 있다. 표준화된 오류 코드(Error Code)는 진단 장비가 문제 원인을 파악하고 적절한 대응을 수행할 수 있도록 지원한다.

Routing Activation은 UNECE R155와 ISO/SAE 21434와 같은 현대 자동차 사이버보안 규격과도 밀접한 관계를 가진다. 이러한 규격은 차량 내부 시스템에 대한 접근을 엄격하게 관리할 것을 요구한다. Routing Activation은 이러한 요구사항을 충족시키기 위한 핵심 접근 제어 메커니즘으로 활용된다.

최신 보안 게이트웨이(Security Gateway)는 Routing Activation 기능을 더욱 확장하고 있다. 인증서 검증(Certificate Validation), 보안 채널(Secure Channel), 침입 탐지 시스템(IDS), 행동 분석(Behavior Monitoring), 접근 로그(Access Logging), 권한 관리(Privilege Management), 보안 이벤트 보고(Security Event Reporting) 등이 통합되고 있다. 이로 인해 Routing Activation은 단순한 연결 설정 절차를 넘어 종합적인 보안 프레임워크(Security Framework)로 발전하고 있다.

중앙집중형 컴퓨팅(Centralized Computing) 아키텍처가 확대되면서 Routing Activation의 중요성은 더욱 증가할 것이다. 하나의 고성능 컴퓨터(HPC, High Performance Computing)가 수많은 기능을 제어하는 환경에서는 진단 접근이 차량 전체에 영향을 미칠 수 있기 때문이다. Routing Activation은 이러한 위험을 최소화하기 위해 엄격한 접근 제어를 제공한다.

제조 환경에서도 Routing Activation은 중요한 역할을 수행한다. ECU 설정, 소프트웨어 다운로드, 품질 검사(Quality Inspection), 생산 검증(Production Validation) 과정에서 필요한 진단 경로만 활성화할 수 있기 때문에 생산 효율성과 보안성을 동시에 향상시킬 수 있다.

개발 환경에서는 실험용 ECU, 프로토타입 소프트웨어, 디버깅 인터페이스(Debugging Interface), 내부 파라미터(Parameter)에 접근해야 하는 경우가 많다. Routing Activation은 이러한 특수 기능을 개발 환경에서만 사용할 수 있도록 제한할 수 있다.

미래의 DoIP 아키텍처에서는 Routing Activation이 더욱 지능화될 것으로 예상된다. 인공지능 기반 접근 관리(AI-Assisted Access Management), 적응형 보안 정책(Adaptive Security Policy), 동적 신뢰 평가(Dynamic Trust Evaluation), 클라우드 연계 인증 서비스(Cloud-Based Authorization Service), 제로 트러스트(Zero Trust) 보안 모델 등이 적용될 가능성이 높다.

결론적으로 DoIP Routing Activation은 연결성(Connectivity)과 보안(Security)을 동시에 만족시키는 핵심 기술이다. 이는 진단 통신 경로를 제어하고, 보안 정책을 적용하며, 세션을 관리하고, 승인된 장비에만 ECU 접근을 허용한다. 소프트웨어 정의 차량, 자율주행 시스템, 플릿 인텔리전스(Fleet Intelligence), 피지컬 AI 플랫폼으로 발전하는 미래 환경에서도 Routing Activation은 ISO 13400 기반 진단 시스템의 핵심 구성 요소로서 중요한 역할을 수행하게 될 것이다.

## 12.3 OTA Update Architecture

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

OTA(Over-The-Air) 업데이트 아키텍처는 현대 커넥티드 시스템(Connected System)에서 가장 혁신적인 기술 중 하나로 평가된다. OTA 기술은 소프트웨어(Software), 펌웨어(Firmware), 설정 데이터(Configuration Data), 보안 인증서(Security Certificate), 인공지능 모델(AI Model), 디지털 서비스(Digital Service)를 물리적인 접근 없이 원격으로 배포할 수 있도록 지원한다. 초기에는 소비자 전자기기(Consumer Electronics)의 유지보수성을 향상시키기 위해 개발되었지만, 현재는 소프트웨어 정의 차량(SDV, Software Defined Vehicle), 자율주행 시스템(Autonomous Driving System), 전기차(EV, Electric Vehicle), 산업용 로봇(Industrial Robot), 자율이동로봇(AMR, Autonomous Mobile Robot), 철도 시스템(Railway System), 항공우주 플랫폼(Aerospace Platform), 스마트 팩토리(Smart Factory), 엣지 컴퓨팅 인프라(Edge Computing Infrastructure), 그리고 미래의 피지컬 AI(Physical AI) 생태계에서 핵심 기술로 자리잡고 있다. 소프트웨어가 시스템의 기능(Functionality), 성능(Performance), 안전성(Safety), 그리고 제품 가치를 결정하는 시대가 되면서 OTA는 시스템이 운영 수명 주기(Operation Lifecycle) 동안 지속적으로 진화할 수 있도록 하는 기반 기술이 되었다.

과거에는 소프트웨어 업데이트를 수행하기 위해 반드시 서비스 엔지니어(Service Engineer)가 현장에 방문하여 장비를 연결해야 했다. 자동차 산업에서는 차량을 서비스 센터(Service Center)로 입고한 후 진단 장비(Diagnostic Tool)를 이용하여 ECU를 업데이트하였다. 산업 자동화 시스템과 로봇 시스템 역시 유사한 방식으로 유지보수를 수행하였다. 이러한 방식은 효과적이었지만 시간과 비용이 많이 들고 확장성이 부족했다. 또한 긴급 보안 패치(Security Patch)나 기능 개선(Function Enhancement)을 신속하게 배포하기 어려웠다. OTA 아키텍처는 이러한 문제를 해결하기 위해 등장하였으며, 네트워크를 통해 원격으로 업데이트를 수행할 수 있도록 함으로써 유지보수 효율성을 획기적으로 향상시켰다.

OTA 아키텍처는 크게 소프트웨어 개발 환경(Software Development Environment), 빌드 및 통합 시스템(Build and Integration System), 아티팩트 저장소(Artifact Repository), OTA 관리 서버(OTA Management Server), 클라우드 인프라(Cloud Infrastructure), 통신 게이트웨이(Communication Gateway), 보안 관리 시스템(Security Management System), 플릿 관리 플랫폼(Fleet Management Platform), 엣지 디바이스(Edge Device), 차량 또는 로봇 내부의 컨트롤러(Controller), 그리고 업데이트 에이전트(Update Agent)로 구성된다. 이러한 구성 요소는 소프트웨어 개발부터 최종 배포까지 전체 생명주기를 관리하는 하나의 소프트웨어 배포 생태계(Software Distribution Ecosystem)를 형성한다.

OTA 프로세스는 소프트웨어 개발 단계에서 시작된다. 개발자는 새로운 기능(New Feature), 버그 수정(Bug Fix), 성능 향상(Performance Improvement), 사이버보안 패치(Cybersecurity Patch), AI 모델 업데이트(AI Model Update), 설정 변경(Configuration Change)을 개발한다. 이후 CI/CD(Continuous Integration / Continuous Delivery) 파이프라인을 통해 자동 빌드(Build), 검증(Validation), 테스트(Test), 패키징(Packaging), 디지털 서명(Digital Signing)이 수행된다. 이렇게 생성된 소프트웨어 패키지가 OTA 배포 대상이 된다.

생성된 소프트웨어는 아티팩트 저장소에 보관된다. 저장소는 버전 관리(Version Control), 추적성(Traceability), 메타데이터(Metadata) 관리, 의존성(Dependency) 관리, 릴리즈 정보(Release Information), 무결성 검증(Integrity Verification) 정보를 유지한다. 현대 차량과 로봇은 수백 개의 소프트웨어 컴포넌트(Component)를 포함할 수 있기 때문에 정확한 버전 관리가 필수적이다.

OTA 관리 서버는 전체 OTA 시스템의 중앙 제어 역할을 수행한다. 서버는 소프트웨어 버전, 대상 장치 목록(Device Inventory), 배포 정책(Deployment Policy), 스케줄링 정보(Scheduling Information), 보안 인증 정보(Security Credential), 배포 상태(Deployment Status), 실패 보고서(Failure Report)를 관리한다. 제조사(Manufacturer), 플릿 운영자(Fleet Operator), 서비스 조직(Service Organization)은 OTA 관리 서버를 통해 업데이트 전략을 수립하고 배포를 관리한다.

대규모 OTA 환경에서는 클라우드 인프라가 필수적이다. 수천 대에서 수백만 대의 차량과 로봇이 전 세계에 분산되어 운영될 수 있기 때문이다. 클라우드는 대용량 저장소(Storage), 확장 가능한 컴퓨팅 자원(Scalable Computing Resource), 콘텐츠 전송 네트워크(CDN, Content Delivery Network), 분석 서비스(Analytics Service), 모니터링 시스템(Monitoring System)을 제공한다. 이를 통해 글로벌 규모의 OTA 배포가 가능해진다.

통신 인프라는 OTA 데이터 전송의 핵심 역할을 수행한다. 승용차는 셀룰러 네트워크(Cellular Network), Wi-Fi, 텔레매틱스(Telematics)를 이용할 수 있으며, 산업용 로봇은 Ethernet, Private 5G, 산업용 무선 네트워크(Industrial Wireless Network)를 사용할 수 있다. AMR은 일반적으로 Wi-Fi와 클라우드 연결을 이용한다. 미래의 피지컬 AI 시스템은 여러 통신 기술을 동시에 활용할 가능성이 높다. OTA 아키텍처는 이러한 다양한 네트워크 환경을 지원해야 한다.

차량이나 로봇 내부에서는 OTA 게이트웨이(OTA Gateway)가 업데이트 데이터의 진입점 역할을 수행한다. 게이트웨이는 외부 네트워크에서 전달된 소프트웨어 패키지를 수신하고, 이를 내부 네트워크를 통해 각 ECU나 제어기로 분배한다. 현대 시스템은 Automotive Ethernet, CAN, CAN FD, CAN XL, LIN, FlexRay, EtherCAT, DDS(Data Distribution Service) 등을 함께 사용하기 때문에 게이트웨이는 다양한 통신 프로토콜을 관리해야 한다.

OTA 클라이언트 또는 업데이트 에이전트(Update Agent)는 대상 장치 내부에 존재하는 소프트웨어이다. 이 구성 요소는 OTA 서버와 통신하며 업데이트 가능 여부를 확인하고, 소프트웨어를 다운로드하며, 무결성을 검증하고, 설치 과정을 관리하며, 상태 정보를 보고한다. OTA 에이전트의 신뢰성은 OTA 성공률에 직접적인 영향을 미친다.

소프트웨어 패키지 구성 방식도 중요하다. OTA 시스템은 전체 이미지 업데이트(Full Image Update), 델타 업데이트(Delta Update), 컨테이너 기반 업데이트(Container-Based Update), 애플리케이션 업데이트(Application Update), 설정 업데이트(Configuration Update), AI 모델 업데이트(AI Model Update)를 지원할 수 있다. 전체 이미지 업데이트는 전체 소프트웨어를 교체하는 방식이며, 델타 업데이트는 변경된 부분만 전송하여 대역폭 사용량을 크게 줄인다.

대역폭 최적화(Bandwidth Optimization)는 OTA 설계에서 중요한 요소이다. 현대 차량의 소프트웨어 크기는 수 GB를 초과할 수 있다. 이를 효율적으로 전송하기 위해 압축 알고리즘(Compression Algorithm), 델타 인코딩(Delta Encoding), 콘텐츠 캐싱(Content Caching), P2P(Peer-to-Peer) 배포, 적응형 전송 프로토콜(Adaptive Transfer Protocol)이 활용된다.

보안(Security)은 OTA 아키텍처에서 가장 중요한 요소라고 할 수 있다. OTA 시스템은 차량 제어, 자율주행, 기능안전, 모션 제어(Motion Control), 전력 관리(Power Management), 사이버보안 기능을 변경할 수 있는 권한을 가진다. 따라서 악의적인 공격자가 OTA 기능을 악용할 경우 심각한 결과를 초래할 수 있다. 이를 방지하기 위해 암호화 서명(Cryptographic Signing), 인증서 관리(Certificate Management), 인증(Authentication), 권한 부여(Authorization), 보안 통신(Secure Communication), 침입 탐지 시스템(IDS), HSM(Hardware Security Module), Secure Boot, 무결성 검증(Integrity Verification) 등이 적용된다.

디지털 서명(Digital Signature)은 OTA 보안의 핵심이다. 모든 소프트웨어 패키지는 제조사의 암호화 키(Cryptographic Key)로 서명된다. 장치는 설치 전에 서명을 검증하여 소프트웨어가 신뢰할 수 있는 출처에서 생성되었는지 확인한다. 이를 통해 악성 코드(Malware) 삽입과 변조(Tampering)를 방지할 수 있다.

Secure Boot는 OTA 이후에도 신뢰 체인(Chain of Trust)을 유지한다. 시스템은 부트로더(Bootloader), 운영체제(OS, Operating System), 미들웨어(Middleware), 애플리케이션(Application), 설정 파일(Configuration File)의 무결성을 확인한 후에만 실행을 허용한다.

업데이트 설치 전략으로는 A/B 파티션(A/B Partition) 방식이 널리 사용된다. 현재 실행 중인 파티션은 유지한 상태에서 비활성 파티션(Inactive Partition)에 새로운 소프트웨어를 설치한다. 설치와 검증이 완료되면 시스템은 재부팅 후 새로운 파티션으로 전환한다. 만약 문제가 발생하면 자동으로 이전 버전으로 복구(Rollback)할 수 있다.

롤백(Rollback) 기능은 매우 중요하다. 충분한 검증을 거쳤더라도 소프트웨어 오류가 발생할 수 있기 때문이다. OTA 시스템은 문제가 발생할 경우 이전 버전으로 복구하여 서비스 중단을 최소화한다.

OTA 배포 시에는 스케줄링(Scheduling)과 배포 정책(Deployment Policy)이 중요하다. 차량은 주행 중에 업데이트를 수행해서는 안 되며, 산업용 로봇은 생산 작업 중에 소프트웨어를 교체해서는 안 된다. 따라서 OTA 시스템은 차량 상태(Vehicle State), 배터리 수준(Battery Level), 네트워크 품질(Network Quality), 작업 상태(Operation State)를 고려하여 설치 시점을 결정한다.

플릿 관리(Fleet Management)와 OTA는 밀접하게 연관된다. 플릿 운영자는 수천 대의 차량이나 로봇에 대해 소프트웨어 버전, 보안 상태(Security Status), 배포 진행 상황(Deployment Progress)을 중앙에서 모니터링할 수 있다. 또한 점진적 배포(Staged Rollout)를 수행하여 문제 발생 시 영향을 최소화할 수 있다.

최근에는 AI 모델 업데이트가 OTA의 중요한 활용 분야가 되고 있다. 자율주행 차량, 지능형 로봇, 머신 비전(Machine Vision), 예지 정비 시스템은 지속적으로 학습된 모델을 업데이트해야 한다. OTA는 새로운 인지 모델(Perception Model), 계획 알고리즘(Planning Algorithm), 이상 탐지 모델(Anomaly Detection Model), 언어 모델(Language Model)을 원격으로 배포할 수 있게 한다.

규제 준수(Regulatory Compliance) 역시 중요한 요소이다. 자동차 산업에서는 UNECE R156(Software Update Management System)과 UNECE R155(Cybersecurity Management System)가 대표적인 OTA 관련 규정이다. 향후 로봇, 철도, 산업 자동화 분야에서도 유사한 규제가 확대될 것으로 예상된다.

OTA 시스템은 철저한 검증 및 검증(Verification and Validation)이 필요하다. 기능 시험(Function Test), 보안 평가(Security Assessment), 통신 신뢰성 시험(Communication Reliability Test), 복구 시험(Recovery Test), 롤백 시험(Rollback Test), 대규모 배포 시뮬레이션(Deployment Simulation) 등을 수행해야 한다.

미래의 OTA 아키텍처는 더욱 지능화될 것이다. 인공지능 기반 배포 계획(AI-Assisted Deployment Planning), 예측 스케줄링(Predictive Scheduling), 적응형 대역폭 최적화(Adaptive Bandwidth Optimization), 자율 롤백(Autonomous Rollback), 디지털 트윈(Digital Twin) 검증, 분산 신뢰 프레임워크(Distributed Trust Framework), 자기 복구(Self-Healing) 소프트웨어 생태계가 도입될 가능성이 높다.

결론적으로 OTA 업데이트 아키텍처는 현대 지능형 시스템(Intelligent System)의 소프트웨어 배포 핵심 기술이다. OTA는 운영 수명 주기 동안 지속적인 기능 개선을 가능하게 하며, 유지보수 비용을 절감하고, 혁신 속도를 높이며, 사이버보안 대응 능력을 강화하고, 규제 준수를 지원한다. 앞으로 소프트웨어가 차량, 로봇, 산업 자동화 시스템, 그리고 피지컬 AI 플랫폼의 핵심 경쟁력이 될수록 OTA 아키텍처의 중요성은 더욱 커질 것이며, 차세대 지능형 시스템의 필수 기반 기술로 자리잡게 될 것이다.

## 12.4 Cybersecurity for OTA

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

OTA(Over-The-Air) 시스템을 위한 사이버보안(Cybersecurity)은 현대 커넥티드 플랫폼(Connected Platform)에서 가장 중요한 엔지니어링 분야 중 하나가 되었다. 오늘날 소프트웨어(Software)는 차량(Vehicle), 로봇(Robot), 산업 자동화 시스템(Industrial Automation System), 철도 플랫폼(Railway Platform), 항공우주 시스템(Aerospace System), 스마트 인프라(Smart Infrastructure), 그리고 피지컬 AI(Physical AI) 생태계의 기능(Functionality), 안전성(Safety), 성능(Performance), 그리고 상업적 가치(Commercial Value)를 결정하는 핵심 요소가 되고 있다. OTA 기술은 소프트웨어 업데이트, 펌웨어 업그레이드(Firmware Upgrade), 보안 패치(Security Patch), 설정 변경(Configuration Change), 인공지능 모델(AI Model), 디지털 서비스(Digital Service)를 원격으로 배포할 수 있도록 해준다. 이러한 기능은 유지보수 효율성과 확장성을 크게 향상시키지만, 동시에 강력한 공격 표면(Attack Surface)을 형성하기 때문에 체계적인 사이버보안 체계가 필수적으로 요구된다.

OTA 보안의 가장 큰 과제는 OTA 시스템이 소프트웨어 실행 환경(Software Execution Environment)에 대한 매우 높은 권한을 가진다는 점이다. OTA 플랫폼은 운영체제(OS, Operating System), 부트로더(Bootloader), 애플리케이션(Application), 제어 알고리즘(Control Algorithm), 기능안전 소프트웨어(Safety Software), 통신 모듈(Communication Module), 센서 처리 파이프라인(Sensor Processing Pipeline), 인공지능 모델(AI Model)을 변경할 수 있다. 만약 공격자가 OTA 인프라를 장악하게 되면 단순한 데이터 유출을 넘어 악성 소프트웨어 설치(Malicious Software Installation), 시스템 장악(System Takeover), 기능안전 침해(Safety Violation), 서비스 중단(Service Disruption), 지적재산권 침해(Intellectual Property Theft), 개인정보 유출(Privacy Breach) 등 심각한 결과를 초래할 수 있다. 따라서 OTA 보안은 선택적 기능이 아니라 필수 아키텍처 요구사항으로 간주되어야 한다.

과거에는 소프트웨어 업데이트가 물리적인 서비스 절차를 통해 이루어졌다. 서비스 엔지니어가 차량이나 장비에 직접 연결하여 업데이트를 수행했기 때문에 공격자가 시스템에 접근하려면 물리적인 접근이 필요했다. 그러나 OTA 환경에서는 셀룰러 네트워크(Cellular Network), Wi-Fi, Ethernet, 클라우드 플랫폼(Cloud Platform), 위성 통신(Satellite Communication), Private 5G, 플릿 관리 시스템(Fleet Management System), 엣지 컴퓨팅(Edge Computing)을 통해 원격으로 업데이트가 수행된다. 이러한 연결성은 새로운 보안 위협을 발생시키며, 모든 통신 경로에 대한 보호가 필요하다.

OTA 보안의 출발점은 신뢰 구축(Trust Establishment)이다. OTA 생태계에 참여하는 모든 구성 요소는 서로의 신뢰성을 검증할 수 있어야 한다. 여기에는 소프트웨어 개발 환경(Software Development Environment), 빌드 서버(Build Server), 서명 시스템(Signing System), 아티팩트 저장소(Artifact Repository), OTA 관리 서버(OTA Management Server), 클라우드 서비스(Cloud Service), 게이트웨이(Gateway), 업데이트 에이전트(Update Agent), ECU(Electronic Control Unit), 최종 장치(End Device)가 포함된다. 이러한 신뢰 체계가 없다면 OTA 업데이트의 무결성과 신뢰성을 보장할 수 없다.

신뢰의 근원(Root of Trust)은 OTA 보안 아키텍처의 핵심이다. Root of Trust는 일반적으로 HSM(Hardware Security Module), TPM(Trusted Platform Module), 보안 엘리먼트(Secure Element), TEE(Trusted Execution Environment), 전용 암호화 프로세서(Cryptographic Processor)와 같은 하드웨어 기반 보안 장치를 의미한다. 이들은 암호화 키(Cryptographic Key)를 안전하게 보관하고 신뢰의 출발점을 제공한다. OTA 보안의 모든 검증 과정은 궁극적으로 Root of Trust에 의존한다.

디지털 서명(Digital Signature)은 OTA 보안의 가장 중요한 기술 중 하나이다. 소프트웨어가 배포되기 전에 제조사는 개인 키(Private Key)를 사용하여 소프트웨어 패키지에 서명을 생성한다. OTA를 통해 전달된 소프트웨어는 장치 내의 공개 키(Public Key)를 이용하여 검증된다. 이를 통해 소프트웨어가 신뢰할 수 있는 출처에서 생성되었으며 전송 중 변조되지 않았음을 확인할 수 있다. 디지털 서명이 없다면 공격자는 악성 소프트웨어를 OTA 경로에 삽입할 수 있다.

무결성 검증(Integrity Verification)은 디지털 서명 외에도 중요한 역할을 한다. OTA 시스템은 SHA-256, SHA-384, SHA-512와 같은 해시 알고리즘(Hash Algorithm)을 사용하여 데이터 무결성을 확인한다. 개발 단계에서 생성된 해시 값(Hash Value)과 설치 시 계산된 해시 값을 비교하여 데이터 변조 여부를 확인한다. 무결성 검증은 악의적인 공격뿐 아니라 전송 오류(Transmission Error)나 데이터 손상(Corruption)도 탐지할 수 있다.

인증(Authentication)은 OTA 환경에서 필수적인 보안 기능이다. 개발자, 빌드 서버, OTA 서버, 클라우드 서비스, 게이트웨이, 업데이트 에이전트, 진단 장비(Diagnostic Tool), 플릿 관리 시스템, 최종 장치는 모두 자신의 신원을 증명해야 한다. 이를 위해 패스워드(Password), 인증서(Certificate), 암호화 키, 상호 TLS(Mutual TLS), 챌린지-응답 프로토콜(Challenge-Response Protocol), 하드웨어 토큰(Hardware Token), 다중 인증(MFA, Multi-Factor Authentication)이 사용된다.

권한 부여(Authorization)는 인증 이후 수행되는 과정이다. 인증된 사용자라 하더라도 모든 작업을 수행할 수 있는 것은 아니다. 예를 들어 개발자는 소프트웨어를 업로드할 수 있지만 배포 승인을 할 수 없도록 제한할 수 있다. 플릿 운영자는 업데이트 스케줄링은 가능하지만 소프트웨어 자체를 수정할 수는 없도록 설정할 수 있다. 이러한 세분화된 접근 제어(Fine-Grained Access Control)는 내부자 위협(Insider Threat)과 계정 탈취(Account Compromise)에 대한 방어 수단이 된다.

안전한 통신 채널(Secure Communication Channel)은 데이터 전송 과정의 보안을 보장한다. OTA 데이터는 공용 네트워크(Public Network)를 통해 전달되는 경우가 많기 때문에 도청(Eavesdropping), 중간자 공격(MITM, Man-in-the-Middle Attack), 데이터 변조(Tampering)에 노출될 수 있다. 이를 방지하기 위해 TLS(Transport Layer Security), IPsec(Internet Protocol Security), VPN(Virtual Private Network), 암호화 터널(Encrypted Tunnel), 종단 간 암호화(End-to-End Encryption)가 사용된다.

인증서 관리(Certificate Management)는 OTA 운영에서 중요한 과제이다. 인증서는 장치, 서버, 애플리케이션, 통신 채널의 신원을 보장한다. 수백만 대의 차량과 로봇이 연결된 환경에서는 인증서 생성, 배포, 갱신(Renewal), 폐기(Revocation), 만료 관리(Expiration Management), 신뢰 체인 검증(Trust Chain Validation)이 필수적이다.

Secure Boot는 OTA 이후의 실행 환경을 보호한다. OTA를 통해 올바른 소프트웨어가 설치되었더라도, 시스템 시작 시 악성 코드가 실행될 수 있다면 보안은 무의미하다. Secure Boot는 하드웨어에서 시작하여 부트로더, 운영체제, 미들웨어(Middleware), 애플리케이션, 설정 파일에 이르기까지 모든 구성 요소를 검증한다. 이를 통해 신뢰된 소프트웨어만 실행될 수 있다.

안티 롤백(Anti-Rollback) 보호도 중요하다. 공격자는 이미 취약점이 수정된 최신 버전 대신 오래된 취약한 버전을 다시 설치하려고 시도할 수 있다. 안티 롤백 기능은 최소 허용 버전(Minimum Version Requirement)을 관리하고 이전 버전 설치를 차단함으로써 이러한 공격을 방지한다.

공급망 보안(Supply Chain Security)은 OTA 보안에서 점점 중요해지고 있다. 현대 소프트웨어는 오픈소스(Open Source), 서드파티 라이브러리(Third-Party Library), 미들웨어 공급업체(Middleware Vendor), 클라우드 서비스 제공업체(Cloud Service Provider)의 구성 요소를 포함한다. 따라서 SBOM(Software Bill of Materials), 의존성 추적(Dependency Tracking), 취약점 분석(Vulnerability Scanning), 공급업체 평가(Supplier Assessment), 출처 검증(Provenance Verification)이 필요하다.

위협 모델링(Threat Modeling)은 OTA 보안 설계의 핵심 과정이다. 보안 엔지니어는 공격자(Threat Actor), 공격 경로(Attack Vector), 취약점(Vulnerability), 보호 자산(Protected Asset), 신뢰 경계(Trust Boundary)를 분석한다. 일반적인 위협에는 소프트웨어 변조, 중간자 공격, 서버 침해(Server Compromise), 인증 정보 탈취(Credential Theft), 악성코드 주입(Malware Injection), 서비스 거부 공격(DoS), 인증서 위조(Certificate Forgery), 내부자 공격이 포함된다.

IDS(Intrusion Detection System)와 IPS(Intrusion Prevention System)는 OTA 보안 아키텍처에서 점점 더 중요한 역할을 수행한다. 이들은 네트워크 트래픽, 서버 활동, 장치 동작, 업데이트 프로세스를 모니터링하여 이상 행위(Anomalous Behavior)를 탐지한다. 시그니처 기반 탐지(Signature-Based Detection), 이상 탐지(Anomaly Detection), 머신러닝 기반 분석(Machine Learning Analysis), 위협 인텔리전스(Threat Intelligence)가 활용된다.

OTA 보안은 가용성(Availability)도 고려해야 한다. 지나치게 강력한 보안 정책은 긴급 보안 패치의 배포를 지연시킬 수 있다. 따라서 기밀성(Confidentiality), 무결성(Integrity), 가용성(Availability), 안전성(Safety), 운영 효율성(Operational Efficiency) 간의 균형이 필요하다. 이를 위해 이중화 서버(Redundant Server), CDN(Content Delivery Network), 백업 통신 채널(Backup Communication Channel), 재해 복구(Disaster Recovery) 체계가 구축된다.

기능안전 시스템(Safety-Critical System)은 OTA 보안에서 특별한 고려가 필요하다. 차량, 로봇, 철도, 항공우주, 의료기기(Medical Device), 산업 자동화 시스템은 실제 물리적 동작을 제어하기 때문에 보안 침해가 물리적 사고로 이어질 수 있다. 따라서 기능안전과 사이버보안은 함께 설계되어야 하며, 보안 기능은 안전 목표를 지원해야 한다.

AI 모델(AI Model)은 새로운 보안 과제를 제시한다. 자율주행 차량과 지능형 로봇은 OTA를 통해 인지 모델(Perception Model), 계획 알고리즘(Planning Algorithm), 언어 모델(Language Model), 이상 탐지 모델(Anomaly Detection Model)을 지속적으로 업데이트한다. AI 모델은 중요한 지적재산권(IP)이자 시스템 동작을 결정하는 핵심 요소이므로 모델 도난(Model Theft), 모델 변조(Model Tampering), 모델 중독(Model Poisoning), 적대적 공격(Adversarial Attack)으로부터 보호되어야 한다.

플릿 단위 배포(Fleet-Wide Deployment) 전략도 보안과 관련이 있다. 수백만 대의 장치를 동시에 업데이트할 경우 문제가 발생하면 전체 시스템에 영향을 미칠 수 있다. 따라서 카나리 배포(Canary Deployment), 점진적 배포(Staged Rollout), 파일럿 그룹(Pilot Group), 지역별 배포(Regional Rollout) 전략이 사용된다.

규제(Regulation) 역시 OTA 보안 아키텍처에 큰 영향을 미친다. 자동차 산업에서는 UNECE R155(Cybersecurity Management System)와 UNECE R156(Software Update Management System)이 대표적이다. 또한 ISO/SAE 21434는 차량 사이버보안 엔지니어링 프레임워크를 제공한다. 산업 자동화에서는 IEC 62443이 널리 활용된다.

사고 대응(Incident Response)은 OTA 보안의 필수 요소이다. 완벽한 보안 시스템은 존재하지 않기 때문에 침해 사고 발생 시 탐지(Detection), 격리(Containment), 복구(Recovery), 인증서 폐기(Certificate Revocation), 긴급 패치 배포(Emergency Patch Deployment), 이해관계자 통지(Stakeholder Communication)를 수행할 수 있는 체계가 필요하다.

지속적인 모니터링(Continuous Monitoring)은 장기적인 보안 유지를 위해 필수적이다. 취약점, 공격 기법, 위협 환경은 지속적으로 변화하기 때문에 OTA 플랫폼은 보안 텔레메트리(Security Telemetry), 취약점 정보(Vulnerability Intelligence), 보안 분석(Security Analytics), 인증서 상태 모니터링(Certificate Health Monitoring), 위험 평가(Risk Assessment)를 지속적으로 수행해야 한다.

미래의 OTA 보안 아키텍처는 더욱 지능화될 것이다. 인공지능 기반 이상 탐지(AI-Based Anomaly Detection), 적응형 보안 정책(Adaptive Security Policy), 위협 헌팅(Threat Hunting), 위험 평가 자동화(Risk Assessment Automation), 양자내성암호(Post-Quantum Cryptography), 제로 트러스트(Zero Trust), 분산 신원 관리(Decentralized Identity), 블록체인 기반 무결성 검증(Blockchain-Based Integrity Verification) 기술이 점차 도입될 것으로 예상된다.

결론적으로 OTA를 위한 사이버보안은 단순한 보안 기술의 집합이 아니라 소프트웨어 배포 전 과정을 보호하는 종합적인 아키텍처 분야이다. 이는 신뢰를 구축하고, 소프트웨어 무결성을 보장하며, 통신 채널을 보호하고, 인프라를 안전하게 유지하며, 권한을 통제하고, 규제 준수를 지원하며, 기능안전을 보장한다. 소프트웨어 정의 차량, 지능형 로봇, 산업 자동화 플랫폼, 자율 운송 시스템, 그리고 미래의 피지컬 AI 생태계가 확대될수록 OTA 사이버보안은 신뢰성(Reliability), 안전성(Safety), 회복탄력성(Resilience)을 보장하는 핵심 기반 기술로서 더욱 중요한 역할을 수행하게 될 것이다.

## 12.5 Robot OTA Design

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 OTA(Over-The-Air) 설계는 현대 소프트웨어 업데이트 아키텍처를 로봇 시스템에 맞게 확장한 전문적인 기술 분야이다. OTA 기술은 원래 소비자 전자기기(Consumer Electronics)에서 시작되었으며 이후 소프트웨어 정의 차량(SDV, Software Defined Vehicle)의 핵심 기술로 발전하였다. 그러나 로봇은 동적인 환경(Dynamic Environment)에서 동작하며 물리적인 세계와 직접 상호작용하기 때문에 차량과는 다른 설계 요구사항을 가진다. 자율이동로봇(AMR, Autonomous Mobile Robot), 무인운반차(AGV, Automated Guided Vehicle), 산업용 매니퓰레이터(Industrial Manipulator), 휴머노이드(Humanoid), 사족보행 로봇(Quadruped Robot), 물류 로봇(Logistics Robot), 검사 로봇(Inspection Robot), 농업 로봇(Agricultural Robot), 광산 로봇(Mining Robot), 서비스 로봇(Service Robot), 국방 로봇(Defense Robot), 그리고 미래의 피지컬 AI(Physical AI) 시스템은 점점 더 소프트웨어 중심으로 발전하고 있다. 따라서 소프트웨어 업데이트, AI 모델 배포, 보안 패치, 운영 파라미터 변경, 알고리즘 개선을 원격으로 수행할 수 있는 OTA 기능은 로봇 운영의 필수 요소가 되고 있다.

과거에는 로봇 유지보수가 대부분 현장 서비스 방식으로 수행되었다. 엔지니어가 노트북이나 진단 장비를 로봇에 직접 연결하여 펌웨어(Firmware)를 업로드하고 운영체제(OS, Operating System)를 업데이트하며 설정 파일(Configuration File)을 수정하였다. 로봇 수가 적고 소프트웨어 구조가 단순하던 시절에는 이러한 방식이 충분히 효과적이었다. 그러나 현대의 로봇 시스템은 수백 대에서 수만 대 규모의 플릿(Fleet)으로 운영되며, 다양한 지역과 환경에 분산 배치된다. 이러한 환경에서는 물리적인 유지보수가 매우 비효율적이며 비용도 크게 증가한다. OTA 아키텍처는 이러한 문제를 해결하기 위한 핵심 기술로 등장하였다.

로봇 OTA 설계의 궁극적인 목적은 로봇 생애주기(Lifecycle) 전반에 걸쳐 안전하고 신뢰성 있으며 확장 가능한 소프트웨어 관리 체계를 제공하는 것이다. 현대 로봇은 지속적으로 진화해야 한다. 내비게이션(Navigation) 알고리즘은 개선되고, 인지 시스템(Perception System)은 새로운 AI 모델을 적용하며, 보안 취약점은 패치되어야 하고, 고객 요구사항에 따라 새로운 기능이 추가된다. OTA는 이러한 지속적인 발전을 지원하는 핵심 인프라 역할을 수행한다.

로봇 OTA 시스템은 크게 소프트웨어 개발 환경(Software Development Environment), CI/CD(Continuous Integration / Continuous Delivery) 파이프라인, 아티팩트 저장소(Artifact Repository), OTA 관리 플랫폼(OTA Management Platform), 클라우드 인프라(Cloud Infrastructure), 플릿 관리 시스템(Fleet Management System), 통신 네트워크(Communication Network), 로봇 게이트웨이(Robot Gateway), 온보드 컨트롤러(Onboard Controller), OTA 에이전트(Update Agent), 보안 프레임워크(Security Framework), 모니터링 시스템(Monitoring System)으로 구성된다. 이러한 요소들은 하나의 통합된 소프트웨어 배포 생태계를 형성한다.

소프트웨어 개발 환경은 OTA의 출발점이다. 개발팀은 내비게이션 개선, 인지 알고리즘 향상, 위치추정(Localization) 개선, 모션 제어(Motion Control) 최적화, 사이버보안 패치, 하드웨어 드라이버 업데이트, 클라우드 기능 확장, AI 모델 개선 등을 지속적으로 개발한다. 이후 자동화된 테스트 환경에서 기능 검증(Function Validation), 성능 검증(Performance Validation), 안전성 검증(Safety Validation), 보안 검증(Security Validation)을 수행한 후 OTA 배포 대상으로 승인된다.

CI/CD 파이프라인은 로봇 OTA에서 매우 중요한 역할을 수행한다. 현대 로봇은 운영체제, ROS2(Robot Operating System 2), 인지 모듈, 경로 계획(Path Planning), 위치추정, 통신 프레임워크, 데이터베이스(Database), AI 추론 엔진(Inference Engine), 컨테이너(Container), 클라우드 연동 서비스 등 복잡한 소프트웨어 스택(Software Stack)을 포함한다. 자동화된 빌드와 검증 과정은 이러한 요소들이 서로 호환되는지 확인하고 안정성을 보장한다.

아티팩트 저장소는 소프트웨어 패키지, 펌웨어 이미지, 설정 파일, AI 모델, 컨테이너 이미지, 캘리브레이션 데이터(Calibration Data)를 안전하게 저장한다. 모든 소프트웨어는 버전 관리(Version Control)와 디지털 서명(Digital Signature)을 통해 관리된다. 특히 로봇은 하드웨어 구성, 센서 종류, Payload, 적용 환경에 따라 서로 다른 소프트웨어가 필요할 수 있으므로 세밀한 버전 관리가 중요하다.

OTA 관리 플랫폼은 전체 업데이트 시스템의 중앙 제어 역할을 수행한다. 플랫폼은 로봇 인벤토리(Robot Inventory), 소프트웨어 버전, 하드웨어 구성, 배포 정책, 위치 정보, 운영 상태, 보안 상태를 관리한다. 플릿 운영자는 이를 통해 업데이트 정책을 정의하고 배포 현황을 모니터링할 수 있다.

클라우드 인프라는 OTA 운영의 확장성을 제공한다. 현대 로봇 플릿은 클라우드 기반 관리 시스템과 연계되어 운영된다. 클라우드는 대용량 저장소, 분산 콘텐츠 전송 네트워크(CDN, Content Delivery Network), 엣지 컴퓨팅 자원, 고가용성 서버(High Availability Server)를 제공하여 대규모 OTA 배포를 지원한다.

통신 네트워크는 OTA 데이터 전송의 기반이다. 창고 로봇은 산업용 Wi-Fi를 사용할 수 있으며, 제조 로봇은 Ethernet 네트워크를 사용할 수 있다. 실외 자율주행 로봇은 Wi-Fi, 셀룰러 통신, Private 5G, 위성 통신을 조합하여 사용할 수 있다. 군사용 로봇은 보안 통신망을 사용할 수도 있다. OTA 아키텍처는 이러한 다양한 네트워크 환경을 지원해야 한다.

로봇 OTA 게이트웨이는 소프트웨어 업데이트의 진입점 역할을 수행한다. 현대 로봇은 인지 컴퓨터(Perception Computer), 모션 제어기(Motion Controller), 안전 제어기(Safety Controller), 배터리 관리 시스템(BMS), 통신 모듈, AI 가속기(AI Accelerator) 등 여러 계산 장치를 포함한다. 게이트웨이는 이러한 장치들에 업데이트를 분배하며 라우팅, 보안 검증, 대역폭 관리, 설치 순서를 조정한다.

OTA 에이전트(Update Agent)는 로봇 내부에서 실제 업데이트를 수행하는 소프트웨어이다. OTA 서버와 통신하여 업데이트 가능 여부를 확인하고, 소프트웨어를 다운로드하며, 디지털 서명을 검증하고, 설치를 수행하며, 결과를 보고한다. OTA 에이전트의 신뢰성은 전체 OTA 시스템의 성공률을 좌우한다.

로봇 OTA 설계의 가장 큰 특징은 운영 상태 인식(Operational Awareness)이다. 스마트폰이나 PC와 달리 로봇은 물리적인 환경에서 움직이며 작업을 수행한다. 로봇이 이동 중이거나, 물체를 운반 중이거나, 사람과 상호작용하는 중에 업데이트를 수행하면 심각한 안전 문제가 발생할 수 있다. 따라서 OTA 시스템은 로봇의 현재 상태를 평가한 후 업데이트를 시작해야 한다.

운영 상태 평가는 임무 상태(Mission Status), 위치(Location), 배터리 잔량(Battery Level), 충전소 접근성(Charging Station Availability), 통신 품질(Network Quality), 환경 상태(Environmental Condition), Payload 상태, 안전 시스템 상태 등을 고려한다. 일반적으로 업데이트는 충전 스테이션, 유지보수 구역, 대기 상태(Standby Mode), 서비스 존(Service Zone)에서 수행된다.

안전성(Safety)은 로봇 OTA 설계의 핵심이다. 로봇은 사람과 직접 상호작용하거나 물리적인 작업을 수행하기 때문에 소프트웨어 오류가 실제 사고로 이어질 수 있다. 따라서 내비게이션, 모션 제어, 장애물 회피(Obstacle Avoidance), 위치추정, 센서 융합(Sensor Fusion), 매니퓰레이션(Manipulation), 안전 감시(Safety Monitoring) 기능을 업데이트할 경우 철저한 검증이 필요하다. 이를 위해 시뮬레이션(Simulation), HIL(Hardware-in-the-Loop), 단계적 배포(Staged Deployment), 롤백(Rollback), 설치 후 모니터링(Post-Installation Monitoring)이 활용된다.

AI 모델은 로봇 OTA의 핵심 업데이트 대상이 되고 있다. 현대 로봇은 객체 인식(Object Recognition), 의미 인식(Semantic Understanding), 이상 탐지(Anomaly Detection), 예지 정비(Predictive Maintenance), 인간-로봇 상호작용(Human-Robot Interaction), 자율 의사결정(Autonomous Decision Making)에 머신러닝(Machine Learning)을 활용한다. OTA는 새로운 AI 모델을 원격으로 배포할 수 있게 해준다.

AI 모델 업데이트는 일반 소프트웨어보다 더 복잡하다. AI 모델은 수 GB 이상의 크기를 가질 수 있으며, 추가적인 성능 검증과 벤치마킹(Benchmarking)이 필요하다. 따라서 OTA 플랫폼은 모델 압축(Model Compression), 델타 업데이트(Delta Update), 적응형 다운로드(Adaptive Download), AI 모델 검증(Model Validation)을 지원해야 한다.

사이버보안은 로봇 OTA 설계의 기본 요구사항이다. 로봇은 기업 네트워크, 공장, 물류센터, 공공장소, 중요 인프라에 연결되어 운영된다. OTA 시스템이 침해되면 공격자가 모션 제어 시스템, 센서 데이터, 운영 프로세스, 안전 기능에 접근할 수 있다. 이를 방지하기 위해 상호 인증(Mutual Authentication), 디지털 서명, 인증서 관리(Certificate Management), 암호화 통신(Encrypted Communication), Secure Boot, HSM(Hardware Security Module), IDS(Intrusion Detection System)가 적용된다.

Secure Boot는 OTA 이후에도 신뢰 체인(Chain of Trust)을 유지한다. 로봇은 부트로더, 운영체제, ROS2 미들웨어, 애플리케이션, AI 모델을 실행하기 전에 무결성을 검증한다. 이를 통해 저장장치가 공격받더라도 악성 코드 실행을 차단할 수 있다.

롤백 관리(Rollback Management)는 OTA 설계에서 매우 중요하다. 업데이트 이후 예상치 못한 동작이나 성능 저하가 발생할 수 있기 때문이다. OTA 시스템은 문제가 발생하면 자동으로 이전 버전으로 복구하여 서비스 중단을 최소화해야 한다.

플릿 규모의 배포 전략도 중요하다. 수천 대의 로봇을 동시에 업데이트하면 오류가 발생할 경우 전체 운영에 영향을 미칠 수 있다. 따라서 파일럿 그룹(Pilot Group), 카나리 배포(Canary Deployment), 지역별 배포(Regional Rollout), 위험 기반 배포(Risk-Based Deployment)를 통해 점진적으로 업데이트를 확산시킨다.

모니터링과 텔레메트리(Telemetry)는 OTA 운영을 지속적으로 관찰하기 위해 사용된다. 소프트웨어 버전, 설치 상태, 자원 사용량(Resource Utilization), 통신 성능, 실패 정보, 보안 이벤트, 롤백 발생 여부, 운영 상태를 수집하고 분석한다.

디지털 트윈(Digital Twin)은 미래 OTA 시스템에서 중요한 역할을 수행할 것으로 예상된다. 실제 로봇에 배포하기 전에 가상 환경에서 업데이트를 검증하여 안전성과 성능을 확인할 수 있다. 이를 통해 배포 위험을 크게 줄일 수 있다.

규제 준수(Regulatory Compliance)도 점점 중요해지고 있다. 로봇이 산업 자동화, 의료, 물류, 국방, 공공 인프라에 적용되면서 사이버보안, 소프트웨어 관리, 추적성(Traceability), 운영 책임성(Accountability)에 대한 규제가 강화되고 있다. OTA 시스템은 감사(Audit), 추적성, 보안 증거(Security Evidence), 소프트웨어 출처 검증(Provenance Verification)을 지원해야 한다.

미래의 로봇 OTA는 AI 기반 배포 계획(AI-Assisted Deployment Planning), 자율 업데이트 스케줄링(Autonomous Update Scheduling), 예측 기반 위험 평가(Predictive Risk Assessment), 엣지 컴퓨팅 오케스트레이션(Edge Computing Orchestration), 분산 신뢰 프레임워크(Distributed Trust Framework), 소프트웨어 정의 로보틱스(Software Defined Robotics), 피지컬 AI 생태계와 긴밀하게 통합될 것이다. OTA 시스템은 로봇의 상태를 스스로 평가하고, 최적의 시점에 업데이트를 수행하며, 문제를 예측하여 사전에 대응하는 방향으로 발전할 것으로 예상된다.

결론적으로 로봇 OTA 설계는 현대 로봇 시스템의 지속적인 진화를 가능하게 하는 핵심 인프라이다. OTA는 유지보수 비용을 절감하고, 사이버보안 대응력을 향상시키며, 혁신 속도를 높이고, 대규모 플릿 운영을 가능하게 한다. 향후 자율주행 로봇, 지능형 로봇, 플릿 기반 자동화, 피지컬 AI 시스템이 확대될수록 OTA 아키텍처는 로봇 산업의 핵심 기반 기술로서 더욱 중요한 역할을 수행하게 될 것이다.
