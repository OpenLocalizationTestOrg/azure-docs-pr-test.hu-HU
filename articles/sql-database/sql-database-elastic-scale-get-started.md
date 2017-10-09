---
title: "aaaGet rugalmas adatbáziseszközöket használatába |} Microsoft Docs"
description: "Alapszintű hello rugalmas adatbázis eszközök szolgáltatás az Azure SQL Database, beleértve egy egyszerű alkalmazást mintaalkalmazás ismertetése."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: a84e05c39dffbaef440538602f898acee6e0483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Ismerkedés a rugalmas adatbázis-eszközök
Ez a dokumentum bemutatja a toohello a fejlesztői változat segítenek toorun hello mintaalkalmazást. hello minta létrehoz egy egyszerű szilánkos alkalmazást, és felderíti a rugalmas adatbáziseszközöket főbb képességei. hello mutatja be hello feladatai [elastic database ügyféloldali kódtárának](sql-database-elastic-database-client-library.md).

tooinstall hello könyvtár, nyissa meg túl[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). hello könyvtár hello mintaalkalmazás hello a következő szakaszban ismertetett együtt települ.

## <a name="prerequisites"></a>Előfeltételek
* A Visual Studio 2012 vagy újabb a C#. Töltse le a szabad verzió [Visual Studio letöltések](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 vagy újabb. tooget hello legújabb verziójára, lásd: [NuGet telepítése](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="download-and-run-hello-sample-app"></a>Hello minta alkalmazás letöltése és futtatása
Hello **rugalmas adatbázis-eszközök az Azure SQL - első lépések** minta bemutatja hello legfontosabb szempontja hello fejlesztési felület szilánkos alkalmazás esetében, amely rugalmas adatbáziseszközöket használhat. Kulcs alkalmazási helyzetei összpontosít [shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md), [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md), és [több shard lekérdezése](sql-database-elastic-scale-multishard-querying.md). toodownload és futtatási hello minta, kövesse az alábbi lépéseket: 

1. Töltse le a hello [rugalmas adatbázis-eszközök az Azure SQL - Bevezetés a minta](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) msdn. Bontsa ki a hello minta tooa hely.

2. toocreate a projekt megnyitása hello **ElasticScaleStarterKit.sln** hello megoldást **C#** könyvtár.

3. Hello mintaprojektet hello megoldást, nyissa meg hello **app.config** fájlt. Az Azure SQL adatbázis-kiszolgáló nevét és a bejelentkezési adatait (felhasználónév és jelszó), kövesse a hello fájl tooadd hello utasításokat.

4. Hozza létre és hello alkalmazás futtatásához. Amikor a rendszer kéri, engedélyezze a Visual Studio toorestore hello NuGet-csomagok hello megoldás. Ez letölti a hello hello elastic database ügyféloldali kódtárának legújabb verzióját a NuGet.

5. Hello különböző beállítások toolearn több hello ügyfél könyvtár képességekre vonatkozó kísérletezhet. Megjegyzés: hello lépéseket alkalmazás vesz igénybe, a konzol kimeneti hello hello és szabad tooexplore hello kód hello háttérben látja.
   
    ![Folyamatban van][4]

Congratulations--sikeresen létrehozva, és az első szilánkos alkalmazás SQL Database rugalmas adatbáziseszközöket használatával futtassa. Visual Studio vagy SQL Server Management Studio tooconnect tooyour SQL database szolgáltatást használna, és tegye meg, hogy a létrehozott minta hello hello szilánkok gyorsan át. Új minta shard adatbázisok és a shard manager adatbázist láthatja, hogy hello minta hozott létre.

> [!IMPORTANT]
> Javasoljuk, hogy mindig használjon hello Management Studio legújabb verzióját, hogy a szinkronizált frissítéseket tooAzure és SQL-adatbázis maradhat. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-hello-code-sample"></a>Kulcsfontosságú hello kódminta
* **Leképezések szilánkok és shard kezelése**: hello kód bemutatja, hogyan szilánkok, a tartományokkal és a leképezéseket hello fájlban toowork **ShardManagementUtils.cs**. További információkért lásd: [hello shard térkép manager adatbázisokkal kibővítési](http://go.microsoft.com/?linkid=9862595).  

* **Adatok függő útválasztási**: a tranzakciók toohello jobb shard útválasztását megjelenik-e a **DataDependentRoutingSample.cs**. További információkért lásd: [adatok függő útválasztási](http://go.microsoft.com/?linkid=9862596). 

* **Több szilánkok lekérdezését**: között szilánkok lekérdezését mutatja be hello fájl **MultiShardQuerySample.cs**. További információkért lásd: [több shard lekérdezése](http://go.microsoft.com/?linkid=9862597).

* **Üres szilánkok hozzáadása**: hello iteratív hozzáadása az új üres szilánkok hello kód hello fájlban történik **CreateShardSample.cs**. További információkért lásd: [hello shard térkép manager adatbázisokkal kibővítési](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Egyéb rugalmas bővítést műveletek
* **A felosztás egy meglévő shard**: hello funkció toosplit szilánkok hello által biztosított **vegyes egyesítéses eszköz**. További információkért lásd: [adatok kiterjesztett felhő adatbázisok közötti áthelyezése](sql-database-elastic-scale-overview-split-and-merge.md).

* **Az egyesítés meglévő szilánkok**: Shard összevonása is hello használatával végrehajtott **vegyes egyesítéses eszköz**. További információkért lásd: [adatok kiterjesztett felhő adatbázisok közötti áthelyezése](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Költségek
hello rugalmas adatbáziseszközöket szabadon. Rugalmas adatbáziseszközöket használhat, ha nem érkezik a további díjakat felett az Azure használatának hello költségét. 

Hello mintaalkalmazás például új adatbázisokat hoz létre. Ez hello költsége hello SQL adatbázis-kiadás úgy dönt, és az alkalmazás Azure használati hello függ.

Díjszabási információkért lásd: [díjszabása SQL-adatbázis](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Következő lépések
A rugalmas adatbázis-eszközökkel kapcsolatos további információkért tekintse meg a következő lapok hello:

* Kódminták: 
  * [Az Azure SQL - első lépések a rugalmas adatbázis-eszközök](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Az Azure SQL - entitás Keretrendszeri integrációját rugalmas adatbázis-eszközök](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [A parancsfájl-központ shard rugalmassága](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [rugalmas bővítést bejelentés](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* A Microsoft Virtual Academy: [végrehajtási kibővített használatával horizontális a hello rugalmas adatbázis ügyfél könyvtár videó](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* 9. csatornán: [rugalmas bővítést áttekintő videó](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Vitafóruma: [Azure SQL Database-fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* toomeasure teljesítmény: [shard térkép manager teljesítményszámlálói](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[hello Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run hello Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png

