---
title: "az Azure hálózati figyelőt aaaIntroduction tooPacket rögzítési |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt csomag rögzítése funkció áttekintése"
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
ms.openlocfilehash: 2ce01b391b9c1a1c19aa29c8620628c55586df03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toovariable-packet-capture-in-azure-network-watcher"></a><span data-ttu-id="dda2a-103">Bevezetés toovariable csomagrögzítéssel Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="dda2a-103">Introduction toovariable packet capture in Azure Network Watcher</span></span>

<span data-ttu-id="dda2a-104">Hálózati figyelő változó csomagrögzítéssel lehetővé teszi a virtuális gép toocreate csomag rögzítési munkamenetek tootrack forgalom tooand.</span><span class="sxs-lookup"><span data-stu-id="dda2a-104">Network Watcher variable packet capture allows you toocreate packet capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="dda2a-105">Csomagrögzítéssel segít toodiagnose hálózati rendellenességeket mindkét reaktív és proactivity.</span><span class="sxs-lookup"><span data-stu-id="dda2a-105">Packet capture helps toodiagnose network anomalies both reactively and proactivity.</span></span> <span data-ttu-id="dda2a-106">Egyéb felhasználásra tartalmazzák a hálózati statisztikákat, való információk hálózati behatolások, toodebug ügyfél-kiszolgáló közötti kommunikációt, és még sok más összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="dda2a-106">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span>

<span data-ttu-id="dda2a-107">Csomagrögzítéssel a virtuálisgép-bővítmény hálózati figyelőt keresztül távolról elindult.</span><span class="sxs-lookup"><span data-stu-id="dda2a-107">Packet capture is a virtual machine extension that is remotely started through Network Watcher.</span></span> <span data-ttu-id="dda2a-108">Ez a funkció megkönnyíti a hello okozta terheket egy csomagrögzítéssel manuálisan virtuális gépen futó hello szükséges, amely értékes időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="dda2a-108">This capability eases hello burden of running a packet capture manually on hello desired virtual machine, which saves valuable time.</span></span> <span data-ttu-id="dda2a-109">Csomagrögzítéssel hello portál, a PowerShell, a CLI vagy a REST API-n keresztül is elindítható.</span><span class="sxs-lookup"><span data-stu-id="dda2a-109">Packet capture can be triggered through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="dda2a-110">Például hogyan csomagrögzítéssel is elindítható a virtuális gép a riasztások van.</span><span class="sxs-lookup"><span data-stu-id="dda2a-110">One example of how packet capture can be triggered is with Virtual Machine alerts.</span></span> <span data-ttu-id="dda2a-111">Szűrők a következők: megadott hello rögzítési munkamenet tooensure a kívánt toomonitor forgalom rögzítése.</span><span class="sxs-lookup"><span data-stu-id="dda2a-111">Filters are provided for hello capture session tooensure you capture traffic you want toomonitor.</span></span> <span data-ttu-id="dda2a-112">5 rekordos (protokoll, helyi IP-cím, távoli IP-cím, helyi port és távoli port) alapuló szűrők információkat.</span><span class="sxs-lookup"><span data-stu-id="dda2a-112">Filters are based on 5-tuple (protocol, local IP address, remote IP address, local port, and remote port) information.</span></span> <span data-ttu-id="dda2a-113">hello rögzített adatok hello helyi lemez vagy egy tárolási blob tárolja.</span><span class="sxs-lookup"><span data-stu-id="dda2a-113">hello captured data is stored in hello local disk or a storage blob.</span></span> <span data-ttu-id="dda2a-114">A 10 csomag rögzítési munkamenetek régiónként korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="dda2a-114">There is a limit of 10 packet capture sessions per region per subscription.</span></span> <span data-ttu-id="dda2a-115">Ezt a határt csak toohello munkamenetek vonatkozik, és nem felel meg a csomag rögzítési fájlok mentése helyileg hello VM vagy a storage-fiók toohello.</span><span class="sxs-lookup"><span data-stu-id="dda2a-115">This limit applies only toohello sessions and does not apply toohello saved packet capture files either locally on hello VM or in a storage account.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dda2a-116">Csomagrögzítéssel van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="dda2a-116">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="dda2a-117">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="dda2a-117">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

<span data-ttu-id="dda2a-118">tooreduce hello információkat rögzíti a tooonly hello információt csomag rögzítési munkamenet hello alábbi beállítások érhetők el:</span><span class="sxs-lookup"><span data-stu-id="dda2a-118">tooreduce hello information you capture tooonly hello information you want, hello following options are available for a packet capture session:</span></span>

<span data-ttu-id="dda2a-119">**Konfigurációjának rögzítése**</span><span class="sxs-lookup"><span data-stu-id="dda2a-119">**Capture configuration**</span></span>

|<span data-ttu-id="dda2a-120">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="dda2a-120">Property</span></span>|<span data-ttu-id="dda2a-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="dda2a-121">Description</span></span>|
|---|---|
|<span data-ttu-id="dda2a-122">**Maximális bájtokat csomagot (bájt)**</span><span class="sxs-lookup"><span data-stu-id="dda2a-122">**Maximum bytes per packet (bytes)**</span></span> | <span data-ttu-id="dda2a-123">szám hello bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.</span><span class="sxs-lookup"><span data-stu-id="dda2a-123">hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="dda2a-124">szám hello bájtok számát az egyes csomagok, amelyek rögzítve lesznek, az összes bájt rofilok rögzítésének, ha üresen hagyja.</span><span class="sxs-lookup"><span data-stu-id="dda2a-124">hello number of bytes from each packet that are captured, all bytes are captured if left blank.</span></span> <span data-ttu-id="dda2a-125">Ha csak hello IPv4-fejléc – kell jelző 34 Itt</span><span class="sxs-lookup"><span data-stu-id="dda2a-125">If you need only hello IPv4 header – indicate 34 here</span></span> |
|<span data-ttu-id="dda2a-126">**Maximális bájtokat munkamenet (bájt)**</span><span class="sxs-lookup"><span data-stu-id="dda2a-126">**Maximum bytes per session (bytes)**</span></span> | <span data-ttu-id="dda2a-127">Az adott bájtok teljes száma a rendszer rögzíti, hello érték elérésekor hello munkamenete véget nem ér.</span><span class="sxs-lookup"><span data-stu-id="dda2a-127">Total number of bytes in that are captured, once hello value is reached hello session ends.</span></span>|
|<span data-ttu-id="dda2a-128">**Időkorlát (másodperc)**</span><span class="sxs-lookup"><span data-stu-id="dda2a-128">**Time limit (seconds)**</span></span> | <span data-ttu-id="dda2a-129">Készletek hello csomag időkorlát munkamenet rögzítése.</span><span class="sxs-lookup"><span data-stu-id="dda2a-129">Sets a time constraint on hello packet capture session.</span></span> <span data-ttu-id="dda2a-130">hello alapérték 18000 másodperceken vagy 5 óra.</span><span class="sxs-lookup"><span data-stu-id="dda2a-130">hello default value is 18000 seconds or 5 hours.</span></span>|

<span data-ttu-id="dda2a-131">**Szűrés (nem kötelező)**</span><span class="sxs-lookup"><span data-stu-id="dda2a-131">**Filtering (optional)**</span></span>

|<span data-ttu-id="dda2a-132">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="dda2a-132">Property</span></span>|<span data-ttu-id="dda2a-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="dda2a-133">Description</span></span>|
|---|---|
|<span data-ttu-id="dda2a-134">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="dda2a-134">**Protocol**</span></span> | <span data-ttu-id="dda2a-135">hello protokoll toofilter hello csomagok rögzíti.</span><span class="sxs-lookup"><span data-stu-id="dda2a-135">hello protocol toofilter for hello packet capture.</span></span> <span data-ttu-id="dda2a-136">hello elérhető értékek a következők: TCP és UDP összes.</span><span class="sxs-lookup"><span data-stu-id="dda2a-136">hello available values are TCP, UDP, and All.</span></span>|
|<span data-ttu-id="dda2a-137">**Helyi IP-cím**</span><span class="sxs-lookup"><span data-stu-id="dda2a-137">**Local IP address**</span></span> | <span data-ttu-id="dda2a-138">Ez az érték szűrők hello csomagok rögzítési toopackets, ahol hello helyi IP-cím megegyezik-e a szűrő.</span><span class="sxs-lookup"><span data-stu-id="dda2a-138">This value filters hello packet capture toopackets where hello local IP address matches this filter value.</span></span>|
|<span data-ttu-id="dda2a-139">**Helyi port**</span><span class="sxs-lookup"><span data-stu-id="dda2a-139">**Local port**</span></span> | <span data-ttu-id="dda2a-140">Az érték szűrők hello csomag ahol hello helyi port megegyezik-e a szűrő toopackets rögzíti.</span><span class="sxs-lookup"><span data-stu-id="dda2a-140">This value filters hello packet capture toopackets where hello local port matches this filter value.</span></span>|
|<span data-ttu-id="dda2a-141">**Távoli IP-cím**</span><span class="sxs-lookup"><span data-stu-id="dda2a-141">**Remote IP address**</span></span> | <span data-ttu-id="dda2a-142">Az érték szűrők hello csomag ahol hello távoli IP megegyezik-e a szűrő toopackets rögzíti.</span><span class="sxs-lookup"><span data-stu-id="dda2a-142">This value filters hello packet capture toopackets where hello remote IP matches this filter value.</span></span>|
|<span data-ttu-id="dda2a-143">**Távoli port**</span><span class="sxs-lookup"><span data-stu-id="dda2a-143">**Remote port**</span></span> | <span data-ttu-id="dda2a-144">Az érték szűrők hello csomag ahol hello távoli port megegyezik-e a szűrő toopackets rögzíti.</span><span class="sxs-lookup"><span data-stu-id="dda2a-144">This value filters hello packet capture toopackets where hello remote port matches this filter value.</span></span>|

### <a name="next-steps"></a><span data-ttu-id="dda2a-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dda2a-145">Next steps</span></span>

<span data-ttu-id="dda2a-146">Megtudhatja, hogyan kezelheti csomag rögzítésekre hello portálon keresztül ellátogatva [kezelése az Azure-portálon hello csomagrögzítéssel](network-watcher-packet-capture-manage-portal.md) vagy a PowerShell-lel ellátogatva [csomag rögzítése kezelése a PowerShell használatával](network-watcher-packet-capture-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="dda2a-146">Learn how you can manage packet captures through hello portal by visiting [Manage packet capture in hello Azure portal](network-watcher-packet-capture-manage-portal.md) or with PowerShell by visiting [Manage Packet Capture with PowerShell](network-watcher-packet-capture-manage-powershell.md).</span></span>

<span data-ttu-id="dda2a-147">Megtudhatja, hogyan toocreate proaktív csomag rögzíti látogasson el a virtuális gép riasztásain alapuló [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="dda2a-147">Learn how toocreate proactive packet captures based on virtual machine alerts by visiting [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-packet-capture-overview/figure1.png













