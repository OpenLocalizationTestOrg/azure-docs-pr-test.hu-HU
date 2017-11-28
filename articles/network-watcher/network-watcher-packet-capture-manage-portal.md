---
title: "Csomag rögzíti az Azure hálózati figyelőt - Azure-portálon kezelése |} Microsoft Docs"
description: "Ezen a lapon ismerteti, hogyan kezelheti a csomag rögzítése funkció az Azure-portál használatával hálózati figyelőt"
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
ms.openlocfilehash: 33390532cc4fc1129a4f960d589f41bc95e5a1ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-the-portal"></a><span data-ttu-id="08d75-103">Csomag rögzítésekre kezelése Azure hálózati figyelőt a portál használatával</span><span class="sxs-lookup"><span data-stu-id="08d75-103">Manage packet captures with Azure Network Watcher using the portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="08d75-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="08d75-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="08d75-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08d75-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="08d75-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="08d75-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="08d75-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="08d75-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="08d75-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="08d75-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="08d75-109">Hálózati figyelő csomagrögzítéssel rögzítési munkamenetek nyomon követéséhez forgalma és a virtuális gép létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="08d75-109">Network Watcher packet capture allows you to create capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="08d75-110">Annak érdekében, hogy csak a kívánt forgalom rögzíti a rögzítési munkamenet szűrők célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="08d75-110">Filters are provided for the capture session to ensure you capture only the traffic you want.</span></span> <span data-ttu-id="08d75-111">Csomagrögzítéssel segít diagnosztizálni hálózati rendellenességeket proaktív és reaktív is.</span><span class="sxs-lookup"><span data-stu-id="08d75-111">Packet capture helps to diagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="08d75-112">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, hálózati behatolások, ügyfél-kiszolgáló közötti kommunikációt, és még sok más hibakeresési információkat való összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="08d75-112">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span> <span data-ttu-id="08d75-113">Őket távolról elindítása csomag rögzíti, ez a funkció megkönnyíti a csomagrögzítéssel fut, manuálisan, a kívánt számítógépet, amely értékes időt takaríthat meg okozta terheket.</span><span class="sxs-lookup"><span data-stu-id="08d75-113">By being able to remotely trigger packet captures, this capability eases the burden of running a packet capture manually and on the desired machine, which saves valuable time.</span></span>

<span data-ttu-id="08d75-114">Ez a cikk végigvezeti Önt a különböző felügyeleti feladatok, amelyek jelenleg a csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="08d75-114">This article takes you through the different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="08d75-115">**A csomagrögzítéssel indítása**</span><span class="sxs-lookup"><span data-stu-id="08d75-115">**Start a packet capture**</span></span>](#start-a-packet-capture)
- [<span data-ttu-id="08d75-116">**A csomagrögzítéssel leállítása**</span><span class="sxs-lookup"><span data-stu-id="08d75-116">**Stop a packet capture**</span></span>](#stop-a-packet-capture)
- [<span data-ttu-id="08d75-117">**A csomagrögzítéssel törlése**</span><span class="sxs-lookup"><span data-stu-id="08d75-117">**Delete a packet capture**</span></span>](#delete-a-packet-capture)
- [<span data-ttu-id="08d75-118">**A csomagrögzítéssel letöltése**</span><span class="sxs-lookup"><span data-stu-id="08d75-118">**Download a packet capture**</span></span>](#download-a-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="08d75-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="08d75-119">Before you begin</span></span>

<span data-ttu-id="08d75-120">Ez a cikk feltételezi, hogy rendelkezik-e a következőket:</span><span class="sxs-lookup"><span data-stu-id="08d75-120">This article assumes that you have the following resources:</span></span>

- <span data-ttu-id="08d75-121">A csomagrögzítéssel létrehozni kívánt hálózati figyelőt régióban példánya</span><span class="sxs-lookup"><span data-stu-id="08d75-121">An instance of Network Watcher in the region you want to create a packet capture</span></span>
- <span data-ttu-id="08d75-122">A csomag rögzítési engedélyezett bővítményekhez virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="08d75-122">A virtual machine with the packet capture extension enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08d75-123">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="08d75-123">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="08d75-124">A bővítmény telepítése a Windows virtuális gép a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="08d75-124">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

### <a name="packet-capture-agent-extension-through-the-portal"></a><span data-ttu-id="08d75-125">Csomag rögzítési ügynök bővítmény a portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="08d75-125">Packet Capture agent extension through the portal</span></span>

<span data-ttu-id="08d75-126">A csomag rögzítési Virtuálisgép-ügynök a portálon keresztül telepítéséhez nyissa meg a virtuális gép, kattintson a **bővítmények** > **Hozzáadás** keresse meg a **hálózati figyelő ügynök a Windows**</span><span class="sxs-lookup"><span data-stu-id="08d75-126">To install the packet capture VM agent through the portal, navigate to your virtual machine, click **Extensions** > **Add** and search for **Network Watcher Agent for Windows**</span></span>

![ügynök nézet][agent]

## <a name="packet-capture-overview"></a><span data-ttu-id="08d75-128">Csomag rögzítési áttekintése</span><span class="sxs-lookup"><span data-stu-id="08d75-128">Packet Capture overview</span></span>

<span data-ttu-id="08d75-129">Keresse meg a [Azure-portálon](https://portal.azure.com) kattintson **hálózati** > **hálózati figyelőt** > **csomag rögzítése**</span><span class="sxs-lookup"><span data-stu-id="08d75-129">Navigate to the [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **Packet Capture**</span></span>

<span data-ttu-id="08d75-130">A – Áttekintés lapon látható összes csomag listája létező függetlenül állapotát rögzíti.</span><span class="sxs-lookup"><span data-stu-id="08d75-130">The overview page shows a list of all packet captures that exist no matter the state.</span></span>

> [!NOTE]
> <span data-ttu-id="08d75-131">Csomagrögzítéssel a tárfiókkal igényel a 443-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="08d75-131">Packet capture requires connectivity to the storage account over port 443.</span></span>

![csomag rögzítési áttekintés képernyő][1]

## <a name="start-a-packet-capture"></a><span data-ttu-id="08d75-133">A csomagrögzítéssel indítása</span><span class="sxs-lookup"><span data-stu-id="08d75-133">Start a packet capture</span></span>

<span data-ttu-id="08d75-134">Kattintson a **Hozzáadás** egy csomagrögzítéssel létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="08d75-134">Click **Add** to create a packet capture.</span></span>

<span data-ttu-id="08d75-135">A csomagrögzítéssel lehet definiálni tulajdonságok a következők:</span><span class="sxs-lookup"><span data-stu-id="08d75-135">The properties that can be defined on a packet capture are:</span></span>

<span data-ttu-id="08d75-136">**Fő beállítások**</span><span class="sxs-lookup"><span data-stu-id="08d75-136">**Main settings**</span></span>

- <span data-ttu-id="08d75-137">**Előfizetés** -ezt az értéket az előfizetés használt, a hálózati figyelőt példánya van szükség minden előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="08d75-137">**Subscription** - This value is the subscription that is used, an instance of network watcher is needed in each subscription.</span></span>
- <span data-ttu-id="08d75-138">**Erőforráscsoport** -az erőforráscsoport a célzott alatt álló virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="08d75-138">**Resource group** - The resource group of the virtual machine that is being targeted.</span></span>
- <span data-ttu-id="08d75-139">**A cél virtuális gép** – a virtuális gép, amelyen a csomagrögzítéssel fut.</span><span class="sxs-lookup"><span data-stu-id="08d75-139">**Target virtual machine** - The virtual machine that is running the packet capture</span></span>
- <span data-ttu-id="08d75-140">**Csomag rögzítési neve** -ezt az értéket a csomagrögzítéssel neve.</span><span class="sxs-lookup"><span data-stu-id="08d75-140">**Packet capture name** - This value is the name of the packet capture.</span></span>

<span data-ttu-id="08d75-141">**Konfigurációjának rögzítése**</span><span class="sxs-lookup"><span data-stu-id="08d75-141">**Capture configuration**</span></span>

- <span data-ttu-id="08d75-142">**A Tárfiók** -határozza meg, ha a tárfiók csomagrögzítéssel menti.</span><span class="sxs-lookup"><span data-stu-id="08d75-142">**Storage Account** - Determines if packet capture is saved in a storage account.</span></span>
- <span data-ttu-id="08d75-143">**Fájl** -határozza meg, ha egy csomagrögzítéssel rendszer helyileg menti a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="08d75-143">**File** - Determines if a packet capture is saved locally on the virtual machine.</span></span>
- <span data-ttu-id="08d75-144">**Storage-fiókok** – a kiválasztott tárolási fiók segítségével menti a csomagrögzítéssel a.</span><span class="sxs-lookup"><span data-stu-id="08d75-144">**Storage Accounts** - The selected storage account to save the packet capture in.</span></span> <span data-ttu-id="08d75-145">Alapértelmezett helye https://{storage name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription fiókazonosító} /resourcegroups/ {erőforráscsoport name}/providers/microsoft.compute/virtualmachines/{virtual gép neve} / {éé} / {MM} / {NN} / {HH} packetcapture__{MM}_{SS} {XXX} _ .cap.</span><span class="sxs-lookup"><span data-stu-id="08d75-145">Default location is https://{storage account name}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscription id}/resourcegroups/{resource group name}/providers/microsoft.compute/virtualmachines/{virtual machine name}/{YY}/{MM}/{DD}/packetcapture_{HH}_{MM}_{SS}_{XXX}.cap.</span></span> <span data-ttu-id="08d75-146">(Csak akkor engedélyezett, ha **tárolási** van kiválasztva)</span><span class="sxs-lookup"><span data-stu-id="08d75-146">(Only enabled if **Storage** is selected)</span></span>
- <span data-ttu-id="08d75-147">**Helyi fájl elérési útját** -a helyi elérési út mentése a csomagrögzítéssel virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="08d75-147">**Local file path** - The local path on a virtual machine to save the packet capture.</span></span> <span data-ttu-id="08d75-148">(Csak akkor engedélyezett, ha **fájl** van kiválasztva).</span><span class="sxs-lookup"><span data-stu-id="08d75-148">(Only enabled if **File** is selected).</span></span> <span data-ttu-id="08d75-149">Érvényes elérési utat meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="08d75-149">A Valid path must be supplied</span></span>
- <span data-ttu-id="08d75-150">**Csomag maximális bájtokat** - szám bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.</span><span class="sxs-lookup"><span data-stu-id="08d75-150">**Maximum bytes per packet** - The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span>
- <span data-ttu-id="08d75-151">**Munkamenet maximális bájtokat** – teljes száma, amelyek rögzítve lesznek, az érték elérésekor a csomag rögzítési leáll bájt.</span><span class="sxs-lookup"><span data-stu-id="08d75-151">**Maximum bytes per session** - Total number of bytes that are captured, once the value is reached the packet capture stops.</span></span>
- <span data-ttu-id="08d75-152">**Időkorlát (másodperc)** – beállítja a csomagrögzítéssel leállításához időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="08d75-152">**Time limit (seconds)** - Sets a time limit for the packet capture to stop.</span></span> <span data-ttu-id="08d75-153">Alapértelmezett érték 18000 másodperc.</span><span class="sxs-lookup"><span data-stu-id="08d75-153">Default is 18000 seconds.</span></span>

> [!NOTE]
> <span data-ttu-id="08d75-154">Prémium szintű storage-fiókok jelenleg nem támogatottak a csomag tárolásához rögzíti.</span><span class="sxs-lookup"><span data-stu-id="08d75-154">Premium storage accounts are currently not supported for storing packet captures.</span></span>

<span data-ttu-id="08d75-155">**Szűrés (nem kötelező)**</span><span class="sxs-lookup"><span data-stu-id="08d75-155">**Filtering (Optional)**</span></span>

- <span data-ttu-id="08d75-156">**Protokoll** -a protokollt, hogy a csomagrögzítéssel szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="08d75-156">**Protocol** - The protocol to filter for the packet capture.</span></span> <span data-ttu-id="08d75-157">A lehetséges értékek a következők: TCP, UDP és bármilyen.</span><span class="sxs-lookup"><span data-stu-id="08d75-157">The available values are TCP, UDP, and Any.</span></span>
- <span data-ttu-id="08d75-158">**Helyi IP-cím** -Ha a helyi IP-cím megegyezik-e a szűrő szűrők ezt az értéket a csomagrögzítéssel csomagok.</span><span class="sxs-lookup"><span data-stu-id="08d75-158">**Local IP address** - This value filters the packet capture to packets where the local IP address matches this filter value.</span></span>
- <span data-ttu-id="08d75-159">**Helyi port** -Ha a helyi port megegyezik-e a szűrő szűrők ezt az értéket a csomagrögzítéssel csomagok.</span><span class="sxs-lookup"><span data-stu-id="08d75-159">**Local port** - This value filters the packet capture to packets where the local port matches this filter value.</span></span>
- <span data-ttu-id="08d75-160">**Távoli IP-cím** – Ha az távoli IP-cím megegyezik-e a szűrő szűrők ezt az értéket a csomagrögzítéssel csomagok.</span><span class="sxs-lookup"><span data-stu-id="08d75-160">**Remote IP address** - This value filters the packet capture to packets where the remote IP matches this filter value.</span></span>
- <span data-ttu-id="08d75-161">**Távoli port** -Ha a távoli port megegyezik-e a szűrő szűrők ezt az értéket a csomagrögzítéssel csomagok.</span><span class="sxs-lookup"><span data-stu-id="08d75-161">**Remote port** - This value filters the packet capture to packets where the remote port matches this filter value.</span></span>

> [!NOTE]
> <span data-ttu-id="08d75-162">Port és az IP-cím értékek egyetlen értéket, az értéktartományon vagy egy lehet.</span><span class="sxs-lookup"><span data-stu-id="08d75-162">Port and IP address values can be a single value, range of values, or a set.</span></span> <span data-ttu-id="08d75-163">(Ez azt jelenti, hogy a 80-1024 port) Megadhatja, hogy annyi szűrők, ahányat csak szeretne.</span><span class="sxs-lookup"><span data-stu-id="08d75-163">(that is, 80-1024 for port) You can define as many filters as you want.</span></span>

<span data-ttu-id="08d75-164">Ha az értékek ki van töltve, kattintson **OK** a csomagrögzítéssel létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="08d75-164">Once the values are filled out, click **OK** to create the packet capture.</span></span>

![a csomagrögzítéssel létrehozása][2]

<span data-ttu-id="08d75-166">Lejárt az időkorlát a csomagrögzítéssel be, miután a csomagrögzítéssel leáll, és áttekintheti.</span><span class="sxs-lookup"><span data-stu-id="08d75-166">After the time limit set on the packet capture has expired, the packet capture will stop and can be reviewed.</span></span> <span data-ttu-id="08d75-167">A csomag rögzítésekre munkamenetek manuálisan is leállíthatja.</span><span class="sxs-lookup"><span data-stu-id="08d75-167">You can also manually stop the packet captures sessions.</span></span>

## <a name="delete-a-packet-capture"></a><span data-ttu-id="08d75-168">A csomagrögzítéssel törlése</span><span class="sxs-lookup"><span data-stu-id="08d75-168">Delete a packet capture</span></span>

<span data-ttu-id="08d75-169">A csomag rögzítési nézetben kattintson a **helyi menü** (...), vagy kattintson a jobb gombbal, majd kattintson **törlése** a csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="08d75-169">In the packet capture view, click the **context menu** (...) or right click, and click **delete** to stop the packet capture</span></span>

![A csomagrögzítéssel törlése][3]

> [!NOTE]
> <span data-ttu-id="08d75-171">A csomagrögzítéssel törlése nem érinti a tárfiókban lévő fájlt.</span><span class="sxs-lookup"><span data-stu-id="08d75-171">Deleting a packet capture does not delete the file in the storage account.</span></span>

<span data-ttu-id="08d75-172">A rendszer felkéri a jóváhagyásához a csomagrögzítéssel törléséhez kattintson **Igen**</span><span class="sxs-lookup"><span data-stu-id="08d75-172">You are asked to confirm you want to delete the packet capture, click **Yes**</span></span>

![Jóváhagyás][4]

## <a name="stop-a-packet-capture"></a><span data-ttu-id="08d75-174">A csomagrögzítéssel leállítása</span><span class="sxs-lookup"><span data-stu-id="08d75-174">Stop a packet capture</span></span>

<span data-ttu-id="08d75-175">A csomag rögzítési nézetben kattintson a **helyi menü** (...), vagy kattintson a jobb gombbal, majd kattintson **leállítása** leállítja a csomagrögzítéssel</span><span class="sxs-lookup"><span data-stu-id="08d75-175">In the packet capture view, click the **context menu** (...) or right click, and click **Stop** to stop the packet capture</span></span>

## <a name="download-a-packet-capture"></a><span data-ttu-id="08d75-176">A csomagrögzítéssel letöltése</span><span class="sxs-lookup"><span data-stu-id="08d75-176">Download a packet capture</span></span>

<span data-ttu-id="08d75-177">A csomag rögzítési munkamenet befejezése után a rögzítési fájl feltöltése a blob storage vagy a virtuális gép helyi fájlba.</span><span class="sxs-lookup"><span data-stu-id="08d75-177">Once your packet capture session has completed, the capture file is uploaded to blob storage or to a local file on the VM.</span></span> <span data-ttu-id="08d75-178">A tárolási helye a csomagrögzítéssel definiálása a munkamenet létrehozását.</span><span class="sxs-lookup"><span data-stu-id="08d75-178">The storage location of the packet capture is defined at creation of the session.</span></span> <span data-ttu-id="08d75-179">Eszköz eléréséhez rögzítési-fájlokat egy tárfiókkal a Microsoft Azure Tártallózó, amely innen tölthető le: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="08d75-179">A convenient tool to access these capture files saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="08d75-180">Ha egy tárfiókot meg van adva, csomag rögzítési fájlok kerülnek a storage-fiókok a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="08d75-180">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>
```
https://{storageAccountName}.blob.core.windows.net/network-watcher-logs/subscriptions/{subscriptionId}/resourcegroups/{storageAccountResourceGroup}/providers/microsoft.compute/virtualmachines/{VMName}/{year}/{month}/{day}/packetCapture_{creationTime}.cap
```

## <a name="next-steps"></a><span data-ttu-id="08d75-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08d75-181">Next steps</span></span>

<span data-ttu-id="08d75-182">Csomag rögzíti a virtuális gép a riasztások megtekintésével automatizálása [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="08d75-182">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="08d75-183">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="08d75-183">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-packet-capture-manage-portal/figure1.png
[2]: ./media/network-watcher-packet-capture-manage-portal/figure2.png
[3]: ./media/network-watcher-packet-capture-manage-portal/figure3.png
[4]: ./media/network-watcher-packet-capture-manage-portal/figure4.png
[agent]: ./media/network-watcher-packet-capture-manage-portal/agent.png













