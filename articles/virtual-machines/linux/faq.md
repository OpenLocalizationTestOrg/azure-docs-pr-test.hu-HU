---
title: "Gyakori kérdések a Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "A Resource Manager modellt létrehozott Linux virtuális gépek kapcsolatos gyakori kérdésekre adott válaszok biztosít."
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
ms.openlocfilehash: 0e06d21bd0b6ef807f38e41dcd50c9cd715607a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Linux virtuális gépek kapcsolatos gyakran ismételt kérdések
Ez a cikk foglalkozik az Azure-ban a Resource Manager üzembe helyezési modellel létrehozott Linux virtuális gépek kapcsolatos gyakori kérdésekre. Ez a témakör a Windows verziója: [gyakran feltett kérdés kapcsolatban a Windows virtuális gépek](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mit futtathatok egy Azure-beli virtuális gépen?
Minden előfizető kiszolgálószoftvereket futtathat az Azure-beli virtuális gépeken. További információkért lásd: [Azure-Endorsed Terjesztéseket Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Mennyi tárhelyet használhatok egy virtuális gép esetén?
Minden adatlemez akár 1 TB méretű is lehet. A használható adatlemezek száma a virtuális gép méretétől függ. Részletek: [Virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Az Azure-tárfiók tárhelyet biztosít az operációsrendszer-lemez és bármely adatlemez számára. Minden lemez egy lapblobként tárolt .vhd-fájl. A díjszabás részleteiért lásd [a Storage szolgáltatás díjszabását](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Hogyan lehet fér hozzá a virtuális géphez?
Jelentkezzen be a virtuális gépet, a Secure Shell (SSH) használatával történő távoli kapcsolatot létesíteni. Kapcsolódás című [Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [a Linux és Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Alapértelmezés szerint az SSH legfeljebb 10 párhuzamos kapcsolatot tesz lehetővé. Ezt a számot a konfigurációs fájl szerkesztésével növelheti.

Ha problémákat tapasztal, tekintse meg [hibaelhárítása Secure Shell (SSH) kapcsolatok](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Az ideiglenes lemez (/ dev/sdb1) használatával tárolja az adatokat?
Ne használjon a mennyiségű ideiglenes lemezes (/ dev/sdb1) adatainak tárolásához. Éppen csak átmeneti tárolására. Veszhetnek el, amelyek nem állíthatók vissza adatokat.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Másolja vagy egy meglévő Azure virtuális gép klónozása?
Igen. Útmutatásért lásd: [létrehozása Linux virtuális gép másolatát a Resource Manager üzembe helyezési modellel](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miért nem jelenik meg Kanada központi és Kanada keleti régiók Azure Resource Manageren keresztül?
A két új régiók Kanada központi és Kanada keleti nem automatikusan regisztrálva van a virtuális gép létrehozásához a meglévő Azure-előfizetések. Ez a regisztráció automatikusan történik, amikor egy virtuális gép telepítése Azure Resource Manager használatával bármely más régióban az Azure portálon keresztül. Bármely más Azure-régió, hogy a virtuális gép telepítése után az új régiók elérhetőknek kell lenniük a következő virtuális gépek.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Hozzáadható egy hálózati Adaptert a virtuális gép létrehozása után?
Igen, ez a lehetőség most. A virtuális gép először meg kell állítani a felszabadított. Majd adja hozzá, vagy távolítsa el a hálózati adapter (kivéve, ha a virtuális gép utolsó hálózati adapter). 

## <a name="are-there-any-computer-name-requirements"></a>Vannak-e a számítógép neve követelmények?
Igen. A számítógép neve legfeljebb 64 karakter hosszúságú lehet. Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt az erőforrások elnevezési körül.

## <a name="are-there-any-resource-group-name-requirements"></a>Vannak-e valamely erőforrás csoport vonatkozó követelményeknek?
Igen. Az erőforráscsoport neve legfeljebb 90 karakter hosszúságú lehet. Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforráscsoportok további információt.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Virtuális gép létrehozásakor Mik a username követelmények?
Felhasználónevek 1 – 64 karakter hosszúságúnak kell lennie.

A következő felhasználónevek nem engedélyezettek:

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


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>A jelszavakra vonatkozó követelmények Mik azok a virtuális gép létrehozásakor?
Jelszavak 6 – 72 karakter hosszúságúnak kell és 3 kívül a következő 4 bonyolultsági megfeleljen:

* Alacsonyabb karaktereket
* Felső karaktereket
* Rendelkezik egy számjegy
* Rendelkezik egy speciális karaktert (reguláris kifejezéssel egyező [\W_])

A következő jelszavak használata nem megengedett:

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
