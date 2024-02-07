# BUILD OSC O-DU HIGH

![DU_Plugins](https://hackmd.io/_uploads/rJBM3iHdp.png)

## Prerequisites
1. libxml2
```
sudo yum install -y libxml2
```
2. pcap
```
sudo yum install -y libpcap-devel
```
3. GCC
```
sudo yum groups mark install -y “Development Tools”
```
4. lksctp
```
sudo yum install -y libxml2
```
## Clone O-DU L2 Repository
```
$ git clone https://github.com/o-ran-sc/o-du-l2
```
or 
```
git clone “https://gerrit.o-ran-sc.org/r/o-du/l2”
```

## Setup Environment
1. Copy necessary file/library from DPDK to O-DU lib
```
## Change to the directory of header files
$ cd <DPDK Directory>/dpdk-stable-20.11.3/lib/librte_eal/include

## Copy the header files
$ cp rte_branch_prediction.h rte_common.h rte_dev.h rte_log.h rte_pci_dev_feature_defs.h rte_bus.h rte_compat.h rte_debug.h rte_eal.h rte_per_lcore.h <O-DU High Directory>/l2/src/dpdk_lib
    
## Change to the directory which has rte_config.h
$ cd <DPDK Directory>/dpdk-stable-20.11.3/config
    
## Copy rte_config.h
$ cp rte_config.h <O-DU High Directory>/l2/src/dpdk_lib
    
## Change to the directory which has rte_os.h
$ cd <DPDK Directory>/dpdk-stable-20.11.3/lib/librte_eal/linux/include
    
## Copy rte_os.h
$ cp rte_os.h <O-DU High Directory>/l2/src/dpdk_lib

```

2. Integrate WLS shared library to DU L2 code
```
## Change directory to <O-DU High Directory>/l2/src
$ cd <O-DU High Directory>/l2/src

## Create folder called `wls_lib`
$ mkdir wls_lib

## Change directory to <O-DU Low Directory>/phy
$ cd <O-DU Low Directory>/phy
    
## Copy the content of wls_lib to O-DU High
$ cp -r wls_lib/.  <O-DU High Directory>/l2/src/wls_lib
```

3. Edit make file as your environment
```
$ cd $your_dir/o-du-l2/build/odu/
$ vim ./makefile
```

Adjust the script as your environment

```
L_OPTS+=-lnsl -lrt -lm -lpthread -lsctp 
ifeq ($(PHY), INTEL_L1)
        # -lhugetlbfs -lnuma -ldl -L/root/Intel-L1-20.11.1/dpdk-20.11.1/build/lib                        
        L_OPTS+=-L/home/jsaputro/ORAN/phy/wls_lib/ -lwls                         \
        -lhugetlbfs -lnuma -ldl -L/home/jsaputro/ORAN/dpdk-stable-20.11.3/build/lib                   \
        -lrte_gso -lrte_acl -lrte_hash -lrte_bbdev -lrte_ip_frag -lrte_bitratestats -lrte_ipsec        \
        -lrte_bpf -lrte_jobstats -lrte_telemetry -lrte_kni -lrte_kvargs -lrte_latencystats -lrte_port  \
        -lrte_lpm -lrte_power -lrte_mbuf -lrte_rawdev -lrte_member -lrte_cfgfile -lrte_mempool         \
                  -lrte_cmdline -lrte_rcu -lrte_compressdev -lrte_reorder -lrte_cryptodev -lrte_rib              \
                  -lrte_distributor -lrte_meter  -lrte_ring -lrte_eal -lrte_metrics -lrte_sched -lrte_efd        \
                  -lrte_net -lrte_security -lrte_ethdev -lrte_pci -lrte_stack -lrte_eventdev -lrte_pdump         \
                  -lrte_table -lrte_fib -lrte_pipeline -lrte_timer -lrte_flow_classify -lrte_vhost               \
        -lrte_gro
endif
```

## Build O-DU
How to Clean and Build:
--------------------------
1. Build commands:
   a. odu       - Builds all components of ODU
   b. cu_stub   - Builds all CU Stub
   c. ric_stub  - Builds all RIC_Stub
   d. clean_odu - clean up ODU
   e. clean_cu  - clean up CU Stub
   f. clean_ric - clean up RIC Stub
   g. clean_all - cleanup everything
   h. options:
      * MACHINE=BIT64/BIT32 - Specify underlying machine type. Default is BIT32
      * NODE=TEST_STUB      - Specify if it is a test node. Mandatory for cu_stub/ric_stub. Must not be used for odu
      * MODE=FDD/TDD        - Specify duplex mode. Default is FDD
      * PHY=INTEL_L1        - Specify type of phy. If not specified, PHY stub is used
      * PHY_MODE=TIMER      - Specify mode of phy. Used only if PHY=INTEL_L1. Default is radio mode
      * O1_ENABLE=YES       - Specify if O1 interface is enabled. If not specified, it is disabled 

2. Building ODU binary:
   a. Build folder
```
$ cd l2/build/odu
```
   b. Building ODU binary
```
$ make odu MACHINE=<refer section D.1.h>  MODE=<refer section D.1.h>
```
   c. Cleaning ODU binary
```
$ make clean_odu MACHINE=<refer section D.1.h> MODE=<refer section D.1.h>
```

3. Building CU Stub binary:
   a. Build folder
```
$ cd l2/build/odu
```
   b. Building CU Stub binary
```
$ make cu_stub NODE=<refer section D.1.h> MACHINE=<refer section D.1.h> MODE=<refer section D.1.h>
```
   c. Cleaning CU Stub binary
```
$ make clean_cu NODE=<refer section D.1.h> MACHINE=<refer section D.1.h> MODE=<refer section D.1.h>
```

4. Building RIC Stub binary:
   a. Build folder
```
$ cd l2/build/odu
```
   b. Building RIC Stub binary
```
$ make ric_stub NODE=<refer section D.1.h> MACHINE=<refer section D.1.h> MODE=<refer section D.1.h>
```
   c. Cleaning RIC Stub binary
```
$ make clean_ric NODE=<refer section D.1.h> MACHINE=<refer section D.1.h> MODE=<refer section D.1.h>
```

5. Cleaning ODU, CU Stub and RIC Stub:
```
$ make clean_all
```





## Container ODU
1. Build container image using docker or podman [in here uses podman as container platform]


2. Run ODU same as baremetal do

Captures:
1. ODU
![Screenshot from 2024-01-09 21-56-40](https://hackmd.io/_uploads/S1i01louT.png)


2. CU Stub, RIC Stub, fapi
![Screenshot from 2024-01-09 21-56-46](https://hackmd.io/_uploads/HyDWelid6.png)

3. Packet Capture
![Screenshot from 2024-01-09 21-58-10](https://hackmd.io/_uploads/HJdzlxsOT.png)
![Screenshot from 2024-01-09 21-56-50](https://hackmd.io/_uploads/rJ_GegsOa.png)
