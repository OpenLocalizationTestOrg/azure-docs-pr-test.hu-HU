---
title: "aaaAzure Data Factory - gyakran ismételt kérdések"
description: "Azure Data Factory vonatkozó gyakran ismételt kérdések."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 78289fb4b6e15d74772af6c71ec25c7d2ca1a0bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Az Azure Data Factory - gyakran ismételt kérdések
## <a name="general-questions"></a>Általános kérdések
### <a name="what-is-azure-data-factory"></a>Mi az az Azure Data Factory?
Adat-előállító szolgáltatás egy felhőalapú Adatintegráció szolgáltatás, amely **automatizálja hello mozgás és adatok átalakítása**. Olyan adat-előállítóval, amely berendezések tootake raw anyagok alakíthatják termékek, mint például a Data Factory koordinálja a meglévő szolgáltatások, amely a nyers adatokat gyűjthet, és irányítópulttá, használatra kész adatokat.

Adat-előállító lehetővé teszi toocreate adatvezérelt munkafolyamatok toomove adatok mind a helyszíni és felhőalapú adattároló, valamint számítási szolgáltatásokkal, például az Azure HDInsight és az Azure Data Lake Analytics folyamat vagy átalakítási adatok között. Miután létrehozott egy folyamatot, amely szükséges hello műveletet hajt végre, képes az ütemezés toorun időnként (óránként, naponta, hetente stb.).   

További információkért lásd: [– áttekintés és alapfogalmak](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Hol található az Azure Data Factory díjszabása?
Lásd: [Data Factory díjszabás lap] [ adf-pricing-details] a díjszabás részletei az Azure Data Factory hello hello.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Hogyan kezdjem el az Azure Data Factoryvel?
* Azure Data Factory áttekintését lásd: [Data Factory bemutatása tooAzure](data-factory-introduction.md).
* Hogyan oktatóanyagért túl**másolására/áthelyezésére adatok** használata a másolási tevékenység, lásd: [adatok másolása az Azure Blob Storage tooAzure SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Hogyan oktatóanyagért túl**adatok** HDInsight Hive tevékenységgel. Lásd: [feldolgozni az adatokat a Hadoop-fürt Hive parancsfájl futtatásával](data-factory-build-your-first-pipeline.md)

### <a name="what-is-hello-data-factorys-region-availability"></a>Mi az a hello Data Factory régiónkénti elérhetőség?
Adat-előállító érhető el **Velünk nyugati** és **Észak-Európa**. hello számítási és tárolási szolgáltatások adat-előállítók által használt más régiókban. Lásd: [támogató régiók](data-factory-introduction.md#supported-regions).

### <a name="what-are-hello-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Hello korlátai előállítók/folyamatok/tevékenységek, adatkészletek adatok számára
Lásd: **Azure Data Factory korlátok** hello szakasza [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../azure-subscription-service-limits.md#data-factory-limits) cikk.

### <a name="what-is-hello-authoringdeveloper-experience-with-azure-data-factory-service"></a>Mi az Azure Data Factory szolgáltatással hello szerzői/fejlesztők számára?
Szerző/létrehozhat adat-előállítók a következő eszközök/SDK-k hello egyikének használatával:

* **Azure-portálon** hello adat-előállító paneleken hello Azure-portálon a gazdag felhasználói felületet biztosít az Ön toocreate adatszolgáltatások előállítók kapcsolódó ad. Hello **Data Factory Editor**, amely része is hello portál lehetővé teszi a tooeasily létrehozása társított szolgáltatások, táblák, adathalmazok és adatcsatornák ezen összetevők JSON definícióit megadásával. Lásd: [felépítheti első adatok folyamatát az Azure portál használatával](data-factory-build-your-first-pipeline-using-editor.md) használatának példája portal/szerkesztő toocreate hello és központi telepítése egy adat-előállítóban.
* **A Visual Studio** használhatja a Visual Studio toocreate egy az Azure data factory. Lásd: [felépítheti első adatok folyamatát Visual Studio használatával](data-factory-build-your-first-pipeline-using-vs.md) részleteiről.
* **Az Azure PowerShell** lásd [létrehozása és a figyelő az Azure PowerShell Azure Data Factory](data-factory-build-your-first-pipeline-using-powershell.md) oktatóanyag/forgatókönyv egy PowerShell-lel adat-előállító létrehozásához. Lásd: [Data Factory parancsmag-referencia] [ adf-powershell-reference] tartalom az MSDN Library a Data Factory-parancsmagok átfogó dokumentációját.
* **.NET class Library** programozott módon létrehozhat az adat-előállítók Data Factory .NET SDK használatával. Lásd: [létrehozása, figyelheti és kezelheti a .NET SDK használatával adat-előállítók](data-factory-create-data-factories-programmatically.md) részletes útmutatás a .NET SDK használatával adat-előállító létrehozása. Lásd: [Data Factory osztály kódtár – dokumentáció] [ msdn-class-library-reference] egy átfogó Data Factory .NET SDK-dokumentáció.
* **REST API** is hello hello Azure Data Factory szolgáltatás toocreate által elérhetővé tett REST API-t használja, és telepítheti az adat-előállítók. Lásd: [Data Factory REST API-referencia] [ msdn-rest-api-reference] egy átfogó Data Factory REST API-dokumentáció.
* **Az Azure Resource Manager-sablon** lásd: [oktatóanyag: az első az Azure data factory Azure Resource Manager-sablonnal Build](data-factory-build-your-first-pipeline-using-arm.md) fő részleteit.

### <a name="can-i-rename-a-data-factory"></a>Átnevezheti egy adat-előállító?
Nem. Más Azure-erőforrások, például egy Azure data factory hello neve nem módosítható.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-tooanother"></a>Egy adat-előállító áthelyezhető egy Azure-előfizetés tooanother származó?
Igen. Használjon hello **áthelyezése** gombra a data factory panelen, ahogy az ábra a következő hello:

![Adat-előállító áthelyezése](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-hello-compute-environments-supported-by-data-factory"></a>Mik azok a Data Factory által támogatott hello számítási környezetek?
hello következő táblázat felsorolja a Data Factory és hello tevékenységek ezeken futó által támogatott számítási környezetek.

| Számítási környezet | tevékenységek |
| --- | --- |
| [Igény szerinti HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) vagy [saját HDInsight-fürt](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streamelési](data-factory-hadoop-streaming-activity.md) |
| [Az Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning-tevékenységek: kötegelt végrehajtás és erőforrás frissítése](data-factory-azure-ml-batch-execution-activity.md) |
| [Az Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Az Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL Data Warehouse](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Tárolt eljárás](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Hogyan hasonlítsa össze a Azure Data Factory az SQL Server Integration Services (SSIS)? 
Lásd: hello [Azure Data Factory vs. SSIS](http://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) bemutató az MVP (legtöbb értékelni szakemberek) egyikéből: Reza Rad. Néhány hello legutóbbi módosításainak adat-előállítóban nem jelennek meg a hello programbeli diavetítések. További képességeket tooAzure adat-előállító folyamatosan hozzáadni azt. További képességeket tooAzure adat-előállító folyamatosan hozzáadni azt. Azt fogja beépítse ezeket a frissítéseket a Microsoft integrációs technológiái hello összehasonlítása valamikor az év.   

## <a name="activities---faq"></a>Tevékenységek – gyakori kérdések
### <a name="what-are-hello-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Mik azok a tevékenységek a Data Factory-folyamathoz használható különböző típusú hello?
* [Adatok mozgása tevékenységek](data-factory-data-movement-activities.md) toomove adatokat.
* [Adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) tooprocess/átalakítás adatokat.

### <a name="when-does-an-activity-run"></a>Amikor fut egy tevékenység?
Hello **rendelkezésre állási** konfigurációs beállítás hello a kimeneti adatok tábla határozza meg, hogy hello tevékenység futása közben. Ha nincs megadva bemeneti adatkészletek, hello tevékenység ellenőrzi, hogy teljesülnek-e az összes hello bemeneti adatok függőségek (Ez azt jelenti, hogy **készen** állapot) futtató megkezdése előtt.

## <a name="copy-activity---faq"></a>A másolási tevékenység – gyakori kérdések
### <a name="is-it-better-toohave-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Ennyi az egész jobb toohave több tevékenységek folyamat vagy egy különálló folyamat minden egyes tevékenységhez?
Folyamatok elvileg toobundle kapcsolatos tevékenységeket. Csatlakoztassa őket hello adatkészletek nem hello csővezeték kívül bármely más tevékenység használnak fel, ha egy folyamat megtarthatja hello tevékenységeket. Ezzel a módszerrel nem kell toochain adatcsatorna aktív időszakokat, hogy azok egymással. Emellett hello adatok hello táblák belső toohello folyamat jobban integritása hello adatcsatorna frissítésekor. Összefüggő frissítési lényegében hello összes tevékenységét hello folyamat leáll, eltávolítja azokat, és újra létrehozza azokat. A terv készítése, is könnyebben toosee hello folyamata kapcsolódó hello adatainak hello adatcsatorna fájl tevékenységek egy JSON-ban.

### <a name="what-are-hello-supported-data-stores"></a>Mik azok a hello támogatott adattárolókhoz?
A Data Factory másolási tevékenység egy forrás adatokat tároló tooa fogadó adatokat tároló másol adatokat. Adat-előállítót a következő adatokat tárolja hello támogatja. Bármely forrásból származó adatok csak írható tooany fogadó. Hogyan kattintson egy adatokat tároló toolearn toocopy adatok tooand adott áruházból.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Adatokat tárolja, a * lehet helyszíni vagy Azure IaaS és használatba tooinstall [az adatkezelési átjáró](data-factory-data-management-gateway.md) a-hez vagy az Azure infrastruktúra-szolgáltatási gépen.

### <a name="what-are-hello-supported-file-formats"></a>Mik azok a hello támogatott fájlformátumok?
[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

### <a name="where-is-hello-copy-operation-performed"></a>Hol történik a hello másolási művelet?
Lásd: [világszerte elérhető adatmozgás](data-factory-data-movement-activities.md#global) című szakaszban talál információt. Rövid Ha egy helyszíni adattároló van szó, hello másolási műveletet hajt végre hello adatkezelési átjárót a helyszíni környezetben. És a két felhőalapú tárolók közötti hello adatátvitel esetén hello másolási művelet történik-e az hello régió legközelebbi toohello fogadó helyre a hello ugyanazon a földrajzi.

## <a name="hdinsight-activity---faq"></a>HDInsight tevékenység – gyakori kérdések
### <a name="what-regions-are-supported-by-hdinsight"></a>Milyen régiók HDInsight által támogatott?
Lásd a következő cikket hello című cikknek a földrajzi elérhetőséggel hello: vagy [HDInsight Díjszabásának részleteit][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Milyen régió igény szerinti HDInsight-fürtök által használt?
hello igény szerinti HDInsight-fürt létrehozása a hello azonos régióban, ahol hello fürthöz használt toobe megadott hello tároló létezik.    

### <a name="how-tooassociate-additional-storage-accounts-tooyour-hdinsight-cluster"></a>Hogyan tooassociate további tárfiókok tooyour HDInsight-fürtöt?
Ha saját HDInsight-fürt (BYOC - Bring Your saját fürt) használ, tekintse meg a következő témakörök hello:

* [HDInsight-fürtök használata alternatív Storage-fiókok és a Metaadattárakat][hdinsight-alternate-storage]
* [További Tárfiókok használata a HDInsight Hive][hdinsight-alternate-storage-2]

Ha az igény szerinti fürt hello Data Factory szolgáltatásnak által létrehozott használ, adja meg, további tárfiókok hello HDInsight a társított szolgáltatás, így hello Data Factory szolgáltatásnak is regisztrálja őket az Ön nevében. Hello hello igény társított szolgáltatás JSON-definícióból, használjon **additionalLinkedServiceNames** tulajdonság toospecify alternatív tárolócsoport fiókok, ahogy az alábbi JSON részlet hello:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
Hello a fenti példában otherLinkedServiceName1 és otherLinkedServiceName2 képviselniük amelynek meghatározásokat tartalmaznak hitelesítő adatokat, amelyek a HDInsight fürt igények tooaccess alternatív storage-fiókok hello társított szolgáltatások.

## <a name="slices---faq"></a>A szeletek – gyakori kérdések
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Miért van a bemeneti szeletek nem üzemkész állapotban?
Gyakori hiba nem állít **külső** tulajdonság túl**igaz** hello bemeneti adatkészletet, amikor hello bemeneti adatai külső toohello adat-előállítóban (hello adat-előállító nem elő).

A következő példa hello, csak szükség tooset **külső** a tootrue **dataset1**.  

**DataFactory1** csővezeték-1: dataset1 -> activity1 -> dataset2 activity2 -> -> dataset3 csővezeték 2: dataset3 -> activity3 dataset4 ->

Ha egy másik, egy folyamatot, amely dataset4 (hozott létre adatcsatorna adat-előállítóban 1 2) az adat-előállító, megjelölése dataset4 egy külső adathalmaz mert hello dataset elő egy másik adat-előállítóban (DataFactory1, nem DataFactory2).  

**DataFactory2**    
Feldolgozási sor 1: dataset4 -> activity4 dataset5 ->

Hello külső tulajdonság nincs megfelelően beállítva, ha győződjön meg arról, hogy létezik-e hello bemeneti adatok hello bemeneti adatkészlet-definícióban meghatározott hello helyen.

### <a name="how-toorun-a-slice-at-another-time-than-midnight-when-hello-slice-is-being-produced-daily"></a>Hogyan toorun egy másik időpontban, mint amikor hello szelet alatt hozzák naponta éjfél szelet?
Használjon hello **eltolás** tulajdonság toospecify hello időtartamát, amellyel hello szelet toobe hozott létre. Lásd: [adatkészlet rendelkezésre állási](data-factory-create-datasets.md#dataset-availability) szakasz ezt a tulajdonságot vonatkozó további információért. Íme egy gyors példát:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Napi szeletek kezdjék **reggel 6 óra** hello alapértelmezett éjfél helyett.     

### <a name="how-can-i-rerun-a-slice"></a>Hogyan lehet futtassa újból a szeletet?
A szelet valamelyik a következő módokon hello futtathatja:

* A figyelő és App kezelése toorerun egy tevékenységek ablakát vagy szelet. Lásd: [kijelölt tevékenység windows újrafuttatása](data-factory-monitor-manage-app.md#perform-batch-actions) utasításokat.   
* Kattintson a **futtatása** hello parancs sávján hello **ADATSZELET** hello szelet hello Azure-portálon a panelen.
* Futtatás **Set-AzureRmDataFactorySliceStatus** állapotú parancsmag beállítása túl**Várakozás** hello adatszelethez.   

    ```PowerShell
    Set-AzureRmDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
Lásd: [Set-AzureRmDataFactorySliceStatus] [ set-azure-datafactory-slice-status] hello parancsmag vonatkozó további információért.

### <a name="how-long-did-it-take-tooprocess-a-slice"></a>Mennyi ideig azt vette tooprocess szelet?
Mennyi ideig tartott tooprocess egy adatszelet figyelő & App kezelése tooknow tevékenység ablak Explorer használja. Lásd: [tevékenység ablak Explorer](data-factory-monitor-manage-app.md#activity-window-explorer) részleteiről.

Azt is megteheti hello hello Azure-portál a következő:  

1. Kattintson a **adatkészletek** hello csempét **adat-előállító** a data factory paneljét.
2. Kattintson az adott adatforrásra hello hello **adatkészletek** panelen.
3. Jelölje be hello szelet, amely a hello megváltozása **legutóbbi szeletek** hello listájában **tábla** panelen.
4. Kattintson a hello hello tevékenységfuttatási **tevékenységek** hello listájában **ADATSZELET** panelen.
5. Kattintson a **tulajdonságok** hello csempét **tevékenység futtatása részletei** panelen.
6. Megtekintheti az hello **időtartam** mezőt a értéket. Ez az érték tooprocess hello szelet hello idő.   

### <a name="how-toostop-a-running-slice"></a>Hogyan toostop futó szelet?
Ha toostop hello csővezetéket végrehajtása van szüksége, használhatja [Suspend-AzureRmDataFactoryPipeline](/powershell/module/azurerm.datafactories/suspend-azurermdatafactorypipeline) parancsmag. Jelenleg az hello folyamat felfüggesztése nem állítja le a folyamatban lévő hello szelet végrehajtások. Hello folyamatban végrehajtások befejezése után nem extra szelet van felvételre.

Ha valóban toostop összes hello végrehajtások azonnal, hello módon csak volna toodelete hello csővezeték kell, és hozza létre újra. Ha úgy dönt, hogy toodelete hello csővezeték, nem kell toodelete táblák és összekapcsolt szolgáltatások hello folyamat használja.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/azurerm.datafactories
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/set-azurermdatafactoryslicestatus

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
