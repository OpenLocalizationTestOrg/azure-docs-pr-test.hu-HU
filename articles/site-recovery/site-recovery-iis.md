---
title: "a többrétegű IIS aaaReplicate alapú webalkalmazását az Azure Site Recovery használatával |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate IIS webkiszolgáló farm virtuális gépek Azure Site Recovery segítségével."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 1974265b3cb05f6dc57049876306d2e08424bb97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Azure Site Recovery segítségével többrétegű IIS-alapú webes alkalmazás replikálása

## <a name="overview"></a>Áttekintés


Alkalmazás szoftvere hello motorja egy szervezet üzleti hatékonyságot. Különböző webalkalmazások más célt szolgálhat egy szervezet. Lehet, például a feldolgozását, pénzügyi alkalmazásokat és ügyfelek által használt webhelyek ezek némelyike a lehető legnagyobb mértékben kritikus végzi egy szervezet számára. Szüksége lesz a hello szervezet toohave fel őket és a termelékenység mindig tooprevent veszteséggel fut, és megakadályozzák fontosabb bármely sérülést toohello márka kép hello szervezet.

Kritikus webalkalmazások általában úgy vannak beállítva, mint hello webes, az adatbázis és a különböző rétegek alkalmazás Többrétegű alkalmazások. Különböző rétegek elosztva alatt, leszámítva hello alkalmazások lehet, hogy is használja több kiszolgáló minden egyes réteg tooload egyenleg hello forgalom. Ezenkívül hello hozzárendelések különböző rétegek közötti és hello webkiszolgálón alapul statikus IP-címeket. Feladatátvétel esetén a leképezések némelyike kell toobe frissítve, különösen akkor, ha több webhely hello webkiszolgálón konfigurálva van. Esetén a webalkalmazások SSL használatával tanúsítványok kötései toobe frissíteni kell.

Hagyományos nem replikációs alapján helyreállítási módszerek tartalmaz, amely különböző konfigurációs fájlt, beállításjegyzék-beállítások, kötések, egyéni összetevők (COM vagy .NET), tartalom és is tanúsítványok és manuális lépések keresztül helyreállított hello fájlok biztonsági mentése. Ezek a technológiák egyértelműen nehézkes, amelyek nagyon eséllyel fordulnak elő, és nem méretezhető hiba. Például könnyen lehetséges, hogy tooforget, tanúsítványok biztonsági mentése és hagyható nem választott, de toobuy új hello a kiszolgálóhoz tartozó tanúsítványait a feladatátvételt követően.

Egy jó vész-helyreállítási megoldást kell modellezési helyreállítási tervek hello körül fent összetett alkalmazási architektúrákban engedélyezése és hello teszi testre szabott tooadd lépéseket toohandle alkalmazástársítások ezért biztosító különböző rétegek közötti is rendelkeznek egy egy kattintással meg arról, hogy megoldás létrehozása a hello eseményeket, ami tooa katasztrófa RTO csökkentéséhez.


Ez a cikk ismerteti, hogyan tooprotect az IIS alapú webes alkalmazás használja a [Azure Site Recovery](site-recovery-overview.md). Ez a cikk az IIS-alapú webes alkalmazás tooAzure, hogyan teheti a vész-helyreállítási részletezéshez és feladatátvételi hello alkalmazás tooAzure adhat szakaszában gyakorlati tanácsok a három réteg replikálására.


## <a name="prerequisites"></a>Előfeltételek

Megkezdése előtt győződjön meg arról, hogy hello következő:

1. [A virtuális gép tooAzure replikálása](site-recovery-vmware-to-azure.md)
1. Hogyan túl[egy helyreállítási hálózathoz. terv](site-recovery-network-design.md)
1. [Ez a teszt feladatátvételi tooAzure](./site-recovery-test-failover-to-azure.md)
1. [Ez a feladatátvételi tooAzure](site-recovery-failover.md)
1. Hogyan túl[tartományvezérlő replikálása](site-recovery-active-directory.md)
1. Hogyan túl[SQL-kiszolgáló replikálása](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Központi telepítés minták
Az IIS-alapú webalkalmazás általában a következő a következő telepítési környezeteken hello egyikét:

** Telepítési minta 1 ** az IIS webfarm alkalmazás kérelem Routing(ARR), az IIS-kiszolgáló és a Microsoft SQL Server alapú.

![Telepítési minta](./media/site-recovery-iis/deployment-pattern1.png)

**Telepítési minta 2** az IIS webfarm alkalmazás kérelem Routing(ARR), IIS-kiszolgálót, kiszolgáló és a Microsoft SQL Server alapú.


![Telepítési minta](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Webhely-helyreállítási támogatás

Ez a cikk VMware virtuális gépek létrehozása az IIS 7.5-ös verziója a Windows Server 2012 R2 Enterprise kiszolgálóval hello célra használták. Mivel helyreállítási helyreplikálásának alkalmazás független, hello javaslatok itt megadott várható toohold, valamint a következő forgatókönyvek és az IIS más verzióját.

### <a name="source-and-target"></a>Forrása és célja

**A forgatókönyv** | **másodlagos hely tooa** | **tooAzure**
--- | --- | ---
**Hyper-V** | Igen | Igen
**VMware** | Igen | Igen
**Fizikai kiszolgáló** | Nem | Igen

## <a name="replicate-virtual-machines"></a>Virtuális gépek replikálása

Hajtsa végre a [Ez az útmutató](site-recovery-vmware-to-azure.md) toostart összes hello az IIS webkiszolgáló farm virtuális gépek tooAzure replikálása.

Ha statikus IP-címet használ, akkor adja meg a használni kívánt virtuális gép tootake a hello hello hello IP [ **cél IP-címet** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) a számítási és hálózati beállításainak megadása.

![Cél IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Helyreállítási terv létrehozása

A helyreállítási terv lehetővé teszi, hogy a különböző rétegek egy többrétegű alkalmazást, ezért az alkalmazás konzisztencia fenntartása hello feladatátvételi sorrendje. Hajtsa végre a következő lépések hello többrétegű webalkalmazás esetén a helyreállítási terv létrehozása során.  [További információ a helyreállítási terv létrehozása](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-toofailover-groups"></a>Virtuális gépek toofailover csoportok hozzáadása
Egy tipikus többrétegű IIS-webalkalmazás olyan virtuális gépek, hello webes réteg által egy IIS-kiszolgálót és egy alkalmazás réteg SQL adatbázis-rétegből állnak. Minden e virtuális gépek toodifferent csoport hozzáadása alatt réteg alapján. [További információ a helyreállítási terv customising](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Helyreállítási terv létrehozása. Adja hozzá az, hogy azok leállítási utolsó vagy első felvet csoport 1 tooensure hello adatbázis réteg virtuális gépeket.

1. Adja hozzá a hello alkalmazás réteg virtuális gépein csoport 2 úgy, hogy azok kerülnek sorra, miután hello adatbázis-rétegből állapotba nem kerül.

1. Hello webes réteg virtuális gépek hozzáadása csoport 3 úgy, hogy azok kerülnek sorra, miután üzembe hello alkalmazásszinten jelentkezett.

1. Load balance virtuális gépek hozzáadása csoport 4 úgy, hogy azok kerülnek sorra, miután hello webes réteg leállását.


### <a name="adding-scripts-toohello-recovery-plan"></a>A parancsfájlok toohello helyreállítási terv hozzáadása
Szükség lehet toodo bizonyos műveletek hello Azure virtuális gépek post feladatátvételi és tesztelési célú feladatátvevő toomake IIS webkiszolgáló farm függvény megfelelően. Automatizálhatja hello post feladatátvételi művelet, például a DNS-bejegyzés frissítése webhelykötések módosítása, a kapcsolati karakterláncban adja hozzá a megfelelő parancsfájlokat hello helyreállítási tervben alábbi módosítása. [Ismerje meg, további információk hozzáadása a parancsfájl helyreállítási terv](./site-recovery-create-recovery-plans.md#add-scripts).

#### <a name="dns-update"></a>A DNS-frissítés
Ha a hello DNS DNS dinamikus frissítést, majd virtuális gépek általában módosítását hello DNS hello új IP-, ha elindítják a van konfigurálva. Ha azt szeretné, tooadd egy explicit lépés tooupdate DNS hello az új IP-címek a virtuális gépek hello majd adja hozzá ezt a [tooupdate IP a DNS-parancsfájl](https://aka.ms/asr-dns-update) a post műveletek a helyreállítási terv csoportok.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Az alkalmazás web.config kapcsolati karakterlánc
hello kapcsolati karakterláncot határozza meg, amely a webhely hello hello adatbázis kommunikál.

Hello kapcsolati karakterlánc hordoz magában, ha hello hello adatbázis virtuális gép nevét, nincs további lépések végzésére lesz szükséges post feladatátvevő és hello alkalmazás tudják tooautomatically toohello DB kapcsolatba. Is, ha hello IP-cím hello adatbázis virtuális gép őrzi meg, akkor nem lesz szükség tooupdate hello kapcsolati karakterláncot. Ha hello kapcsolati karakterlánc utal toohello adatbázis virtuális gép IP-címet használ, el kell frissíteni toobe post feladatátvételi. Például alább a kapcsolódási karakterlánc hello mutat toohello IP-127.0.1.2 DB

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Frissítheti az hello kapcsolati karakterláncot a webes réteg hozzáadásával [IIS kapcsolat frissítő parancsfájlt](https://aka.ms/asr-update-webtier-script-classic) hello helyreállítási tervben 3 csoport után.

#### <a name="site-bindings-for-hello-application"></a>Hely kötései hello alkalmazáshoz
Minden hely kötése típusának hello is információkat, hello IP-cím, mely hello IIS kiszolgáló figyel toohello kérelmek hello hely, a hello port számát és a hello állomásnevek hello hely kötésének áll. A feladatátvétel hello időpontjában ilyen kötést módosítania kell frissíteni, a hozzájuk társított runbookokat hello IP-cím megváltozása esetén toobe.

> [!NOTE]
>
> Ha "az összes ki nem osztott" az alábbi hello példában látható módon hello webhelykötések van megjelölve, nem kell tooupdate a kötés post feladatátvételi. Is, ha egy helyhez társított hello IP-cím nem változik a post feladatátvételi, hello hely kötés szükséges frissítése (hello IP-cím megőrzési függ hello hálózati architektúra és alhálózatok hozzárendelt toohello elsődleges és a helyreállítási helyeken, és ezért előfordulhat, hogy, vagy nem lehet valósítható meg a szervezet számára.)

![SSL-kötés](./media/site-recovery-iis/sslbinding.png)

Ha hello IP-cím társítva van egy helyhez, akkor minden hely kötései hello új IP-cím tooupdate. Hozzáadhat [IIS webes réteg frissítő parancsfájlt](https://aka.ms/asr-web-tier-update-runbook-classic) a helyreállítási terv toochange hello helykötések 3 csoport után.


#### <a name="update-load-balancer-ip-address"></a>Load balancer IP-címének frissítése
Ha a virtuális gép alkalmazáskérelmek irányítására, vegye fel [IIS ARR feladatátvételi parancsfájl](https://aka.ms/asr-iis-arrtier-failover-script-classic) csoport 4 tooupdate hello IP-cím után.

#### <a name="hello-ssl-cert-binding-for-an-https-connection"></a>hello SSL-tanúsítvány kötés egy https-kapcsolat számára
Webhelyek rendelkezhet társított SSL-tanúsítvány, amely segít annak biztosításában hello webkiszolgálót és hello felhasználó böngészője közötti biztonságos kommunikációhoz. Ha hello webhely https-kapcsolat és egy társított https hely kötése toohello IP-cím hello IIS-kiszolgáló SSL-tanúsítvány kötést, egy új hely kötésének toobe hello cert hello az IP-címe hello az IIS virtuális gép feladatátvételi utáni hozzá lesz szüksége.

hello SSL-tanúsítványt kibocsáthatja elleni-

a) hello teljesen minősített tartománynevét hello webhely<br>
b) hello hello kiszolgáló neve<br>
c) helyettesítő tanúsítvány hello tartománynév<br>
d) egy IP-cím – Ha hello SSL-tanúsítványt ad ki hello IP hello IIS-kiszolgáló ellen, hello IP-címe hello állították ki egy másik SSL-tanúsítványt kell toobe hello Azure webhely az IIS-kiszolgálón, és a tanúsítvány további SSL-kötést kell létrehozni toobe. Ezért akkor ajánlott toonot használja az SSL-tanúsítványt IP állították ki. Ez egy kevésbé elterjedt lehetőséget, és hamarosan megszűnnek új hitelesítésszolgáltató/böngésző fórum változások szerint.

#### <a name="update-hello-dependency-between-hello-web-and-hello-application-tier"></a>Frissítés hello függőségi hello webes és hello alkalmazás szint között
Ha egy adott alkalmazásfüggőséget hello hello virtuális gépek IP-címe alapján, kell tooupdate a függőségi post feladatátvételi.

## <a name="doing-a-test-failover"></a>A teszt feladatátvétel
Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) toodo feladatátvételi tesztet.

1.  Nyissa meg tooAzure portálon, és válassza a helyreállítási szolgáltatás tárolójából.
1.  Kattintson az IIS webfarm létrehozott hello helyreállítási terv.
1.  Kattintson a "Test Failover".
1.  Válassza ki a helyreállítási pont és az Azure-beli virtuális hálózat toostart hello teszt feladatátvételi folyamat.
1.  Hello másodlagos környezetben működik, ha az érvényesítést végezheti el.
1.  Ha hello ellenőrzés befejeződött, kiválaszthatja, hogy érvényesítést végrehajtani, és hello teszt feladatátvételi környezet törölve lesznek.

## <a name="doing-a-failover"></a>A feladatátvétel
Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.

1.  Nyissa meg tooAzure portálon, és válassza a helyreállítási szolgáltatás tárolójából.
1.  Kattintson az IIS webfarm létrehozott hello helyreállítási terv.
1.  Kattintson a "Failover".
1.  Válassza ki a helyreállítási pont toostart hello feladatátvételi folyamatot.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [replikálja más alkalmazások](site-recovery-workload.md) Site Recovery segítségével.
