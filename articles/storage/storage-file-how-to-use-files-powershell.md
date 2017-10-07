---
title: aaaHow toouse PowerShell toomanage Azure File storage |} Microsoft Docs
description: Ismerje meg, hogy toouse PowerShell toomanage Azure File storage.
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
ms.openlocfilehash: 0e30e8796cf8bbf5f9249b26179d5e0f9077c8fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a>Hogyan toouse PowerShell toomanage Azure File storage
Használhatja az Azure PowerShell toocreate és fájlmegosztásokhoz.

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a>Az Azure Storage hello PowerShell-parancsmagjainak telepítése
tooprepare toouse PowerShell, töltse le és telepítse hello Azure PowerShell-parancsmagokat. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) hello telepítse pont és a telepítési utasításokat.

> [!NOTE]
> Javasoljuk, hogy letöltése és telepítése vagy frissítése toohello legújabb Azure PowerShell modul.
> 
> 

Kattintson a **Start** gombra, és írja be a **Windows PowerShell** kifejezést egy Azure PowerShell ablak megnyitásához. hello PowerShell ablakban terhelésének hello Azure Powershell modul.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Környezet létrehozása a tárfiókhoz és a fiókkulcshoz
Hozzon létre hello tárfiók környezetét. hello környezet magában foglalja a hello tárfiók neve és a fiók kulcsot. Útmutatás a fiókkulcs másolását hello [Azure-portálon](https://portal.azure.com), lásd: [megtekintése és másolása tárelérési kulcsok](storage-create-storage-account.md#view-and-copy-storage-access-keys).

Cserélje le `storage-account-name` és `storage-account-key` a tárfiók neve és az alábbi példa hello kulccsal.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Új fájlmegosztás létrehozása
Hozzon létre hello nevű új megosztást `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Így létrejött egy fájlmegosztás a fájltárolóban. A következő lépésben hozzá kell adnia egy könyvtárat és egy fájlt.

> [!IMPORTANT]
> a fájlmegosztás nevét hello összes kisbetűnek kell lennie. A fájlmegosztások és fájlok elnevezésére vonatkozó információkért lásd: [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Megosztások, könyvtárak, fájlok és metaadatok elnevezése és hivatkozása).
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a>Hozzon létre egy könyvtárat hello fájlmegosztásban
Hozzon létre egy könyvtárat hello megosztáson található. A következő példa hello, hello könyvtár neve a `CustomLogs`.

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a>Töltse fel a helyi fájl toohello könyvtár
Töltsön fel egy helyi fájl toohello könyvtárat. hello alábbi példa feltölt egy fájlt a `C:\temp\Log1.txt`. Szerkesztése hello fájl elérési útját, hogy tooa érvényes fájlt a helyi számítógépen.

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a>Hello könyvtárban hello fájlok listázása
toosee hello fájl hello könyvtárban, listázhatja összes hello directory fájlt. A parancs visszaadja hello fájlt és alkönyvtárt (ha vannak ilyenek) hello CustomLogs könyvtárban.

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

A Get-AzureStorageFile parancs bármilyen átadott könyvtárobjektum fájljait és könyvtárait listázza. "Get-AzureStorageFile-Share $s" hello gyökérkönyvtárában fájlok és könyvtárak listáját adja vissza. tooget egy alkönyvtár fájljait a listáját, hogy toopass hello alkönyvtár tooGet-azurestoragefile parancsnak. Ez az a funkciója, – hello hello parancs mentése toohello cső első része egy CustomLogs alkönyvtár hello directory példányát adja vissza. Amelyet aztán átad a Get-azurestoragefile parancsnak, ami hello fájlok és könyvtárak visszaadja a CustomLogs.

## <a name="copy-files"></a>Fájlok másolása
Azure PowerShell 0.9.7-es verziójával kezdve másolhat egy fájl tooanother, egy fájl tooa blob vagy egy blob tooa fájl. Az alábbiakban bemutatjuk, hogyan tooperform ezek másolása műveletek PowerShell-parancsmagok használatával.

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

* [Gyakori kérdések](storage-files-faq.md)
* [hibaelhárítással](storage-troubleshoot-file-connection-problems.md)