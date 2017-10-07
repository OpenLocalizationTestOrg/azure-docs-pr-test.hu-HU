---
title: "hello Azure portálon keresztül Azure Cosmos DB fiók aaaManage |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage az Azure Cosmos DB fiók hello Azure portálon keresztül. Útmutató, hello Azure Portal tooview, másolás, törlés és hozzáférési fiókok használatával található."
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
ms.openlocfilehash: 77ad953cf558a519674be08ad913a12202f69496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-an-azure-cosmos-db-account"></a>Hogyan toomanage Azure Cosmos DB fiók
Megtudhatja, hogyan tooset globális konzisztenciahiba, kulcsok használata, és az Azure-portálon hello Azure Cosmos DB-fiók törlése.

## <a id="consistency"></a>Azure Cosmos DB konzisztencia beállításainak kezelése
Hello jobb konzisztenciaszint kiválasztása attól függ, hogy az alkalmazás hello szemantikáját. Tanulmányozza át az hello elérhető konzisztenciaszintek az Azure Cosmos Adatbázisba olvasásával [toomaximize rendelkezésre állásának és teljesítményének a Azure Cosmos DB használatával konzisztencia szintek][consistency]. Azure Cosmos-adatbázis konzisztencia, a rendelkezésre állási és a teljesítmény garanciák, az adatbázis fiókjához tartozó minden konzisztencia szintet biztosít. Az adatbázis-fiók beállítása az erős konzisztencia szintű futnia kell az adatok zárt tooa egyetlen Azure-régió, és globális nem érhető el. A ugyanakkor hello, laza konzisztenciaszintek - a kötött elavulási, munkamenet és végül mind engedélyezés hello meg tooassociate tetszőleges számú Azure-régiók adatbázis fiókját. hello egyszerű lépések bemutatják, hogyan tooselect hello konzisztenciaszint alapértelmezett adatbázis-fiókjához. 

### <a name="toospecify-hello-default-consistency-for-an-azure-cosmos-db-account"></a>toospecify hello alapértelmezett konzisztencia Azure Cosmos DB fiók
1. A hello [Azure-portálon](https://portal.azure.com/), Azure Cosmos DB-fiókját.
2. Hello-fiók panelen kattintson a **alapértelmezett konzisztencia**.
3. A hello **alapértelmezett konzisztencia** panelen, jelölje be hello új konzisztencia szinten, és kattintson **mentése**.
    ![Alapértelmezett konzisztencia munkamenet][5]

## <a id="keys"></a>Megtekintése, másolása és tárelérési kulcsok újragenerálása
Egy Azure Cosmos DB fiók létrehozásakor hello szolgáltatás hello Azure Cosmos DB fiók használatakor a hitelesítéshez használt két fő tárelérési kulcsokat hoz létre. Két tárelérési kulcsok megadásával Azure Cosmos DB lehetővé teszi tooregenerate hello kulcsok nem megszakítás tooyour Azure Cosmos DB fiók. 

A hello [Azure-portálon](https://portal.azure.com/), hozzáférési hello **kulcsok** hello erőforrás menüjéből hello panel **Azure Cosmos DB fiók** panel tooview, másolása és újragenerálása hello elérési kulcsainak, olyan használt tooaccess Azure Cosmos DB fiókját.

![Az Azure portál képernyőképe, a kulcsok panelen](./media/manage-account/keys.png)

> [!NOTE]
> Hello **kulcsok** panel is tartalmaz elsődleges és másodlagos kapcsolati karakterláncok, amelyek lehetnek használt tooconnect tooyour hello a fiók [adatáttelepítési eszköz](import-data.md).
> 
> 

Csak olvasható kulcsokat is elérhetők a panel. Olvasás és az olvasási műveletek kicsit hoz létre, törölhet, illetve cserél nem lekérdezések.

### <a name="copy-an-access-key-in-hello-azure-portal"></a>Az elérési kulcs másolása hello Azure portál
A hello **kulcsok** panelen hello kattintson **másolási** toocopy gomb toohello sarkában hello kulcs kívánja.

![Megtekintése és másolása hívóbetű hello Azure portálon, a kulcsok panelen](./media/manage-account/copykeys.png)

### <a name="regenerate-access-keys"></a>Tárelérési kulcsok újragenerálása
Meg kell változtatni hello hozzáférési kulcsok tooyour Azure Cosmos DB fiók rendszeresen toohelp a kapcsolatok nagyobb biztonságban. Két tárelérési kulcsok rendelt tooenable meg toomaintain kapcsolatok toohello Azure Cosmos DB fiók az egyik kulcs, amíg a újragenerálja hello más hozzáférési kulcsot.

> [!WARNING]
> A tárelérési kulcsok újragenerálása hatással van az aktuális kulcs hello függő alkalmazások. Hello hozzáférési kulcs tooaccess hello Azure Cosmos DB fiókhoz használó összes ügyfelet frissített toouse hello új kulcsot kell lennie.
> 
> 

Ha alkalmazások vagy szolgáltatások hello Azure Cosmos DB fiók használatával, elvesznek hello kapcsolatok Ha újragenerálja a kulcsokat, kivéve, ha rotálja a kulcsokat felhő. hello lépései hello folyamat működés közbeni a kulcsok részt.

1. Frissítés hello hozzáférési kulcs az alkalmazás kódja tooreference hello másodlagos elérési kulcsára hello Azure Cosmos DB fiók.
2. Az Azure Cosmos DB fiók hello elsődleges elérési kulcs újragenerálása. A hello [Azure Portal](https://portal.azure.com/), Azure Cosmos DB-fiókját.
3. A hello **Azure Cosmos DB fiók** panelen kattintson a **kulcsok**.
4. A hello **kulcsok** panelen hello újbóli létrehozás gombra kattintva, majd kattintson a **Ok** , amelyet az új kulcs toogenerate tooconfirm.
    ![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-keys.png)
5. Miután ellenőrizte, hello új kulcs használható (körülbelül 5 perc után újragenerálása), frissíteni az hello elérési kulcsot a az alkalmazás kódja tooreference hello új elsődleges elérési kulcsát.
6. Hello másodlagos elérési kulcs újragenerálása.
   
    ![Tárelérési kulcsok újragenerálása](./media/manage-account/regenerate-secondary-key.png)

> [!NOTE]
> Is igénybe vehet néhány percet egy újonnan létrehozott kulcs lehet használt tooaccess Azure Cosmos DB fiókját.
> 
> 

## <a name="get-hello--connection-string"></a>Hello kapcsolati karakterlánc beolvasása
tooretrieve a kapcsolati karakterlánc, a következő hello: 

1. A hello [Azure-portálon](https://portal.azure.com), Azure Cosmos DB-fiókját.
2. A hello erőforrás kattintson **kulcsok**.
3. Hello kattintson **másolási** gomb következő toohello **elsődleges kapcsolódási karakterlánc** vagy **másodlagos kapcsolati karakterlánc** mezőbe. 

Hello használatakor hello kapcsolati karakterlánc [Azure Cosmos DB adatbázis áttelepítési eszköz](import-data.md), hozzáfűző hello adatbázis neve toohello vége hello kapcsolati karakterláncot. `AccountEndpoint=< >;AccountKey=< >;Database=< >`.

## <a id="delete"></a>Azure Cosmos DB-fiók törlése
egy Azure Cosmos DB tooremove hello Azure portál, amely már nem használ, kattintson a jobb gombbal hello fiók nevét, a fiókot, és kattintson a **fiók törlése**.

![Hogyan toodelete egy Azure Cosmos DB fiókként hello Azure portál](./media/manage-account/deleteaccount.png)

1. A hello [Azure-portálon](https://portal.azure.com/), toodelete kívánja hello Azure Cosmos DB fiók eléréséhez.
2. A hello **Azure Cosmos DB fiók** panelen kattintson a jobb gombbal a hello fiókot, és kattintson a **fiók törlése**. 
3. Az eredményül kapott megerősítő panelen hello írja be a hello Azure Cosmos DB fiók neve tooconfirm, amelyet az toodelete hello fiók.
4. Kattintson a hello **törlése** gombra.

![Hogyan toodelete egy Azure Cosmos DB fiókként hello Azure portál](./media/manage-account/delete-account-confirm.png)

## <a id="next"></a>Következő lépések
Ismerje meg, hogyan túl[Ismerkedés az Azure Cosmos DB fiókja](http://go.microsoft.com/fwlink/p/?LinkId=402364).

<!--Image references-->
[5]: ./media/manage-account/documentdb_change_consistency-1.png

<!--Reference style links - using these makes hello source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
