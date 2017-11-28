---
title: "a Service Fabric aaaAzure az API Management – első lépések |} Microsoft Docs"
description: "Ez az útmutató bemutatja, hogyan tooquickly el az Azure API Management és a Service Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: f76f3f39a92f89892d6a02ecaab1ec3d343fe2a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a><span data-ttu-id="7de62-103">A Service Fabric az Azure API Management – első lépések</span><span class="sxs-lookup"><span data-stu-id="7de62-103">Service Fabric with Azure API Management Quick Start</span></span>

<span data-ttu-id="7de62-104">Ez az útmutató bemutatja, hogyan tooset Azure API Management a Service Fabric össze, és az első API művelet toosend forgalom tooback-szolgáltatások konfigurálása a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="7de62-104">This guide shows you how tooset up Azure API Management with Service Fabric and configure your first API operation toosend traffic tooback-end services in Service Fabric.</span></span> <span data-ttu-id="7de62-105">További információ az Azure API Management-forgatókönyvet az Service Fabric toolearn lásd: hello [áttekintése](service-fabric-api-management-overview.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7de62-105">toolearn more about Azure API Management scenarios with Service Fabric, see hello [overview](service-fabric-api-management-overview.md) article.</span></span> 

## <a name="deploy-api-management-and-service-fabric-tooazure"></a><span data-ttu-id="7de62-106">Az API Management és a Service Fabric tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="7de62-106">Deploy API Management and Service Fabric tooAzure</span></span>

<span data-ttu-id="7de62-107">első lépés hello toodeploy API Management és a Service Fabric-fürt tooAzure egy megosztott virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="7de62-107">hello first step is toodeploy API Management and a Service Fabric cluster tooAzure in a shared Virtual Network.</span></span> <span data-ttu-id="7de62-108">Ez lehetővé teszi az API Management toocommunicate közvetlenül a Service Fabric, így műveleteket hajthat végre a szolgáltatásészlelés, a szolgáltatás partíció megoldása és a forgalom közvetlen tooany háttér a Service Fabric szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7de62-108">This allows API Management toocommunicate directly with Service Fabric so it can perform service discovery, service partition resolution, and forward traffic directly tooany backend service in Service Fabric.</span></span>

### <a name="topology"></a><span data-ttu-id="7de62-109">topológia</span><span class="sxs-lookup"><span data-stu-id="7de62-109">Topology</span></span>

<span data-ttu-id="7de62-110">Ez az útmutató hello következő telepíti, amelyben az API Management és a Service Fabric is alhálózatain topológia tooAzure hello ugyanazon a virtuális hálózaton:</span><span class="sxs-lookup"><span data-stu-id="7de62-110">This guide deploys hello following topology tooAzure in which API Management and Service Fabric are in subnets of hello same Virtual Network:</span></span>

 ![Képfelirat][sf-apim-topology-overview]

<span data-ttu-id="7de62-112">tooget gyorsan lépéseket, Resource Manager-sablonok áll rendelkezésre egyes telepítési lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7de62-112">tooget started quickly, Resource Manager templates are provided for each deployment step:</span></span>

 - <span data-ttu-id="7de62-113">Hálózati topológia:</span><span class="sxs-lookup"><span data-stu-id="7de62-113">Network topology:</span></span>
    - <span data-ttu-id="7de62-114">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-114">[network.json][network-arm]</span></span>
    - <span data-ttu-id="7de62-115">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-115">[network.parameters.json][network-parameters-arm]</span></span>
 - <span data-ttu-id="7de62-116">Service Fabric-fürt:</span><span class="sxs-lookup"><span data-stu-id="7de62-116">Service Fabric cluster:</span></span>
    - <span data-ttu-id="7de62-117">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-117">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="7de62-118">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-118">[cluster.parameters.json][cluster-parameters-arm]</span></span>
 - <span data-ttu-id="7de62-119">API-kezelés:</span><span class="sxs-lookup"><span data-stu-id="7de62-119">API Management:</span></span>
    - <span data-ttu-id="7de62-120">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-120">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="7de62-121">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-121">[apim.parameters.json][apim-parameters-arm]</span></span>

### <a name="sign-in-tooazure-and-select-your-subscription"></a><span data-ttu-id="7de62-122">Jelentkezzen be tooAzure, és jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="7de62-122">Sign in tooAzure and select your subscription</span></span>

<span data-ttu-id="7de62-123">Ez az útmutató használ [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="7de62-123">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="7de62-124">Amikor egy új PowerShell-munkamenetet indít el, jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését, Azure parancsok végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="7de62-124">When you start a new PowerShell session, sign in tooyour Azure account and select your subscription before you execute Azure commands.</span></span>
 
<span data-ttu-id="7de62-125">Bejelentkezés tooyour Azure-fiók:</span><span class="sxs-lookup"><span data-stu-id="7de62-125">Sign in tooyour Azure account:</span></span>

```powershell
PS > Login-AzureRmAccount
```

<span data-ttu-id="7de62-126">Jelölje ki az előfizetését:</span><span class="sxs-lookup"><span data-stu-id="7de62-126">Select your subscription:</span></span>

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a><span data-ttu-id="7de62-127">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="7de62-127">Create a resource group</span></span>

<span data-ttu-id="7de62-128">Hozzon létre egy új erőforráscsoportot az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="7de62-128">Create a new resource group for your deployment.</span></span> <span data-ttu-id="7de62-129">Adjon neki egy nevet és egy helyet.</span><span class="sxs-lookup"><span data-stu-id="7de62-129">Give it a name and a location.</span></span>

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-hello-network-topology"></a><span data-ttu-id="7de62-130">Hello hálózati topológia központi telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="7de62-130">Deploy hello network topology</span></span>

<span data-ttu-id="7de62-131">hello első lépéseként tooset mentése hello hálózati topológia toowhich API Management és hello Service Fabric-fürt lesz telepítve.</span><span class="sxs-lookup"><span data-stu-id="7de62-131">hello first step is tooset up hello network topology toowhich API Management and hello Service Fabric cluster will be deployed.</span></span> <span data-ttu-id="7de62-132">Hello [network.json] [ network-arm] Resource Manager-sablon olyan virtuális hálózatot (VNET), két alhálózattal és két hálózati biztonsági csoportok (NSG) konfigurált toocreate.</span><span class="sxs-lookup"><span data-stu-id="7de62-132">hello [network.json][network-arm] Resource Manager template is configured toocreate a Virtual Network (VNET) with two subnets and two Network Security Groups (NSG).</span></span> 

<span data-ttu-id="7de62-133">Hello [network.parameters.json] [ network-parameters-arm] paraméterfájl hello alhálózatok és a telepítendő API Management és a Service Fabric az NSG-ket hello nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7de62-133">hello [network.parameters.json][network-parameters-arm] parameters file contains hello names of hello subnets and NSGs that API Management and Service Fabric will be deployed to.</span></span> <span data-ttu-id="7de62-134">Ez az útmutató hello paraméterértékeket nem szükséges módosítani toobe.</span><span class="sxs-lookup"><span data-stu-id="7de62-134">For this guide, hello parameter values do not need toobe changed.</span></span> <span data-ttu-id="7de62-135">hello API Management és a Service Fabric Resource Manager sablonok használja ezeket az értékeket, így ha itt módosításuk, módosítania kell azokat hello más Resource Manager-sablonok ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="7de62-135">hello API Management and Service Fabric Resource Manager templates use these values, so if they are modified here, you must modify them in hello other Resource Manager templates accordingly.</span></span> 

 1. <span data-ttu-id="7de62-136">Töltse le a következő Resource Manager sablon és a paraméterek fájl hello:</span><span class="sxs-lookup"><span data-stu-id="7de62-136">Download hello following Resource Manager template and parameters file:</span></span>

    - <span data-ttu-id="7de62-137">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-137">[network.json][network-arm]</span></span>
    - <span data-ttu-id="7de62-138">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-138">[network.parameters.json][network-parameters-arm]</span></span>

 2. <span data-ttu-id="7de62-139">A következő PowerShell toodeploy hello Resource Manager sablonnal és paraméterfájlokkal parancsfájlok hello hálózati telepítő hello használata:</span><span class="sxs-lookup"><span data-stu-id="7de62-139">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files for hello network setup:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-hello-service-fabric-cluster"></a><span data-ttu-id="7de62-140">Hello Service Fabric-fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="7de62-140">Deploy hello Service Fabric cluster</span></span>

<span data-ttu-id="7de62-141">Hello hálózati erőforrások végzett telepítését, miután hello következő lépésre toodeploy a Service Fabric fürt toohello VNET hello alhálózati és NSG hello Service Fabric-fürt jelöltek ki.</span><span class="sxs-lookup"><span data-stu-id="7de62-141">Once hello network resources have finished deploying, hello next step is toodeploy a Service Fabric cluster toohello VNET in hello subnet and NSG designated for hello Service Fabric cluster.</span></span> <span data-ttu-id="7de62-142">Ebben az oktatóanyagban hello Service Fabric Resource Manager-sablon előre konfigurált toouse hello nevek hello virtuális Hálózatot, alhálózatot és hello előző lépésben beállított NSG.</span><span class="sxs-lookup"><span data-stu-id="7de62-142">For this tutorial, hello Service Fabric Resource Manager template is pre-configured toouse hello names of hello VNET, subnet, and NSG that you set up in hello previous step.</span></span> 

<span data-ttu-id="7de62-143">Service Fabric fürt Resource Manager-sablon hello konfigurált toocreate tanúsítvány biztonsági használó biztonságos fürtök.</span><span class="sxs-lookup"><span data-stu-id="7de62-143">hello Service Fabric cluster Resource Manager template is configured toocreate a secure cluster with certificate security.</span></span> <span data-ttu-id="7de62-144">hello tanúsítványok is használt toosecure csomópontok kommunikáció a fürt és a toomanage felhasználói hozzáférés tooyour Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="7de62-144">hello certificate is used toosecure node-to-node communication for your cluster and toomanage user access tooyour Service Fabric cluster.</span></span> <span data-ttu-id="7de62-145">Az API Management szolgáltatás felderítése a tanúsítvány tooaccess hello Service Fabric-szolgáltatás használ.</span><span class="sxs-lookup"><span data-stu-id="7de62-145">API Management uses this certificate tooaccess  hello Service Fabric Naming Service for service discovery.</span></span>

<span data-ttu-id="7de62-146">Ez a lépés szükséges tanúsítvánnyal rendelkező Key Vault a fürt biztonsági.</span><span class="sxs-lookup"><span data-stu-id="7de62-146">This step requires having a certificate in Key Vault for cluster security.</span></span> <span data-ttu-id="7de62-147">Key Vault használó biztonságos fürtök beállításával kapcsolatos további információkért lásd: [Ez az útmutató a fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md)</span><span class="sxs-lookup"><span data-stu-id="7de62-147">For more information on setting up a secure cluster with Key Vault, see [this guide on creating a cluster in Azure using Resource Manager](service-fabric-cluster-creation-via-arm.md)</span></span>

> [!NOTE]
> <span data-ttu-id="7de62-148">A hozzáférés a fürthöz használt hozzáadása toohello tanúsítvány hozzáadhatja Azure Active Directory-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="7de62-148">You may add Azure Active Directory authentication in addition toohello certificate used for cluster access.</span></span> <span data-ttu-id="7de62-149">Az Azure Active Directory hello ajánlott módja toomanage felhasználói hozzáférés tooyour Service Fabric-fürt, de nem szükséges toocomplete Ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="7de62-149">Azure Active Directory is hello recommended way toomanage user access tooyour Service Fabric cluster, but is not necessary toocomplete this tutorial.</span></span> <span data-ttu-id="7de62-150">Egy tanúsítványra szükség mindkét módszer fürt a csomópontok biztonság és az Azure API Management hitelesítést, amely jelenleg nem támogatja az Azure Active Directoryban a Service Fabric háttérkiszolgáló hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7de62-150">A certificate is required either way for cluster node-to-node security and for Azure API Management authentication, which currently does not support authenticating with Azure Active Directory for a Service Fabric backend.</span></span>

 1. <span data-ttu-id="7de62-151">Töltse le a következő Resource Manager sablon és a paraméterek fájl hello:</span><span class="sxs-lookup"><span data-stu-id="7de62-151">Download hello following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="7de62-152">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-152">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="7de62-153">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-153">[cluster.parameters.json][cluster-parameters-arm]</span></span>

 2. <span data-ttu-id="7de62-154">Töltse ki a hello üres paraméterek között hello `cluster.parameters.json` fájl a telepítéshez, beleértve a hello [Key Vault információk](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) a fürt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="7de62-154">Fill in hello empty parameters in hello `cluster.parameters.json` file for your deployment, including hello [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) for your cluster certificate.</span></span>

 3. <span data-ttu-id="7de62-155">A következő PowerShell parancs toodeploy hello Resource Manager sablonnal és paraméterfájlokkal fájlok toocreate hello Service Fabric-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="7de62-155">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files toocreate hello Service Fabric cluster:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a><span data-ttu-id="7de62-156">Az API Management telepítése</span><span class="sxs-lookup"><span data-stu-id="7de62-156">Deploy API Management</span></span>

<span data-ttu-id="7de62-157">Végezetül telepítése az API Management toohello VNET hello az alhálózaton, és az API Management kijelölt NSG.</span><span class="sxs-lookup"><span data-stu-id="7de62-157">Finally, deploy API Management toohello VNET in hello subnet and NSG designated for API Management.</span></span> <span data-ttu-id="7de62-158">Toowait hello Service Fabric fürt telepítési toofinish az API Management telepítése előtt nem kell.</span><span class="sxs-lookup"><span data-stu-id="7de62-158">You do not need toowait for hello Service Fabric cluster deployment toofinish before deploying API Management.</span></span> 

<span data-ttu-id="7de62-159">Ebben az oktatóanyagban hello API Management Resource Manager-sablon előre konfigurált toouse hello nevek hello virtuális Hálózatot, alhálózatot és hello előző lépésben beállított NSG.</span><span class="sxs-lookup"><span data-stu-id="7de62-159">For this tutorial, hello API Management Resource Manager template is pre-configured toouse hello names of hello VNET, subnet, and NSG that you set up in hello previous step.</span></span> 

 1. <span data-ttu-id="7de62-160">Töltse le a következő Resource Manager sablon és a paraméterek fájl hello:</span><span class="sxs-lookup"><span data-stu-id="7de62-160">Download hello following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="7de62-161">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-161">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="7de62-162">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="7de62-162">[apim.parameters.json][apim-parameters-arm]</span></span>

 2. <span data-ttu-id="7de62-163">Töltse ki a hello üres paraméterek között hello `apim.parameters.json` az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="7de62-163">Fill in hello empty parameters in hello `apim.parameters.json` for your deployment.</span></span>

 3. <span data-ttu-id="7de62-164">Használja a következő PowerShell parancs toodeploy hello Resource Manager sablonnal és paraméterfájlokkal fájlok az API Management hello:</span><span class="sxs-lookup"><span data-stu-id="7de62-164">Use hello following PowerShell command toodeploy hello Resource Manager template and parameter files for API Management:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a><span data-ttu-id="7de62-165">API-kezelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7de62-165">Configure API Management</span></span>

<span data-ttu-id="7de62-166">Után az API Management és a Service Fabric-fürt vannak telepítve, a Service Fabric háttér állíthatja be az API Management.</span><span class="sxs-lookup"><span data-stu-id="7de62-166">Once your API Management and Service Fabric cluster are deployed, you can configure a Service Fabric backend in API Management.</span></span> <span data-ttu-id="7de62-167">Ez lehetővé teszi egy háttér szolgáltatás házirend által küldött forgalmat tooyour Service Fabric-fürt toocreate.</span><span class="sxs-lookup"><span data-stu-id="7de62-167">This allows you toocreate a backend service policy that sends traffic tooyour Service Fabric cluster.</span></span>

### <a name="api-management-security"></a><span data-ttu-id="7de62-168">API Management biztonsági</span><span class="sxs-lookup"><span data-stu-id="7de62-168">API Management Security</span></span>

<span data-ttu-id="7de62-169">tooconfigure hello Service Fabric háttér, először tooconfigure API Management biztonsági beállításait.</span><span class="sxs-lookup"><span data-stu-id="7de62-169">tooconfigure hello Service Fabric backend, you first need tooconfigure API Management security settings.</span></span> <span data-ttu-id="7de62-170">tooconfigure biztonsági beállításait, lépjen a tooyour API Management szolgáltatásba a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7de62-170">tooconfigure security settings, go tooyour API Management service in hello Azure portal.</span></span>

#### <a name="enable-hello-api-management-rest-api"></a><span data-ttu-id="7de62-171">Hello API Management REST API engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7de62-171">Enable hello API Management REST API</span></span>

<span data-ttu-id="7de62-172">hello API Management REST API jelenleg hello csak úgy tooconfigure egy háttérszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="7de62-172">hello API Management REST API is currently hello only way tooconfigure a backend service.</span></span> <span data-ttu-id="7de62-173">első lépés hello tooenable hello API Management REST API-t, és biztosíthatja.</span><span class="sxs-lookup"><span data-stu-id="7de62-173">hello first step is tooenable hello API Management REST API and secure it.</span></span>

 1. <span data-ttu-id="7de62-174">Hello API-kezelés szolgáltatás, válassza ki **felügyeleti API - előzetes** alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="7de62-174">In hello API Management service, select **Management API - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="7de62-175">Ellenőrizze a hello **engedélyezése API Management REST API** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="7de62-175">Check hello **Enable API Management REST API** checkbox.</span></span>
 3. <span data-ttu-id="7de62-176">Vegye figyelembe a hello felügyeleti API URL-CÍMÉT – ez használjuk fel hello Service Fabric háttér újabb tooset hello URL-címe</span><span class="sxs-lookup"><span data-stu-id="7de62-176">Note hello Management API URL - this is hello URL we'll use later tooset up hello Service Fabric backend</span></span>
 4. <span data-ttu-id="7de62-177">Generate egy **hozzáférési tokent** kiválasztásával lejárati dátummal és a kulcsot, majd kattintson az hello **Generate** gomb hello lap alján hello felé.</span><span class="sxs-lookup"><span data-stu-id="7de62-177">Generate an **access Token** by selecting an expiry date and a key, then click hello **Generate** button toward hello bottom of hello page.</span></span>
 5. <span data-ttu-id="7de62-178">Másolás hello **hozzáférési jogkivonat** és mentheti – Ez a következő lépéseket hello fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="7de62-178">Copy hello **access token** and save it - we'll use this in hello following steps.</span></span> <span data-ttu-id="7de62-179">Vegye figyelembe, hogy ez nem azonos a hello elsődleges és másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7de62-179">Note this is different from hello primary key and secondary key.</span></span>

#### <a name="upload-a-service-fabric-client-certificate"></a><span data-ttu-id="7de62-180">A Service Fabric ügyfél-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="7de62-180">Upload a Service Fabric client certificate</span></span>

<span data-ttu-id="7de62-181">A Service Fabric-fürtöt használ, amely rendelkezik hozzáférési tooyour fürt tanúsítványt szolgáltatásészlelés az API Management kell hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="7de62-181">API Management must authenticate with your Service Fabric cluster for service discovery using a client certificate that has access tooyour cluster.</span></span> <span data-ttu-id="7de62-182">Az egyszerűség kedvéért a oktatóanyag használ a fürt hello hello Service Fabric-fürt, amely alapértelmezés szerint használt tooaccess lehet létrehozásakor megadott ugyanazt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7de62-182">For simplicity, this tutorial uses hello same certificate specified when creating hello Service Fabric cluster, which by default can be used tooaccess your cluster.</span></span>

 1. <span data-ttu-id="7de62-183">Hello API-kezelés szolgáltatás, válassza ki **ügyféltanúsítványok - előzetes** alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="7de62-183">In hello API Management service, select **Client certificates - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="7de62-184">Kattintson a hello **+ Hozzáadás** gomb</span><span class="sxs-lookup"><span data-stu-id="7de62-184">Click hello **+ Add** button</span></span>
 2. <span data-ttu-id="7de62-185">Hello titkos kulcs fájlja (.pfx) a Service Fabric-fürt létrehozásakor megadott hello fürt tanúsítvány kiválasztása, adjon neki egy nevet, és adja meg a hello titkos kulcs jelszava.</span><span class="sxs-lookup"><span data-stu-id="7de62-185">Select hello private key file (.pfx) of hello cluster certificate that you specified when creating your Service Fabric cluster, give it a name, and provide hello private key password.</span></span>

> [!NOTE]
> <span data-ttu-id="7de62-186">Ez az oktatóanyag hello ugyanaz a tanúsítvány az ügyfél-hitelesítés és a fürt-csomópontok biztonsági használja.</span><span class="sxs-lookup"><span data-stu-id="7de62-186">This tutorial uses hello same certificate for client authentication and cluster node-to-node security.</span></span> <span data-ttu-id="7de62-187">Ha a Service Fabric-fürt rendelkezik egy konfigurált tooaccess, hogy külön ügyfél-tanúsítványt használhatja.</span><span class="sxs-lookup"><span data-stu-id="7de62-187">You may use a separate client certificate if you have one configured tooaccess your Service Fabric cluster.</span></span>

### <a name="configure-hello-backend"></a><span data-ttu-id="7de62-188">Hello háttér konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7de62-188">Configure hello backend</span></span>

<span data-ttu-id="7de62-189">Most, hogy az API Management a biztonsági beállítások konfigurálása hello Service Fabric háttér konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="7de62-189">Now that API Management security is configured, you can configure hello Service Fabric backend.</span></span> <span data-ttu-id="7de62-190">A Service Fabric háttérkiszolgálókon hello Service Fabric-fürt esetén hello háttér ahelyett, hogy egy adott Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7de62-190">For Service Fabric backends, hello Service Fabric cluster is hello backend, rather than a specific Service Fabric service.</span></span> <span data-ttu-id="7de62-191">Ez lehetővé teszi, hogy a szabályzatban tooroute toomore, mint egy szolgáltatás hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="7de62-191">This allows a single policy tooroute toomore than one service in hello cluster.</span></span>

<span data-ttu-id="7de62-192">Ez a lépés hello hozzáférési jogkivonat korábban létrehozott igényel, és hello tooAPI felügyeleti hello előző lépésben töltött fel a fürt-tanúsítványának ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="7de62-192">This step requires hello access token you generated earlier and hello thumbprint for your cluster certificate you uploaded tooAPI Management in hello previous step.</span></span>

> [!NOTE]
> <span data-ttu-id="7de62-193">Az API Management hello előző lépésben használt külön ügyfél-tanúsítványt, ha szüksége hello ujjlenyomat hello ügyféltanúsítvánnyal hozzáadása toohello fürt tanúsítvány ujjlenyomata ebben a lépésben.</span><span class="sxs-lookup"><span data-stu-id="7de62-193">If you used a separate client certificate in hello previous step for API Management, you need hello thumbprint for hello client certificate in addition toohello cluster certificate thumbprint in this step.</span></span>

<span data-ttu-id="7de62-194">A következő HTTP PUT kérés toohello API Management API URL korábban feljegyzett hello API Management REST API tooconfigure hello Service Fabric háttérszolgáltatást engedélyezésekor hello küldése.</span><span class="sxs-lookup"><span data-stu-id="7de62-194">Send hello following HTTP PUT request toohello API Management API URL you noted earlier when enabling hello API Management REST API tooconfigure hello Service Fabric backend service.</span></span> <span data-ttu-id="7de62-195">Megjelenik egy `HTTP 201 Created` választ, ha a hello parancs sikeres.</span><span class="sxs-lookup"><span data-stu-id="7de62-195">You should see an `HTTP 201 Created` response when hello command succeeds.</span></span> <span data-ttu-id="7de62-196">Minden mező további információkért lásd: hello API Management [háttér-API referenciadokumentációt](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span><span class="sxs-lookup"><span data-stu-id="7de62-196">For more information on each field, see hello API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span></span>

<span data-ttu-id="7de62-197">HTTP-parancs és az URL-címe:</span><span class="sxs-lookup"><span data-stu-id="7de62-197">HTTP command and URL:</span></span>
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

<span data-ttu-id="7de62-198">Kérelem fejlécei:</span><span class="sxs-lookup"><span data-stu-id="7de62-198">Request headers:</span></span>
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

<span data-ttu-id="7de62-199">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="7de62-199">Request body:</span></span>
```http
{
    "description": "<description>",
    "url": "<fallback service name>",
    "protocol": "http",
    "resourceId": "<cluster HTTP management endpoint>",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": [ "<cluster HTTP management endpoint>" ],
            "clientCertificateThumbprint": "<client cert thumbprint>",
            "serverCertificateThumbprints": [ "<cluster cert thumbprint>" ],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

<span data-ttu-id="7de62-200">Hello **URL-cím** itt paraméter egy teljesen minősített nevét a fürt összes kérelmet, a szolgáltatás tooby alapértelmezett irányítva, ha a háttérkiszolgáló házirendben megadott szolgáltatásnév nem.</span><span class="sxs-lookup"><span data-stu-id="7de62-200">hello **url** parameter here is a fully-qualified service name of a service in your cluster that all requests are routed tooby default if no service name is specified in a backend policy.</span></span> <span data-ttu-id="7de62-201">Használhat egy hamis nevét, például a "fabric: / hamis/szolgáltatás" Ha nem kívánja toohave tartalék szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7de62-201">You may use a fake service name, such as "fabric:/fake/service" if you do not intend toohave a fallback service.</span></span>

<span data-ttu-id="7de62-202">Tekintse meg az API Management toohello [háttér-API referenciadokumentációt](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) kapcsolatban további részleteket a minden mező.</span><span class="sxs-lookup"><span data-stu-id="7de62-202">Refer toohello API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) for more details on each field.</span></span>

#### <a name="example"></a><span data-ttu-id="7de62-203">Példa</span><span class="sxs-lookup"><span data-stu-id="7de62-203">Example</span></span>

```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
Authorization: SharedAccessSignature 230948023984&Ld93cRGcNU6KZ4uVz7JlfTec4eX43Q9Nu8ndatOgBzs6+f559Pkf3iHX2cSge+r42pn35qGY3TitjrIl13hwcQ==
Content-Type: application/json

{
    "description": "My Service Fabric backend",
    "url": "fabric:/myapp/myservice",
    "protocol": "http",
    "resourceId": "https://your-cluster.westus.cloudapp.azure.com:19080",
    "properties": {
        "serviceFabricCluster": {
            "managementEndpoints": ["https://your-cluster.westus.cloudapp.azure.com:19080"],
            "clientCertificateThumbprint": "57bc463aba3aea3a12a18f36f44154f819f0fe32",
            "serverCertificateThumbprints": ["57bc463aba3aea3a12a18f36f44154f819f0fe32"],
            "maxPartitionResolutionRetries" : 5
        }
    }
}
```

## <a name="deploy-a-service-fabric-back-end-service"></a><span data-ttu-id="7de62-204">A Service Fabric háttér-szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="7de62-204">Deploy a Service Fabric back-end service</span></span>

<span data-ttu-id="7de62-205">Most, hogy a Service Fabric konfigurálva, egy háttér tooAPI felügyeleti hello, az API-k esetében, amelyek forgalmat küldeni a Service Fabric-szolgáltatások tooyour hozhat létre a háttér-házirendeket.</span><span class="sxs-lookup"><span data-stu-id="7de62-205">Now that you have hello Service Fabric configured as a backend tooAPI Management, you can author backend policies for your APIs that send traffic tooyour Service Fabric services.</span></span> <span data-ttu-id="7de62-206">De először meg kell a Service Fabric tooaccept kérelmek futó szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7de62-206">But first you need a service running in Service Fabric tooaccept requests.</span></span>

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a><span data-ttu-id="7de62-207">A Service Fabric-szolgáltatás egy HTTP-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="7de62-207">Create a Service Fabric service with an HTTP endpoint</span></span>

<span data-ttu-id="7de62-208">Ebben az oktatóanyagban létrehozunk egy egyszerű állapot nélküli ASP.NET Core megbízható szolgáltatás hello alapértelmezett webes API projektsablon használatával.</span><span class="sxs-lookup"><span data-stu-id="7de62-208">For this tutorial, we'll create a basic stateless ASP.NET Core Reliable Service using hello default Web API project template.</span></span> <span data-ttu-id="7de62-209">Ezzel létrehoz egy HTTP-végpont a szolgáltatás, amely akkor lesz Azure API Management elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="7de62-209">This creates an HTTP endpoint for your service, which you'll expose through Azure API Management:</span></span>

```
/api/values
```

<span data-ttu-id="7de62-210">Első lépésként [állítja be a fejlesztési környezetet az ASP.NET Core fejlesztési](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="7de62-210">Start by [setting up your development environment for ASP.NET Core development](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span></span>

<span data-ttu-id="7de62-211">Miután beállította a fejlesztőkörnyezetet, indítsa el a Visual Studio rendszergazdaként, és az ASP.NET Core szolgáltatás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="7de62-211">Once your development environment is set up, start Visual Studio as Administrator and create an ASP.NET Core service:</span></span>

 1. <span data-ttu-id="7de62-212">A Visual Studio alkalmazásban válassza ki a fájl -> Új projekt.</span><span class="sxs-lookup"><span data-stu-id="7de62-212">In Visual Studio, select File -> New Project.</span></span>
 2. <span data-ttu-id="7de62-213">Válassza ki a felhő hello Service Fabric-alkalmazás sablont, és nevezze el **"ApiApplication"**.</span><span class="sxs-lookup"><span data-stu-id="7de62-213">Select hello Service Fabric Application template under Cloud and name it **"ApiApplication"**.</span></span>
 3. <span data-ttu-id="7de62-214">Válassza ki az ASP.NET Core Szolgáltatássablon hello és name hello projekt **"WebApiService"**.</span><span class="sxs-lookup"><span data-stu-id="7de62-214">Select hello ASP.NET Core service template and name hello project **"WebApiService"**.</span></span>
 4. <span data-ttu-id="7de62-215">Válassza ki a webes API-t az ASP.NET Core 1.1 hello projekt sablont.</span><span class="sxs-lookup"><span data-stu-id="7de62-215">Select hello Web API ASP.NET Core 1.1 project template.</span></span>
 5. <span data-ttu-id="7de62-216">Hello projekt létrehozása után nyissa meg a `PackageRoot\ServiceManifest.xml` , és távolítsa el a hello `Port` attribútumot hello végpont erőforrás-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="7de62-216">Once hello project is created, open `PackageRoot\ServiceManifest.xml` and remove hello `Port` attribute from hello endpoint resource configuration:</span></span>
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    <span data-ttu-id="7de62-217">Ez lehetővé teszi a Service Fabric toospecify egy portjával dinamikusan hello alkalmazás porttartományát, amely azt nyitják meg a hálózati biztonsági csoport hello hello fürt Resource Manager-sablon, a forgalom tooflow tooit engedélyezi az API Management.</span><span class="sxs-lookup"><span data-stu-id="7de62-217">This allows Service Fabric toospecify a port dynamically from hello application port range, which we opened through hello Network Security Group in hello cluster Resource Manager template, allowing traffic tooflow tooit from API Management.</span></span>
 
 6. <span data-ttu-id="7de62-218">Nyomja le az F5 a Visual Studio tooverify hello webes API-t helyileg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="7de62-218">Press F5 in Visual Studio tooverify hello web API is available locally.</span></span> 

    <span data-ttu-id="7de62-219">Nyissa meg a Service Fabric Explorerben talál, és részletekbe menően tárhatják tooa ASP.NET Core toosee hello alapcímnek hello szolgáltatást figyel a következőn: hello adott példánya.</span><span class="sxs-lookup"><span data-stu-id="7de62-219">Open Service Fabric Explorer and drill down tooa specific instance of hello ASP.NET Core service toosee hello base address hello service is listening on.</span></span> <span data-ttu-id="7de62-220">Adja hozzá `/api/values` toohello cím kiinduló, majd nyissa meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="7de62-220">Add `/api/values` toohello base address and open it in a browser.</span></span> <span data-ttu-id="7de62-221">Ez elindítja a hello hello ValuesController hello webes API-sablonban a Get metódust.</span><span class="sxs-lookup"><span data-stu-id="7de62-221">This invokes hello Get method on hello ValuesController in hello Web API template.</span></span> <span data-ttu-id="7de62-222">Hello alapértelmezett válasz hello sablon, egy JSON-tömb két karakterláncot tartalmazó által biztosított adja vissza:</span><span class="sxs-lookup"><span data-stu-id="7de62-222">It returns hello default response that is provided by hello template, a JSON array that contains two strings:</span></span>

    ```json
    ["value1", "value2"]`
    ```

    <span data-ttu-id="7de62-223">Ez a hello végpontot, amely akkor lesz az Azure API Management elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="7de62-223">This is hello endpoint that you'll expose through API Management in Azure.</span></span>

 7. <span data-ttu-id="7de62-224">Végezetül telepíteni hello alkalmazás tooyour fürtöket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7de62-224">Finally, deploy hello application tooyour cluster in Azure.</span></span> <span data-ttu-id="7de62-225">[Visual Studio használatával](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), kattintson a jobb gombbal a hello projektet, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="7de62-225">[Using Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), right-click hello Application project and select **Publish**.</span></span> <span data-ttu-id="7de62-226">Adja meg a fürt végpontja (például `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello alkalmazás tooyour Service Fabric-fürt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7de62-226">Provide your cluster endpoint (for example, `mycluster.westus.cloudapp.azure.com:19000`) toodeploy hello application tooyour Service Fabric cluster in Azure.</span></span>

<span data-ttu-id="7de62-227">Az ASP.NET Core állapotmentes szolgáltatások nevű `fabric:/ApiApplication/WebApiService` most futnia kell a Service Fabric-fürt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7de62-227">An ASP.NET Core stateless service named `fabric:/ApiApplication/WebApiService` should now be running in your Service Fabric cluster in Azure.</span></span>

## <a name="create-an-api-operation"></a><span data-ttu-id="7de62-228">Hozzon létre egy API-művelet</span><span class="sxs-lookup"><span data-stu-id="7de62-228">Create an API operation</span></span>

<span data-ttu-id="7de62-229">Most már készen áll a toocreate az API Management egy műveletet a rendszer az adott külső ügyfelek használata toocommunicate hello hello Service Fabric-fürt futtatja az ASP.NET Core állapotmentes szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="7de62-229">Now we're ready toocreate an operation in API Management that external clients use toocommunicate with hello ASP.NET Core stateless service running in hello Service Fabric cluster.</span></span>

 1. <span data-ttu-id="7de62-230">Jelentkezzen be Azure-portálon toohello, és keresse meg a tooyour API-kezelés szolgáltatás telepítését.</span><span class="sxs-lookup"><span data-stu-id="7de62-230">Log in toohello Azure portal and navigate tooyour API Management service deployment.</span></span>
 2. <span data-ttu-id="7de62-231">Hello API-kezelés szolgáltatás panelen válassza ki a **API-k – előzetes**</span><span class="sxs-lookup"><span data-stu-id="7de62-231">In hello API Management service blade, select **APIs - Preview**</span></span>
 3. <span data-ttu-id="7de62-232">Adja hozzá egy új API hello kattintva **üres API** mezőbe, majd kitöltésével hello párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="7de62-232">Add a new API by clicking hello **Blank API** box and filling out hello dialog box:</span></span>

     - <span data-ttu-id="7de62-233">**Webszolgáltatás URL-címe**: A Service Fabric háttérkiszolgálókon, ez az URL-cím érték nem használható.</span><span class="sxs-lookup"><span data-stu-id="7de62-233">**Web service URL**: For Service Fabric backends, this URL value is not used.</span></span> <span data-ttu-id="7de62-234">Bármely érték itt helyezhet el.</span><span class="sxs-lookup"><span data-stu-id="7de62-234">You can put any value here.</span></span> <span data-ttu-id="7de62-235">A jelen oktatóanyag esetében használja: `http://servicefabric`.</span><span class="sxs-lookup"><span data-stu-id="7de62-235">For this tutorial, use: `http://servicefabric`.</span></span>
     - <span data-ttu-id="7de62-236">**Név**: bármely nevezze el az API-t.</span><span class="sxs-lookup"><span data-stu-id="7de62-236">**Name**: Provide any name for your API.</span></span> <span data-ttu-id="7de62-237">A jelen oktatóanyag esetében használja `Service Fabric App`.</span><span class="sxs-lookup"><span data-stu-id="7de62-237">For this tutorial, use `Service Fabric App`.</span></span>
     - <span data-ttu-id="7de62-238">**URL-séma**: jelölje be a HTTP, HTTPS vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="7de62-238">**URL scheme**: Select either HTTP, HTTPS, or both.</span></span> <span data-ttu-id="7de62-239">A jelen oktatóanyag esetében használja `both`.</span><span class="sxs-lookup"><span data-stu-id="7de62-239">For this tutorial, use `both`.</span></span>
     - <span data-ttu-id="7de62-240">**API URL-cím utótag**: Adjon meg egy utótagot az API felületen.</span><span class="sxs-lookup"><span data-stu-id="7de62-240">**API URL Suffix**: Provide a suffix for our API.</span></span> <span data-ttu-id="7de62-241">A jelen oktatóanyag esetében használja `myapp`.</span><span class="sxs-lookup"><span data-stu-id="7de62-241">For this tutorial, use `myapp`.</span></span>
 
 4. <span data-ttu-id="7de62-242">Hello API létrehozása után kattintson **+ Hozzáadás művelet** tooadd egy előtér-API műveletet.</span><span class="sxs-lookup"><span data-stu-id="7de62-242">Once hello API is created, click **+ Add operation** tooadd a front-end API operation.</span></span> <span data-ttu-id="7de62-243">Töltse ki hello értékeket:</span><span class="sxs-lookup"><span data-stu-id="7de62-243">Fill out hello values:</span></span>
    
     - <span data-ttu-id="7de62-244">**URL-cím**: válasszon `GET` , és adjon meg egy URL-címe hello API.</span><span class="sxs-lookup"><span data-stu-id="7de62-244">**URL**: Select `GET` and provide a URL path for hello API.</span></span> <span data-ttu-id="7de62-245">A jelen oktatóanyag esetében használja `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="7de62-245">For this tutorial, use `/api/values`.</span></span>
     
       <span data-ttu-id="7de62-246">Alapértelmezés szerint a hello URL-címe itt megadott toohello Service Fabric háttérszolgáltatást küldött hello URL-címe.</span><span class="sxs-lookup"><span data-stu-id="7de62-246">By default, hello URL path specified here is hello URL path sent toohello backend Service Fabric service.</span></span> <span data-ttu-id="7de62-247">Ha hello azonos URL-címe Itt a szolgáltatás által használt, ebben az esetben `/api/values`, majd a hello művelet további módosítás nélkül működik.</span><span class="sxs-lookup"><span data-stu-id="7de62-247">If you use hello same URL path here that your service uses, in this case `/api/values`, then hello operation works without further modification.</span></span> <span data-ttu-id="7de62-248">Megadhat egy URL-címe itt háttéralkalmazásának Service Fabric-szolgáltatás által használt hello URL-címe eltérő ebben az esetben a program elérési újraírási kell toospecify a művelet házirend később.</span><span class="sxs-lookup"><span data-stu-id="7de62-248">You may also specify a URL path here that is different from hello URL path used by your backend Service Fabric service, in which case you will also need toospecify a path rewrite in your operation policy later.</span></span>
     - <span data-ttu-id="7de62-249">**Megjelenített név**: Adjon meg egy tetszőleges nevet hello API.</span><span class="sxs-lookup"><span data-stu-id="7de62-249">**Display name**: Provide any name for hello API.</span></span> <span data-ttu-id="7de62-250">A jelen oktatóanyag esetében használja `Values`.</span><span class="sxs-lookup"><span data-stu-id="7de62-250">For this tutorial, use `Values`.</span></span>

## <a name="configure-a-backend-policy"></a><span data-ttu-id="7de62-251">Háttér-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7de62-251">Configure a backend policy</span></span>

<span data-ttu-id="7de62-252">hello háttér házirend kötelékek mindent együtt.</span><span class="sxs-lookup"><span data-stu-id="7de62-252">hello backend policy ties everything together.</span></span> <span data-ttu-id="7de62-253">Ez a szolgáltatás toowhich rendszer kérést átirányítja a Service Fabric hello háttér konfigurálására szolgáló.</span><span class="sxs-lookup"><span data-stu-id="7de62-253">This is where you configure hello backend Service Fabric service toowhich requests are routed.</span></span> <span data-ttu-id="7de62-254">A házirend tooany API művelet is alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="7de62-254">You can apply this policy tooany API operation.</span></span> <span data-ttu-id="7de62-255">Hello [háttérkonfiguráció a Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) hello következő kérelem útválasztási vezérlők biztosítja:</span><span class="sxs-lookup"><span data-stu-id="7de62-255">hello [backend configuration for Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) provides hello following request routing controls:</span></span> 
 - <span data-ttu-id="7de62-256">Példány kiválasztása szolgáltatás a Service Fabric szolgáltatás példány neve, vagy szoftveresen kötött megadásával (például `"fabric:/myapp/myservice"`), és létre hello HTTP-kérelem (például `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span><span class="sxs-lookup"><span data-stu-id="7de62-256">Service instance selection by specifying a Service Fabric service instance name, either hardcoded (for example, `"fabric:/myapp/myservice"`) or generated from hello HTTP request (for example, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span></span>
 - <span data-ttu-id="7de62-257">A Service Fabric particionálási sémát használó partíciós kulcs létrehozása általi megoldása partíció.</span><span class="sxs-lookup"><span data-stu-id="7de62-257">Partition resolution by generating a partition key using any Service Fabric partitioning scheme.</span></span>
 - <span data-ttu-id="7de62-258">Állapotalapú szolgáltatások replika kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="7de62-258">Replica selection for stateful services.</span></span>
 - <span data-ttu-id="7de62-259">Megoldási ismételje meg a feltételeket, amelyek lehetővé teszik toospecify hello feltételek újra feloldani a helyét, és küldje el újra a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="7de62-259">Resolution retry conditions that allow you toospecify hello conditions for re-resolving a service location and resending a request.</span></span>

<span data-ttu-id="7de62-260">A jelen oktatóanyag esetében győződjön meg arról, hogy útvonalak közvetlenül toohello ASP.NET Core állapotmentes szolgáltatások korábban telepített kérelmek háttér szabályzat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="7de62-260">For this tutorial, create a backend policy that routes requests directly toohello ASP.NET Core stateless service deployed earlier:</span></span>

 1. <span data-ttu-id="7de62-261">Válassza ki, és szerkesztheti a hello **házirendek bejövő** a hello `Values` hello Szerkesztés ikonra kattint, és jelölje be a művelet **kódnézetben**.</span><span class="sxs-lookup"><span data-stu-id="7de62-261">Select and edit hello **inbound policies** for hello `Values` operation by clicking hello edit icon, and then selecting **Code View**.</span></span>
 2. <span data-ttu-id="7de62-262">A kód hello házirendszerkesztő, adjon hozzá egy `set-backend-service` házirend a bejövő házirendek, ahogy az itt látható, és kattintson a hello **mentése** gombra:</span><span class="sxs-lookup"><span data-stu-id="7de62-262">In hello policy code editor, add a `set-backend-service` policy under inbound policies as shown here and click hello **Save** button:</span></span>
    
    ```xml
    <policies>
      <inbound>
        <base/>
        <set-backend-service 
           backend-id="servicefabric"
           sf-service-instance-name="fabric:/ApiApplication/WebApiService"
           sf-resolve-condition="@((int)context.Response.StatusCode != 200)" />
      </inbound>
      <backend>
        <base/>
      </backend>
      <outbound>
        <base/>
      </outbound>
    </policies>
    ```

<span data-ttu-id="7de62-263">A Service Fabric háttér-házirend attribútumainak teljes készletét, tekintse meg a toohello [API Management háttér-dokumentáció](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span><span class="sxs-lookup"><span data-stu-id="7de62-263">For a full set of Service Fabric back-end policy attributes, refer toohello [API Management back-end documentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span></span>

### <a name="add-hello-api-tooa-product"></a><span data-ttu-id="7de62-264">Hello API tooa termék hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7de62-264">Add hello API tooa product.</span></span> 

<span data-ttu-id="7de62-265">Hello API hívása előtt, ahol biztosíthat hozzáférést toousers tooa termék hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="7de62-265">Before you can call hello API, it must be added tooa product where you can grant access toousers.</span></span> 

 1. <span data-ttu-id="7de62-266">Hello API-kezelés szolgáltatás, válassza ki **termékek - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="7de62-266">In hello API Management service, select **Products - PREVIEW**.</span></span>
 2. <span data-ttu-id="7de62-267">Alapértelmezés szerint az API Management szolgáltatók két termékek: alapszintű és a korlátlan.</span><span class="sxs-lookup"><span data-stu-id="7de62-267">By default, API Management providers two products: Starter and Unlimited.</span></span> <span data-ttu-id="7de62-268">Válassza ki a hello korlátlan terméket.</span><span class="sxs-lookup"><span data-stu-id="7de62-268">Select hello Unlimited product.</span></span>
 3. <span data-ttu-id="7de62-269">Válassza ki az API-k.</span><span class="sxs-lookup"><span data-stu-id="7de62-269">Select APIs.</span></span>
 4. <span data-ttu-id="7de62-270">Kattintson a hello **+ Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7de62-270">Click hello **+ Add** button.</span></span>
 5. <span data-ttu-id="7de62-271">Jelölje be hello `Service Fabric App` API hello előző lépésekben létrehozott, és kattintson a hello **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="7de62-271">Select hello `Service Fabric App` API you created in hello previous steps and click hello **Select** button.</span></span>

### <a name="test-it"></a><span data-ttu-id="7de62-272">Tesztelheti</span><span class="sxs-lookup"><span data-stu-id="7de62-272">Test it</span></span>

<span data-ttu-id="7de62-273">Most megpróbálhatja üzenetet küld egy kérelem tooyour háttér-szolgáltatás a Service Fabric API Management keresztül közvetlenül hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7de62-273">You can now try sending a request tooyour back-end service in Service Fabric through API Management directly from hello Azure portal.</span></span>

 1. <span data-ttu-id="7de62-274">Hello API-kezelés szolgáltatás, válassza ki **API - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="7de62-274">In hello API Management service, select **API - PREVIEW**.</span></span>
 2. <span data-ttu-id="7de62-275">A hello `Service Fabric App` hello előző lépésekben létrehozott API kiválasztása hello **teszt** fülre.</span><span class="sxs-lookup"><span data-stu-id="7de62-275">In hello `Service Fabric App` API you created in hello previous steps, select hello **Test** tab.</span></span>
 3. <span data-ttu-id="7de62-276">Kattintson a hello **küldése** gomb toosend tesztelési kérelem toohello háttérszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7de62-276">Click hello **Send** button toosend a test request toohello backend service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7de62-277">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7de62-277">Next steps</span></span>

<span data-ttu-id="7de62-278">Ezen a ponton rendelkeznie kell egy egyszerű telepítése a Service Fabric- és API-kezelés.</span><span class="sxs-lookup"><span data-stu-id="7de62-278">At this point, you should have a basic setup with Service Fabric and API Management.</span></span>

<span data-ttu-id="7de62-279">Ez az oktatóanyag a Service Fabric-fürt beállítása gyorsan tooget alapvető tanúsítvány alapú hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="7de62-279">This tutorial uses basic certificate-based user authentication for your Service Fabric cluster tooget you set up quickly.</span></span> <span data-ttu-id="7de62-280">A Service Fabric-fürt speciális felhasználóhitelesítést célszerű a [Azure Active Directory hitelesítési](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span><span class="sxs-lookup"><span data-stu-id="7de62-280">More advanced user authentication for your Service Fabric cluster is preferable with [Azure Active Directory authentication](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span></span> 

<span data-ttu-id="7de62-281">Ezt követően [létrehozása és speciális termékbeállítások konfigurálása az Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare az alkalmazás a valós életben-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="7de62-281">Next, [create and configure advanced product settings in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) tooprepare your application for real world traffic.</span></span>

<!-- links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/

[network-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.json
[network-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/network.parameters.json

[apim-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.json
[apim-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/apim.parameters.json

[cluster-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.json
[cluster-parameters-arm]:https://github.com/Azure-Samples/service-fabric-api-management/blob/master/cluster.parameters.json


<!-- pics -->
[sf-apim-topology-overview]: ./media/service-fabric-api-management-quickstart/sf-apim-topology-overview.png
