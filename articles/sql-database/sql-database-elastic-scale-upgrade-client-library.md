---
title: "aaaUpgrade toohello legújabb elastic database ügyféloldali kódtárának |} Microsoft Docs"
description: "Frissítse az alkalmazások és a Nuget segítségével könyvtárak"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: cc2c9179be4c53ca59cd24d832127cf277c6e695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-an-app-toouse-hello-latest-elastic-database-client-library"></a>Frissítse az alkalmazás toouse hello legújabb elastic database ügyféloldali kódtár
Új verzióit hello [Elastic Database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md) NuGetand hello NuGetPackage Manager felületén a Visual Studio elérhető. Frissítések hibajavításokat tartalmaz, és új képességeket hello ügyféloldali kódtár támogatja.

**Hello legújabb verziójához:** túl Ugrás[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Az alkalmazás hello új könyvtárral, valamint módosíthatja a meglévő Shard térkép Manager metaadatok tárolódnak az Azure SQL-adatbázisok toosupport új funkciókat.

Ezeket a lépéseket hajtja végre sorrendben biztosítja, hogy hello ügyféloldali kódtár régi verziói már nem telepítve legyen a környezetben metaadat-objektum frissítésekor, ami azt jelenti, hogy a régi verziójú metaadat-objektum nem jön létre a frissítés után.   

## <a name="upgrade-steps"></a>Frissítési lépések
**1. Az alkalmazások frissítése.** A Visual Studio, a letöltés és a hivatkozás hello ügyfél könyvtár legújabb be a fejlesztési projektek hello Library; szalagtárat használó összes majd újraépítése, és telepítheti. 

* Válassza ki a Visual Studio megoldás **eszközök** --> **NuGet-Csomagkezelő** -->  **NuGet-csomagok kezelése megoldáshoz**. 
* (A visual Studio 2013) Hello bal oldali panelen, jelölje ki a **frissítések**, majd válassza ki a hello **frissítés** hello csomag gombjára **Azure SQL Database rugalmas méretezési ügyféloldali kódtár** hello megjelenő ablak.
* (A visual Studio 2015) Állítsa be a szűrőmezőbe hello túl**elérhető frissítés**. Jelölje ki a hello csomag tooupdate, majd kattintson a hello **frissítés** gombra.
* (A visual Studio 2017) Hello felső hello párbeszédpanel, válassza ki a **frissítések**. Jelölje ki a hello csomag tooupdate, majd kattintson a hello **frissítés** gombra.
* Hozza létre és telepítheti. 

**2. Frissítse a parancsfájlokat.** Ha használ **PowerShell** toomanage szilánkok, a parancsfájlok [hello új szalagtár verzió letöltéséhez](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) , és másolja azt hello könyvtárba, amelyből végre parancsfájlokat. 

**3. Frissítse a felosztás egyesítéses szolgáltatást.** Ha hello rugalmas adatbázis vegyes egyesítéses eszköz tooreorganize horizontálisan skálázott adatok, [töltse le és telepítse a legújabb verziójának hello hello eszköz](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Hello szolgáltatás található a frissítési lépések részletes [Itt](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Frissítse a Shard térkép Manager adatbázisainak**. A szilánkok Maps támogatása az Azure SQL Database hello metaadatok frissítéséhez.  Ez elvégezhető, PowerShell vagy a C# segítségével két módja van. Mindkét lehetőség alább láthatók.

***1. lehetőség: A frissítési metaadatok PowerShell használatával***

1. Hello legújabb parancssori segédprogramot letölteni a NuGet [Itt](http://nuget.org/nuget.exe) és tooa mappa mentéséhez. 
2. Nyisson meg egy parancssort, lépjen a toohello ugyanabban a mappában, és probléma hello parancsot:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Keresse meg a toohello almappa tartalmazó hello új DLL ügyfélverzió imént letöltött, például:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Hello rugalmas adatbázis ügyfél frissítési szkriptlet letöltését hello [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), és menteni a hello azonos tartalmazó mappa hello dll-Fájljában.
5. Ebből a könyvtárból futtassa a "PowerShell.\upgrade.ps1" hello parancssorból, és hello útmutatást követve.

***2. lehetőség: A frissítési metaadatok használatával C#***

Alternatív megoldásként hozzon létre egy Visual Studio alkalmazás, amely megnyitja a ShardMapManager elemein végiglépkedve összes szilánkok és hello metaadatok frissítést végez hello metódusok meghívása [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) és [ UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) ebben a példában látható módon: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Ezek a technológiák a metaadatok frissítésekre alkalmazható többször kárt nélkül. Például ha egy régebbi ügyfélverzió véletlenül egy shard hoz létre, miután már frissítette, frissítést futtathatja újra közötti összes szilánkok tooensure adott hello legfrissebb metaadatok megtalálható az infrastruktúra egészében. 

**Megjegyzés:** új hello ügyféloldali kódtár közzétett-date továbbra is toowork előzetes verzióihoz hello Shard térkép kezelő metaadatainak az Azure SQL Database, és fordítva.   Tootake előnyeit, néhány új szolgáltatását hello hello legújabb ügyfélprogram, metaadatok azonban toobe frissíteni kell.   Vegye figyelembe, hogy metaadat-frissítések nem érinti a felhasználó-adatokat és vonatkozó adatokat, csak objektumok hello Shard térkép Manager által létrehozott és használt.  És alkalmazások továbbra is toooperate hello frissítési sorrend a fent leírt keresztül. 

## <a name="elastic-database-client-version-history"></a>A rugalmas adatbázis ügyfél verziójának előzményei
A korábbi verziók go túl[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png

