---
title: "aaaUse PowerShell tooget Azure Data Lake Store használatába |} Microsoft Docs"
description: "Használja az Azure PowerShell toocreate Data Lake Store-fiók és alapszintű műveletek végrehajtása"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 9c958bfd63e412ec0b0a4113a149f61aee026bc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Az Azure Data Lake Store használatának első lépései az Azure PowerShell használatával
> [!div class="op_single_selector"]
> * [Portál](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Ismerje meg, hogyan toouse Azure PowerShell toocreate egy Azure Data Lake tárolásához fiók és alapvető műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le, a fiók törlése stb. További információk a Data Lake Store-ról: [Overview of Data Lake Store](data-lake-store-overview.md) (A Data Lake Store áttekintése).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Az **Azure PowerShell 1.0-s vagy újabb verziója**. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="authentication"></a>Authentication
Ez a cikk egy egyszerűbb hitelesítési módszert alkalmaz a Data Lake Store felszólító tooenter az Azure-fiók hitelesítő adatokat használ. hello hozzáférési szint tooData Lake Store fiók és a fájl rendszer majd hello hozzáférési szint a bejelentkezett felhasználó hello szabályozza. Van azonban más megoldások jól tooauthenticate a Data Lake Store, mint amelyek **végfelhasználói hitelesítési** vagy **szolgáltatások közötti hitelesítési**. További információt és útmutatást tooauthenticate, lásd: [végfelhasználói hitelesítési](data-lake-store-end-user-authenticate-using-active-directory.md) vagy [szolgáltatások közötti hitelesítési](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store-fiók létrehozása
1. Az asztalon nyisson meg egy új Windows PowerShell ablakot, és adja meg a következő kódrészletet toolog az Azure-fiók tooyour hello hello előfizetés beállítása és hello Data Lake Store-szolgáltató regisztrálása. Amikor felszólító toolog, győződjön meg arról, hogy jelentkezik be a rendelkezésre álló hello előfizetés rendszergazdájaként/tulajdonosaként:

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Az Azure Data Lake Store-fiókok egy Azure-erőforráscsoporthoz vannak társítva. Először hozzon létre egy Azure-erőforráscsoportot.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure-erőforráscsoport létrehozása](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Azure-erőforráscsoport létrehozása")
3. Hozzon létre egy Azure Data Lake Store-fiókot. megadott hello neve csak kisbetűket és számokat tartalmazhat.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure Data Lake Store-fiók létrehozása](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Azure Data Lake Store-fiók létrehozása")
4. Győződjön meg arról, hogy hello fiók sikeresen létrejött.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    a Ez lehet kimeneti hello **igaz**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Könyvtárstruktúrák létrehozása az Azure Data Lake Store-ban
Könyvtárak létrehozása az Azure Data Lake Store-fiók toomanage alatt, és adatok tárolásához.

1. Adjon meg egy gyökérkönyvtárat.

        $myrootdir = "/"
2. Hozzon létre egy új könyvtárat **mynewdirectory** hello megadott legfelső szintű.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Ellenőrizze, hogy új hello könyvtárba sikeresen létrehozva.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Hello hasonló kimenetnek kell megjelennie:

    ![A könyvtár ellenőrzése](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "A könyvtár ellenőrzése")

## <a name="upload-data-tooyour-azure-data-lake-store"></a>Adatok tooyour Azure Data Lake Store feltöltése
Az adatok tooData Lake Store közvetlenül gyökérkönyvtárában hello szint vagy tooa hello fiókon belül létrehozott feltölthet. hello alábbi kódtöredékek bemutatják, hogyan tooupload néhány minta toohello adatkönyvtára (**mynewdirectory**) hello előző szakaszban létrehozott.

Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Töltse le a hello fájlt, és a számítógépre, például C:\sampledata\ egy helyi könyvtárban tárolja.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>A Data Lake Store-ban lévő adatok átnevezése, letöltése és törlése
toorename egy fájl a következő parancs hello használata:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

toodownload egy fájl a következő parancs hello használata:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

toodelete egy fájl a következő parancs hello használata:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Amikor a rendszer kéri, adja meg a **Y** toodelete hello elemet. Ha több fájl toodelete, megadhatja az összes hello elérési utat, vesszővel elválasztva.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Az Azure Data Lake Store-fiók törlése
A következő parancs toodelete hello a Data Lake Store-fiókot használni.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Amikor a rendszer kéri, adja meg a **Y** toodelete hello fiók.

## <a name="performance-guidance-while-using-powershell"></a>Teljesítménnyel kapcsolatos útmutató a PowerShell használata során

Az alábbiakban hello legfontosabb beállítások, amelyek lehetnek bennünket tooget hello lehető legjobb teljesítményt PowerShell toowork a Data Lake Store használata során:

| Tulajdonság            | Alapértelmezett | Leírás |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Ez a paraméter lehetővé teszi toochoose hello száma párhuzamos szálainak vagy minden fájl feltöltése. Ez a szám hello maximális szálak / fájl felosztható jelöli, de kevesebb szálak kaphat a forgatókönyvtől függően (pl. egy 1 KB fájlt feltölteni, ha elérhetővé válik egy szálat még akkor is, ha 20 szálak fel).  |
| ConcurrentFileCount | 10      | Ez a paraméter kifejezetten a mappák fel- és letöltéséhez kapcsolódik. Ez a paraméter meghatározza, hogy hello száma párhuzamos feltöltött, illetve letöltött fájlokat. Ez a szám hello feltöltött, illetve egy időben letöltött egyidejű fájlok maximális számát jelöli, de kevesebb párhuzamossági kaphat a forgatókönyvtől függően (pl. két fájlt feltölteni, ha két egyidejű fájlfeltöltéseket kapja, még akkor is, ha meg kell kérni. 15). |

**Példa**

Ez a parancs letölti a fájlokat az Azure Data Lake Store toohello felhasználói helyi meghajtóról 20 szálak számát, és 100 egyidejű fájlokat használja.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-hello-value-tooset-for-these-parameters"></a>Hogyan állapítható meg e paraméterek hello érték tooset?

Az alábbiakban olvashat némi útmutatást ezzel kapcsolatban.

* **1. lépés: Hello teljes szálak számának meghatározása** -kiszámításának hello teljes szál száma toouse el. Általánosságban 6 szálat használjon minden egyes fizikai maghoz.

        Total thread count = total physical cores * 6

    **Példa**

    Feltéve, hogy futtatja a hello PowerShell-parancsok D14 VM 16 maggal rendelkező

        Total thread count = 16 cores * 6 = 96 threads


* **2. lépés: Kiszámításához PerFileThreadCount** -azt a PerFileThreadCount hello fájlok hello méretének kiszámításához. 2.5 GB-nál kisebb fájlok esetében nincs nincs szükség toochange ennek a paraméternek, mert hello alapértelmezett 10 is elegendő. 2.5 GB-nál nagyobb méretű fájlokhoz 10 szálat hello alapja hello első 2,5 GB, és a fájlméret 1 szál minden további 256 MB-os növekedést hozzáadása kell használnia. Ha olyan mappát másol, amelyben nagyon eltérő méretű fájlok találhatók, érdemes hasonló fájlméret alapján csoportosítani a fájlokat. Az eltérő fájlméretek az optimálisnál kisebb teljesítménnyel járhatnak. Ha ez nem lehetséges toogroup hasonló méretűek, célszerű PerFileThreadCount hello legnagyobb mérete alapján.

        PerFileThreadCount = 10 threads for hello first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Példa**

    Ha pedig az 1 GB-os too10GB közötti 100 fájlokat, használjuk hello 10 GB-os, hello legnagyobb fájlméret egyenlet, amely hasonló hello olvashatók.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **3. lépés: Kiszámításához ConcurrentFilecount** -használata hello teljes szálak száma és PerFileThreadCount toocalculate ConcurrentFileCount alapján a következő egyenlet hello.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Példa**

    A jelenleg használt hello példa értékek alapján

        96 = 40 * ConcurrentFileCount

    Igen **ConcurrentFileCount** van **2.4**, amely azt is túl kerekítése**2**.

### <a name="further-tuning"></a>További hangolás

Lehet szükség, mert a fájl mérete toowork számos további hangolása. számítási fent hello jól vagy hello fájlok nagyobb és szorosabb toohello 10 GB-os tartomány működik. Ha ehelyett sok különböző, nagyon eltérő méretű fájllal rendelkezik, akkor csökkentheti a PerFileThreadCount értékét. Hello PerFileThreadCount csökkentésével ConcurrentFileCount növelheti azt. Ezért, ha feltételezzük, hogy a fájlok többsége kisebb hello 5GB közé, az alábbiakat tehetjük újra a számítás elvégzése:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

Igen **ConcurrentFileCount** most 96/20, amely 4.8, kerekíti túl**4**.

A Folytatás tootune ezek a beállítások módosításával hello **PerFileThreadCount** felfelé és lefelé attól függően, a fájlméret hello terjesztése.

### <a name="limitation"></a>Korlátozás

* **A fájlok száma nem éri el ConcurrentFileCount**: Ha a feltölteni kívánt fájlok hello száma értéke kisebb a hello **ConcurrentFileCount** számolt ki, majd csökkentse  **ConcurrentFileCount** toobe egyenlő toohello azon fájlok száma. Használhat a fennmaradó szálak tooincrease **PerFileThreadCount**.

* **Túl sok szál**: Ha növeli a szál száma túl sok, a fürtméret növelése nélkül, a teljesítmény csökkenését hello kockázatát futtatja. Lehet versengés problémák váltáskor környezet – a hello CPU.

* **Nincs elegendő feldolgozási**: Ha hello CONCURRENCY paraméterének értéke nem megfelelő, akkor előfordulhat, hogy a fürt túl kicsi. A fürtben, ez több egyidejű hello csomópontok száma növelhető.

* **Szabályozási hibák**: Elképzelhető, hogy szabályozási hibákat tapasztal, ha az egyidejűség túl magas. Ha Ön hibák szabályozás, meg kell csökkentse hello párhuzamossági vagy, lépjen velünk kapcsolatba.

## <a name="next-steps"></a>Következő lépések
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)

