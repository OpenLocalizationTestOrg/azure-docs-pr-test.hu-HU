---
title: "Windows virtuális gépekkel kapcsolatos problémákat, az Azure-ban telepítése aaaTroubleshoot |} Microsoft Docs"
description: "Problémamegoldás telepítését Windows virtuális gép Azurethe Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: genli
ms.openlocfilehash: 30de25827c050cc266761cfc14548bcc64237dac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Problémamegoldás telepítését Windows virtuális gép az Azure-ban

tootroubleshoot virtuális gép (VM) telepítési problémákat Azure, tekintse át a hello [leggyakoribb problémák](#top-issues) a gyakori hibák és megoldására.

Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.

## <a name="top-issues"></a>Problémák
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="hello-cluster-cannot-support-hello-requested-vm-size"></a>hello fürt nem támogatja a kért Virtuálisgép-méretet hello
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.
- Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:
    - Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek. Kattintson a **erőforráscsoportok** > az erőforráscsoport > **erőforrások** > a rendelkezésre állási csoport > **virtuális gépek** > a virtuális gép > **leállítása**.
    - Amikor az összes virtuális gépek leállítási hello, és hello VM szükséges hello mérete.
    - Indítsa el először hello új virtuális Gépet, és jelölje hello virtuális gépek leállítása, majd válassza az Indítás parancsot.


## <a name="hello-cluster-does-not-have-free-resources"></a>hello fürtnek nincs szabad erőforrást
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Próbálkozzon újra később hello kéréssel.
- Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani
    - Hozzon létre egy virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).
    - Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Hogyan használja és az Azure windows ügyfél lemezkép telepítéséhez?

Használhatja a Windows 7, Windows 8 vagy Windows 10 az Azure-ban fejlesztési/Tesztelési forgatókönyvek ha megfelelő (korábbi nevén MSDN) Visual Studio-előfizetéssel rendelkezik. Ez [cikk](client-images.md) vázol fel hello Windows-ügyféllel rendelkező Azure-ban és felhasználási területei hello Azure gyűjtemény lemezképei jogosultsági követelményei.

## <a name="how-can-i-deploy-a-virtual-machine-using-hello-hybrid-use-benefit-hub"></a>Hogyan telepítheti a virtuális gépek hello hibrid használata juttatás (HUB)?

Többféle különböző módokon toodeploy Windows virtuális gépek hello Azure hibrid használata juttatásra.

Egy nagyvállalati szerződés előfizetéshez:

• Az adott piactéren elérhető rendszerkép, amelyek az Azure hibrid használata juttatás előre konfigurált virtuális gépek telepítéséhez.

A nagyvállalati szerződés:

• Feltöltése egy egyéni virtuális Gépet, és telepítése a Resource Manager-sablon vagy Azure PowerShell segítségével.

További információkért tekintse meg a következő erőforrások hello:

 - [Az Azure hibrid használata juttatás áttekintése](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [Letölthető – gyakori kérdések](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Windows Server és a Windows ügyfél Azure hibrid használata juttatás](hybrid-use-benefit-licensing.md).

 - [Hogyan használhatom hello hibrid használja juttatás az Azure-ban](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hogyan aktiválhatja a Visual Studio Enterprise (BizSpark) a havi keretet

tooactivate a havi jóváírása, ez [cikk](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-tooadd-enterprise-devtest-toomy-enterprise-agreement-ea-tooget-access-toowindow-client-images"></a>Tooadd vállalati fejlesztési és tesztelési célú toomy nagyvállalati szerződés (EA) tooget miként férhetnek hozzá az tooWindow ügyfél képek?

hello képességét toocreate előfizetések hello vállalati fejlesztési és tesztelési célú alapján kínálnak engedélye toodo kezelhet korlátozott tooAccount tulajdonosai a vállalati rendszergazda igen. hello fiók tulajdonosának hello Azure fiók portálon keresztül előfizetések hoz létre, és kell adja aktív Visual Studio-előfizetők társadminisztrátorként. Így azok kezelése, és használja a fejlesztéshez és teszteléshez szükséges hello erőforrásokat. További információkért lásd: [vállalati fejlesztési és tesztelési célú](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>Az illesztőprogramok hiányoznak a Windows-N sorozatú virtuális Gépemhez

Illesztőprogramok Windows-alapú virtuális gépek találhatók [Itt](n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>A GPU példánya nem található a N sorozatú virtuális Gépen belül

tootake előny az Azure N sorozatú virtuális gépeket futtathatnak Windows Server 2016 hello GPU lehetőségeit vagy Windows Server 2012 R2, telepítenie kell NVIDIA video-illesztőprogramok a virtuális gépek telepítést követően. Illesztőprogram-telepítő információ [Windows virtuális gépek](n-series-driver-setup.md) és [Linux virtuális gépek](../linux/n-series-driver-setup.md).

## <a name="are-client-images-supported-for-n-series"></a>Ügyfél képek N-adatsorozathoz támogatottak?

Jelenleg Azure csak támogatja az N-sorozat a Windows Server és Linux operációs rendszert futtató virtuális gépeken.

## <a name="is-n-series-vms-available-in-my-region"></a>Érhető el N sorozatú virtuális gépek régiómban?

Ellenőrizheti a hello rendelkezésre biztosít hello [régió tábla által elérhető termékek](https://azure.microsoft.com/regions/services), és az árképzés terén [Itt](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-tooi-get-them"></a>Milyen ügyfél képek is I használata és telepítése az Azure-ban, és hogyan tooI be őket?

Használhatja a Windows 7, Windows 8 vagy Windows 10 fejlesztési és tesztelési célú forgatókönyvek az Azure-ban biztosított megfelelő (korábbi nevén MSDN) Visual Studio-előfizetéssel rendelkezik. 

- Windows 10-lemezképek elérhetők belül hello Azure gyűjteményből [jogosult fejlesztési és tesztelési célú kínál](client-images.md#eligible-offers). 
- A Visual Studio-előfizetők ajánlat bármilyen típusú belül is [megfelelően készítse elő és hozzon létre](prepare-for-upload-vhd-image.md) egy 64 bites Windows 7, Windows 8 vagy Windows 10-lemezképet, majd [tooAzure feltöltése](upload-generalized-managed.md). hello használata korlátozott toodev/test aktív Visual Studio-előfizetők által marad.

Ez [cikk](client-images.md) vázol fel hello jogosultsági követelményei Windows-ügyféllel rendelkező Azure-ban és hello használata Azure-katalógus képek.

## <a name="i-am-not-able-toosee-vm-size-family-that-i-want-when-resizing-my-vm"></a>A számítógépen nem tudja toosee Virtuálisgép-méretet termékcsalád, amelyek szeretnék, ha a virtuális gép átméretezésével.

Amikor egy virtuális gép fut, nem telepített tooa fizikai kiszolgálón. hello fizikai kiszolgálók Azure-régiók közös fizikai hardver fürtök vannak csoportosítva. A hello VM áthelyezése toobe toodifferent hardver fürtök igénylő virtuális gépek átméretezésével eltér attól függően, hogy melyik üzembe helyezési modellel használt toodeploy hello VM volt.

- Klasszikus üzembe helyezési modellel, hello felhő szolgáltatástelepítés telepített virtuális gépek el kell távolítani, és toochange hello virtuális gépek tooa mérete egy másik mérete termékcsalád újratelepíteni.

- Erőforrás-kezelő üzembe helyezési modellben telepített virtuális gépek, le kell állítania minden virtuális gép hello rendelkezésre állási csoportban, a virtuális gép hello rendelkezésre állási készlet hello méretének módosítása előtt.

## <a name="hello-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>hello felsorolt Virtuálisgép-méret nem támogatott a rendelkezésre állási csoport központi telepítése során.

Hello rendelkezésre állási csoport fürtön támogatott méret kiválasztása. Ajánlott létrehozásakor rendelkezésre állási készlet toochoose hello legnagyobb Virtuálisgép-méretet úgy gondolja, hogy kell, és rendelkezik, amelyek az első központi telepítési toohello rendelkezésre állási csoportban.

## <a name="can-i-add-an-existing-classic-vm-tooan-availability-set"></a>Adhat hozzá egy meglévő klasszikus virtuális gép tooan rendelkezésre állási csoportot?

Igen. Hozzáadhat egy meglévő klasszikus virtuális gép tooa új vagy meglévő rendelkezésre állási csoportban. További információ: [adja hozzá a virtuális gép tooan rendelkezésre állási készlet](classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Következő lépések
Ha ez a cikk bármely pontján további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/).

Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.
