---
title: "az Azure Blob Storage használatával Python aaaMove adatok tooand |} Microsoft Docs"
description: "Adatok tooand áthelyezése az Azure Blob Storage pythonos környezetekben"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: c2be9600e0d6cb05bcf4109a8d554db522704ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-python"></a>Adatok tooand áthelyezése az Azure Blob Storage pythonos környezetekben
Ez a témakör ismerteti, hogyan toolist, töltse fel, és töltse le a blobok hello Python API használatával. Az Azure SDK-ban megadott Python API hello a következőket teheti:

* Tároló létrehozása
* Blobok feltöltése a tárolóba
* Blobok letöltése
* Lista hello a tárolóban lévő blobok
* Blob törlése

Hello Python API használatával kapcsolatos további információkért lásd: [hogyan tooUse hello Blob Storage szolgáltatással való Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Ha virtuális Gépet, amely hello parancsfájlok által biztosított be lett állítva az [adatok tudományos virtuális gépek Azure-ban](machine-learning-data-science-virtual-machines.md), akkor az AzCopy már telepítve van a virtuális gép hello.
> 
> [!NOTE]
> A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és hello megfelelő kulcs fiók rendelkezik. Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.

* tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).

## <a name="upload-data-tooblob"></a>Adatok tooBlob feltöltése
Adja hozzá a következő kódrészletet, amelyben tooprogrammatically access Azure Storage kívánja Python kódok hello tetején hello:

    from azure.storage.blob import BlobService

Hello **BlobService** objektum lehetővé teszi, hogy a tárolók és blobok. a következő kód hello objektumot hoz létre BlobService hello tárfiók neve és a fiók kulcsot használ. Fiók neve és a fiókkulcsot cserélje le a valódi fiókot és kulcsot.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

A következő módszerek tooupload adatblobja tooa hello használata:

1. PUT\_blokk\_blob\_a\_(feltölt egy fájl megadott elérési hello hello tartalmát) elérési útja
2. PUT\_block_blob\_a\_(feltölt egy már megnyitott fájl vagy adatfolyam tartalmát hello) fájlt
3. PUT\_blokk\_blob\_a\_bájt (feltöltések egy bájttömb)
4. PUT\_blokk\_blob\_a\_szöveg (megadott hello feltölt hello használatával szöveges értéket a megadott kódolás)

a következő példakód hello feltölt egy helyi fájl tooa tárolót:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

hello alábbi mintakód feltölt (kivéve a könyvtárak) minden hello fájlok helyi könyvtár tooblob tárolási:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in hello LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading hello data %s"%blob_name


## <a name="download-data-from-blob"></a>A Blob adatok letöltése
Módszerek toodownload adatokat a blob a következő hello használata:

1. első\_blob\_való\_elérési útja
2. első\_blob\_való\_fájl
3. első\_blob\_való\_bájt
4. első\_blob\_való\_szöveg

Ezek a módszerek végző hello szükséges adattömbösítő Ha hello hello adatok mérete túllépi a 64 MB.

hello alábbi mintakód letölti a blob tároló tooa helyi fájlba hello tartalmát:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

hello alábbi mintakód letölti összes BLOB egy tárolót. Lista használ\_blobok érhető el, hello tárolóban lévő blobok tooget hello listáját, és letölti azokat tooa helyi könyvtárba.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading hello data %s"%blob.name
