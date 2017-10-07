---
title: "az Azure parancssori 2.0 aaaUse felület tooget Azure Data Lake Store használatába |} Microsoft Docs"
description: "Használja az Azure platformfüggetlen parancssori 2.0 toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 374dcd6cdbc13ad19f6c65502329986ecae60ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a>Az Azure Data Lake Store használatának első lépései az Azure CLI 2.0 használatával
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Ismerje meg, hogyan toouse Azure CLI 2.0 toocreate egy Azure Data Lake tárolásához fiók és alapvető műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).

hello Azure CLI 2.0 Azure új parancssori felületet Azure-erőforrások kezeléséhez. A szolgáltatás macOS, Linux és Windows rendszereken használható. További információért lásd: [Az Azure CLI 2.0 áttekintése](https://docs.microsoft.com/cli/azure/overview). Is megtalálhatja hello [Azure Data Lake Store CLI 2.0 hivatkozás](https://docs.microsoft.com/cli/azure/dls) teljes listáját a parancsokat és szintaxist.


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI 2.0** – az utasításokért lásd: [Az Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="authentication"></a>Authentication

Ez a cikk egy egyszerűbb hitelesítési módszert használ a Data Lake Store-ral, ahol Ön végfelhasználóként jelentkezik be. hello hozzáférési szint tooData Lake Store fiók és a fájl rendszer majd hello hozzáférési szint a bejelentkezett felhasználó hello szabályozza. Van azonban más megoldások jól tooauthenticate a Data Lake Store, mint amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**. További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).


## <a name="log-in-tooyour-azure-subscription"></a>Jelentkezzen be tooyour Azure-előfizetés

1. Jelentkezzen be az Azure-előfizetésébe.

    ```azurecli
    az login
    ```

    A kód toouse hello következő lépésben kapott. Egy webes böngésző tooopen hello lap https://aka.ms/devicelogin használata, és írja be a hello kód tooauthenticate. A hitelesítő adataival felszólító toolog áll.

2. Jelentkezzen be, miután hello ablak felsorolja az összes hello Azure-fiókjához társított előfizetéseket. A következő parancs toouse adott előfizetés hello használata.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store-fiók létrehozása

1. Hozzon létre egy új erőforráscsoportot. A következő parancs hello adja meg hello toouse kívánt paraméterértékeket. Ha hello a hely neve szóközt tartalmaz, tegye idézőjelek közé foglalt. Például: „USA 2. keleti régiója”. 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Hello Data Lake Store-fiók létrehozása.
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappák létrehozása Data Lake Store-fiókban

Mappák létrehozása az Azure Data Lake Store-fiók toomanage alatt, és adatok tárolásához. A következő parancs toocreate nevű egy mappát használja hello **mynewfolder** : hello hello Data Lake Store gyökérmappájában.

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> Hello `--folder` paraméter biztosítja, hogy hello parancs létrehoz egy mappát. Ha ez a paraméter nincs megadva, hello parancs létrehoz egy üres nevű mynewfolder: hello hello Data Lake Store-fiók gyökérkönyvtárában.
> 
>

## <a name="upload-data-tooa-data-lake-store-account"></a>Töltse fel az adatok tooa Data Lake Store-fiók

Feltöltheti tooData Lake adattárban közvetlenül gyökérmappában hello szint vagy tooa hello fiókon belül létrehozott. hello alábbi kódtöredékek bemutatják, hogyan tooupload néhány minta toohello Adatmappa (**mynewfolder**) hello előző szakaszban létrehozott.

Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Töltse le a hello fájlt, és a számítógépre, például C:\sampledata\ egy helyi könyvtárban tárolja.

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Hello cél hello teljes elérési útját együtt hello fájlnevet kell megadnia.
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a>A Data Lake Store-fiók fájljainak listázása

Használja a következő parancs toolist hello fájlok egy Data Lake Store-fiókban hello.

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

hello kimenet az hasonló toohello következő legyen:

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a>A Data Lake Store-fiókban lévő adatok átnevezése, letöltése és törlése 

* **a fájl toorename**, a következő parancs hello használata:
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **a fájl toodownload**, használja a következő parancs hello. Győződjön meg arról, hogy hello cél elérési út már létezik.
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > hello parancs hello célmappa hoz létre, ha nem létezik.
    > 
    >

* **a fájl toodelete**, a következő parancs hello használata:
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Ha azt szeretné, hogy toodelete hello mappa **mynewfolder** és hello fájl **vehicle1_09142014_copy.csv** együtt egy parancsban, használjon hello – recurse paraméter

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a>A Data Lake Store-fiókhoz tartozó engedélyek és a hozzáférés-vezérlési listák használata

Ebben a szakaszban, megtudhatja, hogyan toomanage hozzáférés-vezérlési listák és az engedélyek az Azure CLI 2.0 verziót használja. A hozzáférés-vezérlési listák Azure Data Lake Store-beli használatának részletes leírásáért lásd: [Az Azure Data Lake Store szolgáltatásban található hozzáférés-vezérlés](data-lake-store-access-control.md).

* **egy fájl vagy mappa tulajdonosának tooupdate hello**, a következő parancs hello használata:

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* **egy fájl vagy mappa engedélyeit tooupdate hello**, a következő parancs hello használata:

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* **a megadott elérési út a hozzáférés-vezérlési listák tooget hello**, a következő parancs hello használata:

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    hello kimeneti hasonló toohello következő legyen:

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* **egy bejegyzést a hozzáférés-vezérlési Listában tooset**, a következő parancs hello használata:

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* **egy bejegyzést a hozzáférés-vezérlési Listában tooremove**, a következő parancs hello használata:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* **egy teljes alapértelmezett hozzáférés-vezérlési lista tooremove**, a következő parancs hello használata:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* **egy teljes nem alapértelmezett ACL tooremove**, a következő parancs hello használata:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a>Data Lake Store-fiók törlése
A következő parancs toodelete Data Lake Store-fiók hello használata.

```azurecli
az dls account delete --account mydatalakestore
```

Amikor a rendszer kéri, adja meg a **Y** toodelete hello fiók.

## <a name="next-steps"></a>Következő lépések

* [Az Azure Data Lake Store CLI 2.0 dokumentációja](https://docs.microsoft.com/cli/azure/dls)
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
