---
title: az Azure Storage Azure CLI 2.0 aaaUsing hello |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület (CLI) 2.0 az Azure Storage toocreate és a storage-fiókok kezelése és az Azure-blobokat és fájlok használatát. hello Azure CLI 2.0 pythonban írt platformfüggetlen eszköz."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: marsma
ms.openlocfilehash: 14e6eb0c913676380c90a72563276245e7f08aa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-20-with-azure-storage"></a>Az Azure Storage hello Azure CLI 2.0 használatával

hello nyílt forráskódú, platformok közötti Azure CLI 2.0 parancsokat biztosít a hello Azure platformon való munkához. Nagy részét hello hello található ugyanezeket a funkciókat biztosít [Azure-portálon](https://portal.azure.com), beleértve a funkciógazdag adatelérési.

Az útmutató azt mutatja be toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform működik-e az Azure-tárfiók erőforrásokat több feladatot. Azt javasoljuk, hogy letöltése és telepítése vagy frissítése toohello hello CLI 2.0 legújabb verzióját az útmutató használata előtt.

hello útmutatóban hello példák feltételezik hello Ubuntu Bash rendszerhéjat hello használatát, de más platformokon hasonló módon végre kell hajtania. 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató feltételezi, hogy tudomásul veszi hello Azure Storage alapvető fogalmait. Azt is feltételezi, hogy Ön képes toosatisfy hello létrehozása követelmények az Azure és a Storage szolgáltatás hello alatt megadott.

### <a name="accounts"></a>Fiókok
* **Azure-fiók**: Ha még nem rendelkezik Azure-előfizetéssel, [egy ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).
* **Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](storage-create-storage-account.md) cikk [Tárfiók létrehozása](storage-create-storage-account.md#create-a-storage-account) szakaszát.

### <a name="install-hello-azure-cli-20"></a>Hello Azure CLI 2.0 telepítése

Töltse le és telepítse az Azure CLI 2.0 hello hello ismertetett lépéseket követve [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2).

> [!TIP]
> Ha problémája merül fel a hello telepítés, tekintse meg a hello [telepítési hibák elhárítása](/cli/azure/install-az-cli2#installation-troubleshooting) hello cikk és hello [telepítése hibaelhárítási](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) útmutató a Githubon.
>

## <a name="working-with-hello-cli"></a>Hello CLI használata

Hello parancssori felület telepítése után is használhatja hello `az` parancsot a parancssori felület (Bash, terminált, parancssor) tooaccess hello Azure parancssori felület parancsait. Típus hello `az` parancs toosee hello alap parancsokat (a következő egy példa a kimenetre hello csonkolódtak) teljes listáját:

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome toohello cool new Azure CLI!

Here are hello base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

A parancssori felület hello parancs végrehajtása `az storage --help` toolist hello `storage` alcsoportokat parancsot. hello alcsoportokat hello leírását az nyújt áttekintést hello funkció hello Azure parancssori Felületet biztosít a tároló-erőforrások használata.

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use hello standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues tooeffectively scale applications according tootraffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-hello-cli-tooyour-azure-subscription"></a>Csatlakozás hello CLI tooyour Azure-előfizetés

az Azure-előfizetéshez erőforrásokat hello toowork, kell először jelentkezik be a tooyour az Azure-fiók `az login`. Többféleképpen is bejelentkezhet:

* **Interaktív bejelentkezés**:`az login`
* **Jelentkezzen be a felhasználónevet és jelszót**:`az login -u johndoe@contoso.com -p VerySecret`
  * A Microsoft és fiókok számára, hogy a többtényezős hitelesítés használata mellett nem működik.
* **Jelentkezzen be egy egyszerű**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`

## <a name="azure-cli-20-sample-script"></a>Az Azure CLI 2.0 parancsfájlpéldát

Dolgozunk lesz ezután az egy kis héjparancsfájlt, amely néhány alapvető Azure CLI 2.0 parancsok toointeract állít ki az Azure Storage-erőforrások. hello parancsfájl először létrehoz egy új tárolót a tárfiókban lévő, majd feltölti a meglévő fájlt (a blob) toothat tároló. Majd felsorolja az összes BLOB hello tárolóban, és végül letölti a hello fájl tooa cél a helyi számítógépen, amely akkor adja meg.

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating hello container..."
az storage container create --name $container_name

echo "Uploading hello file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing hello blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading hello file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

**Konfigurálja és hello parancsprogrammal**

1. Nyissa meg a kedvenc szövegszerkesztőjével, majd másolja, és illessze be a parancsfájl megelőző be hello szerkesztő hello.

2. Következő lépésként frissítse hello parancsfájl változók tooreflect a konfigurációs beállításokat. Cserélje le a következő értékeket a megadott hello:

   * **\<storage_account_name\>**  hello a tárfiók nevét.
   * **\<storage_account_key\>**  hello elsődleges vagy másodlagos elérési kulcsot a tárfiók.
   * **\<container_name\>**  egy nevet az új tároló toocreate, például az "azure-cli-minta-container" hello.
   * **\<blob_name\>**  hello cél blob hello tároló nevét.
   * **\<file_to_upload\>**  toosmall-fájl elérési útja a helyi számítógépen, például a hello "~ / images/HelloWorld.png".
   * **\<destination_file\>**  hello a cél elérési utat, például a "~ / downloadedImage.png".

3. Hello szükséges változók frissítése után mentse hello parancsfájlt, és zárja be a szerkesztőt. hello következő lépések azt feltételezik, hogy a parancsfájl nevű **my_storage_sample.sh**.

4. Szükség esetén jelölje meg hello parancsfájl végrehajtható, mint:`chmod +x my_storage_sample.sh`

5. Hello parancsfájlok végrehajtását. Például a Bash:`./my_storage_sample.sh`

Kell kimeneti hasonló toohello következő, és hello  **\<destination_file\>**  hello megadott parancsfájl megjelenjen-e a helyi számítógépen.

```
Creating hello container...
{
  "created": true
}
Uploading hello file...
Percent complete: %100.0
Listing hello blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading hello file...
Name
---------
README.md
Done
```

> [!TIP]
> hello előző kimeneti van **tábla** formátumban. Megadhatja, amely a kimeneti formátum toouse hello megadásával `--output` argumentumának a parancssori felület parancsait, vagy állítsa az segítségével globálisan `az configure`.
>

## <a name="manage-storage-accounts"></a>Tárfiókok kezelése

### <a name="create-a-new-storage-account"></a>Új tárfiók létrehozása
Azure Storage toouse, tároló-fiók szükséges. A számítógép túl beállítása után létrehozhat egy új Azure Storage-fiók[tooyour előfizetés csatlakozás](#connect-to-your-azure-subscription).

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* `--location`[Szükséges]: helyét. Például "USA nyugati régiója".
* `--name`[Szükséges]: hello tárfiók neve. hello neve 3 too24 karakter hosszúságúnak kell, és csak kisbetűs alfanumerikus karaktereket használjon.
* `--resource-group`[Szükséges]: erőforráscsoport nevét.
* `--sku`[Szükséges]: hello tárfiók Termékváltozat. Megengedett értékek:
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a>Környezeti változók értékét az alapértelmezett Azure storage-fiók
Az Azure-előfizetéshez több tárfiókot is lehet. az egyik legyen tooselect toouse minden ezt követő tárolási parancsok, ezek a környezeti változók állíthatja be:

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Egy másik módja tooset alapértelmezett tárfiók kapcsolati karakterlánc használatával van. Először a hello hello kapcsolati karakterlánc beolvasása `show-connection-string` parancs:

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

Majd a Másolás hello kimeneti kapcsolati karakterláncot, és állítsa be a hello `AZURE_STORAGE_CONNECTION_STRING` környezeti változó (szükség lehet tooenclose hello kapcsolati karakterlánc idézőjelben):

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> A következő részekben a cikk hello összes példák feltételezik, hogy beállított hello `AZURE_STORAGE_ACCOUNT` és `AZURE_STORAGE_ACCESS_KEY` környezeti változókat.
>

## <a name="create-and-manage-blobs"></a>Hozzon létre és blobok kezelése
Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatok, például szövegek vagy bináris adatok, hozzáfér a bárhol a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Ez a szakasz feltételezi, hogy Ön már ismeri a Azure Blob storage fogalmakat. Részletes információkért lásd: [az Azure Blob storage .NET használatának első lépései](../blobs/storage-dotnet-how-to-use-blobs.md) és [Blob szolgáltatással kapcsolatos fogalmak](/rest/api/storageservices/blob-service-concepts).

### <a name="create-a-container"></a>Tároló létrehozása
Az Azure storage összes blobjának egy tárolóban kell lennie. Egy tároló hello segítségével létrehozható `az storage container create` parancs:

```azurecli
az storage container create --name <container_name>
```

Beállíthatja egy három szintje olvasási hozzáférés egy új tároló választható hello megadásával `--public-access` argumentum:

* `off`(alapértelmezett): tároló adata titkos toohello fiók tulajdonosának.
* `blob`: Blobok nyilvános olvasási hozzáférés.
* `container`: Nyilvános olvasási és lista hozzáférés toohello teljes tárolóhoz.

További információkért lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../blobs/storage-manage-access-to-resources.md).

### <a name="upload-a-blob-tooa-container"></a>Töltse fel a blobtárolóban tooa
Az Azure Blob storage blokkméretet támogatja, hozzáfűzése, és a lapblobokat. Feltöltés tooa tároló blobok hello segítségével `blob upload` parancs:

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 Alapértelmezés szerint hello `blob upload` parancs tölt *.vhd fájlok toopage blobot, vagy egyéb blokkblobokat. toospecify egy másik típusa egy blob feltöltése, használhatja a hello `--type` argumentum--engedélyezett értékek `append`, `block`, és `page`.

 A hello másik blob típusok további információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).


### <a name="download-a-blob-from-a-container"></a>Blob letöltése tárolóból
Ez a példa bemutatja, hogyan toodownload tárolókból blob:

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok

A tárolóhoz hello hello blobok listázása [az tárolási blob lista](/cli/azure/storage/blob#list) parancsot.

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a>Blobok másolása
Tárfiókokon és régiókon belül vagy azok között aszinkron módon másolhatja át a blobokat.

hello következő példa bemutatja, hogyan toocopy blobok a egy tárolási fiók tooanother. Azt először létre kell hoznia egy tárolót hello forrás tárfiókot, a benne található blobokat nyilvános olvasási hozzáférés megadása. A következő azt feltöltése egy fájl toohello tárolót, és végül másolási hello blob a tárolóban a cél tárfiókkal hello egy tárolóba.

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob toocontainer in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account toodestination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

A fenti példa hello hello céltárolója már léteznie kell hello cél tárfiókkal hello másolási művelet toosucceed. Emellett hello forrás blob hello megadott `--source-uri` argumentumot kell egy közös hozzáférésű jogosultságkód (SAS) jogkivonatot, vagy a nyilvánosan hozzáférhető, ebben a példában látható módon.

### <a name="delete-a-blob"></a>Blob törlése
egy blob toodelete hello használata `blob delete` parancs:

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a>Hozzon létre és fájlmegosztások kezelése
Az Azure File storage közös tárterületet hello Server Message Block (SMB) protokollt használó alkalmazások számára biztosít. A Microsoft Azure virtuális gépek és felhőszolgáltatások, valamint a helyszíni alkalmazások megosztott csatlakoztatott megosztásokon keresztül. A fájlmegosztások és a fájladatok hello Azure CLI segítségével kezelheti. Az Azure File storage további információkért lásd: [Ismerkedés az Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) vagy [hogyan toouse Linux Azure File storage](../storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Fájlmegosztás létrehozása
Egy Azure fájlmegosztás egy SMB-fájlmegosztás, az Azure-ban. Minden könyvtárak és fájlok fájlmegosztást kell létrehozni. Egy fiók korlátlan számú megosztást tartalmazhat, és egy megosztási tárolhatók fájlok mentése toohello kapacitáskorlátait hello tárfiók korlátlan számú. hello alábbi példa létrehoz egy nevű fájlmegosztás **megosztás**.

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a>Könyvtár létrehozása
Egy könyvtárat biztosít az Azure fájlmegosztások hierarchikus struktúra. hello alábbi példa létrehoz egy könyvtárat nevű **könyvtárnév** hello fájlmegosztásban.

```azurecli
az storage directory create --name myDir --share-name myshare
```

Az elérési út például tartalmazhatnak több szintjéről **dir1/dir2**. Azonban úgy kell beállítania, hogy létezik-e minden szülő-könyvtár egy alkönyvtár létrehozása előtt. Például a következő elérési út **dir1/dir2**, először létre kell hoznia directory **dir1**, majd hozza létre a könyvtár **dir2**.

### <a name="upload-a-local-file-tooa-share"></a>Helyi tooa fájlmegosztás feltöltése
hello alábbi példa feltölt egy fájlt a **~/temp/samplefile.txt** a hello tooroot **megosztás** fájlmegosztást. Hello `--source` argumentum hello meglévő helyi fájl tooupload határozza meg.

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

Csakúgy, mint létrehozni a könyvtárat, adja meg hello megosztás tooupload hello fájl tooan meglévő címtárhoz az hello megosztáson belül az elérési út:

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

Hello megosztáson található, a fájl mentése too1 TB méretű lehet.

### <a name="list-hello-files-in-a-share"></a>A megosztási hello fájlok listázása
Fájlok és könyvtárak olyan megosztáson található is listázhatja hello segítségével `az storage file list` parancs:

```azurecli
# List hello files in hello root of a share
az storage file list --share-name myshare --output table

# List hello files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List hello files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a>Fájlok másolása      
Egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl másolhatja. Ha például toocopy különböző megosztásban található fájl tooa könyvtár:        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a>Következő lépések
Az alábbiakban néhány további források további hello Azure CLI 2.0 használatával kapcsolatban.

* [Ismerkedés az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Az Azure CLI 2.0 parancsdokumentációja](/cli/azure)
* [Az Azure CLI 2.0 a Githubon](https://github.com/Azure/azure-cli)
