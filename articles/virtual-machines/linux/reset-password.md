---
title: "aaaHow tooreset helyi Linux jelszó Azure virtuális gépeken |} Microsoft Docs"
description: "Bevezetni hello lépéseket tooreset hello helyi Linux jelszó Azure virtuális gépen"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: delhan
ms.openlocfilehash: b28a679a36bf93c6881633eefa03aef3cd33e804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-linux-password-on-azure-vms"></a><span data-ttu-id="1554d-103">Hogyan tooreset helyi Linux jelszó Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="1554d-103">How tooreset local Linux password on Azure VMs</span></span>

<span data-ttu-id="1554d-104">Ez a cikk több módszerek tooreset helyi Linux virtuális gép (VM) jelszavak be.</span><span class="sxs-lookup"><span data-stu-id="1554d-104">This article introduces several methods tooreset local Linux Virtual Machine (VM) passwords.</span></span> <span data-ttu-id="1554d-105">Ha hello felhasználói fiók lejárt vagy csak toocreate egy új fiókot, hello a következő módszerek toocreate egy új helyi rendszergazdai fiókot használja, és visszaszerezzék az access toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1554d-105">If hello user account is expired or you just want toocreate a new account, you can use hello following methods toocreate a new local admin account and re-gain access toohello VM.</span></span>

## <a name="symptoms"></a><span data-ttu-id="1554d-106">Probléma</span><span class="sxs-lookup"><span data-stu-id="1554d-106">Symptoms</span></span>

<span data-ttu-id="1554d-107">Nem jelentkezik be a virtuális gép toohello, és megjelenik egy üzenet, amely jelzi, hogy a használt hello jelszó nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="1554d-107">You can't log in toohello VM, and you receive a message that indicates that hello password that you used is incorrect.</span></span> <span data-ttu-id="1554d-108">Ezenkívül nem hajtható végre tooreset vmagent esetében a jelszó hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="1554d-108">Additionally, you can't use VMAgent tooreset your password on hello Azure Portal.</span></span> 

## <a name="manual-password-reset-procedure"></a><span data-ttu-id="1554d-109">Manuális jelszó-átállítási eljárás</span><span class="sxs-lookup"><span data-stu-id="1554d-109">Manual Password Reset procedure</span></span>

1.  <span data-ttu-id="1554d-110">Törölje a virtuális gép hello, és láthatóan tartja hello csatlakoztatott lemezeket.</span><span class="sxs-lookup"><span data-stu-id="1554d-110">Delete hello VM and keep hello attached disks.</span></span>

2.  <span data-ttu-id="1554d-111">Hello adatok lemez tooanother meghajtón az operációs rendszer csatolása hello ideiglenes virtuális gép ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="1554d-111">Attach hello OS Drive as a data disk tooanother temporal VM in hello same location.</span></span>

3.  <span data-ttu-id="1554d-112">Futtassa az SSH-parancstól követően hello historikus VM toobecome hello felettes felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1554d-112">Run hello following SSH command on hello temporal VM toobecome a super-user.</span></span>


    ~~~~
    sudo su
    ~~~~

4.  <span data-ttu-id="1554d-113">Futtatás **fdisk -l** vagy a rendszer naplókat toofind hello pillantást újonnan csatolt lemezen.</span><span class="sxs-lookup"><span data-stu-id="1554d-113">Run **fdisk -l** or look at system logs toofind hello newly attached disk.</span></span> <span data-ttu-id="1554d-114">Keresse meg a hello meghajtó neve toomount.</span><span class="sxs-lookup"><span data-stu-id="1554d-114">Locate hello drive name toomount.</span></span> <span data-ttu-id="1554d-115">Ezután a hello historikus VM, tekintse meg a megfelelő hello naplófájl.</span><span class="sxs-lookup"><span data-stu-id="1554d-115">Then on hello temporal VM, look in hello relevant log file.</span></span>

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    <span data-ttu-id="1554d-116">hello az alábbiakban látható egy példa a kimenetre hello grep parancs:</span><span class="sxs-lookup"><span data-stu-id="1554d-116">hello following is example output of hello grep command:</span></span>

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  <span data-ttu-id="1554d-117">Hozzon létre egy csatlakoztatási pontot nevű **tempmount**.</span><span class="sxs-lookup"><span data-stu-id="1554d-117">Create a mount point called **tempmount**.</span></span>

    ~~~~
    mkdir /tempmount
    ~~~~

6.  <span data-ttu-id="1554d-118">Az operációs rendszer hello lemez csatlakoztatása hello csatlakoztatási pontot.</span><span class="sxs-lookup"><span data-stu-id="1554d-118">Mount hello OS disk on hello mount point.</span></span> <span data-ttu-id="1554d-119">Általában toomount sdc1 vagy sdc2 szükséges.</span><span class="sxs-lookup"><span data-stu-id="1554d-119">You usually need toomount sdc1 or sdc2.</span></span> <span data-ttu-id="1554d-120">A partíció hello nem működő számítógép lemezéről etc könyvtárban üzemeltető hello függ.</span><span class="sxs-lookup"><span data-stu-id="1554d-120">This will depend on hello hosting partition in /etc directory from hello broken machine disk.</span></span>

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  <span data-ttu-id="1554d-121">Készítsen biztonsági másolatot a módosítások végrehajtása előtt:</span><span class="sxs-lookup"><span data-stu-id="1554d-121">Perform a backup before making any changes:</span></span>

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  <span data-ttu-id="1554d-122">Alaphelyzetbe állítja a hello jelszó szükséges:</span><span class="sxs-lookup"><span data-stu-id="1554d-122">Reset hello user’s password that you need:</span></span>

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  <span data-ttu-id="1554d-123">Áthelyezési hello módosított fájlok toohello megfelelő helyére hello hibás gép lemezén tárolnia.</span><span class="sxs-lookup"><span data-stu-id="1554d-123">Move hello modified files toohello correct location on hello broken machine's disk.</span></span>

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back toohello root and unmount hello disk.

    ~~~~
    <span data-ttu-id="1554d-124">CD / umount /tempmount</span><span class="sxs-lookup"><span data-stu-id="1554d-124">cd / umount /tempmount</span></span>
    ~~~~

11. Detach hello disk from hello management portal.

12. Recreate hello VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk tooanother Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How toodelete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
