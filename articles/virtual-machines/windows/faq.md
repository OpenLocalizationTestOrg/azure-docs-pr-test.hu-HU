---
title: "Windows-alapú virtuális gépek Azure-ban kapcsolatos aaaFAQ |} Microsoft Docs"
description: "Itt választ toosome hello hello Resource Manager modellt létrehozott Windows virtuális gépek kapcsolatos gyakori kérdéseket."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 757da816-a050-4889-a010-6f75d7978eb7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: cynthn
ms.openlocfilehash: ee366a04bda347ce2be07bde4fc6bad306cc1da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Windows virtuális gépek kapcsolatos gyakran ismételt kérdések
Ez a cikk foglalkozik Windows virtuális gépek létrehozása az Azure-ban hello Resource Manager üzembe helyezési modellben kapcsolatos gyakori kérdésekre. Ez a témakör hello Linux verziója: [gyakran feltett kérdés kapcsolatos Linux virtuális gépek](../linux/faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="what-can-i-run-on-an-azure-vm"></a>Mit futtathatok egy Azure-beli virtuális gépen?
Minden előfizető kiszolgálószoftvereket futtathat az Azure-beli virtuális gépeken. Az Azure-ban futó Microsoft server szoftver hello támogatási szabályzata kapcsolatos információkért lásd: [Microsoft server szoftver támogatása az Azure virtuális gépek](https://support.microsoft.com/kb/2721672)

A Windows 7, Windows 8.1 és Windows 10 egyes verziói elérhető tooMSDN Azure juttatás előfizetők és az MSDN fejlesztői és teszt – használatalapú fizetés-előfizetők fejlesztési és tesztelési feladatokhoz. Részletekért, többek között az utasításokért és korlátozásokért tekintse meg az [MSDN-előfizetők számára elérhető Windows-rendszerképeket](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) ismertető cikket. 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Mennyi tárhelyet használhatok egy virtuális gép esetén?
Minden egyes adatlemez mentése too1 TB lehet. adatlemezek használhatja hello száma hello hello virtuális gép méretétől függ. Részletek: [Virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Az Azure Managed lemezek hello új és ajánlott lemez tárolási ajánlatok használható Azure virtuális gépekkel állandó adatok tárolására. Egy-egy virtuális géppel több felügyelt lemez is használható. A Managed Disks kétféle tartós tárolási lehetőséget kínál: Premium és Standard szintű Managed Disks. Díjszabási információkért lásd: [lemezek árképzési felügyelt](https://azure.microsoft.com/pricing/details/managed-disks).

Az Azure storage-fiókok is biztosít tárolási hello operációsrendszer-lemez és adatlemezeket. Minden lemez egy lapblobként tárolt .vhd-fájl. A díjszabás részleteiért lásd [a Storage szolgáltatás díjszabását](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Hogyan lehet fér hozzá a virtuális géphez?
Távoli asztali kapcsolat (RDP) használatával a Windows virtuális gépek távoli kapcsolatot létesíteni. Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A két egyidejű kapcsolatok maximális támogatottak, kivéve, ha egy távoli asztali munkamenetgazda hello kiszolgáló van konfigurálva.  

Ha a távoli asztal problémákat tapasztal, tekintse meg [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Ha jártas a Hyper-V, akkor előfordulhat, hogy keres egy eszköz hasonló tooVMConnect. Azure nem kínál hasonló eszközt, mert a konzol hozzáférési tooa virtuális gép nem támogatott.

## <a name="can-i-use-hello-temporary-disk-hello-d-drive-by-default-toostore-data"></a>Hello mennyiségű ideiglenes lemezes (hello alapértelmezés szerint a d meghajtó) toostore adatok használata
Ne használjon hello ideiglenes toostore lemezadatokat. Értéke csak átmeneti tárolás és így lenne veszhetnek el, amelyek nem állíthatók vissza adatokat. Ha hello virtuális gép tooa másik gazdagépre helyezi előfordulhat adatvesztés. A virtuális gépek átméretezésével, hello állomás vagy hello gazdagépen hardverhiba frissítése közé tartoznak a hello okok miatt virtuális gépek áthelyezése előfordulhat, hogy.

Ha egy alkalmazás, amelyet a toouse hello D: meghajtóbetűjel, a meghajtó-betűjelek, hogy hello mennyiségű ideiglenes lemezes használja D: nem hozzárendelheti. Útmutatásért lásd: [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).


## <a name="how-can-i-change-hello-drive-letter-of-hello-temporary-disk"></a>Hogyan módosítható hello mennyiségű ideiglenes lemezes hello betűjele?
Hello meghajtóbetűjel által áthelyezése hello oldal fájlja és a meghajtó-betűjelek újbóli módosíthatja, de meg arról, hogy a meghatározott sorrendben lépéseket hello toomake van szüksége. Útmutatásért lásd: [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="can-i-add-an-existing-vm-tooan-availability-set"></a>Adhat hozzá egy meglévő virtuális gép tooan rendelkezésre állási csoportot?
Nem. Ha azt szeretné, hogy a virtuális gép toobe részei egy rendelkezésre állási csoportnak, toocreate hello VM halmazában hello kell. Jelenleg nem áll rendelkezésre egy módon tooadd egy virtuális gép tooan rendelkezésre állási csoport létrehozása után.

## <a name="can-i-upload-a-virtual-machine-tooazure"></a>Feltöltheti a virtuális gép tooAzure?
Igen. Útmutatásért lásd: [áttelepítése a helyszíni virtuális gépek tooAzure](on-prem-to-azure.md).

## <a name="can-i-resize-hello-os-disk"></a>Átméretezhetek hello operációsrendszer-lemez?
Igen. Útmutatásért lásd: [hogyan tooexpand hello az operációs rendszer meghajtóját, a virtuális gépen az Azure-erőforráscsoport](expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Másolja vagy egy meglévő Azure virtuális gép klónozása?
Igen. Felügyelt lemezképek használatával, hozzon létre egy virtuális gép lemezképét, és több új virtuális gépek hello kép toobuild használhatja. Útmutatásért lásd: [hozzon létre egy egyéni lemezképet, a virtuális gépek](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Miért nem jelenik meg Kanada központi és Kanada keleti régiók Azure Resource Manageren keresztül?

hello két új régió Kanada központi és Kanada keleti nem automatikusan regisztrálva van a virtuális gép létrehozásához a meglévő Azure-előfizetések. Ez a regisztráció automatikusan történik, amikor egy virtuális gép telepítve van a hello Azure portál tooany más régióban, Azure Resource Manager használatával. A virtuális gépek után van telepített tooany más Azure-régió, hello új régiók elérhetőknek kell lenniük a következő virtuális gépek.

## <a name="does-azure-support-linux-vms"></a>Támogatja az Azure Linux virtuális gépek?
Igen. tooquickly hozzon létre egy Linux virtuális gép tootry el, lásd: [Linux virtuális gép létrehozása az Azure-ban a portál hello](../linux/quick-create-portal.md).

## <a name="can-i-add-a-nic-toomy-vm-after-its-created"></a>Vehetek fel egy hálózati adapter toomy virtuális gép létrehozása után?
Igen, ez a lehetőség most. hello virtuális gép első igények toobe felszabadított leállt. Ezt követően adja hozzá, vagy távolítsa el a hálózati adapter (kivéve, ha utolsó hálózati adapterén hello VM hello). 

## <a name="are-there-any-computer-name-requirements"></a>Vannak-e a számítógép neve követelmények?
Igen. hello számítógép neve legfeljebb 15 karakter hosszúságú lehet. Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) további információt az erőforrások elnevezési körül.

## <a name="are-there-any-resource-group-name-requirements"></a>Vannak-e valamely erőforrás csoport vonatkozó követelményeknek?
Igen. hello az erőforráscsoport neve legfeljebb 90 karakter hosszúságú lehet. Lásd: [elnevezési konvenciókat szabályokat és korlátozásokat](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) erőforráscsoportok további információt.

## <a name="what-are-hello-username-requirements-when-creating-a-vm"></a>Virtuális gép létrehozásakor Mik hello username követelmények?

Felhasználónevek legfeljebb 20 karakter hosszúságú lehet, és nem végződhet pontra ("."). 


a következő felhasználónevek hello nem engedélyezettek:
<table>
    <tr>
        <td style="text-align:center">Rendszergazda </td><td style="text-align:center"> Rendszergazda </td><td style="text-align:center"> Felhasználó </td><td style="text-align:center"> Felhasználó1</td>
    </tr>
    <tr>
        <td style="text-align:center">Teszt </td><td style="text-align:center"> Felhasználó2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> Felhasználó3</td>
    </tr>    <tr>
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
Jelszavak 12 – 123 karakter hosszúságúnak kell és felel meg a 3, 4 összetettségi követelményeknek hello kívül:

* Alacsonyabb karaktereket
* Felső karaktereket
* Rendelkezik egy számjegy
* Rendelkezik egy speciális karaktert (reguláris kifejezéssel egyező [\W_])

a következő jelszavak hello nem engedélyezettek:

<table>
    <tr>
        <td>abc@123 </td>
        <td>P@$$w0rd </td>
        <td>P@ssw0rd </td>
        <td>P@ssword123 </td>
        <td>Pa$ $word </td>
    </tr>
    <tr>
        <td>pass@word1 </td>
        <td>Jelszót! </td>
        <td>Jelszó1 </td>
        <td>Password22 </td>
        <td>ILOVEYOU! </td>
    </tr>
</table>
