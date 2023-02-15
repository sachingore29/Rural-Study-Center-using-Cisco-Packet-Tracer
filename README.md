# Rural-Study-Center-using-Cisco-Packet-Tracer
A Project made using Cisco Packet Tracer
Rural Study Center using Cisco Packet Tracer is about designing a topology of a network that is a LAN (Local Area Network ) for a Rural Area in which various computers of different Sections are Setup so that they can interact and communicate with each other by interchanging data .
Rural Study Center design comprises of Main Admin Office,Other offices (Registration ,Accounts,Job office),Vocational Training Lab ,Digital Training Module,Server Room .

 Step 1  : Create Networks with proper labelling of ports and networks .
 First open the Cisco Packet Tracer desktop and select the devices ,the Create a network topology of Rural Study Center as shown in the uploaded image, use an automatic connecting cable to connect the devices with others.

Step 2 : IP addressing Plan 

            An Internet Protocol address is a numerical label assigned to each device connected to a computer network that uses the Internet Protocol for communication. An IP address serves two main functions: host or network interface identification and location addressing.

             
              
Vocational Training Lab                 -   	192.168.1.0 /24    [ VLAN 10 ]
               Server Room                    -               	192.168.2.0 /24
               Main Admin Office         -          	192.168.3.0/24     [VLAN 30]
               Other Offices (Registration,Accounts,Job office)      -                                 	192.168.4.0/24    [VLAN 40]
               Digital Training Module     -             	192.168.5.0/24     [VLAN 50]

       Subnet :
A subnetwork or subnet is a logical subdivision of an IP network. The practice of dividing a network into two or more networks is called subnetting. Computers that belong to a subnet are addressed with an identical most-significant bit-group in their IP addresses.
Prefix size 24,subnet mask 255.255.255.0 ,Available subnets 1 ,usable hosts per subnet 254,total usable hosts 254



Step 3 : Configure routers with IP address an


Step 3 : Configure routers with IP address and subnet mask   
            Configure the ports of Routers as per given table

  Router Name
Subnet mask	Interface  with
	IP address             

    Router 0
255.255.255.0	Vocational Training Lab      ( Fa0/0  )
	192.168.1.1            

    Router  0
255.0.0.0	Router 1    (Serial 2/0)
	40.1.1.1                 

   Router 1
255.255.255.0	Server Room     ( Fa0/0  )
	192.168.2.1            

    Router  1
255.255.255.0	Digital Training Module (Fa 1/0)
	192.168.5.1              

  Router 1
255.0.0.0	Router 0     ( Serial 2/0  )
	40.1.1.2                    

    Router  1
255.0.0.0	Router 2    (Serial 3/0)
	50.1.1.1                 

   Router 2
255.255.255.0	Main Admin Office  (Fa0/0 )
	192.168.3.1            

  Router 2
255.255.255.0	Other Offices (Fa1/0)
	192.168.4.1            

  Router 2
255.0.0.0	Router 1 (Serial 3/0)
	50.1.1.2                 



 Put routing information in the Routers as per given table

Router name	RIP Network Address 
	
Router 0	40.0.0.0 ,    192.168.1.0
Router 1	40.0.0.0 , 50.0.0.0 , 192.168.2.0 , 192.168.5.0 
Router 2	50.0.0.0 ,192.168.3.0  , 192.168.4.0
                            


  Step 4 : DHCP (Dynamic Host Configuration Protocol ) configuration is performed on routers to assign an IP address , Subnet mask , gateway address at DNS Server address to the host systems ,with this configuration the dyanamic IP address is assigned which enables the administrator to easily connect a new host in the configured network .
Configuring the DHCP server 

	Give static address (192.168.2.2) (subnet 255.255.255.0 and default gateway (192.168.2.1) to DHCP Server   
	Switch on DHCP services and define range of the address pool  as per given table
Pool Name
subnet mask	Default Gateway
	Start IP address        

           Digital Training Module
255.255.255.0 	192.168.2.1
	192.168.5.11        

           Other Offices
255.255.255.0	192.168.2.1
	192.168.4.11        

            Main Admin Office
255.255.255.0 	192.168.2.1
	192.168.3.11        

            Vocational Training Lab
255.255.255.0	192.168.2.1
	192.168.1.11        


	Configure the ports which are gateway of the network 
Go to command line interface CLI of Router 0
Router > en
Router # config t
Router (config)#int Fa0/0 (command for interface at which our network is attached )
Router(config-if)#ip helper-address  192.168.2.2 (IP address of the DHCP server)
Router(config-if)#exit
Router(config)#exit
Router#
	Now go to desktop of host pcs in vocational training lab and check DHCP IP configuration
	Do similarly for other Routers (Router 1 ,Router 2)

RESULT 1 : Using DHCP IP address given to all  host devices  in the entire network . A Successful connection is established between all the devices connected in the network , Verify by transmitting packet from PC0 in Virtual Training Center to PC2 of Main Admin Office  and transmitting packet from PC0 of virtual Training Center to PC 6 of Other Offices (Job Office) . Using DHCP IP address given to all pcs in the entire network .

Step 5 :- To activate access point and wireless devices  of Digital Training Module (Wireless Networking)
	Go to access point device config mode select port 1 status on ,put SSID as RURAL STUDY CENTER select wep and put wep key 1234567890 (you can put any key )
	Go to physical module of laptop 1 turn off power switch ,remove network interface and add wireless module again turn laptop on .
	Go to config mode of laptop 1 select wireless0 make port status on and put SSID as RURAL STUDY CENTER ,select wep,put wep key as 1234567890
	Go to desktop of laptop 1 and select pc wireless ,click on connect than click on refresh ,again click on connect put wep key 1 as 1234567890 ,click on connect 
	In this way we had activated access point and laptop 1 for wireless communication ,similarly we can active laptop 2 and other wireless devices 


RESULT 2 : Verify by transmitting packet from laptop 1 to laptop 0 and from PC 10 to laptop 1,  A Successful connection is established between all the wireless  devices  connected to access point  in the network .

Step 6 : To improve network performance to provides security  as admininistator can configure access list according to the needs and deny the unwanted packets from entering the network ,provides control over the traffic as it can permit or deny according to the need of network.For RURAL STUDY CENTER  here we are applying Standard Access-List ( close to the destination) to deny access of Vocational Training Center (Entire network with IP address 192.168.1.0 ) to Main Admin Office  (network with IP address of 192.168.3.0 )

	Go to CLI of Router 2 (close to our destination )and switch to privilage mode from user mode with the help of enable command  and use show ip interface brief command to check Router 2 configurations in brief ,now check routing table by using show ip route command 
	To configure the standard ACL ,we will go to router configure mode with the help of configure terminal command 
	Now use command access-list 10(standard ACL number ) deny 192.168.1.0 (network of vocational training center ) 0.255.255.255 (when we are deny or permit entire network using network ID use  wild card mask )
	Access-list 10 permit any (allowing other network to communicate ) ,than select interface Fa0/0 (where we are applying )
	Command ip access-group 10 out (inbound /outbound specify the traffic to be filtered),press control z and use command show access -list 10 
   Router>
   Router>en
   Router# show ip interface brief 
   Router# show ip route
   Router # config t
   Router(config)# access-list 10 deny 192.168.1.0 0.255.255.255
   Router(config)#
   Router(config)#access-list 10 permit any
   Router(config)#interface Fa0/0
   Router(config-if)#
   Router(config-if)#ip access-group 10 out 
   Router (config-if)#
   Router(config-if)# ^z (control z)
   Router #
   Router # show access-lists 10 

RESULT 3 : Verify by transmitting packet from PC0 (Vocational training lab ) to PC2 (Main admin office ) you will get access denied (failed status) now verify by transmitting  packet from PC0 (Vocational training lab ) to PC6 (Other offices) you will get access (succesful status) . here we are applied Standard Access-List ( close to the destination) to deny access of Vocational Training Center (Entire network with IP address 192.168.1.0 ) to Main Admin Office  (network with IP address of 192.168.3.0 )


Future Scope :  the network design can be expanded to connect to various other modules  and installing Firewall for better security.

#####    Any one can come to this repository can forkit can use for their own project  and even contribute to this repository . contributions are invited to add firewall for better

For better security












































