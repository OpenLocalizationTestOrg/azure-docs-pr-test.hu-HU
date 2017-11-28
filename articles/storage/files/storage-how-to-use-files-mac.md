---
title: "aaaMount Azure fájlmegosztás SMB-n keresztül macOS |} Microsoft Docs"
description: "Ismerje meg, hogyan toomount egy Azure-fájl megosztása SMB-n keresztül macOS."
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
ms.openlocfilehash: 71aaec8a77b770fe147b783c0ab9f86830176bec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a><span data-ttu-id="1d6b9-103">Azure-fájlmegosztás csatlakoztatása SMB protokoll segítségével macOS rendszeren</span><span class="sxs-lookup"><span data-stu-id="1d6b9-103">Mount Azure File share over SMB with macOS</span></span>
<span data-ttu-id="1d6b9-104">[Az Azure File storage](../storage-dotnet-how-to-use-files.md) hello iparági szabvány használja a Microsoft-szolgáltatás, amely lehetővé teszi a hello Azure toocreate és -felhasználási hálózati fájlmegosztások.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-104">[Azure File storage](../storage-dotnet-how-to-use-files.md) is Microsoft's service that enables you toocreate and use network file shares in hello Azure using hello industry standard.</span></span> <span data-ttu-id="1d6b9-105">Az Azure-fájlmegosztások a macOS rendszer Sierra (10.12) és El Capitan (10.11) verzióra csatlakoztathatók.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-105">Azure File shares can be mounted in macOS Sierra (10.12) and El Capitan (10.11).</span></span> <span data-ttu-id="1d6b9-106">Ez a cikk mutatja be két különböző módon toomount egy Azure fájlmegosztás macOS hello kereső felhasználói felület és a Terminálszolgáltatások hello használatával.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-106">This article shows two different ways toomount an Azure File share on macOS with hello Finder UI and using hello Terminal.</span></span>

> [!Note]  
> <span data-ttu-id="1d6b9-107">Az Azure-fájlmegosztás SMB protokollon keresztül történő csatlakoztatása előtt javasoljuk, hogy tiltsa le az SMB-csomagaláírást.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-107">Before mounting an Azure File share over SMB, we recommend disabling SMB packet signing.</span></span> <span data-ttu-id="1d6b9-108">Nem így előfordulhat, hogy yield gyenge teljesítményt, amikor macOS hello Azure fájlmegosztás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-108">Not doing so may yield poor performance when accessing hello Azure File share from macOS.</span></span> <span data-ttu-id="1d6b9-109">Az SMB-kapcsolatot lesz titkosítva, így ez nincs hatással a hello biztonsági a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-109">Your SMB connection will be encrypted, so this does not affect hello security of your connection.</span></span> <span data-ttu-id="1d6b9-110">A Terminálszolgáltatások hello hello következő parancsok letiltja a SMB-csomagokat, ez szerint [Apple támogatási cikk a letiltása az SMB-csomagokat](https://support.apple.com/HT205926):</span><span class="sxs-lookup"><span data-stu-id="1d6b9-110">From hello terminal, hello following commands will disable SMB packet signing, as described by this [Apple support article on disabling SMB packet signing](https://support.apple.com/HT205926):</span></span>  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a><span data-ttu-id="1d6b9-111">Az Azure-fájlmegosztások macOS rendszerre történő csatlakoztatásának előfeltételei</span><span class="sxs-lookup"><span data-stu-id="1d6b9-111">Prerequisites for mounting an Azure File share on macOS</span></span>
* <span data-ttu-id="1d6b9-112">**A tárfiók neve**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello hello tárfiókja nevére.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-112">**Storage account name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="1d6b9-113">**Tárfiók kulcsa**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello elsődleges (vagy másodlagos) storage-kulcs.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-113">**Storage account key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="1d6b9-114">Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-114">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="1d6b9-115">**Győződjön meg arról, hogy a 445-ös port nyitva legyen**: Az SMB protokoll a 445-ös TCP porton keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-115">**Ensure port 445 is open**: SMB communicates over TCP port 445.</span></span> <span data-ttu-id="1d6b9-116">Az ügyfélszámítógép (hello Mac) ellenőrizze, hogy a tűzfal nem blokkolja a 445-ös TCP-port toomake.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-116">On your client machine (hello Mac), check toomake sure your firewall is not blocking TCP port 445.</span></span>

## <a name="mount-an-azure-file-share-via-finder"></a><span data-ttu-id="1d6b9-117">Azure-fájlmegosztás csatlakoztatása a Finder segítségével</span><span class="sxs-lookup"><span data-stu-id="1d6b9-117">Mount an Azure File share via Finder</span></span>
1. <span data-ttu-id="1d6b9-118">**Nyissa meg a keresési**: kereső macOS alapértelmezés szerint nyitva, de gondoskodhat hello jelenleg kiválasztott alkalmazás hello "macOS szembesülhetnek ikon" gombra kattintva hello dock:</span><span class="sxs-lookup"><span data-stu-id="1d6b9-118">**Open Finder**: Finder is open on macOS by default, but you can ensure it is hello currently selected application by clicking hello "macOS face icon" on hello dock:</span></span>  
    <span data-ttu-id="1d6b9-119">![hello macOS szembesülhetnek ikon](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span><span class="sxs-lookup"><span data-stu-id="1d6b9-119">![hello macOS face icon](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)</span></span>

2. <span data-ttu-id="1d6b9-120">**Válassza ki a "Csatlakozás tooServer" hello "Ugrás" menü a**: hello hello UNC-útvonalat használ [Előfeltételek](#preq), átalakítása hello kezdete dupla fordított perjel (`\\`) túl`smb://` és egyéb fordított (`\`) tooforwards perjeleket (`/`).</span><span class="sxs-lookup"><span data-stu-id="1d6b9-120">**Select "Connect tooServer" from hello "Go" Menu**: Using hello UNC path from hello [prerequisites](#preq), convert hello beginning double backslash (`\\`) too`smb://` and all other backslashes (`\`) tooforwards slashes (`/`).</span></span> <span data-ttu-id="1d6b9-121">A hivatkozás hello hasonlóan kell kinéznie: ![hello "Csatlakozás tooServer" párbeszédpanel](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span><span class="sxs-lookup"><span data-stu-id="1d6b9-121">Your link should look like hello following: ![hello "Connect tooServer" dialog](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)</span></span>

3. <span data-ttu-id="1d6b9-122">**Hello megosztás nevét és a tárolási fiók kulcs használata a felhasználónevet és jelszót kérő**: kattintson a "Csatlakozás" hello "Csatlakozás tooServer" párbeszédpanelen, amikor kérni fogja hello felhasználóneve és jelszava (Ez lesz a macOS rendelkező autopopulated username).</span><span class="sxs-lookup"><span data-stu-id="1d6b9-122">**Use hello share name and storage account key when prompted for a username and password**: When you click "Connect" on hello "Connect tooServer" dialog, you will be prompted for hello username and password (This will be autopopulated with your macOS username).</span></span> <span data-ttu-id="1d6b9-123">Lehetősége van hello a helyezi el a macOS kulcslánc hello megosztás neve/tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-123">You have hello option of placing hello share name/storage account key in your macOS Keychain.</span></span>

4. <span data-ttu-id="1d6b9-124">**Használjon hello Azure fájlmegosztás tetszés szerint**: után és hello megosztás nevét és a tárolási fiók kulcsot a hello felhasználóneve és jelszava, hello megosztást is csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-124">**Use hello Azure File share as desired**: After substituting hello share name and storage account key in for hello username and password, hello share will be mounted.</span></span> <span data-ttu-id="1d6b9-125">Használhat ez történik, mint általában egy helyi fájl/mappa megosztáshoz, többek között a húzása hello fájlmegosztás be:</span><span class="sxs-lookup"><span data-stu-id="1d6b9-125">You may use this as you would normally use a local folder/file share, including dragging and dropping files into hello file share:</span></span>

    ![Pillanatfelvétel egy csatlakoztatott Azure-fájlmegosztásról](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a><span data-ttu-id="1d6b9-127">Azure-fájlmegosztás csatlakoztatása a Terminál segítségével</span><span class="sxs-lookup"><span data-stu-id="1d6b9-127">Mount an Azure File share via Terminal</span></span>
1. <span data-ttu-id="1d6b9-128">Cserélje le `<storage-account-name>` hello a tárfiók nevére.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-128">Replace `<storage-account-name>` with hello name of your storage account.</span></span> <span data-ttu-id="1d6b9-129">Adja meg a tárfiók kulcsát, amikor a rendszer felkéri erre.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-129">Provide Storage Account Key as password when prompted.</span></span> 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. <span data-ttu-id="1d6b9-130">**Használjon hello Azure fájlmegosztás tetszés szerint**: hello Azure fájlmegosztás hello előző parancs által meghatározott hello csatlakozási ponton is csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-130">**Use hello Azure File share as desired**: hello Azure File share will be mounted at hello mount point specified by hello previous command.</span></span>  

    ![Pillanatkép készítése a csatlakoztatott hello Azure fájlmegosztás](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a><span data-ttu-id="1d6b9-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d6b9-132">Next steps</span></span>
<span data-ttu-id="1d6b9-133">Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.</span><span class="sxs-lookup"><span data-stu-id="1d6b9-133">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="1d6b9-134">Apple-támogatási cikk - hogyan tooconnect fájlmegosztás a Mac gépen</span><span class="sxs-lookup"><span data-stu-id="1d6b9-134">Apple Support Article - How tooconnect with File Sharing on your Mac</span></span>](https://support.apple.com/HT204445)
* [<span data-ttu-id="1d6b9-135">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="1d6b9-135">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="1d6b9-136">Hibaelhárítás a Windows rendszerben</span><span class="sxs-lookup"><span data-stu-id="1d6b9-136">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="1d6b9-137">Hibaelhárítás a Linux rendszerben</span><span class="sxs-lookup"><span data-stu-id="1d6b9-137">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)    