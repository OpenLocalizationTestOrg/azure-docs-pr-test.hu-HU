---
title: "aaaManage csomagrögzítéseket Azure hálózati figyelőt - Azure-portál |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toomanage hello csomag rögzítése funkció az Azure-portál használatával hálózati figyelőt"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 59edd945-34ad-4008-809e-ea904781d918
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 431b145ee215b8d9421fb2aacdce08a0de90b35e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-hello-portal"></a><span data-ttu-id="b8f42-103">Csomag rögzítésekre kezelése Azure hálózati figyelőt hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="b8f42-103">Manage packet captures with Azure Network Watcher using hello portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="b8f42-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b8f42-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="b8f42-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b8f42-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="b8f42-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b8f42-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="b8f42-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b8f42-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="b8f42-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="b8f42-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="b8f42-109">Hálózati figyelő csomagrögzítéssel lehetővé teszi toocreate rögzítési munkamenetek tootrack forgalom tooand virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="b8f42-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="b8f42-110">Szűrők előírt hello rögzítési munkamenet tooensure csak a kívánt hello forgalom rögzíti.</span><span class="sxs-lookup"><span data-stu-id="b8f42-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="b8f42-111">Csomagrögzítéssel segít toodiagnose hálózati rendellenességek észlelését, proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="b8f42-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="b8f42-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="b8f42-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="b8f42-113">Képes tooremotely eseményindító csomag rögzíti őket, ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel fut, manuálisan, hello kívánt számítógépet, amely értékes időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="b8f42-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="b8f42-114">Ez a cikk végigvezeti hello csomagrögzítéssel jelenleg elérhető különböző felügyeleti feladatok.</span><span class="sxs-lookup"><span data-stu-id="b8f42-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="b8f42-115">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="b8f42-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="b8f42-116">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="b8f42-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="b8f42-117">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="b8f42-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="b8f42-118">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="b8f42-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="b8f42-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b8f42-119">Before you begin</span></span>

<span data-ttu-id="b8f42-120">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="b8f42-120">This article assumes that you have hello following resources:</span></span>

- <span data-ttu-id="b8f42-121">Egy példányát a csomagrögzítéssel toocreate kívánt hálózati figyelőt hello régióban</span><span class="sxs-lookup"><span data-stu-id="b8f42-121">An instance of Network Watcher in hello region you want toocreate a packet capture</span></span>
- <span data-ttu-id="b8f42-122">Hello csomagok rendelkező virtuális gép rögzítése engedélyezett bővítményekhez.</span><span class="sxs-lookup"><span data-stu-id="b8f42-122">A virtual machine with hello packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b8f42-123">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="b8f42-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="b8f42-124">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="b8f42-124">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

### <a name="packet-capture-agent-extension-through-hello-portal"></a><span data-ttu-id="b8f42-125">Csomag rögzítési ügynök bővítmény hello portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="b8f42-125">Packet Capture agent extension through hello portal</span></span>

<span data-ttu-id="b8f42-126">tooinstall hello csomagrögzítéssel Virtuálisgép-ügynök hello portálon, keresse meg a virtuális gép tooyour, kattintson a **bővítmények** > **Hozzáadás** keresse meg a **hálózati figyelő ügynöke Windows**</span><span class="sxs-lookup"><span data-stu-id="b8f42-126">tooinstall hello packet capture VM agent through hello portal, navigate tooyour virtual machine, click **Extensions** > **Add** and search for **Network Watcher Agent for Windows**</span></span>

![ügynök nézet][agent]

## <a name="packet-capture-overview"></a><span data-ttu-id="b8f42-128">Csomag rögzítési áttekintése</span><span class="sxs-lookup"><span data-stu-id="b8f42-128">Packet Capture overview</span></span>

<span data-ttu-id="b8f42-129">Keresse meg a toohello [Azure-portálon](https://portal.azure.com) kattintson **hálózati** > **hálózati figyelőt** > **csomag rögzítése**</span><span class="sxs-lookup"><span data-stu-id="b8f42-129">Navigate toohello [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **Packet Capture**</span></span>

<span data-ttu-id="b8f42-130">hello – áttekintés oldalra látható, minden csomag listája függetlenül hello állapot létező rögzíti.</span><span class="sxs-lookup"><span data-stu-id="b8f42-130">hello overview page shows a list of all packet captures that exist no matter hello state.</span></span>

> [!NOTE]
> <span data-ttu-id="b8f42-131">Csomagrögzítéssel kapcsolat toohello storage-fiók szükséges a 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b8f42-131">Packet capture requires connectivity toohello storage account over port 443.</span></span>

![csomag rögzítési áttekintés képernyő][1]

## <a name="start-a-packet-capture"></a><span data-ttu-id="b8f42-133">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="b8f42-133">Start a packet capture</span></span>

<span data-ttu-id="b8f42-134">Kattintson a **Hozzáadás** toocreate egy csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="b8f42-134">Click **Add** toocreate a packet capture.</span></span>

<span data-ttu-id="b8f42-135">a csomagrögzítéssel lehet definiálni hello tulajdonságok a következők:</span><span class="sxs-lookup"><span data-stu-id="b8f42-135">hello properties that can be defined on a packet capture are:</span></span>

<span data-ttu-id="b8f42-136">**Fő beállítások**</span><span class="sxs-lookup"><span data-stu-id="b8f42-136">**Main settings**</span></span>

- <span data-ttu-id="b8f42-137">**Előfizetés** -ezt az értéket hello előfizetés használt, a hálózati figyelőt példánya van szükség minden előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b8f42-137">**Subscription** - This value is hello subscription that is used, an instance of network watcher is needed in each subscription.</span></span>
- <span data-ttu-id="b8f42-138">**Erőforráscsoport** -hello erőforráscsoport alatt célzott hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b8f42-138">**Resource group** - hello resource group of hello virtual machine that is being targeted.</span></span>
- <span data-ttu-id="b8f42-139">**Virtuális gép cél** -hello csomagrögzítéssel futtatott hello virtuális gép</span><span class="sxs-lookup"><span data-stu-id="b8f42-139">**Target virtual machine** - hello virtual machine that is running hello packet capture</span></span>
- <span data-ttu-id="b8f42-140">**Csomag rögzítési neve** -e értéke hello csomagrögzítéssel hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b8f42-140">**Packet capture name** - This value is hello name of hello packet capture.</span></span>

<span data-ttu-id="b8f42-141">**Konfigurációjának rögzítése**</span><span class="sxs-lookup"><span data-stu-id="b8f42-141">**Capture configuration**</span></span>

- <span data-ttu-id="b8f42-142">**A Tárfiók** -határozza meg, ha a tárfiók csomagrögzítéssel menti.</span><span class="sxs-lookup"><span data-stu-id="b8f42-142">**Storage Account** - Determines if packet capture is saved in a storage account.</span></span>
- <span data-ttu-id="b8f42-143">**Fájl** -határozza meg, ha egy csomagrögzítéssel mentett hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="b8f42-143">**File** - Determines if a packet capture is saved locally on hello virtual machine.</span></span>
- <span data-ttu-id="b8f42-144">**Storage-fiókok** -hello kiválasztott tárolási fiók toosave hello csomagok rögzítése a.</span><span class="sxs-lookup"><span data-stu-id="b8f42-144">**Storage Accounts** - hello selected storage account toosave hello packet capture in.</span></span> <span data-ttu-id="b8f42-145">Alapértelmezett helye https://{storage name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription fiókazonosító} /resourcegroups/ {erőforráscsoport name}/providers/microsoft.compute/virtualmachines/{virtual gép neve} / {éé} / {MM} / {NN} / {HH} packetcapture__{MM}_{SS} {XXX} _ .cap.</span><span class="sxs-lookup"><span data-stu-id="b8f42-145">Default location is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription id}/resourcegroups/{resource group name}/providers/microsoft.compute/virtualmachines/{virtual machine name}/{YY}/{MM}/{DD}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap.</span></span> <span data-ttu-id="b8f42-146">(Csak akkor engedélyezett, ha **tárolási** van kiválasztva)</span><span class="sxs-lookup"><span data-stu-id="b8f42-146">(Only enabled if **Storage** is selected)</span></span>
- <span data-ttu-id="b8f42-147">**Helyi fájl elérési útját** -hello helyi elérési út egy virtuális gép toosave hello csomagrögzítéssel a.</span><span class="sxs-lookup"><span data-stu-id="b8f42-147">**Local file path** - hello local path on a virtual machine toosave hello packet capture.</span></span> <span data-ttu-id="b8f42-148">(Csak akkor engedélyezett, ha **fájl** van kiválasztva).</span><span class="sxs-lookup"><span data-stu-id="b8f42-148">(Only enabled if **File** is selected).</span></span> <span data-ttu-id="b8f42-149">Érvényes elérési utat meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="b8f42-149">A Valid path must be supplied</span></span>
- <span data-ttu-id="b8f42-150">**Csomag maximális bájtokat** -szám hello bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.</span><span class="sxs-lookup"><span data-stu-id="b8f42-150">**Maximum bytes per packet** - hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span>
- <span data-ttu-id="b8f42-151">**Munkamenet maximális bájtokat** – teljes rögzített, hello érték elérésekor hello csomagok rögzítési leállítja bájtok száma.</span><span class="sxs-lookup"><span data-stu-id="b8f42-151">**Maximum bytes per session** - Total number of bytes that are captured, once hello value is reached hello packet capture stops.</span></span>
- <span data-ttu-id="b8f42-152">**Időkorlát (másodperc)** -hello csomagok rögzítési toostop határidő beállítása.</span><span class="sxs-lookup"><span data-stu-id="b8f42-152">**Time limit (seconds)** - Sets a time limit for hello packet capture toostop.</span></span> <span data-ttu-id="b8f42-153">Alapértelmezett érték 18000 másodperc.</span><span class="sxs-lookup"><span data-stu-id="b8f42-153">Default is 18000 seconds.</span></span>

> [!NOTE]
> <span data-ttu-id="b8f42-154">Prémium szintű storage-fiókok jelenleg nem támogatottak a csomag tárolásához rögzíti.</span><span class="sxs-lookup"><span data-stu-id="b8f42-154">Premium storage accounts are currently not supported for storing packet captures.</span></span>

<span data-ttu-id="b8f42-155">**Szűrés (nem kötelező)**</span><span class="sxs-lookup"><span data-stu-id="b8f42-155">**Filtering (Optional)**</span></span>

- <span data-ttu-id="b8f42-156">**Protokoll** -protokoll toofilter hello csomagok rögzítési hello.</span><span class="sxs-lookup"><span data-stu-id="b8f42-156">**Protocol** - hello protocol toofilter for hello packet capture.</span></span> <span data-ttu-id="b8f42-157">hello elérhető értékek a következők: TCP, UDP és bármilyen.</span><span class="sxs-lookup"><span data-stu-id="b8f42-157">hello available values are TCP, UDP, and Any.</span></span>
- <span data-ttu-id="b8f42-158">**Helyi IP-cím** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello helyi IP-cím megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="b8f42-158">**Local IP address** - This value filters hello packet capture toopackets where hello local IP address matches this filter value.</span></span>
- <span data-ttu-id="b8f42-159">**Helyi port** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello helyi port megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="b8f42-159">**Local port** - This value filters hello packet capture toopackets where hello local port matches this filter value.</span></span>
- <span data-ttu-id="b8f42-160">**Távoli IP-cím** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello távoli IP megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="b8f42-160">**Remote IP address** - This value filters hello packet capture toopackets where hello remote IP matches this filter value.</span></span>
- <span data-ttu-id="b8f42-161">**Távoli port** -ezt az értéket szűrők hello csomagok rögzítési toopackets ahol hello távoli port megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="b8f42-161">**Remote port** - This value filters hello packet capture toopackets where hello remote port matches this filter value.</span></span>

> [!NOTE]
> <span data-ttu-id="b8f42-162">Port és az IP-cím értékek egyetlen értéket, az értéktartományon vagy egy lehet.</span><span class="sxs-lookup"><span data-stu-id="b8f42-162">Port and IP address values can be a single value, range of values, or a set.</span></span> <span data-ttu-id="b8f42-163">(Ez azt jelenti, hogy a 80-1024 port) Megadhatja, hogy annyi szűrők, ahányat csak szeretne.</span><span class="sxs-lookup"><span data-stu-id="b8f42-163">(that is, 80-1024 for port) You can define as many filters as you want.</span></span>

<span data-ttu-id="b8f42-164">Amikor hello értékek ki van töltve, kattintson a **OK** toocreate hello csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="b8f42-164">Once hello values are filled out, click **OK** toocreate hello packet capture.</span></span>

![a csomagrögzítéssel létrehozása][2]

<span data-ttu-id="b8f42-166">Hello időkorlátot a beállítása után hello csomagrögzítéssel lejárt, hello csomagrögzítéssel leáll, és áttekintheti.</span><span class="sxs-lookup"><span data-stu-id="b8f42-166">After hello time limit set on hello packet capture has expired, hello packet capture will stop and can be reviewed.</span></span> <span data-ttu-id="b8f42-167">Állítsa le a hello csomagok készítését munkamenetek manuálisan is.</span><span class="sxs-lookup"><span data-stu-id="b8f42-167">You can also manually stop hello packet captures sessions.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="b8f42-168">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="b8f42-168">Delete a packet capture</span></span>

<span data-ttu-id="b8f42-169">Hello csomagban rögzítése a nézetet, kattintson a hello **helyi menü** (...), vagy kattintson a jobb gombbal, és kattintson a **törlése** toostop hello csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="b8f42-169">In hello packet capture view, click hello **context menu** (...) or right click, and click **delete** toostop hello packet capture</span></span>

![A csomagrögzítéssel törlése][3]

> [!NOTE]
> <span data-ttu-id="b8f42-171">A csomagrögzítéssel törlése nem érinti hello fájl hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="b8f42-171">Deleting a packet capture does not delete hello file in hello storage account.</span></span>

<span data-ttu-id="b8f42-172">A rendszer felkéri kattintson a kívánt toodelete hello csomagrögzítéssel tooconfirm **Igen**</span><span class="sxs-lookup"><span data-stu-id="b8f42-172">You are asked tooconfirm you want toodelete hello packet capture, click **Yes**</span></span>

![Jóváhagyás][4]

## <a name="stop-a-packet-capture"></a><span data-ttu-id="b8f42-174">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="b8f42-174">Stop a packet capture</span></span>

<span data-ttu-id="b8f42-175">Hello csomagban rögzítése a nézetet, kattintson a hello **helyi menü** (...), vagy kattintson a jobb gombbal, és kattintson a **leállítása** toostop hello csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="b8f42-175">In hello packet capture view, click hello **context menu** (...) or right click, and click **Stop** toostop hello packet capture</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="b8f42-176">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="b8f42-176">Download a packet capture</span></span>

<span data-ttu-id="b8f42-177">A csomag rögzítési munkamenet befejezése után hello rögzítési fájl feltöltött tooblob tároló- és tooa helyi fájlt a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="b8f42-177">Once your packet capture session has completed, hello capture file is uploaded tooblob storage or tooa local file on hello VM.</span></span> <span data-ttu-id="b8f42-178">hello tárolási helye hello csomagrögzítéssel definiálása hello munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="b8f42-178">hello storage location of hello packet capture is defined at creation of hello session.</span></span> <span data-ttu-id="b8f42-179">Egy eszköz tooaccess ezek fájlok rögzítését, mentett tooa tárfiók a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="b8f42-179">A convenient tool tooaccess these capture files saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="b8f42-180">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok mentése tooa tárfiókot, a következő helyen hello:</span><span class="sxs-lookup"><span data-stu-id="b8f42-180">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="b8f42-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8f42-181">Next steps</span></span>

<span data-ttu-id="b8f42-182">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="b8f42-182">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="b8f42-183">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="b8f42-183">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













