---
title: az Azure Storage Azure CLI 1.0 aaaUsing hello |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület (CLI) 1.0 az Azure Storage toocreate és a storage-fiókok kezelése és az Azure-blobokat és fájlok használatát. hello Azure parancssori felület egy olyan többplatformos eszköz"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 25e459403dde631741403c8722ed07beafac35c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-10-with-azure-storage"></a>Az Azure Storage hello Azure CLI 1.0 használatával

## <a name="overview"></a>Áttekintés

hello Azure parancssori felület számos nyílt forráskódú, platformok közötti parancsok végzett munka hello Azure platformra. Nagy részét hello hello található ugyanezeket a funkciókat biztosít [Azure-portálon](https://portal.azure.com) , valamint a gazdag adat-hozzáférési funkciókat.

Ez az útmutató azt fogja feltárja hogyan toouse [Azure parancssori felület (CLI)](../../cli-install-nodejs.md) tooperform számos fejlesztését és a felügyeleti feladatot az Azure Storage. Azt javasoljuk, hogy letöltése és telepítése vagy frissítése toohello Azure CLI legújabb az útmutató használata előtt.

Ez az útmutató feltételezi, hogy tudomásul veszi hello Azure Storage alapvető fogalmait. hello az útmutató számos parancsfájlt toodemonstrate hello használata az Azure Storage Azure CLI hello. Lehet, hogy tooupdate hello parancsfájl-változókat minden parancsprogram futtatása előtt a konfiguráció alapján.

> [!NOTE]
> hello útmutató példákat hello Azure CLI parancs és a parancsfájl klasszikus tárfiókokat. Lásd: [Using hello Azure parancssori felület Mac, Linux és Windows Azure Resource Manager](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) az erőforrás-kezelő storage-fiókok Azure parancssori felület parancsait.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-hello-azure-cli-in-5-minutes"></a>Ismerkedés az Azure Storage és hello Azure CLI 5 percben
Ez az útmutató Ubuntu használja példák, de más operációs Rendszeri platformokon hasonló módon végre kell hajtania.

**Új tooAzure:** a Microsoft Azure-előfizetés és az adott előfizetéshez tartozó Microsoft-fiókkal. Az Azure megvásárlási lehetőségeinek információkért lásd: [ingyenes](https://azure.microsoft.com/pricing/free-trial/), [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/), és [ajánlatok](https://azure.microsoft.com/pricing/member-offers/) (az MSDN, a Microsoft Partner Network, és a BizSpark és egyéb Microsoft programok tagjai).

Lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure-előfizetések további információt.

**Miután létrehozta a Microsoft Azure-előfizetésre és -fiókra:**

1. Töltse le és telepítse az Azure parancssori felület hello utasításokat a következő témakörben ismertetett hello [telepítés hello Azure CLI](../../cli-install-nodejs.md).
2. Hello Azure parancssori felület telepítése után, fogja tudni toouse hello azure parancsot a parancssori felület (Bash, terminál, parancssor) tooaccess hello Azure parancssori felület parancsait. Típus hello _azure_ parancsot, és meg kell jelennie a következő kimeneti hello.

    ![Az Azure parancs kimenete](./media/storage-azure-cli/azure_command.png)   
3. Hello parancssori felület, írja be `azure storage` minden toolist az azure storage parancsok hello és hello funkciók hello Azure parancssori Felületet biztosít első benyomást kell szereznie. Beírhatja a parancsnév **-h** paraméter (például `azure storage share create -h`) parancsszintaxis toosee részleteit.
4. Most lesz ad egy egyszerű parancsprogram, amely tartalmazza az alapszintű Azure CLI parancsok tooaccess Azure Storage. hello parancsfájl először kérni fogja tooset két változó a tárfiók és a kulcsot. Ezután hello parancsfájl hozzon létre egy új tárolót az új tárfiókot, és töltse fel a meglévő lemezkép fájl (blob) toothat tároló. Miután hello parancsfájl megjeleníti az adott tároló összes BLOB, le fogja tölteni hello kép fájl toohello célkönyvtáron létező hello helyi számítógépen.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating hello container..."
    azure storage container create $container_name

    echo "Uploading hello image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing hello blobs..."
    azure storage blob list $container_name

    echo "Downloading hello image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. A helyi számítógépen nyissa meg az előnyben részesített szövegszerkesztőben (például vim). Parancsfájl fent hello írja be a szövegszerkesztőben.
6. Most tooupdate hello parancsfájl-változókat a konfigurációs beállítások alapján van szüksége.

   * **< Storage_account_name >** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tárfiók. **Fontos:** hello tárfiókja nevére hello Azure egyedinek kell lennie. Az kisbetűnek kell lennie, túl!
   * **< storage_account_key >** hello hozzáférési kulcsot a tárfiók.
   * **< Container_name >** hello megadott hello parancsfájl nevét használja, vagy adjon meg egy új nevet a tároló.
   * **< Image_to_upload >** , mint a helyi számítógépen, adja meg egy elérési utat tooa kép: "~ / images/HelloWorld.png".
   * **< Destination_folder >** adjon meg egy elérési utat tooa helyi könyvtár toostore fájlokat töltött le az Azure Storage, például: "~/downloadImages".
7. Hello szükséges változók vim frissítése után nyomja le az billentyűkombinációk `ESC`, `:`, `wq!` toosave hello parancsfájl.
8. toorun ezt a parancsfájlt, egyszerűen típus hello parancsprogramfájljának nevét hello bash konzolon. Ez a parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött hello lemezképfájlt. a következő képernyőkép hello látható egy példa a kimenetre:

Hello parancsfájl futtatása után rendelkeznie kell egy helyi célmappát, amely tartalmazza a letöltött hello lemezképfájlt.

## <a name="manage-storage-accounts-with-hello-azure-cli"></a>Az Azure CLI hello storage-fiókok kezelése
### <a name="connect-tooyour-azure-subscription"></a>Csatlakozás Azure-előfizetés tooyour
Hello tárolási parancsok többsége Azure-előfizetés nélkül fog működni, de javasolt tooconnect tooyour hello Azure CLI-előfizetést. tooconfigure hello Azure CLI toowork az előfizetéséhez, a hello lépésekkel [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Új tárfiók létrehozása
az Azure storage toouse, szüksége lesz egy tárfiókot. Létrehozhat egy új Azure-tárfiókot, a számítógép tooconnect tooyour előfizetés konfigurálása után.

```azurecli
azure storage account create <account_name>
```

a tárfiók nevére hello kell 3 és 24 karakter hosszúságúnak és kell használnia csak számokat és kisbetűket tartalmazhatnak.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Környezeti változók alapértelmezett Azure storage-fiók beállítása
Az előfizetés több tárfiókot is lehet. Válassza ki az egyiket, és állítsa be azt az hello környezeti változók minden hello tároló parancsai hello ugyanazt a munkamenetet. Ez lehetővé teszi toorun hello Azure CLI tárolási hello tároló megadása nélkül parancsok fiókot, és kulcs explicit módon.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Egy másik módja tooset alapértelmezett tárfiók kapcsolati karakterláncot használ. Először is hello kapcsolati karakterlánc lekéréséhez parancs:

```azurecli
azure storage account connectionstring show <account_name>
```

Ezután másolja a hello kimeneti kapcsolati karakterláncot, és állítsa be úgy a tooenvironment változó:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Hozzon létre és blobok kezelése
Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Ez a szakasz feltételezi, hogy Ön már ismeri a hello Azure Blob storage fogalmakat. Részletes információkért lásd: [az Azure Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Tároló létrehozása
Az Azure storage összes blobjának egy tárolóban kell lennie. Létrehozhat egy személyes tárolót hello segítségével `azure storage container create` parancs:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> A névtelen olvasási hozzáférés három szintje van: **ki**, **Blob**, és **tároló**. tooprevent névtelen hozzáférés tooblobs, set hello engedély paraméter túl**ki**. Alapértelmezés szerint a hello új tároló privát, és csak a fiók tulajdonosának hello keresztül elérhető legyen. tooallow névtelen nyilvános olvasási hozzáférés tooblob erőforrásokat, de nem toocontainer metaadatok vagy toohello listája hello tárolóban lévő blobok, hello engedély paraméter értéke túl**Blob**. tooallow teljes nyilvános olvasási tooblob erőforrások eléréséhez, a tároló metaadatait, és a hello tárolóban lévő blobok hello listája, hello engedély paraméter értéke túl**tároló**. További információkért lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat. További információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).

tooa tárolóban lévő blobok tooupload, hello használható `azure storage blob upload`. Alapértelmezés szerint ez a parancs hello helyi fájlok tooa blokkblob feltöltését. toospecify hello típus hello BLOB, hello használható `--blobtype` paraméter.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Blobok letöltése a tárolóból
hello a következő példa bemutatja, hogyan toodownload blobok a tárolóból.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Blobok másolása
Tárfiókokon és régiókon belül vagy azok között aszinkron módon másolhatja át a blobokat.

hello következő példa bemutatja, hogyan toocopy blobok a egy tárolási fiók tooanother. Ez a példa azt létrehozni egy tárolót, amelyben blobokat nyilvánosan, is névtelenül érhető el.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Ez a minta egy aszinkron másolatot hajt végre. Figyelheti a hello állapotát minden egyes másolási művelet hello futtatásával `azure storage blob copy show` műveletet.

Vegye figyelembe, hogy hello forrás URL-címe hello másolási művelet a megadott kell nyilvánosan hozzáférhető, vagy egy (közös hozzáférésű jogosultságkód) SAS-jogkivonatot tartalmazza.

### <a name="delete-a-blob"></a>Blob törlése
toodelete blob, hello az alábbi parancsot használja:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Hozzon létre és fájlmegosztások kezelése
Az Azure File storage közös tárterületet hello szabványos SMB protokollt használó alkalmazások számára biztosít. A Microsoft Azure virtuális gépek és felhőszolgáltatások, valamint a helyszíni alkalmazások megosztott csatlakoztatott megosztásokon keresztül. A fájlmegosztások és a fájladatok hello Azure CLI segítségével kezelheti. Az Azure File storage további információkért lásd: [Ismerkedés az Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) vagy [hogyan toouse Linux Azure File storage](../storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Fájlmegosztás létrehozása
Egy Azure fájlmegosztás egy SMB-fájlmegosztás, az Azure-ban. Minden könyvtárak és fájlok fájlmegosztást kell létrehozni. Egy fiók korlátlan számú megosztást tartalmazhat, és egy megosztási tárolhatók fájlok mentése toohello kapacitáskorlátait hello tárfiók korlátlan számú. hello alábbi példa létrehoz egy nevű fájlmegosztás **megosztás**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Könyvtár létrehozása
Egy könyvtár egy választható hierarchikus struktúra biztosít az Azure fájlmegosztások. hello alábbi példa létrehoz egy könyvtárat nevű **könyvtárnév** hello fájlmegosztásban.

```azurecli
azure storage directory create myshare myDir
```

Vegye figyelembe, hogy a könyvtár elérési útja tartalmazhatnak több szintjéről *pl.*, **a / b**. Azonban úgy kell beállítania, hogy létezik-e az összes fölérendelt könyvtárak. Például a következő elérési út **a / b**, létre kell hoznia könyvtárat **egy** először, majd hozza létre a könyvtár **b**.

### <a name="upload-a-local-file-toodirectory"></a>Egy helyi fájl toodirectory feltöltése
hello alábbi példa feltölt egy fájlt a **~/temp/samplefile.txt** toohello **könyvtárnév** könyvtár. Szerkesztése hello fájl elérési útját, hogy tooa érvényes fájlt a helyi számítógépen:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Figyelje meg, hogy hello megosztáson található, a fájl mentése too1 TB méretű lehet.

### <a name="list-hello-files-in-hello-share-root-or-directory"></a>Hello fájlok listázása a hello megosztás legfelső szintű vagy a könyvtár
Hello fájlok és alkönyvtárak megosztás legfelső szintű vagy egy könyvtár a következő parancs hello segítségével jeleníthetők meg:

```azurecli
azure storage file list myshare myDir
```

Vegye figyelembe, hogy hello könyvtárnév hello listázása művelet esetén nem kötelező. Ha nincs megadva, hello parancs hello megosztás hello gyökérkönyvtárában hello tartalmát jeleníti meg.

### <a name="copy-files"></a>Fájlok másolása
Azure CLI 0.9.8-as verzióját kezdve másolhatja egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájlt. Az alábbiakban bemutatjuk, hogyan tooperform ezek másolása műveletekbe parancssori felület parancsait. toocopy fájl toohello új könyvtár:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

toocopy blob tooa fájl könyvtár:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Következő lépések

Azure CLI 1.0 parancsdokumentációja találhat itt tároló-erőforrások használata:

* [Az Azure parancssori felület parancsait erőforrás-kezelő módban](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Az Azure parancssori felület parancsait Azure szolgáltatásfelügyelet módban](../../cli-install-nodejs.md)

Akkor is előfordulhat, hogy például a tootry hello [Azure CLI 2.0](../storage-azure-cli.md), a következő generációs CLI hello Resource Manager üzembe helyezési modellben való használatra, pythonban írt.
