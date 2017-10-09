---
title: "N-sorozat aaaAzure illesztőprogram telepítése Linux |} Microsoft Docs"
description: "Hogyan tooset NVIDIA GPU illesztőprogram N sorozatú virtuális gépek Azure-ban futó Linux"
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7db1b3859f9075c6d9f0319f39418946ea08743f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>N-sorozat linuxos virtuális gépek NVIDIA GPU illesztőprogramok telepítéséhez

Azure N-sorozat hello GPU képességek előnyeit tootake futó Linux virtuális gépeket, a támogatott NVIDIA video-illesztőprogramok telepítését. Ez a cikk lépéseit illesztőprogram beállítása az N-sorozatú virtuális gép telepítése után. Telepítési információk illesztőprogram érhető el is [Windows virtuális gépek](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


N sorozatú virtuális gép specifikációk, tárolási kapacitás, és a lemez adatai: [GPU Linux Virtuálisgép-méretek](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a>Portok HV virtuális gépek rács illesztőprogramok telepítése

tooinstall NVIDIA rács illesztőprogramok Permanens virtuális gépeken, egy SSH-kapcsolat tooeach VM ellenőrizze, és a Linux-disztribúció hello utasításai. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Futtassa a hello `lspci` parancsot. Ellenőrizze, hogy hello NVIDIA M60 kártya vagy kártyák PCI eszközként látható.

2. Telepítse a frissítéseket.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Tiltsa le a hello Nouveau kernel-illesztőprogram, amely nem kompatibilis a hello NVIDIA illesztőprogram. (Csak használata hello NVIDIA illesztőprogram Permanens virtuális gépeken) toodo, hozzon létre egy fájlt a `/etc/modprobe.d `nevű `nouveau.conf` a tartalom a következő hello:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. Hello virtuális gép újraindul, és csatlakozzon újra. Kilépés X kiszolgáló:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Töltse le és telepítse hello rács illesztőprogramját:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. Amikor a rendszer megkérdezi, hogy kívánja-e toorun hello nvidia-xconfig segédprogram tooupdate az X-konfigurációs fájlt, jelölje ki **Igen**.

7. A telepítést követően /etc/nvidia/gridd.conf.template tooa új fájl gridd.conf a hely etc/nvidia/másolása

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Adja hozzá a hello túl a következő`/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>7.3 centOS-alapú vagy a Red Hat Enterprise Linux 7.3.


1. Frissítés hello kernel és DKMS.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. Tiltsa le a hello Nouveau kernel-illesztőprogram, amely nem kompatibilis a hello NVIDIA illesztőprogram. (Csak használata hello NVIDIA illesztőprogram Permanens virtuális gépeken) toodo, hozzon létre egy fájlt a `/etc/modprobe.d `nevű `nouveau.conf` a tartalom a következő hello:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. Indítsa újra a virtuális gép hello, csatlakozzon újra, és telepítse a Hyper-v legújabb Linux integrációs szolgáltatások hello
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. Csatlakozzon újra a virtuális gép toohello, és futtassa a hello `lspci` parancsot. Ellenőrizze, hogy hello NVIDIA M60 kártya vagy kártyák PCI eszközként látható.
 
5. Töltse le és telepítse hello rács illesztőprogramját:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. Amikor a rendszer megkérdezi, hogy kívánja-e toorun hello nvidia-xconfig segédprogram tooupdate az X-konfigurációs fájlt, jelölje ki **Igen**.

7. A telepítést követően /etc/nvidia/gridd.conf.template tooa új fájl gridd.conf a hely etc/nvidia/másolása
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Adja hozzá a hello túl a következő`/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.

### <a name="verify-driver-installation"></a>Illesztőprogram telepítésének ellenőrzése


tooquery hello GPU az eszköz állapotát, SSH toohello VM és futtatási hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram hello illesztőprogrammal telepítve. 

Kimeneti hasonló toohello következő jelenik meg:

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 kiszolgáló
Ha egy X11 van szüksége a távoli kapcsolatok tooan portok HV VM server [x11vnc](http://www.karlrunge.com/x11vnc/) használata ajánlott, mivel lehetővé teszi a hardveres gyorsítás képek. hello hello M60 eszköz BusID kell kézzel felvenni toohello xconfig fájl (`etc/X11/xorg.conf` Ubuntu 16.04 LTS, a `/etc/X11/XF86config` CentOS 7.3 vagy a Red Hat Enterprise Server 7.3). Adja hozzá a `"Device"` hasonló toohello következő szakaszt:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Emellett frissítse a `"Screen"` szakasz toouse ezt az eszközt.
 
hello BusID található futtatásával

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
hello BusID módosíthatja, ha egy virtuális gép lekérdezi teljesítményigény vagy újraindítása után. Ezért érdemes lehet egy parancsfájl tooupdate hello BusID hello X11 konfigurációs toouse amikor egy virtuális gép újraindítása után. Példa:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed too${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Hozzon létre egy bejegyzést, az ezt a fájlt is elindítható a rendszerindító legfelső szintű `/etc/rc.d/rc3.d`.


## <a name="install-cuda-drivers-for-nc-vms"></a>Hálózati vezérlő által virtuális gépek CUDA illesztőprogramok telepítése

Az alábbiakban lépéseket tooinstall NVIDIA illesztőprogramok Linux NC virtuális gépeken a hello NVIDIA CUDA eszközkészlet 8.0. 

C és C++ fejlesztők is telepíthet hello teljes eszközkészlet toobuild GPU-az elérését gyorsítja fel alkalmazásokat. További információkért lásd: hello [CUDA a telepítési útmutató](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> CUDA illesztőprogram letöltési hivatkozásokat itt megadott aktuális kiadvány időpontban. Hello legújabb CUDA illesztőprogramokat, a Microsoft hello [NVIDIA](http://www.nvidia.com/) webhelyet.
>

tooinstall CUDA eszközkészlet, egy SSH-kapcsolat tooeach VM ellenőrizze. amely a rendszer hello tooverify egy CUDA-kompatibilis grafikus processzort tartalmaz, futtassa a következő parancs hello rendelkezik:

```bash
lspci | grep -i NVIDIA
```
A következő példa (megjelenítő egy NVIDIA Tesla K80-kártyán) kimeneti hasonló toohello jelenik meg:

![lspci parancs kimenete](./media/n-series-driver-setup/lspci.png)

Majd a terjesztéshez futtatási telepítési parancsokat.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Töltse le és telepít hello CUDA illesztőprogramokat.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  hello telepítés több percet is igénybe vehet.

2. toooptionally telepítés hello teljes CUDA eszközkészlet, típus:

  ```bash
  sudo apt-get install cuda
  ```

3. Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>7.3 centOS-alapú vagy a Red Hat Enterprise Linux 7.3.

1. Frissítések letöltése. 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. Kapcsolódjon újra toohello virtuális gép, és telepítse a Hyper-V legújabb Linux integrációs szolgáltatások hello.

  > [!IMPORTANT]
  > Ha telepítette a CentOS-alapú HPC-lemezkép egy NC24r VM, hagyja ki a tooStep 3. Azure RDMA-illesztőprogramjai és a Linux integrációs szolgáltatások előre telepített hello lemezképet, mert a LIS nem kell frissíteni, és a kernel frissítések alapértelmezés szerint le vannak tiltva.
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. Kapcsolódjon újra toohello virtuális gép, és a telepítés folytatásához a következő parancsok hello:

  ```bash
  sudo yum install kernel-devel

  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-8.0.61-1.x86_64.rpm

  wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

  sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo yum install cuda-drivers
  ```

  hello telepítés több percet is igénybe vehet. 

4. toooptionally telepítés hello teljes CUDA eszközkészlet, típus:

  ```bash
  sudo yum install cuda
  ```

5. Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.


### <a name="verify-driver-installation"></a>Illesztőprogram telepítésének ellenőrzése


tooquery hello GPU az eszköz állapotát, SSH toohello VM és futtatási hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram hello illesztőprogrammal telepítve. 

Kimeneti hasonló toohello következő jelenik meg:

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a>Illesztőprogram-frissítést CUDA

Azt javasoljuk, hogy rendszeres időközönként frissíti CUDA illesztőprogramok a telepítés utáni.

#### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>7.3 centOS-alapú vagy a Red Hat Enterprise Linux 7.3.

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a>Hibaelhárítás

* Egy ismert probléma az Azure virtuális gépeken N-sorozat hello 4.4.0-75 Linux kernel futó Ubuntu 16.04 LTS CUDA illesztőprogramok van. Ha egy korábbi kernel frissít, frissítse az tooat legalább kernel verzió 4.4.0-77. 



## <a name="next-steps"></a>Következő lépések

* A hello N sorozatú virtuális gépek hello NVIDIA Feldolgozóegységekkel kapcsolatos további információkért lásd:
    * [NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (az Azure NC virtuális gépek)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (az Azure Permanens virtuális gépek)

* a telepített NVIDIA illesztőprogramokat, a Linux virtuális gép rendszerképének toocapture lásd [hogyan toogeneralize és a Linux virtuális gép rögzítése](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
