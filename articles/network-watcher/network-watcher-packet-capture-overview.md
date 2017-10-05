---
title: "Az Azure hálózati figyelőt csomagrögzítéssel bemutatása |} Microsoft Docs"
description: "Ezen a lapon a hálózati figyelőt csomag rögzítése funkció áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3a81afaa-ecd9-4004-b68e-69ab56913356
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4fdd007c2cfad7b42f26ab2cacfba06d95c8dad3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-variable-packet-capture-in-azure-network-watcher"></a><span data-ttu-id="714ac-103">Az Azure hálózati figyelőt változó csomagrögzítéssel bemutatása</span><span class="sxs-lookup"><span data-stu-id="714ac-103">Introduction to variable packet capture in Azure Network Watcher</span></span>

<span data-ttu-id="714ac-104">Hálózati figyelő változó csomagrögzítéssel csomag rögzítési munkamenetek nyomon követéséhez forgalma és a virtuális gép létrehozását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="714ac-104">Network Watcher variable packet capture allows you to create packet capture sessions to track traffic to and from a virtual machine.</span></span> <span data-ttu-id="714ac-105">Csomag rögzítési segítségével mindkét hálózati rendellenességeket reaktív diagnosztizálásához és proactivity.</span><span class="sxs-lookup"><span data-stu-id="714ac-105">Packet capture helps to diagnose network anomalies both reactively and proactivity.</span></span> <span data-ttu-id="714ac-106">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, hálózati behatolások, ügyfél-kiszolgáló közötti kommunikációt, és még sok más hibakeresési információkat való összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="714ac-106">Other uses include gathering network statistics, gaining information on network intrusions, to debug client-server communications and much more.</span></span>

<span data-ttu-id="714ac-107">Csomagrögzítéssel a virtuálisgép-bővítmény hálózati figyelőt keresztül távolról elindult.</span><span class="sxs-lookup"><span data-stu-id="714ac-107">Packet capture is a virtual machine extension that is remotely started through Network Watcher.</span></span> <span data-ttu-id="714ac-108">Ez a funkció megkönnyíti a csomagrögzítéssel manuálisan futó a használni kívánt virtuális gépet, amely értékes időt takaríthat meg okozta terheket.</span><span class="sxs-lookup"><span data-stu-id="714ac-108">This capability eases the burden of running a packet capture manually on the desired virtual machine, which saves valuable time.</span></span> <span data-ttu-id="714ac-109">A portál, a PowerShell, a parancssori felület vagy a REST API-n keresztül is elindítható a csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="714ac-109">Packet capture can be triggered through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="714ac-110">Például hogyan csomagrögzítéssel is elindítható a virtuális gép a riasztások van.</span><span class="sxs-lookup"><span data-stu-id="714ac-110">One example of how packet capture can be triggered is with Virtual Machine alerts.</span></span> <span data-ttu-id="714ac-111">A rögzítési munkamenet rögzíti a figyelni kívánt forgalom biztosításához szűrők célokat szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="714ac-111">Filters are provided for the capture session to ensure you capture traffic you want to monitor.</span></span> <span data-ttu-id="714ac-112">5 rekordos (protokoll, helyi IP-cím, távoli IP-cím, helyi port és távoli port) alapuló szűrők információkat.</span><span class="sxs-lookup"><span data-stu-id="714ac-112">Filters are based on 5-tuple (protocol, local IP address, remote IP address, local port, and remote port) information.</span></span> <span data-ttu-id="714ac-113">A rögzített adatok a helyi lemez vagy egy tárolási blob tárolja.</span><span class="sxs-lookup"><span data-stu-id="714ac-113">The captured data is stored in the local disk or a storage blob.</span></span> <span data-ttu-id="714ac-114">A 10 csomag rögzítési munkamenetek régiónként korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="714ac-114">There is a limit of 10 packet capture sessions per region per subscription.</span></span> <span data-ttu-id="714ac-115">Ezt a határt csak a munkamenetek vonatkozik, és nem vonatkozik a mentett csomag rögzítési fájlok helyileg a virtuális gép vagy egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="714ac-115">This limit applies only to the sessions and does not apply to the saved packet capture files either locally on the VM or in a storage account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="714ac-116">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="714ac-116">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="714ac-117">A bővítmény telepítése a Windows virtuális gép a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="714ac-117">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

<span data-ttu-id="714ac-118">Az információk csak a szükséges információkkal rögzítené csökkentése érdekében az alábbi beállítások érhetők el egy csomag rögzítési munkamenet:</span><span class="sxs-lookup"><span data-stu-id="714ac-118">To reduce the information you capture to only the information you want, the following options are available for a packet capture session:</span></span>

<span data-ttu-id="714ac-119">**Konfigurációjának rögzítése**</span><span class="sxs-lookup"><span data-stu-id="714ac-119">**Capture configuration**</span></span>

|<span data-ttu-id="714ac-120">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="714ac-120">Property</span></span>|<span data-ttu-id="714ac-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="714ac-121">Description</span></span>|
|---|---|
|<span data-ttu-id="714ac-122">**Maximális bájtokat csomagot (bájt)**</span><span class="sxs-lookup"><span data-stu-id="714ac-122">**Maximum bytes per packet (bytes)**</span></span> | <span data-ttu-id="714ac-123">A szám bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.</span><span class="sxs-lookup"><span data-stu-id="714ac-123">The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="714ac-124">A szám bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.</span><span class="sxs-lookup"><span data-stu-id="714ac-124">The number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="714ac-125">Ha csak az IPv4-fejléc – kell jelző 34 Itt</span><span class="sxs-lookup"><span data-stu-id="714ac-125">If you need only the IPv4 header – indicate 34 here</span></span> |
|<span data-ttu-id="714ac-126">**Maximális bájtokat munkamenet (bájt)**</span><span class="sxs-lookup"><span data-stu-id="714ac-126">**Maximum bytes per session (bytes)**</span></span> | <span data-ttu-id="714ac-127">Az adott bájtok teljes száma a rendszer rögzíti, az érték elérésekor a munkamenet véget ér.</span><span class="sxs-lookup"><span data-stu-id="714ac-127">Total number of bytes in that are captured, once the value is reached the session ends.</span></span>|
|<span data-ttu-id="714ac-128">**Időkorlát (másodperc)**</span><span class="sxs-lookup"><span data-stu-id="714ac-128">**Time limit (seconds)**</span></span> | <span data-ttu-id="714ac-129">Beállítja a csomag időkorlát munkamenet rögzítése.</span><span class="sxs-lookup"><span data-stu-id="714ac-129">Sets a time constraint on the packet capture session.</span></span> <span data-ttu-id="714ac-130">Az alapértelmezett érték 18000 másodperceken vagy 5 óra.</span><span class="sxs-lookup"><span data-stu-id="714ac-130">The default value is 18000 seconds or 5 hours.</span></span>|

<span data-ttu-id="714ac-131">**Szűrés (nem kötelező)**</span><span class="sxs-lookup"><span data-stu-id="714ac-131">**Filtering (optional)**</span></span>

|<span data-ttu-id="714ac-132">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="714ac-132">Property</span></span>|<span data-ttu-id="714ac-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="714ac-133">Description</span></span>|
|---|---|
|<span data-ttu-id="714ac-134">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="714ac-134">**Protocol**</span></span> | <span data-ttu-id="714ac-135">A csomagrögzítéssel szűrhet protokollt.</span><span class="sxs-lookup"><span data-stu-id="714ac-135">The protocol to filter for the packet capture.</span></span> <span data-ttu-id="714ac-136">A lehetséges értékek a következők: TCP és UDP összes.</span><span class="sxs-lookup"><span data-stu-id="714ac-136">The available values are TCP, UDP, and All.</span></span>|
|<span data-ttu-id="714ac-137">**Helyi IP-cím**</span><span class="sxs-lookup"><span data-stu-id="714ac-137">**Local IP address**</span></span> | <span data-ttu-id="714ac-138">Ez az érték a csomagrögzítéssel csomagok szűrése, ahol a helyi IP-cím megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="714ac-138">This value filters the packet capture to packets where the local IP address matches this filter value.</span></span>|
|<span data-ttu-id="714ac-139">**Helyi port**</span><span class="sxs-lookup"><span data-stu-id="714ac-139">**Local port**</span></span> | <span data-ttu-id="714ac-140">Ez az érték a csomagrögzítéssel csomagok szűrése, ahol a helyi port megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="714ac-140">This value filters the packet capture to packets where the local port matches this filter value.</span></span>|
|<span data-ttu-id="714ac-141">**Távoli IP-cím**</span><span class="sxs-lookup"><span data-stu-id="714ac-141">**Remote IP address**</span></span> | <span data-ttu-id="714ac-142">Ez az érték a csomagrögzítéssel csomagok szűrése, ahol az távoli IP-cím megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="714ac-142">This value filters the packet capture to packets where the remote IP matches this filter value.</span></span>|
|<span data-ttu-id="714ac-143">**Távoli port**</span><span class="sxs-lookup"><span data-stu-id="714ac-143">**Remote port**</span></span> | <span data-ttu-id="714ac-144">Ez az érték a csomagrögzítéssel csomagok szűrése, ahol a távoli port megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="714ac-144">This value filters the packet capture to packets where the remote port matches this filter value.</span></span>|

### <a name="next-steps"></a><span data-ttu-id="714ac-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="714ac-145">Next steps</span></span>

<span data-ttu-id="714ac-146">Megtudhatja, hogyan kezelheti a portálon keresztül csomag rögzítésekre ellátogatva [kezelése az Azure portálon csomagrögzítéssel](network-watcher-packet-capture-manage-portal.md) vagy a PowerShell-lel ellátogatva [csomag rögzítése kezelése a PowerShell-lel](network-watcher-packet-capture-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="714ac-146">Learn how you can manage packet captures through the portal by visiting [Manage packet capture in the Azure portal](network-watcher-packet-capture-manage-portal.md) or with PowerShell by visiting [Manage Packet Capture with PowerShell](network-watcher-packet-capture-manage-powershell.md).</span></span>

<span data-ttu-id="714ac-147">Látogasson el a virtuális gép riasztások alapján proaktív csomag rögzítésekre létrehozásával [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="714ac-147">Learn how to create proactive packet captures based on virtual machine alerts by visiting [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













