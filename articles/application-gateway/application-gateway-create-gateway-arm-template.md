---
title: "Hozzon létre egy Azure Application Gateway - sablonok |} Microsoft Docs"
description: "Ez az oldal utasításokat tartalmaz egy Azure Application Gateway Azure Resource Manager-sablonnal történő létrehozásához"
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
ms.openlocfilehash: 46cca89ccb5bd77d57fabc3e9027fcebd38da8e7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a><span data-ttu-id="8ac61-103">Application Gateway létrehozása az Azure Resource Manager-sablonokkal</span><span class="sxs-lookup"><span data-stu-id="8ac61-103">Create an application gateway by using the Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8ac61-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8ac61-104">Azure portal</span></span>](application-gateway-create-gateway-portal.md)
> * [<span data-ttu-id="8ac61-105">Azure Resource Manager PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ac61-105">Azure Resource Manager PowerShell</span></span>](application-gateway-create-gateway-arm.md)
> * [<span data-ttu-id="8ac61-106">Klasszikus Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ac61-106">Azure Classic PowerShell</span></span>](application-gateway-create-gateway.md)
> * [<span data-ttu-id="8ac61-107">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="8ac61-107">Azure Resource Manager template</span></span>](application-gateway-create-gateway-arm-template.md)
> * [<span data-ttu-id="8ac61-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8ac61-108">Azure CLI</span></span>](application-gateway-create-gateway-cli.md)

<span data-ttu-id="8ac61-109">Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="8ac61-109">Azure Application Gateway is a layer-7 load balancer.</span></span> <span data-ttu-id="8ac61-110">Feladatátvételt és teljesítményalapú útválasztást biztosít a HTTP-kérelmek számára különböző kiszolgálók között, függetlenül attól, hogy a felhőben vagy a helyszínen találhatóak.</span><span class="sxs-lookup"><span data-stu-id="8ac61-110">It provides failover and performance-routing HTTP requests between different servers, whether they are on the cloud or on-premises.</span></span> <span data-ttu-id="8ac61-111">Az Application Gateway számos alkalmazáskézbesítési vezérlőszolgáltatást (ADC) biztosít, beleértve a HTTP-terheléselosztást, a cookie-alapú munkamenet-affinitást, a Secure Sockets Layer- (SSL-) alapú kiszervezést, az egyéni állapotmintákat, a többhelyes támogatást és még sok mást.</span><span class="sxs-lookup"><span data-stu-id="8ac61-111">Application Gateway provides many application delivery controller (ADC) features including HTTP load balancing, cookie-based session affinity, Secure Sockets Layer (SSL) offload, custom health probes, support for multi-site, and many others.</span></span> <span data-ttu-id="8ac61-112">Támogatott funkció teljes listájának megkereséséhez látogasson el a [Alkalmazásátjáró áttekintése](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="8ac61-112">To find a complete list of supported features, visit [Application Gateway overview](application-gateway-introduction.md)</span></span>

<span data-ttu-id="8ac61-113">Ez a cikk útmutatást nyújt a letöltés és módosítani egy meglévő Azure Resource Manager-sablont a Githubból, és a sablon a Githubon, PowerShell és az Azure parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="8ac61-113">This article walks you through downloading and modifying an existing Azure Resource Manager template from GitHub and deploying the template from GitHub, PowerShell, and the Azure CLI.</span></span>

<span data-ttu-id="8ac61-114">Ha közvetlenül a GitHubból helyezi üzembe az Azure Resource Manager-sablont változtatások nélkül, ugorjon a sablont a GitHubból telepítő lépésre.</span><span class="sxs-lookup"><span data-stu-id="8ac61-114">If you are simply deploying the Azure Resource Manager template directly from GitHub without any changes, skip to deploy a template from GitHub.</span></span>

## <a name="scenario"></a><span data-ttu-id="8ac61-115">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="8ac61-115">Scenario</span></span>

<span data-ttu-id="8ac61-116">Ebben a forgatókönyvben az alábbiakat fogja tenni:</span><span class="sxs-lookup"><span data-stu-id="8ac61-116">In this scenario you will:</span></span>

* <span data-ttu-id="8ac61-117">Hozzon létre egy alkalmazás webalkalmazási tűzfal.</span><span class="sxs-lookup"><span data-stu-id="8ac61-117">Create an application gateway with web application firewall.</span></span>
* <span data-ttu-id="8ac61-118">Létrehoz egy VirtualNetwork1 nevű virtuális hálózatot a 10.0.0.0/16 egy fenntartott CIDR-blokkjával.</span><span class="sxs-lookup"><span data-stu-id="8ac61-118">Create a virtual network named VirtualNetwork1 with a reserved CIDR block of 10.0.0.0/16.</span></span>
* <span data-ttu-id="8ac61-119">Létrehoz egy Appgatewaysubnet nevű alhálózatot, amelynek a CIDR-blokkja 10.0.0.0/28 lesz.</span><span class="sxs-lookup"><span data-stu-id="8ac61-119">Create a subnet called Appgatewaysubnet that uses 10.0.0.0/28 as its CIDR block.</span></span>
* <span data-ttu-id="8ac61-120">Beállít két korábban konfigurált háttér IP-címet a webkiszolgálóknak, amelyek között el szeretné osztani a forgalom terhelését.</span><span class="sxs-lookup"><span data-stu-id="8ac61-120">Set up two previously configured back-end IPs for the web servers you want to load balance the traffic.</span></span> <span data-ttu-id="8ac61-121">Ebben a példasablonban a háttér IP-cím a 10.0.1.10 és a 10.0.1.11.</span><span class="sxs-lookup"><span data-stu-id="8ac61-121">In this template example, the back-end IPs are 10.0.1.10 and 10.0.1.11.</span></span>

> [!NOTE]
> <span data-ttu-id="8ac61-122">Ezek a beállítások a sablon paraméterei.</span><span class="sxs-lookup"><span data-stu-id="8ac61-122">Those settings are the parameters for this template.</span></span> <span data-ttu-id="8ac61-123">A sablon testreszabása, módosíthatja a szabályokat, a figyelő, SSL és a azuredeploy.json fájl más beállítások.</span><span class="sxs-lookup"><span data-stu-id="8ac61-123">To customize the template, you can change rules, the listener, SSL, and other options in the azuredeploy.json file.</span></span>

![Forgatókönyv](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a><span data-ttu-id="8ac61-125">Az Azure Resource Manager-sablon letöltése és megismerése</span><span class="sxs-lookup"><span data-stu-id="8ac61-125">Download and understand the Azure Resource Manager template</span></span>

<span data-ttu-id="8ac61-126">A GitHubból letöltheti a meglévő Azure Resource Manager-sablont, amellyel létrehozhat egy virtuális hálózatot két alhálózattal, végrehajthatja a kívánt módosításokat, és újra felhasználhatja azt.</span><span class="sxs-lookup"><span data-stu-id="8ac61-126">You can download the existing Azure Resource Manager template to create a virtual network and two subnets from GitHub, make any changes you might want, and reuse it.</span></span> <span data-ttu-id="8ac61-127">Ehhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="8ac61-127">To do so, use the following steps:</span></span>

1. <span data-ttu-id="8ac61-128">Navigáljon a [Alkalmazásátjáró létrehozása webalkalmazási tűzfal engedélyezve van a](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="8ac61-128">Navigate to [Create Application Gateway with web application firewall enabled](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="8ac61-129">Kattintson az **azuredeploy.json**, majd a **RAW** elemre.</span><span class="sxs-lookup"><span data-stu-id="8ac61-129">Click **azuredeploy.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="8ac61-130">Mentse a fájlt egy helyi mappába a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="8ac61-130">Save the file to a local folder on your computer.</span></span>
1. <span data-ttu-id="8ac61-131">Ha már ismeri az Azure Resource Manager-sablonokat, akkor ugorjon a 7. lépéshez.</span><span class="sxs-lookup"><span data-stu-id="8ac61-131">If you are familiar with Azure Resource Manager templates, skip to step 7.</span></span>
1. <span data-ttu-id="8ac61-132">Nyissa meg a mentett fájlt, és tekintse meg a tartalom **paraméterek** sorban</span><span class="sxs-lookup"><span data-stu-id="8ac61-132">Open the file that you saved and look at the contents under **parameters** in line</span></span>
1. <span data-ttu-id="8ac61-133">Az Azure Resource Manager-sablonparaméterek az üzembe helyezés során kitölthető paraméterek helyőrzőiként működnek.</span><span class="sxs-lookup"><span data-stu-id="8ac61-133">Azure Resource Manager template parameters provide a placeholder for values that can be filled out during deployment.</span></span>

  | <span data-ttu-id="8ac61-134">Paraméter</span><span class="sxs-lookup"><span data-stu-id="8ac61-134">Parameter</span></span> | <span data-ttu-id="8ac61-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="8ac61-135">Description</span></span> |
  | --- | --- |
  | <span data-ttu-id="8ac61-136">**subnetPrefix**</span><span class="sxs-lookup"><span data-stu-id="8ac61-136">**subnetPrefix**</span></span> |<span data-ttu-id="8ac61-137">Az átjáró-alhálózat CIDR-blokkja.</span><span class="sxs-lookup"><span data-stu-id="8ac61-137">CIDR block for the application gateway subnet.</span></span> |
  | <span data-ttu-id="8ac61-138">**applicationGatewaySize**</span><span class="sxs-lookup"><span data-stu-id="8ac61-138">**applicationGatewaySize**</span></span> | <span data-ttu-id="8ac61-139">Az Alkalmazásátjáró mérete.</span><span class="sxs-lookup"><span data-stu-id="8ac61-139">Size of the application gateway.</span></span>  <span data-ttu-id="8ac61-140">WAF csak akkor engedélyezett, közepes és nagy méretű.</span><span class="sxs-lookup"><span data-stu-id="8ac61-140">WAF only allows medium and large.</span></span> |
  | <span data-ttu-id="8ac61-141">**backendIpaddress1**</span><span class="sxs-lookup"><span data-stu-id="8ac61-141">**backendIpaddress1**</span></span> |<span data-ttu-id="8ac61-142">Az első webalkalmazás-kiszolgáló IP-címe.</span><span class="sxs-lookup"><span data-stu-id="8ac61-142">IP address of the first web server.</span></span> |
  | <span data-ttu-id="8ac61-143">**backendIpaddress2**</span><span class="sxs-lookup"><span data-stu-id="8ac61-143">**backendIpaddress2**</span></span> |<span data-ttu-id="8ac61-144">A második webalkalmazás-kiszolgáló IP-címe.</span><span class="sxs-lookup"><span data-stu-id="8ac61-144">IP address of the second web server.</span></span> |
  | <span data-ttu-id="8ac61-145">**wafEnabled**</span><span class="sxs-lookup"><span data-stu-id="8ac61-145">**wafEnabled**</span></span> | <span data-ttu-id="8ac61-146">Határozza meg, ha engedélyezve van-e a WAF beállítást.</span><span class="sxs-lookup"><span data-stu-id="8ac61-146">Setting to determine if WAF is enabled.</span></span>|
  | <span data-ttu-id="8ac61-147">**wafMode**</span><span class="sxs-lookup"><span data-stu-id="8ac61-147">**wafMode**</span></span> | <span data-ttu-id="8ac61-148">A webalkalmazási tűzfal módját.</span><span class="sxs-lookup"><span data-stu-id="8ac61-148">Mode of the web application firewall.</span></span>  <span data-ttu-id="8ac61-149">Elérhető lehetőségek **megelőzési** vagy **észlelési**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-149">Available options are **prevention** or **detection**.</span></span>|
  | <span data-ttu-id="8ac61-150">**wafRuleSetType**</span><span class="sxs-lookup"><span data-stu-id="8ac61-150">**wafRuleSetType**</span></span> | <span data-ttu-id="8ac61-151">WAF szabálykészletben típusa.</span><span class="sxs-lookup"><span data-stu-id="8ac61-151">Ruleset type for WAF.</span></span>  <span data-ttu-id="8ac61-152">OWASP jelenleg az egyetlen támogatott beállítás.</span><span class="sxs-lookup"><span data-stu-id="8ac61-152">Currently OWASP is the only supported option.</span></span> |
  | <span data-ttu-id="8ac61-153">**wafRuleSetVersion**</span><span class="sxs-lookup"><span data-stu-id="8ac61-153">**wafRuleSetVersion**</span></span> |<span data-ttu-id="8ac61-154">Szabálykészletben verziója.</span><span class="sxs-lookup"><span data-stu-id="8ac61-154">Ruleset version.</span></span> <span data-ttu-id="8ac61-155">Program 2.2.9-es és 3.0 OWASP CRS opció jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="8ac61-155">OWASP CRS 2.2.9 and 3.0 are currently the supported options.</span></span> |

1. <span data-ttu-id="8ac61-156">Ellenőrizze a tartalmat a **erőforrások** , és figyelje meg a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="8ac61-156">Check the content under **resources** and notice the following properties:</span></span>

   * <span data-ttu-id="8ac61-157">**type**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-157">**type**.</span></span> <span data-ttu-id="8ac61-158">A sablon által létrehozott erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="8ac61-158">Type of resource being created by the template.</span></span> <span data-ttu-id="8ac61-159">Ebben az esetben a típus `Microsoft.Network/applicationGateways`, amely olyan átjárót jelöli.</span><span class="sxs-lookup"><span data-stu-id="8ac61-159">In this case, the type is `Microsoft.Network/applicationGateways`, which represents an application gateway.</span></span>
   * <span data-ttu-id="8ac61-160">**Név**</span><span class="sxs-lookup"><span data-stu-id="8ac61-160">**name**.</span></span> <span data-ttu-id="8ac61-161">Az erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="8ac61-161">Name for the resource.</span></span> <span data-ttu-id="8ac61-162">Figyelje meg a `[parameters('applicationGatewayName')]`, ami azt jelenti, a név biztosított bemenetként, vagy egy paraméterfájl üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="8ac61-162">Notice the use of `[parameters('applicationGatewayName')]`, which means that the name is provided as input by you or by a parameter file during deployment.</span></span>
   * <span data-ttu-id="8ac61-163">**properties**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-163">**properties**.</span></span> <span data-ttu-id="8ac61-164">Az erőforrás tulajdonságainak listája.</span><span class="sxs-lookup"><span data-stu-id="8ac61-164">List of properties for the resource.</span></span> <span data-ttu-id="8ac61-165">A sablon az Application Gateway létrehozása során a virtuális hálózatot és a nyilvános IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="8ac61-165">This template uses the virtual network and public IP address during application gateway creation.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8ac61-166">További információt a sablonok: [Resource Manager-sablonok referenciája](/templates/)</span><span class="sxs-lookup"><span data-stu-id="8ac61-166">For more information on templates visit: [Resource Manager templates reference](/templates/)</span></span>

1. <span data-ttu-id="8ac61-167">Lépjen vissza [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span><span class="sxs-lookup"><span data-stu-id="8ac61-167">Navigate back to [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-waf).</span></span>
1. <span data-ttu-id="8ac61-168">Kattintson a **azuredeploy-parameters.json**, és kattintson a **RAW**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-168">Click **azuredeploy-parameters.json**, and then click **RAW**.</span></span>
1. <span data-ttu-id="8ac61-169">Mentse a fájlt egy helyi mappába a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="8ac61-169">Save the file to a local folder on your computer.</span></span>
1. <span data-ttu-id="8ac61-170">Nyissa meg a mentett fájlt, és módosítsa a paraméterek értékeit.</span><span class="sxs-lookup"><span data-stu-id="8ac61-170">Open the file that you saved and edit the values for the parameters.</span></span> <span data-ttu-id="8ac61-171">A következő értékek használatával helyezze üzembe a forgatókönyvünkben ismertetett Application Gateway-t.</span><span class="sxs-lookup"><span data-stu-id="8ac61-171">Use the following values to deploy the application gateway described in our scenario.</span></span>

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

1. <span data-ttu-id="8ac61-172">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="8ac61-172">Save the file.</span></span> <span data-ttu-id="8ac61-173">A JSON-sablont és a paramétersablont online JSON érvényesítési eszközök, például a [JSlint.com](http://www.jslint.com/) segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8ac61-173">You can test the JSON template and parameter template by using online JSON validation tools like [JSlint.com](http://www.jslint.com/).</span></span>

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a><span data-ttu-id="8ac61-174">Az Azure Resource Manager-sablon üzembe helyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="8ac61-174">Deploy the Azure Resource Manager template by using PowerShell</span></span>

<span data-ttu-id="8ac61-175">Ha még sosem használta az Azure PowerShell, látogasson el: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az utasításokat követve jelentkezzen be Azure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="8ac61-175">If you have never used Azure PowerShell, visit: [How to install and configure Azure PowerShell](/powershell/azure/overview) and follow the instructions to sign into Azure and select your subscription.</span></span>

1. <span data-ttu-id="8ac61-176">PowerShell-bejelentkezési</span><span class="sxs-lookup"><span data-stu-id="8ac61-176">Login to PowerShell</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="8ac61-177">Keresse meg a fiókot az előfizetésekben.</span><span class="sxs-lookup"><span data-stu-id="8ac61-177">Check the subscriptions for the account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="8ac61-178">A rendszer kérni fogja a hitelesítő adatokkal történő hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="8ac61-178">You are prompted to authenticate with your credentials.</span></span>

1. <span data-ttu-id="8ac61-179">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="8ac61-179">Choose which of your Azure subscriptions to use.</span></span>

    ```powershell
    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
    ```

1. <span data-ttu-id="8ac61-180">Szükség esetén hozzon létre egy erőforráscsoportot a **New-AzureResourceGroup** parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="8ac61-180">If needed, create a resource group by using the **New-AzureResourceGroup** cmdlet.</span></span> <span data-ttu-id="8ac61-181">A következő példában hozzon létre egy erőforráscsoportot AppgatewayRG nevű USA keleti régiója helyen.</span><span class="sxs-lookup"><span data-stu-id="8ac61-181">In the following example, you create a resource group called AppgatewayRG in East US location.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name AppgatewayRG -Location "West US"
    ```

1. <span data-ttu-id="8ac61-182">Futtassa a **New-AzureRmResourceGroupDeployment** parancsmagot, hogy az előzőleg letöltött és módosított sablonnal és paraméterfájlokkal üzembe helyezhesse az új virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="8ac61-182">Run the **New-AzureRmResourceGroupDeployment** cmdlet to deploy the new virtual network by using the preceding template and parameter files you downloaded and modified.</span></span>
    
    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a><span data-ttu-id="8ac61-183">Az Azure Resource Manager-sablon üzembe helyezése az Azure CLI használatával</span><span class="sxs-lookup"><span data-stu-id="8ac61-183">Deploy the Azure Resource Manager template by using the Azure CLI</span></span>

<span data-ttu-id="8ac61-184">Az Azure Resource Manager sablon Azure CLI használatával történő üzembe helyezéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8ac61-184">To deploy the Azure Resource Manager template you downloaded by using Azure CLI, follow the following steps:</span></span>

1. <span data-ttu-id="8ac61-185">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Az Azure CLI telepítése és konfigurálása](/cli/azure/install-azure-cli) című szakaszt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="8ac61-185">If you have never used Azure CLI, see [Install and configure the Azure CLI](/cli/azure/install-azure-cli) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>

1. <span data-ttu-id="8ac61-186">Szükség esetén a `az group create` parancs futtatásával hozzon létre egy erőforráscsoportot, a következő kódrészletben látható módon.</span><span class="sxs-lookup"><span data-stu-id="8ac61-186">If necessary, run the `az group create` command to create a resource group, as shown in the following code snippet.</span></span> <span data-ttu-id="8ac61-187">Figyelje meg a parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="8ac61-187">Notice the output of the command.</span></span> <span data-ttu-id="8ac61-188">A kimenet után látható lista ismerteti a használt paramétereket.</span><span class="sxs-lookup"><span data-stu-id="8ac61-188">The list shown after the output explains the parameters used.</span></span> <span data-ttu-id="8ac61-189">További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8ac61-189">For more information about resource groups, visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```azurecli
    az group create --location westus --name appgatewayRG
    ```
    
    <span data-ttu-id="8ac61-190">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-190">**-n (or --name)**.</span></span> <span data-ttu-id="8ac61-191">Az új erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="8ac61-191">Name for the new resource group.</span></span> <span data-ttu-id="8ac61-192">A mi esetünkben *appgatewayRG*.</span><span class="sxs-lookup"><span data-stu-id="8ac61-192">For our scenario, it's *appgatewayRG*.</span></span>
    
    <span data-ttu-id="8ac61-193">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-193">**-l (or --location)**.</span></span> <span data-ttu-id="8ac61-194">Az Azure-régió, ahol az új erőforráscsoport létrejön.</span><span class="sxs-lookup"><span data-stu-id="8ac61-194">Azure region where the new resource group is created.</span></span> <span data-ttu-id="8ac61-195">A mi esetünkben rendelkezik *westus*.</span><span class="sxs-lookup"><span data-stu-id="8ac61-195">For our scenario, it's *westus*.</span></span>

1. <span data-ttu-id="8ac61-196">Futtassa a `az group deployment create` parancsmag használatával történő telepítéséről az új virtuális hálózat sablonnal és paraméterfájlokkal fájlok letöltött és módosított az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="8ac61-196">Run the `az group deployment create` cmdlet to deploy the new virtual network by using the template and parameter files you downloaded and modified in the preceding step.</span></span> <span data-ttu-id="8ac61-197">A kimenet után látható lista ismerteti a használt paramétereket.</span><span class="sxs-lookup"><span data-stu-id="8ac61-197">The list shown after the output explains the parameters used.</span></span>

    ```azurecli
    az group deployment create --resource-group appgatewayRG --name TestAppgatewayDeployment --template-file azuredeploy.json --parameters @azuredeploy-parameters.json
    ```

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a><span data-ttu-id="8ac61-198">Az Azure Resource Manager-sablon üzembe helyezése kattintással végrehajtható üzembe helyezéssel</span><span class="sxs-lookup"><span data-stu-id="8ac61-198">Deploy the Azure Resource Manager template by using click-to-deploy</span></span>

<span data-ttu-id="8ac61-199">Az Azure Resource Manager-sablonok használatának másik módja a kattintással végrehajtható üzembe helyezés.</span><span class="sxs-lookup"><span data-stu-id="8ac61-199">Click-to-deploy is another way to use Azure Resource Manager templates.</span></span> <span data-ttu-id="8ac61-200">Ez egy egyszerű mód a sablonok Azure portállal történő használatára.</span><span class="sxs-lookup"><span data-stu-id="8ac61-200">It's an easy way to use templates with the Azure portal.</span></span>

1. <span data-ttu-id="8ac61-201">Ugrás a [hozzon létre egy alkalmazás webalkalmazási tűzfal](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span><span class="sxs-lookup"><span data-stu-id="8ac61-201">Go to [Create an application gateway with web application firewall](https://azure.microsoft.com/documentation/templates/101-application-gateway-waf/).</span></span>

1. <span data-ttu-id="8ac61-202">Kattintson a **Deploy to Azure** (Üzembe helyezés az Azure-ban) elemre.</span><span class="sxs-lookup"><span data-stu-id="8ac61-202">Click **Deploy to Azure**.</span></span>

    ![Üzembe helyezés az Azure-ban](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)
    
1. <span data-ttu-id="8ac61-204">Töltse ki a központi telepítési sablon paramétereit a portálon, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ac61-204">Fill out the parameters for the deployment template on the portal and click **OK**.</span></span>

    ![Paraméterek](./media/application-gateway-create-gateway-arm-template/ibiza1.png)
    
1. <span data-ttu-id="8ac61-206">Válassza ki **elfogadom a feltételeket és a fenti feltételek** kattintson **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="8ac61-206">Select **I agree to the terms and conditions stated above** and click **Purchase**.</span></span>

1. <span data-ttu-id="8ac61-207">Az Egyéni üzembe helyezés panelen kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8ac61-207">On the Custom deployment blade, click **Create**.</span></span>

## <a name="providing-certificate-data-to-resource-manager-templates"></a><span data-ttu-id="8ac61-208">Tanúsítvány adatait a Resource Manager sablonokhoz szolgáltató</span><span class="sxs-lookup"><span data-stu-id="8ac61-208">Providing certificate data to Resource Manager templates</span></span>

<span data-ttu-id="8ac61-209">Ha SSL-t egy sablont használ, meg kell adni a feltöltendő helyett Base64 kódolású karakterláncnak kell a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="8ac61-209">When using SSL with a template, the certificate needs to be provided in a base64 string instead of being uploaded.</span></span> <span data-ttu-id="8ac61-210">Alakítsa át a .pfx vagy Base64 kódolású karakterlánc .cer használja a következő parancsok egyikét.</span><span class="sxs-lookup"><span data-stu-id="8ac61-210">To convert a .pfx or .cer to a base64 string use one of the following commands.</span></span> <span data-ttu-id="8ac61-211">Az alábbi parancsokat a tanúsítvány Base64 kódolású karakterlánc, amely a sablonhoz megadható alakítsa át.</span><span class="sxs-lookup"><span data-stu-id="8ac61-211">The following commands convert the certificate to a base64 string, which can be provided to the template.</span></span> <span data-ttu-id="8ac61-212">A várt kimeneti karakterlánc, amely egy változó tárolja, és a sablon a beillesztett.</span><span class="sxs-lookup"><span data-stu-id="8ac61-212">The expected output is a string that can be stored in a variable and pasted in the template.</span></span>

### <a name="macos"></a><span data-ttu-id="8ac61-213">macOS</span><span class="sxs-lookup"><span data-stu-id="8ac61-213">macOS</span></span>
```bash
cert=$( base64 <certificate path and name>.pfx )
echo $cert
```

### <a name="windows"></a><span data-ttu-id="8ac61-214">Windows</span><span class="sxs-lookup"><span data-stu-id="8ac61-214">Windows</span></span>
```powershell
[System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("<certificate path and name>.pfx"))
```

## <a name="delete-all-resources"></a><span data-ttu-id="8ac61-215">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="8ac61-215">Delete all resources</span></span>

<span data-ttu-id="8ac61-216">Ez a cikk létrehozott összes erőforrást törli, végezze el az alábbi lépések egyikét:</span><span class="sxs-lookup"><span data-stu-id="8ac61-216">To delete all resources created in this article, complete one of the following steps:</span></span>

### <a name="powershell"></a><span data-ttu-id="8ac61-217">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ac61-217">PowerShell</span></span>

```powershell
Remove-AzureRmResourceGroup -Name appgatewayRG
```

### <a name="azure-cli"></a><span data-ttu-id="8ac61-218">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8ac61-218">Azure CLI</span></span>

```azurecli
az group delete --name appgatewayRG
```

## <a name="next-steps"></a><span data-ttu-id="8ac61-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8ac61-219">Next steps</span></span>

<span data-ttu-id="8ac61-220">Ha SSL-alapú kiszervezést szeretne konfigurálni, tekintse meg a következőt: [Application Gateway konfigurálása SSL-alapú kiszervezéshez](application-gateway-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="8ac61-220">If you want to configure SSL offload, visit: [Configure an application gateway for SSL offload](application-gateway-ssl.md).</span></span>

<span data-ttu-id="8ac61-221">Ha konfigurálni szeretne egy ILB-vel használni kívánt Application Gateway-t: [Application Gateway létrehozása belső terheléselosztóval (ILB)](application-gateway-ilb.md).</span><span class="sxs-lookup"><span data-stu-id="8ac61-221">If you want to configure an application gateway to use with an internal load balancer, visit: [Create an application gateway with an internal load balancer (ILB)](application-gateway-ilb.md).</span></span>

<span data-ttu-id="8ac61-222">Ha további általános információra van szüksége a terheléselosztás beállításaival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="8ac61-222">If you want more information about load balancing options in general, visit:</span></span>

* [<span data-ttu-id="8ac61-223">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="8ac61-223">Azure Load Balancer</span></span>](https://azure.microsoft.com/documentation/services/load-balancer/)
* [<span data-ttu-id="8ac61-224">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="8ac61-224">Azure Traffic Manager</span></span>](https://azure.microsoft.com/documentation/services/traffic-manager/)

