---
title: "aaaImpactful karbantartása, a Windows-alapú virtuális gépek Azure-ban |} Microsoft Docs"
description: "Windows virtuális gépek impactful karbantartása."
services: virtual-machines-windows
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: 98afaea0fdca796177e075b33615b03f1e7a0fdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="impactful-maintenance-for-virtual-machines"></a>A virtuális gépek impactful karbantartás

Van néhány olyan esetekben, ahol az alapul szolgáló infrastruktúra tooplanned karbantartási toohello miatt a virtuális gép újraindítása után. Alatt a virtuális gépek Azure-ban üzemeltetett impactful toothe rendelkezésre állását, hello következő is elérhető az Ön toouse:

-   Legalább 30 nappal előtt hello hatás küld.

-   Láthatóság toohello karbantartási időszakok minden egyes virtuális gépenként.

-   Rugalmasság és a vezérlés beállítása hello pontos idő karbantartás hatással lehet a virtuális gépek számára.

A Microsoft Azure-ban karbantartási ismétlési van ütemezve. Kezdeti ismétlési rendelés tooreduce hello járulékos kockázatok, az új javítások és képességek a kisebb hatóköre lehet. Újabb ismétlési több régióba is span (hello a soha nem ugyanabban a régióban pár). A virtuális gép egy karbantartási egyetlen munkamenetben tartalmazza. Egy iteráció meghiúsul, ha egy másik, a jövőbeli, iterációs fennmaradó virtuális gépek jelennek meg.

hello tervezett karbantartás iterációs két szakasza van: Pre-emptive karbantartási időszakon kívül ütemezett-karbantartási időszakot.

Hello **Pre-emptive karbantartási időszak** biztosít hello rugalmasságot tooinitiate hello karbantartási a virtuális gépeken. Ezzel a módszerrel határozza meg, ha a virtuális gépek érintettek, hello hello frissítés és hello idő között az egyes virtuális gépek rendelkezésre sorozata. Minden virtuális gép toosee lekérdezni, hogy azt tervezett karbantartás, és ellenőrizze az utolsó kezdeményezett karbantartási kérése hello eredményét.

Hello **ütemezett karbantartási időszak** Azure ütemezett karbantartás hello a virtuális gépek esetén. Időablak, a következő preemptív karbantartási időszak, amely során továbbra is kereshet hello karbantartási időszak, de már nem képes tooorchestrate hello karbantartási.

## <a name="availability-considerations-during-planned-maintenance"></a>Tervezett karbantartás során a rendelkezésre állási lehetőségekért 

### <a name="paired-regions"></a>Párhuzamos régiók

Minden Azure-régió, egy másik régióban belül hello párosított azonos földrajzi hely, egy regionális pár együtt elvégzése. Karbantartási végrehajtásakor Azure csak a hello virtuálisgép-példányok egy régió a kulcspár frissíti. Például hello északi középső Régiójában lévő virtuális gépek frissítésekor Azure nem fogja frissíteni a virtuális gépek déli középső Régiójában: hello ugyanannyi időt vesz igénybe. Ez egy másik időpontra lesz ütemezve a régiók közötti feladatátvétel vagy terheléselosztás érdekében. Azonban más régiókból, mint például a karbantartás alatt lehet Észak-Európa hello azonos időben USA keleti régiója is.
Tudjon meg többet az [az Azure-régió párok](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="single-instance-vms-vs-availability-set-or-vm-scale-set"></a>Egyetlen példány virtuális gépek vs. A rendelkezésre állási csoport vagy a Virtuálisgép-méretezési csoport

A munkaterhelések virtuális gépek használata az Azure-ban való telepítésekor hello virtuális gépek rendelkezésre állási készlet rendelés tooprovide magas rendelkezésre állású tooyour alkalmazás belül is létrehozhat. Ez a konfiguráció biztosítja, hogy szolgáltatáskimaradás vagy karbantartási események, legalább egy virtuális gép elérhető.

Egy rendelkezésre állási csoportot belül az egyes virtuális gépek too20 frissítési tartomány vannak elosztva. Tervezett karbantartás közben csak egyetlen frissítési tartományi egy adott időpontban van hatással. frissítési tartományok befolyásolja hello sorrendjét nem egymás után folytathatja tervezett karbantartás során. Egyetlen példány a virtuális gép (nem a rendelkezésre állási csoport része), nincs módja toopredict van, vagy felfedheti, amely, és hány virtuális gépek érintett együtt.

Virtuálisgép-méretezési készlet, amely lehetővé teszi az Azure számítási erőforrás toodeploy, és az azonos virtuális gépek egyetlen erőforrásként kezelésére.
hello méretezési tooan rendelkezésre állási csoport frissítési tartományok formájában hasonló garanciákat nyújt. 

A magas rendelkezésre állású virtuális gépek konfigurálásával kapcsolatos további információkért lásd: [ *hello a Windows virtuális gépek rendelkezésre állásának kezelése*](../linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="scheduled-events"></a>Ütemezett események

Az Azure metaadat-szolgáltatás lehetővé teszi a virtuális gép Azure-ban üzemeltetett toodiscover adatait. Ütemezett események, kitett hello kategóriák, felületek információhoz várható események (például újraindítás), az alkalmazás előkészítése őket, és korlátozza megszakítása.

Ütemezett eseményekkel kapcsolatos további információkért tekintse meg túl[Azure metaadat-szolgáltatás - ütemezett események](../virtual-machines-scheduled-events.md).

## <a name="maintenance-discovery-and-notifications"></a>Karbantartási felderítési és értesítések

Karbantartási ütemterv látható toocustomers egyes virtuális gépek hello szinten. Használhatja az Azure portál, a preemptív és ütemezett karbantartás Windows API-t, a PowerShell vagy a CLI tooquery. Ezenkívül várhatóan hello esetében (e-mail) értesítést kaphat, ha egy (vagy több) a virtuális gépek érintett hello folyamat során.

Preemptív karbantartási, mind az ütemezett karbantartási fázisban kezdő értesítést. Egy Azure-előfizetés egyetlen értesítést tooreceive várt. hello értesítésküldés toohello előfizetés rendszergazda és a társadminisztrátornak alapértelmezés szerint. A karbantartási értesítési hello célközönség is konfigurálhatja.

### <a name="view-hello-maintenance-window-in-hello-portal"></a>Nézet hello karbantartási időszak hello portálon 

Hello Azure-portált használja, és keresse meg a virtuális gépek karbantartás ütemezve.

1.  Bejelentkezés toohello Azure-portálon.

2.  Kattintson a gombra, majd nyissa meg a hello **virtuális gépek** panelen.

3.  Kattintson a hello **oszlopok** gomb tooopen hello az elérhető oszlopok toochoose listája

4.  Válassza ki, és adja hozzá a hello **karbantartási időszak** oszlopok. Virtuális gépek ütemezett karbantartás hello karbantartási időszakok illesztett rendelkezik. Miután karbantartás befejeződött, vagy meg lett szakítva egy, a karbantartási időszak van már nem jelenik meg.

### <a name="query-maintenance-details-using-hello-azure-api"></a>Lekérdezés karbantartás részletei hello Azure API használatával

Használjon hello [API Virtuálisgép-adatok beolvasása](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) , és keressen hello példány toodiscover hello karbantartási részleteinek megtekintése egy adott virtuális Gépre. hello válasz hello a következő elemeket tartalmazza:

  - isCustomerInitiatedMaintenanceAllowed: azt jelzi, hogy a virtuális gép hello preemptív helyezze üzembe újra most is kezdeményezhető.

  - preMaintenanceWindowStartTime: hello hello preemptív karbantartási időszak kezdő időpontja.

  - preMaintenanceWindowEndTime: hello befejezésének hello preemptív karbantartási időszaknál. Ezt követően már nem lesz képes tooinitiate karbantartási a virtuális Gépet.
    
  - maintenanceWindowStartTime: hello kezdési időpont hello ütemezett karbantartási időszaknál, amikor a virtuális gép érintett.

  - maintenanceWindowEndTime: hello befejezésének hello ütemezett karbantartási időszaknál.
  
  - lastOperationResultCode: hello az utolsó karbantartási-helyezze üzembe újra művelet eredménye.
 
  - lastOperationMessage: az utolsó karbantartási-helyezze üzembe újra műveletet hello eredményét leíró üzenet.

## <a name="pre-emptive-redeploy"></a>Preemptív helyezze üzembe újra

Preemptív helyezze üzembe újra művelet hello rugalmasságot toocontrol hello időpontot, amikor karbantartási alkalmazott tooyour virtuális gépeket az Azure biztosít. Tervezett karbantartás preemptív karbantartási időszak eldöntheti, ahol a pontos idő hello az egyes a virtuális gépek toobe újraindítása után kezdődik. A következők használatát esetben, ha ilyen funkció hasznos:

-   Karbantartási kell toobe koordinált hello end ügyféllel.

-   Azure által kínált hello ütemezett karbantartási időszaknál túl kicsi.
    Annak oka az lehet, hogy hello ablak toobe történik hello legforgalmasabb idővel a hét, vagy túl hosszú.

-   A többpéldányos vagy a többrétegű alkalmazások között két virtuális gép vagy egy bizonyos feladatütemezési toofollow elegendő időt kell.

A virtuális gép preemptív helyezze üzembe újra hívásakor hello VM tooan csomópont már megtörtént, és majd bekapcsolja azt vissza, a konfigurációs beállításokat és a kapcsolódó erőforrások megőrzése átvitel során. Ennek során hello mennyiségű ideiglenes lemezes elvész, és a dinamikus IP-címek társított virtuális hálózati adapter frissítése.

Preemptív helyezze üzembe újra virtuális gépenként egyszer engedélyezett. Hello folyamat során hiba történik, ha hello művelet leáll, hello virtuális gép nem változik, és ki van zárva a hello tervezett karbantartás iterációs. A rendszer kapcsolatba lépni a később egy új ütemezéssel rendelkező, majd egy új lehetőség tooschedule és a sorrend hello hatás érhető el a virtuális gépeken.