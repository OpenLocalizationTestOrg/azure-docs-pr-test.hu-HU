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
# <a name="how-tooreset-local-linux-password-on-azure-vms"></a>Hogyan tooreset helyi Linux jelszó Azure virtuális gépeken

Ez a cikk több módszerek tooreset helyi Linux virtuális gép (VM) jelszavak be. Ha hello felhasználói fiók lejárt vagy csak toocreate egy új fiókot, hello a következő módszerek toocreate egy új helyi rendszergazdai fiókot használja, és visszaszerezzék az access toohello virtuális gép.

## <a name="symptoms"></a>Probléma

Nem jelentkezik be a virtuális gép toohello, és megjelenik egy üzenet, amely jelzi, hogy a használt hello jelszó nem megfelelő. Ezenkívül nem hajtható végre tooreset vmagent esetében a jelszó hello Azure portálon. 

## <a name="manual-password-reset-procedure"></a>Manuális jelszó-átállítási eljárás

1.  Törölje a virtuális gép hello, és láthatóan tartja hello csatlakoztatott lemezeket.

2.  Hello adatok lemez tooanother meghajtón az operációs rendszer csatolása hello ideiglenes virtuális gép ugyanazon a helyen.

3.  Futtassa az SSH-parancstól követően hello historikus VM toobecome hello felettes felhasználó.


    ~~~~
    sudo su
    ~~~~

4.  Futtatás **fdisk -l** vagy a rendszer naplókat toofind hello pillantást újonnan csatolt lemezen. Keresse meg a hello meghajtó neve toomount. Ezután a hello historikus VM, tekintse meg a megfelelő hello naplófájl.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    hello az alábbiakban látható egy példa a kimenetre hello grep parancs:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Hozzon létre egy csatlakoztatási pontot nevű **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Az operációs rendszer hello lemez csatlakoztatása hello csatlakoztatási pontot. Általában toomount sdc1 vagy sdc2 szükséges. A partíció hello nem működő számítógép lemezéről etc könyvtárban üzemeltető hello függ.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Készítsen biztonsági másolatot a módosítások végrehajtása előtt:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Alaphelyzetbe állítja a hello jelszó szükséges:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Áthelyezési hello módosított fájlok toohello megfelelő helyére hello hibás gép lemezén tárolnia.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back toohello root and unmount hello disk.

    ~~~~
    CD / umount /tempmount
    ~~~~

11. Detach hello disk from hello management portal.

12. Recreate hello VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk tooanother Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How toodelete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
