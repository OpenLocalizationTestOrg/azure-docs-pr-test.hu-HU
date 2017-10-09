---
title: "Azure Cloud rendszerhéj (előzetes verzió) aaaPersist-fájlok |} Microsoft Docs"
description: "A forgatókönyv a hogyan Azure Cloud rendszerhéj fájlokat továbbra is fennáll."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: b230453d5551c545553d63559741950206e4f1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a><span data-ttu-id="73e38-103">Azure Cloud rendszerhéj fájlok megőrzése</span><span class="sxs-lookup"><span data-stu-id="73e38-103">Persist files in Azure Cloud Shell</span></span>
<span data-ttu-id="73e38-104">Felhő rendszerhéj kihasználja az Azure File storage toopersist fájlok munkamenetei között.</span><span class="sxs-lookup"><span data-stu-id="73e38-104">Cloud Shell takes advantage of Azure File storage toopersist files across sessions.</span></span>

## <a name="set-up-a-clouddrive-file-share"></a><span data-ttu-id="73e38-105">Állítson be egy `clouddrive` fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="73e38-105">Set up a `clouddrive` file share</span></span>
<span data-ttu-id="73e38-106">A kezdeti kezdőlapon felhő rendszerhéj kéri egy új vagy meglévő fájlmegosztás toopersist fájlok között munkamenetek tooassociate.</span><span class="sxs-lookup"><span data-stu-id="73e38-106">On initial start, Cloud Shell prompts you tooassociate a new or existing file share toopersist files across sessions.</span></span>

### <a name="create-new-storage"></a><span data-ttu-id="73e38-107">Új tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="73e38-107">Create new storage</span></span>

<span data-ttu-id="73e38-108">Ha alapszintű beállításokat alkalmazza, és válassza ki az előfizetés csak, felhő rendszerhéj az Ön nevében hello támogatott régióban legközelebbi tooyou hoz létre három erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="73e38-108">When you use basic settings and select only a subscription, Cloud Shell creates three resources on your behalf in hello supported region that's nearest tooyou:</span></span>
* <span data-ttu-id="73e38-109">Erőforráscsoport:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="73e38-109">Resource group: `cloud-shell-storage-<region>`</span></span>
* <span data-ttu-id="73e38-110">Storage-fiók:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="73e38-110">Storage account: `cs<uniqueGuid>`</span></span>
* <span data-ttu-id="73e38-111">Fájlmegosztás:`cs-<user>-<domain>-com-<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="73e38-111">File share: `cs-<user>-<domain>-com-<uniqueGuid>`</span></span>

![hello előfizetési beállítás](media/basic-storage.png)

<span data-ttu-id="73e38-113">hello fájlmegosztás csatlakoztatja `clouddrive` a a `$Home` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="73e38-113">hello file share mounts as `clouddrive` in your `$Home` directory.</span></span> <span data-ttu-id="73e38-114">hello fájlmegosztás egyben használt toostore egy 5 GB-os lemezképnek, amely automatikusan jön létre, és frissíti, és továbbra is fennáll a `$Home` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="73e38-114">hello file share is also used toostore a 5-GB image that's created for you and that automatically updates and persists your `$Home` directory.</span></span> <span data-ttu-id="73e38-115">Ez egy egyszeri művelet, és hello fájlmegosztás automatikusan csatlakoztatja a további munkamenetekhez.</span><span class="sxs-lookup"><span data-stu-id="73e38-115">This is a one-time action, and hello file share mounts automatically in subsequent sessions.</span></span>

### <a name="use-existing-resources"></a><span data-ttu-id="73e38-116">Létező erőforrásokból</span><span class="sxs-lookup"><span data-stu-id="73e38-116">Use existing resources</span></span>

<span data-ttu-id="73e38-117">Speciális beállítás hello segítségével társíthatja a meglévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="73e38-117">By using hello advanced option, you can associate existing resources.</span></span> <span data-ttu-id="73e38-118">Amikor hello tárolási telepítő parancssor jelenik meg, válasszon **megjelenítése speciális beállítások** tooview további beállításokat.</span><span class="sxs-lookup"><span data-stu-id="73e38-118">When hello storage setup prompt appears, select **Show advanced settings** tooview additional options.</span></span> <span data-ttu-id="73e38-119">Meglévő fájlmegosztások kap egy 5 GB-os felhasználói lemezkép toopersist a `$Home` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="73e38-119">Existing file shares receive a 5-GB user image toopersist your `$Home` directory.</span></span> <span data-ttu-id="73e38-120">hello legördülő menükben helyi redundáns & georedundáns storage-fiókok és a felhő rendszerhéj régió szűrve.</span><span class="sxs-lookup"><span data-stu-id="73e38-120">hello drop-down menus are filtered for your Cloud Shell region and for local-redundant & geo-redundant storage accounts.</span></span>

![hello erőforrás tárolócsoport-beállítás](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a><span data-ttu-id="73e38-122">Erőforrás létrehozása az Azure-erőforrás házirendjével korlátozása</span><span class="sxs-lookup"><span data-stu-id="73e38-122">Restrict resource creation with an Azure resource policy</span></span>
<span data-ttu-id="73e38-123">A felhő rendszerhéj létrehozott tárfiókok vannak látva `ms-resource-usage:azure-cloud-shell`.</span><span class="sxs-lookup"><span data-stu-id="73e38-123">Storage accounts that you create in Cloud Shell are tagged with `ms-resource-usage:azure-cloud-shell`.</span></span> <span data-ttu-id="73e38-124">Ha azt szeretné, hogy a storage-fiókok létrehozása a felhő rendszerhéj toodisallow felhasználóit, hozzon létre egy [Azure-erőforrás házirend címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) , amely váltja ki ezt a címkét.</span><span class="sxs-lookup"><span data-stu-id="73e38-124">If you want toodisallow users from creating storage accounts in Cloud Shell, create an [Azure resource policy for tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) that are triggered by this specific tag.</span></span>

## <a name="how-cloud-shell-works"></a><span data-ttu-id="73e38-125">Felhő rendszerhéj működése</span><span class="sxs-lookup"><span data-stu-id="73e38-125">How Cloud Shell works</span></span>
<span data-ttu-id="73e38-126">Felhő rendszerhéj továbbra is fennáll, mind a következő módszerek hello keresztül fájlokat:</span><span class="sxs-lookup"><span data-stu-id="73e38-126">Cloud Shell persists files through both of hello following methods:</span></span>
* <span data-ttu-id="73e38-127">A lemez lemezkép létrehozása a `$Home` directory toopersist összes hello könyvtár tartalmát.</span><span class="sxs-lookup"><span data-stu-id="73e38-127">Creating a disk image of your `$Home` directory toopersist all contents within hello directory.</span></span> <span data-ttu-id="73e38-128">hello lemezképét mentette: a megadott fájlmegosztással `acc_<User>.img` : `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, és automatikusan szinkronizálja a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="73e38-128">hello disk image is saved in your specified file share as `acc_<User>.img` at `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, and it automatically syncs changes.</span></span>

* <span data-ttu-id="73e38-129">A megadott fájlmegosztással csatlakoztatása `clouddrive` a a `$Home` való közvetlen fájl megosztási interakció könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="73e38-129">Mounting your specified file share as `clouddrive` in your `$Home` directory for direct file share interaction.</span></span> <span data-ttu-id="73e38-130">`/Home/<User>/clouddrive`túl van leképezve`fileshare.storage.windows.net/fileshare`.</span><span class="sxs-lookup"><span data-stu-id="73e38-130">`/Home/<User>/clouddrive` is mapped too`fileshare.storage.windows.net/fileshare`.</span></span>
 
> [!NOTE]
> <span data-ttu-id="73e38-131">Az összes fájlt a `$Home` címtár, például az SSH-kulcsokat, a lemez a csatlakoztatott fájl megosztáson található felhasználói lemezkép megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="73e38-131">All files in your `$Home` directory, such as SSH keys, are persisted in your user disk image, which is stored in your mounted file share.</span></span> <span data-ttu-id="73e38-132">Gyakorlati tanácsok alkalmazza, ha az adatokat továbbra is fennáll a `$Home` directory és a csatlakoztatott fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="73e38-132">Apply best practices when you persist information in your `$Home` directory and mounted file share.</span></span>

## <a name="use-hello-clouddrive-command"></a><span data-ttu-id="73e38-133">Használjon hello `clouddrive` parancs</span><span class="sxs-lookup"><span data-stu-id="73e38-133">Use hello `clouddrive` command</span></span>
<span data-ttu-id="73e38-134">Felhő rendszerhéj is futtathatja egy olyan parancsot `clouddrive`, amely lehetővé teszi, hogy toomanually frissítés hello fájlmegosztás meg csatlakoztatott tooCloud rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="73e38-134">With Cloud Shell, you can run a command called `clouddrive`, which enables you toomanually update hello file share that's mounted tooCloud Shell.</span></span>
<span data-ttu-id="73e38-135">![Hello "clouddrive" parancs futtatása](media/clouddrive-h.png)</span><span class="sxs-lookup"><span data-stu-id="73e38-135">![Running hello "clouddrive" command](media/clouddrive-h.png)</span></span>

## <a name="mount-a-new-clouddrive"></a><span data-ttu-id="73e38-136">Egy új csatlakoztatása`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="73e38-136">Mount a new `clouddrive`</span></span>

### <a name="prerequisites-for-manual-mounting"></a><span data-ttu-id="73e38-137">Manuális csatlakoztatásának előfeltételei</span><span class="sxs-lookup"><span data-stu-id="73e38-137">Prerequisites for manual mounting</span></span>
<span data-ttu-id="73e38-138">Frissítheti az hello fájlmegosztás hello segítségével felhő rendszerhéj társított `clouddrive mount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="73e38-138">You can update hello file share that's associated with Cloud Shell by using hello `clouddrive mount` command.</span></span>

<span data-ttu-id="73e38-139">Ha egy meglévő fájlmegosztást csatlakoztatja, hello tárfiókok kell lennie:</span><span class="sxs-lookup"><span data-stu-id="73e38-139">If you mount an existing file share, hello storage accounts must be:</span></span>
* <span data-ttu-id="73e38-140">Helyileg redundáns tárolás vagy georedundáns toosupport tárolófájl-megosztások.</span><span class="sxs-lookup"><span data-stu-id="73e38-140">Locally-redundant storage or geo-redundant storage toosupport file shares.</span></span>
* <span data-ttu-id="73e38-141">A hozzárendelt régióban található.</span><span class="sxs-lookup"><span data-stu-id="73e38-141">Located in your assigned region.</span></span> <span data-ttu-id="73e38-142">Amikor bevezetése, hello régió rendelt hello erőforráscsoport-név szerepel toois `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="73e38-142">When you are onboarding, hello region you are assigned toois listed in hello resource group name `cloud-shell-storage-<region>`.</span></span>

### <a name="supported-storage-regions"></a><span data-ttu-id="73e38-143">Támogatott tárolási régiók</span><span class="sxs-lookup"><span data-stu-id="73e38-143">Supported storage regions</span></span>
<span data-ttu-id="73e38-144">hello az Azure files kell lennie, hello azonos régióban gépként hello felhő rendszerhéj, hogy van-e csatlakoztatni őket.</span><span class="sxs-lookup"><span data-stu-id="73e38-144">hello Azure files must reside in hello same region as hello Cloud Shell machine that you're mounting them to.</span></span> <span data-ttu-id="73e38-145">Felhő rendszerhéj fürtök jelenleg szerepel a következő régiókban hello:</span><span class="sxs-lookup"><span data-stu-id="73e38-145">Cloud Shell clusters currently exist in hello following regions:</span></span>
|<span data-ttu-id="73e38-146">Terület</span><span class="sxs-lookup"><span data-stu-id="73e38-146">Area</span></span>|<span data-ttu-id="73e38-147">Régió</span><span class="sxs-lookup"><span data-stu-id="73e38-147">Region</span></span>|
|---|---|
|<span data-ttu-id="73e38-148">Amerika</span><span class="sxs-lookup"><span data-stu-id="73e38-148">Americas</span></span>|<span data-ttu-id="73e38-149">USA keleti régiója, USA déli középső RÉGIÓJA, USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="73e38-149">East US, South Central US, West US</span></span>|
|<span data-ttu-id="73e38-150">Európa</span><span class="sxs-lookup"><span data-stu-id="73e38-150">Europe</span></span>|<span data-ttu-id="73e38-151">Észak-Európa, Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="73e38-151">North Europe, West Europe</span></span>|
|<span data-ttu-id="73e38-152">Ázsia és a Csendes-óceáni térség</span><span class="sxs-lookup"><span data-stu-id="73e38-152">Asia Pacific</span></span>|<span data-ttu-id="73e38-153">India központi, Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="73e38-153">India Central, Southeast Asia</span></span>|

### <a name="hello-clouddrive-mount-command"></a><span data-ttu-id="73e38-154">Hello `clouddrive mount` parancs</span><span class="sxs-lookup"><span data-stu-id="73e38-154">hello `clouddrive mount` command</span></span>

> [!NOTE]
> <span data-ttu-id="73e38-155">Ha egy új fájlmegosztást most csatlakoztatni, új felhasználói lemezkép hoz létre a `$Home` könyvtár, mert az előző `$Home` kép tartják hello előző fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="73e38-155">If you're mounting a new file share, a new user image is created for your `$Home` directory, because your previous `$Home` image is kept in hello previous file share.</span></span>

<span data-ttu-id="73e38-156">Futtassa a hello `clouddrive mount` hello paraméterek a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="73e38-156">Run hello `clouddrive mount` command with hello following parameters:</span></span>

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

<span data-ttu-id="73e38-157">tooview további információkért futtassa `clouddrive mount -h`, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="73e38-157">tooview more details, run `clouddrive mount -h`, as shown here:</span></span>

![Futó hello "clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a><span data-ttu-id="73e38-159">Válassza le a lemezképet`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="73e38-159">Unmount `clouddrive`</span></span>
<span data-ttu-id="73e38-160">Egy fájlmegosztás meg csatlakoztatott tooCloud rendszerhéj bármikor is leválasztása.</span><span class="sxs-lookup"><span data-stu-id="73e38-160">You can unmount a file share that's mounted tooCloud Shell at any time.</span></span> <span data-ttu-id="73e38-161">Ha a fájlmegosztás nem csatlakoztatott, fogja felszólító toomount egy új fájl megosztási előzetes tooyour következő munkamenet.</span><span class="sxs-lookup"><span data-stu-id="73e38-161">Once your file share is unmounted, you will be prompted toomount a new file share prior tooyour next session.</span></span>

<span data-ttu-id="73e38-162">tooremove fájl megosztása a felhőalapú rendszerhéjból:</span><span class="sxs-lookup"><span data-stu-id="73e38-162">tooremove a file share from Cloud Shell:</span></span>
1. <span data-ttu-id="73e38-163">Futtassa az `clouddrive unmount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="73e38-163">Run `clouddrive unmount`.</span></span>
2. <span data-ttu-id="73e38-164">Megerősíti, és erősítse meg a hello megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="73e38-164">Acknowledge and confirm hello prompts.</span></span>

<span data-ttu-id="73e38-165">A fájlmegosztás tooexist folytatódik, kivéve, ha manuálisan törölni.</span><span class="sxs-lookup"><span data-stu-id="73e38-165">Your file share will continue tooexist unless you delete it manually.</span></span> <span data-ttu-id="73e38-166">Felhő rendszerhéj program már nem keres a fájlmegosztást a további munkamenetekhez.</span><span class="sxs-lookup"><span data-stu-id="73e38-166">Cloud Shell will no longer search for this file share on subsequent sessions.</span></span>

<span data-ttu-id="73e38-167">tooview további információkért futtassa `clouddrive unmount -h`, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="73e38-167">tooview more details, run `clouddrive unmount -h`, as shown here:</span></span>

![Futó hello "clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> <span data-ttu-id="73e38-169">A parancs futtatása minden olyan erőforrásnál nem törli.</span><span class="sxs-lookup"><span data-stu-id="73e38-169">Running this command will not delete any resources.</span></span> <span data-ttu-id="73e38-170">Egy erőforráscsoport, a tárfiókhoz vagy a fájlmegosztással rendelkezik, amely törlésével leképezve tooCloud rendszerhéj véglegesen törli a `$Home` directory lemezképet és egyéb fájlokat a fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="73e38-170">Manually deleting a resource group, storage account, or file share that is mapped tooCloud Shell will permanently delete your `$Home` directory image and any other files in your file share.</span></span> <span data-ttu-id="73e38-171">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="73e38-171">This action cannot be undone.</span></span>

## <a name="list-clouddrive-file-shares"></a><span data-ttu-id="73e38-172">Lista `clouddrive` fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="73e38-172">List `clouddrive` file shares</span></span>
<span data-ttu-id="73e38-173">mely fájlmegosztás csatlakoztatva van, mint a toodiscover `clouddrive`, futtassa a következő hello `df` parancsot.</span><span class="sxs-lookup"><span data-stu-id="73e38-173">toodiscover which file share is mounted as `clouddrive`, run hello following `df` command.</span></span> 

<span data-ttu-id="73e38-174">hello fájl elérési útja tooclouddrive jeleníti meg a tárfiók nevét és fájlmegosztás hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="73e38-174">hello file path tooclouddrive shows your storage account name and file share in hello URL.</span></span> <span data-ttu-id="73e38-175">Például: `//storageaccountname.file.core.windows.net/filesharename`</span><span class="sxs-lookup"><span data-stu-id="73e38-175">For example, `//storageaccountname.file.core.windows.net/filesharename`</span></span>

```
justin@Azure:~$ df
Filesystem                                               1K-blocks     Used Available Use% Mounted on
overlay                                                   30428648 15585636  14826628  52% /
tmpfs                                                       986704        0    986704   0% /dev
tmpfs                                                       986704        0    986704   0% /sys/fs/cgroup
/dev/sda1                                                 30428648 15585636  14826628  52% /etc/hosts
shm                                                          65536        0     65536   0% /dev/shm
//mystoragename.file.core.windows.net/fileshareName        6291456  5242944   1048512  84% /usr/justin/clouddrive
/dev/loop0                                                 5160576   601652   4296780  13% /home/justin
```

## <a name="transfer-local-files-toocloud-shell"></a><span data-ttu-id="73e38-176">Átviteli helyi fájlok tooCloud rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="73e38-176">Transfer local files tooCloud Shell</span></span>
<span data-ttu-id="73e38-177">Hello `clouddrive` hello Azure portál tárolás panel directory szinkronizál.</span><span class="sxs-lookup"><span data-stu-id="73e38-177">hello `clouddrive` directory syncs with hello Azure portal storage blade.</span></span> <span data-ttu-id="73e38-178">Ezen a panelen tootransfer helyi fájlok tooor a fájlmegosztásból.</span><span class="sxs-lookup"><span data-stu-id="73e38-178">Use this blade tootransfer local files tooor from your file share.</span></span> <span data-ttu-id="73e38-179">Felhő rendszerhéj található fájlokat frissítése tükrözi hello a file storage grafikus felhasználói Felülettel hello panel frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="73e38-179">Updating files from within Cloud Shell is reflected in hello file storage GUI when you refresh hello blade.</span></span>

### <a name="download-files"></a><span data-ttu-id="73e38-180">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="73e38-180">Download files</span></span>

![A helyi fájlok listája](media/download.png)
1. <span data-ttu-id="73e38-182">A hello Azure-portálon válassza a toohello csatlakoztatott fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="73e38-182">In hello Azure portal, go toohello mounted file share.</span></span>
2. <span data-ttu-id="73e38-183">Válassza ki a hello cél fájlt.</span><span class="sxs-lookup"><span data-stu-id="73e38-183">Select hello target file.</span></span>
3. <span data-ttu-id="73e38-184">Jelölje be hello **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="73e38-184">Select hello **Download** button.</span></span>

### <a name="upload-files"></a><span data-ttu-id="73e38-185">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="73e38-185">Upload files</span></span>

![Helyi fájlok toobe feltöltve.](media/upload.png)
1. <span data-ttu-id="73e38-187">Nyissa meg tooyour csatlakoztatott fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="73e38-187">Go tooyour mounted file share.</span></span>
2. <span data-ttu-id="73e38-188">Jelölje be hello **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="73e38-188">Select hello **Upload** button.</span></span>
3. <span data-ttu-id="73e38-189">Jelölje be hello vagy fájl, amelyet az tooupload.</span><span class="sxs-lookup"><span data-stu-id="73e38-189">Select hello file or files that you want tooupload.</span></span>
4. <span data-ttu-id="73e38-190">Erősítse meg a hello feltöltése.</span><span class="sxs-lookup"><span data-stu-id="73e38-190">Confirm hello upload.</span></span>

<span data-ttu-id="73e38-191">Most látnia kell, hogy elérhető-e hello fájlok a `clouddrive` felhő rendszerhéj könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="73e38-191">You should now see hello files that are accessible in your `clouddrive` directory in Cloud Shell.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73e38-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73e38-192">Next steps</span></span>
[<span data-ttu-id="73e38-193">Felhő rendszerhéj gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="73e38-193">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="73e38-194">
[További tudnivalók az Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span><span class="sxs-lookup"><span data-stu-id="73e38-194">
[Learn about Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span></span> <br><span data-ttu-id="73e38-195">
[További információk a tárolási címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span><span class="sxs-lookup"><span data-stu-id="73e38-195">
[Learn about storage tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span></span> <br>
