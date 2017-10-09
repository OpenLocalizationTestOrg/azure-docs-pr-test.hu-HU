---
title: "aaaCommon felhőalapú szolgáltatás kezelési feladatai (klasszikus) |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage cloud services – a hello a klasszikus Azure portálon."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 03b1d632f1480d0f65c87b7f8ffc747417acf7b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a>TooManage Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-manage.md)
>
>

A hello **Felhőszolgáltatások** területén hello klasszikus Azure portál, lehet frissíteni a szerepkör-szolgáltatás vagy a központi telepítés, előléptetni egy szakaszos telepítés tooproduction, hivatkozás erőforrások tooyour felhőalapú szolgáltatás, így megtekintheti a hello erőforrás függőségek méretezési erőforrások hello együtt és egy felhőalapú szolgáltatás, vagy a központi telepítés törlése.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Hogyan: Felhő szerepkör-szolgáltatás vagy a központi telepítés
Ha tooupdate hello alkalmazáskód kell a felhőalapú szolgáltatás, akkor **frissítés** hello irányítópult **Felhőszolgáltatások** lapon vagy **példányok** lap. Egyetlen szerepkör vagy az összes szerepkör frissítheti. Egy új service-csomag tooupload és a szolgáltatás konfigurációs fájlja lesz szüksége.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), hello irányítópulton, **Felhőszolgáltatások** lapon vagy **példányok** lapján kattintson **frissítés**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. A **üzemelő példány címkéje**, adjon meg egy nevet tooidentify hello deployment (például mycloudservice4). Hello üzemelő példány címkéje alatt található **gyors üzembe helyezési** hello irányítópulton.
3. A **csomag**, használjon **Tallózás** tooupload hello felhőszolgáltatás csomagfájlját (.cspkg).
4. A **konfigurációs**, használjon **Tallózás** tooupload hello szolgáltatás konfigurációs fájlját (.cscfg).
5. A **szerepkör**, jelölje be **összes** Ha azt szeretné, hogy tooupgrade hello az összes szerepkör felhőalapú szolgáltatás. egyetlen szerepkör tooperform frissítéséhez tooupdate kívánt válassza hello szerepkör. Akkor is, ha egy adott szerepkör tooupdate választja, hello hello szolgáltatás konfigurációs fájljában frissítései alkalmazott tooall szerepkörök.
6. Ha hello a frissítés módosításai hello szerepkörök száma vagy hello méretének minden olyan szerepkört, jelölje be a hello **frissítés engedélyezése, ha a szerepkör méretét vagy a szerepkörök számának változik** jelölőnégyzetet tooenable hello frissítés tooproceed.

    Vegye figyelembe, hogy ha egy szerepkört (Ez azt jelenti, hogy hello egy virtuális gép méretét, amelyen a szerepkör példánya) hello méretének módosítása vagy a szerepkörök számának hello, minden szerepkörpéldányt (virtuális gép) visszaállított kell lennie, és bármely helyi adatok elvesznek.

7. Ha bármely szolgáltatás szerepkörök csak egy szerepkör példánya, válassza ki a hello **frissítése akkor is, ha egy vagy több szerepkör egyetlen példányt jelölőnégyzet tartalmaz** tooenable hello frissítési tooproceed.

    Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosítása a felhőalapú szolgáltatás frissítése közben. Ha szerepkörönként legalább két szerepkörpéldányokat (virtuális gépek). Amely lehetővé teszi, hogy egy virtuális gép tooprocess ügyfélkérelmek más hello frissítése közben.

8. Kattintson a **OK** (jelölő) toobegin hello szolgáltatás frissítése.

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a>Hogyan: felcserélése központi telepítések toopromote egy szakaszos telepítés tooproduction
Használjon **felcserélése** toopromote egy felhőalapú szolgáltatás tooproduction átmeneti központi telepítését. Ha úgy dönt, hogy toodeploy egy felhőalapú szolgáltatás új kiadását, tesztelése, és új kiadási tesztelése a felhőalapú szolgáltatás tesztelési környezetben, amíg a használják az ügyfelek a jelenlegi kiadásban hello éles környezetben. Amikor készen áll a toopromote hello új kiadási tooproduction, használja **felcserélése** tooswitch hello URL-címek alapján mely hello két központi telepítések tárgyalja.

Hello telepítéseit kicserélheti **Felhőszolgáltatások** lap vagy hello irányítópult.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**.
2. A felhőszolgáltatások hello listájában kattintson a hello cloud service tooselect azt.
3. Kattintson a **felcserélése**.

    hello következő megerősítési kérés nyílik meg.

    ![Cloud Services lapozófájl-kapacitás](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Miután meggyőződött hello információkat, kattintson a **Igen** tooswap hello központi telepítéseket.

    hello telepítési swap gyorsan mert megváltoztató egyedül hello hello virtuális IP-címek (VIP) hello telepítésekhez.

    toosave számítási költségek, törölheti hello telepítését a hello átmeneti környezet, ha biztos benne, hogy hello új éles környezet az elvárt módon működnek.

### <a name="common-questions-about-swapping-deployments"></a>Központi telepítések csere kapcsolatos gyakori kérdésekre

**Mik azok a telepítések csere hello előfeltételei**

Nincsenek a sikeres telepítés swap két fő előfeltételei:

- Ha azt szeretné, hogy egy statikus IP-cím toouse a termelési aljzat, kell lefoglalni egy a, valamint az átmeneti helyet. Ellenkező esetben hello swap sikertelen lesz.

- A szerepkörök az összes példányát kell futnia, hello swap elvégzése előtt. Ellenőrizheti, hogy a példány, a klasszikus Azure portálon hello vagy hello állapotának [Get-AzureRole parancs a Windows PowerShell hello](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Vegye figyelembe, hogy Vendég operációs rendszer frissítése és a szolgáltatás javító műveleteket is eredményezheti, hogy telepítési toofail cseréje. Lásd: [felhőalapú szolgáltatás központi telepítési problémák elhárítása](cloud-services-troubleshoot-deployment-problems.md) további részleteket.

**A lapozófájl-kapacitás negatívan befolyásolja az alkalmazáshoz állásidő? Hogyan tudom kezelje azt?**

Hello utolsó szakaszban leírtak telepítési egy felcserélés általában nagyon gyorsan mert csak egy hello Azure terheléselosztó a konfiguráció megváltozott. Néhány esetben azonban képes tíz vagy több másodpercre és átmeneti kapcsolati hibákat eredményezhet. toolimit hatás tooyour ügyfelek, vegye fontolóra [ügyfél újrapróbálkozási logika](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-tooa-cloud-service"></a>Útmutató: egy erőforrás tooa felhőalapú szolgáltatás csatolása
tooshow a felhőalapú szolgáltatás függőségek más erőforrások, az Azure SQL Database-példányt vagy egy tárolási fiók toohello felhőszolgáltatás is kapcsolhat. Hivatkozásra, és leválasztását hello erőforrások **kapcsolt erőforrásokban** lapon, és figyelheti a hello cloud service irányítópulton használatát. Ha kapcsolt tárfiókra monitoring bekapcsolva, kérelmek teljes száma a figyelheti hello felhőalapú szolgáltatás irányítópultját.

Használjon **hivatkozás** toolink egy új vagy meglévő SQL-adatbázis példányt, vagy a tárolási fiók tooyour felhőalapú szolgáltatás. Hello adatbázis együtt hello felhő szerepkör-szolgáltatás, amely a hello használja majd méretezheti **méretezési** lap. (A storage-fiókok arányosan automatikusan a használati növekedése.) További információkért lásd: [hogyan tooScale egy felhőalapú szolgáltatás és a kapcsolt erőforrásokban](cloud-services-how-to-scale.md).

Ezen kívül figyeléséhez, kezeléséhez, és hello hello adatbázis méretezése **adatbázisok** munkaterület hello a klasszikus Azure portálon.

"Linking" erőforrás abban az értelemben nem kapcsolatot az alkalmazás toohello erőforrás. Ha létrehoz egy új adatbázis használatával **hivatkozás**, tooadd hello kapcsolati karakterláncok tooyour alkalmazás kódja és majd frissítési hello felhőszolgáltatás lesz szüksége. Biztosítani kell tooadd kapcsolati karakterláncok, ha az alkalmazás erőforrást használ, a kapcsolt tárfiókra.

a következő eljárás hello ismerteti, hogyan toolink egy új SQL Database-példányt telepítése egy új SQL-kiszolgálón, tooa felhőalapú szolgáltatás.

### <a name="toolink-a-sql-database-instance-tooa-cloud-service"></a>egy SQL-adatbázis példány tooa felhőszolgáltatás toolink
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**. Kattintson a hello cloud service tooopen hello irányítópult hello nevét.
2. Kattintson a **kapcsolódó erőforrások**.

    Hello **kapcsolt erőforrásokban** lap megnyitásakor.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Jelölje be az **erőforrás hivatkozás** vagy **hivatkozás**.

    Hello **hivatkozás erőforrás** varázsló elindul.

    ![Hivatkozás 1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Kattintson a **hozzon létre egy új erőforrást** vagy **csatolni a meglévő erőforrás**.
5. Válassza ki a hello típusú erőforrás toolink. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **SQL-adatbázis**. (Csak a klasszikus Azure portálon hello támogatja, a tárolási fiók tooa felhőszolgáltatást linking.)
6. toocomplete hello adatbázis-konfiguráció, kövesse az utasításokat a hello súgójában **SQL-adatbázisok** hello a klasszikus Azure portálon területén.

    Követheti a művelet hello üzenet területen linking hello hello előrehaladását.

    Linking befejeződése után hello kapcsolódó erőforrás hello cloud service irányítópult hello állapotának figyelheti meg. Csatolt SQL-adatbázis méretezésével kapcsolatos információkért lásd: [hogyan tooScale egy felhőalapú szolgáltatás és a kapcsolt erőforrásokban](cloud-services-how-to-scale.md).

### <a name="toounlink-a-linked-resource"></a>a csatolt erőforrás toounlink
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**. Kattintson a hello cloud service tooopen hello irányítópult hello nevét.
2. Kattintson a **kapcsolt erőforrásokban**, majd válassza ki a hello erőforrás.
3. Kattintson a **leválasztása**. Kattintson a **Igen** : hello rendszer felszólítsa a jóváhagyásra.

    SQL-adatbázis leválasztása hatástalan hello adatbázis vagy hello alkalmazás kapcsolatok toohello adatbázis. Továbbra is kezelheti a hello hello adatbázis **SQL-adatbázisok** hello a klasszikus Azure portálon területén.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Útmutató: egy felhőalapú szolgáltatás és a központi telepítései törlése
Egy felhőalapú szolgáltatás törlése előtt törölni kell minden meglévő telepítés.

toosave számítási költségek, törölheti a tesztelési környezet, hogy a várt módon működik-e az éles telepítési ellenőrzése után. Akkor is, ha egy felhőalapú szolgáltatás nem fut áll szerepkörpéldányokat számlázott számítási költségeit.

A következő eljárás toodelete hello használja, a központi telepítés vagy a felhőalapú szolgáltatás.

1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**.
2. Válassza ki a hello felhőalapú szolgáltatás, és kattintson a **törlése**. (tooselect egy felhőalapú szolgáltatás hello az irányítópult megnyitása nélkül kattintson bárhová hello cloud service bejegyzésben hello nevén kívül.)

    Ha egy telepítés átmeneti és üzemi, megjelenik egy hello aljához hello ablakban a következő lehetőségek hasonló toohello gombot. Hello felhőalapú szolgáltatás törlése előtt törölni kell a meglévő telepítések.

    ![Menü törlése](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. toodelete a telepítést, kattintson a **éles környezet törlése** vagy **átmeneti központi telepítés törlése**. Ezután hello megerősítő kérdés, kattintson **Igen**.
4. Ha azt tervezi, toodelete hello felhőalapú szolgáltatás, ismételje meg a 3, szükség esetén toodelete a másik telepítés.
5. toodelete hello felhőalapú szolgáltatás, kattintson a **törlése a felhőalapú szolgáltatás**. Ezután hello megerősítő kérdés, kattintson **Igen**.

> [!NOTE]
> Ha részletes figyelését a felhőalapú szolgáltatás van konfigurálva, Azure hello figyelési adatok importáljon a tárfiókba, hello felhőszolgáltatás törlése nem törli. Manuálisan kell toodelete hello adatokat. Ha toofind hello metrikák táblák kapcsolatos információkért lásd: "How to: hello a klasszikus Azure portálon kívül részletes figyelési adatok eléréséhez" a [tooMonitor Cloud Services](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Következő lépések
* [A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure.md).
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name.md).
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md).
