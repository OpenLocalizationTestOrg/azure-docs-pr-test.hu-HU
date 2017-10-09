---
title: "aaaTroubleshoot Azure virtuális hálózati átjáró és a kapcsolatok - PowerShell |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse hello Azure hálózati figyelőt hibaelhárítása a PowerShell-parancsmag"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: bd568d34091209390c57d22475559bdb99ad2c59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="ac3b2-103">Virtuális hálózati átjáró és az Azure hálózati figyelő PowerShell kapcsolatok hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="ac3b2-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="ac3b2-104">Portál</span><span class="sxs-lookup"><span data-stu-id="ac3b2-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="ac3b2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ac3b2-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="ac3b2-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="ac3b2-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="ac3b2-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ac3b2-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="ac3b2-108">REST API</span><span class="sxs-lookup"><span data-stu-id="ac3b2-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="ac3b2-109">Hálózati figyelőt számos lényeges képességét biztosítja, felügyeletében toounderstanding a hálózati erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="ac3b2-110">Ezek a képességek egyik erőforrás hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="ac3b2-111">Erőforrások hibaelhárítása hívható hello portál, a PowerShell, a CLI vagy a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="ac3b2-112">Meghívásakor, a hálózati figyelőt megvizsgálja a virtuális hálózati átjáró vagy a kapcsolat hello állapotát, és adja vissza az eredményekről.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ac3b2-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ac3b2-113">Before you begin</span></span>

<span data-ttu-id="ac3b2-114">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="ac3b2-115">Látogasson el a támogatott átjáró típusok, listája [támogatott átjárótípusok](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="ac3b2-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="ac3b2-116">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ac3b2-116">Overview</span></span>

<span data-ttu-id="ac3b2-117">Erőforrás hibakeresési hello lehetőséget nyújt a virtuális hálózati átjárók és kapcsolatok felmerülő problémák hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-117">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="ac3b2-118">A kérelem tooresource hibaelhárítása, amikor folyamatban van-e a naplók lekérdezett és megvizsgálni.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-118">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="ac3b2-119">Ha vizsgálat befejeződött, hello eredmény akkor minősül.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-119">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="ac3b2-120">Erőforrás-hibaelhárítási kérelmek hosszú ideig futniuk kér, ami eltarthat több percig tooreturn eredményt.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-120">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="ac3b2-121">a hibaelhárítás hello naplófájljainak tárolása a megadott tárfiók egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-121">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="troubleshoot-a-gateway-or-connection"></a><span data-ttu-id="ac3b2-122">Átjáró vagy csatlakozási hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="ac3b2-122">Troubleshoot a gateway or connection</span></span>

1. <span data-ttu-id="ac3b2-123">Keresse meg a toohello [Azure-portálon](https://portal.azure.com) kattintson **hálózati** > **hálózati figyelőt** > **VPN diagnosztika**</span><span class="sxs-lookup"><span data-stu-id="ac3b2-123">Navigate toohello [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **VPN Diagnostics**</span></span>
2. <span data-ttu-id="ac3b2-124">Válassza ki a **előfizetés**, **erőforráscsoport**, és **hely**.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-124">Select a **Subscription**, **Resource Group**, and **Location**.</span></span>
3. <span data-ttu-id="ac3b2-125">Erőforrás hibakeresési hello hello erőforrás állapotának adatait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-125">Resource troubleshooting returns data about hello health of hello resource.</span></span> <span data-ttu-id="ac3b2-126">Menti a megfelelő naplók tooa tárfiók toobe felül is.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-126">It also saves relevant logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="ac3b2-127">Kattintson a **Tárfiók** tooselect egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-127">Click **Storage Account** tooselect a storage account.</span></span>
4. <span data-ttu-id="ac3b2-128">A hello **tárfiókok** panelen válasszon ki egy meglévő tárfiókot használ, vagy kattintson **+ tárfiók** toocreate egy újat.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-128">On hello **Storage accounts** blade, select an existing storage account or click **+ Storage account** toocreate a new one.</span></span>
5. <span data-ttu-id="ac3b2-129">A hello **tárolók** panelen válasszon ki egy meglévő tárolót, vagy kattintson **+ tároló** toocreate egy új tároló.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-129">On hello **Containers** blade, choose an existing container or click **+ Container** toocreate a new container.</span></span> <span data-ttu-id="ac3b2-130">Ha végzett a kattintson **kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="ac3b2-130">When complete click **Select**</span></span>
6. <span data-ttu-id="ac3b2-131">Válassza ki a hello átjáró és a kapcsolat erőforrások tootroubleshoot, és kattintson a **hibaelhárítás indítása**</span><span class="sxs-lookup"><span data-stu-id="ac3b2-131">Select hello Gateway and Connection resources tootroubleshoot and click **Start Troubleshooting**</span></span>

<span data-ttu-id="ac3b2-132">Ha több erőforrás van kiválasztva, hibaelhárítási van egyidejű futtatását átjáró-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-132">If multiple resources are selected, troubleshooting is run concurrently on gateway resources.</span></span> <span data-ttu-id="ac3b2-133">Nem futtatható a kapcsolat és a hozzá társított hello átjáró ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-133">It can not run on a connection and it's associated gateway at hello same time.</span></span> <span data-ttu-id="ac3b2-134">Ajánlott tootroubleshoot átjárók először, mert a kapcsolat hibaelhárítási hosszabb folyamat.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-134">It is recommended tootroubleshoot gateways first as connection troubleshooting is a longer process.</span></span> <span data-ttu-id="ac3b2-135">Az erőforrás-hello VPN diagnosztika futtatása közben **HIBAELHÁRÍTÁSI állapot** oszlop megjelenik egy állapotának **fut**</span><span class="sxs-lookup"><span data-stu-id="ac3b2-135">While VPN Diagnostics is running on a resource hello **TROUBLESHOOTING STATUS** column will show a status of **Running**</span></span>

<span data-ttu-id="ac3b2-136">Amikor végzett, hello állapotmódosítások oszlop túl**kifogástalan** vagy **nem kifogástalan állapotú**.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-136">When complete, hello status column changes too**Healthy** or **Unhealthy**.</span></span>

![teljes hibaelhárítása][2]

## <a name="understanding-hello-results"></a><span data-ttu-id="ac3b2-138">Hello eredmények ismertetése</span><span class="sxs-lookup"><span data-stu-id="ac3b2-138">Understanding hello results</span></span>

<span data-ttu-id="ac3b2-139">A hello **részletek** szakasz hello ablak hello **állapot** lapon állapotát jeleníti meg hello hello utolsó hibaelhárítási futtatási kijelölt hello erőforráson.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-139">In hello **Details** section of hello window, hello **Status** tab shows hello status of hello last troubleshooting run on hello selected resource.</span></span> <span data-ttu-id="ac3b2-140">Hello legújabb diagnosztikai eredmények látható xx perc után utolsó Futtatás hello.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-140">Results of hello latest diagnostic are shown xx minutes after hello last run.</span></span>

|<span data-ttu-id="ac3b2-141">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ac3b2-141">Property</span></span>  |<span data-ttu-id="ac3b2-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="ac3b2-142">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="ac3b2-143">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="ac3b2-143">Resource</span></span>     | <span data-ttu-id="ac3b2-144">Hivatkozás toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-144">A link toohello resource.</span></span>        |
|<span data-ttu-id="ac3b2-145">Tárolási elérési útja</span><span class="sxs-lookup"><span data-stu-id="ac3b2-145">Storage path</span></span>     |  <span data-ttu-id="ac3b2-146">Elérési út toohello tárfiók és tároló, amely tartalmazza a hello naplók (Ha bármelyik keletkezett hello futtatása során).</span><span class="sxs-lookup"><span data-stu-id="ac3b2-146">Path toohello storage account and container that contain hello logs (if any were produced during hello run).</span></span> <span data-ttu-id="ac3b2-147">Ez a beállítás nem maradnak, ha kilép a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-147">This setting does not persist after you leave hello portal.</span></span>        |
|<span data-ttu-id="ac3b2-148">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ac3b2-148">Summary</span></span>     | <span data-ttu-id="ac3b2-149">Hello erőforrás állapotának összegzését.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-149">Summary of hello resource health.</span></span>        |
|<span data-ttu-id="ac3b2-150">Részletek</span><span class="sxs-lookup"><span data-stu-id="ac3b2-150">Detail</span></span>     | <span data-ttu-id="ac3b2-151">Hello erőforrás állapota részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-151">Detailed information on hello resource health.</span></span>        |
|<span data-ttu-id="ac3b2-152">Legutóbbi futtatás</span><span class="sxs-lookup"><span data-stu-id="ac3b2-152">Last run</span></span>     | <span data-ttu-id="ac3b2-153">utolsó hello idő hello idejű hibaelhárításhoz lett futott.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-153">hello time hello last time troubleshooting was ran.</span></span>        |


<span data-ttu-id="ac3b2-154">Hello **művelet** lapon általános útmutatást biztosít a hogyan tooresolve hello probléma.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-154">hello **Action** tab provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="ac3b2-155">Is művelet az hello probléma, ha egy hivatkozás által biztosított további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-155">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="ac3b2-156">Hello esetében nincs további útmutatás, ahol hello választ biztosít hello URL-cím tooopen támogatási esetet.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-156">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="ac3b2-157">Hello választ, és mi tartozik hello tulajdonságainak kapcsolatos további információkért látogasson el [hálózati figyelő hibaelhárítása – áttekintés](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="ac3b2-157">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>


## <a name="next-steps"></a><span data-ttu-id="ac3b2-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac3b2-158">Next steps</span></span>

<span data-ttu-id="ac3b2-159">Ha a beállítások módosítása, hogy stop VPN-kapcsolatot, lásd: [hálózati biztonsági csoportok kezelése](../virtual-network/virtual-network-manage-nsg-arm-portal.md) hello hálózati biztonsági csoport és a biztonsági szabályokat, amelyek lehet, hogy a szóban forgó tootrack.</span><span class="sxs-lookup"><span data-stu-id="ac3b2-159">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
