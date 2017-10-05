---
title: "Azure Cloud rendszerhéj (előzetes verzió) fájlok maradnak |} Microsoft Docs"
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
ms.openlocfilehash: 61a8bfcf3704f361432400771d8fcc8b81927b53
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="persist-files-in-azure-cloud-shell"></a><span data-ttu-id="bcb4a-103">Azure Cloud rendszerhéj fájlok megőrzése</span><span class="sxs-lookup"><span data-stu-id="bcb4a-103">Persist files in Azure Cloud Shell</span></span>
<span data-ttu-id="bcb4a-104">Felhő rendszerhéj kihasználja az Azure File storage fájlok megőrizni a munkamenetek között.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-104">Cloud Shell takes advantage of Azure File storage to persist files across sessions.</span></span>

## <a name="set-up-a-clouddrive-file-share"></a><span data-ttu-id="bcb4a-105">Állítson be egy `clouddrive` fájlmegosztás</span><span class="sxs-lookup"><span data-stu-id="bcb4a-105">Set up a `clouddrive` file share</span></span>
<span data-ttu-id="bcb4a-106">A kezdeti kezdőlapon a felhő rendszerhéj kéri, hogy társítson egy új vagy meglévő fájlmegosztást fájlok megőrizni a munkamenetek között.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-106">On initial start, Cloud Shell prompts you to associate a new or existing file share to persist files across sessions.</span></span>

### <a name="create-new-storage"></a><span data-ttu-id="bcb4a-107">Új tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="bcb4a-107">Create new storage</span></span>

<span data-ttu-id="bcb4a-108">Ha alapszintű beállításokat alkalmazza, és válassza ki az előfizetés csak, felhőalapú rendszerhéj az Ön nevében, akkor a legközelebb esik a támogatott régióban hoz létre három erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-108">When you use basic settings and select only a subscription, Cloud Shell creates three resources on your behalf in the supported region that's nearest to you:</span></span>
* <span data-ttu-id="bcb4a-109">Erőforráscsoport:`cloud-shell-storage-<region>`</span><span class="sxs-lookup"><span data-stu-id="bcb4a-109">Resource group: `cloud-shell-storage-<region>`</span></span>
* <span data-ttu-id="bcb4a-110">Storage-fiók:`cs<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="bcb4a-110">Storage account: `cs<uniqueGuid>`</span></span>
* <span data-ttu-id="bcb4a-111">Fájlmegosztás:`cs-<user>-<domain>-com-<uniqueGuid>`</span><span class="sxs-lookup"><span data-stu-id="bcb4a-111">File share: `cs-<user>-<domain>-com-<uniqueGuid>`</span></span>

![Az előfizetési beállítás](media/basic-storage.png)

<span data-ttu-id="bcb4a-113">A fájlmegosztás csatlakoztatja `clouddrive` a a `$Home` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-113">The file share mounts as `clouddrive` in your `$Home` directory.</span></span> <span data-ttu-id="bcb4a-114">A fájlmegosztás is tárolására szolgál egy 5 GB-os lemezképet, amely jön létre, és, amely automatikusan frissíti, és továbbra is fennáll a `$Home` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-114">The file share is also used to store a 5-GB image that's created for you and that automatically updates and persists your `$Home` directory.</span></span> <span data-ttu-id="bcb4a-115">Ez egy egyszeri művelet, és a további munkamenetekhez automatikusan csatlakoztatja a fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-115">This is a one-time action, and the file share mounts automatically in subsequent sessions.</span></span>

### <a name="use-existing-resources"></a><span data-ttu-id="bcb4a-116">Létező erőforrásokból</span><span class="sxs-lookup"><span data-stu-id="bcb4a-116">Use existing resources</span></span>

<span data-ttu-id="bcb4a-117">A speciális beállítás használatával társíthatja a meglévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-117">By using the advanced option, you can associate existing resources.</span></span> <span data-ttu-id="bcb4a-118">Ha a tárolási telepítő parancssor jelenik meg, válassza ki a **megjelenítése speciális beállítások** további beállítások.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-118">When the storage setup prompt appears, select **Show advanced settings** to view additional options.</span></span> <span data-ttu-id="bcb4a-119">Meglévő fájlmegosztások kap egy 5 GB-os felhasználói lemezképet a `$Home` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-119">Existing file shares receive a 5-GB user image to persist your `$Home` directory.</span></span> <span data-ttu-id="bcb4a-120">A legördülő menükben helyi redundáns & georedundáns storage-fiókok és a felhő rendszerhéj régió szűrve.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-120">The drop-down menus are filtered for your Cloud Shell region and for local-redundant & geo-redundant storage accounts.</span></span>

![Az erőforrás-csoport beállítás](media/advanced-storage.png)

### <a name="restrict-resource-creation-with-an-azure-resource-policy"></a><span data-ttu-id="bcb4a-122">Erőforrás létrehozása az Azure-erőforrás házirendjével korlátozása</span><span class="sxs-lookup"><span data-stu-id="bcb4a-122">Restrict resource creation with an Azure resource policy</span></span>
<span data-ttu-id="bcb4a-123">A felhő rendszerhéj létrehozott tárfiókok vannak látva `ms-resource-usage:azure-cloud-shell`.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-123">Storage accounts that you create in Cloud Shell are tagged with `ms-resource-usage:azure-cloud-shell`.</span></span> <span data-ttu-id="bcb4a-124">Ha azt szeretné, hogy a storage-fiókok létrehozása a felhő rendszerhéj letilthatja, hozzon létre egy [Azure-erőforrás házirend címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) , amely váltja ki ezt a címkét.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-124">If you want to disallow users from creating storage accounts in Cloud Shell, create an [Azure resource policy for tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy-tags) that are triggered by this specific tag.</span></span>

## <a name="how-cloud-shell-works"></a><span data-ttu-id="bcb4a-125">Felhő rendszerhéj működése</span><span class="sxs-lookup"><span data-stu-id="bcb4a-125">How Cloud Shell works</span></span>
<span data-ttu-id="bcb4a-126">Felhő rendszerhéj továbbra is fennáll, az alábbi eljárások közül mindkettő keresztül fájlokat:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-126">Cloud Shell persists files through both of the following methods:</span></span>
* <span data-ttu-id="bcb4a-127">A lemez lemezkép létrehozása a `$Home` directory megőrizni a címtárban található összes tartalmat.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-127">Creating a disk image of your `$Home` directory to persist all contents within the directory.</span></span> <span data-ttu-id="bcb4a-128">Az alapkonfiguráció lemezképét mentette: a megadott fájlmegosztással `acc_<User>.img` : `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, és automatikusan szinkronizálja a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-128">The disk image is saved in your specified file share as `acc_<User>.img` at `fileshare.storage.windows.net/fileshare/.cloudconsole/acc_<User>.img`, and it automatically syncs changes.</span></span>

* <span data-ttu-id="bcb4a-129">A megadott fájlmegosztással csatlakoztatása `clouddrive` a a `$Home` való közvetlen fájl megosztási interakció könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-129">Mounting your specified file share as `clouddrive` in your `$Home` directory for direct file share interaction.</span></span> <span data-ttu-id="bcb4a-130">`/Home/<User>/clouddrive`van leképezve `fileshare.storage.windows.net/fileshare`.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-130">`/Home/<User>/clouddrive` is mapped to `fileshare.storage.windows.net/fileshare`.</span></span>
 
> [!NOTE]
> <span data-ttu-id="bcb4a-131">Az összes fájlt a `$Home` címtár, például az SSH-kulcsokat, a lemez a csatlakoztatott fájl megosztáson található felhasználói lemezkép megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-131">All files in your `$Home` directory, such as SSH keys, are persisted in your user disk image, which is stored in your mounted file share.</span></span> <span data-ttu-id="bcb4a-132">Gyakorlati tanácsok alkalmazza, ha az adatokat továbbra is fennáll a `$Home` directory és a csatlakoztatott fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-132">Apply best practices when you persist information in your `$Home` directory and mounted file share.</span></span>

## <a name="use-the-clouddrive-command"></a><span data-ttu-id="bcb4a-133">Használja a `clouddrive` parancs</span><span class="sxs-lookup"><span data-stu-id="bcb4a-133">Use the `clouddrive` command</span></span>
<span data-ttu-id="bcb4a-134">Felhő rendszerhéj is futtathatja egy olyan parancsot `clouddrive`, amely lehetővé teszi, hogy manuálisan frissítse a felhő rendszerhéj a fájlmegosztást, amely csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-134">With Cloud Shell, you can run a command called `clouddrive`, which enables you to manually update the file share that's mounted to Cloud Shell.</span></span>
<span data-ttu-id="bcb4a-135">![A "clouddrive" parancs futtatása](media/clouddrive-h.png)</span><span class="sxs-lookup"><span data-stu-id="bcb4a-135">![Running the "clouddrive" command](media/clouddrive-h.png)</span></span>

## <a name="mount-a-new-clouddrive"></a><span data-ttu-id="bcb4a-136">Egy új csatlakoztatása`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="bcb4a-136">Mount a new `clouddrive`</span></span>

### <a name="prerequisites-for-manual-mounting"></a><span data-ttu-id="bcb4a-137">Manuális csatlakoztatásának előfeltételei</span><span class="sxs-lookup"><span data-stu-id="bcb4a-137">Prerequisites for manual mounting</span></span>
<span data-ttu-id="bcb4a-138">A fájlmegosztás tartozó felhő rendszerhéj használatával frissítheti a `clouddrive mount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-138">You can update the file share that's associated with Cloud Shell by using the `clouddrive mount` command.</span></span>

<span data-ttu-id="bcb4a-139">Ha egy meglévő fájlmegosztást csatlakoztatja, a tárfiókok kell lennie:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-139">If you mount an existing file share, the storage accounts must be:</span></span>
* <span data-ttu-id="bcb4a-140">Helyileg redundáns tárolás vagy georedundáns tárolás fájlmegosztások támogatásához.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-140">Locally-redundant storage or geo-redundant storage to support file shares.</span></span>
* <span data-ttu-id="bcb4a-141">A hozzárendelt régióban található.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-141">Located in your assigned region.</span></span> <span data-ttu-id="bcb4a-142">Amikor bevezetése, a régiót hozzá van rendelve, az erőforráscsoport neve szerepel `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-142">When you are onboarding, the region you are assigned to is listed in the resource group name `cloud-shell-storage-<region>`.</span></span>

### <a name="supported-storage-regions"></a><span data-ttu-id="bcb4a-143">Támogatott tárolási régiók</span><span class="sxs-lookup"><span data-stu-id="bcb4a-143">Supported storage regions</span></span>
<span data-ttu-id="bcb4a-144">Az Azure files és a felhő rendszerhéj gépet, hogy van-e csatlakoztatni őket ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-144">The Azure files must reside in the same region as the Cloud Shell machine that you're mounting them to.</span></span> <span data-ttu-id="bcb4a-145">Felhő rendszerhéj fürtök jelenleg a következő régiókban található:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-145">Cloud Shell clusters currently exist in the following regions:</span></span>
|<span data-ttu-id="bcb4a-146">Terület</span><span class="sxs-lookup"><span data-stu-id="bcb4a-146">Area</span></span>|<span data-ttu-id="bcb4a-147">Régió</span><span class="sxs-lookup"><span data-stu-id="bcb4a-147">Region</span></span>|
|---|---|
|<span data-ttu-id="bcb4a-148">Amerika</span><span class="sxs-lookup"><span data-stu-id="bcb4a-148">Americas</span></span>|<span data-ttu-id="bcb4a-149">USA keleti régiója, USA déli középső RÉGIÓJA, USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="bcb4a-149">East US, South Central US, West US</span></span>|
|<span data-ttu-id="bcb4a-150">Európa</span><span class="sxs-lookup"><span data-stu-id="bcb4a-150">Europe</span></span>|<span data-ttu-id="bcb4a-151">Észak-Európa, Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="bcb4a-151">North Europe, West Europe</span></span>|
|<span data-ttu-id="bcb4a-152">Ázsia és a Csendes-óceáni térség</span><span class="sxs-lookup"><span data-stu-id="bcb4a-152">Asia Pacific</span></span>|<span data-ttu-id="bcb4a-153">India központi, Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="bcb4a-153">India Central, Southeast Asia</span></span>|

### <a name="the-clouddrive-mount-command"></a><span data-ttu-id="bcb4a-154">A `clouddrive mount` parancs</span><span class="sxs-lookup"><span data-stu-id="bcb4a-154">The `clouddrive mount` command</span></span>

> [!NOTE]
> <span data-ttu-id="bcb4a-155">Ha egy új fájlmegosztást most csatlakoztatni, új felhasználói lemezkép hoz létre a `$Home` könyvtár, mert az előző `$Home` kép tartják a korábbi fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-155">If you're mounting a new file share, a new user image is created for your `$Home` directory, because your previous `$Home` image is kept in the previous file share.</span></span>

<span data-ttu-id="bcb4a-156">Futtassa a `clouddrive mount` parancsot a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-156">Run the `clouddrive mount` command with the following parameters:</span></span>

```
clouddrive mount -s mySubscription -g myRG -n storageAccountName -f fileShareName
```

<span data-ttu-id="bcb4a-157">További részletek megtekintéséhez futtassa `clouddrive mount -h`, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-157">To view more details, run `clouddrive mount -h`, as shown here:</span></span>

![Fut a "clouddrive mount'command](media/mount-h.png)

## <a name="unmount-clouddrive"></a><span data-ttu-id="bcb4a-159">Válassza le a lemezképet`clouddrive`</span><span class="sxs-lookup"><span data-stu-id="bcb4a-159">Unmount `clouddrive`</span></span>
<span data-ttu-id="bcb4a-160">Egy fájlmegosztást, hogy bármikor a felhő rendszerhéj csatlakoztatott is leválasztása.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-160">You can unmount a file share that's mounted to Cloud Shell at any time.</span></span> <span data-ttu-id="bcb4a-161">Ha a fájlmegosztás nem csatlakoztatott, kérni fogja a következő munkamenet előtt egy új fájlmegosztást csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-161">Once your file share is unmounted, you will be prompted to mount a new file share prior to your next session.</span></span>

<span data-ttu-id="bcb4a-162">Fájlmegosztás eltávolítása felhő rendszerhéj:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-162">To remove a file share from Cloud Shell:</span></span>
1. <span data-ttu-id="bcb4a-163">Futtassa az `clouddrive unmount` parancsot.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-163">Run `clouddrive unmount`.</span></span>
2. <span data-ttu-id="bcb4a-164">Igazolja, és erősítse meg a megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-164">Acknowledge and confirm the prompts.</span></span>

<span data-ttu-id="bcb4a-165">A fájlmegosztás továbbra is létezik, kivéve, ha manuálisan törölni.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-165">Your file share will continue to exist unless you delete it manually.</span></span> <span data-ttu-id="bcb4a-166">Felhő rendszerhéj program már nem keres a fájlmegosztást a további munkamenetekhez.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-166">Cloud Shell will no longer search for this file share on subsequent sessions.</span></span>

<span data-ttu-id="bcb4a-167">További részletek megtekintéséhez futtassa `clouddrive unmount -h`, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="bcb4a-167">To view more details, run `clouddrive unmount -h`, as shown here:</span></span>

![Fut a "clouddrive unmount'command](media/unmount-h.png)

> [!WARNING]
> <span data-ttu-id="bcb4a-169">A parancs futtatása minden olyan erőforrásnál nem törli.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-169">Running this command will not delete any resources.</span></span> <span data-ttu-id="bcb4a-170">Egy erőforráscsoport, a tárfiókhoz vagy a felhő rendszerhéj leképezett fájlmegosztás törlésével véglegesen törli a `$Home` directory lemezképet és egyéb fájlokat a fájlmegosztásban.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-170">Manually deleting a resource group, storage account, or file share that is mapped to Cloud Shell will permanently delete your `$Home` directory image and any other files in your file share.</span></span> <span data-ttu-id="bcb4a-171">Ez a művelet nem vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-171">This action cannot be undone.</span></span>

## <a name="list-clouddrive-file-shares"></a><span data-ttu-id="bcb4a-172">Lista `clouddrive` fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="bcb4a-172">List `clouddrive` file shares</span></span>
<span data-ttu-id="bcb4a-173">Derítsen fel, melyik fájlmegosztás csatlakoztatva van, mint `clouddrive`, futtassa a következő `df` parancsot.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-173">To discover which file share is mounted as `clouddrive`, run the following `df` command.</span></span> 

<span data-ttu-id="bcb4a-174">A fájl elérési útját clouddrive jeleníti meg a tárfiók nevét és az URL-címben fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-174">The file path to clouddrive shows your storage account name and file share in the URL.</span></span> <span data-ttu-id="bcb4a-175">Például: `//storageaccountname.file.core.windows.net/filesharename`</span><span class="sxs-lookup"><span data-stu-id="bcb4a-175">For example, `//storageaccountname.file.core.windows.net/filesharename`</span></span>

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

## <a name="transfer-local-files-to-cloud-shell"></a><span data-ttu-id="bcb4a-176">Helyi fájlok átvitele a felhő rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="bcb4a-176">Transfer local files to Cloud Shell</span></span>
<span data-ttu-id="bcb4a-177">A `clouddrive` directory szinkronizál az Azure portál tárolás panel.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-177">The `clouddrive` directory syncs with the Azure portal storage blade.</span></span> <span data-ttu-id="bcb4a-178">Ezen a panelen vihet át a helyi fájlokat vagy a fájlmegosztásból.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-178">Use this blade to transfer local files to or from your file share.</span></span> <span data-ttu-id="bcb4a-179">Felhő rendszerhéj található fájlokat frissítése is megjelenik a file storage grafikus felhasználói Felülettel amikor frissíteni a panelt.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-179">Updating files from within Cloud Shell is reflected in the file storage GUI when you refresh the blade.</span></span>

### <a name="download-files"></a><span data-ttu-id="bcb4a-180">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="bcb4a-180">Download files</span></span>

![A helyi fájlok listája](media/download.png)
1. <span data-ttu-id="bcb4a-182">Az Azure-portálon lépjen a csatlakoztatott fájlmegosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-182">In the Azure portal, go to the mounted file share.</span></span>
2. <span data-ttu-id="bcb4a-183">Válassza ki a cél-fájlt.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-183">Select the target file.</span></span>
3. <span data-ttu-id="bcb4a-184">Válassza ki a **letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-184">Select the **Download** button.</span></span>

### <a name="upload-files"></a><span data-ttu-id="bcb4a-185">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="bcb4a-185">Upload files</span></span>

![Fel kell tölteni a helyi fájlok](media/upload.png)
1. <span data-ttu-id="bcb4a-187">Nyissa meg a csatlakoztatott fájlmegosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-187">Go to your mounted file share.</span></span>
2. <span data-ttu-id="bcb4a-188">Válassza ki a **feltöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-188">Select the **Upload** button.</span></span>
3. <span data-ttu-id="bcb4a-189">Válassza ki a fájl vagy a feltölteni kívánt fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-189">Select the file or files that you want to upload.</span></span>
4. <span data-ttu-id="bcb4a-190">Erősítse meg a feltöltést.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-190">Confirm the upload.</span></span>

<span data-ttu-id="bcb4a-191">Most látnia kell, hogy elérhető-e a fájlokat a `clouddrive` felhő rendszerhéj könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="bcb4a-191">You should now see the files that are accessible in your `clouddrive` directory in Cloud Shell.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcb4a-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bcb4a-192">Next steps</span></span>
[<span data-ttu-id="bcb4a-193">Felhő rendszerhéj gyors üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="bcb4a-193">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="bcb4a-194">
[További tudnivalók az Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span><span class="sxs-lookup"><span data-stu-id="bcb4a-194">
[Learn about Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage)</span></span> <br><span data-ttu-id="bcb4a-195">
[További információk a tárolási címkék](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span><span class="sxs-lookup"><span data-stu-id="bcb4a-195">
[Learn about storage tags](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)</span></span> <br>
