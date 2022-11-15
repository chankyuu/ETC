# LTE

## Summary

UE(사용자 단말)가 LTE 망에 접속할 때, 우선적으로 PDN(Packet Data Network)으로 연결해야 한다. 따라서 UE에게 PDN 주소(PDN IP address)가 할당되고 LTE 망에는(즉, UE와 P-GW 간에는) default 베어러가 생성된다. 이 default 베어러는 사용자 단말이 LTE 망에서 끊기기 전까지 유지된다.(즉, 초기 접속시 UE에게 할당한 IP 주소가 유지된다.)  

Default 베어러는 APN(Access Point Name)* 별로 생성되므로 사용자는 APN 별로 IP 주소를 할당받게 된다. 사용자가 할당받는 IP 주소 유형은 IPv4, IPv6 또는 IPv4/IPv6 주소일 수 있다.

*Default 베어러는 PDN 별로 생성된다. APN은 PDN ID로 PDN 정보는 사용자에게는 APN으로 전달되므로 사용자 입장에서 APN으로 표현하였다.


본 문서는 사용자 단말이 LTE 망에 초기 접속할 때 LTE 망이 사용자에게 IP 주소를 할당하는 방법에 대하여 설명한다. 사용자 단말은 하나의 PDN을 이용하고 PDN은 인터넷이며 IP 주소 유형은 IPv4인 경우를 예로 든다.


IP 주소 할당 방법은 다음 두가지로 나누어진다.
1) 지리적으로 같은 장소에서 여러 번 망에 초기 접속하는 경우(예, UE가 power off 후 다시 power on) IP 주소 할당 방법에 따라 사용자에게 IP 주소가 어떻게 할당되는지 설명한다. 
2) 지리적으로 다른 장소(두 도시)에서 초기 접속하는 경우 IP 주소 할당 방법에 따라 IP 주소가 어떻게 할당되는지 살펴본다.

이 문서에서는 사용자 단말이 동일한 지역에서 서로 다른 시간에 PDN 서비스를 이용하고자 LTE 망에 접속할때 LTE 망이 IP 주소를 할당하는 방법과 절차를 설명하였다.

## IP 주소 할당 방법 종류 및 특징

![image](https://user-images.githubusercontent.com/60462925/201264062-fda8ef0c-a498-47b4-8258-67eab38599d3.png)

사용자 단말이 LTE 망으로 접속하기 위하여 PDN과 연결이 되어있어야 하는데, 이를 위해 P-GW는 사용자 단말이 PDN에서 사용할 IP 주소(즉, PDN 주소)를 할당하고 이를 UE에게 부여한다.   
P-GW에서 사용자 단말에 IP 주소를 할당하는 방법은 다음 두가지로 나누어진다.  
1) 유동 IP 주소 할당 : 사용자 단말이 망에 접속할 때마다 망(P-GW)에서 자동으로 할당하는 방법
2) 고정 IP 주소 할당 : 가입할 때 IP 주소를 부여하고 망에 접속할 때마다 가입시 부여받은 IP 주소를 할당받는 방법


## 유동 IP 주소 할당

![image](https://user-images.githubusercontent.com/60462925/201266320-06b9115d-ae63-4407-b409-82a75eef2e9f.png)

 + **LTE 망 초기 접속 절차**
   1. P-GW(PDN 망)에서 사용자 단말에게 할당할 IP 주소 자동으로 결정
   2.  사업자는 P-GW에 미리 IP Pool을 provisioning해 놓고, 사용자 단말이 LTE 망에 초기 접속하면 망에서 동적으로 IP 주소를 결정하여 할당한다.
   3. LTE 망에 접속할 때마다 다른 유동 IP 주소 할당받음   
  
    
 + **IP Provisioning at P-GW**  

    ![image](https://user-images.githubusercontent.com/60462925/201266854-730220fc-0601-49d4-bb31-cfccbfedeee2.png)

    P-GW에는 사용자 단말에 할당할 IP 주소를 가진 IP Pool과 DNS 서버 IP 주소가 미리 설정되어 있다.

 + **유동 IP 주소 할당 절차**  
    1). [UE -> MME] PDN Connectivity 요청  
    
    + 사용자 단말이 power on되면 MME로 PDN Connectivity Request(PDN type=IPv4, PCO=DNS Server IPv4 Address Request) 메시지를 전달하여 PDN 연결을 요청한다.  
    (\*PDN Request 보낼 때 PCO 파라미터를 사용하여 외부 프로토콜/어플리케이션과 관련된 프로토콜을 요구할 수 있다(ex, DNS 서버 주소 요청, P-CSCF 주소 요청 등))  
    
    + IPv4 주소와 함께 PCO 필드에 의해 DNS 서버 IP 주소가 요청된다.   
    
    + PDN Connectivity Request 메시지는 ESM 메시지로 EMM 메시지인 Attach Request(IMSI*, ESM Message Container) 메시지의 ESM Message Container 필드에 포함되어 전달된다.    
    (\*UE가 유효한 old GUTI를 갖고 있으면 EPS Mobile ID로 GUTI가 사용될 수 있다.)

   2). ~ 3). [MME -> S-GW -> P-GW] Session 생성 요청  
    
    + MME는 HSS로부터 받은 가입 프로파일(subscription profile)을 기반으로 EPS 세션을 생성하기 위하여 Create Session Request(IMSI, PDN Type=IPv4, PDN Address=0.0.0.0, 
    PCO=DNS Server IPv4 Address Request) 메시지를 P-GW로 전달한다.   
    
    + 유동 IP 할당 방법이므로 가입 프로파일에는 IP 주소 정보가 없다.   
    
    + Create Session Request 메시지는 PDN 주소 필드에 0.0.0.0 값을 갖고 PCO 필드에 UE로부터 수신한 PCO 정보를 그대로 포함한다.  
   
   4). [P-GW] PDN 주소 및 DNS 서버 주소 할당

     + P-GW는 PDN 타입과 PDN 주소(0.0.0.0)를 보고 UE에게 IPv4 주소를 할당해야 함을 인지하고, IPv4 pool에서 IP 주소를 선택하여 UE에게 할당한다(ex. UE IP=1.1.1.5). PCO 필드에 의해 DNS 서버 IP 주소가 요청되었으므로 DNS 서버 IP 주소를 같이 할당한다.
   
   5). ~ 6). [P-GW -> S-GW -> MME] Session 생성 응답
   
     + 단계 2)와 단계 3)에 대한 응답으로 Create Session Response 메시지를 MME로 전달한다. 
  
     + 이 메시지는 PDN 주소에 local P-GW가 동적으로 할당한 UE IP 주소를 포함하고, 사용자가 PCO 필드를 통해 요청한 DNS 서버 IP 주소를 PCO 필드에 포함하여 전달한다.

   7). [MME -> UE] Default 베어러 Context 활성화 요구
   
     + MME는 UE에게 Activate Default EPS Bearer Context Request (PDN Type=IPv4, PDN Address=UE IP(1.1.1.5), PCO={Primary DNS IP, Secondary DNS IP}) 메시지를 전달하여 default 베어러 context를 활성화 시킬 것을 요청한다. 
     + 이 ESM 메시지는 UE IP 주소와 DNS 서버 IP 주소를 포함하며, EMM 메시지인 Attach Accept 메시지에 포함되어 전달된다.

   8). [UE] PDN 서비스를 이용할 IP 주소 획득 : 유동 IP 주소

     + 사용자 단말은 IP 주소(유동 IP 주소: 1.1.1.5)와 함께 DNS 서버 IP 주소(Primary DNS IP=10.1.1.1, Secondary DNS IP=10.1.1.2)를 획득한다. 
     + UE와 P-GW 간에는 default 베어러가 설정되고, 이제 UE는 PDN으로 연결됐다.

## 고정 IP 주소 할당

![image](https://user-images.githubusercontent.com/60462925/201273659-0801b9cd-8db8-4502-9a7b-29afc8547916.png)

 + **LTE 망 초기 접속 절차**
   1. 사용자가 사업자 망에 가입시 사용자 단말이 영구적으로 사용할 IP 주소를 할당해줌
   2. 사업자는 사용자 단말에게 할당한 고정 IP를 부수적인 가입 정보와 함께 망(HSS)에 가입정보로 provisioning해 놓고, 사용자 단말이 LTE 망에 접속하면 P-GW는 HSS로부터 고정 IP 주소를 수신하여 이를 사용자 단말에 전달한다.
   3. LTE 망에 접속할 때마다 같은 IP 주소 할당받음 

 + **IP Provisioning at HSS**  

    ![image](https://user-images.githubusercontent.com/60462925/201273864-edf520bb-1efd-426c-bd2a-d6ce48fac059.png)

    HSS에는 가입자 별로 가입 프로파일이 제공되어 있다.
    (*가입 프로파일 : PDN 접속에 사용되는 PDN 타입, PDN 주소를 포함)
    
 + **IP Provisioning at P-GW**  

    ![image](https://user-images.githubusercontent.com/60462925/201273929-d766f9f4-0c8d-44c8-8341-0426828d73f0.png)

    P-GW에는 사용자 단말에 할당할 DNS 서버 IP 주소가 미리 설정되어 있다.
    (*고정 IP 주소 할당에서는 사용자 단말에 할당될 IP 주소가 이미 HSS에 제공되어 있기 때문에 P-GW에 DNS 서버 IP 주소만 설정되어 있다.)

 + **고정 IP 주소 할당 절차**  
    1). [UE -> MME] PDN Connectivity 요청  
    
      + 사용자 단말이 power on되면 MME로 PDN Connectivity Request(PDN Type=IPv4, PCO=DNS Server IPv4 Address Request) 메시지를 전달하여 PDN 연결을 요청한다. 
      
      +  IPv4 주소와 함께 PCO 필드에 의해 DNS 서버 IP 주소가 요청된다.

    2). [MME -> HSS] LTE 망에 등록 요청  
    
      +  MME는 HSS로 Update Location Request 메시지를 전달하여 UE가 해당 MME(ex. MME1)의 제어를 받음을 알리고 망에 등록한다.
   
    
    3). [HSS -> MME] 가입 프로파일 전달

      + HSS는 UE가 해당 MME1에 등록되어 있음을 파악하고, MME1으로 Update Location Answer(IMSI, PDN Type=IPv4, PDN Address=Static UE IP(1.1.1.1)) 메시지를 전달하여 UE의 가입 프로파일을 전달한다. 이 가입 프로파일은 UE에게 부여한 고정 IP 주소를 포함한다.
   
    4). ~ 5). [MME -> S-GW -> P-GW] Session 생성 요청
   
      + MME는 HSS로부터 UE의 가입 프로파일을 수신하여 UE가 고정 IP 주소(1.1.1.1)를 가짐을 파악한다. 
      
      + MME는 PDN 주소 필드에는 HSS로부터 수신한 고정 IP 주소를 포함하고 PCO 필드에는 UE로부터 수신한 PCO 정보를 그대로 포함하여 Create Session Request(IMSI, PDN Type=IPv4, PDN Address=Static UE IP(1.1.1.1), PCO=DNS Server IPv4 Address Request) 메시지를 발생하여 P-GW로 전달한다.


    6). ~ 7). [P-GW -> S-GW -> MME] Session 생성 응답
   
      + 단계 5)와 단계 4)에 대한 응답으로 P-GW와 S-GW는 Create Session Response (IMSI, PDN Type=IPv4, PDN Address=Static UE IP(1.1.1.1), PCO={Primary DNS IP, Secondary DNS IP}) 메시지를 MME로 전달한다.
      
      + 이 메시지는 PDN 주소 필드에 UE가 부여받은 고정 IP 주소를 포함하고, PCO 필드에 사용자가 요청한 DNS 서버 IP 주소를 포함한다.


    8). [MME -> UE] Default 베어러 Context 활성화 요구

      + MME는 UE에게 Activate Default EPS Bearer Context Request (PDN Type=IPv4, PDN Address=Static UE IP(1.1.1.1), PCO={Primary DNS IP, Secondary DNS IP}) 메시지를 전달하여 default 베어러 context를 활성화 시킬 것을 요청한다. 
      
      + 이 ESM 메시지는 사용자의 고정 IP 주소(1.1.1.1)와 DNS 서버 IP 주소를 포함하며, EMM 메시지인 Attach Accept 메시지에 포함되어 전달된다.

    
    9). [UE] PDN 접속 IP 주소 획득 : 고정 IP 주소
    
      + 사용자 단말은 IP 주소(고정 IP 주소: 1.1.1.1)와 함께 DNS 서버 IP 주소(Primary DNS IP=10.1.1.1, Secondary DNS IP=10.1.1.2)를 획득한다. 

      + UE와 P-GW 간에는 default 베어러가 설정되고, 이제 UE는 PDN으로 연결됐다.

## terms
 + UE : User Equipment
 + PDN : Packet Data Network의 약어로, 쉽게 IP Network라고 보면 된다. 하나의 단말에서 각각 다른 네트워크를 구성하기 위해 사용한다. PDN에서 사용할 IP 주소가 주어짐
 + APN : Access Point Name의 약어로, PDN의 이름 = APN이다(즉, PDN 정보는 사용자에게 APN으로 전달됨).
 + P-GW : PDN Gateway
 + PCO : Protocol Configuration Options
 + bearer : 
 + HSS : Home Subscriber Server
 + MME : Mobile Management Entity
 + IMSI : International Mobile Subscriber Identity
 + EPS : Evolved Packet System
 + ESM : EPS Session Management
 + EMM : EPS Mobility Management
 + GUTI : Globally Unique Temporary Identifier

## References
https://www.netmanias.com/ko/?m=view&id=techdocs&no=5807