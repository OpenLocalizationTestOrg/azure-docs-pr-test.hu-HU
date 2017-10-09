---
title: "aaaAzure Security Center és a Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "További tudnivalók a Azure Windows virtuális gép az Azure Security Center biztonsági."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 238bf4e266a24a536d35dd647db6056ab39a1f1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Virtuális gép biztonsági figyelje az Azure Security Centerben

Az Azure Security Center segítségével, hogy lássák az Azure-erőforrás biztonsági eljárásokat. A Security Center kínál, integrált biztonsági figyelés. Ellenkező esetben szabályzatkezelést fenyegetések azonosítására képes. Ebben az oktatóanyagban további tudnivalók az Azure Security Center, és hogyan:
 
> [!div class="checklist"]
> * Adatgyűjtés beállítása
> * Biztonsági házirendek beállítása
> * Megtekintheti és konfigurációs állapotát kapcsolatos problémák megoldása
> * Tekintse át a fenyegetést észlelt  

## <a name="security-center-overview"></a>A Security Center áttekintése

A Security Center lehetséges a virtuális gép (VM) konfigurációs problémák azonosítja, és biztonsági fenyegetések megcélzott. Ezek közé tartozik a virtuális gépek hálózati biztonsági csoportok, a nem titkosított lemezek és a találgatásos Remote Desktop Protocol (RDP) támadások hiányoznak. hello Security Center irányítópultjának könnyen olvasható diagramokban hello információ látható.

tooaccess hello hello hello menüjében válassza az Azure-portálon a Security Center irányítópultján **Security Center**. Hello irányítópulton tekintse meg az Azure környezetben hello biztonsági állapotát, található az aktuális javaslatok számát, és hello fenyegetés riasztások aktuális állapotának megtekintése. Minden egyes magas szintű diagram toosee kibontásával további információkhoz juthat.

![Security Center irányítópultjának](./media/tutorial-azure-security/asc-dash.png)

A Security Center túllép adatok felderítési tooprovide javaslatok, hogy az észlelt problémákat. Például ha egy virtuális Gépet egy hálózati biztonsági csoport nélkül lett telepítve, a Security Center ajánlás olyan környezetekben, a javítási lépéseket, amelyek jeleníti meg. Automatikus javítási kap a Security Center hello kontextusában maradjanak.  

![Javaslatok](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Adatgyűjtés beállítása

Virtuális gép biztonsági beállításokat tud bejutni látható, meg kell tooset fel a Security Center adatgyűjtés. Ebbe beletartozik az adatok gyűjtésének bekapcsolása, és létre kell hozni egy az Azure storage-fiók toohold gyűjtött adatait. 

1. Az hello Security Center irányítópultján kattintson **biztonsági házirend**, majd válassza ki az előfizetését. 
2. A **adatgyűjtés**, jelölje be **a**.
3. toocreate a storage-fiók kiválasztása **válasszon tárfiókot**. Ezt követően válassza **OK**.
4. A hello **biztonsági házirend** panelen válassza **mentése**. 

hello Security Center adatok gyűjtemény ügynök majd telepítve van az összes virtuális gépet, és adatgyűjtés megkezdése. 

## <a name="set-up-a-security-policy"></a>A biztonsági házirendben

Biztonsági házirendek tartoznak használt toodefine hello elemek, amelynek a Security Center adatokat gyűjt, és ajánlásokat. Különböző biztonsági házirendek toodifferent beállítása az Azure-erőforrások is alkalmazhatja. Bár az alapértelmezés szerint minden házirendelemek szemben Azure-erőforrások értékelődnek ki, kikapcsolhatja az összes Azure-erőforrások vagy erőforráscsoport egyedi házirend elemek. A Security Center biztonsági házirendekkel kapcsolatos részletesebb információk: [biztonsági házirendek beállítása az Azure Security Centerben](../../security-center/security-center-policies.md). 

tooset be egy biztonsági házirendet, az összes Azure-erőforrások:

1. A Security Center irányítópultjának hello, jelölje be **biztonsági házirend**, majd válassza ki az előfizetését.
2. Válassza ki **megakadályozási szabályzat**.
3. Kapcsolja be, vagy kapcsolja ki a megjeleníteni kívánt tooapply tooall házirendelemek Azure-erőforrások.
4. Amikor elkészült, válassza a beállítások, válassza ki a **OK**.
5. A hello **biztonsági házirend** panelen válassza **mentése**. 

a megadott erőforráscsoport-házirend tooset:

1. A Security Center irányítópultjának hello, jelölje be **biztonsági házirend**, majd válassza ki egy erőforráscsoportot.
2. Válassza ki **megakadályozási szabályzat**.
3. Kapcsolja be, vagy kapcsolja ki a házirend elemek megjeleníteni kívánt tooapply toohello erőforráscsoportot.
4. A **ÖRÖKLÉSI**, jelölje be **egyedi**.
5. Amikor elkészült, válassza a beállítások, válassza ki a **OK**.
6. A hello **biztonsági házirend** panelen válassza **mentése**.  

Ön is bármikor kikapcsolhatják az adatgyűjtést ezen a lapon megadott erőforráscsoport.

A következő példa hello, egyedi házirendet létrejött nevű erőforráscsoport *myResoureGroup*. Ezt a házirendet, a lemez titkosítása és a webes alkalmazás tűzfal javaslatok ki vannak kapcsolva.

![Egyedi házirend](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Virtuális gép konfigurációs állapotának megtekintése

Miután engedélyezve van a használatra vonatkozó adatok gyűjtésének és állíthat be a biztonsági házirendet, a Security Center tooprovide riasztások és javaslatok kezdődik. Virtuális gépek vannak telepítve, mint hello adatváltozások gyűjtési ügynök telepítve van. A Security Center majd fel van töltve a hello adatokkal új virtuális gépeket. Részletes információ a virtuális gép konfigurációs állapotát: [a Security Center a virtuális gépek védelme](../../security-center/security-center-virtual-machine-recommendations.md). 

Mivel összegyűjtött adatok hello erőforrás állapota minden egyes virtuális gép és a kapcsolódó Azure-erőforrás összesíti. hello információk könnyen olvasható diagramon látható. 

tooview erőforrás állapota:

1.  A Security Center irányítópultján hello alatt **erőforrás biztonsági állapota**, jelölje be **számítási**. 
2.  A hello **számítási** panelen válassza **virtuális gépek**. Ez a nézet a virtuális gépek hello konfigurációs állapotának összegzését tartalmazza.

![Számítási állapota](./media/tutorial-azure-security/compute-health.png)

toosee ajánlások a virtuális gépek, válassza ki a hello virtuális gép. Javaslatok, a javítási hello Ez az oktatóanyag következő szakasza részletesen ismertetnek.

## <a name="remediate-configuration-issues"></a>Konfigurációs problémák megoldásához

A Security Center toopopulate konfigurációs adataival megkezdése után javaslatok alapján készülnek hello biztonsági házirend beállítása. Például ha egy virtuális Gépet egy társított hálózati biztonsági csoport nélkül hozták létre, egy javaslatokkal egy toocreate. 

toosee összes ajánlás listája: 

1. A Security Center irányítópultjának hello, jelölje be **javaslatok**.
2. Válassza ki az adott javaslat. Megjelenik egy lista, amelynek erőforrásait hello ajánlás vonatkozik.
3. tooapply ajánlás olyan környezetekben, válassza ki az adott erőforrás. 
4. Hello követésével javítási lépéseket. 

Sok esetben a Security Center lépéseit végrehajthatóként készíthet tooaddress ajánlást Security Center maradjanak. A következő példa hello a Security Center észleli a hálózati biztonsági csoport, amely rendelkezik egy korlátlan bejövő szabályt. Hello javaslat oldalon kiválaszthatja hello **bejövő szabályok szerkesztése** gombra. felhasználói felület, amely szükséges toomodify hello szabály hello jelenik meg. 

![Javaslatok](./media/tutorial-azure-security/remediation.png)

Javaslatok szervizelt vannak, mint megoldottként vannak beállítva. 

## <a name="view-detected-threats"></a>Észlelt fenyegetéseket megtekintése

Továbbá a tooresource konfigurációs javaslatait a Security Center figyelmeztetések jeleníti meg. hello biztonsági riasztások szolgáltatás összesíti az egyes virtuális gép, Azure hálózati naplók és összekapcsolt partneri megoldások toodetect biztonsági fenyegetések ellen Azure-erőforrások gyűjtött adatokat. A Security Center threat detection képességeivel kapcsolatos részletesebb információk: [az Azure Security Center az észlelési képességek](../../security-center/security-center-detection-capabilities.md).

hello biztonsági riasztások szolgáltatáshoz szükséges hello Security Center árképzési szint toobe közötti *szabad* túl*szabványos*. Egy 30 napos **ingyenes próbaverzió** magasabb tarifacsomagra toothis áthelyezése esetén érhető el. 

toochange hello árképzési szint:  

1. Az hello Security Center irányítópultján kattintson **biztonsági házirend**, majd válassza ki az előfizetését.
2. Válassza ki **tarifacsomag**.
3. Új réteg hello, majd válassza ki és **válasszon**.
4. A hello **biztonsági házirend** panelen válassza **mentése**. 

Miután megváltoztatta a hello IP-címek, hello biztonsági riasztások graph indul toopopulate biztonsági fenyegetések észlelése.

![Biztonsági riasztások](./media/tutorial-azure-security/security-alerts.png)

Válassza ki a riasztási tooview információ. Például hello fenyegetés, hello észlelés ideje, az összes fenyegetés kísérletek leírása, és ajánlott javítási hello. A következő példa hello RDP találgatásos támadás volt észlelhető, 294 sikertelen RDP próbálnak. Javasolt megoldás valósul meg.

![RDP-támadás](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban az Azure Security Center beállítása, és ezután tekintse át a virtuális gépek a Security Center. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Adatgyűjtés beállítása
> * Biztonsági házirendek beállítása
> * Megtekintheti és konfigurációs állapotát kapcsolatos problémák megoldása
> * Tekintse át a fenyegetést észlelt

Előzetes toohello következő útmutató toolearn toocreate CI/CD-ről a Visual Studio Team Services csővezeték- és IIS-t futtató Windows virtuális gép.

> [!div class="nextstepaction"]
> [A Visual Studio Team Services CI/CD-folyamat](./tutorial-vsts-iis-cicd.md)
