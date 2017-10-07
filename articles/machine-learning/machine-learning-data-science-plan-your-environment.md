---
title: "aaaIdentify forgatókönyvek és az elemzés során - Azure megtervezése |} Microsoft Docs"
description: "Tervezze meg a speciális elemzés fő kérdések sorát teszi figyelembe véve."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 421520dd-7728-4d29-889c-ebe6a0a6fb07
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: e445973be0d020a4f9949e5c9d8554fbbd4b515f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooidentify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Hogyan tooidentify forgatókönyvek és tervezése advanced analytics adatok feldolgozása
Milyen erőforrásokat kell tervezi tooinclude egy környezet toodo beállítása a bővített feldolgozás esetén a dataset analytics? Ez a cikk javasol kérdések tooask azonosításához hello feladatok és a kapcsolódó erőforrások sorozata a forgatókönyvéhez. hello ahhoz, hogy a prediktív elemzés magas szintű lépéseket mutatja be [hello Team adatok tudományos folyamat (TDSP) újdonságai?](data-science-process-overview.md). Egy adott erőforráshoz hello feladatok megfelelő tooyour adott forgatókönyv egyes lépéseket igényel. hello hasonló fontos kérdések tooidentify a forgatókönyv probléma adatok logisztikai, jellemzők, ennek hello adatkészleteket, és hello eszközök és nyelvek toodo hello elemzés inkább hello minőségét.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logisztikai kérdések: adatok helye és a mozgás
hello logisztikai kérdéseket vonatkoznak hello hello helyét **adatforrás**, hello **célhelyre** Azure-ban, és mozgó hello adatokra vonatkozó követelmények, beleértve a hello ütemezés, mennyiségét és erőforrások érint. hello adatok esetleg toobe többször áthelyezése hello analytics folyamat során. Egy általános forgatókönyv toomove helyi adatok át Azure storage működik valamilyen formában, majd a Machine Learning Studióhoz.

1. **Mi az az adatforrást?** Ennyi az egész helyi vagy hello felhőben? Példa:
   
   * hello adata nyilvánosan elérhető HTTP-cím.
   * hello adatok egy helyi vagy hálózati fájl helyen található.
   * hello adatokat az SQL Server-adatbázisban.
   * hello adatai egy Azure storage-tároló
2. **Mi az Azure cél hello?** Ha nem, a kell toobe feldolgozása vagy modellezési? Példa:
   
   * Azure Blob Storage
   * SQL Azure-adatbázisok
   * Azure virtuális gépen futó SQL Server
   * HDInsight (Hadoop az Azure-on) vagy a Hive táblák
   * Azure Machine Learning
   * Csatlakoztatható Azure virtuális merevlemezeket.
3. **Hogyan lesz a toomove hello adatokat?**  hello eljárások és erőforrások elérhető tooingest vagy terheléselosztási adatok eltérőek a tárolási és környezetekben feldolgozása különböző módszereket a hello a következő témaköröket.
   
   * [Adatok betöltése az elemzés a tárolási környezetekben](machine-learning-data-science-ingest-data.md)
   * [A betanítási adatok importálása az Azure Machine Learning Studio a különféle adatforrásokból származó](machine-learning-data-science-import-data.md).
4. **Szükséges rendszeres időközönként áthelyezték, vagy az áttelepítés során módosíthat toobe hello adatokat?** Azure Data Factory (ADF) esetekben érdemes folyamatosan áttelepített adatok igényeinek toobe, különösen akkor, ha egy hibrid forgatókönyvben, amely hozzáfér a helyszíni és felhőalapú erőforrásokat is szerepet kap, amikor hello adatai vagy van tranzakcióalapú vagy toobe módosítani kell üzleti logika rendelkezik hozzáadott tooit hello keretében áttelepítendő. További információkért lásd: [tárolt adatok mozgatása egy helyi SQL server tooSQL Azure az Azure Data Factoryvel](machine-learning-data-science-move-sql-azure-adf.md)
5. **Hello adatok mekkora áthelyezése toobe tooAzure?** Nagyon nagy adatkészletek túlléphetik a hello tárolókapacitását bizonyos környezetekben. Egy vonatkozó példáért lásd: hello értékelése méretkorlátait a Machine Learning Studio hello a következő szakaszban. Ebben az esetben egy minta hello adatok hello elemzési során is használható. Hogyan toodown-minta a DataSet adatkészlet különböző Azure-alapú környezetekben, lásd: [az adatokat a csapat az tudományos folyamata hello](machine-learning-data-science-sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Adatok jellemzőit kérdések: típusa, formátum és mérete
Ezeket a kérdéseket kulcs tooplanning tárhelyét, és feldolgozási környezetek, amelyek, megfelelő toovarious típusú adatokat, és amelyek bizonyos korlátozások.

1. **Mik azok a hello adattípusok?** Példa:
   
   * Numerikus
   * Kategorikus
   * Karakterláncok
   * Bináris
2. **Az adatok formázását?** Példa:
   
   * Vesszővel tagolt (CSV) vagy (TSV) egybesimított fájlokba tabulátorral tagolt
   * Tömörített és tömörítetlen
   * Azure-blobokat
   * Hadoop Hive táblák
   * SQL Server-táblákra
3. **Mekkora az adatokat?**
   
   * Kis: 2GB-nál kisebb
   * Közepes: Nagyobb, mint 2GB és 10GB-nál kisebb
   * Nagy: 10GB-nál nagyobb

Hello Azure Machine Learning Studio környezet például érvénybe:

* Hello adatok formátumú és az Azure Machine Learning Studio által támogatott eszköztípusok listájáért lásd: [adatok formátumok és a támogatott adattípusok](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) szakasz.
* Információ az Azure Machine Learning Studio adatok korlátozásai: hello **hogyan nagy hello adatkészlet lehet a modulok?** szakasza [importálása és exportálása az adatok a Machine Learning szolgáltatáshoz](machine-learning-faq.md#machine-learning-studio-questions)

A más hello analytics folyamat során használt Azure-szolgáltatások korlátozásai hello információkért lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Minőségi kérdésekre: feltárására és előzetes feldolgozás
1. **Mi ismeri az adatokkal kapcsolatban?** Adatokba Ha toogain egy megértése alapvető jellemzőit. Hány értékek hiányoznak, vagy mi mintákra vagy a trendeket, exhibits, mi kiugró tartozik. Ebben a lépésben szükség esetén, amelyek hello legmegfelelőbb szolgáltatások javaslat vagy írja be a elemzési feltételezéseket vonatkozókat, és vonatkozókat kapcsolatos további adatok gyűjtése előzetes feldolgozás hello mértékének meghatározásához fontos. Leíró statisztika kiszámításához és képi megjelenítéseket olyan hasznos technikákat adatok végzett vizsgálat végrehajtásához. Hogyan tooexplore különböző Azure környezetben dataset: részletes [hello csapat az tudományos folyamata az adatokba](machine-learning-data-science-explore-data.md).
2. **Hello adatok előzetes feldolgozás, vagy tisztítás szükséges?**
   Előzetesen feldolgozni, és az adatok tisztítása, amelyek általában kell elvégzése előtt a DataSet adatkészlet nem használható hatékonyan gépi tanulás fontos feladatokat. Nyers adatok gyakran zajos vagy nem megbízható, és előfordulhat, hogy értékek hiányzik. Ilyen adatok használata a modellezési félrevezető eredményeket hozhat létre. Ismertetését lásd: [tooprepare adatainak továbbfejlesztett gépi tanulási feladatok](machine-learning-data-science-prepare-data.md).

## <a name="tools-and-languages-questions"></a>Eszközök és nyelvek vonatkozó kérdések
Az számos beállítások itt attól függően, hogy milyen nyelveket és fejlesztői környezetek vagy eszközök kell, vagy a conformable használja.

1. **Nyelvek miről inkább toouse elemzés?**  
   
   * R
   * Python
   * SQL
2. **Milyen eszközöket használja az adatok elemzésére?**
   
   * [A Microsoft Azure Powershell](/powershell/azure/overview) -parancsfájl nyelv használt tooadminister az Azure-erőforrások parancsfájl nyelven.
   * [Az Azure Machine Learning Studióban](machine-learning-what-is-ml-studio.md)
   * [Fordulat elemzés](http://www.revolutionanalytics.com/revolution-r-open)
   * [Rstudióból](http://www.rstudio.com)
   * [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Jupyter notebookok](http://jupyter.org/)
   * [Microsoft Power BI](http://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>A speciális elemzés forgatókönyv azonosítása
Ha megválaszolta az előző szakaszban hello hello kérdéseket, áll készen toodetermine melyik a legjobb forgatókönyv megfelel az Ön esetében. hello szituáció vázolt [forgatókönyvek az Azure Machine Learning speciális elemzésekre](machine-learning-data-science-plan-sample-scenarios.md).

