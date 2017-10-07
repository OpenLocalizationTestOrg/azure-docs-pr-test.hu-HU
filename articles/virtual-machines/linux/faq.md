---
title: "Linux virtuális gépek Azure-ban kérdések aaaFrequently |} Microsoft Docs"
description: "Itt választ toosome hello hello Resource Manager modellt létrehozott Linux virtuális gépek kapcsolatos gyakori kérdéseket."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: 0afd08123dddc408851065c46deedc3146dbec20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Linux virtuális gépek kapcsolatos gyakran ismételt kérdések
Ez a cikk foglalkozik az Azure-ban hello Resource Manager üzembe helyezési modellben létrehozott Linux virtuális gépek kapcsolatos gyakori kérdésekre. Ez a témakör hello Windows verziója: [gyakran feltett kérdés kapcsolatban a Windows virtuális gépek](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mit futtathatok egy Azure-beli virtuális gépen?
Minden előfizető kiszolgálószoftvereket futtathat az Azure-beli virtuális gépeken. További információkért lásd: [Azure-Endorsed Terjesztéseket Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Mennyi tárhelyet használhatok egy virtuális gép esetén?
Minden egyes adatlemez mentése too1 TB lehet. adatlemezek használhatja hello száma hello hello virtuális gép méretétől függ. Részletek: [Virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure-tárfiók tárolási biztosít hello operációsrendszer-lemez és adatlemezeket. Minden lemez egy lapblobként tárolt .vhd-fájl. A díjszabás részleteiért lásd [a Storage szolgáltatás díjszabását](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Hogyan lehet fér hozzá a virtuális géphez?
A távoli kapcsolat toolog toohello virtuális gépen, Secure Shell (SSH) használatával hoz létre. Hello utasításokat meg, hogyan tooconnect [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [a Linux és Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Alapértelmezés szerint az SSH legfeljebb 10 párhuzamos kapcsolatot tesz lehetővé. Ezt a számot növelheti hello konfigurációs fájl szerkesztésével.

Ha problémákat tapasztal, tekintse meg [hibaelhárítása Secure Shell (SSH) kapcsolatok](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-hello-temporary-disk-devsdb1-toostore-data"></a>Hello mennyiségű ideiglenes lemezes (/ dev/sdb1) toostore adatok használata
Ne használjon hello ideiglenes (/ dev/sdb1) toostore lemezadatokat. Éppen csak átmeneti tárolására. Veszhetnek el, amelyek nem állíthatók vissza adatokat.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Másolja vagy egy meglévő Azure virtuális gép klónozása?
Igen. Útmutatásért lásd: [hogyan egy-egy példányát a Linux virtuális gépek toocreate hello Resource Manager üzembe helyezési modellben](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miért nem jelenik meg Kanada központi és Kanada keleti régiók Azure Resource Manageren keresztül?
hello két új régió Kanada központi és Kanada keleti nem automatikusan regisztrálva van a virtuális gép létrehozásához a meglévő Azure-előfizetések. Ez a regisztráció automatikusan történik, amikor egy virtuális gép telepítve van a hello Azure portál tooany más régióban, Azure Resource Manager használatával. A virtuális gépek után van telepített tooany más Azure-régió, hello új régiók elérhetőknek kell lenniük a következő virtuális gépek.

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a>Vehetek fel egy hálózati adapter toomy virtuális gép létrehozása után?
Igen, ez a lehetőség most. hello virtuális gép első igények toobe felszabadított leállt. Ezt követően adja hozzá, vagy távolítsa el a hálózati adapter (kivéve, ha utolsó hálózati adapterén hello VM hello). 

## <a name="are-there-any-computer-name-requirements"></a>Vannak-e a számítógép neve követelmények?
Igen. hello számítógép neve legfeljebb 64 karakter hosszúságú lehet. Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt az erőforrások elnevezési körül.

## <a name="are-there-any-resource-group-name-requirements"></a>Vannak-e valamely erőforrás csoport vonatkozó követelményeknek?
Igen. hello az erőforráscsoport neve legfeljebb 90 karakter hosszúságú lehet. Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforráscsoportok további információt.

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a>Virtuális gép létrehozásakor Mik hello username követelmények?
Felhasználónevek 1 – 64 karakter hosszúságúnak kell lennie.

a következő felhasználónevek hello nem engedélyezettek:

<table>
    <tr>
        <td style="text-align:center">Rendszergazda </td><td style="text-align:center"> Rendszergazda </td><td style="text-align:center"> Felhasználó </td><td style="text-align:center"> Felhasználó1</td>
    </tr>
    <tr>
        <td style="text-align:center">Teszt </td><td style="text-align:center"> Felhasználó2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> Felhasználó3</td>
    </tr>
    <tr>
        <td style="text-align:center">rendszergazda1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> egy</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">biztonsági mentés </td><td style="text-align:center"> Konzol </td><td style="text-align:center"> David </td><td style="text-align:center"> Vendég</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> Tulajdonos </td><td style="text-align:center"> legfelső szintű </td><td style="text-align:center"> kiszolgáló</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Támogatás </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Teszt2 </td><td style="text-align:center"> Teszt3 </td><td style="text-align:center"> Felhasználó4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-hello-password-requirements-when-creating-a-vm"></a>Virtuális gép létrehozásakor Mik hello jelszavakra vonatkozó követelmények?
Jelszavak 6 – 72 karakter hosszúságúnak kell és felel meg a 3, 4 összetettségi követelményeknek hello kívül:

* Alacsonyabb karaktereket
* Felső karaktereket
* Rendelkezik egy számjegy
* Rendelkezik egy speciális karaktert (reguláris kifejezéssel egyező [\W_])

a következő jelszavak hello nem engedélyezettek:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Jelszót!</td>
        <td style="text-align:center">Jelszó1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
