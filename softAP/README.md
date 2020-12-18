# to build
$ make build flash

# flash and monitor
$ make monitor

# connect a station to esp32 SSID TestSSID
# reported as connected in the monitor window
I (29283) wifi:new:<1,1>, old:<1,1>, ap:<1,1>, sta:<255,255>, prof:1
I (29283) wifi:station: 00:0f:00:e1:d4:8f join, AID=1, bgn, 40U
I (29283) wifi softAP: station 00:0f:00:e1:d4:8f join, AID=1
I (29283) wifi softAP: WIFI Listing:
I (29293) wifi softAP:     [0]: 000F00E1D48F 0.0.0.0
I (29523) esp_netif_lwip: DHCP server assigned IP to a station, IP is: 192.168.4.2
I (30763) wifi softAP: WIFI Listing:
I (30763) wifi softAP:     [0]: 000F00E1D48F 192.168.4.2

# A wifi listing is repeated every 10 seconds
#
# reboot esp32 device 2 methods described
# method 1
#     type CTRL']' in monitor
#     now reconnect monitor
$ make monitor


# lots of errors of the form 

W (3223) wifi:m f deauth

E (3223) wifi:esf_buf: t=3 l=28 max:32, alloc:32 no eb, TXQ_BLOCK=4000
W (3233) wifi:alloc eb len=28 type=3 fail, heap:226836

# ulitimately the IP address is not determined and listed as 0.0.0.0
I (11823) wifi:new:<1,1>, old:<1,1>, ap:<1,1>, sta:<255,255>, prof:1
I (11823) wifi:station: 00:0f:00:e1:d4:8f join, AID=1, bgn, 40U
I (11823) wifi softAP: station 00:0f:00:e1:d4:8f join, AID=1
I (11833) wifi softAP: WIFI Listing:
I (11833) wifi softAP:     [0]: 000F00E1D48F 0.0.0.0
I (20773) wifi softAP: WIFI Listing:
I (20773) wifi softAP:     [0]: 000F00E1D48F 0.0.0.0

I have noticed this is specific to some station types
I have connected 
- Linux station    000F00E1D48F
- iPhone station   5611A01BBB66
- iPad station     1C9E46CB0740
- Android station  74EB80B26E12

The real problem is that the station initially handed an IP Address before the reboot,
thinks it still has that address, thus causing a conflict when this same address is handed out
to another station 

I tried connecting the 4 stations listed above as stations, after the reset I get the following

I (9883) esp_netif_lwip: DHCP server assigned IP to a station, IP is: 192.168.4.2
I (10293) wifi:new:<1,1>, old:<1,0>, ap:<1,1>, sta:<255,255>, prof:1
I (10293) wifi:station: 00:0f:00:e1:d4:8f join, AID=3, bgn, 40U
I (10293) wifi softAP: station 00:0f:00:e1:d4:8f join, AID=3
I (10303) wifi softAP: WIFI Listing:
I (10303) wifi softAP:     [0]: 5611A01BBB66 0.0.0.0
I (10313) wifi softAP:     [1]: 1C9E46CB0740 192.168.4.2
I (10313) wifi softAP:     [2]: 74EB80B26E12 0.0.0.0
I (10323) wifi softAP:     [3]: 000F00E1D48F 0.0.0.0

the behavior is random - indicating a race condition.

a solution would be to be able to disconnect a station and force authentication 
- is that possible ?

There is no other method to ensure unicast UDP can be implemented