---
title: "Azure hálózati figyelőt - Azure-portálon aaaCheck kapcsolatot |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse kapcsolatot ellenőrzi a hálózati figyelőt hello Azure-portál használatával"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2017
ms.author: gwallace
ms.openlocfilehash: ef6ecccd688f06f70003a5b59771c15bcbe8f3e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-hello-azure-portal"></a><span data-ttu-id="e606e-103">Ellenőrizze a kapcsolatot az Azure hálózati figyelőt hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="e606e-103">Check connectivity with Azure Network Watcher using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e606e-104">Portál</span><span class="sxs-lookup"><span data-stu-id="e606e-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="e606e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e606e-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="e606e-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e606e-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="e606e-107">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="e606e-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="e606e-108">Ismerje meg, hogyan hozhatók létre a toouse kapcsolat tooverify, ha egy virtuális gép tooa megadott végpont a közvetlen TCP-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="e606e-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="e606e-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e606e-109">Before you begin</span></span>

<span data-ttu-id="e606e-110">Ez a cikk feltételezi, hogy rendelkezik-e a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="e606e-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="e606e-111">Egy példány toocheck kapcsolat kívánt hálózati figyelőt hello régióban.</span><span class="sxs-lookup"><span data-stu-id="e606e-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="e606e-112">Virtuális gépek toocheck kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="e606e-112">Virtual machines toocheck connectivity with.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e606e-113">Kapcsolat ellenőrzése van szükség a virtuálisgép-bővítmény `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="e606e-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="e606e-114">A virtuális gép Windows hello-bővítmény telepítése a Microsoft [a Windows Azure hálózati figyelő ügynök virtuálisgép-bővítmény](../virtual-machines/windows/extensions-nwa.md) és a Linux virtuális gép helyezést [Azure hálózati figyelő ügynök virtuálisgép-bővítmény Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="e606e-114">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="e606e-115">Ellenőrizze a kapcsolat tooa virtuális gép</span><span class="sxs-lookup"><span data-stu-id="e606e-115">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="e606e-116">Ebben a példában kapcsolat tooa cél virtuális gép ellenőrzi a 80-as porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e606e-116">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

<span data-ttu-id="e606e-117">Nyissa meg a tooyour hálózati figyelőt, és kattintson a **kapcsolat ellenőrzése (előzetes verzió)**.</span><span class="sxs-lookup"><span data-stu-id="e606e-117">Navigate tooyour Network Watcher and click **Connectivity check (Preview)**.</span></span> <span data-ttu-id="e606e-118">Válassza ki a hello virtuális gép toocheck közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="e606e-118">Select hello virtual machine toocheck connectivity from.</span></span> <span data-ttu-id="e606e-119">A hello **cél** rész válassza **válasszon ki egy virtuális gépet** és hello megfelelő virtuális gép és a port tootest kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="e606e-119">In hello **Destination** section choose **Select a virtual machine** and choose hello correct virtual machine and port tootest.</span></span>

<span data-ttu-id="e606e-120">Miután rákattintott **ellenőrizze**, megadott hello port hello virtuális gépek közötti hello kapcsolatot ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="e606e-120">Once you click **Check**, hello connectivity between hello virtual machines on hello port specified are checked.</span></span> <span data-ttu-id="e606e-121">Hello példában hello cél virtuális gép nem érhető el, útválasztók ugrásainak listája látható.</span><span class="sxs-lookup"><span data-stu-id="e606e-121">In hello example, hello destination VM is unreachable, a listing of hops are shown.</span></span>

![A virtuális gép kapcsolat eredmények ellenőrzése][1]

## <a name="check-remote-endpoint-connectivity"></a><span data-ttu-id="e606e-123">Ellenőrizze a kapcsolatot a távoli végpont</span><span class="sxs-lookup"><span data-stu-id="e606e-123">Check remote endpoint connectivity</span></span>

<span data-ttu-id="e606e-124">toocheck hello kapcsolat és a késés tooa távoli végpont, válassza a hello **adja meg manuálisan** hello választógomb **cél** szakaszt, bemeneti hello URL-cím és hello port, és kattintson **ellenőrzése** .</span><span class="sxs-lookup"><span data-stu-id="e606e-124">toocheck hello connectivity and latency tooa remote endpoint, choose hello **Specify manually** radio button in hello **Destination** section, input hello url and hello port and click **Check**.</span></span>  <span data-ttu-id="e606e-125">Például a webhelyek és a tárolási végpontok távoli végpontok szolgál.</span><span class="sxs-lookup"><span data-stu-id="e606e-125">This is used for remote endpoints like websites and storage endpoints.</span></span>

![A webhely csatlakozási eredmények ellenőrzése][2]

## <a name="next-steps"></a><span data-ttu-id="e606e-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e606e-127">Next steps</span></span>

<span data-ttu-id="e606e-128">Ismerje meg, hogyan tooautomate csomagrögzítéseket virtuális gép riasztások megtekintésével [riasztási kiváltott csomagrögzítéssel létrehozása](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="e606e-128">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="e606e-129">Keresése, ha bizonyos adatforgalom engedélyezett a virtuális gép kívül vagy belül ellátogatva [ellenőrizze IP folyamat ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="e606e-129">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

[1]: ./media/network-watcher-connectivity-portal/figure1.png
[2]: ./media/network-watcher-connectivity-portal/figure2.png
