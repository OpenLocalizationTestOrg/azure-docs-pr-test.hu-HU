---
title: "Azure gyors üzembe helyezés – Objektumok továbbítása Azure Blob-tárolókra és -tárolókról az Azure CLI-vel | Microsoft Docs"
description: "Gyorsan megismerheti az objektumok az Azure Blob-tárolókra és -tárolókról az Azure CLI-vel való továbbításának módját."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/19/2017
ms.author: tamram
ms.openlocfilehash: 7313df35baadf7aa6d476f44b113dc60e6845f4b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/18/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-the-azure-cli"></a>Objektumok továbbítása Azure Blob-tárolókra és -tárolókról az Azure CLI-vel

Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható. Ez a rövid útmutató részletesen bemutatja, hogyan lehet az Azure CLI használatával adatokat fel- és letölteni az Azure Blob Storage-ba.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a CLI helyi telepítését és használatát választja, akkor ehhez a gyorsútmutatóhoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).

[!INCLUDE [storage-quickstart-tutorial-intro-include-cli](../../../includes/storage-quickstart-tutorial-intro-include-cli.md)]

## <a name="create-a-container"></a>Tároló létrehozása

A blobok minden esetben egy tárolóba lesznek feltöltve. A blobok csoportjait hasonló módon rendszerezheti, mint a fájlokat a számítógép mappáiban.

Hozzon létre blobok tárolására alkalmas tárolót az [az storage container create](/cli/azure/storage/container#create) parancs segítségével.

```azurecli-interactive
az storage container create --name mystoragecontainer
```

## <a name="upload-a-blob"></a>Blob feltöltése

A Blob Storage támogatja a blokkblobokat, a hozzáfűző blobokat és a lapblobokat. A blobtárolókban tárolt fájlok a legtöbb esetben blokkblobként vannak tárolva. A hozzáfűző blobokat akkor használjuk, ha meglévő blobokhoz adatokat szeretnénk hozzáadni a meglévő tartalmak módosítása nélkül (például naplózáshoz). A lapblobok az IaaS virtuális gépek VHD fájljait támogatják.

Először hozza létre a blobba feltölteni kívánt fájlt.
Az Azure Cloud Shell használata esetén a fájl létrehozásához alkalmazza a következőt: `vi helloworld`, amikor a fájl megnyílik, nyomja le az **Insert** billentyűt, írja be a „Hello world” szöveget, majd nyomja le az **Esc** billentyűt, írja be a `:x` parancsot, és nyomja le az **Enter** billentyűt.

Ebben a példában egy blobot töltünk fel a legutóbbi lépésben, az [az storage blob upload](/cli/azure/storage/blob#upload) paranccsal létrehozott tárolóba.

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/path/to/local/file
```

Ha az imént leírt módon hozott létre fájlt az Azure Cloud Shellben, használhatja inkább ezt a CLI-parancsot (itt ugyan nem kellett útvonalat megadni, mivel a fájl az alapkönyvtárban lett létrehozva, normális esetben azonban meg kellene adni az útvonalat):

```azurecli-interactive
az storage blob upload \
    --container-name mystoragecontainer \
    --name helloworld
    --file helloworld
```

Ez az eljárás létrehozza a blobot, ha az még nem létezett, és felülírja, ha már igen. Mielőtt továbblépne, töltsön fel annyi fájlt, amennyit csak szeretne.

Ha egyszerre több fájlt szeretne feltölteni, használhatja az [az storage blob upload-batch](/cli/azure/storage/blob#upload-batch) parancsot.

## <a name="list-the-blobs-in-a-container"></a>A tárolóban lévő blobok listázása

Listázza ki a tárolóban található blobokat az [az storage blob list](/cli/azure/storage/blob#list) paranccsal.

```azurecli-interactive
az storage blob list \
    --container-name mystoragecontainer \
    --output table
```

## <a name="download-a-blob"></a>Blob letöltése

Az [az storage blob download](/cli/azure/storage/blob#download) paranccsal letöltheti a korábban feltöltött blobot.

```azurecli-interactive
az storage blob download \
    --container-name mystoragecontainer \
    --name blobName \
    --file ~/destination/path/for/file
```

## <a name="data-transfer-with-azcopy"></a>Adatátvitel az AzCopy használatával

Az [AzCopy](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) segédprogram az Azure Storage esetében egy további lehetőséget kínál az adatok nagy teljesítményű, parancsfájlalapú átvitelére. Az AzCopy segítségével blob, fájl és tábla típusú tárolókból és tárolókba vihet át adatokat.

Egy példaként íme a *myfile.txt* nevű fájl a *mystoragecontainer* tárolóba való feltöltésére szolgáló AzCopy-parancs.

```bash
azcopy \
    --source /mnt/myfiles \
    --destination https://mystorageaccount.blob.core.windows.net/mystoragecontainer \
    --dest-key <storage-account-access-key> \
    --include "myfile.txt"
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szüksége az erőforráscsoportjában lévő egyik erőforrásra sem (beleértve a jelen rövid útmutatóban létrehozott tárfiókot is), törölje az erőforráscsoportot az [az group delete](/cli/azure/group#delete) paranccsal.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>További lépések

Ennek a rövid útmutatónak a segítségével elsajátította a fájlok a helyi lemez és az Azure Blob Storage valamely tárolója közötti átvitelét. Ha bővebb információra van szüksége a blobok Azure Storage-beli használatával kapcsolatban, lépjen tovább az Azure Blob Storage használatáról szóló oktatóanyagra.

> [!div class="nextstepaction"]
> [Útmutató: Blob Storage-műveletek elvégzése az Azure CLI-vel](storage-how-to-use-blobs-cli.md)
