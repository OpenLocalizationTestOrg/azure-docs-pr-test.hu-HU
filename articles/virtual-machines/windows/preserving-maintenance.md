---
title: "aaa VM megóvása karbantartása a Windows-alapú virtuális gépek Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: b798f0afd9d8dc60ca8a78f7cc77435a0ddc76fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a><span data-ttu-id="35642-103">Virtuális gép megőrzi az karbantartási a helyszíni virtuális gép áttelepítése</span><span class="sxs-lookup"><span data-stu-id="35642-103">VM preserving maintenance with In-place VM migration</span></span>

<span data-ttu-id="35642-104">Míg a frissítések többségét hello nincs hatása toohosted virtuális gépeket, vannak esetek, ahol frissítések toocomponents vagy szolgáltatások eredményez-e a minimális zavaró tényező toorunning virtuális gépek (nélkül hello virtuális gép teljes újraindítás).</span><span class="sxs-lookup"><span data-stu-id="35642-104">While hello majority of updates have no impact toohosted VMs, there are cases where updates toocomponents or services result in minimal interference toorunning VMs (without a full reboot of hello virtual machine).</span></span>

<span data-ttu-id="35642-105">Ezeket a frissítéseket, amely lehetővé teszi a helyszíni élő áttelepítés, más néven "memória-megőrzi az update" technológiával történik.</span><span class="sxs-lookup"><span data-stu-id="35642-105">These updates are accomplished with technology that enables in-place live migration, also called "memory-preserving update".</span></span> <span data-ttu-id="35642-106">Hello állomás frissítésekor hello virtuális gép el van helyezve egy "felfüggesztett" állapotba kerül, RAM, hello memóriája megőrzi az üzemeltetési környezet (pl. az alapul szolgáló operációs rendszert) hello hello szükséges frissítések és javítások alkalmazása közben.</span><span class="sxs-lookup"><span data-stu-id="35642-106">When updating hello host, hello virtual machine is placed into a “paused” state, preserving hello memory in RAM, while hello hosting environment (e.g. the underlying operating system) applies hello necessary updates and patches.</span></span>
<span data-ttu-id="35642-107">hello virtuális gép majd folytatódik a felfüggesztés 30 másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="35642-107">hello virtual machine is then resumed within 30 seconds of being paused.</span></span>
<span data-ttu-id="35642-108">Folytatja a futtatását, miután a hello óra hello virtuális gép automatikus szinkronizálásának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="35642-108">After resuming, hello clock of hello virtual machine is automatically synchronized.</span></span>

<span data-ttu-id="35642-109">Nem minden frissítés is telepíthető a mechanizmussal, de a rövid késleltetés időszak alatt a megadott ezen frissítéseinek telepítéséhez módon jelentősen csökkenti a hatás toovirtual gépek.</span><span class="sxs-lookup"><span data-stu-id="35642-109">Not all updates can be deployed by using this mechanism, but given the short pause period, deploying updates in this way greatly reduces impact toovirtual machines.</span></span>

<span data-ttu-id="35642-110">Többpéldányos (virtuális gépek rendelkezésre állási csoportba) frissítései alkalmazott frissítési tartományok egyszerre.</span><span class="sxs-lookup"><span data-stu-id="35642-110">Multi-instance updates (VMs in an availability set) are applied one update domain at a time.</span></span>

<span data-ttu-id="35642-111">Egyes alkalmazások negatív hatással lehet több, mint a többire ezeket a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="35642-111">Some applications may be impacted by these updates more than others.</span></span> <span data-ttu-id="35642-112">Valós idejű esemény feldolgozása, médiaadatfolyam vagy átkódolás vagy hálózati forgatókönyvek, magas teljesítmény végző alkalmazások például nem lehet tervezett tootolerate egy 30 másodperces szünet.</span><span class="sxs-lookup"><span data-stu-id="35642-112">Applications that perform real-time event processing, media streaming or transcoding, or high throughput networking scenarios, for example, may not be designed tootolerate a 30 second pause.</span></span> <span data-ttu-id="35642-113">A virtuális gépen futó alkalmazások is információ a jövőbeli frissítések hívó hello [ütemezett események](../virtual-machines-scheduled-events.md) hello API-ja [Azure metaadat-szolgáltatás](../virtual-machines-instancemetadataservice-overview.md).</span><span class="sxs-lookup"><span data-stu-id="35642-113">Applications running in a virtual machine can learn about upcoming updates by calling hello [Scheduled Events](../virtual-machines-scheduled-events.md) API of hello [Azure Metadata Service](../virtual-machines-instancemetadataservice-overview.md).</span></span>
