---
title: "a shard térkép manager aaaPerformance számlálói"
description: "ShardMapManager osztály- és a függő útválasztási teljesítményszámlálói"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: b090aba0-2e30-454c-96b3-dffa281f539a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: ddove
ms.openlocfilehash: d24198563d9fa88d12e6c464dbe89bc300e72ca0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-counters-for-shard-map-manager"></a>Teljesítményszámlálók a szilánkleképezés-kezelőhöz
Rögzítheti a hello teljesítményét egy [shard térkép manager](sql-database-elastic-scale-shard-map-management.md), különösen akkor, ha használatával [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md). Számlálók hello Microsoft.Azure.SqlDatabase.ElasticScale.Client osztály metódusai jönnek létre.  

Számlálók használt tootrack hello teljesítményének [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md) műveletek. Ezek a számlálók a Teljesítményfigyelőben, hello hello "Rugalmas adatbázis: a Shard kezelése" kategóriához tartozó érhetők el.

**Hello legújabb verziójához:** túl Ugrás[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Lásd még: [frissítse az alkalmazás toouse hello legújabb elastic database ügyféloldali kódtár](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Előfeltételek
* toocreate hello teljesítménykategória és -számlálók, hello felhasználói hello helyi egy részét kell **rendszergazdák** csoport hello hello alkalmazást futtató számítógépen.  
* a teljesítmény toocreate a teljesítményszámláló-példány, és frissítse a hello számlálók, hello felhasználói vagy hello tagjának kell lennie **rendszergazdák** vagy **Teljesítményfigyelő felhasználók** csoport. 

## <a name="create-performance-category-and-counters"></a>Hozzon létre teljesítménykategória és -számlálók
toocreate hello számlálók, hívja hello CreatePeformanceCategoryAndCounters hello [ShardMapManagmentFactory osztály](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Csak a rendszergazda hello metódus segítségével hajtható végre: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Is [ez](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) hello tooexecute PowerShell parancsfájlt. hello hoz létre a következő teljesítményszámlálók hello:  

* **Gyorsítótárazott hozzárendelések**: hello shard térkép gyorsítótárazott hozzárendelések száma.
* **DDR művelet/mp**: hello shard térkép útválasztási műveletek függő adatokat. Ez a számláló akkor frissül, amikor hívása túl[OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) a sikeres kapcsolat toohello cél shard eredményez. 
* **Keresési találatok száma gyorsítótárban másodpercenként leképezési**: sikeres gyorsítótár keresési műveletek leképezéseihez hello shard leképezés. 
* **Keresési gyorsítótár-tévesztései/másodperc leképezési**: gyorsítótár sikertelen keresési műveletek leképezéseihez hello shard leképezés.
* **Hozzárendelések, vagy a gyorsítótár másodpercenkénti frissített**: mely hozzárendelések közé éppen felvett vagy a frissített gyorsítótár hello shard térkép aránya. 
* **Hozzárendelések eltávolítja a gyorsítótár másodpercenkénti**: sebesség, amellyel hozzárendelések el hello shard térkép-gyorsítótárból. 

Teljesítményszámlálók minden gyorsítótárazott shard leképezés folyamatonként jön létre.  

## <a name="notes"></a>Megjegyzések
hello alábbi eseményeket indít hello teljesítményszámlálók hello létrehozása:  

* Hello inicializálása [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) rendelkező [különösen betöltése](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), ha hello ShardMapManager tartalmazza a szilánkcímtárban. Ezek közé tartozik a hello [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) és hello [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) módszerek.
* A szilánkok leképezés sikeres keresési (használatával [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) vagy [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* A sikeres létrehozást shard térkép CreateShardMap() használatával.

hello teljesítményszámlálók frissíti a hello shard térkép és hozzárendelések végrehajtott összes gyorsítótár-műveletekhez. Sikeres eltávolítás hello shard térkép DeleteShardMap () reults használatát hello teljesítménypéldány számlálók törlése.  

## <a name="best-practices"></a>Ajánlott eljárások
* Létrehozási hello teljesítménykategória és a számláló csak egyszer hello ShardMapManager objektum létrehozása előtt kell elvégezni. Minden hello parancs végrehajtása a CreatePerformanceCategoryAndCounters() törli hello előző számlálók (összes példány által küldött adatok elvesztése), és újakat hoz létre.  
* Teljesítmény számlálópéldány folyamatonként jönnek létre. Bármely alkalmazás összeomlása és -eltávolítási szilánkok leképezés hello gyorsítótárból hello teljesítmény számlálók példányok törlése eredményez.  

### <a name="see-also"></a>Lásd még:
[A rugalmas adatbázis-szolgáltatások áttekintése](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

