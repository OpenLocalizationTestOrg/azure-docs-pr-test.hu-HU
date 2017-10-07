---
title: "egy Azure fájlmegosztás aaaMount és a hozzáférés hello ugyanazt a Windows |} Microsoft Docs"
description: "Csatlakoztassa egy Azure fájlmegosztás és a Windows hello megosztásra hozzáférést."
services: storage
documentationcenter: na
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
ms.openlocfilehash: 15ac468d9d7b8e0a195b024926ed4dd9790360d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-an-azure-file-share-and-access-hello-share-in-windows"></a>Egy Azure fájlmegosztás és a Windows hello megosztásra hozzáférés csatlakoztatása
[Az Azure File storage](storage-dotnet-how-to-use-files.md) a Microsoft easy toouse felhő fájlrendszer. Az Azure-fájlmegosztások Windows és Windows Server rendszeren csatlakoztathatók. Ez a cikk bemutatja három különböző módon toomount egy Azure fájlmegosztás Windows rendszeren: a fájl Explorer felhasználói felületén, és PowerShell keresztül hello parancssor hello. 

Rendelés toomount egy Azure fájlmegosztás hello kívül az Azure-régió, például a helyszínen vagy a különböző Azure-régiót helyezkedik el, a hello az operációs rendszer támogatniuk kell az SMB 3.0-s. 

Az Azure-fájlmegosztás az operációs rendszer verziójától függően egy helyszíni vagy Azure-beli virtuális gépen lévő Windows-gépen csatlakoztatható. Alábbi táblázat bemutatja a hello 

| Windows-verzió        | SMB-verzió |Azure-beli virtuális gépen csatlakoztatható|Helyszínen csatlakoztatható|
|------------------------|-------------|---------------------|---------------------|
| Windows 7              | SMB 2.1     | Igen                 | Nem                  |
| Windows Server 2008 R2 | SMB 2.1     | Igen                 | Nem                  |
| Windows 8              | SMB 3.0     | Igen                 | Igen                 |
| Windows Server 2012    | SMB 3.0     | Igen                 | Igen                 |
| Windows Server 2012 R2 | SMB 3.0     | Igen                 | Igen                 |
| Windows 10             | SMB 3.0     | Igen                 | Igen                 |

> [!Note]  
> Véve mindig ajánlott legutóbbi KB-os verzióhoz tartozó Windows hello.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Az Azure-fájlmegosztások Windowson történő csatlakoztatásának előfeltételei 
* **A tárfiók neve**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello hello tárfiókja nevére.

* **Tárfiók kulcsa**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello elsődleges (vagy másodlagos) storage-kulcs. Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.

* **Győződjön meg arról, hogy a 445-ös port nyitva van**: Az Azure File Storage SMB protokollt használ. Toosee SMB 445 - TCP-porton keresztül kommunikál ellenőrizze, hogy a tűzfal nem blokkolja-e az ügyfélszámítógép 445-ös TCP-portok.

## <a name="mount-hello-azure-file-share-with-file-explorer"></a>Hello Azure fájlmegosztás Fájlkezelőben csatlakoztatása
> [!Note]  
> Vegye figyelembe, hogy az utasításoknak hello látható a Windows 10 és némileg eltérőek lehetnek a régebbi kiadásokban. 

1. **Nyissa meg a Fájlkezelőt**: Ez végezhető megnyitása a Start menü hello, vagy Win + E helyi lenyomásával.

2. **Keresse meg a hello ablak hello bal oldalon toohello "A számítógép" elemre. Ez a művelet módosítja az hello menük hello szalagon érhető el. Az hello számítógép menüben válassza a "Hálózati meghajtó csatlakoztatása"**.
    
    ![A képernyőfelvétel a hello "Hálózati meghajtó csatlakoztatása" legördülő menü](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Másolás hello UNC elérési út ablaktáblájáról hello "Csatlakozás" hello Azure-portálon**: hogyan toofind ezt az információt található részletes leírását [Itt](storage-file-how-to-use-files-portal.md#connect-to-file-share).

    ![hello UNC elérési út hello Azure File storage Connect panelről](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Válasszon hello meghajtóbetűjelet, és írja be a hello UNC elérési utat.** 
    
    ![Egy hello "Hálózati meghajtó csatlakoztatása" párbeszédpanel képernyőképe](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **$A a Tárfiók nevét használja hello `Azure\` hello felhasználónév és a Tárfiók kulcsa hello jelszóként.**
    
    ![Egy hello hálózati hitelesítő párbeszédpanel képernyőképe](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Használja az Azure-fájlmegosztást igény szerint**.
    
    ![Az Azure-fájlmegosztás most már csatlakoztatva van](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Készen áll a toodismount (vagy leválasztása) hello Azure fájlmegosztás, ehhez hello bejegyzésre hello megosztás hello "hálózati helyek" a Fájlkezelőben a jobb gombbal kattint rá, majd válassza a "Kapcsolat bontása"**.

## <a name="mount-hello-azure-file-share-with-powershell"></a>Csatlakoztassa a hello Azure fájlmegosztás a PowerShell használatával
1. **Használjon hello következő parancsot a toomount hello Azure fájlmegosztás**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello megfelelő információkkal.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Használjon hello Azure fájlmegosztás tetszés szerint**.

3. **Ha elkészült, válassza le a hello Azure fájlmegosztás használata a következő parancs hello**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> Hello segítségével `-Persist` paraméter `New-PSDrive` toomake hello Azure File megosztás látható toohello részeinek hello operációs rendszer, amíg csatlakoztatva.

## <a name="mount-hello-azure-file-share-with-command-prompt"></a>Csatlakoztassa a hello Azure fájlmegosztás parancssorral
1. **Használjon hello következő parancsot a toomount hello Azure fájlmegosztás**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` hello megfelelő információkkal.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Használjon hello Azure fájlmegosztás tetszés szerint**.

3. **Ha elkészült, válassza le a hello Azure fájlmegosztás használata a következő parancs hello**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Konfigurálhatja hello Azure File megosztás tooautomatically újracsatlakozás újraindításkor Windows tárolásakor hello hitelesítő adatait. a következő parancs hello megmaradnak hello hitelesítő adatait:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.

* [Gyakori kérdések](storage-files-faq.md)
* [hibaelhárítással](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Elméleti cikkek és videók
* [Azure File Storage: zökkenőmentes felhőalapú SMB fájlrendszer Windows és Linux rendszerekhez](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Hogyan toouse Azure File storage Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Azure File Storage-eszköztámogatás
* [Using Azure PowerShell with Azure Storage (Az Azure PowerShell és az Azure Storage együttes használata)](storage-powershell-guide-full.md)
* [Hogyan toouse Microsoft Azure Storage AzCopy](storage-use-azcopy.md)
* [Az Azure Storage hello Azure parancssori felület használatával](storage-azure-cli.md#create-and-manage-file-shares)
* [Az Azure File Storage-problémák hibaelhárítása](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Blogbejegyzések
* [Azure File storage is now generally available (Mostantól általánosan elérhető az Azure File Storage)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Az Azure File Storage ismertetése](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (A Microsoft Azure File szolgáltatás bemutatása)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Áttelepítési adatok tooAzure fájl](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referencia
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)
