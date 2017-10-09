---
title: "Azure Data Lake Analytics Azure parancssori felület használatának aaaManage |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage Data Lake Analytics-fiókok, adatforrások, feladatok és a felhasználók Azure parancssori felület használatával"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0af1f89080739b39f6980989b7694734cc135715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Azure parancssori felület (CLI) használatával Azure Data Lake Analytics kezelése
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, adatforrások, felhasználók és használó feladatok hello Azure parancssori felület. más eszközök használatával toosee felügyeleti témakörök kattintson hello lapválasztóra.


**Előfeltételek**

Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Azure parancssori felület (CLI)**. Lásd: [Install and configure Azure CLI](../cli-install-nodejs.md) (Az Azure parancssori felület telepítése és konfigurálása).
  * Töltse le és telepítse a hello **kiadás előtti** [Azure CLI-eszközeit](https://github.com/MicrosoftBigData/AzureDataLake/releases) a rendezés toocomplete ebben a bemutatóban.
* **Hitelesítési**használatával hello a következő parancsot:
  
        azure login
    Munkahelyi vagy iskolai fiókkal hitelesítésről további információkért lásd: [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../xplat-cli-connect.md).
* **Kapcsoló toohello Azure Resource Manager módra**használatával hello a következő parancsot:
  
        azure config mode arm

**toolist hello Data Lake Store és a Data Lake Analytics parancsokat:**

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a>Fiókok kezelése
Minden Data Lake Analytics-feladatok futtatásához rendelkeznie kell egy Data Lake Analytics-fiók. Azure HDInsight eltérően nem kell fizetnie az Analytics-fiók, amikor nem fut egy feladat.  Csak kell fizetnie, ha egy feladat fut. hello ideje.  További információkért lásd: [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).  

### <a name="create-accounts"></a>Fiókok létrehozása
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a>Fiókok frissítése
hello következő parancs frissíti egy meglévő Data Lake Analytics-fiók hello tulajdonságairól

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a>Fiókok listázása
Lista Data Lake Analytics-fiókok 

    azure datalake analytics account list

Egy adott erőforráscsoportban lista Data Lake Analytics-fiókok

    azure datalake analytics account list -g "<Azure Resource Group Name>"

Egy adott Data Lake Analytics-fiók részletek

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a>Törölje a Data Lake Analytics-fiókok
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Fiók adatforrások kezelése
A Data Lake Analytics jelenleg a következő adatforrások hello támogatja:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Az Analytics-fiók létrehozásakor ki kell jelölnie egy Azure Data Lake tárolási fiók toobe hello alapértelmezett tárfiók. hello alapértelmezett ADL tárfiókot használja toostore feladat metaadatok és a feladat naplók. Az Analytics-fiók létrehozása után hozzáadhat további Data Lake-tárfiókokat és/vagy az Azure Storage-fiók. 

### <a name="find-hello-default-adl-storage-account"></a>Hello alapértelmezett ADL tárfiók keresése
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

hello érték megtalálható-e tulajdonságok: datalakeStoreAccount:name.

### <a name="add-additional-azure-blob-storage-accounts"></a>További Azure Blob storage-fiókok hozzáadása
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> Csak a Blob storage rövid nevek használata támogatott.  Ne használjon teljes Tartománynevet, például "myblob.blob.core.windows.net".
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a>További Data Lake Store-fiók hozzáadása
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

[-d] egy választható kapcsoló tooindicate van-e hozzáadni kívánt Data Lake hello hello alapértelmezett Data Lake-fiók. 

### <a name="update-existing-data-source"></a>Meglévő adatforrás frissítése.
egy meglévő Data Lake Store fiók toobe hello alapértelmezett tooset:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

a Blob storage meglévő fiókkulcs tooupdate:

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a>Az adatforrások listája:
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![A Data Lake Analytics lista adatforrás](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a>Adatforrások törlése:
Data Lake Store-fiók toodelete:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

a Blob storage-fiók toodelete:

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a>Feladatok kezelése
Data Lake Analytics-fiók egy feladat létrehozása előtt kell rendelkeznie.  További információkért lásd: [kezelése Data Lake Analytics-fiókok](#manage-accounts).

### <a name="list-jobs"></a>Lista feladatok
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![A Data Lake Analytics lista adatforrás](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a>Lekérhet feladatadatokat
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a>Feladatok elküldéséhez
> [!NOTE]
> hello alapértelmezett prioritás feladat 1000, és a feladat párhuzamossági fokát hello alapértelmezett értéke 1.
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a>Feladatok megszakítása
Hello lista parancs toofind hello feladatazonosító használja, és aztán toocancel hello feladat.

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a>Katalógus kezelése
hello U-SQL catalog használt toostructure adatok és a kód, így azok megoszthatók U-SQL-parancsfájlok. hello katalógus lehetővé teszi, hogy a lehető legjobb teljesítmény hello Azure Data Lake-adatokkal szolgálja. További információk: [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md) (U-SQL-katalógus használata).

### <a name="list-catalog-items"></a>A szolgáltatáskatalógusban található elemek listája
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

hello típusok tartalmazza az adatbázis, a séma, a szerelvény, a külső adatforrás, a tábla, a táblázat értékű függvény és a tábla statisztikai adatainak.

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a>ARM-csoportok használata
Az alkalmazások általában számos összetevőből állnak, például webalkalmazásból, adatbázisból, adatbázis-kiszolgálóból, tárolóból és külső szolgáltatásokból. Az Azure Resource Manager (ARM) lehetővé teszi az alkalmazás egy csoportot, hivatkozott tooas Azure-erőforráscsoport hello erőforrásokat toowork. Központi telepítése, frissítése, figyelheti vagy törölje az összes hello erőforrások egyetlen, koordinált műveletben az alkalmazáshoz. A telepítéshez egy sablont használ, amely különböző, például tesztelési, átmeneti és üzemi környezetekben is képes működni. Tisztázhatja a szervezete teljes csoport hello hello összegzett költségeinek megtekintésével. További információk: [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése). 

A Data Lake Analytics szolgáltatás a következő összetevők hello az alábbiakból állhat:

* Azure Data Lake Analytics-fiók
* Szükséges alapértelmezett Azure Data Lake-tárfiókra
* További Azure Data Lake-tárfiókokat
* További Azure Storage-fiókok

Létrehozhat egy ARM-csoport toomake alatt mindezen összetevők azokat könnyebben toomanage.

![Azure Data Lake Analytics-fiók és tárolás](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Data Lake Analytics-fiók és a függő tárfiókok hello kell helyezni hello azonos Azure-adatközpont.
azonban hello ARM csoport elhelyezhető egy másik adatközpont.  

## <a name="see-also"></a>Lásd még:
* [A Microsoft Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [A Data Lake Analytics használatának első lépései az Azure Portallal](data-lake-analytics-get-started-portal.md)
* [Az Azure Data Lake Analytics kezelése az Azure Portal használatával](data-lake-analytics-manage-use-portal.md)
* [Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portallal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

