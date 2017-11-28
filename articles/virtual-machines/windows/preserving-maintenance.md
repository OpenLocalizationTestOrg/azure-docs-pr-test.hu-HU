---
title: "Virtuális gép megőrzi az karbantartási Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "A helyszíni virtuális gép áttelepítés megőrzi az frissítések memória."
services: virtual-machines-windows
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: 8888bafbc3aba24168312b611a9b4fbde25f376d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a><span data-ttu-id="48f85-103">Virtuális gép megőrzi az karbantartási a helyszíni virtuális gép áttelepítése</span><span class="sxs-lookup"><span data-stu-id="48f85-103">VM preserving maintenance with In-place VM migration</span></span>

<span data-ttu-id="48f85-104">Amíg frissítések többségét üzemeltetett virtuális gépek nincs hatása, vannak esetek, ahol az összetevők vagy a szolgáltatások frissítéseket futó virtuális gépek (nélkül a virtuális gép teljes újraindítás) minimális zavaró tényező eredményez.</span><span class="sxs-lookup"><span data-stu-id="48f85-104">While the majority of updates have no impact to hosted VMs, there are cases where updates to components or services result in minimal interference to running VMs (without a full reboot of the virtual machine).</span></span>

<span data-ttu-id="48f85-105">Ezeket a frissítéseket, amely lehetővé teszi a helyszíni élő áttelepítés, más néven "memória-megőrzi az update" technológiával történik.</span><span class="sxs-lookup"><span data-stu-id="48f85-105">These updates are accomplished with technology that enables in-place live migration, also called "memory-preserving update".</span></span> <span data-ttu-id="48f85-106">Amikor frissíti a gazdagép, a virtuális gép el van helyezve egy "felfüggesztett" állapotba kerül, RAM, a memória megőrzi, amíg az üzemeltetési környezet (pl. az alapul szolgáló operációs rendszert) alkalmazza a szükséges frissítések és javítások.</span><span class="sxs-lookup"><span data-stu-id="48f85-106">When updating the host, the virtual machine is placed into a “paused” state, preserving the memory in RAM, while the hosting environment (e.g. the underlying operating system) applies the necessary updates and patches.</span></span>
<span data-ttu-id="48f85-107">A virtuális gép majd folytatódik a felfüggesztés 30 másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="48f85-107">The virtual machine is then resumed within 30 seconds of being paused.</span></span>
<span data-ttu-id="48f85-108">A virtuális gép órája a folytatás után automatikusan szinkronizálódik.</span><span class="sxs-lookup"><span data-stu-id="48f85-108">After resuming, the clock of the virtual machine is automatically synchronized.</span></span>

<span data-ttu-id="48f85-109">Nem minden frissítés helyezhető üzembe ezzel a mechanizmussal, de a rövid felfüggesztési időszak miatt a frissítések ilyen telepítése nagymértékben csökkenti a virtuális gépekre gyakorolt hatást.</span><span class="sxs-lookup"><span data-stu-id="48f85-109">Not all updates can be deployed by using this mechanism, but given the short pause period, deploying updates in this way greatly reduces impact to virtual machines.</span></span>

<span data-ttu-id="48f85-110">Többpéldányos (virtuális gépek rendelkezésre állási csoportba) frissítései alkalmazott frissítési tartományok egyszerre.</span><span class="sxs-lookup"><span data-stu-id="48f85-110">Multi-instance updates (VMs in an availability set) are applied one update domain at a time.</span></span>

<span data-ttu-id="48f85-111">Egyes alkalmazások negatív hatással lehet több, mint a többire ezeket a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="48f85-111">Some applications may be impacted by these updates more than others.</span></span> <span data-ttu-id="48f85-112">Valós idejű esemény feldolgozása, médiaadatfolyam vagy átkódolás vagy hálózati forgatókönyvek, magas teljesítmény végző alkalmazások például előfordulhat, hogy nem kell megtervezni, hogy egy 30 másodperces szünet működését.</span><span class="sxs-lookup"><span data-stu-id="48f85-112">Applications that perform real-time event processing, media streaming or transcoding, or high throughput networking scenarios, for example, may not be designed to tolerate a 30 second pause.</span></span> <span data-ttu-id="48f85-113">A virtuális gépen futó alkalmazások jövőbeli frissítések többet is megtudhat meghívásával a [ütemezett események](../virtual-machines-scheduled-events.md) API-ja a [Azure metaadat-szolgáltatás](../virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="48f85-113">Applications running in a virtual machine can learn about upcoming updates by calling the [Scheduled Events](../virtual-machines-scheduled-events.md) API of the [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span></span>
