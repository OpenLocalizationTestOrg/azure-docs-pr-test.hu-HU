---
title: "aaaCannot törli a virtuális hálózatot az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot hello probléma az Azure virtuális hálózat nem törölhető."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: a9050ab238ccb0380fd46130430222efb8f42388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-failed-toodelete-a-virtual-network-in-azure"></a><span data-ttu-id="4fb51-103">Hibáinak elhárítása: Egy virtuális hálózatot az Azure-ban toodelete nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="4fb51-103">Troubleshooting: Failed toodelete a virtual network in Azure</span></span>

<span data-ttu-id="4fb51-104">Hibák akkor fordulhat elő, amikor megpróbál toodelete a Microsoft Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="4fb51-104">You might receive errors when you try toodelete a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="4fb51-105">Ez a cikk ismerteti a hibaelhárítási lépéseket toohelp a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="4fb51-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a><span data-ttu-id="4fb51-106">Hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="4fb51-106">Troubleshooting guidance</span></span> 

1. <span data-ttu-id="4fb51-107">[Ellenőrizze, hogy a virtuális hálózati átjáró fut-e a virtuális hálózati hello](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="4fb51-107">[Check whether a virtual network gateway is running in hello virtual network](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span></span>
2. <span data-ttu-id="4fb51-108">[Ellenőrizze, hogy fut-e olyan átjárót hello virtuális hálózat](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="4fb51-108">[Check whether an application gateway is running in hello virtual network](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span></span>
3. <span data-ttu-id="4fb51-109">[Ellenőrizze, hogy engedélyezve van-e Azure Active Directory tartományi szolgáltatások a virtuális hálózati hello](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="4fb51-109">[Check whether Azure Active Directory Domain Service is enabled in hello virtual network](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span></span>
4. <span data-ttu-id="4fb51-110">[Ellenőrizze, hogy a virtuális hálózati hello csatlakoztatott tooother erőforrás](#check-whether-the-virtual-network-is-connected-to-other-resource).</span><span class="sxs-lookup"><span data-stu-id="4fb51-110">[Check whether hello virtual network is connected tooother resource](#check-whether-the-virtual-network-is-connected-to-other-resource).</span></span>
5. <span data-ttu-id="4fb51-111">[Ellenőrizze, hogy egy virtuális gép továbbra is fut a virtuális hálózati hello](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="4fb51-111">[Check whether a virtual machine is still running in hello virtual network](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span></span>
6. <span data-ttu-id="4fb51-112">[Ellenőrizze, hogy hello virtuális hálózati áttelepítés Beragadt](#check-whether-the-virtual-network-is-stuck-in-migration).</span><span class="sxs-lookup"><span data-stu-id="4fb51-112">[Check whether hello virtual network is stuck in migration](#check-whether-the-virtual-network-is-stuck-in-migration).</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="4fb51-113">Hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="4fb51-113">Troubleshooting steps</span></span>

### <a name="check-whether-a-virtual-network-gateway-is-running-in-hello-virtual-network"></a><span data-ttu-id="4fb51-114">Ellenőrizze, hogy fut-e a virtuális hálózati átjáró hello virtuális hálózatban</span><span class="sxs-lookup"><span data-stu-id="4fb51-114">Check whether a virtual network gateway is running in hello virtual network</span></span>

<span data-ttu-id="4fb51-115">tooremove hello virtuális hálózaton, akkor először el kell távolítania hello virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="4fb51-115">tooremove hello virtual network, you must first remove hello virtual network gateway.</span></span>

<span data-ttu-id="4fb51-116">Klasszikus virtuális hálózatot, nyissa meg a toohello **áttekintése** hello klasszikus virtuális hálózatot az Azure-portálon hello oldalán.</span><span class="sxs-lookup"><span data-stu-id="4fb51-116">For classic virtual networks, go toohello **Overview** page of hello classic virtual network in hello Azure portal.</span></span> <span data-ttu-id="4fb51-117">A hello **VPN-kapcsolatok** szakaszban, ha hello virtuális hálózati átjáró hello fut, látni fogja a hello IP hello átjáró címét.</span><span class="sxs-lookup"><span data-stu-id="4fb51-117">In hello **VPN connections** section, if hello gateway is running in hello virtual network, you will see hello IP address of hello gateway.</span></span> 

![Ellenőrizze, hogy az átjáró fut-e](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

<span data-ttu-id="4fb51-119">Virtuális hálózatok, nyissa meg a toohello **áttekintése** hello virtuális hálózati oldalán.</span><span class="sxs-lookup"><span data-stu-id="4fb51-119">For virtual networks, go toohello **Overview** page of hello virtual network.</span></span> <span data-ttu-id="4fb51-120">Ellenőrizze **csatlakoztatott eszközök** hello virtuális hálózati átjáró.</span><span class="sxs-lookup"><span data-stu-id="4fb51-120">Check **Connected devices** for hello virtual network gateway.</span></span>

![Ellenőrizze a hello csatlakoztatott eszközön](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

<span data-ttu-id="4fb51-122">Mielőtt eltávolíthatná hello átjáró, először távolítsa el **kapcsolat** hello átjáró objektumokat.</span><span class="sxs-lookup"><span data-stu-id="4fb51-122">Before you can remove hello gateway, first remove any **Connection** objects in hello gateway.</span></span> 

### <a name="check-whether-an-application-gateway-is-running-in-hello-virtual-network"></a><span data-ttu-id="4fb51-123">Ellenőrizze, hogy fut-e olyan átjárót hello virtuális hálózatban</span><span class="sxs-lookup"><span data-stu-id="4fb51-123">Check whether an application gateway is running in hello virtual network</span></span>

<span data-ttu-id="4fb51-124">Nyissa meg toohello **áttekintése** hello virtuális hálózati oldalán.</span><span class="sxs-lookup"><span data-stu-id="4fb51-124">Go toohello **Overview** page of hello virtual network.</span></span> <span data-ttu-id="4fb51-125">Ellenőrizze a hello **csatlakoztatott eszközök** az hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="4fb51-125">Check hello **Connected devices** for hello application gateway.</span></span>

![Ellenőrizze a hello csatlakoztatott eszközön](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

<span data-ttu-id="4fb51-127">Ha olyan átjárót, el kell távolítania azt hello virtuális hálózat törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="4fb51-127">If there is an application gateway, you must remove it before you can delete hello virtual network.</span></span>

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-hello-virtual-network"></a><span data-ttu-id="4fb51-128">Ellenőrizze, hogy engedélyezve van-e Azure Active Directory tartományi szolgáltatások hello virtuális hálózatban</span><span class="sxs-lookup"><span data-stu-id="4fb51-128">Check whether Azure Active Directory Domain Service is enabled in hello virtual network</span></span>

<span data-ttu-id="4fb51-129">Ha Active Directory tartományi szolgáltatások hello engedélyezett és a csatlakoztatott toohello virtuális hálózat, a virtuális hálózat nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="4fb51-129">If hello Active Directory Domain Service is enabled and connected toohello virtual network, you cannot delete this virtual network.</span></span> 

![Ellenőrizze a hello csatlakoztatott eszközön](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

<span data-ttu-id="4fb51-131">toodisable hello szolgáltatást, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4fb51-131">toodisable hello service, follow these steps:</span></span>

1. <span data-ttu-id="4fb51-132">Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4fb51-132">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="4fb51-133">Hello bal oldali ablaktáblában jelöljön ki **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4fb51-133">In hello left pane, select  **Active Directory**.</span></span>
3. <span data-ttu-id="4fb51-134">Válassza ki, amelyen engedélyezve van az Active Directory tartományi szolgáltatások hello Azure Active Directory (Azure AD) címtár.</span><span class="sxs-lookup"><span data-stu-id="4fb51-134">Select hello Azure Active Directory (Azure AD) directory that has Active Directory Domain Service enabled.</span></span>
4. <span data-ttu-id="4fb51-135">Jelölje be hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="4fb51-135">Select hello **Configure** tab.</span></span>
5. <span data-ttu-id="4fb51-136">A **tartományi szolgáltatások**, hello módosítása **engedélyezése tartományi szolgáltatásokat a címtárhoz** beállítás túl**nem**.</span><span class="sxs-lookup"><span data-stu-id="4fb51-136">Under **domain services**, change hello **Enable domain services for this directory** option too**No**.</span></span>  

### <a name="check-whether-hello-virtual-network-is-connected-tooother-resource"></a><span data-ttu-id="4fb51-137">Ellenőrizze, hogy a virtuális hálózati hello csatlakoztatott tooother erőforrás</span><span class="sxs-lookup"><span data-stu-id="4fb51-137">Check whether hello virtual network is connected tooother resource</span></span>

<span data-ttu-id="4fb51-138">Ellenőrizze a kapcsolatcsoport hivatkozások, a kapcsolatok és a virtuális hálózati társviszony.</span><span class="sxs-lookup"><span data-stu-id="4fb51-138">Check for Circuit Links, connections, and virtual network peerings.</span></span> <span data-ttu-id="4fb51-139">Ezek a virtuális hálózat törlése toofail okozhat.</span><span class="sxs-lookup"><span data-stu-id="4fb51-139">Any of these can cause a virtual network deletion toofail.</span></span> 

<span data-ttu-id="4fb51-140">hello ajánlott Törlés sorrendje a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="4fb51-140">hello recommended deletion order is as follows:</span></span>

1. <span data-ttu-id="4fb51-141">Átjáró-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="4fb51-141">Gateway connections</span></span>
2. <span data-ttu-id="4fb51-142">Átjárók</span><span class="sxs-lookup"><span data-stu-id="4fb51-142">Gateways</span></span>
3. <span data-ttu-id="4fb51-143">IP-címek</span><span class="sxs-lookup"><span data-stu-id="4fb51-143">IPs</span></span>
4. <span data-ttu-id="4fb51-144">Virtuális hálózati társviszony</span><span class="sxs-lookup"><span data-stu-id="4fb51-144">Virtual network peerings</span></span>
5. <span data-ttu-id="4fb51-145">App Service Environment-környezet (ASE)</span><span class="sxs-lookup"><span data-stu-id="4fb51-145">App Service Environment (ASE)</span></span>

### <a name="check-whether-a-virtual-machine-is-still-running-in-hello-virtual-network"></a><span data-ttu-id="4fb51-146">Ellenőrizze, hogy egy virtuális gép továbbra is fut a virtuális hálózati hello</span><span class="sxs-lookup"><span data-stu-id="4fb51-146">Check whether a virtual machine is still running in hello virtual network</span></span>

<span data-ttu-id="4fb51-147">Győződjön meg arról, hogy egyetlen virtuális gép hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="4fb51-147">Make sure that no virtual machine is in hello virtual network.</span></span>

### <a name="check-whether-hello-virtual-network-is-stuck-in-migration"></a><span data-ttu-id="4fb51-148">Ellenőrizze, hogy hello virtuális hálózati áttelepítés Beragadt</span><span class="sxs-lookup"><span data-stu-id="4fb51-148">Check whether hello virtual network is stuck in migration</span></span>

<span data-ttu-id="4fb51-149">Ha a virtuális hálózati hello Beragadt a migrálási állapota, nem lehet törölni.</span><span class="sxs-lookup"><span data-stu-id="4fb51-149">If hello virtual network is stuck in a migration state, it cannot be deleted.</span></span> <span data-ttu-id="4fb51-150">Futtassa a következő parancs tooabort hello áttelepítési hello, és törölje a virtuális hálózati hello.</span><span class="sxs-lookup"><span data-stu-id="4fb51-150">Run hello following command tooabort hello migration, and then delete hello virtual network.</span></span>

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a><span data-ttu-id="4fb51-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fb51-151">Next steps</span></span>

- [<span data-ttu-id="4fb51-152">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="4fb51-152">Azure Virtual Network</span></span>](virtual-networks-overview.md)
- [<span data-ttu-id="4fb51-153">Azure Virtual Network – Gyakori kérdések (GYIK)</span><span class="sxs-lookup"><span data-stu-id="4fb51-153">Azure Virtual Network frequently asked questions (FAQ)</span></span>](virtual-networks-faq.md)