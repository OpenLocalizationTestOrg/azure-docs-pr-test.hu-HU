---
title: "az Azure portál tooget aaaUse Data Lake Store használatába |} Microsoft Docs"
description: "Hello Azure portál toocreate egy Data Lake Store-fiókot használja, és alapszintű műveletek végrehajtása a Data Lake Store hello"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/06/2017
ms.author: nitinme
ms.openlocfilehash: 6bb3413f00bfa4393f08aed18bc1d5f8a2f28fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-hello-azure-portal"></a>Ismerkedés az Azure Data Lake Store használatának hello Azure-portálon
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

Megtudhatja, hogyan toouse hello Azure portál toocreate egy Azure Data Lake Store-fiókot, és alapvető műveleteket, mint például mappák létrehozása, és feltöltése adatfájlok le, a fiók törlése stb. További információkat [az Azure Data Lake Store áttekintésében](data-lake-store-overview.md) olvashat.

a következő két videók hello tartalmazhat hello ugyanazokat az információkat, ebben a cikkben leírtak szerint:

* [Data Lake Store-fiók létrehozása](https://mix.office.com/watch/1k1cycy4l4gen)
* [Adatkezelés a Data Lake Store hello adatkezelő használatával](https://mix.office.com/watch/icletrxrh6pc)

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store-fiók létrehozása

1. Jelentkezzen be az új toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **NEW** (új) **Data + Storage** (Adatok és tárolás), majd az **Azure Data Lake Store** elemre. Hello hello olvasni **Azure Data Lake Store** panelt, és kattintson **létrehozása** hello bal alsó sarkában hello panelen található.
3. A hello **új Data Lake Store** panelen adjon meg hello értékeket, ahogy az alábbi képernyőfelvétel a hello:
   
    ![Új Azure Data Lake Store-fiók létrehozása](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Új Azure Data Lake Store-fiók létrehozása")
   
   * **Név**. Adjon meg egy egyedi nevet hello Data Lake Store-fiók.
   * **Előfizetés**. Válassza ki a használt toocreate új Data Lake Store-fiók kívánt hello előfizetést.
   * **Erőforráscsoport**. Válasszon ki egy meglévő erőforráscsoportot, vagy válassza ki a hello **hozzon létre új** beállítás toocreate egyet. Az erőforráscsoport egy tároló, amely alkalmazásokhoz kapcsolódó erőforrásokat tárol. További információk: [Erőforráscsoportok az Azure-ban](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Hely**: jelöljön ki egy helyet, ahová toocreate hello Data Lake Store-fiók.
   * **Titkosítási beállítások**. Három beállítás érhető el:
     
     * **A titkosítás letiltása**.
     * **Azure Data Lake által kezelt kulcsok használata**.  Ha azt szeretné, az Azure Data Lake Store toomanage a titkosítási kulcsok.
     * **Azure Key Vault-kulcsok kiválasztása**. Kiválaszthat egy meglévő Azure Key Vault-tárolót, vagy létrehozhat egy újat. a kulcstároló toouse hello kulcsokat, hozzá kell rendelnie hello tooaccess hello Azure Key Vault Azure Data Lake Store-fiók engedélyeit. Hello útmutatásért lásd: [engedélyek tooAzure Key Vault hozzárendelése](#assign-permissions-to-azure-key-vault).
       
        ![Data Lake Store-titkosítás](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Data Lake Store-titkosítás")
       
        Kattintson a **OK** a hello **titkosítási beállítások** panelen.

        További információkat [az adatok az Azure Data Lake Store-ban történő titkosítását](./data-lake-store-encryption.md) ismertető cikkben talál.

4. Kattintson a **Create** (Létrehozás) gombra. Ha úgy dönt, hogy toopin hello fiók toohello irányítópult, visszakerül toohello irányítópultot, és láthatja a Data Lake Store-fiók kiépítés hello előrehaladását. Egyszer hello Data Lake Store-fiók üzembe helyezése hello fiók panelen megjelenik.

Az Azure Resource Manager-sablonok használatával is létrehozhat Data Lake Store-fiókokat. Ezek a sablonok az [Azure gyors üzembehelyezési sablonokból](https://azure.microsoft.com/resources/templates/?term=data+lake+store) érhetők el:

- Adattitkosítás nélkül: [Azure Data Lake Store-fiók üzembe helyezése adattitkosítás nélkül](https://azure.microsoft.com/en-us/resources/templates/101-data-lake-store-no-encryption/).
- A Data Lake Store adattitkosításának használatával: [Azure Data Lake Store-fiók üzembe helyezése titkosítással (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
- Az Azure Key Vault adattitkosításának használatával: [Azure Data Lake Store-fiók üzembe helyezése titkosítással (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).

### <a name="assign-permissions-to-azure-key-vault"></a>Engedélyek tooAzure Key Vault hozzárendelése
Ha az Azure Key Vault tooconfigure titkosítási kulcsokat a Data Lake Store-fiók hello, konfigurálnia kell a hozzáférés hello Data Lake Store-fiók és hello Azure Key Vault fiók között. Hajtsa végre a következő lépéseket toodo így hello.

1. Ha használta az Azure Key Vault hello kulcsokat, hello Data Lake Store-fiók paneljén hello figyelmeztetést jelenít meg hello tetején. Kattintson a hello figyelmeztetés tooopen hello **kulcsot tároló engedélyek konfigurálása** panelen.
   
    ![Data Lake Store-titkosítás](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Data Lake Store-titkosítás")
2. hello két beállítások tooconfigure hozzáférés megjelenik.
   
   * Hello első lehetőség, kattintson **Grant engedéllyel** tooconfigure hozzáférést. hello első lehetőség engedélyezve van, csak ha hello felhasználó által létrehozott hello Data Lake Store-fiók is hello Azure Key Vault egy rendszergazda.
   * hello másik lehetőség egy toorun hello PowerShell parancsmag hello panelen jelenik meg. Hello képességét toogrant engedélyekkel rendelkezzen az Azure Key Vault hello vagy toobe hello tulajdonosa hello Azure Key Vault szükséges. Hello parancsmag futtatása után térjen vissza toohello panelt, és kattintson a **engedélyezése** tooconfigure hozzáférést.

## <a name="createfolder"></a>Mappák létrehozása az Azure Data Lake Store-fiókban
Mappák létrehozása a Data Lake Store-fiók toomanage alatt, és adatok tárolásához.

1. Nyissa meg a létrehozott hello Data Lake Store-fiókot. Hello bal oldali ablaktáblában kattintson **Tallózás**, kattintson a **Data Lake Store**, majd hello Data Lake Store panelen, kattintson a toocreate mappák kérünk hello fióknevet. Ha rögzítette hello fiók toohello kezdőpulton, kattintson a fiók csempéjére.
2. A Data Lake Store-fiók panelén kattintson a **Data Explorer** (Adatkezelő) elemre.
   
    ![Mappák létrehozása a Data Lake Store-fiókban](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Mappák létrehozása a Data Lake Store-fiókban")
3. A Data Lake Store-fiók panelen kattintson a **új mappa**, írja be a hello új mappa nevét, és kattintson **OK**.
   
    ![Mappák létrehozása a Data Lake Store-fiókban](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Mappák létrehozása a Data Lake Store-fiókban")
   
    az újonnan létrehozott hello mappa hello szerepel **adatkezelő** panelen. Bármilyen szinten létrehozhat beágyazott mappákat.
   
    ![Mappák létrehozása a Data Lake-fiókban](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Mappák létrehozása a Data Lake-fiókban")

## <a name="uploaddata"></a>Töltse fel az adatok tooAzure Data Lake Store-fiók
Feltöltheti az adatok tooan közvetlenül gyökérmappában hello szint vagy tooa hello fiókon belül létrehozott Azure Data Lake Store-fiók. A hello képernyőképe, a következő kövesse hello lépéseket tooupload fájl tooa almappa hello **adatkezelő** panelen. A képernyőfelvételen a hello fájl feltöltött tooa almappa (vörös téglalappal jelölt) hello útkövetését látható.

Néhány példa adatok tooupload keres, ha kaphat a hello **Ambulance Data** hello mappát [Azure Data Lake Git-tárház](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Adatok feltöltése](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Adatok feltöltése")

## <a name="properties"></a>Tulajdonságok és műveletek hello a tárolt adatok
Kattintson a hello újonnan hozzáadott fájlra tooopen hello **tulajdonságok** panelen. hello fájl- és hello műveleteket hajthat végre hello fájl társított hello tulajdonságok ezen a panelen érhetők el. Hello teljes elérési útja toofile is másolhatja a Azure Data Lake Store-fiókban, piros hello mezőjében a következő képernyőkép hello kiemelve:

![Hello adatok tulajdonságainak](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "hello adatok tulajdonságai")

* Kattintson a **előzetes** toosee hello fájl közvetlenül hello böngészőből előnézetét. Hello hello előnézet formátumát is megadhat. Kattintson a **előzetes**, kattintson **formátum** hello a **fájljának előnézete** panelt, és a hello **File Preview Format** panelen adja meg a hello beállítások, például sorok toodisplay számának kódolás toouse, elválasztó karakter toouse, stb.
  
  ![A fájl előnézetének formátuma](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "A fájl előnézetének formátuma")
* Kattintson a **letöltése** toodownload hello fájl tooyour számítógép.
* Kattintson a **átnevezése fájl** toorename hello fájlt.
* Kattintson a **törölje a(z)** toodelete hello fájlt.

## <a name="secure-your-data"></a>Az adatok védelme
Az Azure Active Directory és a hozzáférés-vezérlés (ACLs) segítségével az Azure Data Lake Store-fiókban tárolt hello adatok biztonságát. Útmutatást, lásd: toodo [adatok védelme az Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Az Azure Data Lake Store-fiók törlése
toodelete az Azure Data Lake Store-fiók, a Data Lake Store panelen kattintson a **törlése**. tooconfirm hello művelet, lesz toodelete kívánja hello fiók felszólító tooenter hello nevét. Írja be a hello hello fiók nevét, és kattintson **törlése**.

![Data Lake-fiók törlése](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Data Lake-fiók törlése")

## <a name="next-steps"></a>Következő lépések
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
* [A Data Lake Store diagnosztikai naplóinak elérése](data-lake-store-diagnostic-logs.md)

