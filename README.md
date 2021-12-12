# Ubuntu-22.04-Jammy-Jellyfish-Nexus-7-2012-grouper-rev.-E1565-kernel-5.15.0-rc4
Linux on Nexus 7 2012 grouper rev.E1565

Link tải trên Gdrive:



Ubuntu 22.04 Jammy Jellyfish ext4 kernel-5.15.0-rc4-next-20211011-postmarketos-grate

https://drive.google.com/drive/u/1/folders/1cIJaryNta43DQVxzOQu3RDl4IJNVQ9i0



Các bước chuẩn bị tạo .img cơ bản như sau:



1. Mở img bằng lệnh sau để mount và chỉnh sửa trong quyền sudo su

# sudo modprobe loop

# sudo losetup -f

# sudo losetup /dev/loop0 /home/[username]/jammy-preinstalled-server-armhf+raspi.img

# sudo partprobe /dev/loop0

# sudo gparted /dev/loop0

2. Format /boot thành ext2 và đặt label là pmOS_boot, chọn manage flag là boot. Đặt label cho rootfs là pmOS_root

3. Extract kernel-5.15.0-rc4-next-20211011-postmarketos-grate.zip

4. Copy boot.img, vmlinuz, initramfs, extend đến phân vùng /pmOS_boot

5. Copy /lib/module và /lib/firmware vào đúng /lib trên phân vùng /pmOS_root

6. Sửa /etc/fstab vì kernel 5.15.0-rc4 tìm LABEL pmOS_boot và pmOS_root để khởi động trong Ubuntu

7. Copy thư mục dtb vào /user/share. Copy libturbojpeg.so.0.2 vào /lib/armhfgnueabi. Copy asound.conf, sysctl.conf, modules, deviceinfo, pointercal vào /etc. Copy asound.state /var/lib/alsa, /xorg vào /usr/lib, /xorg.conf.d vào /usr/share/X11, sửa cloud.cfg trong /etc/cloud

8. cpufreq.start, temp_throttle, clear_ram copy vào /opt để chỉnh lại virtual memory nếu cần.

9. Dùng losetup, partprobe, gparted, truncate để resize và cut vùng unallocated trong file .img cho vừa kích thước userdata trên Nexus7 8GB(có thể dùng với 16GB và 32GB)



default user was kim, default passwd was possible



default user was ubuntu, default passwd was ubuntu



Đã thử và thành công boot được Ubuntu 22.04  Jammy Jellyfish lên Nexus 7 2012, tạo local user/passwd theo bài viết này cho cloud-init như 1 phần bổ sung khi bị cloud-init: used fallback datasource thì module cuối trong cloud.cfg không chạy để khởi tạo user/passwd ubuntu:ubuntu

https://stackoverflow.com/questions/61591885/how-do-i-set-a-custom-password-with-cloud-init-on-ubuntu-20-04



Đây là bản preinstalled server nên không có GUI và cần có micro usb-otg keyboard để nhập lệnh



Cách kiểm tra Nexus 7 2012 là mã cũ PM269 hay E1565. Tham khảo:

https://wiki.postmarketos.org/wiki/Google_Nexus_7_2012_(asus-grouper)



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM



Do I have grouper or tilapia?



TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia



Which hardware revision of grouper do I have?





TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Install Alpine Linux on Virtualbox:

[MEDIA=youtube]1_bsycXrFcI[/MEDIA]



Đặt máy về bootloader, để flash boot.img qua fastboot hoặc dùng method của postmarketOS. Kết nối Nexus 7 vào máy tính qua cáp usb chuẩn 1.0 → 2.0



$ sudo adb start-server



$ sudo adb reboot bootloader



$ sudo fastboot flash boot <boot_filename>.img



Vào TWRP for grouper 3.3.1-0 trở lên https://dl.twrp.me/grouper/

Dùng adb shell trên PC/laptop hoặc Advance/Terminal trong twrp để umount mmcblk0p9 (làm 2 lần cho chắc ăn vì android mount /data và /sdcard trên cùng mmcblk0p9) [với tilapia (bản 3G) là mmcblk0p10]



1. TWRP(Advance → Terminal): $ df

2. TWRP(Advance → Terminal): $ umount /dev/block/mmcblk0p__  <- fill partition number

3. TWRP(Advance → Terminal): $ umount /dev/block/mmcblk0p__  <- fill partition number



On PC/Laptop terminal:



$ adb push <rootfs_filename>.img /dev/block/mmcblk0p__  <- fill partition number



grouper has likely data on /dev/block/mmcblk0p9 but make sure!
tilapia has likely data on /dev/block/mmcblk0p10 but make sure!



remove cloud-init, snapd, needrestart service

https://askubuntu.com/questions/1346874/failed-to-check-for-processor-microcode-upgrades-at-the-end-of-apt-get-how-ca



# sudo apt --purge remove  needrestart



Rotate fbcon



# sudo su



# echo 1 | sudo tee /sys/class/graphics/fbcon/rotate



# echo 1 | sudo tee /sys/class/graphics/fbcon/rotate_all



# chown -hvR ubuntu /home/ubuntu



Install xubuntu-core



# sudo apt install xubuntu-core^



# sudo apt install bluez blueman htop neofetch iio-sensor-proxy perl x11-xserver-utils network-manager onboard



Rotate screen

https://gitlab.com/gullradriel/asus-grouper-nexus-7-sensor-daemon



Các phần mềm hỗ trợ để trong /opt



Image source là Raspberry Pi Generic (Hard-Float) preinstalled server image:

http://cdimages.ubuntu.com/ubuntu-server/daily-preinstalled/current/jammy-preinstalled-server-armhf+raspi.img.xz



Xem thêm hướng dẫn trong bài post này:

https://tinhte.vn/thread/ubuntu-21-04-1-hirsute-hippo-pre-image-cho-nexus-7-2012-wifi-rev-e1565-kernel-5-14-rc3-next-grate.3392201/
