---
title: "Az API Management – első lépések az Azure Service Fabric |} Microsoft Docs"
description: "Ez az útmutató bemutatja, hogyan gyorsan Ismerkedés az Azure API Management és a Service Fabric."
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
ms.openlocfilehash: e9f44d8a43d274768f43261fea68f0da9c681ae1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="service-fabric-with-azure-api-management-quick-start"></a><span data-ttu-id="0500c-103">A Service Fabric az Azure API Management – első lépések</span><span class="sxs-lookup"><span data-stu-id="0500c-103">Service Fabric with Azure API Management Quick Start</span></span>

<span data-ttu-id="0500c-104">Ez az útmutató bemutatja, hogyan Azure API Management a Service Fabric beállítása és konfigurálása az első API művelet forgalmat küldeni a Service Fabric háttér-szolgáltatásaihoz.</span><span class="sxs-lookup"><span data-stu-id="0500c-104">This guide shows you how to set up Azure API Management with Service Fabric and configure your first API operation to send traffic to back-end services in Service Fabric.</span></span> <span data-ttu-id="0500c-105">A Service Fabric Azure API Management-forgatókönyvekkel kapcsolatos további tudnivalókért tekintse meg a [áttekintése](service-fabric-api-management-overview.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="0500c-105">To learn more about Azure API Management scenarios with Service Fabric, see the [overview](service-fabric-api-management-overview.md) article.</span></span> 

## <a name="deploy-api-management-and-service-fabric-to-azure"></a><span data-ttu-id="0500c-106">Az Azure API Management és a Service Fabric telepítése</span><span class="sxs-lookup"><span data-stu-id="0500c-106">Deploy API Management and Service Fabric to Azure</span></span>

<span data-ttu-id="0500c-107">Az első lépés egy megosztott virtuális hálózatot az Azure API Management és a Service Fabric-fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="0500c-107">The first step is to deploy API Management and a Service Fabric cluster to Azure in a shared Virtual Network.</span></span> <span data-ttu-id="0500c-108">Ez lehetővé teszi az API Management és közvetlenül kommunikáljanak a Service Fabric ezért szolgáltatásészlelés, a szolgáltatás partíció megoldása és a forgalom közvetlenül által a háttérszolgáltatáshoz képes végezni a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0500c-108">This allows API Management to communicate directly with Service Fabric so it can perform service discovery, service partition resolution, and forward traffic directly to any backend service in Service Fabric.</span></span>

### <a name="topology"></a><span data-ttu-id="0500c-109">topológia</span><span class="sxs-lookup"><span data-stu-id="0500c-109">Topology</span></span>

<span data-ttu-id="0500c-110">Ez az útmutató a következő topológia telepíti az Azure-ba, amelyben az API Management és a Service Fabric-alhálózataihoz az azonos virtuális hálózatban szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="0500c-110">This guide deploys the following topology to Azure in which API Management and Service Fabric are in subnets of the same Virtual Network:</span></span>

 ![Képfelirat][sf-apim-topology-overview]

<span data-ttu-id="0500c-112">Gyorsan és egyszerűen a Resource Manager-sablonok minden központi telepítési lépés áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="0500c-112">To get started quickly, Resource Manager templates are provided for each deployment step:</span></span>

 - <span data-ttu-id="0500c-113">Hálózati topológia:</span><span class="sxs-lookup"><span data-stu-id="0500c-113">Network topology:</span></span>
    - <span data-ttu-id="0500c-114">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-114">[network.json][network-arm]</span></span>
    - <span data-ttu-id="0500c-115">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-115">[network.parameters.json][network-parameters-arm]</span></span>
 - <span data-ttu-id="0500c-116">Service Fabric-fürt:</span><span class="sxs-lookup"><span data-stu-id="0500c-116">Service Fabric cluster:</span></span>
    - <span data-ttu-id="0500c-117">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-117">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="0500c-118">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-118">[cluster.parameters.json][cluster-parameters-arm]</span></span>
 - <span data-ttu-id="0500c-119">API-kezelés:</span><span class="sxs-lookup"><span data-stu-id="0500c-119">API Management:</span></span>
    - <span data-ttu-id="0500c-120">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-120">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="0500c-121">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-121">[apim.parameters.json][apim-parameters-arm]</span></span>

### <a name="sign-in-to-azure-and-select-your-subscription"></a><span data-ttu-id="0500c-122">Jelentkezzen be az Azure-ba, és jelölje ki az előfizetését</span><span class="sxs-lookup"><span data-stu-id="0500c-122">Sign in to Azure and select your subscription</span></span>

<span data-ttu-id="0500c-123">Ez az útmutató használ [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="0500c-123">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="0500c-124">Amikor egy új PowerShell-munkamenetet indít el, jelentkezzen be az Azure-fiókjával, és jelölje ki az előfizetését, Azure parancsok végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="0500c-124">When you start a new PowerShell session, sign in to your Azure account and select your subscription before you execute Azure commands.</span></span>
 
<span data-ttu-id="0500c-125">Jelentkezzen be az Azure-fiókjával:</span><span class="sxs-lookup"><span data-stu-id="0500c-125">Sign in to your Azure account:</span></span>

```powershell
PS > Login-AzureRmAccount
```

<span data-ttu-id="0500c-126">Jelölje ki az előfizetését:</span><span class="sxs-lookup"><span data-stu-id="0500c-126">Select your subscription:</span></span>

```powershell
PS > Get-AzureRmSubscription
PS > Set-AzureRmContext -SubscriptionId <guid>
```

### <a name="create-a-resource-group"></a><span data-ttu-id="0500c-127">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="0500c-127">Create a resource group</span></span>

<span data-ttu-id="0500c-128">Hozzon létre egy új erőforráscsoportot az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="0500c-128">Create a new resource group for your deployment.</span></span> <span data-ttu-id="0500c-129">Adjon neki egy nevet és egy helyet.</span><span class="sxs-lookup"><span data-stu-id="0500c-129">Give it a name and a location.</span></span>

```powershell
PS > New-AzureRmResourceGroup -Name <my-resource-group> -Location westus
```

### <a name="deploy-the-network-topology"></a><span data-ttu-id="0500c-130">A hálózati topológia központi telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="0500c-130">Deploy the network topology</span></span>

<span data-ttu-id="0500c-131">Az első lépés, hogy a hálózati topológia, amely az API Management és a Service Fabric-fürt telepítése beállítása.</span><span class="sxs-lookup"><span data-stu-id="0500c-131">The first step is to set up the network topology to which API Management and the Service Fabric cluster will be deployed.</span></span> <span data-ttu-id="0500c-132">A [network.json] [ network-arm] Resource Manager-sablon létrehozása egy virtuális hálózatot (VNET) két alhálózat és két hálózati biztonsági csoportok (NSG) van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0500c-132">The [network.json][network-arm] Resource Manager template is configured to create a Virtual Network (VNET) with two subnets and two Network Security Groups (NSG).</span></span> 

<span data-ttu-id="0500c-133">A [network.parameters.json] [ network-parameters-arm] paraméterek fájl tartalmazza az alhálózatok és a telepítendő API Management és a Service Fabric az NSG-ket nevét.</span><span class="sxs-lookup"><span data-stu-id="0500c-133">The [network.parameters.json][network-parameters-arm] parameters file contains the names of the subnets and NSGs that API Management and Service Fabric will be deployed to.</span></span> <span data-ttu-id="0500c-134">Ez az útmutató a paraméterértékek nem kell módosítani.</span><span class="sxs-lookup"><span data-stu-id="0500c-134">For this guide, the parameter values do not need to be changed.</span></span> <span data-ttu-id="0500c-135">Az API Management és a Service Fabric Resource Manager sablonok használata ezek érték található, ezért ha itt módosításuk, módosítania kell azokat a többi Resource Manager sablon ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="0500c-135">The API Management and Service Fabric Resource Manager templates use these values, so if they are modified here, you must modify them in the other Resource Manager templates accordingly.</span></span> 

 1. <span data-ttu-id="0500c-136">Töltse le a következő Resource Manager sablon és a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0500c-136">Download the following Resource Manager template and parameters file:</span></span>

    - <span data-ttu-id="0500c-137">[Network.JSON][network-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-137">[network.json][network-arm]</span></span>
    - <span data-ttu-id="0500c-138">[Network.Parameters.JSON][network-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-138">[network.parameters.json][network-parameters-arm]</span></span>

 2. <span data-ttu-id="0500c-139">A következő PowerShell-parancs segítségével a hálózat beállítása a Resource Manager sablonnal és paraméterfájlokkal fájlok telepítése:</span><span class="sxs-lookup"><span data-stu-id="0500c-139">Use the following PowerShell command to deploy the Resource Manager template and parameter files for the network setup:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\network.json -TemplateParameterFile .\network.parameters.json -Verbose
    ```

### <a name="deploy-the-service-fabric-cluster"></a><span data-ttu-id="0500c-140">A Service Fabric-fürt központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0500c-140">Deploy the Service Fabric cluster</span></span>

<span data-ttu-id="0500c-141">Ha a hálózati erőforrások végzett központi telepítése, a következő lépés a Service Fabric-fürt központi telepítése a virtuális hálózat, az alhálózat és a Service Fabric-fürt számára kijelölt NSG.</span><span class="sxs-lookup"><span data-stu-id="0500c-141">Once the network resources have finished deploying, the next step is to deploy a Service Fabric cluster to the VNET in the subnet and NSG designated for the Service Fabric cluster.</span></span> <span data-ttu-id="0500c-142">Ebben az oktatóanyagban a Service Fabric Resource Manager-sablon a virtuális Hálózatot, alhálózatot és az előző lépésben beállított NSG-neveket használja az előre konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0500c-142">For this tutorial, the Service Fabric Resource Manager template is pre-configured to use the names of the VNET, subnet, and NSG that you set up in the previous step.</span></span> 

<span data-ttu-id="0500c-143">A Service Fabric-fürt Resource Manager sablon hozzon létre egy biztonságos fürtöt a biztonsági tanúsítvány van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0500c-143">The Service Fabric cluster Resource Manager template is configured to create a secure cluster with certificate security.</span></span> <span data-ttu-id="0500c-144">Ezzel a tanúsítvánnyal a fürt a csomópontok kommunikáció biztosításához és a Service Fabric-fürt felhasználói hozzáférésének kezelése.</span><span class="sxs-lookup"><span data-stu-id="0500c-144">The certificate is used to secure node-to-node communication for your cluster and to manage user access to your Service Fabric cluster.</span></span> <span data-ttu-id="0500c-145">Az API Management ezt a tanúsítványt használja a Service Fabric-szolgáltatás a szolgáltatás felderítése eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0500c-145">API Management uses this certificate to access  the Service Fabric Naming Service for service discovery.</span></span>

<span data-ttu-id="0500c-146">Ez a lépés szükséges tanúsítvánnyal rendelkező Key Vault a fürt biztonsági.</span><span class="sxs-lookup"><span data-stu-id="0500c-146">This step requires having a certificate in Key Vault for cluster security.</span></span> <span data-ttu-id="0500c-147">Key Vault használó biztonságos fürtök beállításával kapcsolatos további információkért lásd: [Ez az útmutató a fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md)</span><span class="sxs-lookup"><span data-stu-id="0500c-147">For more information on setting up a secure cluster with Key Vault, see [this guide on creating a cluster in Azure using Resource Manager](service-fabric-cluster-creation-via-arm.md)</span></span>

> [!NOTE]
> <span data-ttu-id="0500c-148">Azure Active Directory-hitelesítés mellett a hozzáférés a fürthöz használt tanúsítvány adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="0500c-148">You may add Azure Active Directory authentication in addition to the certificate used for cluster access.</span></span> <span data-ttu-id="0500c-149">Az Azure Active Directory felhasználói hozzáférése a Service Fabric-fürt kezeléséhez ajánlott módja, de nincs szükség az oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="0500c-149">Azure Active Directory is the recommended way to manage user access to your Service Fabric cluster, but is not necessary to complete this tutorial.</span></span> <span data-ttu-id="0500c-150">Egy tanúsítványra szükség mindkét módszer fürt a csomópontok biztonság és az Azure API Management hitelesítést, amely jelenleg nem támogatja az Azure Active Directoryban a Service Fabric háttérkiszolgáló hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0500c-150">A certificate is required either way for cluster node-to-node security and for Azure API Management authentication, which currently does not support authenticating with Azure Active Directory for a Service Fabric backend.</span></span>

 1. <span data-ttu-id="0500c-151">Töltse le a következő Resource Manager sablon és a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0500c-151">Download the following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="0500c-152">[cluster.JSON][cluster-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-152">[cluster.json][cluster-arm]</span></span>
    - <span data-ttu-id="0500c-153">[cluster.Parameters.JSON][cluster-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-153">[cluster.parameters.json][cluster-parameters-arm]</span></span>

 2. <span data-ttu-id="0500c-154">Adja meg az üres paraméterei a `cluster.parameters.json` fájl az üzembe helyezés többek között a [Key Vault információk](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) a fürt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="0500c-154">Fill in the empty parameters in the `cluster.parameters.json` file for your deployment, including the [Key Vault information](service-fabric-cluster-creation-via-arm.md#set-up-a-key-vault) for your cluster certificate.</span></span>

 3. <span data-ttu-id="0500c-155">A következő PowerShell-parancs segítségével a Resource Manager sablonnal és paraméterfájlokkal fájlokat, hogy a Service Fabric-fürt központi telepítése:</span><span class="sxs-lookup"><span data-stu-id="0500c-155">Use the following PowerShell command to deploy the Resource Manager template and parameter files to create the Service Fabric cluster:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\cluster.json -TemplateParameterFile .\cluster.parameters.json -Verbose
    ```

### <a name="deploy-api-management"></a><span data-ttu-id="0500c-156">Az API Management telepítése</span><span class="sxs-lookup"><span data-stu-id="0500c-156">Deploy API Management</span></span>

<span data-ttu-id="0500c-157">Végezetül az alhálózat a virtuális hálózat és az API Management kijelölt NSG API Management központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="0500c-157">Finally, deploy API Management to the VNET in the subnet and NSG designated for API Management.</span></span> <span data-ttu-id="0500c-158">Nem kell a Service Fabric fürt üzembe helyezés befejeződik az API Management üzembe helyezése előtt várja meg.</span><span class="sxs-lookup"><span data-stu-id="0500c-158">You do not need to wait for the Service Fabric cluster deployment to finish before deploying API Management.</span></span> 

<span data-ttu-id="0500c-159">Ebben az oktatóanyagban az API Management Resource Manager-sablon a virtuális Hálózatot, alhálózatot és az előző lépésben beállított NSG-neveket használja az előre konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0500c-159">For this tutorial, the API Management Resource Manager template is pre-configured to use the names of the VNET, subnet, and NSG that you set up in the previous step.</span></span> 

 1. <span data-ttu-id="0500c-160">Töltse le a következő Resource Manager sablon és a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0500c-160">Download the following Resource Manager template and parameters file:</span></span>
 
    - <span data-ttu-id="0500c-161">[APIM.JSON][apim-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-161">[apim.json][apim-arm]</span></span>
    - <span data-ttu-id="0500c-162">[APIM.Parameters.JSON][apim-parameters-arm]</span><span class="sxs-lookup"><span data-stu-id="0500c-162">[apim.parameters.json][apim-parameters-arm]</span></span>

 2. <span data-ttu-id="0500c-163">Adja meg az üres paraméterei a `apim.parameters.json` az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="0500c-163">Fill in the empty parameters in the `apim.parameters.json` for your deployment.</span></span>

 3. <span data-ttu-id="0500c-164">A következő PowerShell-parancs segítségével a Resource Manager sablonnal és paraméterfájlokkal fájlok telepítése az API Management:</span><span class="sxs-lookup"><span data-stu-id="0500c-164">Use the following PowerShell command to deploy the Resource Manager template and parameter files for API Management:</span></span>

    ```powershell
    PS > New-AzureRmResourceGroupDeployment -ResourceGroupName <my-resource-group> -TemplateFile .\apim.json -TemplateParameterFile .\apim.parameters.json -Verbose
    ```

## <a name="configure-api-management"></a><span data-ttu-id="0500c-165">API-kezelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0500c-165">Configure API Management</span></span>

<span data-ttu-id="0500c-166">Után az API Management és a Service Fabric-fürt vannak telepítve, a Service Fabric háttér állíthatja be az API Management.</span><span class="sxs-lookup"><span data-stu-id="0500c-166">Once your API Management and Service Fabric cluster are deployed, you can configure a Service Fabric backend in API Management.</span></span> <span data-ttu-id="0500c-167">Ez lehetővé teszi, hogy a szabályzatban háttér szolgáltatás forgalmat küld a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="0500c-167">This allows you to create a backend service policy that sends traffic to your Service Fabric cluster.</span></span>

### <a name="api-management-security"></a><span data-ttu-id="0500c-168">API Management biztonsági</span><span class="sxs-lookup"><span data-stu-id="0500c-168">API Management Security</span></span>

<span data-ttu-id="0500c-169">A Service Fabric háttér konfigurálásához először API biztonsági beállítások megadásához.</span><span class="sxs-lookup"><span data-stu-id="0500c-169">To configure the Service Fabric backend, you first need to configure API Management security settings.</span></span> <span data-ttu-id="0500c-170">A biztonsági beállítások konfigurálása, nyissa meg az API Management szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="0500c-170">To configure security settings, go to your API Management service in the Azure portal.</span></span>

#### <a name="enable-the-api-management-rest-api"></a><span data-ttu-id="0500c-171">Az API Management REST API engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0500c-171">Enable the API Management REST API</span></span>

<span data-ttu-id="0500c-172">Az API Management REST API jelenleg az egyetlen lehetőség a háttér-szolgáltatás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="0500c-172">The API Management REST API is currently the only way to configure a backend service.</span></span> <span data-ttu-id="0500c-173">Az első lépés, hogy engedélyezze az API Management REST API-t és a biztonság.</span><span class="sxs-lookup"><span data-stu-id="0500c-173">The first step is to enable the API Management REST API and secure it.</span></span>

 1. <span data-ttu-id="0500c-174">Válassza ki az API Management szolgáltatásban **felügyeleti API - előzetes** alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="0500c-174">In the API Management service, select **Management API - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="0500c-175">Ellenőrizze a **engedélyezése API Management REST API** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="0500c-175">Check the **Enable API Management REST API** checkbox.</span></span>
 3. <span data-ttu-id="0500c-176">Megjegyzés: a felügyeleti API URL-CÍMÉT – Ez az URL-cím később fogjuk használni a Service Fabric-háttérrendszer beállítása</span><span class="sxs-lookup"><span data-stu-id="0500c-176">Note the Management API URL - this is the URL we'll use later to set up the Service Fabric backend</span></span>
 4. <span data-ttu-id="0500c-177">Generate egy **hozzáférési tokent** kiválasztásával lejárati dátummal és a kulcsot, majd kattintson a **Generate** gombra az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="0500c-177">Generate an **access Token** by selecting an expiry date and a key, then click the **Generate** button toward the bottom of the page.</span></span>
 5. <span data-ttu-id="0500c-178">Másolás a **hozzáférési jogkivonat** és mentheti – Ez a következő lépésekben fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="0500c-178">Copy the **access token** and save it - we'll use this in the following steps.</span></span> <span data-ttu-id="0500c-179">Vegye figyelembe, hogy ez különbözik az elsődleges és másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0500c-179">Note this is different from the primary key and secondary key.</span></span>

#### <a name="upload-a-service-fabric-client-certificate"></a><span data-ttu-id="0500c-180">A Service Fabric ügyfél-tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="0500c-180">Upload a Service Fabric client certificate</span></span>

<span data-ttu-id="0500c-181">A szolgáltatás felderítése használ a fürt eléréséhez használt tanúsítványt a Service Fabric-fürt API-kezelés kell hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="0500c-181">API Management must authenticate with your Service Fabric cluster for service discovery using a client certificate that has access to your cluster.</span></span> <span data-ttu-id="0500c-182">Az egyszerűség kedvéért ez az oktatóanyag a Service Fabric-fürt, amely alapértelmezés szerint a fürt eléréséhez használható létrehozásakor megadott ugyanazt a tanúsítványt használja.</span><span class="sxs-lookup"><span data-stu-id="0500c-182">For simplicity, this tutorial uses the same certificate specified when creating the Service Fabric cluster, which by default can be used to access your cluster.</span></span>

 1. <span data-ttu-id="0500c-183">Válassza ki az API Management szolgáltatásban **ügyféltanúsítványok - előzetes** alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="0500c-183">In the API Management service, select **Client certificates - PREVIEW** under **Security**.</span></span>
 2. <span data-ttu-id="0500c-184">Kattintson a **+ Hozzáadás** gomb</span><span class="sxs-lookup"><span data-stu-id="0500c-184">Click the **+ Add** button</span></span>
 2. <span data-ttu-id="0500c-185">Válassza ki a titkos kulcs fájlját (.pfx) a fürt tanúsítvány, a Service Fabric-fürt létrehozásakor megadott, adjon neki egy nevet, és adja meg a titkos kulcs jelszava.</span><span class="sxs-lookup"><span data-stu-id="0500c-185">Select the private key file (.pfx) of the cluster certificate that you specified when creating your Service Fabric cluster, give it a name, and provide the private key password.</span></span>

> [!NOTE]
> <span data-ttu-id="0500c-186">Ez az oktatóanyag az ügyfél-hitelesítés és a fürt-csomópontok biztonsági ugyanazt a tanúsítványt használja.</span><span class="sxs-lookup"><span data-stu-id="0500c-186">This tutorial uses the same certificate for client authentication and cluster node-to-node security.</span></span> <span data-ttu-id="0500c-187">Külön ügyfél-tanúsítványt használhatja, ha van a Service Fabric-fürt elérésére.</span><span class="sxs-lookup"><span data-stu-id="0500c-187">You may use a separate client certificate if you have one configured to access your Service Fabric cluster.</span></span>

### <a name="configure-the-backend"></a><span data-ttu-id="0500c-188">A háttérkiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="0500c-188">Configure the backend</span></span>

<span data-ttu-id="0500c-189">Most, hogy az API Management a biztonsági beállítások konfigurálása a Service Fabric háttér konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="0500c-189">Now that API Management security is configured, you can configure the Service Fabric backend.</span></span> <span data-ttu-id="0500c-190">A Service Fabric háttérkiszolgálókon a Service Fabric-fürt esetén a háttér ahelyett, hogy egy adott Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0500c-190">For Service Fabric backends, the Service Fabric cluster is the backend, rather than a specific Service Fabric service.</span></span> <span data-ttu-id="0500c-191">Ez lehetővé teszi egy útvonalat a fürt több szolgáltatás egyetlen házirendet.</span><span class="sxs-lookup"><span data-stu-id="0500c-191">This allows a single policy to route to more than one service in the cluster.</span></span>

<span data-ttu-id="0500c-192">Ebben a lépésben az előző lépésben az API Management feltöltött a fürt tanúsítványt igényel a hozzáférési jogkivonat korábban létrehozott és az ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="0500c-192">This step requires the access token you generated earlier and the thumbprint for your cluster certificate you uploaded to API Management in the previous step.</span></span>

> [!NOTE]
> <span data-ttu-id="0500c-193">Az API Management az előző lépésben használt külön ügyfél-tanúsítványt, ha az ügyféltanúsítványt a fürt tanúsítvány ujjlenyomata ebben a lépésben mellett kell az ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="0500c-193">If you used a separate client certificate in the previous step for API Management, you need the thumbprint for the client certificate in addition to the cluster certificate thumbprint in this step.</span></span>

<span data-ttu-id="0500c-194">A következő HTTP PUT kérelmek az API Management API URL-címet korábban feljegyzett, ha engedélyezi az API Management REST API-t konfigurálja a Service Fabric háttérszolgáltatáshoz küldése</span><span class="sxs-lookup"><span data-stu-id="0500c-194">Send the following HTTP PUT request to the API Management API URL you noted earlier when enabling the API Management REST API to configure the Service Fabric backend service.</span></span> <span data-ttu-id="0500c-195">Megjelenik egy `HTTP 201 Created` választ, ha a parancs sikeres.</span><span class="sxs-lookup"><span data-stu-id="0500c-195">You should see an `HTTP 201 Created` response when the command succeeds.</span></span> <span data-ttu-id="0500c-196">Minden mező további információkért lásd: az API Management [háttér-API referenciadokumentációt](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span><span class="sxs-lookup"><span data-stu-id="0500c-196">For more information on each field, see the API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend).</span></span>

<span data-ttu-id="0500c-197">HTTP-parancs és az URL-címe:</span><span class="sxs-lookup"><span data-stu-id="0500c-197">HTTP command and URL:</span></span>
```http
PUT https://your-apim.management.azure-api.net/backends/servicefabric?api-version=2016-10-10
```

<span data-ttu-id="0500c-198">Kérelem fejlécei:</span><span class="sxs-lookup"><span data-stu-id="0500c-198">Request headers:</span></span>
```http
Authorization: SharedAccessSignature <your access token>
Content-Type: application/json
```

<span data-ttu-id="0500c-199">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="0500c-199">Request body:</span></span>
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

<span data-ttu-id="0500c-200">A **URL-cím** itt paraméter értéke a teljes szolgáltatás a szolgáltatás nevét a a fürt összes kérelem átirányított alapértelmezés szerint ha a háttérkiszolgáló házirendben megadott szolgáltatásnév nem.</span><span class="sxs-lookup"><span data-stu-id="0500c-200">The **url** parameter here is a fully-qualified service name of a service in your cluster that all requests are routed to by default if no service name is specified in a backend policy.</span></span> <span data-ttu-id="0500c-201">Használhat egy hamis nevét, például a "fabric: / hamis/szolgáltatás" Ha nem tervezi, hogy a tartalék szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0500c-201">You may use a fake service name, such as "fabric:/fake/service" if you do not intend to have a fallback service.</span></span>

<span data-ttu-id="0500c-202">Tekintse meg az API Management [háttér-API referenciadokumentációt](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) kapcsolatban további részleteket a minden mező.</span><span class="sxs-lookup"><span data-stu-id="0500c-202">Refer to the API Management [backend API reference documentation](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#a-namebackenda-backend) for more details on each field.</span></span>

#### <a name="example"></a><span data-ttu-id="0500c-203">Példa</span><span class="sxs-lookup"><span data-stu-id="0500c-203">Example</span></span>

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

## <a name="deploy-a-service-fabric-back-end-service"></a><span data-ttu-id="0500c-204">A Service Fabric háttér-szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="0500c-204">Deploy a Service Fabric back-end service</span></span>

<span data-ttu-id="0500c-205">Most, hogy a Service Fabric egy háttéralkalmazását az API Management konfigurálva, az API-k esetében, amelyek forgalmat küldeni a Service Fabric-szolgáltatásokhoz hozhat létre a háttér-házirendeket.</span><span class="sxs-lookup"><span data-stu-id="0500c-205">Now that you have the Service Fabric configured as a backend to API Management, you can author backend policies for your APIs that send traffic to your Service Fabric services.</span></span> <span data-ttu-id="0500c-206">De először meg kell a Service Fabric-kérelmek fogadásához futó szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="0500c-206">But first you need a service running in Service Fabric to accept requests.</span></span>

### <a name="create-a-service-fabric-service-with-an-http-endpoint"></a><span data-ttu-id="0500c-207">A Service Fabric-szolgáltatás egy HTTP-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="0500c-207">Create a Service Fabric service with an HTTP endpoint</span></span>

<span data-ttu-id="0500c-208">Ebben az oktatóanyagban létrehozunk egy alapszintű állapotmentes ASP.NET Core megbízható szolgáltatás az alapértelmezett webes API projektet sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="0500c-208">For this tutorial, we'll create a basic stateless ASP.NET Core Reliable Service using the default Web API project template.</span></span> <span data-ttu-id="0500c-209">Ezzel létrehoz egy HTTP-végpont a szolgáltatás, amely akkor lesz Azure API Management elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="0500c-209">This creates an HTTP endpoint for your service, which you'll expose through Azure API Management:</span></span>

```
/api/values
```

<span data-ttu-id="0500c-210">Első lépésként [állítja be a fejlesztési környezetet az ASP.NET Core fejlesztési](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="0500c-210">Start by [setting up your development environment for ASP.NET Core development](service-fabric-add-a-web-frontend.md#set-up-your-environment-for-aspnet-core).</span></span>

<span data-ttu-id="0500c-211">Miután beállította a fejlesztőkörnyezetet, indítsa el a Visual Studio rendszergazdaként, és az ASP.NET Core szolgáltatás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="0500c-211">Once your development environment is set up, start Visual Studio as Administrator and create an ASP.NET Core service:</span></span>

 1. <span data-ttu-id="0500c-212">A Visual Studio alkalmazásban válassza ki a fájl -> Új projekt.</span><span class="sxs-lookup"><span data-stu-id="0500c-212">In Visual Studio, select File -> New Project.</span></span>
 2. <span data-ttu-id="0500c-213">Válassza ki a Cloud Service Fabric-alkalmazás sablont, és adjon neki nevet **"ApiApplication"**.</span><span class="sxs-lookup"><span data-stu-id="0500c-213">Select the Service Fabric Application template under Cloud and name it **"ApiApplication"**.</span></span>
 3. <span data-ttu-id="0500c-214">Válassza ki az ASP.NET Core szolgáltatás sablont, és a nevet a projektnek **"WebApiService"**.</span><span class="sxs-lookup"><span data-stu-id="0500c-214">Select the ASP.NET Core service template and name the project **"WebApiService"**.</span></span>
 4. <span data-ttu-id="0500c-215">Válassza a webes API-t az ASP.NET Core 1.1 projekt sablont.</span><span class="sxs-lookup"><span data-stu-id="0500c-215">Select the Web API ASP.NET Core 1.1 project template.</span></span>
 5. <span data-ttu-id="0500c-216">A projekt létrehozása után nyissa meg a `PackageRoot\ServiceManifest.xml` , és távolítsa el a `Port` attribútumot a végpont erőforrás-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="0500c-216">Once the project is created, open `PackageRoot\ServiceManifest.xml` and remove the `Port` attribute from the endpoint resource configuration:</span></span>
 
    ```xml
    <Resources>
      <Endpoints>
        <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" />
      </Endpoints>
    </Resources>
    ```

    <span data-ttu-id="0500c-217">Ez lehetővé teszi, hogy a Service Fabric az alkalmazás porttartományát, amelyet azt a hálózati biztonsági csoport révén a fürt Resource Manager sablon keresztülhaladó forgalmat felé haladjanak API Management megnyitni a dinamikus port megadásához.</span><span class="sxs-lookup"><span data-stu-id="0500c-217">This allows Service Fabric to specify a port dynamically from the application port range, which we opened through the Network Security Group in the cluster Resource Manager template, allowing traffic to flow to it from API Management.</span></span>
 
 6. <span data-ttu-id="0500c-218">Nyomja le az F5 ellenőrzése a webes API-t a Visual Studio helyileg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="0500c-218">Press F5 in Visual Studio to verify the web API is available locally.</span></span> 

    <span data-ttu-id="0500c-219">Nyissa meg Service Fabric Explorer és megtekintheti az egyes egy adott példányt az ASP.NET Core szolgáltatás alapcímét megjelenítéséhez a szolgáltatás figyeli.</span><span class="sxs-lookup"><span data-stu-id="0500c-219">Open Service Fabric Explorer and drill down to a specific instance of the ASP.NET Core service to see the base address the service is listening on.</span></span> <span data-ttu-id="0500c-220">Adja hozzá `/api/values` alap történő címet, majd nyissa meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0500c-220">Add `/api/values` to the base address and open it in a browser.</span></span> <span data-ttu-id="0500c-221">Ez elindítja a ValuesController a webes API-sablonban a Get metódust.</span><span class="sxs-lookup"><span data-stu-id="0500c-221">This invokes the Get method on the ValuesController in the Web API template.</span></span> <span data-ttu-id="0500c-222">A sablon által biztosított alapértelmezett választ, egy JSON-tömb, amely tartalmazza a két karakterláncot adja vissza:</span><span class="sxs-lookup"><span data-stu-id="0500c-222">It returns the default response that is provided by the template, a JSON array that contains two strings:</span></span>

    ```json
    ["value1", "value2"]`
    ```

    <span data-ttu-id="0500c-223">Ez a végpont fogjuk az Azure API Management keresztül teszi ki.</span><span class="sxs-lookup"><span data-stu-id="0500c-223">This is the endpoint that you'll expose through API Management in Azure.</span></span>

 7. <span data-ttu-id="0500c-224">Végül telepítse az alkalmazást az Azure-ban a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0500c-224">Finally, deploy the application to your cluster in Azure.</span></span> <span data-ttu-id="0500c-225">[Visual Studio használatával](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), kattintson a jobb gombbal a projektet, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="0500c-225">[Using Visual Studio](service-fabric-publish-app-remote-cluster.md#to-publish-an-application-using-the-publish-service-fabric-application-dialog-box), right-click the Application project and select **Publish**.</span></span> <span data-ttu-id="0500c-226">Adja meg a fürt végpontja (például `mycluster.westus.cloudapp.azure.com:19000`) az alkalmazás számára az Azure Service Fabric-fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="0500c-226">Provide your cluster endpoint (for example, `mycluster.westus.cloudapp.azure.com:19000`) to deploy the application to your Service Fabric cluster in Azure.</span></span>

<span data-ttu-id="0500c-227">Az ASP.NET Core állapotmentes szolgáltatások nevű `fabric:/ApiApplication/WebApiService` most futnia kell a Service Fabric-fürt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0500c-227">An ASP.NET Core stateless service named `fabric:/ApiApplication/WebApiService` should now be running in your Service Fabric cluster in Azure.</span></span>

## <a name="create-an-api-operation"></a><span data-ttu-id="0500c-228">Hozzon létre egy API-művelet</span><span class="sxs-lookup"><span data-stu-id="0500c-228">Create an API operation</span></span>

<span data-ttu-id="0500c-229">Most még az API Management külső ügyfelek által használt szolgáltatással való kommunikációra az ASP.NET Core állapotmentes a Service Fabric-fürt fut egy művelet létrehozására alkalmas.</span><span class="sxs-lookup"><span data-stu-id="0500c-229">Now we're ready to create an operation in API Management that external clients use to communicate with the ASP.NET Core stateless service running in the Service Fabric cluster.</span></span>

 1. <span data-ttu-id="0500c-230">Jelentkezzen be az Azure-portálon, és keresse meg az API Management szolgáltatás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="0500c-230">Log in to the Azure portal and navigate to your API Management service deployment.</span></span>
 2. <span data-ttu-id="0500c-231">Az API Management szolgáltatás panelen válassza ki a **API-k – előzetes**</span><span class="sxs-lookup"><span data-stu-id="0500c-231">In the API Management service blade, select **APIs - Preview**</span></span>
 3. <span data-ttu-id="0500c-232">Egy olyan új API hozzáadni, kattintson a **üres API** mezőbe, majd töltse ki a párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="0500c-232">Add a new API by clicking the **Blank API** box and filling out the dialog box:</span></span>

     - <span data-ttu-id="0500c-233">**Webszolgáltatás URL-címe**: A Service Fabric háttérkiszolgálókon, ez az URL-cím érték nem használható.</span><span class="sxs-lookup"><span data-stu-id="0500c-233">**Web service URL**: For Service Fabric backends, this URL value is not used.</span></span> <span data-ttu-id="0500c-234">Bármely érték itt helyezhet el.</span><span class="sxs-lookup"><span data-stu-id="0500c-234">You can put any value here.</span></span> <span data-ttu-id="0500c-235">A jelen oktatóanyag esetében használja: `http://servicefabric`.</span><span class="sxs-lookup"><span data-stu-id="0500c-235">For this tutorial, use: `http://servicefabric`.</span></span>
     - <span data-ttu-id="0500c-236">**Név**: bármely nevezze el az API-t.</span><span class="sxs-lookup"><span data-stu-id="0500c-236">**Name**: Provide any name for your API.</span></span> <span data-ttu-id="0500c-237">A jelen oktatóanyag esetében használja `Service Fabric App`.</span><span class="sxs-lookup"><span data-stu-id="0500c-237">For this tutorial, use `Service Fabric App`.</span></span>
     - <span data-ttu-id="0500c-238">**URL-séma**: jelölje be a HTTP, HTTPS vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="0500c-238">**URL scheme**: Select either HTTP, HTTPS, or both.</span></span> <span data-ttu-id="0500c-239">A jelen oktatóanyag esetében használja `both`.</span><span class="sxs-lookup"><span data-stu-id="0500c-239">For this tutorial, use `both`.</span></span>
     - <span data-ttu-id="0500c-240">**API URL-cím utótag**: Adjon meg egy utótagot az API felületen.</span><span class="sxs-lookup"><span data-stu-id="0500c-240">**API URL Suffix**: Provide a suffix for our API.</span></span> <span data-ttu-id="0500c-241">A jelen oktatóanyag esetében használja `myapp`.</span><span class="sxs-lookup"><span data-stu-id="0500c-241">For this tutorial, use `myapp`.</span></span>
 
 4. <span data-ttu-id="0500c-242">Az API létrehozása után kattintson **+ Hozzáadás művelet** hozzáadása egy előtér-API-művelet.</span><span class="sxs-lookup"><span data-stu-id="0500c-242">Once the API is created, click **+ Add operation** to add a front-end API operation.</span></span> <span data-ttu-id="0500c-243">Töltse ki az értékeket:</span><span class="sxs-lookup"><span data-stu-id="0500c-243">Fill out the values:</span></span>
    
     - <span data-ttu-id="0500c-244">**URL-cím**: válasszon `GET` , és adjon meg egy URL-címet a API-t.</span><span class="sxs-lookup"><span data-stu-id="0500c-244">**URL**: Select `GET` and provide a URL path for the API.</span></span> <span data-ttu-id="0500c-245">A jelen oktatóanyag esetében használja `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="0500c-245">For this tutorial, use `/api/values`.</span></span>
     
       <span data-ttu-id="0500c-246">Alapértelmezés szerint a URL-címet itt megadott URL-címet kap a háttérkiszolgálón Service Fabric-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0500c-246">By default, the URL path specified here is the URL path sent to the backend Service Fabric service.</span></span> <span data-ttu-id="0500c-247">Ha azonos URL-címet Itt a szolgáltatás által használt, ebben az esetben `/api/values`, majd a művelet további módosítás nélkül működik.</span><span class="sxs-lookup"><span data-stu-id="0500c-247">If you use the same URL path here that your service uses, in this case `/api/values`, then the operation works without further modification.</span></span> <span data-ttu-id="0500c-248">Megadhat egy URL-címe itt, amely eltér a Service Fabric-szolgáltatás háttéralkalmazása által használt URL-címet, ebben az esetben is szüksége lesz később adjon meg egy elérési utat módosítsa úgy a művelet házirend.</span><span class="sxs-lookup"><span data-stu-id="0500c-248">You may also specify a URL path here that is different from the URL path used by your backend Service Fabric service, in which case you will also need to specify a path rewrite in your operation policy later.</span></span>
     - <span data-ttu-id="0500c-249">**Megjelenített név**: bármely nevezze el az API-t.</span><span class="sxs-lookup"><span data-stu-id="0500c-249">**Display name**: Provide any name for the API.</span></span> <span data-ttu-id="0500c-250">A jelen oktatóanyag esetében használja `Values`.</span><span class="sxs-lookup"><span data-stu-id="0500c-250">For this tutorial, use `Values`.</span></span>

## <a name="configure-a-backend-policy"></a><span data-ttu-id="0500c-251">Háttér-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0500c-251">Configure a backend policy</span></span>

<span data-ttu-id="0500c-252">A háttérrendszer házirend kötelékek mindent együtt.</span><span class="sxs-lookup"><span data-stu-id="0500c-252">The backend policy ties everything together.</span></span> <span data-ttu-id="0500c-253">Ez azért, ahol konfigurálhatja a Service Fabric háttérszolgáltatáshoz rendszer kérést átirányítja.</span><span class="sxs-lookup"><span data-stu-id="0500c-253">This is where you configure the backend Service Fabric service to which requests are routed.</span></span> <span data-ttu-id="0500c-254">Ezt a házirendet alkalmazhat API-művelet.</span><span class="sxs-lookup"><span data-stu-id="0500c-254">You can apply this policy to any API operation.</span></span> <span data-ttu-id="0500c-255">A [háttérkonfiguráció a Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) biztosít a következő kérés útválasztási vezérlők:</span><span class="sxs-lookup"><span data-stu-id="0500c-255">The [backend configuration for Service Fabric](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService) provides the following request routing controls:</span></span> 
 - <span data-ttu-id="0500c-256">A Service Fabric szolgáltatás példány neve, vagy szoftveresen kötött megadásával példány kiválasztása szolgáltatás (például `"fabric:/myapp/myservice"`) vagy a HTTP-kérelem generált (például `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span><span class="sxs-lookup"><span data-stu-id="0500c-256">Service instance selection by specifying a Service Fabric service instance name, either hardcoded (for example, `"fabric:/myapp/myservice"`) or generated from the HTTP request (for example, `"fabric:/myapp/users/" + context.Request.MatchedParameters["name"]`).</span></span>
 - <span data-ttu-id="0500c-257">A Service Fabric particionálási sémát használó partíciós kulcs létrehozása általi megoldása partíció.</span><span class="sxs-lookup"><span data-stu-id="0500c-257">Partition resolution by generating a partition key using any Service Fabric partitioning scheme.</span></span>
 - <span data-ttu-id="0500c-258">Állapotalapú szolgáltatások replika kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="0500c-258">Replica selection for stateful services.</span></span>
 - <span data-ttu-id="0500c-259">Megoldási újrapróbálkozási feltételeket, amelyek segítségével adhatja meg a feltételeket újra feloldani a helyét, és küldje el újra a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="0500c-259">Resolution retry conditions that allow you to specify the conditions for re-resolving a service location and resending a request.</span></span>

<span data-ttu-id="0500c-260">Ebben az oktatóanyagban a szabályzatban háttér közvetlenül kérelmeket továbbítja a korábban telepített ASP.NET Core állapotmentes szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="0500c-260">For this tutorial, create a backend policy that routes requests directly to the ASP.NET Core stateless service deployed earlier:</span></span>

 1. <span data-ttu-id="0500c-261">Válassza ki, és szerkesztheti a **házirendek bejövő** a a `Values` ehhez kattintson a Szerkesztés ikonra, és jelölje be a művelet **kódnézetben**.</span><span class="sxs-lookup"><span data-stu-id="0500c-261">Select and edit the **inbound policies** for the `Values` operation by clicking the edit icon, and then selecting **Code View**.</span></span>
 2. <span data-ttu-id="0500c-262">A kód házirendszerkesztő, adja hozzá a `set-backend-service` házirend a bejövő házirendek, ahogy az itt látható, és kattintson a **mentése** gombra:</span><span class="sxs-lookup"><span data-stu-id="0500c-262">In the policy code editor, add a `set-backend-service` policy under inbound policies as shown here and click the **Save** button:</span></span>
    
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

<span data-ttu-id="0500c-263">A Service Fabric háttér-házirend attribútumainak teljes készletét, tekintse meg a [API Management háttér-dokumentáció](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span><span class="sxs-lookup"><span data-stu-id="0500c-263">For a full set of Service Fabric back-end policy attributes, refer to the [API Management back-end documentation](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#SetBackendService)</span></span>

### <a name="add-the-api-to-a-product"></a><span data-ttu-id="0500c-264">A termék adja hozzá az API-t.</span><span class="sxs-lookup"><span data-stu-id="0500c-264">Add the API to a product.</span></span> 

<span data-ttu-id="0500c-265">Az API hívása előtt azt hozzá kell adni egy termékre, ahol hozzáférést biztosíthat a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="0500c-265">Before you can call the API, it must be added to a product where you can grant access to users.</span></span> 

 1. <span data-ttu-id="0500c-266">Válassza ki az API Management szolgáltatásban **termékek - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="0500c-266">In the API Management service, select **Products - PREVIEW**.</span></span>
 2. <span data-ttu-id="0500c-267">Alapértelmezés szerint az API Management szolgáltatók két termékek: alapszintű és a korlátlan.</span><span class="sxs-lookup"><span data-stu-id="0500c-267">By default, API Management providers two products: Starter and Unlimited.</span></span> <span data-ttu-id="0500c-268">Jelölje ki a korlátlan terméket.</span><span class="sxs-lookup"><span data-stu-id="0500c-268">Select the Unlimited product.</span></span>
 3. <span data-ttu-id="0500c-269">Válassza ki az API-k.</span><span class="sxs-lookup"><span data-stu-id="0500c-269">Select APIs.</span></span>
 4. <span data-ttu-id="0500c-270">Kattintson a **+ Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0500c-270">Click the **+ Add** button.</span></span>
 5. <span data-ttu-id="0500c-271">Válassza ki a `Service Fabric App` a fenti lépéseket, majd kattintson a létrehozott API a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="0500c-271">Select the `Service Fabric App` API you created in the previous steps and click the **Select** button.</span></span>

### <a name="test-it"></a><span data-ttu-id="0500c-272">Tesztelheti</span><span class="sxs-lookup"><span data-stu-id="0500c-272">Test it</span></span>

<span data-ttu-id="0500c-273">Most megpróbálhatja egy kérést küld a háttér-szolgáltatás a Service Fabric API Management keresztül közvetlenül az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="0500c-273">You can now try sending a request to your back-end service in Service Fabric through API Management directly from the Azure portal.</span></span>

 1. <span data-ttu-id="0500c-274">Válassza ki az API Management szolgáltatásban **API - előzetes**.</span><span class="sxs-lookup"><span data-stu-id="0500c-274">In the API Management service, select **API - PREVIEW**.</span></span>
 2. <span data-ttu-id="0500c-275">Az a `Service Fabric App` létrehozta az előző lépéseket, jelölje be az API a **teszt** fülre.</span><span class="sxs-lookup"><span data-stu-id="0500c-275">In the `Service Fabric App` API you created in the previous steps, select the **Test** tab.</span></span>
 3. <span data-ttu-id="0500c-276">Kattintson a **küldése** gombra kattintva a háttérszolgáltatáshoz tesztelési kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="0500c-276">Click the **Send** button to send a test request to the backend service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0500c-277">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0500c-277">Next steps</span></span>

<span data-ttu-id="0500c-278">Ezen a ponton rendelkeznie kell egy egyszerű telepítése a Service Fabric- és API-kezelés.</span><span class="sxs-lookup"><span data-stu-id="0500c-278">At this point, you should have a basic setup with Service Fabric and API Management.</span></span>

<span data-ttu-id="0500c-279">Ez az oktatóanyag segítségével gyorsan beállíthatja alapvető a tanúsítvány alapú hitelesítést a Service Fabric-fürt használja.</span><span class="sxs-lookup"><span data-stu-id="0500c-279">This tutorial uses basic certificate-based user authentication for your Service Fabric cluster to get you set up quickly.</span></span> <span data-ttu-id="0500c-280">A Service Fabric-fürt speciális felhasználóhitelesítést célszerű a [Azure Active Directory hitelesítési](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span><span class="sxs-lookup"><span data-stu-id="0500c-280">More advanced user authentication for your Service Fabric cluster is preferable with [Azure Active Directory authentication](service-fabric-cluster-creation-via-arm.md#set-up-azure-active-directory-for-client-authentication).</span></span> 

<span data-ttu-id="0500c-281">Ezt követően [létrehozása és speciális termékbeállítások konfigurálása az Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) valós forgalom az alkalmazás előkészítése.</span><span class="sxs-lookup"><span data-stu-id="0500c-281">Next, [create and configure advanced product settings in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-product-with-rules) to prepare your application for real world traffic.</span></span>

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
