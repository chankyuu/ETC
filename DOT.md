# NetMgr_preview
Describe the overall flow for the NetMgr.

## Call Manager to Packet Connect Manager(1/2)
  
 ![image](https://user-images.githubusercontent.com/60462925/201004330-e47a6b48-40f4-48f7-adc8-290703e4a04e.png)

## Call Manager to Packet Connect Manager(2/2)

 ![image](https://user-images.githubusercontent.com/60462925/201004614-a9e89e82-a77e-4a12-843d-1a157350beca.png)

## Setup Data Call Procedure

  ![image](https://user-images.githubusercontent.com/60462925/201004676-891cb8aa-6254-438d-883b-348b3eeb7060.png)

## APN Retry Process Flow

  ![image](https://user-images.githubusercontent.com/60462925/201004736-445a3d58-2956-4220-a63e-72763da68f3a.png)

## Desribe
 1. CallManager -> PacketConnectMgr(PacketConnectReq)
 2. PacketConnectMgr 내부의 *PACKET_CONN*에서 APNConnectMgr(=PacketHandlerC)를 통해 APN 연결을 확인한 후 *DATA_CONN*으로 진행, *DATA_CONN*에서 SOAPSessionMgr를 통해 실제 OCC와 연결한다.

 3. APNConnectMgr(=PacketHandlerC) 내부   
      3-1. PKT_IDLE 상태에서 always on / non always on에 따라 retry한다.   
      3-2. Connecting : RIL을 통해 SetupDataCall request 후 response 대기, 성공하면 PacketInterfaceMgr로 정보 전달하여서 routing setting  
      3-3. Connected : 연결된 상태, 5초 간격으로 연결 정보 프린트
      3-4. Disconnecting : Retry, spec, network 문제로 연결이 종료될 때 PacketInterfaceMgr로 전달하여 routing 정보 clear, RIL을 통해 DeactivateDataCall request.

 4. PacketInterfaceMgr : APNConnectMgr에서 SetupDataCall이 성공적으로 이루어지면 Network로부터 전달받은 IP, DNS, GW등의 정보로 routing, NAT 등 설정 / NetIOController, FirewallMgr로 구성


## Tracker Issue

  + TCPXI-22605 [Packet] Other Rmnets are assigned to the same rmnet_data0
    + ISSUE CASE
      Rmnet0와 Rmnet1에 동일한 IP가 할당됨

    + RC  
      Becase NetworkMgr in AP received no information of IP address, NetworkMgr waits
      util receive the information of IP address. NetworkMgr should stop waiting,
      but there was no condition about PDP_FAIL case
      
    + CA  

      ![image](https://user-images.githubusercontent.com/60462925/201004920-aa0c306f-5ec6-4e02-84ec-485ccac5a4ee.png)


  + TCPXI-27176 [GB][Packet] Invalid APN, but all connected
    + ISSUE CASE
      유효하지 않은 APN인데도 Call이 연결됨

    + RC  
      setupdatacall의 response가 오기전에 변경된 apn으로 setupdatacall을 다시 요청함

    + CA  

       ![image](https://user-images.githubusercontent.com/60462925/201005019-eec29869-978e-40c7-b895-57fc2daceb7e.png)
