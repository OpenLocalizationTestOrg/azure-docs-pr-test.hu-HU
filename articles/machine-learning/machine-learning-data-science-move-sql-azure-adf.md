---
title: "egy helyi SQL Server tooSQL Azure szolgáltatásban az Azure Data Factory aaaMove adatait |} Microsoft Docs"
description: "Állítsa be az ADF-feldolgozási folyamat composes két együtt adatok áthelyezése a naponta helyszíni adatbázisok között, és hello felhőben található adatok áttelepítési tevékenységeket."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 7f7e78c7a84a259539221d3235b76bb5a3cf9866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-sql-server-toosql-azure-with-azure-data-factory"></a>Adatok áthelyezése egy helyi SQL server tooSQL Azure az Azure Data Factoryvel
Ez a témakör bemutatja, hogyan toomove adatait egy helyi SQL Server-adatbázis tooa SQL Azure Database keresztül Azure Blob Storage használatával hello Azure Data Factory (ADF).

Az egy táblázatot, amely összefoglalja az áthelyezett adatok tooan Azure SQL Database különböző beállítások [adatok tooan Azure SQL-adatbázis áthelyezése az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).

## <a name="intro"></a>Bevezetés: ADF és mikor lehet használt toomigrate adatokat?
Az Azure Data Factory koordinálja, és automatikusan hello adatátviteli, valamint az adatok átalakítása felhőalapú teljes körűen felügyelt adatok integrációs szolgáltatás. hello kulcs hello ADF modellben fogalma folyamat. Egy folyamat tevékenységek, logikai csoportját, amelyek mindegyike meghatározza az hello műveletek tooperform adatkészletek szereplő hello adatok. Társított szolgáltatások olyan használt toodefine hello információk a Data Factory tooconnect toohello adatforrásaihoz szükséges.

Az ADF meglévő adatfeldolgozási szolgáltatások az adatok folyamatok, amelyek magas rendelkezésre állású és felügyelt hello felhőben állítható össze. Ezen adatok folyamatok is lehet ütemezett tooingest, előkészítése, átalakítására, elemzése és adatok közzététele, és ADF kezeli, és koordinálja a hello összetett adatokat és feldolgozási függőségek. Megoldások is gyorsan beépített és a telepített hello felhő csatlakozás helyszíni egyre több és a felhő tárolt adatforrások.

Érdemes lehet ADF:

* Ha adatok igényeinek toobe folyamatosan át egy hibrid forgatókönyvekben, amelyek mindkettőt fér hozzá a helyszíni és felhőalapú erőforrások
* Ha hello adatok tranzakcióalapú van, vagy igények toobe módosítható vagy nem rendelkezik az üzleti logika hozzá tooit, amikor az áttelepíteni.

ADF hello ütemezés és a feladatok által kezelt adatok rendszeres időközönként hello mozgása egyszerű JSON-parancsfájlok használata a figyelését teszi lehetővé. ADF is rendelkeznek egyéb képességeit, például a összetett műveletek támogatása. Az ADF további információkért lásd: hello dokumentációját a [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>hello forgatókönyv
Az ADF-feldolgozási folyamat két adatok áttelepítési tevékenységek composes beállítjuk. Együtt mozognak adatok naponta egy helyi SQL-adatbázis és az Azure SQL Database hello felhő között. hello két tevékenységek a következők:

* adatok másolása egy helyi SQL Server adatbázis tooan Azure Blob Storage-fiók
* adatok másolása az hello Azure Blob Storage-fiók tooan Azure SQL Database.

> [!NOTE]
> hello látható itt volt módosítani a hello hello ADF csapat által biztosított oktatóanyag részletes lépései: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) toohello a témakör vonatkozó szakaszaihoz hivatkozik Ha a megfelelő biztosított.
>
>

## <a name="prereqs"></a>Előfeltételek
Ez az oktatóanyag feltételezi, hogy rendelkezik:

* Egy **Azure-előfizetés**. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Egy **Azure storage-fiók**. Egy Azure storage-fiók ebben az oktatóanyagban hello adatok tárolásához használja. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk. Hello storage-fiók létrehozását követően használja a tooaccess hello tárolási tooobtain hello fiók szükséges. Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Hozzáférés tooan **Azure SQL Database**. Ha be kell állítania egy Azure SQL Database, hello tpoic [Ismerkedés a Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) bemutatja, hogy miként tooprovision Azure SQL adatbázis új példányát.
* Telepített és konfigurált **Azure PowerShell** helyileg. Útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

> [!NOTE]
> Ez az eljárás használja hello [Azure-portálon](https://portal.azure.com/).
>
>

## <a name="upload-data"></a>Feltöltés hello adatok tooyour a helyszíni SQL Server
Hello használjuk [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello áttelepítési folyamat. hello NYC Taxi adatkészlet érhető el, a feladás egy vagy több, az Azure blob storage leírtaknak megfelelően [NYC Taxi adatok](http://www.andresmh.com/nyctaxitrips/). hello adatok van két fájlt, hello trip_data.csv fájl, amely út adatokat tartalmaz, és hello trip_far.csv fájlt, amely minden út kifizette hello jegy ára részleteit tartalmazza. A minta és az ezek a fájlok leírása szerepelnek [NYC Taxi Utazgatással adatkészlet leírása](machine-learning-data-science-process-sql-walkthrough.md#dataset).

A saját adatok tooa készletét itt megadott hello eljárás igazítja, vagy hello lépésekkel hello NYC Taxi dataset használatával leírtak szerint. a helyszíni SQL Server adatbázisba NYC Taxi dataset tooupload hello eljárással hello leírt [tömeges adatok importálása az SQL Server-adatbázisba](machine-learning-data-science-process-sql-walkthrough.md#dbload). Ezeket az utasításokat egy SQL Server egy Azure virtuális gépen, de a helyszíni SQL Server toohello feltöltése hello eljárását hello azonos.

## <a name="create-adf"></a>Egy Azure Data Factory létrehozása
egy új Azure Data Factory és az erőforráscsoport létrehozása a hello utasításokat hello [Azure-portálon](https://portal.azure.com/) biztosított [hozzon létre egy Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory). Név hello új ADF példány *adfdsp* és name hello erőforráscsoport létrehozásánál *adfdsprg*.

## <a name="install-and-configure-up-hello-data-management-gateway"></a>Telepítse és konfigurálja az adatkezelési átjáró hello mentése
tooenable a folyamatok egy az Azure data factory toowork egy helyi SQL Server, a tooadd szükség van rá a társított szolgáltatás toohello adat-előállítóban. a csatolt szolgáltatása a helyszíni SQL Server kiszolgáló toocreate, kell:

* Töltse le és telepítse a Microsoft adatkezelési átjáró hello a helyi számítógépre.
* Konfigurálja a kapcsolódó hello szolgáltatást hello a helyszíni adatok forrás toouse hello átjáró.

Az adatkezelési átjáró hello rendezi sorba, és deserializes hello forrás és a fogadó adatokat hello számítógépen hol tárolja.

Telepítési utasításokat és az adatkezelési átjáró részleteinek: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)

## <a name="adflinkedservices"></a>Hozzon létre csatolt szolgáltatások tooconnect toohello erőforrásokat
A társított szolgáltatás az Azure Data Factory tooconnect tooa adatforrás, melyhez a szükséges hello információkat határozza meg. hello lépésről lépésre társított szolgáltatások létrehozásához megadott [társított szolgáltatások létrehozásához](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).

Három erőforrások van ebben a forgatókönyvben összekapcsolt szolgáltatások van szükség.

1. [A helyszíni SQL Server társított szolgáltatás](#adf-linked-service-onprem-sql)
2. [Az Azure Blob Storage társított szolgáltatás](#adf-linked-service-blob-store)
3. [Az Azure SQL database társított szolgáltatás](#adf-linked-service-azure-sql)

### <a name="adf-linked-service-onprem-sql"></a>A helyszíni SQL Server-adatbázis társított szolgáltatás
hello toocreate kapcsolódó hello szolgáltatást a helyi SQL Server:

* Kattintson a hello **adattár** a klasszikus Azure portál hello ADF kezdőlapja
* Válassza ki **SQL** , és írja be a hello *felhasználónév* és *jelszó* a hello a helyszíni SQL Server-felhasználó hitelesítő adatait. Tooenter hello kiszolgálónév másként van szüksége egy **teljesen minősített kiszolgálónév fordított perjel példány neve (kiszolgáló_neve\példány_neve)**. A társított szolgáltatás neve hello *adfonpremsql*.

### <a name="adf-linked-service-blob-store"></a>A Blob társított szolgáltatás
toocreate hello hello Azure Blob Storage-fiókhoz társított szolgáltatás:

* Kattintson a hello **adattár** a klasszikus Azure portál hello ADF kezdőlapja
* Válassza ki **Azure Storage-fiók**
* Adja meg a hello Azure Blob Storage fiók kulcs és a tároló nevét. Társított szolgáltatás neve hello *adfds*.

### <a name="adf-linked-service-azure-sql"></a>Az Azure SQL database társított szolgáltatás
hello Azure SQL Database toocreate kapcsolódó hello szolgáltatást:

* Kattintson a hello **adattár** a klasszikus Azure portál hello ADF kezdőlapja
* Válassza ki **Azure SQL** , és írja be a hello *felhasználónév* és *jelszó* hello Azure SQL adatbázis hitelesítő adatait. Hello *felhasználónév* kell megadni,  *user@servername* .   

## <a name="adf-tables"></a>Adja meg, és hozzon létre táblák toospecify hogyan tooaccess hello adatkészletek
Hozzon létre táblákat, adja meg a következő eljárások parancsfájlalapú hello hello struktúra, helyét és hello adatkészletek rendelkezésre állását. JSON-fájlok használt toodefine hello táblákat. Ezek a fájlok hello szerkezete további információkért lásd: [adatkészletek](../data-factory/data-factory-create-datasets.md).

> [!NOTE]
> Végre kell hajtani hello `Add-AzureAccount` parancsmag hello végrehajtása előtt [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) , amely közvetlenül az Azure-előfizetés hello parancsmag tooconfirm van kiválasztva a hello parancs végrehajtása. Ez a parancsmag dokumentációjáért lásd: [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

hello JSON-alapú definíciók hello táblák neve a következő hello használata:

* Hello **táblanév** hello a helyszíni SQL server esetében *nyctaxi_data*
* Hello **Tárolónév** hello Azure Blob Storage a fiók esetében *containername*  

Három definíciói az ADF adatcsatorna van szükség:

1. [A helyszíni SQL-tábla](#adf-table-onprem-sql)
2. [A BLOB tábla](#adf-table-blob-store)
3. [Az SQL Azure-tábla](#adf-table-azure-sql)

> [!NOTE]
> Ezek az eljárások Azure PowerShell toodefine használja, és hello ADF tevékenységek létrehozása. De ezek a feladatok hello Azure-portál használatával is elvégezhető. További információkért lásd: [adatkészletek létrehozása](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).
>
>

### <a name="adf-table-onprem-sql"></a>A helyszíni SQL-tábla
hello hello-definíció a helyszíni SQL Server megadott JSON-fájl a következő hello:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

hello oszlopnevek nem szerepeltek itt. Részterv választhat a hello oszlopnevek itt többek között (részletekért ellenőrizze a hello [ADF dokumentáció](../data-factory/data-factory-data-movement-activities.md) témakör.

Hello JSON-definícióból hello tábla másolja egy nevű fájlba *onpremtabledef.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\onpremtabledef.json*). Hello tábla létrehozása az ADF hello Azure PowerShell-parancsmag a következő:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>A BLOB tábla
Hello hello tábla definícióját kimeneti blob helyére hello alábbi (Ez leképezi a helyszíni tooAzure blob okozhatnak hello adatait):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Hello JSON-definícióból hello tábla másolja egy nevű fájlba *bloboutputtabledef.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\bloboutputtabledef.json*). Hello tábla létrehozása az ADF hello Azure PowerShell-parancsmag a következő:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <a name="adf-table-azure-sq"></a>Az SQL Azure-tábla
Az SQL Azure kimeneti hello hello tábla definícióját hello következő (ebben a sémában leképezhető hello blob érkező hello adatok) kell megadni:

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Hello JSON-definícióból hello tábla másolja egy nevű fájlba *AzureSqlTable.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\AzureSqlTable.json*). Hello tábla létrehozása az ADF hello Azure PowerShell-parancsmag a következő:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <a name="adf-pipeline"></a>Adja meg, és hello folyamat létrehozása
Adja meg a toohello tartozó hello tevékenység a következő feldolgozási sorban, és hozzon létre hello folyamat a következő eljárások parancsfájlalapú hello. A JSON-fájl használt toodefine hello csővezeték tulajdonságok.

* hello parancsfájl feltételezi, hogy hello **adatcsatorna neve** van *AMLDSProcessPipeline*.
* Ne feledje, hogy hivatott hello periodikusságát hello adatcsatorna toobe végrehajtva a következő napi szinten és -felhasználási hello alapértelmezett végrehajtási idő hello feladat (12 óra UTC).

> [!NOTE]
> hello alábbi eljárások használata az Azure PowerShell toodefine és hello ADF folyamatot létrehozni. Azonban ez a feladat az Azure portál használatával is elvégezhető. További információkért lásd: [létrehozási folyamat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).
>
>

Hello definíciói használatával megadott korábban hello csővezeték definíciója hello ADF van megadva az alábbiak szerint:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL tooAzure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server tooblob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data tooSql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Másolja a JSON-definícióból hello folyamatának nevű fájlba *pipelinedef.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\pipelinedef.json*). Hozzon létre hello folyamat az ADF hello Azure PowerShell-parancsmag a következő:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Ellenőrizze, hogy is látható hello csővezeték hello a klasszikus Azure portál hello ADF jelenik meg a következőképpen (kattintást hello diagram)

![Az ADF-feldolgozási folyamat](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <a name="adf-pipeline-start"></a>Hello folyamat elindítása
hello csővezeték futtatható a következő parancs hello használata:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Hello *startdate* és *enddate* paraméterértékeket kell írni a hello tényleges dátumok, amelyek között hello adatcsatorna toorun kívánt toobe.

Hello folyamat végrehajtása során, ha meg tudja toosee hello adatok megjelennek hello tárolóban hello BLOB, naponta egy fájl kiválasztott kell lennie.

Vegye figyelembe, hogy a jelenleg nem alkalmazhatók az hello funkcióit ADF toopipe adatok Növekményesen rendelkezik. További információ a hogyan toodo ezzel és más ADF, által biztosított képességek: hello [ADF dokumentáció](https://azure.microsoft.com/services/data-factory/).
