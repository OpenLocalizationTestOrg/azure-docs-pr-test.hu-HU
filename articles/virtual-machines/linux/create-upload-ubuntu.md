---
title: "aaaCreate és feltöltése az Ubuntu Linux VHD az Azure-ban"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD), amely tartalmazza az Ubuntu Linux operációs rendszer."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: cc546a487f769b32432a7e80ddcd0f6af44e201f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Ubuntus virtuális gép előkészítése Azure-beli használatra
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Hivatalos Ubuntu felhő lemezképek
Ubuntu most teszi közzé a következő hivatalos Azure VHD [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Ha saját speciális Ubuntu kép toobuild kell az Azure-ba, helyett eljárással hello manuális alá az alábbi ismert ajánlott toostart működik a VHD-k és igény szerint testre szabhatja. hello Újdonságok kép mindig hello helyek a következő helyen találhatók:

* Ubuntu 12.04/pontos: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy az Ubuntu Linux operációs rendszer tooa virtuális merevlemez már telepítve van. Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik. Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).

**Ubuntu telepítési megjegyzések**

* Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.
* hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.  Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.
* Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott. LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel. Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.  További információk a hello lépéseket találhatók.
* Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.

## <a name="manual-steps"></a>Manuális lépések
> [!NOTE]
> Toocreate megkísérlése előtt a saját egyéni Ubuntu rendszerképet az Azure-ba, fontolja meg hello segítségével előzetesen elkészített és tesztelése a képek [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) helyette.
> 
> 

1. Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.

2. Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.

3. Cserélje le a hello hello kép toouse Ubuntu Azure repók az aktuális tárházak találhatók. hello lépések kissé hello Ubuntu verziójától függően eltérőek.
   
    Szerkesztése előtt `/etc/apt/sources.list`, az ajánlott toomake biztonsági:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. hello Ubuntu Azure lemezképek Mostantól követi hello *hardver engedélyezése* (HWE) kernel. Frissítse a hello operációs rendszer toohello legújabb kernel hello a következő parancsok futtatásával:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    **Lásd még:**
    - [https://wiki.ubuntu.com/kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Hello kernel rendszerindító sor lárvajárat tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA. a nyitott toodo `/etc/default/grub` egy szövegszerkesztőben található nevű hello változó `GRUB_CMDLINE_LINUX_DEFAULT` (vagy felveheti Ön is szükség esetén), és módosítsa a következő paraméterek tooinclude hello:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Mentse és zárja be ezt a fájlt, és futtassa `sudo update-grub`. Ezzel biztosíthatja, hogy minden konzol üzenetküldés toohello első soros port, amely segít a problémák hibakeresési Azure technikai támogatási szolgálathoz.

6. Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.  Ez általában akkor hello alapértelmezett.

7. Hello Azure Linux ügynök telepítése:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    Hello `walinuxagent` csomag távolíthatja el a hello `NetworkManager` és `NetworkManager-gnome` a csomagokat, ha telepítve van.

8. Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

## <a name="next-steps"></a>Következő lépések
Ön éppen most már készen áll a toouse az Ubuntu Linux virtuális merevlemez toocreate új virtuális gépek Azure-ban. Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="references"></a>Referencia
Ubuntu hardver engedélyezése (HWE) kernel:

* [http://blog.utlemming.org/2015/01/ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [http://blog.utlemming.org/2015/02/1204-Azure-cloud-Images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

