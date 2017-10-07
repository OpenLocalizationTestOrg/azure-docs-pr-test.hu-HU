---
title: "aaaIntroduction tooData gyári, egy integrációs szolgáltatás |} Microsoft Docs"
description: "A témakör ismerteti, hogy mi is az Azure Data Factory: egy felhőalapú adatintegrációs szolgáltatás, amellyel előkészíthető és automatizálható az adatok továbbítása és átalakítása."
keywords: "adatintegrálás, felhőalapú adatintegráció, mi az az azure data factory"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 4cc30515315efc938951057743ff8eb3701214ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-data-factory"></a>Data Factory bemutatása tooAzure 
## <a name="what-is-azure-data-factory"></a>Mi az az Azure Data Factory?
A hello world a big data hogyan van meglévő adatok kihasználhatók a vállalati? Ennyi az egész hivatkozási adatokat a helyszíni adatforrások vagy más különböző adatforrások használatával hello felhőben létrehozott lehetséges tooenrich adatokat? Például egy játék vállalati gyűjt hello felhőben játékok által előállított sok naplókat. A naplók toogain elemzések toocustomer beállítások, demográfiai, használati viselkedés szeretne tooanalyze stb tooidentify felfelé-értékesít és kereszt-értékesítési lehetőségek, új vonatkozóan szolgáltatások toodrive üzleti növekedési fejlesztése és a jobb felhasználói élményt nyújtja toocustomers. 

tooanalyze ezek a naplók hello vállalati toouse hello referenciaadatok például ügyféladatok, játék információkért marketing kampány adatait, amely egy helyi tárolóban kell. Ezért hello vállalat szeretne tooingest naplóadatokat felhő adattárból hello és a helyszíni adattárból hello referenciaadatok. Ezt követően hello adatfeldolgozásra hello a Hadoop használatával a felhő (Azure HDInsight), és tegye közzé a hello eredménye egy felhőalapú adatraktára, például az Azure SQL Data warehouse-bA vagy egy helyszíni adatok adatok tárolására, például az SQL Server. Kíván a munkafolyamat toorun hetente egyszer. 

Mire van szükség egy platform, amely lehetővé teszi, hogy a vállalat toocreate hello egy munkafolyamatot, amely a helyszíni és felhőalapú adattároló, és -átalakítási és -folyamat adatok betöltési meglévő számítási szolgáltatások, például a Hadoop használatával, és hello eredmények tooan helyszíni közzététele vagy adattárolójának felhő BI alkalmazások tooconsume. 

![Data Factory – áttekintés](media/data-factory-introduction/what-is-azure-data-factory.png) 

Az Azure Data Factory hello platform az ilyen típusú forgatókönyvek. Ez egy **felhőalapú integrációs szolgáltatás, amely lehetővé teszi az adatvezérelt munkafolyamatok toocreate hello felhő levezényli és automatizálja a adatmozgatást és a data transformation**. Azure Data Factory használatával hozhat létre és ütemezése az adatvezérelt munkafolyamatok (más néven folyamatok), amely képes különálló adattároló származó adatok, folyamat/átalakítás hello adatok például az Azure HDInsight Hadoop, Spark, Azure Data Lake számítási szolgáltatások használatával Elemzés és az Azure Machine Learning, és a kimeneti adatok toodata tárolja, például az Azure SQL Data Warehouse az üzleti intelligenciával alkalmazások tooconsume közzététele.  

Ez inkább egy kinyerési és betöltési (EL), majd egy átalakítási és betöltési (TL) platform, mintsem egy hagyományos kinyerési, átalakítási és betöltési (ETL) platform. hello átalakítások során végrehajtott olyan tootransform/folyamat adatok számítási szolgáltatásainak a használatával, hanem például hozzáadására szolgáló ők hello tooperform átalakítások származtatott oszlopok, számbavételi sorok, a rendezés az adatok, stb. 

Az Azure Data Factoryben, felhasznált és a munkafolyamatok hello adatok jelenleg **idő szeletelhetők adatok** (óránként, naponta, hetente, stb.). Például beállítható, hogy egy folyamat naponta egyszer olvasson bemeneti adatokat, dolgozza fel őket, és hozzon létre kimeneti adatokat. A munkafolyamatok egyetlen alkalommal is futtathatók.  
  

## <a name="how-does-it-work"></a>Hogyan működik? 
Azure Data Factory (adatok által vezérelt munkafolyamatok) hello adatcsatornák végrehajtania a hello a következő három lépésből áll:

![Az Azure Data Factory három szakasza](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Csatlakozás és összegyűjtés
A vállalatok a legkülönfélébb adatokkal rendelkeznek a legkülönfélébb forrásokból. hello információs éles rendszer létrehozásának első lépéseként tooconnect tooall hello szükséges adatforrásokat, és feldolgozás, például a szolgáltatott szoftver szolgáltatásokat, megosztások, FTP, web services fájlt, és helyezze át a hello adatok igény szerint tooa központi helye a következő feldolgozása.

Adat-előállító nélkül vállalatok kell egyéni az adatátviteli összetevők összeállítását, vagy egyéni szolgáltatások toointegrate írni ezeket az adatforrások és a feldolgozási. Költséges és rögzített toointegrate és karbantartása az ilyen rendszerekre, és gyakran nem rendelkezik hello enterprise osztályú figyelés és riasztás, és egy teljes körűen felügyelt szolgáltatás kínáló hello szabályozza.

Data Factory egy a helyszíni adatok adatcsatorna toomove adatait a másolási tevékenység hello használata, és a felhő forrás adatokat tároló tooa központosítás adattár hello felhőben további elemzés céljából. Például is összegyűjtötte az adatokat az Azure Data Lake Store és átalakítás hello adatok később egy Azure Data Lake Analytics számítási szolgáltatás segítségével. Vagy begyűjtheti az adatokat egy Azure Blob Storage tárolóból, és később átalakíthatja azokat egy Azure HDInsight Hadoop-fürt használatával.

### <a name="transform-and-enrich"></a>Átalakítás és bővítés
Ha egy központi adattárból hello felhőben található adatokat, kívánt hello gyűjtött adatok toobe dolgozni, vagy át legyenek-e számítási szolgáltatások, például a HDInsight Hadoop, a Spark, a Data Lake Analytics és a gépi tanulás használatával. Azt szeretné, hogy tooreliably átalakítva által előállított adatokat egy fenntarthatóvá és ellenőrzött ütemezés toofeed termelési környezetben megbízható adatokkal. 

### <a name="publish"></a>Közzététel 
Átalakított adatok letölteni hello felhő tooon helyszíni adatforrások, például az SQL Server, vagy tartsa a felhő a megjeleníthető tárolási adatforrások által az üzleti intelligenciával és elemzőeszközök és más alkalmazások.

## <a name="key-components"></a>A legfontosabb összetevők
Az Azure-előfizetések több Azure Data Factory-példányt (más néven adat-előállítókat) is tartalmazhatnak. Az Azure Data Factory együttműködése tooprovide hello platform, amelyen létrehozhatja azt az adatvezérelt munkafolyamatok lépéseket toomove és átalakítás adatokkal négy fő összetevőből áll. 

### <a name="pipeline"></a>Folyamat
Az adat-előállító egy vagy több folyamattal rendelkezhet. A folyamatok tevékenységek csoportjai. Egy adatcsatorna tevékenységeinek hello együtt, a feladat elvégzésére. Például egy folyamat képes tevékenységek csoportja, amely egy Azure blob adatait ingests tartalmaz, és futtassa a Hive-lekérdezések egy HDInsight fürt toopartition hello adatok. hello ennek előnye, hogy hello folyamat lehetővé teszi a toomanage hello tevékenységek helyett mindegyik készletként külön-külön. Például telepítheti, és egymástól függetlenül ütemezése hello sorban, hello tevékenységek helyett. 

### <a name="activity"></a>Tevékenység
Egy folyamat egy vagy több tevékenységgel rendelkezhet. Tevékenységek hello műveletek tooperform határozza meg az adatokat. A másolási tevékenység toocopy adatok használhat például egy-egy tároló tooanother adattárolóból. Hasonlóképpen előfordulhat, hogy egy Hive tevékenységgel, az Azure HDInsight fürt tootransform Hive-lekérdezések futó, vagy az adatok elemzéséhez. A Data Factory két típusú tevékenységet támogat: az adattovábbítási tevékenységeket és az adatátalakítási tevékenységeket.

### <a name="data-movement-activities"></a>Adattovábbítási tevékenységek
A Data Factory másolási tevékenység egy forrás adatokat tároló tooa fogadó adatokat tároló másol adatokat. Adat-előállítót a következő adatokat tárolja hello támogatja. Bármely forrásból származó adatok csak írható tooany fogadó. Hogyan kattintson egy adatokat tároló toolearn toocopy adatok tooand adott áruházból.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

További információkért tekintse meg az [adattovábbítási tevékenységekről](data-factory-data-movement-activities.md) szóló cikket.

### <a name="data-transformation-activities"></a>Adatátalakítási tevékenységek
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

További információkért tekintse meg az [adatátalakítási tevékenységekről](data-factory-data-transformation-activities.md) szóló cikket.

### <a name="custom-net-activities"></a>Egyéni .NET-tevékenységek
Ha szüksége van a toomove adatok/egy adattárból, hogy a másolási tevékenység nem támogatja, vagy a saját logikát használó adatok átalakítása, hozzon létre egy **egyéni .NET tevékenység**. További információ az egyéni tevékenységek létrehozásával és használatával kapcsolatban: [Egyéni tevékenységek használata Azure Data Factory-folyamatban](data-factory-use-custom-activities.md).

### <a name="datasets"></a>Adathalmazok
Minden tevékenység nulla vagy több adatkészletet fogad bemenetként, és egy vagy több adatkészletet állít elő kimenetként. Adatkészletek hello adattároló, amely egyszerűen pontot, vagy a kívánt toouse a tevékenység bemeneti vagy kimeneti, hello referenciaadatok adatstruktúra jelölik. Például egy Azure Blob-adathalmazra meghatározza hello blob tároló és mappa hello Azure Blob Storage mely hello a feldolgozási sor olvassa el a hello adatokat. Vagy egy Azure SQL-tábla a dataset meghatározza, hogy hello tevékenység írják hello toowhich hello kimeneti adatait. 

### <a name="linked-services"></a>Társított szolgáltatások
Társított szolgáltatások sokkal hasonlóak a kapcsolati karakterláncok, amelyek a Data Factory tooconnect tooexternal erőforrások szükséges hello kapcsolati adatainak megadása. Azt gondolja, hogy így - összekapcsolt szolgáltatás hello kapcsolat toohello adatok forrása határozza meg, és a DataSet adatkészlet hello adatok szerkezete hello jelöli. Például az Azure tárolás társított szolgáltatásának megadja a kapcsolati karakterlánc tooconnect toohello Azure Storage-fiók. És egy Azure Blob-adathalmazra hello blobtárolót és hello adatokat tartalmazó hello mappát adja meg.   

A társított szolgáltatásokat két célból használjuk a Data Factoryban:

* toorepresent egy **adattár** többek között, de nem kizárólag egy helyi SQL Server, Oracle adatbázis, fájlmegosztás vagy egy Azure Blob Storage-fiók. Lásd: hello [adatok mozgása tevékenységek](#data-movement-activities) támogatott adattárolókhoz szakasza listáját.
* toorepresent egy **számítási erőforrás** , üzemeltethet hello végrehajtási tevékenység. Például hello HDInsightHive tevékenység fut egy HDInsight Hadoop-fürt. A támogatott számítási környezetek listája az [Adatátalakítási tevékenységek](#data-transformation-activities) szakaszban található.

### <a name="relationship-between-data-factory-entities"></a>Data Factory-entitások közötti kapcsolatok
![Ábra: A Data Factory áttekintése, felhőalapú adatintegrációs szolgáltatás – főbb fogalmak](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**2. ábra.** Az adatkészlet, a tevékenység, a folyamat és a társított szolgáltatás közötti kapcsolatok

## <a name="supported-regions"></a>Támogatott régiók
Jelenleg, létrehozhat adat-előállítók hello **USA nyugati régiója**, **USA keleti régiója**, és **Észak-Európa** régiók. Azonban egy adat-előállító is hozzáférést, és más Azure-régiók toomove adatok adattárolók között a szolgáltatások számítási vagy folyamat adatok számítási szolgáltatások.

Maga az Azure Data Factory nem tárol adatokat. Lehetővé teszi az adatvezérelt munkafolyamatok közötti tooorchestrate Mozgás létrehozása [adattárolókhoz támogatott](#data-movement-activities) és az adatok feldolgozása [szolgáltatások számítási](#data-transformation-activities) más régiókban vagy egy helyszíni környezet. Azt is lehetővé teszi túl[felügyeletéhez és kezeléséhez munkafolyamatok](data-factory-monitor-manage-pipelines.md) mind programozott és felhasználói felülete mechanizmust.

Annak ellenére, hogy a Data Factory érhető el csak **USA nyugati régiója**, **USA keleti régiója**, és **Észak-Európa** régiókban, hello szolgáltatás működtetéséhez hello adatátvitelt jelölik a Data Factory érhető el [globálisan](data-factory-data-movement-activities.md#global) több régióban. Ha egy adattár egy tűzfal mögött található, majd egy [az adatkezelési átjáró](data-factory-move-data-between-onprem-and-cloud.md) inkább a helyszíni környezetben a kurzor hello adatokat telepítve.

Tegyük fel például, hogy számítási környezetei, mint például az Azure HDInsight-fürt és az Azure Machine Learning a nyugat-európai régión kívül futnak. Hozzon létre és Észak-Európában Azure Data Factory-példány használata, és a számítási környezetek Nyugat-Európában olyan tooschedule feladatok alkalmazza. Adat-előállító tootrigger hello feladat néhány ezredmásodperc a számítási környezet vesz igénybe, de hello idő a számítógépes környezet hello feladat futtatása nem változik.

## <a name="get-started-with-creating-a-pipeline"></a>Bevezetés a folyamatok létrehozásába
Azure Data Factory egyik ezen eszközök vagy API-k toocreate adatok folyamatok használhatja: 

- Azure Portal
- Visual Studio
- PowerShell
- .NET API
- REST API
- Azure Resource Manager-sablon 

Hogyan toobuild adat-előállítók adatokkal folyamatok, toolearn hello oktatóanyagok a következő témakör utasításait kövesse:

| Oktatóanyag | Leírás |
| --- | --- |
| [Két felhőalapú adattár közötti adatáthelyezés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |Ebben az oktatóanyagban hoz létre egy adat-előállító egy folyamatot, amely **helyezi át az adatokat** Blob tároló tooSQL adatbázisából. |
| [Adatok átalakítása Hadoop-fürttel](data-factory-build-your-first-pipeline.md) |Az oktatóanyag során kiépíti az első Azure data factoryját egy olyan adatfolyamattal, amely egy Azure HDInsight (Hadoop) fürtön futtatott Hive-parancsprogrammal **dolgozza fel az adatokat**. |
| [Egy helyszíni és egy felhőalapú adattár közötti adatáthelyezés adatkezelési átjáró segítségével](data-factory-move-data-between-onprem-and-cloud.md) |Ebben az oktatóanyagban hoz létre egy adat-előállítót a folyamat, amely **helyezi át az adatokat** a egy **helyszíni** SQL Server adatbázis tooan Azure-blobot. Hello forgatókönyv részeként telepítse, és konfigurálja a hello az adatkezelési átjáró a számítógépre. |
