---
title: "az Azure-ban módosulnak aaaLinux VM eszközök neve |} Microsoft Docs"
description: "Miért eszköz nevek módosulnak, és megoldást nyújtanak a probléma hello ismerteti."
services: virtual-machines-linux
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: 
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 07/12/2017
ms.author: genli
ms.openlocfilehash: 4d3a5853d61edd2c8e8b85ab69e5ed3b3bc00bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a>Hibáinak elhárítása: Linux virtuális gép névvel, módosulnak

hello a cikk azt ismerteti, miért eszközök neve után indítsa újra a Linux virtuális gép (VM), vagy csatlakoztassa újra hello lemezek módosulnak. A probléma hello megoldást is kínál.

## <a name="symptom"></a>Jelenség

Hello, amikor a Microsoft Azure-ban futó Linux virtuális gépek a következő problémák léphetnek fel.

- virtuális gép hello tooboot újraindítás után sikertelen lesz.

- Ha adatlemezt leválasztott és objektumkörnyezetben, hello eszközök nevek lemezek módosult.

- Egy alkalmazás vagy a lemez eszköznév használatával hivatkozó parancsfájl sikertelen lesz. Látnia, hogy hello hello lemez eszköz neve megváltozott.

## <a name="cause"></a>Ok

Eszköz elérési utak, a Linux nem garantált toobe konzisztens újraindításainál. Eszköz neve a fő (levél) és a kisebb számot áll.  Ha az illesztőprogram hello Linux tárolási egy új eszközt észlel, hello közötti fő- és alverzió eszköz számok tooit rendeli. Ha egy eszközt a rendszer eltávolítja, hello eszköz számadatok felszabadított toobe adhat meg később újból.

hello probléma oka, hogy hello Linux hello SCSI-alrendszere által ütemezett vizsgálatot eszköz történik aszinkron módon történik. hello végső eszköz elérési elnevezési újraindításainál változhat. 

## <a name="solution"></a>Megoldás

tooresolve probléma állandó elnevezési használja. Nincsenek négy módszerek toopersistent elnevezési - fájlrendszer felirat, uuid, -azonosító szerint és az elérési út. Javasoljuk a hello filesystem címke és UUID módszerek Azure Linux virtuális gépekhez. 

A legtöbb terjesztéseket is adja meg vagy hello **nofail** vagy **nobootwait** fstab beállítások. Ezek a beállítások csak a rendszer tooboot, még akkor is, ha hello lemezhiba toomount indításkor. Hello terjesztési dokumentációjában olvashat ezeket a paramétereket. További információ a hogyan tooconfigure egy Linux virtuális gép toouse adatlemezt, hozzáadásakor UUID: [toohello Linux virtuális gép toomount hello új lemez csatlakozás](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk). 

Amikor hello Azure Linux-ügynök telepítve van egy virtuális Gépet, az Udev szabályok tooconstruct szimbolikus hivatkozásokat tartalmaz a **/dev/disk/azure**. Udev szabályok segítségével alkalmazások és parancsfájlok tooidentify lemezek csatolt toohello VM, amelyek típusa és hello logikai Egységet.

## <a name="more-information"></a>További információ

### <a name="identify-disk-luns"></a>Azonosítsa a logikai lemez

Egy alkalmazás logikai egységek toofind használja minden hello csatlakoztatott lemezek és összeállításával szimbolikus csatolást. hello Azure Linux ügynök mostantól tartalmazza az udev szabályok által beállított szimbolikus hivatkozásokat a LUN-toohello eszközökről, az alábbiak szerint:

    $ tree /dev/disk/azure

    /dev/disk/azure
    ├── resource -> ../../sdb
    ├── resource-part1 -> ../../sdb1
    ├── root -> ../../sda
    ├── root-part1 -> ../../sda1
    └── scsi1
        ├── lun0 -> ../../../sdc
        ├── lun0-part1 -> ../../../sdc1
        ├── lun1 -> ../../../sdd
        ├── lun1-part1 -> ../../../sdd1
        ├── lun1-part2 -> ../../../sdd2
        └── lun1-part3 -> ../../../sdd3                                    
                                 

LUN-adatok is lekérhetők hello Linux Vendég lsscsi vagy hasonló eszköz használatával az alábbiak szerint.

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

A Vendég LUN információ használható Azure-előfizetés metaadatok tooidentify hello helyét az Azure storage hello hello partíció adatok tárolására szolgáló virtuális merevlemez. Például használja hello az cli:

    $ az vm show --resource-group testVM --name testVM | jq -r .storageProfile.dataDisks                                        
    [                                                                                                                                                                  
    {                                                                                                                                                                  
    "caching": "None",                                                                                                                                              
      "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 1023,                                                                                                                                             
      "image": null,                                                                                                                                                   
    "lun": 0,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-114353",                                                                                                                    
    "vhd": {                                                                                                                                                          
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-114353.vhd"                                                       
    }                                                                                                                                                              
    },                                                                                                                                                                
    {                                                                                                                                                                   
    "caching": "None",                                                                                                                                               
    "createOption": "empty",                                                                                                                                         
    "diskSizeGb": 512,                                                                                                                                              
    "image": null,                                                                                                                                                   
    "lun": 1,                                                                                                                                                        
    "managedDisk": null,                                                                                                                                             
    "name": "testVM-20170619-121516",                                                                                                                    
    "vhd": {                                                                                                                                                           
      "uri": "https://testVM.blob.core.windows.net/vhd/testVM-20170619-121516.vhd"                                                       
      }                                                                                                                                                             
      }                                                                                                                                                             
    ]

### <a name="discover-filesystem-uuids-by-using-blkid"></a>Fájlrendszer UUID-k használatával blkid felderítése

Egy parancsfájl vagy egy alkalmazás olvashatja blkid hello kimenete vagy hasonló információforrásokat, és olyan szimbolikus csatolásának összeállításához **/dev** való használatra. hello kimenet megjeleníti minden lemezek UUID-k hello csatolt toohello VM és hello eszköz fájl toowhich társítva:

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

hello waagent udev szabályok összeállításához szimbolikus hivatkozásokat tartalmaz a **/dev/disk/azure**:


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


hello alkalmazás használhat ez az információ hello rendszerindító lemez eszköz és hello erőforrás (elmúló) lemez azonosításához. Azure-alkalmazások kell alatt túl**/dev/disk/azure/root-part1** vagy **/dev/disk/azure-resource-part1** toodiscover ezek a partíciók.

Ha további partíciókra hello blkid listából, azok adatlemezt található. Alkalmazások karbantartása hello UUID ezek a partíciók és a futtatás közben használ. egy elérési utat, például a hello toodiscover hello eszköz neve alatt:

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-hello-latest-azure-storage-rules"></a>Hello legújabb az Azure storage szabályok lekérése

toohello legújabb az Azure storage szabályok, futtassa az alábbi parancsokat:

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


További információkért tekintse meg a következő cikkek hello:

- [Ubuntu: UUID használatával](https://help.ubuntu.com/community/UsingUUID)

- [Red Hat: Állandó elnevezése](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [Linux: UUID-k mit tehetnek meg](https://www.linux.com/news/what-uuids-can-do-you)

- [Udev: Bevezetés tooDevice felügyelet a Modern Linux rendszer](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

