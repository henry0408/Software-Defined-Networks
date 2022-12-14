# lab7 ONOS

作为目前最受欢迎的两款开源控制器，OpenDaylight 和 ONOS 在业界非常的火，至于 Ryu/Floodlight 之类，只能靠后站了。OpenDaylight作为 企业（数据中心场景） 主推的控制器，支持的功能较多，也更庞杂；而 ONOS 是运营商主推的控制器，更加贴近运营商的场景。
具体来讲，ODL 采取了更为平缓的 SDN 演进方式，从理念上更为贴近广义的 SDN，即不局限于 OpenFlow 协议，不局限于完全将控制平面从转发设备上剥离，通过已有的网络协议将部分的控制逻辑放到 Controller 上。
ONOS 更贴近于 SDN 诞生之初时狭义的 SDN 概念，即通过 OpenFlow 将控制平面和转发平面完全分离，网络设备只是进行转发的黑盒子，通过 Controller 完成一切计算。ONOS 所选择的理念与运营商自己的利益息息相关，只有将控制能力拿到自己手里，才能在整条产业链上逐步摆脱设备厂商的控制。

## 1. ONOS安装
从https://wiki.onosproject.org/display/ONOS/Basic+ONOS+Tutorial 下载.ovs文件，双击用VM Virtual Box打开

## 2. ONOS cluster setup
### 1) 双击‘Setup ONOS Cluster’
![image](https://user-images.githubusercontent.com/58734009/189521767-97d783d9-9b4d-4130-ab29-7daa0977dc49.png)

![image](https://user-images.githubusercontent.com/58734009/189521790-90390d47-8fe5-4399-85f8-a08ab6cf9151.png)

### 2）双击'ONOS CLI'
![image](https://user-images.githubusercontent.com/58734009/189521825-8aaba395-5207-46e1-8134-4ea2ce6951fd.png)

![image](https://user-images.githubusercontent.com/58734009/189521849-973cf21c-9f5b-4f3d-a81a-7456997afa13.png)

```onos>```表示你在ONOS command line

```mininet>```表示你在mininet

### 3) 双击Spine Leaf Topology
![image](https://user-images.githubusercontent.com/58734009/189522905-87d145f1-2cb5-47cd-87d4-3033aa56de95.png)

![image](https://user-images.githubusercontent.com/58734009/189522918-23ccb820-c97a-4996-a51c-faac3d899978.png)

## 3. Reactive Forwarding
这是一个应用程序，它为到达控制器的每个数据包安装flow；必须在使用“ping”命令之前load。请按照以下步骤进行检查：

* 在mininet中输入：```mininet>h11 ping h41```

  ![image](https://user-images.githubusercontent.com/58734009/189523016-3cedf3ca-ec2d-41f5-b0b2-17d5641d8a88.png)

  ping失败了，因为我们没有加载reactive forwarding application数据转发模块。

* check the list of loaded application：```onos>apps -a -s```

  ![image](https://user-images.githubusercontent.com/58734009/189523056-d3e72204-ba1c-46f1-8853-caf31e802eef.png)

* 加载reactive forwarding application: ```onos>app activate org.onosproject.fwd```

  ![image](https://user-images.githubusercontent.com/58734009/189523130-5a369e58-7a4f-4786-8aa2-87875c239f38.png)

* 再次尝试ping：```mininet>h11 ping h41```

  ![image](https://user-images.githubusercontent.com/58734009/189523354-60553e0c-3208-4962-864e-e896ff2781f8.png)
 
* 关闭reactive forwarding application: ```onos> app deactivate org.onosproject.fwd```

* see a list of ONOS CLI commands: ```onos> help onos```

  ![image](https://user-images.githubusercontent.com/58734009/189523514-c6ef954c-f027-4c7d-a373-9bb9162f53ba.png)

## 4. Devices Command

### 1)  List the device currently known in the system
输入```onos> devices```

![image](https://user-images.githubusercontent.com/58734009/189523898-33342e81-3a8f-4ee6-8635-de37be2cc21c.png)

从设备列表中，我们有以下主要属性—: 设备id和一个指示设备当前是否已启动的布尔值。其他属性还包括设备的类型以及它与ONOS实例之间的角色关系。

## 5. Links Command
list the links detected by ONOS: ```onos> links```

![image](https://user-images.githubusercontent.com/58734009/189524212-82f59499-1889-469a-a8f9-8589d3d9a510.png)

显示了source device-port pair to destination device port pair的 discovered links

‘type’ 表示 whether the link is a direct connection between two devices or not.

## 6. Host command
```onos>hosts```

结果display the hosts’ id, MAC address and where in the network it is connected

 ‘-1’ （在此版本中为id后面的None）in the id filed is represents the VLAN information, in this case there is no VLAN.
 
 ## 7. Flows Command
 observe which flow entries are currently registered in the system\
 * PENDING_ADD – the flow has been submitted and forwarded to the swtich
 * ADDED – the flow has been added to the switch
 * PENDING_REMOVE – the request to remove the flow has been submitted and forwarded to the switch
 * REMOVED – the rule has been removed

![image](https://user-images.githubusercontent.com/58734009/189525442-e170d15e-cd13-4ae0-9192-c87d511897e2.png)

Each flow entry is tagged by an application id that identifies which application installed this flow entry用来标识哪个应用程序安装了这个流条目。此特性将帮助管理员识别哪些应用程序可能行为不当或消耗了许多资源。

Additionally, each flow entry defines a selector and treatment which is the set of traffic matched by the flow entry and how this traffic should be handled
每个流条目都定义了一个选择器和处理方法，它是由流条目匹配的流量集，以及应该如何处理该流量

## 8. Paths Command

* 输入```onos>paths<Tab>``` （注意<Tab>是直接按键盘Tab键）
  
  ![image](https://user-images.githubusercontent.com/58734009/189533052-20339470-72bb-4d53-86c6-3874008664a3.png)

* 输入```onos>paths of:000000000000000b of:000000000000000e```
  
  ![image](https://user-images.githubusercontent.com/58734009/189533128-62deb7c8-9da4-4f38-aff5-2db077f04792.png)

## 9. Internet Command
输入```onos>intents```

* SUBMITTED – the intent has been submitted and will be processed soon


## 10. Starting your intentions

* 先查看所有可用的hosts：
  ![image](https://user-images.githubusercontent.com/58734009/189533799-cf0c1a04-64ef-4cc8-8a7c-a91b425f8147.png)
* 接着选择任意两个host，并在他们之间install a host to host intent
  Please note, if the host doesn't have VLAN, please change -1 on demand

  ![image](https://user-images.githubusercontent.com/58734009/189533820-11e743a6-94ab-4603-ae02-9d38faf29fc6.png)

* 现在我们设置了两个hosts之间的一条路径，你可以看到intent：  
  ![image](https://user-images.githubusercontent.com/58734009/189533849-a41f6ed3-0339-416b-8286-823d4ca190ef.png)
  
![image](https://user-images.githubusercontent.com/58734009/189533952-99db4c40-eaad-45eb-847c-ca3d4e1be6a5.png)
  
