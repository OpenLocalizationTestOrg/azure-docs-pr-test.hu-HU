---
title: "az Azure Blob Storage használata az AzCopy aaaMove adatok tooand |} Microsoft Docs"
description: "Adatok tooand áthelyezése az Azure Blob Storage AzCopy használatával"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: b381ba004ac16879b6c633950d03d13ad82d5b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azcopy"></a>Adatok tooand áthelyezése az Azure Blob Storage AzCopy használatával
AzCopy parancssori segédprogram feltöltése, letöltése és a table storage, a Microsoft Azure blob és a fájl másolása adatok tooand készült.

AzCopy és további információkat a használja azt a hello Azure platformon telepítésével kapcsolatos útmutatásért lásd: [Ismerkedés az AzCopy parancssori segédprogram hello](../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Ha virtuális Gépet, amely hello parancsfájlok által biztosított be lett állítva az [adatok tudományos virtuális gépek Azure-ban](machine-learning-data-science-virtual-machines.md), akkor az AzCopy már telepítve van a virtuális gép hello.
> 
> [!NOTE]
> A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Jelen dokumentum céljából feltételezzük, hogy rendelkezik Azure-előfizetéssel, a tárfiók, és megfelelő kulcs fiók hello. Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.

* tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>AzCopy parancsok futtatása
toorun AzCopy parancsok, nyisson meg egy parancsablakot, és keresse meg a toohello AzCopy telepítési könyvtárára a számítógépen, ahol hello AzCopy.exe végrehajtható fájl is található. 

hello alapvető AzCopy parancs szintaxisa a következő:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> Hello AzCopy telepítési tooyour rendszer elérési útjának hozzáadása, és futtassa a hello parancsokat bármelyik könyvtárból. Alapértelmezés szerint AzCopy túl van telepítve*% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* vagy *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-tooan-azure-blob"></a>Fájlok tooan Azure blob feltöltése
tooupload egy fájl a következő parancs hello használata:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Egy Azure-blobot a fájlok letöltése
egy fájl futtatását egy Azure blob, a következő parancs használata hello toodownload:

    # Downloading blobs toolocal file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Azure tárolók közötti átviteléhez a BLOB
Azure tárolók közötti tootransfer blobok hello a következő parancsot használja:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: hello sub directory in hello container
    <your_local_directory>: directory of local file system where files toobe uploaded from or hello directory of local file system files toobe downloaded to
    <file_pattern>: pattern of file names toobe transferred. hello standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tippek az AzCopy segítségével
> [!TIP]
> 1. Ha **feltöltése** fájlok, */S* fájlok rekurzív módon feltölt. Ez a paraméter nélkül nem feltöltése fájlok alkönyvtáraiban találhatóak.  
> 2. Ha **letöltése** fájl, */S* keresések tároló rekurzív módon hello amíg hello megadott könyvtárban és annak alkönyvtáraiban található összes fájl, vagy a megfelelő fájlok hello megadott hello a megadott mintának könyvtár, illetve annak alkönyvtáraiba, letöltődnek.  
> 3. Nem adható meg egy **adott blob fájl** hello segítségével toodownload */Source* paraméter. toodownload egy adott fájlt, adja meg a hello blob fájl neve toodownload hello segítségével */mintát* paraméter. **/S** paraméter lehet használt toohave AzCopy keresse meg a fájl neve mintát rekurzív módon. Hello mintát paraméter nélkül AzCopy letölti az összes fájlt, hogy a címtárban.
> 
> 

