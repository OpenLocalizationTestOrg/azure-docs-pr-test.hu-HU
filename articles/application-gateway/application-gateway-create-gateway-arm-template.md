---
title: egy Azure Application Gateway - sablonok aaaCreate |} Microsoft Docs
description: "Ezen a lapon nyújt útmutatást toocreate Azure Alkalmazásátjáró hello Azure Resource Manager-sablon használatával"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: fc18e553852551326d6a302abe2c7f8a08c2eb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-resource-manager-template"></a><span data-ttu-id="5bcb3-103">Alkalmazásátjáró létrehozása hello Azure Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="5bcb3-103">Create an application gateway by using hello Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5bcb3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5bcb3-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="5bcb3-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="5bcb3-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="5bcb3-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5bcb3-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="5bcb3-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="5bcb3-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="5bcb3-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5bcb3-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="5bcb3-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="5bcb3-110">Azt is biztosít feladatátvételi és HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között hello felhőalapú vagy helyszíni legyenek.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on hello cloud or on-premises.</span></span> <span data-ttu-id="5bcb3-111">Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer- (SSL-) alapú kiszervezést, az egyéni állapotmintákat, a többhelyes támogatást és még sok mást.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="5bcb3-112">látogasson el a támogatott funkciók teljes listáját toofind [Alkalmazásátjáró áttekintése](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="5bcb3-112">toofind a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="5bcb3-113">Ez a cikk útmutatást nyújt a letöltés és módosítani egy meglévő Azure Resource Manager-sablont a Githubból, és hello sablont a Githubból, PowerShell és hello Azure CLI telepítése.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-113">This article walks you through downloading and modifying an existing Azure Resource Manager template from GitHub and deploying hello template from GitHub, PowerShell, and hello Azure CLI.</span></span>

<span data-ttu-id="5bcb3-114">Egyszerűen telepítése Azure Resource Manager-sablon hello közvetlenül a Githubból módosítások nélkül, ugorjon a toodeploy egy sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-114">If you are simply deploying hello Azure Resource Manager template directly from GitHub without any changes, skip toodeploy a template from GitHub.</span></span>

## <a name="scenario"></a><span data-ttu-id="5bcb3-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="5bcb3-115">Scenario</span></span>

<span data-ttu-id="5bcb3-116">Ebben a forgatókönyvben az alábbiakat fogja tenni:</span><span class="sxs-lookup"><span data-stu-id="5bcb3-116">In this scenario you will:</span></span>

* <span data-ttu-id="5bcb3-117">Hozzon létre egy alkalmazás webalkalmazási tűzfal.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-117">Create an application gateway with web application firewall.</span></span>
* <span data-ttu-id="5bcb3-118">Létrehoz egy VirtualNetwork1 nevű virtuális hálózatot a 10.0.0.0/16 egy fenntartott CIDR-blokkjával.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-118">Create a virtual network named VirtualNetwork1 with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="5bcb3-119">Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-119">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>
* <span data-ttu-id="5bcb3-120">Két korábban konfigurált háttér IP-címek hello webkiszolgálókhoz beállítani kívánt tooload egyenleg hello forgalom.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-120">Set up two previously configured back-end IPs for hello web servers you want tooload balance hello traffic.</span></span> <span data-ttu-id="5bcb3-121">A sablon példában hello háttér IP-címek 10.0.1.10 és 10.0.1.11.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-121">In this template example, hello back-end IPs are 10.0.1.10 and 10.0.1.11.</span></span>

> [!NOTE]
> <span data-ttu-id="5bcb3-122">Ezek a beállítások olyan hello paraméterek ehhez a sablonhoz.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-122">Those settings are hello parameters for this template.</span></span> <span data-ttu-id="5bcb3-123">toocustomize hello sablon, módosíthatja szabályok, hello figyelő, SSL és egyéb beállítások hello azuredeploy.json fájl.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-123">toocustomize hello template, you can change rules, hello listener, SSL, and other options in hello azuredeploy.json file.</span></span>

![Forgatókönyv](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-hello-azure-resource-manager-template"></a><span data-ttu-id="5bcb3-125">Töltse le és hello Azure Resource Manager sablon ismertetése</span><span class="sxs-lookup"><span data-stu-id="5bcb3-125">Download and understand hello Azure Resource Manager template</span></span>

<span data-ttu-id="5bcb3-126">Töltse le a hello meglévő Azure Resource Manager sablon toocreate virtuális hálózat és két alhálózat a Githubból, hajtsa végre a módosításokat, előfordulhat, hogy szeretné, és újra felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-126">You can download hello existing Azure Resource Manager template toocreate a virtual network and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="5bcb3-127">toodo Igen, használja a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5bcb3-127">toodo so, use hello following steps:</span></span>

1. <span data-ttu-id="5bcb3-128">Keresse meg a túl[Alkalmazásátjáró létrehozása webalkalmazási tűzfal engedélyezve van a](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-128">Navigate too[Create Application Gateway with web application firewall enabled](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="5bcb3-129">Kattintson az **azuredeploy.json**, majd a **RAW** elemre.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-129">Click **azuredeploy.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="5bcb3-130">Mentés hello fájl tooa helyi mappába a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-130">Save hello file tooa local folder on your computer.</span></span>
1. <span data-ttu-id="5bcb3-131">Ha ismeri az Azure Resource Manager-sablonok, ugorjon a toostep 7.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-131">If you are familiar with Azure Resource Manager templates, skip toostep 7.</span></span>
1. <span data-ttu-id="5bcb3-132">Nyissa meg a mentett hello fájlt, és nézze meg alatt látható tartalmakat hello **paraméterek** sorban</span><span class="sxs-lookup"><span data-stu-id="5bcb3-132">Open hello file that you saved and look at hello contents under **parameters** in line</span></span>
1. <span data-ttu-id="5bcb3-133">Az Azure Resource Manager-sablonparaméterek az üzembe helyezés során kitölthető paraméterek helyőrzőiként működnek.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-133">Azure Resource Manager template parameters provide a placeholder for values that can be filled out during deployment.</span></span>

  | <span data-ttu-id="5bcb3-134">Paraméter</span><span class="sxs-lookup"><span data-stu-id="5bcb3-134">Parameter</span></span> | <span data-ttu-id="5bcb3-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="5bcb3-135">Description</span></span> |
  | --- | --- |
  | <span data-ttu-id="5bcb3-136">**subnetPrefix**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-136">**subnetPrefix**</span></span> |<span data-ttu-id="5bcb3-137">Hello alkalmazás átjáró-alhálózat CIDR-blokkja.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-137">CIDR block for hello application gateway subnet.</span></span> |
  | <span data-ttu-id="5bcb3-138">**applicationGatewaySize**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-138">**applicationGatewaySize**</span></span> | <span data-ttu-id="5bcb3-139">Hello Alkalmazásátjáró mérete.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-139">Size of hello application gateway.</span></span>  <span data-ttu-id="5bcb3-140">WAF csak akkor engedélyezett, közepes és nagy méretű.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-140">WAF only allows medium and large.</span></span> |
  | <span data-ttu-id="5bcb3-141">**backendIpaddress1**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-141">**backendIpaddress1**</span></span> |<span data-ttu-id="5bcb3-142">IP-címe hello első webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-142">IP address of hello first web server.</span></span> |
  | <span data-ttu-id="5bcb3-143">**backendIpaddress2**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-143">**backendIpaddress2**</span></span> |<span data-ttu-id="5bcb3-144">Hello második webalkalmazás-kiszolgáló IP-címe.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-144">IP address of hello second web server.</span></span> |
  | <span data-ttu-id="5bcb3-145">**wafEnabled**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-145">**wafEnabled**</span></span> | <span data-ttu-id="5bcb3-146">Toodetermine a beállítást, ha WAF engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-146">Setting toodetermine if WAF is enabled.</span></span>|
  | <span data-ttu-id="5bcb3-147">**wafMode**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-147">**wafMode**</span></span> | <span data-ttu-id="5bcb3-148">Webalkalmazási tűzfal hello módját.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-148">Mode of hello web application firewall.</span></span>  <span data-ttu-id="5bcb3-149">Elérhető lehetőségek **megelőzési** vagy **észlelési**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-149">Available options are **prevention** or **detection**.</span></span>|
  | <span data-ttu-id="5bcb3-150">**wafRuleSetType**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-150">**wafRuleSetType**</span></span> | <span data-ttu-id="5bcb3-151">WAF szabálykészletben típusa.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-151">Ruleset type for WAF.</span></span>  <span data-ttu-id="5bcb3-152">Jelenleg OWASP hello csak akkor támogatja a beállítást.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-152">Currently OWASP is hello only supported option.</span></span> |
  | <span data-ttu-id="5bcb3-153">**wafRuleSetVersion**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-153">**wafRuleSetVersion**</span></span> |<span data-ttu-id="5bcb3-154">Szabálykészletben verziója.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-154">Ruleset version.</span></span> <span data-ttu-id="5bcb3-155">Program 2.2.9-es és 3.0 OWASP CRS opció jelenleg hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-155">OWASP CRS 2.2.9 and 3.0 are currently hello supported options.</span></span> |

1. <span data-ttu-id="5bcb3-156">Hello tartalom alapján ellenőrizze **erőforrások** és a hirdetmény hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="5bcb3-156">Check hello content under **resources** and notice hello following properties:</span></span>

   * <span data-ttu-id="5bcb3-157">**type**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-157">**type**.</span></span> <span data-ttu-id="5bcb3-158">Hello sablon által létrehozott erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-158">Type of resource being created by hello template.</span></span> <span data-ttu-id="5bcb3-159">Ebben az esetben a hello típus: `Microsoft.Network/applicationGateways`, amely olyan átjárót jelöli.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-159">In this case, hello type is `Microsoft.Network/applicationGateways`, which represents an application gateway.</span></span>
   * <span data-ttu-id="5bcb3-160">**Név**</span><span class="sxs-lookup"><span data-stu-id="5bcb3-160">**name**.</span></span> <span data-ttu-id="5bcb3-161">Hello erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-161">Name for hello resource.</span></span> <span data-ttu-id="5bcb3-162">Értesítés hello használata `[parameters('applicationGatewayName')]`, ami azt jelenti, hogy hello neve valósul meg bemeneti adatként, vagy egy paraméterfájl üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-162">Notice hello use of `[parameters('applicationGatewayName')]`, which means that hello name is provided as input by you or by a parameter file during deployment.</span></span>
   * <span data-ttu-id="5bcb3-163">**properties**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-163">**properties**.</span></span> <span data-ttu-id="5bcb3-164">Hello erőforrás tulajdonságainak listája.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-164">List of properties for hello resource.</span></span> <span data-ttu-id="5bcb3-165">Ez a sablon által hello virtuális hálózat és a nyilvános IP-cím application gateway létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-165">This template uses hello virtual network and public IP address during application gateway creation.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5bcb3-166">További információt a sablonok: [Resource Manager-sablonok referenciája](/templates/)</span><span class="sxs-lookup"><span data-stu-id="5bcb3-166">For more information on templates visit: [Resource Manager templates reference](/templates/)</span></span>

1. <span data-ttu-id="5bcb3-167">Lépjen vissza túl[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-167">Navigate back too[https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="5bcb3-168">Kattintson a **azuredeploy-parameters.json**, és kattintson a **RAW**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-168">Click **azuredeploy-parameters.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="5bcb3-169">Mentés hello fájl tooa helyi mappába a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-169">Save hello file tooa local folder on your computer.</span></span>
1. <span data-ttu-id="5bcb3-170">Nyissa meg a mentett hello fájlt, és módosítsa a hello hello paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-170">Open hello file that you saved and edit hello values for hello parameters.</span></span> <span data-ttu-id="5bcb3-171">A következő értékek toodeploy hello Alkalmazásátjáró leírt hello használata.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-171">Use hello following values toodeploy hello application gateway described in our scenario.</span></span>

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "addressPrefix": {
            "value": "10.0.0.0/16"
            },
            "subnetPrefix": {
            "value": "10.0.0.0/28"
            },
            "applicationGatewaySize": {
            "value": "WAF_Medium"
            },
            "capacity": {
            "value": 2
            },
            "backendIpAddress1": {
            "value": "10.0.1.10"
            },
            "backendIpAddress2": {
            "value": "10.0.1.11"
            },
            "wafEnabled": {
            "value": true
            },
            "wafMode": {
            "value": "Detection"
            },
            "wafRuleSetType": {
            "value": "OWASP"
            },
            "wafRuleSetVersion": {
            "value": "3.0"
            }
        }
    }
    ```

1. <span data-ttu-id="5bcb3-172">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-172">Save hello file.</span></span> <span data-ttu-id="5bcb3-173">Tesztelheti hello JSON-sablon és a paraméter sablon JSON érvényesítési online eszközökkel, például a [JSlint.com](http://www.jslint.com/).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-173">You can test hello JSON template and parameter template by using online JSON validation tools like [JSlint.com](http://www.jslint.com/).</span></span>

## <a name="deploy-hello-azure-resource-manager-template-by-using-powershell"></a><span data-ttu-id="5bcb3-174">Hello Azure Resource Manager-sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="5bcb3-174">Deploy hello Azure Resource Manager template by using PowerShell</span></span>

<span data-ttu-id="5bcb3-175">Ha még sosem használta az Azure PowerShell, látogasson el: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) és kövesse az utasításokat toosign hello az Azure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-175">If you have never used Azure PowerShell, visit: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions toosign into Azure and select your subscription.</span></span>

1. <span data-ttu-id="5bcb3-176">Bejelentkezési tooPowerShell</span><span class="sxs-lookup"><span data-stu-id="5bcb3-176">Login tooPowerShell</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="5bcb3-177">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-177">Check hello subscriptions for hello account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="5bcb3-178">A hitelesítő adataival felszólító tooauthenticate áll.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-178">You are prompted tooauthenticate with your credentials.</span></span>

1. <span data-ttu-id="5bcb3-179">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-179">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. <span data-ttu-id="5bcb3-180">Szükség esetén hozzon létre egy erőforráscsoportot hello segítségével **New-használják** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-180">If needed, create a resource group by using hello **New-AzureResourceGroup** cmdlet.</span></span> <span data-ttu-id="5bcb3-181">A következő példa hello hozzon létre egy erőforráscsoportot AppgatewayRG nevű USA keleti régiója helyen.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-181">In hello following example, you create a resource group called AppgatewayRG in East US location.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. <span data-ttu-id="5bcb3-182">Futtassa a hello **New-AzureRmResourceGroupDeployment** parancsmag toodeploy hello új virtuális hálózat használatával a letöltött és módosított sablonnal és paraméterfájlokkal fájlok megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-182">Run hello **New-AzureRmResourceGroupDeployment** cmdlet toodeploy hello new virtual network by using hello preceding template and parameter files you downloaded and modified.</span></span>
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-hello-azure-cli"></a><span data-ttu-id="5bcb3-183">Hello Azure Resource Manager sablon üzembe helyezése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5bcb3-183">Deploy hello Azure Resource Manager template by using hello Azure CLI</span></span>

<span data-ttu-id="5bcb3-184">toodeploy hello Azure Resource Manager sablon Azure CLI használatával kövesse a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="5bcb3-184">toodeploy hello Azure Resource Manager template you downloaded by using Azure CLI, follow hello following steps:</span></span>

1. <span data-ttu-id="5bcb3-185">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](/cli/azure/install-azure-cli) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-185">If you have never used Azure CLI, see [Install and configure hello Azure CLI](/cli/azure/install-azure-cli) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>

1. <span data-ttu-id="5bcb3-186">Ha szükséges, futtatási hello `az group create` parancs toocreate egy erőforráscsoport, ahogy az a következő kódrészletet hello.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-186">If necessary, run hello `az group create` command toocreate a resource group, as shown in hello following code snippet.</span></span> <span data-ttu-id="5bcb3-187">Figyelje meg hello hello parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-187">Notice hello output of hello command.</span></span> <span data-ttu-id="5bcb3-188">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-188">hello list shown after hello output explains hello parameters used.</span></span> <span data-ttu-id="5bcb3-189">További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-189">For more information about resource groups, visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    <span data-ttu-id="5bcb3-190">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-190">**-n (or --name)**.</span></span> <span data-ttu-id="5bcb3-191">Hello új erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-191">Name for hello new resource group.</span></span> <span data-ttu-id="5bcb3-192">A mi esetünkben *appgatewayRG*.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-192">For our scenario, it's *appgatewayRG*.</span></span>
    
    <span data-ttu-id="5bcb3-193">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-193">**-l (or --location)**.</span></span> <span data-ttu-id="5bcb3-194">Azure-régió, ahol hello új erőforráscsoport létrejön.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-194">Azure region where hello new resource group is created.</span></span> <span data-ttu-id="5bcb3-195">A mi esetünkben rendelkezik *westus*.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-195">For our scenario, it's *westus*.</span></span>

1. <span data-ttu-id="5bcb3-196">Futtassa a hello `az group deployment create` parancsmag toodeploy hello új virtuális hálózat hello sablonnal és paraméterfájlokkal letöltött és módosított lépést megelőző hello a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-196">Run hello `az group deployment create` cmdlet toodeploy hello new virtual network by using hello template and parameter files you downloaded and modified in hello preceding step.</span></span> <span data-ttu-id="5bcb3-197">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-197">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-hello-azure-resource-manager-template-by-using-click-to-deploy"></a><span data-ttu-id="5bcb3-198">Hello Azure Resource Manager sablon telepíteni a kattintson a központi telepítése</span><span class="sxs-lookup"><span data-stu-id="5bcb3-198">Deploy hello Azure Resource Manager template by using click-to-deploy</span></span>

<span data-ttu-id="5bcb3-199">Egy másik módja toouse kattintson a központi telepítése az Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-199">Click-to-deploy is another way toouse Azure Resource Manager templates.</span></span> <span data-ttu-id="5bcb3-200">Egy egyszerűen toouse sablonokat hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-200">It's an easy way toouse templates with hello Azure portal.</span></span>

1. <span data-ttu-id="5bcb3-201">Nyissa meg túl[hozzon létre egy alkalmazás webalkalmazási tűzfal](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-201">Go too[Create an application gateway with web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span></span>

1. <span data-ttu-id="5bcb3-202">Kattintson a **tooAzure telepítése**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-202">Click **Deploy tooAzure**.</span></span>

    ![TooAzure telepítése](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. <span data-ttu-id="5bcb3-204">Töltse ki a hello paraméterek hello központi telepítési sablon hello portálon, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-204">Fill out hello parameters for hello deployment template on hello portal and click **OK**.</span></span>

    ![Paraméterek](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. <span data-ttu-id="5bcb3-206">Válassza ki **toohello feltételek és kikötések fenti elfogadom** kattintson **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-206">Select **I agree toohello terms and conditions stated above** and click **Purchase**.</span></span>

1. <span data-ttu-id="5bcb3-207">Hello egyéni telepítési paneljén kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-207">On hello Custom deployment blade, click **Create**.</span></span>

## <a name="providing-certificate-data-tooresource-manager-templates"></a><span data-ttu-id="5bcb3-208">Biztosító tanúsítvány adatok tooResource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="5bcb3-208">Providing certificate data tooResource Manager templates</span></span>

<span data-ttu-id="5bcb3-209">Ha SSL-t egy sablont használ, a hello tanúsítványt kell toobe megadott helyett feltöltendő Base64 kódolású karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-209">When using SSL with a template, hello certificate needs toobe provided in a base64 string instead of being uploaded.</span></span> <span data-ttu-id="5bcb3-210">tooconvert egy .pfx vagy .cer tooa Base64 kódolású karakterlánc hello a következő parancsok egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-210">tooconvert a .pfx or .cer tooa base64 string use one of hello following commands.</span></span> <span data-ttu-id="5bcb3-211">hello következő parancsokat az átalakítás hello tanúsítvány tooa base64 karakterláncot, amely toohello sablon megadható.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-211">hello following commands convert hello certificate tooa base64 string, which can be provided toohello template.</span></span> <span data-ttu-id="5bcb3-212">hello várt kimeneti karakterlánc, amely egy változó tárolja, és a beillesztett hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="5bcb3-212">hello expected output is a string that can be stored in a variable and pasted in hello template.</span></span>

### <a name="macos"></a><span data-ttu-id="5bcb3-213">macOS</span><span class="sxs-lookup"><span data-stu-id="5bcb3-213">macOS</span></span>
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a><span data-ttu-id="5bcb3-214">Windows</span><span class="sxs-lookup"><span data-stu-id="5bcb3-214">Windows</span></span>
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a><span data-ttu-id="5bcb3-215">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="5bcb3-215">Delete all resources</span></span>

<span data-ttu-id="5bcb3-216">toodelete összes erőforrás létrehozása ebben a cikkben a lépéseket követve hello teljes egyikét:</span><span class="sxs-lookup"><span data-stu-id="5bcb3-216">toodelete all resources created in this article, complete one of hello following steps:</span></span>

### <a name="powershell"></a><span data-ttu-id="5bcb3-217">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5bcb3-217">PowerShell</span></span>

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a><span data-ttu-id="5bcb3-218">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5bcb3-218">Azure CLI</span></span>

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a><span data-ttu-id="5bcb3-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bcb3-219">Next steps</span></span>

<span data-ttu-id="5bcb3-220">Ha azt szeretné, hogy az SSL-kiszervezés tooconfigure, látogasson el: [konfigurálása az SSL-kiszervezés Alkalmazásátjáró](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-220">If you want tooconfigure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="5bcb3-221">Ha azt szeretné, hogy egy alkalmazás átjáró toouse belső terheléselosztással tooconfigure, látogasson el: [hozzon létre egy alkalmazást egy belső terheléselosztón (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="5bcb3-221">If you want tooconfigure an application gateway toouse with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="5bcb3-222">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="5bcb3-222">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="5bcb3-223">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="5bcb3-223">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="5bcb3-224">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="5bcb3-224">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

