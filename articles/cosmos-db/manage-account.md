---
title: "Egy Azure Cosmos DB fiók az Azure-portálon kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti az Azure portál Azure Cosmos DB fiókját. Útmutató az Azure portál használatával megtekintése, másolása, törlés és hozzáférési fiókok található."
keywords: "Azure-portálon, a documentdb, az azure, a Microsoft azure"
services: cosmos-db
documentationcenter: 
author: kirillg
manager: jhubbard
editor: cgronlun
ms.assetid: 00fc172f-f86c-44ca-8336-11998dcab45c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kirillg
ms.openlocfilehash: a0c6ec8d490e1adacc96758971ab91d8eaeab45c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-an-azure-cosmos-db-account"></a>Egy Azure Cosmos DB fiók kezelése
Ismerje meg, globális konzisztenciahiba, kulcsok dolgozni, és az Azure portálon Azure Cosmos DB-fiók törlése.

## <a id="consistency"></a>Azure Cosmos DB konzisztencia beállításainak kezelése
A jobb oldali konzisztenciaszint kiválasztása attól függ, hogy az alkalmazás szemantikáját. Meg kell Ismerkedjen meg a rendelkezésre álló konzisztenciaszintek az Azure Cosmos Adatbázisba olvasásával [konzisztenciaszintek használatával a rendelkezésre állás és teljesítmény az Azure Cosmos DB maximalizálása érdekében][consistency]. Azure Cosmos-adatbázis konzisztencia, a rendelkezésre állási és a teljesítmény garanciák, az adatbázis fiókjához tartozó minden konzisztencia szintet biztosít. Az adatbázis-fiók beállítása az erős konzisztencia szintű kell lennie az adatok egyetlen Azure régióra zárt és globálisan nem érhető el. Másrészről, a laza konzisztenciaszintek - a kötött elavulási, munkamenet és végleges engedélyezése, hogy rendelje hozzá Azure-régiók tetszőleges számú adatbázis fiókját. Az alábbi egyszerű lépéseket mutatja be az adatbázis fiókjához tartozó alapértelmezett konzisztencia szintet. 

### <a name="to-specify-the-default-consistency-for-an-azure-cosmos-db-account"></a>Az alapértelmezett konzisztencia Azure Cosmos DB fiók megadása
1. Az a [Azure-portálon](https://portal.azure.com/), Azure Cosmos DB-fiókját.
2. A fiók paneljén kattintson **alapértelmezett konzisztencia**.
3. Az a **alapértelmezett konzisztencia** panelen válassza ki az új konzisztenciaszint, és kattintson a **mentése**.
    ![Alapértelmezett konzisztencia munkamenet][5]

## <a id="keys"></a>Megtekintése, másolása és tárelérési kulcsok újragenerálása
Egy Azure Cosmos DB fiók létrehozásakor a szolgáltatás két fő kulcsot, amely az Azure Cosmos DB fiók elérésekor hitelesítéshez használt állít elő. Két tárelérési kulcsok megadásával Azure Cosmos DB teszi Azure Cosmos DB fiókjába megszakítás nélkül a kulcsok újragenerálását. 

Az a [Azure-portálon](https://portal.azure.com/), hozzáférés a **kulcsok** erőforrás-menüjéből panelen a **Azure Cosmos DB fiók** megtekintése, másolása és újragenerálása a tárelérési kulcsok, amelyek használt panel a Azure Cosmos DB-fiók eléréséhez.

![Az Azure portál képernyőképe, a kulcsok panelen](./media/manage-account/keys.png)

> [!NOTE]
> A **kulcsok** panel is tartalmaz, amelyek segítségével a fiókjához elsődleges és másodlagos kapcsolati karakterláncok a [adatáttelepítési eszköz](import-data.md).
> 
> 

Csak olvasható kulcsokat is elérhetők a panel. Olvasás és az olvasási műveletek kicsit hoz létre, törölhet, illetve cserél nem lekérdezések.

### <a name="copy-an-access-key-in-the-azure-portal"></a>Az elérési kulcs másolása az Azure portálon
Az a **kulcsok** panelen kattintson a **másolási** kíván másolni a kulcs jobbra látható gombra.

![Megtekintése és másolása egy hozzáférési kulcsot az Azure portálon, a kulcsok panelen](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Tárelérési kulcsok újragenerálása
Módosítsa a tárelérési kulcsok rendszeres időközönként a Azure Cosmos DB fiókját biztonságosabbá a kapcsolatokat. Engedélyezi, hogy az egyik kulcs, amíg a többi hozzáférési kulcs újragenerálása Azure Cosmos DB fiókhoz kapcsolatok karbantartása két tárelérési kulcsok vannak hozzárendelve.

> [!WARNING]
> A tárelérési kulcsok újragenerálása hatással van az aktuális kulcs függő alkalmazások. A Azure Cosmos DB-fiók eléréséhez a hozzáférési kulcsot használó összes ügyfelet frissíteni kell az új kulcs használatához.
> 
> 

Ha alkalmazások vagy az Azure Cosmos DB fiókkal felhőszolgáltatások, elvesznek a kapcsolatok Ha újragenerálja a kulcsokat, kivéve, ha rotálja a kulcsokat. A működés közbeni a kulcsok részt vevő folyamat lépései.

1. Frissítse az alkalmazás kódjában hivatkozhasson rá az Azure Cosmos DB fiók másodlagos elérési kulcsát a hozzáférési kulcsot.
2. Újragenerálja az elsődleges elérési kulcsot az Azure Cosmos DB fiók. Az a [Azure Portal](https://portal.azure.com/), Azure Cosmos DB-fiókját.
3. Az a **Azure Cosmos DB fiók** panelen kattintson a **kulcsok**.
4. Az a **kulcsok** panelen kattintson az újbóli létrehozás gombra, majd **Ok** annak ellenőrzéséhez, hogy szeretné-e hozzon létre egy új kulcsot.
    ![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-keys.png)
5. Miután ellenőrizte, hogy az új kulcs használható (körülbelül 5 perc után újragenerálása), frissítse az alkalmazás kódjában hivatkozhasson rá az új elsődleges elérési kulcsát a hozzáférési kulcsot.
6. Generálja újra a másodlagos elérési kulcsot.
   
    ![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Mielőtt egy újonnan létrehozott kulcs segítségével fér hozzá a Azure Cosmos DB fiókjához több percet is igénybe vehet.
> 
> 

## <a name="get-the--connection-string"></a>A kapcsolati karakterlánc beolvasása
A kapcsolati karakterlánc lekéréséhez, tegye a következőket: 

1. Az a [Azure-portálon](https://portal.azure.com), Azure Cosmos DB-fiókját.
2. Az erőforrás menüjében kattintson **kulcsok**.
3. Kattintson a **másolási** megjelenítő gombra a **elsődleges kapcsolódási karakterlánc** vagy **másodlagos kapcsolati karakterlánc** mezőbe. 

A kapcsolati karakterláncot a rendszer használata esetén a [Azure Cosmos DB adatbázis áttelepítési eszköz](import-data.md), az adatbázisnév hozzáfűzése a kapcsolati karakterlánc végén. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>Azure Cosmos DB-fiók törlése
Egy Azure Cosmos DB fiók már nem használ az Azure portálról eltávolításához kattintson a jobb gombbal a fiók nevét, majd kattintson **fiók törlése**.

![Az Azure portál Azure Cosmos DB fiók törlése](./media/manage-account/deleteaccount.png)

1. Az a [Azure-portálon](https://portal.azure.com/), a törölni kívánt Azure Cosmos DB fiók eléréséhez.
2. Az a **Azure Cosmos DB fiók** panelen kattintson a jobb gombbal a fiókot, és kattintson a **fiók törlése**. 
3. Az eredményül kapott megerősítő panelen írja be a Azure Cosmos DB nevét annak megerősítéséhez, hogy a fiók törlése.
4. Kattintson a **törlése** gombra.

![Az Azure portál Azure Cosmos DB fiók törlése](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Következő lépések
Megtudhatja, hogyan [Ismerkedés az Azure Cosmos DB fiókja](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
