# to build
make build flash

# flash and monitor
make monitor

# connect a station
# listen on udp/9999
# on linux
# $ nc -lku 9999

# reboot device
# IP correctly assigned and data sent to station

#
# reboot device
# method 1
#     type CTRL']' in monitor
#     reconnect monitor
#     make monitor
# method 2
#     hit reboot button on board
#

# lots of errors of the form 

W (3223) wifi:m f deauth

E (3223) wifi:esf_buf: t=3 l=28 max:32, alloc:32 no eb, TXQ_BLOCK=4000
W (3233) wifi:alloc eb len=28 type=3 fail, heap:226836

# ulitimately the IP address is not determined

