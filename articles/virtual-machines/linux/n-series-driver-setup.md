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
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a><span data-ttu-id="d9531-103">N-sorozat linuxos virtuális gépek NVIDIA GPU illesztőprogramok telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="d9531-103">Install NVIDIA GPU drivers on N-series VMs running Linux</span></span>

<span data-ttu-id="d9531-104">Azure N-sorozat hello GPU képességek előnyeit tootake futó Linux virtuális gépeket, a támogatott NVIDIA video-illesztőprogramok telepítését.</span><span class="sxs-lookup"><span data-stu-id="d9531-104">tootake advantage of hello GPU capabilities of Azure N-series VMs running Linux, install supported NVIDIA graphics drivers.</span></span> <span data-ttu-id="d9531-105">Ez a cikk lépéseit illesztőprogram beállítása az N-sorozatú virtuális gép telepítése után.</span><span class="sxs-lookup"><span data-stu-id="d9531-105">This article provides driver setup steps after you deploy an N-series VM.</span></span> <span data-ttu-id="d9531-106">Telepítési információk illesztőprogram érhető el is [Windows virtuális gépek](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d9531-106">Driver setup information is also available for [Windows VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


<span data-ttu-id="d9531-107">N sorozatú virtuális gép specifikációk, tárolási kapacitás, és a lemez adatai: [GPU Linux Virtuálisgép-méretek](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d9531-107">For N-series VM specs, storage capacities, and disk details, see [GPU Linux VM sizes](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a><span data-ttu-id="d9531-108">Portok HV virtuális gépek rács illesztőprogramok telepítése</span><span class="sxs-lookup"><span data-stu-id="d9531-108">Install GRID drivers for NV VMs</span></span>

<span data-ttu-id="d9531-109">tooinstall NVIDIA rács illesztőprogramok Permanens virtuális gépeken, egy SSH-kapcsolat tooeach VM ellenőrizze, és a Linux-disztribúció hello utasításai.</span><span class="sxs-lookup"><span data-stu-id="d9531-109">tooinstall NVIDIA GRID drivers on NV VMs, make an SSH connection tooeach VM and follow hello steps for your Linux distribution.</span></span> 

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="d9531-110">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="d9531-110">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="d9531-111">Futtassa a hello `lspci` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9531-111">Run hello `lspci` command.</span></span> <span data-ttu-id="d9531-112">Ellenőrizze, hogy hello NVIDIA M60 kártya vagy kártyák PCI eszközként látható.</span><span class="sxs-lookup"><span data-stu-id="d9531-112">Verify that hello NVIDIA M60 card or cards are visible as PCI devices.</span></span>

2. <span data-ttu-id="d9531-113">Telepítse a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="d9531-113">Install updates.</span></span>

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. <span data-ttu-id="d9531-114">Tiltsa le a hello Nouveau kernel-illesztőprogram, amely nem kompatibilis a hello NVIDIA illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="d9531-114">Disable hello Nouveau kernel driver, which is incompatible with hello NVIDIA driver.</span></span> <span data-ttu-id="d9531-115">(Csak használata hello NVIDIA illesztőprogram Permanens virtuális gépeken) toodo, hozzon létre egy fájlt a `/etc/modprobe.d `nevű `nouveau.conf` a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="d9531-115">(Only use hello NVIDIA driver on NV VMs.) toodo this, create a file in `/etc/modprobe.d `named `nouveau.conf` with hello following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. <span data-ttu-id="d9531-116">Hello virtuális gép újraindul, és csatlakozzon újra.</span><span class="sxs-lookup"><span data-stu-id="d9531-116">Reboot hello VM and reconnect.</span></span> <span data-ttu-id="d9531-117">Kilépés X kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="d9531-117">Exit X server:</span></span>

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. <span data-ttu-id="d9531-118">Töltse le és telepítse hello rács illesztőprogramját:</span><span class="sxs-lookup"><span data-stu-id="d9531-118">Download and install hello GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 

6. <span data-ttu-id="d9531-119">Amikor a rendszer megkérdezi, hogy kívánja-e toorun hello nvidia-xconfig segédprogram tooupdate az X-konfigurációs fájlt, jelölje ki **Igen**.</span><span class="sxs-lookup"><span data-stu-id="d9531-119">When you're asked whether you want toorun hello nvidia-xconfig utility tooupdate your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="d9531-120">A telepítést követően /etc/nvidia/gridd.conf.template tooa új fájl gridd.conf a hely etc/nvidia/másolása</span><span class="sxs-lookup"><span data-stu-id="d9531-120">After installation completes, copy /etc/nvidia/gridd.conf.template tooa new file gridd.conf at location /etc/nvidia/</span></span>

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. <span data-ttu-id="d9531-121">Adja hozzá a hello túl a következő`/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="d9531-121">Add hello following too`/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="d9531-122">Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="d9531-122">Reboot hello VM and proceed tooverify hello installation.</span></span>


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="d9531-123">7.3 centOS-alapú vagy a Red Hat Enterprise Linux 7.3.</span><span class="sxs-lookup"><span data-stu-id="d9531-123">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>


1. <span data-ttu-id="d9531-124">Frissítés hello kernel és DKMS.</span><span class="sxs-lookup"><span data-stu-id="d9531-124">Update hello kernel and DKMS.</span></span>
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. <span data-ttu-id="d9531-125">Tiltsa le a hello Nouveau kernel-illesztőprogram, amely nem kompatibilis a hello NVIDIA illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="d9531-125">Disable hello Nouveau kernel driver, which is incompatible with hello NVIDIA driver.</span></span> <span data-ttu-id="d9531-126">(Csak használata hello NVIDIA illesztőprogram Permanens virtuális gépeken) toodo, hozzon létre egy fájlt a `/etc/modprobe.d `nevű `nouveau.conf` a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="d9531-126">(Only use hello NVIDIA driver on NV VMs.) toodo this, create a file in `/etc/modprobe.d `named `nouveau.conf` with hello following contents:</span></span>

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. <span data-ttu-id="d9531-127">Indítsa újra a virtuális gép hello, csatlakozzon újra, és telepítse a Hyper-v legújabb Linux integrációs szolgáltatások hello</span><span class="sxs-lookup"><span data-stu-id="d9531-127">Reboot hello VM, reconnect, and install hello latest Linux Integration Services for Hyper-V:</span></span>
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. <span data-ttu-id="d9531-128">Csatlakozzon újra a virtuális gép toohello, és futtassa a hello `lspci` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d9531-128">Reconnect toohello VM and run hello `lspci` command.</span></span> <span data-ttu-id="d9531-129">Ellenőrizze, hogy hello NVIDIA M60 kártya vagy kártyák PCI eszközként látható.</span><span class="sxs-lookup"><span data-stu-id="d9531-129">Verify that hello NVIDIA M60 card or cards are visible as PCI devices.</span></span>
 
5. <span data-ttu-id="d9531-130">Töltse le és telepítse hello rács illesztőprogramját:</span><span class="sxs-lookup"><span data-stu-id="d9531-130">Download and install hello GRID driver:</span></span>

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
  ``` 
6. <span data-ttu-id="d9531-131">Amikor a rendszer megkérdezi, hogy kívánja-e toorun hello nvidia-xconfig segédprogram tooupdate az X-konfigurációs fájlt, jelölje ki **Igen**.</span><span class="sxs-lookup"><span data-stu-id="d9531-131">When you're asked whether you want toorun hello nvidia-xconfig utility tooupdate your X configuration file, select **Yes**.</span></span>

7. <span data-ttu-id="d9531-132">A telepítést követően /etc/nvidia/gridd.conf.template tooa új fájl gridd.conf a hely etc/nvidia/másolása</span><span class="sxs-lookup"><span data-stu-id="d9531-132">After installation completes, copy /etc/nvidia/gridd.conf.template tooa new file gridd.conf at location /etc/nvidia/</span></span>
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. <span data-ttu-id="d9531-133">Adja hozzá a hello túl a következő`/etc/nvidia/gridd.conf`:</span><span class="sxs-lookup"><span data-stu-id="d9531-133">Add hello following too`/etc/nvidia/gridd.conf`:</span></span>
 
  ```
  IgnoreSP=TRUE
  ```
9. <span data-ttu-id="d9531-134">Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="d9531-134">Reboot hello VM and proceed tooverify hello installation.</span></span>

### <a name="verify-driver-installation"></a><span data-ttu-id="d9531-135">Illesztőprogram telepítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d9531-135">Verify driver installation</span></span>


<span data-ttu-id="d9531-136">tooquery hello GPU az eszköz állapotát, SSH toohello VM és futtatási hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram hello illesztőprogrammal telepítve.</span><span class="sxs-lookup"><span data-stu-id="d9531-136">tooquery hello GPU device state, SSH toohello VM and run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span> 

<span data-ttu-id="d9531-137">Kimeneti hasonló toohello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d9531-137">Output similar toohello following appears:</span></span>

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a><span data-ttu-id="d9531-139">X11 kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d9531-139">X11 server</span></span>
<span data-ttu-id="d9531-140">Ha egy X11 van szüksége a távoli kapcsolatok tooan portok HV VM server [x11vnc](http://www.karlrunge.com/x11vnc/) használata ajánlott, mivel lehetővé teszi a hardveres gyorsítás képek.</span><span class="sxs-lookup"><span data-stu-id="d9531-140">If you need an X11 server for remote connections tooan NV VM, [x11vnc](http://www.karlrunge.com/x11vnc/) is recommended because it allows hardware acceleration of graphics.</span></span> <span data-ttu-id="d9531-141">hello hello M60 eszköz BusID kell kézzel felvenni toohello xconfig fájl (`etc/X11/xorg.conf` Ubuntu 16.04 LTS, a `/etc/X11/XF86config` CentOS 7.3 vagy a Red Hat Enterprise Server 7.3).</span><span class="sxs-lookup"><span data-stu-id="d9531-141">hello BusID of hello M60 device must be manually added toohello xconfig file (`etc/X11/xorg.conf` on Ubuntu 16.04 LTS, `/etc/X11/XF86config` on CentOS 7.3 or Red Hat Enterprise Server 7.3).</span></span> <span data-ttu-id="d9531-142">Adja hozzá a `"Device"` hasonló toohello következő szakaszt:</span><span class="sxs-lookup"><span data-stu-id="d9531-142">Add a `"Device"` section similar toohello following:</span></span>
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
<span data-ttu-id="d9531-143">Emellett frissítse a `"Screen"` szakasz toouse ezt az eszközt.</span><span class="sxs-lookup"><span data-stu-id="d9531-143">Additionally, update your `"Screen"` section toouse this device.</span></span>
 
<span data-ttu-id="d9531-144">hello BusID található futtatásával</span><span class="sxs-lookup"><span data-stu-id="d9531-144">hello BusID can be found by running</span></span>

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
<span data-ttu-id="d9531-145">hello BusID módosíthatja, ha egy virtuális gép lekérdezi teljesítményigény vagy újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="d9531-145">hello BusID can change when a VM gets reallocated or rebooted.</span></span> <span data-ttu-id="d9531-146">Ezért érdemes lehet egy parancsfájl tooupdate hello BusID hello X11 konfigurációs toouse amikor egy virtuális gép újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="d9531-146">Therefore, you may want toouse a script tooupdate hello BusID in hello X11 configuration when a VM is rebooted.</span></span> <span data-ttu-id="d9531-147">Példa:</span><span class="sxs-lookup"><span data-stu-id="d9531-147">For example:</span></span>

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed too${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

<span data-ttu-id="d9531-148">Hozzon létre egy bejegyzést, az ezt a fájlt is elindítható a rendszerindító legfelső szintű `/etc/rc.d/rc3.d`.</span><span class="sxs-lookup"><span data-stu-id="d9531-148">This file can be invoked as root on boot by creating an entry for it in `/etc/rc.d/rc3.d`.</span></span>


## <a name="install-cuda-drivers-for-nc-vms"></a><span data-ttu-id="d9531-149">Hálózati vezérlő által virtuális gépek CUDA illesztőprogramok telepítése</span><span class="sxs-lookup"><span data-stu-id="d9531-149">Install CUDA drivers for NC VMs</span></span>

<span data-ttu-id="d9531-150">Az alábbiakban lépéseket tooinstall NVIDIA illesztőprogramok Linux NC virtuális gépeken a hello NVIDIA CUDA eszközkészlet 8.0.</span><span class="sxs-lookup"><span data-stu-id="d9531-150">Here are steps tooinstall NVIDIA drivers on Linux NC VMs from hello NVIDIA CUDA Toolkit 8.0.</span></span> 

<span data-ttu-id="d9531-151">C és C++ fejlesztők is telepíthet hello teljes eszközkészlet toobuild GPU-az elérését gyorsítja fel alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="d9531-151">C and C++ developers can optionally install hello full Toolkit toobuild GPU-accelerated applications.</span></span> <span data-ttu-id="d9531-152">További információkért lásd: hello [CUDA a telepítési útmutató](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span><span class="sxs-lookup"><span data-stu-id="d9531-152">For more information, see hello [CUDA Installation Guide](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).</span></span>


> [!NOTE]
> <span data-ttu-id="d9531-153">CUDA illesztőprogram letöltési hivatkozásokat itt megadott aktuális kiadvány időpontban.</span><span class="sxs-lookup"><span data-stu-id="d9531-153">CUDA driver download links provided here are current at time of publication.</span></span> <span data-ttu-id="d9531-154">Hello legújabb CUDA illesztőprogramokat, a Microsoft hello [NVIDIA](http://www.nvidia.com/) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="d9531-154">For hello latest CUDA drivers, visit hello [NVIDIA](http://www.nvidia.com/) website.</span></span>
>

<span data-ttu-id="d9531-155">tooinstall CUDA eszközkészlet, egy SSH-kapcsolat tooeach VM ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="d9531-155">tooinstall CUDA Toolkit, make an SSH connection tooeach VM.</span></span> <span data-ttu-id="d9531-156">amely a rendszer hello tooverify egy CUDA-kompatibilis grafikus processzort tartalmaz, futtassa a következő parancs hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="d9531-156">tooverify that hello system has a CUDA-capable GPU, run hello following command:</span></span>

```bash
lspci | grep -i NVIDIA
```
<span data-ttu-id="d9531-157">A következő példa (megjelenítő egy NVIDIA Tesla K80-kártyán) kimeneti hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d9531-157">You will see output similar toohello following example (showing an NVIDIA Tesla K80 card):</span></span>

![lspci parancs kimenete](./media/n-series-driver-setup/lspci.png)

<span data-ttu-id="d9531-159">Majd a terjesztéshez futtatási telepítési parancsokat.</span><span class="sxs-lookup"><span data-stu-id="d9531-159">Then run installation commands specific for your distribution.</span></span>

### <a name="ubuntu-1604-lts"></a><span data-ttu-id="d9531-160">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="d9531-160">Ubuntu 16.04 LTS</span></span>

1. <span data-ttu-id="d9531-161">Töltse le és telepít hello CUDA illesztőprogramokat.</span><span class="sxs-lookup"><span data-stu-id="d9531-161">Download and install hello CUDA drivers.</span></span>
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  <span data-ttu-id="d9531-162">hello telepítés több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d9531-162">hello installation can take several minutes.</span></span>

2. <span data-ttu-id="d9531-163">toooptionally telepítés hello teljes CUDA eszközkészlet, típus:</span><span class="sxs-lookup"><span data-stu-id="d9531-163">toooptionally install hello complete CUDA toolkit, type:</span></span>

  ```bash
  sudo apt-get install cuda
  ```

3. <span data-ttu-id="d9531-164">Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="d9531-164">Reboot hello VM and proceed tooverify hello installation.</span></span>

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="d9531-165">7.3 centOS-alapú vagy a Red Hat Enterprise Linux 7.3.</span><span class="sxs-lookup"><span data-stu-id="d9531-165">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

1. <span data-ttu-id="d9531-166">Frissítések letöltése.</span><span class="sxs-lookup"><span data-stu-id="d9531-166">Get updates.</span></span> 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. <span data-ttu-id="d9531-167">Kapcsolódjon újra toohello virtuális gép, és telepítse a Hyper-V legújabb Linux integrációs szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="d9531-167">Reconnect toohello VM and install hello latest Linux Integration Services for Hyper-V.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d9531-168">Ha telepítette a CentOS-alapú HPC-lemezkép egy NC24r VM, hagyja ki a tooStep 3.</span><span class="sxs-lookup"><span data-stu-id="d9531-168">If you installed a CentOS-based HPC image on an NC24r VM, skip tooStep 3.</span></span> <span data-ttu-id="d9531-169">Azure RDMA-illesztőprogramjai és a Linux integrációs szolgáltatások előre telepített hello lemezképet, mert a LIS nem kell frissíteni, és a kernel frissítések alapértelmezés szerint le vannak tiltva.</span><span class="sxs-lookup"><span data-stu-id="d9531-169">Because Azure RDMA drivers and Linux Integration Services are pre-installed in hello image, LIS should not be upgraded, and kernel updates are disabled by default.</span></span>
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. <span data-ttu-id="d9531-170">Kapcsolódjon újra toohello virtuális gép, és a telepítés folytatásához a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="d9531-170">Reconnect toohello VM and continue installation with hello following commands:</span></span>

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

  <span data-ttu-id="d9531-171">hello telepítés több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d9531-171">hello installation can take several minutes.</span></span> 

4. <span data-ttu-id="d9531-172">toooptionally telepítés hello teljes CUDA eszközkészlet, típus:</span><span class="sxs-lookup"><span data-stu-id="d9531-172">toooptionally install hello complete CUDA toolkit, type:</span></span>

  ```bash
  sudo yum install cuda
  ```

5. <span data-ttu-id="d9531-173">Hello virtuális gép újraindul, és folytassa a tooverify hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="d9531-173">Reboot hello VM and proceed tooverify hello installation.</span></span>


### <a name="verify-driver-installation"></a><span data-ttu-id="d9531-174">Illesztőprogram telepítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d9531-174">Verify driver installation</span></span>


<span data-ttu-id="d9531-175">tooquery hello GPU az eszköz állapotát, SSH toohello VM és futtatási hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) parancssori segédprogram hello illesztőprogrammal telepítve.</span><span class="sxs-lookup"><span data-stu-id="d9531-175">tooquery hello GPU device state, SSH toohello VM and run hello [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) command-line utility installed with hello driver.</span></span> 

<span data-ttu-id="d9531-176">Kimeneti hasonló toohello következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d9531-176">Output similar toohello following appears:</span></span>

![NVIDIA eszköz állapota](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a><span data-ttu-id="d9531-178">Illesztőprogram-frissítést CUDA</span><span class="sxs-lookup"><span data-stu-id="d9531-178">CUDA driver updates</span></span>

<span data-ttu-id="d9531-179">Azt javasoljuk, hogy rendszeres időközönként frissíti CUDA illesztőprogramok a telepítés utáni.</span><span class="sxs-lookup"><span data-stu-id="d9531-179">We recommend that you periodically update CUDA drivers after deployment.</span></span>

#### <a name="ubuntu-1604-lts"></a><span data-ttu-id="d9531-180">Ubuntu 16.04 LTS</span><span class="sxs-lookup"><span data-stu-id="d9531-180">Ubuntu 16.04 LTS</span></span>

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a><span data-ttu-id="d9531-181">7.3 centOS-alapú vagy a Red Hat Enterprise Linux 7.3.</span><span class="sxs-lookup"><span data-stu-id="d9531-181">CentOS-based 7.3 or Red Hat Enterprise Linux 7.3</span></span>

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a><span data-ttu-id="d9531-182">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="d9531-182">Troubleshooting</span></span>

* <span data-ttu-id="d9531-183">Egy ismert probléma az Azure virtuális gépeken N-sorozat hello 4.4.0-75 Linux kernel futó Ubuntu 16.04 LTS CUDA illesztőprogramok van.</span><span class="sxs-lookup"><span data-stu-id="d9531-183">There is a known issue with CUDA drivers on Azure N-series VMs running hello 4.4.0-75 Linux kernel on Ubuntu 16.04 LTS.</span></span> <span data-ttu-id="d9531-184">Ha egy korábbi kernel frissít, frissítse az tooat legalább kernel verzió 4.4.0-77.</span><span class="sxs-lookup"><span data-stu-id="d9531-184">If you are upgrading from an earlier kernel version, upgrade tooat least kernel version 4.4.0-77.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="d9531-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9531-185">Next steps</span></span>

* <span data-ttu-id="d9531-186">A hello N sorozatú virtuális gépek hello NVIDIA Feldolgozóegységekkel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="d9531-186">For more information about hello NVIDIA GPUs on hello N-series VMs, see:</span></span>
    * <span data-ttu-id="d9531-187">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (az Azure NC virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="d9531-187">[NVIDIA Tesla K80](http://www.nvidia.com/object/tesla-k80.html) (for Azure NC VMs)</span></span>
    * <span data-ttu-id="d9531-188">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (az Azure Permanens virtuális gépek)</span><span class="sxs-lookup"><span data-stu-id="d9531-188">[NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (for Azure NV VMs)</span></span>

* <span data-ttu-id="d9531-189">a telepített NVIDIA illesztőprogramokat, a Linux virtuális gép rendszerképének toocapture lásd [hogyan toogeneralize és a Linux virtuális gép rögzítése](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d9531-189">toocapture a Linux VM image with your installed NVIDIA drivers, see [How toogeneralize and capture a Linux virtual machine](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>