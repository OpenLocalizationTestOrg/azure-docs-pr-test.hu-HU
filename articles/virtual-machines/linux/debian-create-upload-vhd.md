---
title: aaaPrepare egy Debian Linux VHD az Azure-ban |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate Debian 7 & 8 VHD-fájlokat az Azure-telepítés."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: e67d126de3db146357a6509aedb5f478576768f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a>Debian-alapú VHD előkészítése Azure-beli használatra
## <a name="prerequisites"></a>Előfeltételek
Ez a szakasz azt feltételezi, hogy már telepítette a Debian Linux operációs rendszer hello letöltésének .iso fájlból [Debian webhely](https://www.debian.org/distrib/) tooa virtuális merevlemezt. Több különféle eszköz létezik toocreate .vhd fájlok; Hyper-V csak egy példaként szolgál. A Hyper-v-vel utasításokért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>A telepítéssel kapcsolatos megjegyzések
* Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.
* hello újabb VHDX formátum nem támogatott az Azure-ban. Hello tooVHD lemezformátum Hyper-V kezelőjével vagy a hello konvertálhatja **convert-vhd** parancsmag.
* Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott. LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.
* Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel. hello Azure Linux ügynök konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet. További információk a hello lépéseket találhatók.
* Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.

## <a name="use-azure-manage-toocreate-debian-vhds"></a>Azure-kezelése toocreate Debian VHD-k használata
Eszközök is elérhetők az Azure-ba, Debian virtuális merevlemezek létrehozásának például hello [azure-kezelése](https://github.com/credativ/azure-manage) parancsfájl [credativ](http://www.credativ.com/). Ez az ajánlott megközelítést alkalmazva, és teljesen új lemezkép létrehozása hello. Debian 8 virtuális merevlemez toocreate például futtassa a következő parancsok toodownload azure-kezelése hello (és függőségeinek), és hello azure_build_image parancsfájl futtatása:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Manuálisan a Debian virtuális merevlemez előkészítése
1. A Hyper-V kezelőjében válassza ki a hello virtuális gépet.
2. Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.
3. Hello sort megjegyzésbe **deb cdrom** a `/etc/apt/source.list` Ha hello virtuális gép ISO-fájl alapján.
4. Hello szerkesztése `/etc/default/grub` fájlt, és módosítsa a hello **GRUB_CMDLINE_LINUX** paraméter használata a következőképpen történik a tooinclude kiegészítő rendszermag paraméterek az Azure-bA.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. Hello lárvajárat újraépítése, majd futtassa:
   
        # sudo update-grub
6. Adja hozzá a Debian tartozó Azure adattárak too/etc/apt/sources.list Debian 7 vagy 8:
   
    **Debian 7.x "Wheezy"**
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. Hello Azure Linux ügynök telepítése:
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. Debian 7 esetén szükséges toorun hello 3.16-alapú kernel adattárból hello wheezy backports. Először létre kell hoznia egy /etc/apt/preferences.d/linux.pref meghívásra hello tartalma a következő fájl:
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    Ezután futtassa a "sudo apt-get telepítése linux-lemezkép-amd64" tooinstall hello új kernel.
3. Hello virtuális gép kiosztásának megszüntetése és előkészíti az Azure-on történő üzembe helyezéséhez, és futtassa:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. Kattintson a **művelet** -> leállítási le a Hyper-V kezelőjében. A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.

## <a name="next-steps"></a>Következő lépések
Ön éppen most már készen áll a toouse a Debian virtuális merevlemez toocreate új virtuális gépeket az Azure-ban. Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

