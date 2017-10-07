---
title: "aaaMove adatok tooor SSIS-összekötők használata Azure Blob Storage-ból |} Microsoft Docs"
description: "Azure Blob Storage SSIS-összekötők használata adatok tooor áthelyezése."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 15068af1c69f11e74e109ee5ae2b9f1a674ea388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooor-from-azure-blob-storage-using-ssis-connectors"></a>Az Azure Blob Storage SSIS-összekötők használata adatok tooor áthelyezése
Hello [SQL Server Integration Services funkciócsomag a Azure](https://msdn.microsoft.com/library/mt146770.aspx) biztosít összetevők tooconnect tooAzure adatátviteli Azure és a helyszíni adatforrások és az Azure-ban tárolt folyamat adatok között.

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

Ügyfelek hello felhőbe került át a helyszíni adatokhoz, ha azok azt bármely Azure-szolgáltatások tooleverage hello teljes hatványra emelésének Azure technológiák hello suite el. Akkor használható, például az Azure Machine Learning vagy egy HDInsight-fürtre.

Ez a művelet rendszerint lesz hello első lépése a hello [SQL](machine-learning-data-science-process-sql-walkthrough.md) és [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) forgatókönyvek.

SSIS tooaccomplish üzleti használó kanonikus forgatókönyvek tárgyalja a hibrid adatok integrációs feladatokhoz közös van szüksége, a következő témakörben: [ezzel nagyobb az SQL Server Integration Services funkciócsomag a Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog.

> [!NOTE]
> A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ebben a cikkben leírt tooperform hello feladatok rendelkeznie kell Azure-előfizetés és Azure-tárfiók beállítása. Kell ismernie az Azure storage fiók név és fiókkulcs kulcs tooupload vagy adatok letöltése.

* tooset fel egy **Azure-előfizetés**, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* Létrehozásával egy **tárfiók** és az első fiók és a kulcsadatokat, lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).

toouse hello **SSIS-összekötők**, akkor le kell töltenie:

* **Az SQL Server 2014 vagy 2016 szabvány (vagy újabb)**: telepítése magában foglalja az SQL Server Integration Services.
* **Microsoft SQL Server 2014 vagy 2016 Integration Services funkciócsomag Azure**: ezek letölthető, illetve a hello [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) és [az SQL Server 2016 Integration Szolgáltatások](https://www.microsoft.com/download/details.aspx?id=49492) lapokat.

> [!NOTE]
> SSIS SQL Server telepítve van, de nem része a hello Express verzió. Milyen alkalmazások szerepelnek az SQL Server különböző kiadásai a további információkért lásd: [SQL Server kiadásai](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

Oktatási anyagok SSIS lásd [beavatkozás nélküli a képzés SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Információ tooget fel futó SISS toobuild egyszerű kinyerési, átalakítási és betöltési (ETL) csomagok használata, lásd: [SSIS-oktatóanyag: egyszerű ETL-csomag létrehozása](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Töltse le a NYC Taxi adatkészlet
hello leírt itt nyilvánosan elérhető adatkészlet használata – hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) adatkészlet. hello adatkészlet hamarosan 173 millió taxi fel a következőt: hello évben 2013 áll. Az adatok két típusa van: út részletezi az adatok és a jegy ára adatokat. Minden hónapban van egy fájlt, mert tudunk 24 fájlok összes, amely egy tömörítetlen körülbelül 2GB.

## <a name="upload-data-tooazure-blob-storage"></a>Töltse fel az adatok tooAzure blob-tároló
hello SSIS toomove adatok funkciócsomag helyszíni tooAzure blobtárolóból, hello példányának használjuk [ **Azure Blob feltöltése feladat**](https://msdn.microsoft.com/library/mt146776.aspx), itt látható:

![Konfigurálja-adatok-tudományos-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

tevékenység által használt hello hello paraméterek részelemcímkék ismertetését itt:

| Mező | Leírás |
| --- | --- |
| **AzureStorageConnection** |Egy meglévő Azure Storage Connection Manager határozza meg, vagy létrehoz egy új üzemeltetett egy tooan Azure storage-fiók, amely toowhere hello blob fájlok hivatkozik. |
| **BlobContainer** |Hello feltöltött fájlokat blobként hello blobtárolóban hello nevét adja meg. |
| **BlobDirectory** |Megadja a hello blob könyvtárat, a blokkblob hello feltöltött fájl tárolására. hello blob könyvtár virtuális hierarchikus. Ha hello blob már létezik, it ia helyett. |
| **LocalDirectory** |Hello helyi könyvtárat, amely tartalmazza a hello fájlok toobe feltöltött határozza meg. |
| **Fájlnév** |Megadja egy név szűrő tooselect fájlokat hello megadott név mintával. Például MySheet\*.xls\* például MySheet001.xls és MySheetABC.xlsx fájlokat tartalmazza |
| **TimeRangeFrom/TimeRangeTo** |Megadja, idő tartomány szűrőt. A fájlok után módosítva *TimeRangeFrom* és előtt *TimeRangeTo* tartalmazza. |

> [!NOTE]
> Hello **AzureStorageConnection** hitelesítő adatokat kell a megfelelő toobe és hello **BlobContainer** hello átviteli megkísérlése előtt.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Az Azure blob storage adatok letöltése
az Azure blob storage tooon helyszíni storage SSIS, a toodownload adatok hello-példány használata [Azure Blob feltöltése feladat](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Speciális SSIS-Azure-forgatókönyvek
hello SSIS funkciócsomag csomagolási feladat által kezelt együtt összetett adatfolyamok toobe teszi lehetővé. Például hello blob sikerült adatcsatorna közvetlenül a HDInsight-fürtöt, amelynek kimenete hátsó tooa blob és tooon helyszíni tárolási letöltése sikerült. SSIS egy HDInsight-fürt további SSIS-összekötők használata futtathat Hive és a Pig feladatot:

* SSIS, használja a Hive parancsfájl az Azure hdinsight-fürtöt toorun [Azure HDInsight Hive feladat](https://msdn.microsoft.com/library/mt146771.aspx).
* SSIS, használja a Pig-parancsprogram egy Azure hdinsight-fürtöt toorun [Azure HDInsight Pig feladatot](https://msdn.microsoft.com/library/mt146781.aspx).

