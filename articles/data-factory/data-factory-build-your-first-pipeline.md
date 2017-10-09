---
title: "Data Factory-oktatóanyag: adatok első folyamatát |} Microsoft Docs"
description: "Az Azure Data Factory oktatóanyag bemutatja, hogyan toocreate és feldolgozza a Hive eszközzel adatokat tartalmazó data factory ütemezés parancsfájl a Hadoop-fürthöz."
services: data-factory
keywords: "az Azure data factory oktatóanyag, hadoop-fürt, hadoop hive"
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.assetid: 81f36c76-6e78-4d93-a3f2-0317b413f1d0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: ed9c0ade4500d4ac1f7c2c2312c1fa675e0b1f02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-pipeline-tootransform-data-using-hadoop-cluster"></a>Oktatóanyag: Az első adatcsatorna tootransform adatok Hadoop-fürt létrehozása
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Ebben az oktatóanyagban a első Azure data factory és olyan adatokat hoz létre. hello csővezeték bemeneti adatokat az Azure HDInsight (Hadoop) fürt tooproduce kimeneti adatokat a Hive parancsfájl futtatásával alakítja át.  

Ez a cikk áttekintése és hello oktatóanyag előfeltételei tartalmazza. Hello Előfeltételek befejezése után elvégezhető a következő eszközök/SDK-k hello egyikével hello oktatóanyag: Azure-portálon, a Visual Studio, a PowerShell, a Resource Manager-sablon, a REST API-t. Válasszon hello lehetőségek közül hello legördülő lista hello kezdete (vagy) hivatkozások a jelen cikk toodo hello oktatóanyag ezek a lehetőségek egyikének használatával hello végén.    

## <a name="tutorial-overview"></a>Az oktatóanyag áttekintése
Ebben az oktatóanyagban hajtsa végre a lépéseket követve hello:

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több, helyezze át, és az adatok átalakítása adatok folyamatok. 

    Ebben az oktatóanyagban létrehoz egy folyamat hello adat-előállítóban. 
2. Hozzon létre egy **csővezeték**. Egy folyamat rendelkezhet egy vagy több tevékenységet (Példa: másolási tevékenység során, a HDInsight Hive tevékenység). A példa a Hive parancsfájlok futtatására szolgál egy HDInsight Hadoop-fürt HDInsight Hive-tevékenység hello használja. hello parancsfájl először létrehoz egy táblát, amely hivatkozások hello Azure blob storage-ban tárolt nyers webes naplóadatokat, és ezután partíciók hello által évhez és hónaphoz nyers adatok.

    Ebben az oktatóanyagban hello folyamat futtatja a Hive-lekérdezések egy Azure HDInsight Hadoop-fürt hello Hive tevékenységek tootransform adatait használja. 
3. Hozzon létre **összekapcsolt szolgáltatások**. A társított szolgáltatás toolink adattárat, vagy egy számítási szolgáltatás toohello adat-előállító létrehozása. Egy adattárból, például az Azure Storage hello feldolgozási soros tevékenységek bemeneti/kimeneti adatokat tartalmazza. A számítási szolgáltatás, például a HDInsight Hadoop-fürt folyamatok/átalakítások adatokat.

    Ebben az oktatóanyagban létrehoz két összekapcsolt szolgáltatások: **Azure Storage** és **Azure HDInsight**. hello Azure Storage társított szolgáltatás hivatkozások egy Azure Storage-fiók, amely a bemeneti/kimeneti adatok toohello adat-előállító hello tárolja. Az Azure HDInsight kapcsolódó szolgáltatás hivatkozásokat tartalmaz, amely használt tootransform adatok toohello adat-előállító Azure HDInsight-fürtöt. 
3. Hozzon létre a bemeneti és kimeneti **adatkészletek**. Egy bemeneti adatkészlet hello feldolgozási soros tevékenység hello a megadott és egy kimeneti adatkészlet hello kimeneti hello tevékenység jelenti.

    Az oktatóanyag, hello bemeneti és kimeneti adatkészletek bemeneti helyét adja meg, és a kimeneti adatok hello Azure Blob Storage tárolóban. hello Azure tárolás társított szolgáltatásának határozza meg, mi az Azure Storage-fiókot használni. Egy bemeneti adatkészlet határozza meg, ahol hello bemeneti fájlok találhatók, és egy kimeneti adatkészlet határozza meg, ahol hello kimeneti fájlok kerülnek. 


Lásd: [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk részletes áttekintés az Azure Data Factory.
  
Íme hello **diagram nézet** hello minta adat-előállító létrehozása ebben az oktatóanyagban a. **MyFirstPipeline** egy tevékenysége, amely akkor Hive típusú **AzureBlobInput** bemeneti és előállított dataset **AzureBlobOutput** egy kimeneti adatkészletet. 

![A Data Factory oktatóanyag diagram nézet](media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)


Ebben az oktatóanyagban **inputdata** mappában található hello **adfgetstarted** Azure blob-tároló input.log nevű egyetlen fájlt tartalmaz. Ez a naplófájl a három hónapos bejegyzésekkel rendelkezik: január, február és a 2016. március. Az alábbiakban hello minta sorok havonta hello bemeneti fájl. 

```
2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

Hello az adatcsatorna HDInsight Hive tevékenységet hello fájl feldolgozása után hello tevékenység egy parancsprogramot futtathat Hive hello HDInsight-fürt partíciókat, hogy évhez és hónaphoz által bemeneti adatai. hello parancsfájl három kimeneti mappa, amely tartalmaz minden hónap bejegyzéseinek fájlt hoz létre.  

```
adfgetstarted/partitioneddata/year=2016/month=1/000000_0
adfgetstarted/partitioneddata/year=2016/month=2/000000_0
adfgetstarted/partitioneddata/year=2016/month=3/000000_0
```

Hello minta sorok fent látható, hello először egy (-2016-01-01) nyelven van megírva toohello 000000_0 fájl hello hónap = 1 mappa. Ehhez hasonlóan hello második íródott toohello fájl hello hónap = 2 mappa és hello harmadik egyik nyelven van megírva toohello fájl hello hónap = 3 mappa.  

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő előfeltételek hello kell rendelkeznie:

1. **Azure-előfizetés** – Ha nem rendelkezik Azure-előfizetéssel, is létrehozhat egy ingyenes próbafiók néhány percig. Lásd: hello [ingyenes próba](https://azure.microsoft.com/pricing/free-trial/) foglalkozó hogyan beszerezhet egy ingyenes próbafiókot.
2. **Az Azure Storage** – hello adattárolás ebben az oktatóanyagban egy általános célú standard Azure-tárfiókot használja. Ha egy általános célú szabványos Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk. A létrehozást követően hello tárfiókot, jegyezze fel hello **fióknév** és **elérési kulcsot**. Lásd: [megtekintése, másolása és újragenerálása tárolási hívóbetűk](../storage/common/storage-create-storage-account.md#view-and-copy-storage-access-keys).
3. Töltse le, és tekintse át a hello Hive lekérdezés fájl (**HQL**) helyen: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql](https://adftutorialfiles.blob.core.windows.net/hivetutorial/partitionweblogs.hql). Ez a lekérdezés átalakítja a bemeneti adatok tooproduce kimeneti adatokat. 
4. Töltse le, és tekintse át a hello minta bemeneti fájl (**input.log**) helyen: [https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log](https://adftutorialfiles.blob.core.windows.net/hivetutorial/input.log)
5. Hozzon létre egy blob-tároló nevű **adfgetstarted** az Azure Blob Storage tárolóban. 
6. Töltse fel **partitionweblogs.hql** toohello fájl **parancsfájl** hello mappájában **adfgetstarted** tároló. Használjon például az eszközök [Microsoft Azure Tártallózó](http://storageexplorer.com/). 
7. Töltse fel **input.log** toohello fájl **inputdata** hello mappájában **adfgetstarted** tároló. 

Hello Előfeltételek befejezése után a következő eszközök/SDK-k toodo hello oktatóanyag hello közül választhat: 

- [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Azure-portál és a Visual Studio adja meg az adat-előállítók kialakításának grafikus felhasználói Felülettel módot. Mivel a PowerShell, a Resource Manager-sablon és a REST API-beállítások az adat-előállítók kialakításának scripting/programozási megoldást kínál.

> [!NOTE]
> hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat. Azt nem másolja az adatokat egy forrás adatokat tároló tooa cél adattárból. Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után). Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket. 





  
