---
title: "VM-hálózat átbocsátóképességét aaaOptimize |} Microsoft Docs"
description: "Ismerje meg, hogyan toooptimize Azure virtuális gép hálózati átviteli sebesség."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: a5cff2d0ab6e3553c3f90d99629521a431477de0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a><span data-ttu-id="342bc-103">Az Azure virtuális gépek hálózati teljesítmény optimalizálása</span><span class="sxs-lookup"><span data-stu-id="342bc-103">Optimize network throughput for Azure virtual machines</span></span>

<span data-ttu-id="342bc-104">Azure virtuális gépek (VM) rendelkezik, amely további optimalizálhatók a hálózat átviteli sebességét az alapértelmezett hálózati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="342bc-104">Azure virtual machines (VM) have default network settings that can be further optimized for network throughput.</span></span> <span data-ttu-id="342bc-105">Ez a cikk ismerteti, hogyan toooptimize a hálózati átviteli sebesség a Microsoft Azure Windows és Linux virtuális gépeken, beleértve például Ubuntu, a CentOS és a Red Hat fő terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="342bc-105">This article describes how toooptimize network throughput for Microsoft Azure Windows and Linux VMs, including major distributions such as Ubuntu, CentOS and Red Hat.</span></span>

## <a name="windows-vm"></a><span data-ttu-id="342bc-106">Windowsos VM</span><span class="sxs-lookup"><span data-stu-id="342bc-106">Windows VM</span></span>

<span data-ttu-id="342bc-107">Ha a Windows virtuális gép használata támogatott [az elérését gyorsítja fel hálózati](virtual-network-create-vm-accelerated-networking.md), hello optimális átviteli konfigurációját, hogy a funkció engedélyezése lenne.</span><span class="sxs-lookup"><span data-stu-id="342bc-107">If your Windows VM is supported with [Accelerated Networking](virtual-network-create-vm-accelerated-networking.md), enabling that feature would be hello optimal configuration for throughput.</span></span> <span data-ttu-id="342bc-108">A többi Windows virtuális fogadó oldali skálázás (RSS) használatával érhető el újabb maximális átviteli sebesség, mint egy virtuális gép RSS nélkül.</span><span class="sxs-lookup"><span data-stu-id="342bc-108">For all other Windows VMs, using Receive Side Scaling (RSS) can reach higher maximal throughput than a VM without RSS.</span></span> <span data-ttu-id="342bc-109">Alapértelmezés szerint a Windows virtuális gép RSS is tiltható le.</span><span class="sxs-lookup"><span data-stu-id="342bc-109">RSS may be disabled by default in a Windows VM.</span></span> <span data-ttu-id="342bc-110">Végezze el a következő lépéseket toodetermine, hogy engedélyezve van az RSS hello és tooenable, ha le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="342bc-110">Complete hello following steps toodetermine whether RSS is enabled and tooenable it if it's disabled.</span></span>

1. <span data-ttu-id="342bc-111">Adja meg a hello `Get-NetAdapterRss` PowerShell parancs toosee, ha engedélyezve van az RSS a hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="342bc-111">Enter hello `Get-NetAdapterRss` PowerShell command toosee if RSS is enabled for a network adapter.</span></span> <span data-ttu-id="342bc-112">Hello a következő egy példa a kimenetre visszakapott hello `Get-NetAdapterRss`, RSS nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="342bc-112">In hello following example output returned from hello `Get-NetAdapterRss`, RSS is not enabled.</span></span>

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. <span data-ttu-id="342bc-113">Adja meg a következő parancs tooenable RSS hello:</span><span class="sxs-lookup"><span data-stu-id="342bc-113">Enter hello following command tooenable RSS:</span></span>

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    <span data-ttu-id="342bc-114">hello előző parancs nincs kimenete.</span><span class="sxs-lookup"><span data-stu-id="342bc-114">hello previous command does not have an output.</span></span> <span data-ttu-id="342bc-115">hello parancs módosítani a hálózati kártya beállításait, ideiglenes kapcsolatok megszakadását okozva körülbelül egy percet.</span><span class="sxs-lookup"><span data-stu-id="342bc-115">hello command changed NIC settings, causing temporary connectivity loss for about one minute.</span></span> <span data-ttu-id="342bc-116">Egy Reconnecting párbeszédpanel során hello kapcsolatok megszakadását.</span><span class="sxs-lookup"><span data-stu-id="342bc-116">A Reconnecting dialog box appears during hello connectivity loss.</span></span> <span data-ttu-id="342bc-117">Hello harmadik kísérlet után a kapcsolat jellemzően helyre.</span><span class="sxs-lookup"><span data-stu-id="342bc-117">Connectivity is typically restored after hello third attempt.</span></span>
3. <span data-ttu-id="342bc-118">Győződjön meg arról, hogy engedélyezve van az RSS a virtuális gép hello hello megadásával `Get-NetAdapterRss` újra a parancsot.</span><span class="sxs-lookup"><span data-stu-id="342bc-118">Confirm that RSS is enabled in hello VM by entering hello `Get-NetAdapterRss` command again.</span></span> <span data-ttu-id="342bc-119">Ha sikeres, a következő egy példa a kimenetre hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="342bc-119">If successful, hello following example output is returned:</span></span>

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a><span data-ttu-id="342bc-120">Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="342bc-120">Linux VM</span></span>

<span data-ttu-id="342bc-121">Az RSS az Azure Linux virtuális gép alapértelmezés szerint mindig engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="342bc-121">RSS is always enabled by default in an Azure Linux VM.</span></span> <span data-ttu-id="342bc-122">Linux kernelek január, 2017 óta megjelent új hálózati optimalizálási beállításokat, amelyek lehetővé teszik egy Linux virtuális gép tooachieve nagyobb hálózati teljesítményt tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="342bc-122">Linux kernels released since January, 2017 include new network optimization options that enable a Linux VM tooachieve higher network throughput.</span></span>

### <a name="ubuntu"></a><span data-ttu-id="342bc-123">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="342bc-123">Ubuntu</span></span>

<span data-ttu-id="342bc-124">A sorrend tooget hello optimalizálása először frissítse toohello legújabb támogatott verzió frissítésétől. június 2017, amely:</span><span class="sxs-lookup"><span data-stu-id="342bc-124">In order tooget hello optimization, first update toohello latest supported version, as of June 2017, which is:</span></span>
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
<span data-ttu-id="342bc-125">Hello frissítés befejeződése után adja meg a következő parancsok tooget hello legújabb kernel hello:</span><span class="sxs-lookup"><span data-stu-id="342bc-125">After hello update is complete, enter hello following commands tooget hello latest kernel:</span></span>

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

<span data-ttu-id="342bc-126">Nem kötelező parancs:</span><span class="sxs-lookup"><span data-stu-id="342bc-126">Optional command:</span></span>

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a><span data-ttu-id="342bc-127">Ubuntu Azure betekintő kernel</span><span class="sxs-lookup"><span data-stu-id="342bc-127">Ubuntu Azure Preview kernel</span></span>
> [!WARNING]
> <span data-ttu-id="342bc-128">Az Azure Linux Preview kernel nem rendelkezhet azonos szintű rendelkezésre állást és megbízhatóságot piactéren elérhető rendszerkép és kernelek, amelyek általában a rendelkezésre állási kiadás hello.</span><span class="sxs-lookup"><span data-stu-id="342bc-128">This Azure Linux Preview kernel may not have hello same level of availability and reliability as Marketplace images and kernels that are in general availability release.</span></span> <span data-ttu-id="342bc-129">hello szolgáltatás nem támogatott, van, korlátozott képességeit, és nem lehet a megbízható, mint az alapértelmezett kernel hello.</span><span class="sxs-lookup"><span data-stu-id="342bc-129">hello feature is not supported, may have constrained capabilities, and may not be as reliable as hello default kernel.</span></span> <span data-ttu-id="342bc-130">A kernel ne használja termelési számítási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="342bc-130">Do not use this kernel for production workloads.</span></span>

<span data-ttu-id="342bc-131">Jelentős teljesítménye szempontjából hello Azure Linux kernel javasolt telepítésével érhető el.</span><span class="sxs-lookup"><span data-stu-id="342bc-131">Significant throughput performance can be achieved by installing hello proposed Azure Linux kernel.</span></span> <span data-ttu-id="342bc-132">tootry a kernel adja hozzá a sor too/etc/apt/sources.list</span><span class="sxs-lookup"><span data-stu-id="342bc-132">tootry this kernel, add this line too/etc/apt/sources.list</span></span>

```bash
#add this toohello end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

<span data-ttu-id="342bc-133">Ezután futtassa az alábbi parancsokat rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="342bc-133">Then run these commands as root.</span></span>
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a><span data-ttu-id="342bc-134">CentOS</span><span class="sxs-lookup"><span data-stu-id="342bc-134">CentOS</span></span>

<span data-ttu-id="342bc-135">A sorrend tooget hello optimalizálása először frissítse toohello legújabb támogatott verzió frissítésétől július 2017, amely:</span><span class="sxs-lookup"><span data-stu-id="342bc-135">In order tooget hello optimization, first update toohello latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
<span data-ttu-id="342bc-136">Hello frissítés befejezése után telepítse a legújabb Linux integrációs szolgáltatások (LIS) hello.</span><span class="sxs-lookup"><span data-stu-id="342bc-136">After hello update is complete, install hello latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="342bc-137">hello teljesítményoptimalizálási LIS 4.2.2-2-től kezdődő van.</span><span class="sxs-lookup"><span data-stu-id="342bc-137">hello throughput optimization is in LIS, starting from 4.2.2-2.</span></span> <span data-ttu-id="342bc-138">Adja meg a következő parancsok tooinstall LIS hello:</span><span class="sxs-lookup"><span data-stu-id="342bc-138">Enter hello following commands tooinstall LIS:</span></span>

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a><span data-ttu-id="342bc-139">Red Hat</span><span class="sxs-lookup"><span data-stu-id="342bc-139">Red Hat</span></span>

<span data-ttu-id="342bc-140">A sorrend tooget hello optimalizálása először frissítse toohello legújabb támogatott verzió frissítésétől július 2017, amely:</span><span class="sxs-lookup"><span data-stu-id="342bc-140">In order tooget hello optimization, first update toohello latest supported version, as of July 2017, which is:</span></span>
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
<span data-ttu-id="342bc-141">Hello frissítés befejezése után telepítse a legújabb Linux integrációs szolgáltatások (LIS) hello.</span><span class="sxs-lookup"><span data-stu-id="342bc-141">After hello update is complete, install hello latest Linux Integration Services (LIS).</span></span>
<span data-ttu-id="342bc-142">hello teljesítményoptimalizálási LIS 4.2-től kezdődő van.</span><span class="sxs-lookup"><span data-stu-id="342bc-142">hello throughput optimization is in LIS, starting from 4.2.</span></span> <span data-ttu-id="342bc-143">Adja meg a következő parancsok toodownload hello és telepíteni azokat:</span><span class="sxs-lookup"><span data-stu-id="342bc-143">Enter hello following commands toodownload and install LIS:</span></span>

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

<span data-ttu-id="342bc-144">Tudjon meg többet a Linux integrációs szolgáltatások verzió 4.2 a Hyper-V hello megtekintésével [letöltési oldalát](https://www.microsoft.com/download/details.aspx?id=55106).</span><span class="sxs-lookup"><span data-stu-id="342bc-144">Learn more about Linux Integration Services Version 4.2 for Hyper-V by viewing hello [download page](https://www.microsoft.com/download/details.aspx?id=55106).</span></span>

## <a name="next-steps"></a><span data-ttu-id="342bc-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="342bc-145">Next steps</span></span>
* <span data-ttu-id="342bc-146">Most, hogy hello VM optimalizált, lásd: hello eredmény [sávszélesség/átviteli Azure virtuális gép tesztelési](virtual-network-bandwidth-testing.md) a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="342bc-146">Now that hello VM is optimized, see hello result with [Bandwidth/Throughput testing Azure VM](virtual-network-bandwidth-testing.md) for your scenario.</span></span>
* <span data-ttu-id="342bc-147">Ismerje meg [Azure Virtual Network gyakori kérdések (GYIK)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="342bc-147">Learn more with [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
