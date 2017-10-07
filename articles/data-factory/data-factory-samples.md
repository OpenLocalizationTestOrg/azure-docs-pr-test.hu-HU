---
title: "aaaAzure Data Factory - minták"
description: "Minta, amelyeket a kapcsolatos adatokat biztosít a hello Azure Data Factory szolgáltatásnak."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: aa1c15eca21b34b7bcc64358b685d7606baaf691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---samples"></a>Az Azure Data Factory - minták
## <a name="samples-on-github"></a>Minták a Githubon
Hello [GitHub Azure-DataFactory-tárház](https://github.com/azure/azure-datafactory) tartalmaz, amelyek segítenek több mintát gyorsan Azure Data Factory szolgáltatással felkészülési (vagy) hello parancsfájlok módosíthatja, és felhasználhatja őket a saját alkalmazás. hello Samples\JSON mappa tartalmazza JSON kódtöredékek szabhatják.

| Minta | Leírás |
|:--- |:--- |
| [ADF forgatókönyv](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |Ez a minta egy végpontok közötti forgatókönyv Azure Data Factory tooturn adatokkal adatokat a naplófájlokból tooinsights a naplófájlok feldolgozásának biztosít. <br/><br/>Ebben a bemutatóban a hello Data Factory-folyamathoz gyűjt minta naplókat, folyamatok és következőképpen színesíti hello referenciaadatokkal naplók adatait, és átalakítja az hello adatok tooevaluate hello hatékonyságát, amelyek a közelmúltban elindult marketingkampányok. |
| [JSON-minták](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |Ez a minta szabhatják JSON példákat. |
| [HTTP letöltő minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |A minta showcases letöltésének egy HTTP-végpont tooAzure Blob-tároló adatait az egyéni .NET tevékenység. |
| [Kereszt-AppDomain pont nettó tevékenység minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Ez a minta egy egyéni .NET tevékenység, amely nem korlátozott hello ADF indító (például windowsazure.Storage kifejezésre v4.3.0 Newtonsoft.Json v6.0.x, stb.) által használt tooassembly verziók tooauthor lehetővé teszi. |
| [R-parancsfájl futtatása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Ez a minta hello adat-előállító egyéni tevékenységet tartalmaz, amelyek használt tooinvoke RScript.exe lehetnek. Ez a minta csak a saját (nem igény) HDInsight-fürt, amely már a R telepítve van rajta működik. |
| [A Spark HDInsight Hadoop-fürt egy feladat meghívása](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |Ez a példa bemutatja, hogyan toouse MapReduce tevékenység tooinvoke Spark programot. hello spark program egy Azure Blob-tároló tooanother csak másol adatokat. |
| [Twitter elemzése Azure Machine Learning kötegelt pontozási tevékenység használatával](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Ez a példa bemutatja, hogyan toouse AzureMLBatchScoringActivity tooinvoke az Azure Machine Learning modell, amely végrehajtja a twitter véleményeket elemzés, pontozási, előrejelzés stb. |
| [Az egyéni tevékenység Twitter-elemzés](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Ez a példa bemutatja, hogyan toouse egy egyéni .NET tevékenység tooinvoke az Azure Machine Learning-modell, amely elvégzi twitter véleményeket elemzés, pontozási, előrejelzés stb. |
| [Az Azure Machine Learning paraméteres folyamatok](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |hello minta egy végpont C# kód toodeploy N folyamatok pontozási és egy másik régióban paraméter, ahol hello régiók listáját, amelyen megtalálható ez a minta parameters.txt fájlból jön az átképezési biztosít. |
| [Hivatkozás az adatfrissítési Azure Stream Analytics-feladatok](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Ez a példa bemutatja, hogyan toouse Azure Data Factory és az Azure Stream Analytics együtt toorun hello tartalmazó lekérdezések hivatkozási adatokat és beállításokat hello a referenciaadatoknál ütemezés szerint frissítse. |
| [A helyszíni Hortonworks hadooppal hibrid folyamat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |hello minta egy helyszíni Hadoop-fürt egy számítási célként adat-előállítóban futó feladatok, ugyanúgy, mint például egy HDInsight Hadoop-fürt a felhő alapú más számítási célok felvenni használ. |
| [JSON-átalakítás eszköz](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Ez az eszköz lehetővé teszi tooconvert JSONs verziója előzetes too2015 07-01. dátumú előnézeti toolatest vagy 2015-07-01. dátumú előnézeti (alapértelmezett). |
| [U-SQL minta bemeneti fájl](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Ezt a fájlt egy U-SQL-tevékenység által használt mintafájl. |
| [A blob fájl törlése](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | Ez a minta egy C# fájlban hello fájlok másolása után az ADF egyéni .net tevékenység toodelete fájlok Azure Blob helyére hello forrásból részeként felhasználható bővíthető.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sablonok
Hello Azure Resource Manager-sablonok az adat-előállítót a következő a Githubon található.

| Sablon | Leírás |
| --- | --- |
| [Azure Blob Storage tooAzure SQL-adatbázis másolása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Ez a sablon telepítéséhez hoz létre egy Azure data factory egy folyamatot, hogy a hello másolatok adatait az Azure blob storage toohello Azure SQL adatbázis megadva |
| [A Blob Storage Salesforce tooAzure másolása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Ez a sablon telepítése hoz létre egy Azure data factory egy folyamatot, hogy hello másolatok adatait megadott Salesforce fiók toohello Azure blob Storage tárolóban. |
| [Adatok átalakítása Azure HDInsight-fürtök a Hive parancsfájl futtatásával](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Ez a sablon telepítése egy Azure data factory hoz létre egy folyamatot, amely átalakítja az adatokat az Azure HDInsight Hadoop-fürt hello minta Hive parancsfájl futtatásával. |

## <a name="samples-in-azure-portal"></a>Az Azure portál – minták
Használhatja a hello **folyamatok minta** tooyour adat-előállítóban hello kezdőlapján a data factory toodeploy minta folyamatok és a kapcsolódó entitásokra (adatkészletek és összekapcsolt szolgáltatások) csempén.

1. Egy adat-előállító létrehozása, vagy nyissa meg valamelyik adat-előállítót. Lásd: [adatokat másolni a Blob Storage tooSQL adatbázis használata a Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) lépéseket toocreate egy adat-előállító esetében.
2. A hello **adat-előállító** hello adat-előállító paneljén kattintson hello **folyamatok minta** csempére.

    ![A minta folyamatok csempe](./media/data-factory-samples/SamplePipelinesTile.png)
3. A hello **folyamatok minta** panelen hello kattintson **minta** , amelyet az toodeploy.

    ![Minta folyamatok panel](./media/data-factory-samples/SampleTile.png)
4. Adja meg a hello minta konfigurációs beállításait. Például az Azure storage-fiók nevét és a fiók kulcs Azure SQL-kiszolgáló neve, adatbázis, felhasználói Azonosítóját, és a jelszó, stb.

    ![A minta panel](./media/data-factory-samples/SampleBlade.png)
5. Miután végzett hello konfigurációs beállítások megadásával, kattintson **létrehozása** toocreate/telepítés hello minta folyamatok és a kapcsolódó szolgáltatások, illetve táblákat hello folyamatok által használt.
6. Központi telepítés állapotát hello hello minta csempe kattintott a korábban a hello látható **folyamatok minta** panelen.

    ![Üzembe helyezés állapota](./media/data-factory-samples/DeploymentStatus.png)
7. Amikor megjelenik a hello **KözpontiTelepítés sikerült** hello minta Bezárás hello hello mozaikokon üzenet **folyamatok minta** panelen.  
8. A **adat-előállító** panelen látja, hogy összekapcsolt szolgáltatások adathalmazok és adatcsatornák tooyour adat-előállító kerülnek.  

    ![A Data Factory panel](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>A Visual Studio minták
### <a name="prerequisites"></a>Előfeltételek
A számítógépen telepített hello következő kell rendelkeznie:

* Visual Studio 2013 vagy Visual Studio 2015
* Töltse le az Azure SDK-t a Visual Studio 2013-hoz vagy a Visual Studio 2015-höz. Keresse meg a túl[Azure letöltési oldalát](https://azure.microsoft.com/downloads/) kattintson **VS 2013** vagy **VS 2015** a hello **.NET** szakasz.
* Töltse le a legfrissebb hello Azure Data Factory beépülő modul a Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) vagy [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Visual Studio 2013 használatakor is frissítheti hello beépülő modul hello lépések végrehajtásával: hello menüjének **eszközök** -> **bővítmények és frissítések**  ->  **Online** -> **Visual Studio galériában** -> **Microsoft Azure Data Factory Tools for Visual Studio**  ->  **Frissítés**.

### <a name="use-data-factory-templates"></a>Használja a Data Factory sablonok
1. Kattintson a **fájl** hello menüben mutasson túl**új**, és kattintson a **projekt**.
2. A hello **új projekt** párbeszédpanel mezőbe hello a következő lépéseket:

   1. Válassza ki **DataFactory** alatt **sablonok**.
   2. Válassza ki **Data Factory sablonok** hello jobb oldali ablaktáblán.
   3. Adjon meg egy **neve** hello projekthez.
   4. Válassza ki a **hely** hello projekthez.
   5. Kattintson az **OK** gombra.

      ![A New project (Új projekt) párbeszédpanel](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. A hello **Data Factory sablonok** párbeszédpanel megnyitásához, jelölje be hello minta sablon hello **használati eset sablonok** szakaszt, és kattintson a **következő**. hello következő lépések végigvezetik hello segítségével **ügyfél profilkészítési** sablont. Lépések hasonlóak az egyéb minták hello.

    ![Data Factory sablonok párbeszédpanel](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. A hello **Data Factory konfigurációs** párbeszédpanel, kattintson a **következő** a hello **Data Factory alapjai** lap.
5. A hello **konfigurálása adat-előállító** lapján hello a következő lépéseket:
   1. Válassza ki **hozzon létre új adat-előállító**. Igény szerint kiválaszthatja **használja a meglévő adat-előállító**.
   2. Adjon meg egy **neve** hello adat-előállító esetében.
   3. Jelölje be hello **Azure-előfizetés** a kívánja hello data factory toobe létrehozni.
   4. Jelölje be hello **erőforráscsoport** hello adat-előállító esetében.
   5. Jelölje be hello **USA nyugati régiója**, **USA keleti régiója**, vagy **Észak-Európa** a hello **régió**.
   6. Kattintson a **Tovább** gombra.
6. A hello **adattárolókhoz konfigurálása** csoportjában adja meg egy létező **Azure SQL adatbázis** és **Azure storage-fiók** (vagy) adatbázis tárolási létrehozása, és kattintson a Tovább gombra.
7. A hello **számítási konfigurálása** lapon válassza ki az alapértelmezett beállításokat, és kattintson a **következő**.
8. A hello **összegzés** lapon tekintse át az összes beállítást, és kattintson a **következő**.
9. A hello **központi telepítési állapot** lapon Várjon, amíg a hello telepítés befejeződött, és kattintson a **Befejezés**.
10. Kattintson a jobb gombbal a projektre a Solution Explorer hello, és kattintson a **közzététel**.
11. Ha látja **jelentkezzen be Microsoft-fiók tooyour** párbeszédpanelen adja meg Azure-előfizetéssel rendelkező hello fiók hitelesítő adatait, és kattintson a **bejelentkezés**.
12. A következő párbeszédpanel hello kell megjelennie:

    ![Publish (Közzététel) párbeszédpanel](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. A hello **konfigurálása adat-előállító** lapján hello a következő lépéseket:

    1. Ellenőrizze, hogy **használja a meglévő adat-előállító** lehetőséget.
    2. Jelölje be hello **adat-előállító** volt válassza ki a hello sablon használatakor.
    3. Kattintson a **következő** tooswitch toohello **elemek közzététele** lap. (Nyomja meg az **lapon** kívül hello neve mező tooif hello toomove **következő** gomb le van tiltva.)
14. A hello **elemek közzététele** lapon, győződjön meg arról, hogy az összes hello adat-előállítók entitások van kiválasztva, és kattintson a **következő** tooswitch toohello **összegzés** lap.     
15. Olvassa el az összesítő hello és a **következő** toostart hello központi telepítési folyamat és a nézet hello **központi telepítési állapot**.
16. A hello **központi telepítési állapot** lapon hello telepítési folyamatának állapotát hello kell megjelennie. Hello a telepítés befejezése után kattintson a Befejezés gombra.

Lásd: [az első adat-előállítóban (Visual Studio) Build](data-factory-build-your-first-pipeline-using-vs.md) Visual Studio tooauthor adat-előállító entitások használata és a közzététel tooAzure vonatkozó további információért.          
