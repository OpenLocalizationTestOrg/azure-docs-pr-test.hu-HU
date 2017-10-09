---
title: "egy Azure fájlmegosztás aaaHow toocreate |} Microsoft Docs"
description: "Hogyan toocreate egy Azure fájlmegosztás az Azure File storage hello Azure-portálon, a PowerShell és a hello Azure parancssori felület használatával."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 816694e411a993dae881816fc62173e2b7afe990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a>Fájlmegosztás létrehozása az Azure File Storage-ban
Az Azure fájlmegosztások használatával hozhat létre [Azure-portálon](https://portal.azure.com/), hello Azure Storage PowerShell parancsmagjainak, hello Azure Storage ügyfélkódtáraival vagy hello Azure Storage REST API-t. Ebben az oktatóanyagban témák köre:
* [Hogyan toocreate egy Azure-fájl megosztása hello Azure-portál használatával](#Create file share through hello Portal)
* [Hogyan toocreate egy Azure fájlmegosztás Powershell használatával](#Create file share using PowerShell)
* [Hogyan toocreate egy Azure fájlmegosztás parancssori felület használatával](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Előfeltételek
egy Azure fájlmegosztás toocreate, használhatja a storage-fiók, amely már létezik, vagy [hozzon létre egy új Azure-tárfiók](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). egy Azure fájlmegosztás PowerShell toocreate, szüksége lesz hello fiókkulcs és a tárfiók nevét... Ha azt tervezi, hogy a Powershell vagy a CLI toouse kell Tárfiók kulcsa.

## <a name="create-file-share-through-hello-portal"></a>Hello portálon keresztül fájlmegosztás létrehozása
1. **Azure-portál lépjen tooStorage fiók paneljét**:    
    ![Tárfiók panel](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Kattintson a Fájlmegosztás hozzáadása gombra**:    
    ![Kattintson a hello fájl megosztás gomb hozzáadása](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Adja meg a nevet és a kvótát. A kvóta jelenleg legfeljebb 5 TB lehet**:    
    ![Adjon meg egy nevet és a kívánt kvóta hello Új fájlmegosztás](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Tekintse meg az új fájlmegosztást**: ![Az új fájlmegosztás megtekintése](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Töltsön fel egy fájlt**: ![Fájl feltöltése](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Tallózzon az új fájlmegosztásban, és kezelje könyvtárait és fájljait**: ![Fájlmegosztás tallózása](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Fájlmegosztás létrehozása PowerShell-lel
tooprepare toouse PowerShell, töltse le és telepítse hello Azure PowerShell-parancsmagokat. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) hello telepítse pont és a telepítési utasításokat.

> [!Note]  
> Javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul.

1. **A tárfiók és a kulcs környezet létrehozása** hello környezet magában foglalja a hello tárfiók neve és a fiók kulcsot. Útmutatás a fiókkulcs átmásolásához az [Azure Portalról](https://portal.azure.com/): [A tárelérési kulcs megtekintése és másolása](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Hozzon létre egy új fájlmegosztást**:    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> a fájlmegosztás nevét hello összes kisbetűnek kell lennie. A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).

## <a name="create-file-share-through-command-line-interface-cli"></a>Fájlmegosztás létrehozása parancssori felület (CLI) használatával
1. **tooprepare toouse parancssori felület (CLI) letöltése, és hello Azure parancssori felület telepítése.**  
    Lásd: [Az Azure CLI 2.0-s verziójának telepítése](/cli/azure/install-az-cli2.md) és [Bevezetés az Azure CLI 2.0-s verziójának használatába](/cli/azure/get-started-with-azure-cli.md).

2. **Hozzon létre egy kapcsolati karakterlánc toohello tárfiókot toocreate hello megosztást, ahová.**  
    Cserélje le ```<storage-account>``` és ```<resource_group>``` az a fiók nevét és az erőforrás tárolócsoportot az alábbi példa hello.

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. **Fájlmegosztás létrehozása**
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a>Következő lépések
* [Fájlmegosztás csatlakoztatása – Windows](storage-how-to-use-files-windows.md)
* [Fájlmegosztás csatlakoztatása – Linux](../storage-how-to-use-files-linux.md)
* [Fájlmegosztás csatlakoztatása – macOS](storage-how-to-use-files-mac.md)

Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

* [Gyakori kérdések](../storage-files-faq.md)
* [Hibaelhárítás a Windows rendszerben](storage-troubleshoot-windows-file-connection-problems.md)      
* [Hibaelhárítás a Linux rendszerben](storage-troubleshoot-linux-file-connection-problems.md)   