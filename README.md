# Linksys-MX4300
Open Source Wifi Router Firmware

This is all the instruction to get both DD-WRT and OpenWRT working even as a dual boot if so desired.

Follow the corresponding folder with the pdf include for the full instruction. Reverting back to stock firmware and img is also included in case something goes wrong. But all this has been test multiple time without issues. This is mostly for my future reference but others are free to use it as they so desire.

Anywhere pscp is suggested to upload a file, PUTTY can be install to transfer the file to the router through an ethernet cable. Just install PUTTY and then from cmd run
On Windows to Router:

"pscp C:\Users\ADMIN\Downloads\filetotransfer.x root@192.168.1.1:remote_path/file_name"

"pscp C:\Users\ADMIN\Downloads\filetotransfer.x root@192.168.1.1:remote_path/file_name"



<b>Dual firmware partition</b>

Supported by newer Linksys devices

Most newer devices (mostly those with decent amount of flash ROM) have 2 independent firmware partitions. A usage strategy could be, to install OpenWrt only into one of the 2 partitions and leave the vendor firmware in the other partition. No further tools are required to toggle between the two partitions.

Procedure, to manually toggle between the two firmware partitions:

- Switch device power off.
- 3x Switch device power on for 2 seconds, then off again.
- Switch device power on, the device should now boot to the alternative partition.



<b>Installation Summary</b>

When successfully booted into any of the two partitions, a triggered firmware update will flash the other, secondary partition. The partition that is currently booted, stays untouched.

1. Connect the router to the internet.
2. SSH into the router: ssh

Run opkg update && okpg install luci The router has 2 firmware partitions. The above instructions will upload the OpenWRT firmware to your current partition and you can use that without modifying the second partition. If you want to load the OpenWRT onto the second partition, the instructions are:

1. SSH into the router: ssh
2. Check booted partition, by running:
  
   <b>fw_printenv -n boot_part</b>
   
5. SCP the squashfs-factory.bin onto the router.
6. If that command returns a "1", then you can install OpenWRT onto the alternate partition by running:

   <b>mtd -r -e alt_kernel -n write openwrt-qualcommax-ipq807x-linksys_mx4300-squashfs-factory.bin alt_kernel</b>
   
7. If that command returns a "2", then you can install OpenWRT onto the primary partition by running:

   <b>mtd -r -e kernel -n write openwrt-qualcommax-ipq807x-linksys_mx4300-squashfs-factory.bin kernel</b>
   
If you mess up a partition, you can switch to the other one by powering off unit first. Then power cycling the devices 3 times with >2 seconds andd less than 5
seconds between each power cycle (ON & OFF 1 time is one cycle). Then wait 5 sec before powering unit back on.
