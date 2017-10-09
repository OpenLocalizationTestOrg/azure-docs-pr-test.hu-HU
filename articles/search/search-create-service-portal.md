---
title: "az Azure Search szolgáltatást hello portálon aaaCreate |} Microsoft Docs"
description: "Egy Azure Search szolgáltatás hello portálon kiépítése."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: f1c7197a1467e32bd4b360b165c9059e6bb0e496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-service-in-hello-portal"></a>Egy Azure Search szolgáltatás létrehozása hello portálon

Ez a cikk azt ismerteti, hogyan toocreate vagy kiépítéséhez az Azure Search szolgáltatás hello portálon. PowerShell útmutatásért lásd: [Azure Search kezelése a PowerShell-lel](search-manage-powershell.md).

## <a name="subscribe-free-or-paid"></a>Előfizetés (ingyenes és fizetős)

[Nyisson meg egy ingyenes Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) és szabad kreditek tootry kimenő fizetős Azure-szolgáltatásokhoz. Miután jóváírásokat el is használta, megtarthatja hello fiókot, és toouse ingyenes Azure-szolgáltatások, például a webhelyek továbbra is. A hitelkártya soha nem feladata, kivéve, ha explicit módon a beállítások módosításához és kérje meg a toobe számítjuk fel.

Másik lehetőségként [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). Az MSDN előfizetői biztosít Önnek krediteket havonta is használhatja a fizetős Azure-szolgáltatásokat. 

## <a name="find-azure-search"></a>Az Azure Search keresése
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a bal sarok hello felső hello plusz jel ("+").
3. Válassza ki **Web + mobil** > **az Azure Search**.

![](./media/search-create-service-portal/find-search2.png)

## <a name="name-hello-service-and-url-endpoint"></a>Nevű hello szolgáltatás és az URL-végpontjának

A szolgáltatás nevét hello URL-végpontjának szemben, amelyek az API-hívásokban kiállított részét képezi. Adja meg a szolgáltatás nevét a hello **URL-cím** mező. 

Szolgáltatás vonatkozó követelményeknek:
   * 2-60 karakter hosszúságú
   * kisbetűk, számjegyek és kötőjelek ("-")
   * nincs a kötőjel ("-"), először 2 karakternél vagy utolsó egyetlen karakter hello
   * Nincs egymást követő kötőjeleket ("--")

## <a name="select-a-subscription"></a>Előfizetés kiválasztása
Ha egynél több előfizetéssel rendelkezik, válasszon egyet, adatok vagy a fájl tárolási szolgáltatások is vannak. Az Azure Search is automatikus észlelésű Azure Table és a Blob storage, az SQL Database és az Azure Cosmos DB használatával az indexelés *indexelők*, de csak a hello szolgáltatások ugyanahhoz az előfizetéshez.

## <a name="select-a-resource-group"></a>Erőforráscsoport kiválasztása
Erőforráscsoport gyűjteményei, Azure-szolgáltatások és erőforrások együtt használja őket. Például, ha az Azure Search tooindex egy SQL-adatbázist használ, majd mindkét szolgáltatás részét kell képezniük hello ugyanabban az erőforráscsoportban.

> [!TIP]
> Erőforráscsoport törlésekor a hello szolgáltatások belül is törlődnek. Több szolgáltatás használatával, és ezek a hello prototípus projektek ugyanabban az erőforráscsoportban egyszerűbbé teszi a tisztítás követően hello projekt. 

## <a name="select-a-hosting-location"></a>Adjon meg egy üzemeltetési helyet 
Az Azure-szolgáltatások, Azure Search hello világ különböző adatközpontokban lehet üzemeltetni. Vegye figyelembe, hogy [árak eltérőek lehetnek](https://azure.microsoft.com/pricing/details/search/) földrajzi hely.

## <a name="select-a-pricing-tier-sku"></a>Válassza ki a tarifacsomagot (SKU)
[Az Azure Search jelenleg kínált több árképzési szinteket a](https://azure.microsoft.com/pricing/details/search/): szabad, alapszintű vagy Standard. Minden szinthez tartozik a saját [kapacitás és korlátai](search-limits-quotas-capacity.md). Lásd: [válasszon egy tarifacsomagot réteg vagy SKU](search-sku-tier.md) útmutatót.

Ebben a bemutatóban hello Standard csomagra választott azt a szolgáltatás.

## <a name="create-your-service"></a>A szolgáltatás létrehozása

Ne felejtse el toopin egyszerűen hozzáférhetnek a service toohello irányítópult amikor bejelentkezik.

![](./media/search-create-service-portal/new-service2.png)

## <a name="scale-your-service"></a>A szolgáltatás méretezésére
Is igénybe vehet néhány percet toocreate (15 perc vagy több, attól függően, hogy hello réteg) szolgáltatás. A szolgáltatás üzembe helyezése után át lehet méretezni toomeet igényeinek. Az Azure Search szolgáltatás számára is választott hello Standard csomagra, mert a szolgáltatás is skálázható a két dimenziókban: replikák és a partíciók. Kellett hello Basic szint választása esetén csak adhat hozzá replikákat. Hello szabad szolgáltatás kiépítése, ha a skála nem érhető el.

***Partíciók*** engedélyezése a szolgáltatás toostore, és keressen további dokumentumok keresztül.

***Replikák*** engedélyezése a szolgáltatás toohandle keresési lekérdezések nagyobb terhelést.

> [!Important]
> Rendelkeznie kell egy szolgáltatás [írásvédett garantált szolgáltatási szintje 2 replikákat és a 3-replikáit az olvasási/írási SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Nyissa meg a tooyour search szolgáltatás paneljét hello Azure-portálon a.
2. Hello bal oldali navigációs sávon ablaktáblában jelölje ki **beállítások** > **méretezési**.
3. Hello slidebar tooadd replikák és a partíciók használja.

![](./media/search-create-service-portal/settings-scale.png)

> [!Note] 
> Minden szinthez tartozik különböző [korlátok](search-limits-quotas-capacity.md) a hello egyetlen szolgáltatás engedélyezett keresési egységek teljes száma (replikák * partíciók teljes keresési egység =).

## <a name="when-tooadd-a-second-service"></a>Ha egy második szolgáltatás tooadd

Az ügyfelek többsége szolgáltatással egy olyan réteghez, amely hello üzembe [jobb gombbal az erőforrások egyensúlyára](search-sku-tier.md). Egy szolgáltatás, rendelkezhet több indexek, tulajdonos toohello [választja hello réteg maximális korlátok](search-capacity-planning.md), az egyes index egymástól el vannak különítve egymástól. Az Azure Search kérelmek csak lehet kell irányított tooone index, minimalizálja a véletlen vagy szándékos adatok beolvasása más hello esélyét indexeli a hello azonos szolgáltatás.

Bár a legtöbb felhasználó csak egy szolgáltatás használatához szolgáltatás redundancia lehet szükség, ha a működési követelményeinek hello következők:

+ Vész-helyreállítási (az Adatközpont kimaradás). Az Azure Search nem biztosítanak az azonnali hello kimaradás esetén. Javaslatok és útmutatást: [felügyeleti szolgáltatás](search-manage.md).
+ A több-bérlős modellezési vizsgálata megállapította, hogy további szolgáltatások hello optimális Tervező. További információkért lásd: [terv a több-bérlős](search-modeling-multitenant-saas-applications.md).
+ Globálisan központilag telepített alkalmazások esetén van szüksége az Azure Search példánya több régiók toominimize késleltetés az alkalmazás nemzetközi forgalom.

> [!NOTE]
> Az Azure Search elkülönítse nem indexelési és lekérdező munkaterhelések; így akkor soha nem kell létrehoznia elkülönített munkaterhelések több szolgáltatás. Az index mindig lekérdezett hello szolgáltatásban, amelyben az informatikai készült (nem lehet indexet létrehozni egy szolgáltatás és másolásához tooanother).
>

Egy második szolgáltatás nincs szükség a magas rendelkezésre állás érdekében. Magas rendelkezésre állás a lekérdezések érhető el, ha a 2 vagy több replikák hello ugyanazt a szolgáltatást. A replika frissítései egymást követő, ami azt jelenti, hogy legalább egy akkor működik, ha a szolgáltatásfrissítés megkezdődött. Hasznos üzemidő kapcsolatos további információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

## <a name="next-steps"></a>Következő lépések
Egy Azure Search szolgáltatás kiépítését követően készen áll a túl[index definiálása az](search-what-is-an-index.md) , töltse fel, és keresse meg az adatokat.

tooaccess hello szolgáltatást kód vagy parancsfájlból, hello URL-Címének megadása (*szolgáltatásnév*. search.windows.net) és a kulcsot. Adminisztrációs kulcsok teljes hozzáférést; lekérdezési kulcsok hozzáférést csak olvasható. Lásd: [hogyan toouse .NET keresni Azure](search-howto-dotnet-sdk.md) tooget elindult.

Lásd: [felépítéséhez és az első index lekérdezése](search-get-started-portal.md) gyors portálalapú oktatóanyag.

