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
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Az Azure virtuális gépek hálózati teljesítmény optimalizálása

Azure virtuális gépek (VM) rendelkezik, amely további optimalizálhatók a hálózat átviteli sebességét az alapértelmezett hálózati beállításokat. Ez a cikk ismerteti, hogyan toooptimize a hálózati átviteli sebesség a Microsoft Azure Windows és Linux virtuális gépeken, beleértve például Ubuntu, a CentOS és a Red Hat fő terjesztéseket.

## <a name="windows-vm"></a>Windowsos VM

Ha a Windows virtuális gép használata támogatott [az elérését gyorsítja fel hálózati](virtual-network-create-vm-accelerated-networking.md), hello optimális átviteli konfigurációját, hogy a funkció engedélyezése lenne. A többi Windows virtuális fogadó oldali skálázás (RSS) használatával érhető el újabb maximális átviteli sebesség, mint egy virtuális gép RSS nélkül. Alapértelmezés szerint a Windows virtuális gép RSS is tiltható le. Végezze el a következő lépéseket toodetermine, hogy engedélyezve van az RSS hello és tooenable, ha le van tiltva.

1. Adja meg a hello `Get-NetAdapterRss` PowerShell parancs toosee, ha engedélyezve van az RSS a hálózati adapterhez. Hello a következő egy példa a kimenetre visszakapott hello `Get-NetAdapterRss`, RSS nincs engedélyezve.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Adja meg a következő parancs tooenable RSS hello:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    hello előző parancs nincs kimenete. hello parancs módosítani a hálózati kártya beállításait, ideiglenes kapcsolatok megszakadását okozva körülbelül egy percet. Egy Reconnecting párbeszédpanel során hello kapcsolatok megszakadását. Hello harmadik kísérlet után a kapcsolat jellemzően helyre.
3. Győződjön meg arról, hogy engedélyezve van az RSS a virtuális gép hello hello megadásával `Get-NetAdapterRss` újra a parancsot. Ha sikeres, a következő egy példa a kimenetre hello adja vissza:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Linux virtuális gép

Az RSS az Azure Linux virtuális gép alapértelmezés szerint mindig engedélyezve van. Linux kernelek január, 2017 óta megjelent új hálózati optimalizálási beállításokat, amelyek lehetővé teszik egy Linux virtuális gép tooachieve nagyobb hálózati teljesítményt tartalmazhat.

### <a name="ubuntu"></a>Ubuntu

A sorrend tooget hello optimalizálása először frissítse toohello legújabb támogatott verzió frissítésétől. június 2017, amely:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Hello frissítés befejeződése után adja meg a következő parancsok tooget hello legújabb kernel hello:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Nem kötelező parancs:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure betekintő kernel
> [!WARNING]
> Az Azure Linux Preview kernel nem rendelkezhet azonos szintű rendelkezésre állást és megbízhatóságot piactéren elérhető rendszerkép és kernelek, amelyek általában a rendelkezésre állási kiadás hello. hello szolgáltatás nem támogatott, van, korlátozott képességeit, és nem lehet a megbízható, mint az alapértelmezett kernel hello. A kernel ne használja termelési számítási feladatokhoz.

Jelentős teljesítménye szempontjából hello Azure Linux kernel javasolt telepítésével érhető el. tootry a kernel adja hozzá a sor too/etc/apt/sources.list

```bash
#add this toohello end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Ezután futtassa az alábbi parancsokat rendszergazdaként.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

A sorrend tooget hello optimalizálása először frissítse toohello legújabb támogatott verzió frissítésétől július 2017, amely:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Hello frissítés befejezése után telepítse a legújabb Linux integrációs szolgáltatások (LIS) hello.
hello teljesítményoptimalizálási LIS 4.2.2-2-től kezdődő van. Adja meg a következő parancsok tooinstall LIS hello:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

A sorrend tooget hello optimalizálása először frissítse toohello legújabb támogatott verzió frissítésétől július 2017, amely:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Hello frissítés befejezése után telepítse a legújabb Linux integrációs szolgáltatások (LIS) hello.
hello teljesítményoptimalizálási LIS 4.2-től kezdődő van. Adja meg a következő parancsok toodownload hello és telepíteni azokat:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Tudjon meg többet a Linux integrációs szolgáltatások verzió 4.2 a Hyper-V hello megtekintésével [letöltési oldalát](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Következő lépések
* Most, hogy hello VM optimalizált, lásd: hello eredmény [sávszélesség/átviteli Azure virtuális gép tesztelési](virtual-network-bandwidth-testing.md) a forgatókönyvéhez.
* Ismerje meg [Azure Virtual Network gyakori kérdések (GYIK)](virtual-networks-faq.md)
