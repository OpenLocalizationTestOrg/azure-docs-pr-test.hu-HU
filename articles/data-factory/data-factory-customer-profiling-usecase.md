---
title: "aaaUse eset - ügyfelek adatainak összegyűjtése"
description: "Megtudhatja, hogyan Azure Data Factory az adatvezérelt használt toocreate (adatcsatorna) munkafolyamat tooprofile játék ügyfelek."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 47f5e77242366c80cce2a2db65e3c696505b3e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-case---customer-profiling"></a>Használati eset – Ügyfélprofil-készítés
Az Azure Data Factory sok használt szolgáltatások tooimplement hello Cortana Intelligence Suite a megoldás gyorsítók egyike.  További információt a Cortana Intelligence [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). Ez a dokumentum azt ismerteti egy egyszerű használható case toohelp megérteni, hogyan Azure Data Factory is megoldásában közös analytics használatának első lépéseit.

## <a name="scenario"></a>Forgatókönyv
Contoso által létrehozott több platformot játékok játék vállalat esetében: játékok konzolok, kézi eszköz eszközök és személyes számítógépek. Játékosok a játékokat, mivel a naplóadatok nagy mennyiségű jön létre, hogy nyomon követi hello használati szokásokról, játék stílus és beállítások hello felhasználó.  Demográfiai, regionális, kombinálva és a termék adatokat, a Contoso analytics tooguide őket arról, hogyan tooenhance játékosok élmény és a cél azokat a frissítéseket és a játékbeli hajthat végre a későbbi szoftvervásárlások. 

Contoso-cél tooidentify felfelé-értékesít/kereszt-értékesítési lehetőségek a játékosok hello játék előzményei alapján és kényszerítő szolgáltatások toodrive üzleti növekedési hozzáadása, és adjon meg egy jobb felhasználói élmény toocustomers. A használati eset, az egy játék vállalati stratégiai példaként használjuk. hello vállalat szeretne toooptimize a játékok játékosok viselkedés alapján. Az alapelvek tooany üzleti, hogy tooengage próbál alkalmazni az ügyfelek a termékek és szolgáltatások köré, és az ügyfelek élmény javítása érdekében.

Ebben a megoldásban a Contoso biztosítani a közelmúltban elindította marketingkampányt tooevaluate hello hatékonyságát. Azt kezdődnie hello nyers játék naplókat, folyamat és azokat a földrajzi hely meghatározásának adatok kiegészítése, csatlakozik, a referenciaadatok hirdetési, és végül másolja őket egy Azure SQL Database tooanalyze hello kampány hatása.

## <a name="deploy-solution"></a>Megoldás üzembe helyezéséhez
Az összes szükséges tooaccess, és próbálja meg az egyszerű használati eset egy [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/), egy [Azure Blob storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account), és egy [Azure SQL Database](../sql-database/sql-database-get-started.md). Profilkészítési hello csővezetéket hello ügyfél központi telepítése **folyamatok minta** hello kezdőlapján a data factory csempére.

1. Egy adat-előállító létrehozása, vagy nyissa meg valamelyik adat-előállítót. Lásd: [adatokat másolni a Blob Storage tooSQL adatbázis használata a Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) lépéseket toocreate egy adat-előállító esetében.
2. A hello **adat-előállító** hello adat-előállító paneljén kattintson hello **folyamatok minta** csempére.

    ![A minta folyamatok csempe](./media/data-factory-samples/SamplePipelinesTile.png)
3. A hello **folyamatok minta** panelen hello kattintson **profilkészítési ügyfél** , amelyet az toodeploy.

    ![Minta folyamatok panel](./media/data-factory-samples/SampleTile.png)
4. Adja meg a hello minta konfigurációs beállításait. Például az az Azure storage-fiók neve és kulcs, Azure SQL-kiszolgáló neve, adatbázis, felhasználói Azonosítót és jelszót.

    ![A minta panel](./media/data-factory-samples/SampleBlade.png)
5. Miután végzett hello konfigurációs beállítások megadásával, kattintson **létrehozása** toocreate/telepítés hello minta folyamatok és a kapcsolódó szolgáltatások, illetve táblákat hello folyamatok által használt.
6. Központi telepítés állapotát hello hello minta csempe kattintott a korábban a hello látható **folyamatok minta** panelen.

    ![Üzembe helyezés állapota](./media/data-factory-samples/DeploymentStatus.png)
7. Amikor megjelenik a hello **KözpontiTelepítés sikerült** hello minta Bezárás hello hello mozaikokon üzenet **folyamatok minta** panelen.  
8. A **adat-előállító** panelen látja, hogy összekapcsolt szolgáltatások adathalmazok és adatcsatornák tooyour adat-előállító kerülnek.  

    ![A Data Factory panel](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Megoldási áttekintés
Ez egyszerű használati eset példa bemutatja, hogyan használható az Azure Data Factory tooingest, előkészítése, átalakíthatja, elemzése, és adatok közzététele is használható.

![Teljes körű munkafolyamat](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Ábra mutatja be, hogyan hello adatok folyamatok jelennek meg hello Azure-portálon telepítés után.

1. Hello **PartitionGameLogsPipeline** hello nyers játék események beolvassa a blob storage és év, hónap és nap alapján létrehozott partícióknak hoz létre.
2. Hello **EnrichGameLogsPipeline** földrajzi kód referenciaadatokkal particionált játék események csatlakozik, valamint a következőképpen színesíti hello adatok leképezése IP címek toohello megfelelő földrajzi-helyek által.
3. Hello **AnalyzeMarketingCampaignPipeline** csővezeték dúsított hello adatokat használ, és feldolgozza őket az adatok toocreate hello végső kimenetet, amely tartalmazza a kampány marketingtevékenység hatékonyságát hirdetési hello.

Ebben a példában a Data Factory másolja a bemeneti adatok, átalakítás és folyamat hello adataihoz, és a kimeneti hello végső adatok tooan Azure SQL Database használt tooorchestrate tevékenységeket.  Az adatok adatcsatornák hello hálózati megjelenítheti, kezelheti azokat, és azok a felhasználói felület hello állapotának figyelése.

## <a name="benefits"></a>Előnyök
A felhasználói profil analytics optimalizálása, és azt az üzleti céljaihoz igazítása, játék vállalati képes tooquickly gyűjt használati szokásokról, és elemzése a marketingkampányok hello hatékonyságát.

