# TEEP@2024 - Johan ED Saputro

![Protocol Stack](https://hackmd.io/_uploads/BJ5f31Rwp.png)


## OSC Installation

### OS Configuration
**Prerequisites**
1. Centos7 RTOS or RHEL RTOS
2. Intel Basekit
3. DPDK

### Centos 7 RTOS
* in RHEL
you could see guide from RedHat in below
[Redhat RTOS](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_for_real_time/9/html-single/installing_rhel_9_for_real_time/index#assembly_installing-rhel-for-real-time_installing-rhel-9-for-real-time)
* in centos 7

```
$ sudo tee /etc/yum.repos.d/CentOS-rt.repo >/dev/null <<EOF
# CentOS-rt.repo

[rt]
name=CentOS-7 - rt
baseurl=http://mirror.centos.org/centos/\$releasever/rt/\$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
EOF
```

```
$ sudo yum update -y
$ sudo yum install -y kernel-rt rt-tests tuned-profiles-realtime
```

### RHEL RTOS

After setup RTOS check your current machine using 
``` $ neofetch ```
or 
``` $ uname -a ```

they must output like below [displayed rt in kernel boot order]

![Screenshot from 2023-12-30 19-31-49](https://hackmd.io/_uploads/HyqBRFaP6.png)

### Intel Basekit
```
Download Intel Basekit from here
https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit.html
```

```
$ sudo sh ./l_BaseKit_p_${version}_offline.sh -a -c
```

#### export variables and shared library to ~/.bashrc file
```
#source /opt/intel/oneapi/setvars.sh
export PATH=$PATH:/opt/intel/oneapi/compiler/2024.0/bin/

export RTE_TARGET=x86_64-native-linuxapp-icx
export RTE_SDK=$HOME/dpdk-stable-20.11.3
export WIRELESS_SDK_TOOLCHAIN=icx
export SDK_BUILD=build-avx512-icc
export PATH=$PATH:/opt/intel/oneapi/compiler/2024.0/bin/compiler/
```
### Necessary Package for RHEL
```
$ sudo yum install python3-pip gcc gcc-c++ kernel-devel make linuxptp
```

### DPDK Installation
***Prerequisites***
```
$ yum install epel-release -y
$ yum install python36 -y
$ yum install -y python3-pip
$ sudo pip3 install meson
$ sudo yum -y install ninja-build
```


#### Download DPDK source code

