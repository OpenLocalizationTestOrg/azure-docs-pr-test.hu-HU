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
# <a name="troubleshooting-linux-vm-device-names-are-changed"></a><span data-ttu-id="d55c1-103">Hibáinak elhárítása: Linux virtuális gép névvel, módosulnak</span><span class="sxs-lookup"><span data-stu-id="d55c1-103">Troubleshooting: Linux VM device names are changed</span></span>

<span data-ttu-id="d55c1-104">hello a cikk azt ismerteti, miért eszközök neve után indítsa újra a Linux virtuális gép (VM), vagy csatlakoztassa újra hello lemezek módosulnak.</span><span class="sxs-lookup"><span data-stu-id="d55c1-104">hello article explains why device names are changed after you restart a Linux virtual machine (VM), or reattach hello disks.</span></span> <span data-ttu-id="d55c1-105">A probléma hello megoldást is kínál.</span><span class="sxs-lookup"><span data-stu-id="d55c1-105">It also provides hello solution for this problem.</span></span>

## <a name="symptom"></a><span data-ttu-id="d55c1-106">Jelenség</span><span class="sxs-lookup"><span data-stu-id="d55c1-106">Symptom</span></span>

<span data-ttu-id="d55c1-107">Hello, amikor a Microsoft Azure-ban futó Linux virtuális gépek a következő problémák léphetnek fel.</span><span class="sxs-lookup"><span data-stu-id="d55c1-107">You may experience hello following problems when running Linux VMs in Microsoft Azure.</span></span>

- <span data-ttu-id="d55c1-108">virtuális gép hello tooboot újraindítás után sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="d55c1-108">hello VM fails tooboot after a restart.</span></span>

- <span data-ttu-id="d55c1-109">Ha adatlemezt leválasztott és objektumkörnyezetben, hello eszközök nevek lemezek módosult.</span><span class="sxs-lookup"><span data-stu-id="d55c1-109">If data disks are detached and reattached, hello devices names for disks are changed.</span></span>

- <span data-ttu-id="d55c1-110">Egy alkalmazás vagy a lemez eszköznév használatával hivatkozó parancsfájl sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="d55c1-110">An application or script that references a disk by using device name fails.</span></span> <span data-ttu-id="d55c1-111">Látnia, hogy hello hello lemez eszköz neve megváltozott.</span><span class="sxs-lookup"><span data-stu-id="d55c1-111">You find that hello device name of hello disk is changed.</span></span>

## <a name="cause"></a><span data-ttu-id="d55c1-112">Ok</span><span class="sxs-lookup"><span data-stu-id="d55c1-112">Cause</span></span>

<span data-ttu-id="d55c1-113">Eszköz elérési utak, a Linux nem garantált toobe konzisztens újraindításainál.</span><span class="sxs-lookup"><span data-stu-id="d55c1-113">Device paths in Linux are not guaranteed toobe consistent across restarts.</span></span> <span data-ttu-id="d55c1-114">Eszköz neve a fő (levél) és a kisebb számot áll.</span><span class="sxs-lookup"><span data-stu-id="d55c1-114">Device names consist of major (letter) and minor numbers.</span></span>  <span data-ttu-id="d55c1-115">Ha az illesztőprogram hello Linux tárolási egy új eszközt észlel, hello közötti fő- és alverzió eszköz számok tooit rendeli.</span><span class="sxs-lookup"><span data-stu-id="d55c1-115">When hello Linux storage device driver detects a new device, it assigns major and minor device numbers tooit from hello available range.</span></span> <span data-ttu-id="d55c1-116">Ha egy eszközt a rendszer eltávolítja, hello eszköz számadatok felszabadított toobe adhat meg később újból.</span><span class="sxs-lookup"><span data-stu-id="d55c1-116">When a device is removed, hello device numbers are freed toobe reused later.</span></span>

<span data-ttu-id="d55c1-117">hello probléma oka, hogy hello Linux hello SCSI-alrendszere által ütemezett vizsgálatot eszköz történik aszinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="d55c1-117">hello problem occurs because hello device scanning in Linux scheduled by hello SCSI subsystem happens asynchronously.</span></span> <span data-ttu-id="d55c1-118">hello végső eszköz elérési elnevezési újraindításainál változhat.</span><span class="sxs-lookup"><span data-stu-id="d55c1-118">hello final device path naming may vary across restarts.</span></span> 

## <a name="solution"></a><span data-ttu-id="d55c1-119">Megoldás</span><span class="sxs-lookup"><span data-stu-id="d55c1-119">Solution</span></span>

<span data-ttu-id="d55c1-120">tooresolve probléma állandó elnevezési használja.</span><span class="sxs-lookup"><span data-stu-id="d55c1-120">tooresolve this problem, use persistent naming.</span></span> <span data-ttu-id="d55c1-121">Nincsenek négy módszerek toopersistent elnevezési - fájlrendszer felirat, uuid, -azonosító szerint és az elérési út.</span><span class="sxs-lookup"><span data-stu-id="d55c1-121">There are four methods toopersistent naming - by filesystem label, by uuid, by id and by path.</span></span> <span data-ttu-id="d55c1-122">Javasoljuk a hello filesystem címke és UUID módszerek Azure Linux virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="d55c1-122">We recommend hello filesystem label and UUID methods for Azure Linux VMs.</span></span> 

<span data-ttu-id="d55c1-123">A legtöbb terjesztéseket is adja meg vagy hello **nofail** vagy **nobootwait** fstab beállítások.</span><span class="sxs-lookup"><span data-stu-id="d55c1-123">Most distributions also provide either hello **nofail** or **nobootwait** fstab options.</span></span> <span data-ttu-id="d55c1-124">Ezek a beállítások csak a rendszer tooboot, még akkor is, ha hello lemezhiba toomount indításkor.</span><span class="sxs-lookup"><span data-stu-id="d55c1-124">These options enable a system tooboot even if hello disk fails toomount at startup.</span></span> <span data-ttu-id="d55c1-125">Hello terjesztési dokumentációjában olvashat ezeket a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="d55c1-125">Check hello distribution's documentation for more information about these parameters.</span></span> <span data-ttu-id="d55c1-126">További információ a hogyan tooconfigure egy Linux virtuális gép toouse adatlemezt, hozzáadásakor UUID: [toohello Linux virtuális gép toomount hello új lemez csatlakozás](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="d55c1-126">For more information about how tooconfigure a Linux VM toouse a UUID when you add a data disk, see [Connect toohello Linux VM toomount hello new disk](add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> 

<span data-ttu-id="d55c1-127">Amikor hello Azure Linux-ügynök telepítve van egy virtuális Gépet, az Udev szabályok tooconstruct szimbolikus hivatkozásokat tartalmaz a **/dev/disk/azure**.</span><span class="sxs-lookup"><span data-stu-id="d55c1-127">When hello Azure Linux agent is installed on a VM, it uses Udev rules tooconstruct a set of symbolic links under **/dev/disk/azure**.</span></span> <span data-ttu-id="d55c1-128">Udev szabályok segítségével alkalmazások és parancsfájlok tooidentify lemezek csatolt toohello VM, amelyek típusa és hello logikai Egységet.</span><span class="sxs-lookup"><span data-stu-id="d55c1-128">These Udev rules can be used by applications and scripts tooidentify disks are attached toohello VM, their type, and hello LUN.</span></span>

## <a name="more-information"></a><span data-ttu-id="d55c1-129">További információ</span><span class="sxs-lookup"><span data-stu-id="d55c1-129">More information</span></span>

### <a name="identify-disk-luns"></a><span data-ttu-id="d55c1-130">Azonosítsa a logikai lemez</span><span class="sxs-lookup"><span data-stu-id="d55c1-130">Identify disk LUNs</span></span>

<span data-ttu-id="d55c1-131">Egy alkalmazás logikai egységek toofind használja minden hello csatlakoztatott lemezek és összeállításával szimbolikus csatolást.</span><span class="sxs-lookup"><span data-stu-id="d55c1-131">An application can use LUNs toofind all hello attached disks and constructing symbolic links.</span></span> <span data-ttu-id="d55c1-132">hello Azure Linux ügynök mostantól tartalmazza az udev szabályok által beállított szimbolikus hivatkozásokat a LUN-toohello eszközökről, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d55c1-132">hello Azure Linux agent now includes udev rules that set up symbolic links from a LUN toohello devices, as follows:</span></span>

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
                                 

<span data-ttu-id="d55c1-133">LUN-adatok is lekérhetők hello Linux Vendég lsscsi vagy hasonló eszköz használatával az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="d55c1-133">LUN information can also be retrieved from hello Linux guest using lsscsi or similar tool as follows.</span></span>

       $ sudo lsscsi

      [1:0:0:0] cd/dvd Msft Virtual CD/ROM 1.0 /dev/sr0

      [2:0:0:0] disk Msft Virtual Disk 1.0 /dev/sda

      [3:0:1:0] disk Msft Virtual Disk 1.0 /dev/sdb

      [5:0:0:0] disk Msft Virtual Disk 1.0 /dev/sdc

      [5:0:0:1] disk Msft Virtual Disk 1.0 /dev/sdd

<span data-ttu-id="d55c1-134">A Vendég LUN információ használható Azure-előfizetés metaadatok tooidentify hello helyét az Azure storage hello hello partíció adatok tárolására szolgáló virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="d55c1-134">This guest LUN information can be used with Azure subscription metadata tooidentify hello location in Azure storage of hello VHD which stores hello partition data.</span></span> <span data-ttu-id="d55c1-135">Például használja hello az cli:</span><span class="sxs-lookup"><span data-stu-id="d55c1-135">For example, use hello az cli:</span></span>

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

### <a name="discover-filesystem-uuids-by-using-blkid"></a><span data-ttu-id="d55c1-136">Fájlrendszer UUID-k használatával blkid felderítése</span><span class="sxs-lookup"><span data-stu-id="d55c1-136">Discover filesystem UUIDs by using blkid</span></span>

<span data-ttu-id="d55c1-137">Egy parancsfájl vagy egy alkalmazás olvashatja blkid hello kimenete vagy hasonló információforrásokat, és olyan szimbolikus csatolásának összeállításához **/dev** való használatra.</span><span class="sxs-lookup"><span data-stu-id="d55c1-137">A script or application can read hello output of blkid, or similar sources of information, and construct symbolic links in **/dev** for use.</span></span> <span data-ttu-id="d55c1-138">hello kimenet megjeleníti minden lemezek UUID-k hello csatolt toohello VM és hello eszköz fájl toowhich társítva:</span><span class="sxs-lookup"><span data-stu-id="d55c1-138">hello output will show hello UUIDs of all disks attached toohello VM and hello device file toowhich they are associated:</span></span>

    $ sudo blkid -s UUID

    /dev/sr0: UUID="120B021372645f72"
    /dev/sda1: UUID="52c6959b-79b0-4bdd-8ed6-71e0ba782fb4"
    /dev/sdb1: UUID="176250df-9c7c-436f-94e4-d13f9bdea744"
    /dev/sdc1: UUID="b0048738-4ecc-4837-9793-49ce296d2692"

<span data-ttu-id="d55c1-139">hello waagent udev szabályok összeállításához szimbolikus hivatkozásokat tartalmaz a **/dev/disk/azure**:</span><span class="sxs-lookup"><span data-stu-id="d55c1-139">hello waagent udev rules construct a set of symbolic links under **/dev/disk/azure**:</span></span>


    $ ls -l /dev/disk/azure

    total 0
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 resource -> ../../sdb
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 resource-part1 -> ../../sdb1
    lrwxrwxrwx 1 root root  9 Jun  2 23:17 root -> ../../sda
    lrwxrwxrwx 1 root root 10 Jun  2 23:17 root-part1 -> ../../sda1


<span data-ttu-id="d55c1-140">hello alkalmazás használhat ez az információ hello rendszerindító lemez eszköz és hello erőforrás (elmúló) lemez azonosításához.</span><span class="sxs-lookup"><span data-stu-id="d55c1-140">hello application can use this information identify hello boot disk device and hello resource (ephemeral) disk.</span></span> <span data-ttu-id="d55c1-141">Azure-alkalmazások kell alatt túl**/dev/disk/azure/root-part1** vagy **/dev/disk/azure-resource-part1** toodiscover ezek a partíciók.</span><span class="sxs-lookup"><span data-stu-id="d55c1-141">In Azure, applications should refer too**/dev/disk/azure/root-part1** or **/dev/disk/azure-resource-part1** toodiscover these partitions.</span></span>

<span data-ttu-id="d55c1-142">Ha további partíciókra hello blkid listából, azok adatlemezt található.</span><span class="sxs-lookup"><span data-stu-id="d55c1-142">If there are additional partitions from hello blkid list, they reside on a data disk.</span></span> <span data-ttu-id="d55c1-143">Alkalmazások karbantartása hello UUID ezek a partíciók és a futtatás közben használ. egy elérési utat, például a hello toodiscover hello eszköz neve alatt:</span><span class="sxs-lookup"><span data-stu-id="d55c1-143">Applications can maintain hello UUID for these partitions and use a path like hello below toodiscover hello device name at runtime:</span></span>

    $ ls -l /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692

    lrwxrwxrwx 1 root root 10 Jun 19 15:57 /dev/disk/by-uuid/b0048738-4ecc-4837-9793-49ce296d2692 -> ../../sdc1

    
### <a name="get-hello-latest-azure-storage-rules"></a><span data-ttu-id="d55c1-144">Hello legújabb az Azure storage szabályok lekérése</span><span class="sxs-lookup"><span data-stu-id="d55c1-144">Get hello latest Azure storage rules</span></span>

<span data-ttu-id="d55c1-145">toohello legújabb az Azure storage szabályok, futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="d55c1-145">toohello latest Azure storage rules, run th following commands:</span></span>

    # sudo curl -o /etc/udev/rules.d/66-azure-storage.rules https://raw.githubusercontent.com/Azure/WALinuxAgent/master/config/66-azure-storage.rules
    # sudo udevadm trigger --subsystem-match=block


<span data-ttu-id="d55c1-146">További információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="d55c1-146">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="d55c1-147">Ubuntu: UUID használatával</span><span class="sxs-lookup"><span data-stu-id="d55c1-147">Ubuntu: Using UUID</span></span>](https://help.ubuntu.com/community/UsingUUID)

- [<span data-ttu-id="d55c1-148">Red Hat: Állandó elnevezése</span><span class="sxs-lookup"><span data-stu-id="d55c1-148">Red Hat: Persistent Naming</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Storage_Administration_Guide/persistent_naming.html)

- [<span data-ttu-id="d55c1-149">Linux: UUID-k mit tehetnek meg</span><span class="sxs-lookup"><span data-stu-id="d55c1-149">Linux: What UUIDs can do for you</span></span>](https://www.linux.com/news/what-uuids-can-do-you)

- [<span data-ttu-id="d55c1-150">Udev: Bevezetés tooDevice felügyelet a Modern Linux rendszer</span><span class="sxs-lookup"><span data-stu-id="d55c1-150">Udev: Introduction tooDevice Management In Modern Linux System</span></span>](https://www.linux.com/news/udev-introduction-device-management-modern-linux-system)

