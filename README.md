# Netkerfi Lokaverkefni | Útgáfa II

### Private ip á HeimaNet routernum vistast ekki
### Það á að vera 192.168.1.1 en breytist alltaf í 192.168.0.1

* ### Stillingar
    * #### Allir routerar og switch-ar
        * ##### SSH rsa 1024 bita
            * Notandanafn: admin
            * Lykilorð: cisco123
        * ##### Console port
            * Lykilorð: ccna
        * ##### Privileged user
            * Lykilorð: cisco
        * ##### Motd
            * Adgangur bannadur!
        * ##### Allt vistað í startup-config

* ### Routerar 
    
    * #### ISP_EDGE:
        * g0/0 10.0.0.1 255.255.0.0 -> RIX
        * g0/1 10.1.0.1 255.255.0.0 -> R_ADSL
        * g0/2 10.2.0.1 255.255.0.0 -> R_FIBER

    * ##### R_ADSL:
        * g0/0 10.1.0.2 255.255.0.0 -> ISP_EDGE
        * g0/1 10.3.0.1 255.255.0.0 -> S_ADSL

    * ##### R_FIBER:
        * g0/0 10.2.0.2 255.255.0.0 -> ISP_EDGE
        * g0/1 10.4.0.1 255.255.0.0 -> S_FIBER
        * g0/2 10.5.0.1 255.255.0.0 -> DNS server

    * ##### Ljosleidarabox:
        * g0/0 10.4.0.3 255.255.0.0 -> S_FIBER
        * g1/0 10.6.0.1 255.255.0.0 -> VinnuNet

* ### Switch

    * ##### RIX:
        * Vlan 10.0.0.2 255.255.0.0
        * g0/1 -> ISP_EDGE
        * g0/2 -> www.tskoli.is

    * ##### S_FIBER:
        * Vlan 10.4.0.2 255.255.0.0
        * g0/1 -> R_FIBER
        * g1/1 -> Ljosleidarabox

    * ##### S_ADSL:
        * Vlan 10.3.0.2 255.255.0.0
        * g0/1 -> R_ADSL
        * g1/1 -> DSLAM
        * g2/1 -> ISP_DHCP(ADSL)
        
    * ##### S_Vinna:
        * Vlan 192.168.0.100 255.255.255.0
        * Aðgenginleg frá 10.6.0.5
        * g0/1 -> VinnuNet

* ### Wireless routerar

    * ##### HeimaNet:
        * IP valið af dhcp serverinum
        * 192.160.1.1
        * dhcp: frá 192.168.1.100 og max 50 client
        * Wireless net lykilorð: cisco123
            * HeimaNet_2.4Ghz | Spjaldtölva1, Fartolva2
            * HeimaNet_5Ghz | Fartolva1, Simi1
        * Lykilorð fyrir gestanet er: cisco321
            * HeimaNet_Gestir_5Ghz | Simi2

    * ##### VinnuNet:
        * 10.6.0.5
        * 192.160.0.1
        * dhcp: frá 192.168.0.102 og max 50 client
        * Wireless net lykilorð: cisco123
            * VinnuNet_2.4Ghz | Spjaldtölva2, Fartolva3
            * VinnuNet_5Ghz | Fartolva4, Simi3
        * Lykilorð fyrir gestanet er: cisco321
            * VinnuNet_Gestir_5Ghz | Simi4

* ### Serverar

    * ##### www.tskoli.is:
        * 10.0.0.3
        * Vefsíða aðgengileg frá:
            * 10.0.0.3
            * tskoli.is
            * www.tskoli.is

    * ##### ISP_DHCP(ADSL):
        * 10.3.0.3
        * frá 10.3.0.4 og 512 client

    * ##### DNS:
        * 10.5.0.2

    * ##### WebServer:
        * 192.168.0.101
        * Vefsíða aðgengileg frá:
            * 10.6.0.5
            * 192.168.0.101

* ### IP route

    * ##### ISP_EDGE
        * 10.0.0.0 255.255.0.0 g0/0
        * 10.3.0.1 255.255.0.0 g0/1
        * 192.168.1.0 255.255.255.0 g0/1
        * 10.2.0.0 255.255.0.0 g0/2
        * 10.4.0.0 255.255.0.0 g0/2
        * 10.5.0.0 255.255.0.0 g0/2
        * 10.6.0.0 255.255.0.0 g0/2
        * 192.168.0.0 255.255.255.0 g0/2

    * ##### R_ADSL
        * 0.0.0.0 0.0.0.0 g0/0
        * 10.3.0.0 255.255.0.0 g0/1
        * 192.168.1.0 255.255.255.0 g0/1

    * ##### R_FIBER
        * 0.0.0.0 0.0.0.0 g0/0
        * 10.4.0.0 255.255.0.0 g0/1
        * 10.6.0.0 255.255.0.0 g0/1
        * 192.168.0.0 255.255.255.0 g0/1
        * 10.5.0.0 255.255.0.0 g0/2

    * ##### Ljosleidarabox
        * 0.0.0.0 0.0.0.0 g0/0
        * 10.6.0.0 255.255.0.0 g1/0
        * 192.168.0.0 255.255.255.0 g1/0