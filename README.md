# Kernel Communication

> Problem: Write a kernel module that allows creating a device. Another kernel module should monitor  write operations on this device. And each
time there is a write operation on the device, the content is read and sent via a kernel socket to a third module, which prints it in "dmesg".

## Idea
- Break into 2 small problems, monitor for device and TCP network. Transmission Control Protocol is better in data integrity.
- When write operation occur, ran the monitor_callback from monitor module, then the monitor_callback will send to the socket module. The socket module will print to dmesg

## How to run
- First, make file with
```bash
sudo make
```
or 
```bash
sudo make V=1>makeout.txt
``` 
if you want to log to a file

- Then run these command in order:
```bash
sudo insmod socket.ko
```
```bash
sudo insmod monitor.ko
```
```bash
sudo insmod device.ko
```
Then to test, you can run
```bash
echo "Hello" | sudo tee /dev/virtualdevice
```
and check the dmesg
```bash
sudo dmesg
```
## Current issue
- Only a short message can be print in dmesg through the socket module (Fixed. This is due to unproper iovec and msghdr setup in server)
- You have to stop the socket module or use a seperate client module to stop the socket module 