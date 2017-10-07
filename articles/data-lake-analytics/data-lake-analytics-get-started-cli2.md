---
title: "az Azure CLI 2.0 verziót használja Azure Data Lake Analytics használatába aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület 2.0 toocreate Data Lake Analytics-fiók, hozzon létre egy Data Lake Analytics-feladatot U-SQL használatával, valamint hello feladat elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: c4e91c0d3526e4932c2948c0a326d4cedc985791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a>Az Azure Data Lake Analytics használatának első lépései az Azure parancssori felületének 2.0-s verziójával
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Az oktatóanyaggal egy olyan feladatot fog elkészíteni, amely beolvas egy tabulátorral elválasztott értékeket tartalmazó fájlt (TSV), majd konvertálja azt egy vesszővel elválasztott értékeket tartalmazó fájllá (CSV). toogo keresztül hello ugyanezt az oktatóanyagot más használatával támogatott eszközöket, hello hello felül Ez a szakasz a legördülő lista használata.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI 2.0**. Lásd: [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (Az Azure parancssori felület telepítése és konfigurálása).

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

az Azure-előfizetés tooyour toolog:

```
azurecli
az login
```

Kért toobrowse tooa URL-címet, és egy hitelesítési kód megadására.  És a hitelesítő adatok hello utasításokat tooenter kövesse.

Ha már bejelentkezett, hello bejelentkezési parancs felsorolja az előfizetések.

az adott előfizetést toouse:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics-fiók létrehozása
A feladatok futtatásához rendelkeznie kell egy Data Lake Analytics-fiókkal. toocreate Data Lake Analytics-fiók, meg kell adnia a következő elemek hello:

* **Azure-erőforráscsoport**. A Data Lake Analytics-fiókot egy Azure-erőforráscsoportban kell létrehoznia. [Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toowork hello erőforrásokkal egy csoportként az alkalmazás lehetővé teszi. Telepítéséhez, frissítéséhez, vagy törölje az összes hello erőforrások egyetlen, koordinált műveletben az alkalmazáshoz.  

toolist hello meglévő erőforráscsoportok az előfizetéshez tartozó:

```
az group list
```

Új erőforráscsoport toocreate:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **A Data Lake Analytics-fiók neve**. Minden Data Lake Analytics-fiók rendelkezik egy névvel.
* **Hely**. Hello Azure-adatközpont, amely támogatja a Data Lake Analytics egyikét használhatja.
* **Alapértelmezett Data Lake Store-fiók:** minden Data Lake Analytics-fiókhoz tartozik egy alapértelmezett Data Lake Store-fiók.

toolist hello meglévő Data Lake Store-fiók:

```
az dls account list
```

új Data Lake Store-fiók toocreate:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

A következő szintaxist toocreate Data Lake Analytics-fiók hello használata:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Fiók létrehozása után a következő parancsok toolist hello fiókok hello használata, és a fiók adatainak megjelenítése:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-toodata-lake-store"></a>TooData Lake adattárban feltöltése
Az alábbi oktatóanyagban keresési naplókat fog feldolgozni.  hello keresési napló tárolható Data Lake-adattárban vagy Azure Blob Storage tárolóban.

hello Azure portál felhasználói felületet biztosít néhány minta az fájlok toohello alapértelmezett Data Lake Store fiókja, többek között a napló fájl másolása. Lásd: [forrásadatok előkészítése](data-lake-analytics-get-started-portal.md) tooupload hello adatok toohello alapértelmezett Data Lake Store-fiók.

tooupload fájlokat a CLI 2.0, a következő parancsok hello használata:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

A Data Lake Analytics az Azure Blob Storage-hoz is rendelkezik hozzáféréssel.  Adattárolás tooAzure Blob feltöltése, lásd: [az Azure Storage Azure CLI használata hello](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics-feladatok küldése
Data Lake Analytics-feladatok hello hello U-SQL nyelv nyelven íródtak. További információk a U-SQL, toolearn lásd [Ismerkedés a U-SQL nyelv](data-lake-analytics-u-sql-get-started.md) és [U-SQL nyelv eence](http://go.microsoft.com/fwlink/?LinkId=691348).

**a Data Lake Analytics-feladatparancsfájl toocreate**

Hozzon létre egy szövegfájlt az alábbi U-SQL-parancsfájlt, és mentse a hello szöveges fájl tooyour munkaállomáson:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

A U-SQL-parancsfájl beolvassa hello forrásadatfájlt **Extractors.Tsv()**, majd létrehoz egy csv fájl használatával **Outputters.Csv()**.

Hello két elérési utak nem módosítható, kivéve, ha hello forrásfájl átmásolja egy másik helyre.  A Data Lake Analytics hello kimeneti mappát hoz létre, ha még nem létezik.

Egyszerűbb toouse relatív elérési utak alapértelmezett Data Lake Store-fiók tárolt fájlok is. De használhat abszolút elérési utakat is.  Példa:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

A társított tárfiókokban tooaccess fájlok abszolút elérési utakat kell használnia.  hello csatolt Azure Storage-fiókban tárolt fájlok szintaxisa a következő:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> A nyilvános blobokat tartalmazó Azure Blob-tárolók nem támogatottak.      
> A nyilvános tárolókat tartalmazó Azure Blob-tárolók nem támogatottak.      
>

**toosubmit feladatok**

A következő szintaxist toosubmit egy feladat hello használata.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Példa:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**toolist feladatok és a feladat részleteinek megjelenítése**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**toocancel feladatok**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Feladatok eredményeinek lekérése

A feladat befejezése után használja a következő parancsok toolist hello kimeneti fájlok hello, és hello fájlok letöltése:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Példa:

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a>Folyamatok és ismétlődések

**Folyamatok és ismétlődések adatainak lekérése**

Használjon hello `az dla job pipeline` toosee hello csővezeték korábban információkat feladatok parancsok.

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

Használjon hello `az dla job recurrence` toosee hello ismétlődési információkat az előzőleg elküldött feladatok parancsokat.

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a>Következő lépések

* Tekintse meg a Data Lake Analytics CLI 2.0 referenciadokumentum, toosee hello [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).
* Tekintse meg a Data Lake Store CLI 2.0 referenciadokumentum, toosee hello [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).
* egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).
