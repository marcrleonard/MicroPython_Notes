#####Log into the board

`screen /dev/tty.SLAB_USBtoUART 115200`

##### Flash board

`esptool.py --port <serial-port-of-ESP8266> write_flash -fm <flash-mode> 0x00000 <nodemcu-firmware>.bin`

I've accidentally 'bricked' the board before. To redo it, I had to first erase it, THEN flash it.

`esptool.py --port /dev/tty.SLAB_USBtoUART erase_flash`
`esptool.py --port /dev/tty.SLAB_USBtoUART write_flash -fm dout 0x00000 esp8266-20170823-v1.9.2.bin`

#####Transfer Files
`ampy` is the micropython tool from adafruit

List files on root of board:

`ampy --port /dev/tty.SLAB_USBtoUART ls`

When using `ampy run`, the local file you specify does NOT get copied to the board. It will run it as a script. So if it has any dependencies, they need to be copied to the board first. 


#####Driver Downloads
http://micropython.org/download#esp8266

#####Connect to WiFi


```python
def do_connect():
    import network
    sta_if = network.WLAN(network.STA_IF)
    if not sta_if.isconnected():
        print('connecting to network...')
        sta_if.active(True)
        sta_if.connect('ssid', 'password')
        while not sta_if.isconnected():
            pass
    print('network config:', sta_if.ifconfig())

```

#####Get time
```python
import utime
import ntptime
curr_time = utime.localtime(ntptime.time())
# This is the time represented in a tuple form. It is correct, but I think it's
# GMT. So it needs to be offset. Use SimpleTimeOffset!

```






