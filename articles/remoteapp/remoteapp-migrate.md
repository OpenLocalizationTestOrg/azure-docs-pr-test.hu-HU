---
title: "aaaMigrate felhasználói adatokat az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan toomigrate mindkét Azure RemoteApp felhasználói adatokat."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: d7e4fbf1-cb42-4430-94a0-ed6d4676fc86
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aefc6ccc2c6173754acf6cad06102f27c8cb1d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomigrate-data-into-and-out-of-azure-remoteapp"></a><span data-ttu-id="42266-103">Hogyan toomigrate adatok esetében bejövő és kimenő Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="42266-103">How toomigrate data into and out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="42266-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="42266-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="42266-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="42266-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="42266-106">Számos különböző eszközöket és módszerek tootransfer [felhasználói adatok](remoteapp-upd.md) esetében bejövő és kimenő Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="42266-106">You can use many different tools and methods tootransfer [user data](remoteapp-upd.md) into and out of Azure RemoteApp.</span></span> <span data-ttu-id="42266-107">Az alábbiakban néhány módszert:</span><span class="sxs-lookup"><span data-stu-id="42266-107">Here are a few methods:</span></span>

* <span data-ttu-id="42266-108">Másolja és illessze be a használatával a vágólap megosztása</span><span class="sxs-lookup"><span data-stu-id="42266-108">Copy and paste using clipboard sharing</span></span>
* <span data-ttu-id="42266-109">Másolja a fájlokat és adatokat tooa fájlkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="42266-109">Copy files and data tooa file server</span></span>
* <span data-ttu-id="42266-110">Másolja a fájlokat tooOneDrive vállalati böngészőn keresztül</span><span class="sxs-lookup"><span data-stu-id="42266-110">Copy files tooOneDrive for Business through a browser</span></span>
* <span data-ttu-id="42266-111">Másolja a fájlokat a átirányítása</span><span class="sxs-lookup"><span data-stu-id="42266-111">Copy files using redirection</span></span>

> [!NOTE]
> <span data-ttu-id="42266-112">Nem engedélyezte a onedrive vállalati verzió hello üzleti vagy fogyasztói sync-ügynök - azok [használata nem támogatott](remoteapp-onedrive.md) az Azure Remoteappban.</span><span class="sxs-lookup"><span data-stu-id="42266-112">You cannot enable hello OneDrive for Business or Consumer sync agents - they [are not supported](remoteapp-onedrive.md) in Azure RemoteApp.</span></span>
> 
> 

## <a name="use-copy-and-paste-in-file-explorer"></a><span data-ttu-id="42266-113">Másolás és beillesztés a Fájlkezelőben</span><span class="sxs-lookup"><span data-stu-id="42266-113">Use copy and paste in File Explorer</span></span>
<span data-ttu-id="42266-114">A RemoteApp-telepítések engedélyezve van a másolás és beillesztés hello vágólap segítségével [alapértelmezés szerint](remoteapp-redirection.md).</span><span class="sxs-lookup"><span data-stu-id="42266-114">Copy and paste using hello clipboard is enabled in RemoteApp deployments [by default](remoteapp-redirection.md).</span></span> <span data-ttu-id="42266-115">Ez lehetővé teszi, hogy a felhasználók a helyi számítógép és a RemoteApp-alkalmazások közti fájlokat másolni.</span><span class="sxs-lookup"><span data-stu-id="42266-115">This lets users copy files between their local PC and RemoteApp apps.</span></span> <span data-ttu-id="42266-116">Gyakran hello normális működés során a RemoteApp-alkalmazások használata, a felhasználók mentett fájlokat tootheir felhasználóiprofil - áthelyezését, hogy könnyen-e RemoteApp adatokat:</span><span class="sxs-lookup"><span data-stu-id="42266-116">Often, through hello normal course of using apps in RemoteApp, users have saved files tootheir UPDs - moving that data out of RemoteApp is easy:</span></span>

1. <span data-ttu-id="42266-117">[A Fájlkezelőben egy alkalmazás közzététele](remoteapp-publish.md) egy RemoteApp-gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="42266-117">[Publish File Explorer as an app](remoteapp-publish.md) in a RemoteApp collection.</span></span> <span data-ttu-id="42266-118">(Vegye figyelembe, hogy ez a felügyeleti feladatot.)</span><span class="sxs-lookup"><span data-stu-id="42266-118">(Note that this is an administrative task.)</span></span>
2. <span data-ttu-id="42266-119">Közvetlen a felhasználók toolaunch hello fájlkezelő alkalmazást közzétett és toouse adott toocopy és a Beillesztés fájlok azok UPD be- és belőle.</span><span class="sxs-lookup"><span data-stu-id="42266-119">Direct your users toolaunch hello File Explorer app you published and toouse that toocopy and paste files both into their UPD and out of it.</span></span>

## <a name="upload-files-and-data-tooa-file-server-by-using-standard-network-file-copy"></a><span data-ttu-id="42266-120">Töltse fel a fájlokat és adatokat tooa fájlkiszolgáló szabványos hálózati fájlmásolat segítségével</span><span class="sxs-lookup"><span data-stu-id="42266-120">Upload files and data tooa file server by using standard network file copy</span></span>
<span data-ttu-id="42266-121">Gyakran szervezetek fájl kiszolgálók toostore általános adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="42266-121">Often organizations use file servers toostore general data.</span></span> <span data-ttu-id="42266-122">Ha tudja, hello kiszolgáló nevét vagy helyét, a felhasználók navigálhat hello helyi hálózati hello kiszolgáló és a fájlokat, majd másolja, sokkal megszokott módon fent.</span><span class="sxs-lookup"><span data-stu-id="42266-122">If you know hello server name or location, your users can browse hello local network for hello server and then copy their files there, much like they did above.</span></span> <span data-ttu-id="42266-123">Újra lesz toopublish Fájlkezelőben tooRemoteApp szeretné és megosztása a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="42266-123">Again you'll want toopublish File Explorer tooRemoteApp and then share it with your users.</span></span>

> [!NOTE]
> <span data-ttu-id="42266-124">hello fájlkiszolgáló RemoteApp a telepített hello az irányítható hálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="42266-124">hello file server must be on hello routable network that RemoteApp was deployed into.</span></span>
> 
> 

## <a name="copy-files-tooonedrive-for-business"></a><span data-ttu-id="42266-125">A vállalati fájlok tooOneDrive másolása</span><span class="sxs-lookup"><span data-stu-id="42266-125">Copy files tooOneDrive for Business</span></span>
<span data-ttu-id="42266-126">Bár hello OneDrive vállalati szinkronizálási ügynök a RemoteApp nem engedélyezi, akkor továbbra is fájljait átmásolhatja a UPD tooOneDrive vállalati böngészőn keresztül.</span><span class="sxs-lookup"><span data-stu-id="42266-126">Although you cannot enable hello OneDrive for Business sync agent in RemoteApp, you can still copy files from your UPD tooOneDrive for Business through a browser.</span></span> 

1. <span data-ttu-id="42266-127">A Fájlkezelőben tooRemoteApp közzététele, és majd kérje meg a felhasználók tooaccess hello fájlok számára, hogy alkalmazáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="42266-127">Publish File Explorer tooRemoteApp and then tell users tooaccess hello files through that app.</span></span> 
2. <span data-ttu-id="42266-128">Legegyszerűbb tootransfer fájlokat, ha azok tömörített, így a felhasználók hozzon létre egy .zip fájlt, amely tartalmazza az összes hello fájlok toomove tooOneDrive vállalati.</span><span class="sxs-lookup"><span data-stu-id="42266-128">It's easiest tootransfer files if they are compressed, so users should create a .zip file that contains all of hello files toomove tooOneDrive for Business.</span></span>
3. <span data-ttu-id="42266-129">Kérje meg a felhasználók toogo toohello Office 365 portálra, és folytassa a tooOneDrive és hello .zip fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="42266-129">Ask users toogo toohello Office 365 portal, and then go tooOneDrive and upload hello .zip file.</span></span>

## <a name="copy-files-by-using-drive-redirection"></a><span data-ttu-id="42266-130">Másolja a fájlokat átirányítás használatával</span><span class="sxs-lookup"><span data-stu-id="42266-130">Copy files by using drive redirection</span></span>
<span data-ttu-id="42266-131">Ha engedélyezte a [meghajtó átirányítása](remoteapp-redirection.md), már létrehozott egy leképzett meghajtóról a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="42266-131">If you have enabled [drive redirection](remoteapp-redirection.md), you have already created a mapped drive for your users.</span></span> <span data-ttu-id="42266-132">Ebben az esetben azok a zip-fájljaikat a átirányítva hello meghajtó és mentse őket tootheir helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="42266-132">In this case, they can zip their files on hello redirected drive and then save them tootheir local PC.</span></span>

## <a name="how-administrators-can-export-data"></a><span data-ttu-id="42266-133">Hogyan rendszergazdák adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="42266-133">How administrators can export data</span></span>

<span data-ttu-id="42266-134">Az Azure RemoteApp exportálhatja az összes felhasználói profil lemezeket (UPD) felügyeli a összes gyűjteményt belül egy előfizetés tooAzure tárolást biztosít az Azure PowerShell-parancsmag Export-AzureRemoteAppUserDisk.</span><span class="sxs-lookup"><span data-stu-id="42266-134">Administers for Azure RemoteApp can export all user profile disks (UPD) for all collections within a subscription tooAzure Storage using Azure PowerShell cmdlet, Export-AzureRemoteAppUserDisk.</span></span>  <span data-ttu-id="42266-135">Nincs nem képes tooselect egyedi UPD meg.</span><span class="sxs-lookup"><span data-stu-id="42266-135">There is no ability tooselect individual UPD's.</span></span>  <span data-ttu-id="42266-136">Hello PowerShell-parancs végrehajtásakor minden felhasználó lemez fog egy rögzített méretű lemez mérete 50 GB-os és exportált tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="42266-136">When hello PowerShell command is executed, each user disk will be a 50gb in fixed disk size and be exported tooAzure storage.</span></span>  <span data-ttu-id="42266-137">Az Azure storage költségei azonnal tájékozódnia a tárolására.</span><span class="sxs-lookup"><span data-stu-id="42266-137">Costs of Azure storage will incur immediately for this storage.</span></span>  <span data-ttu-id="42266-138">Ellenőrizze a parancs futtatása hiányoznak nem munkamenetek, egyébként pedig hello exportálás sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="42266-138">When running this command ensure there are no sessions otherwise hello export will fail.</span></span>

<span data-ttu-id="42266-139">UPD a tartományhoz csatlakoztatott Azure RemoteApp-telepítésekhez csak akkor használható újra egy távoli asztali szolgáltatások telepítésben, a tartományhoz nem csatlakoztatott központi telepítés nem használható.</span><span class="sxs-lookup"><span data-stu-id="42266-139">UPD's for domain joined Azure RemoteApp deployments can only be used again in an RDS deployment, non-domain joined deployments cannot be used.</span></span>  <span data-ttu-id="42266-140">Ha ezek a lemezek használ a távoli asztali szolgáltatások-telepítés toouse javasoljuk a [parancsfájlok automatikus](https://github.com/arcadiahlyy/aramigration) , exportálása, konvertálja, majd hello UPD meg importálni egy távoli asztali szolgáltatások környezetbe.</span><span class="sxs-lookup"><span data-stu-id="42266-140">If these disks will be used in an RDS deployment we recommend toouse our [automated scripts](https://github.com/arcadiahlyy/aramigration) that will export, convert, and import hello UPD's into an RDS deployment.</span></span>

