# Cài đặt KVM trên CentOS 7

## Mục lục

## 1. Chuẩn bị môi trường CentOS7

Một máy chạy CentOS 7 hỗ trợ môi trường ảo hóa. Thông số:

- **CPU:** 2 nhân, 2 luồng.

- **RAM:** 2GB.

- **Disk:** 50GB.

- Nếu cài đặt trên VMware, ta có thể bật hỗ trợ ảo hóa trong **Virtual Machine Settings**

<img src="https://imgur.com/WlZ2vG2.png">

Kiểm tra xem máy chủ CentOS có hỗ trợ ảo hóa không:

```
[root@localhost ~]# egrep -c "svm|vmx" /proc/cpuinfo
4
```

Nếu giá trị trả về là `0` thì sẽ không hỗ trợ ảo hóa.

## 2. Cài đặt KVM

Cài đặt các gói cần thiết

```
[root@localhost ~]# yum -y install qemu-kvm libvirt virt-install bridge-utils virt-manager
```

Trong đó:

- **qemu-kvm:** Phần phụ trợ cho KVM.

- **libvirt:** cung cấp libvirt mà bạn cần quản lý qemu và KVM bằng libvirt.

- **bridge-utils:** chứa một tiện ích cần thiết để tạo và quản lý các thiết bị bridge.

- **virt-manager:** cung cấp giao diện đồ họa để quản lý máy ảo.

- **virt-install:** Cung cấp lệnh để cài đặt máy ảo.

Sau khi cài đặt xong, kiểm tra lại các module KVM:

```
[root@localhost ~]# lsmod | grep kvm
kvm_intel             188688  0
kvm                   636969  1 kvm_intel
irqbypass              13503  1 kvm
```

Bật libvirt và kích hoạt khởi động cùng hệ thống

```
[root@localhost ~]# systemctl start libvirtd
[root@localhost ~]# systemctl enable libvirtd
```

Để sử dụng đồ họa cho virt-manager, ta cài đặt thêm gói X-window:

```
[root@localhost ~]# yum install "@X Window System" xorg-x11-xauth xorg-x11-fonts-* xorg-x11-utils -y
```

Sau khi cài đặt xong, ta `reboot` lại máy và kích hoạt Virtual Manager bằng lệnh `[root@localhost ~]# virt-manager`.

Để truy cập vào `virtual-manager` ta sử dụng Ứng dụng **Moba Xterm**, sau đó SSH vào máy chủ CentOS vừa cài đặt và sử dụng lệnh `virt-manager`, cửa sổ của Virtual Manager sẽ hiện lên:

<img src="https://imgur.com/pP9oovU.png">

## 3. Cài đặt VM trên KVM

Tạo thư mục để lưu trữ `file iso` cài đặt hệ điều hành. Chúng ta có thể `download`, `copy` các file iso để thuận tiện trong việc cài đặt VM. 

```
[root@localhost ~]# cd /var/lib/libvirt
[root@localhost libvirt]# mkdir file-iso
```

### 3.1. Tạo 1 VM trên KVM bằng virt-manager

#### Bước 1: File -> New Virtual Machine

<img src="https://imgur.com/pDZYZ5R.png">

#### Bước 2: Chọn Local install Media để cài đặt bằng file iso đã lưu trong máy chủ

<img src="https://imgur.com/IN3kMNi.png">

#### Bước 3: Chọn Use Iso image -> Browse để tiến hành chọn file iso

<img src="https://imgur.com/UCMXJPI.png">

#### Bước 4: Chọn Browse Local để tìm đến thư mục lưu file iso

<img src="https://imgur.com/mIyqmWa.png">

#### Bước 5: Filesytem root -> /var/lib/libvirt/file-iso để đến thư mục lưu file iso

<img src="https://imgur.com/cdm02Cw.png">

#### Bước 6: Chọn Iso thành công, chọn forward

<img src="https://imgur.com/UdxXlVV.png">

#### Bước 7: Lựa chọn dung lượng RAM, CPU, Disk

<img src="https://imgur.com/RjhMoxR.png">

<img src="https://imgur.com/EWtnaHT.png">

#### Bước 8: Review lại các thông số cấu hình và lựa chọn dải mạng

<img src="https://imgur.com/cosC7ET.png">

#### Bước 9: Tiến hành cài đặt hệ điều hành

<img src="https://imgur.com/rPRrgYk.png">