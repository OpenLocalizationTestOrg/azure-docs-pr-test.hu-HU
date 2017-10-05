---
title: "Az Azure Resource Manager üzembe helyezési műveleteinek |} Microsoft Docs"
description: "Útmutató megtekintéséhez az Azure Resource Manager üzembe helyezési műveleteket a portál, a PowerShell, az Azure CLI és a REST API-t."
services: azure-resource-manager,virtual-machines
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 01/13/2017
ms.author: tomfitz
ms.openlocfilehash: fb6b3b357fd1f66184e480115a9c863ba31ac193
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a><span data-ttu-id="e438d-103">Az Azure Resource Manager központi telepítési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="e438d-103">View deployment operations with Azure Resource Manager</span></span>


<span data-ttu-id="e438d-104">A műveletek a központi telepítés az Azure portálon keresztül tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="e438d-104">You can view the operations for a deployment through the Azure portal.</span></span> <span data-ttu-id="e438d-105">Előfordulhat, hogy tervezheti meg a műveletek megtekintése hiba beérkezése után üzembe helyezése során, ez a cikk foglalkozik, amelyek nem tudták műveletek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="e438d-105">You may be most interested in viewing the operations when you have received an error during deployment so this article focuses on viewing operations that have failed.</span></span> <span data-ttu-id="e438d-106">A portál, amely lehetővé teszi, hogy könnyedén megtalálhatja a hibák, és határozza meg a lehetséges javítások felületet biztosít.</span><span class="sxs-lookup"><span data-stu-id="e438d-106">The portal provides an interface that enables you to easily find the errors and determine potential fixes.</span></span>

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a><span data-ttu-id="e438d-107">Portál</span><span class="sxs-lookup"><span data-stu-id="e438d-107">Portal</span></span>
<span data-ttu-id="e438d-108">Üzembe helyezési műveleteinek megtekintéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e438d-108">To see the deployment operations, use the following steps:</span></span>

1. <span data-ttu-id="e438d-109">Az erőforráscsoport részt vesz a telepítés figyelje meg a legutóbbi központi telepítés állapotát.</span><span class="sxs-lookup"><span data-stu-id="e438d-109">For the resource group involved in the deployment, notice the status of the last deployment.</span></span> <span data-ttu-id="e438d-110">Ez az állapot részleteinek kiválasztása</span><span class="sxs-lookup"><span data-stu-id="e438d-110">You can select this status to get more details.</span></span>
   
    ![Központi telepítési állapota](./media/resource-manager-deployment-operations/deployment-status.png)
2. <span data-ttu-id="e438d-112">Megjelenik a legutóbbi központi telepítés előzményei.</span><span class="sxs-lookup"><span data-stu-id="e438d-112">You see the recent deployment history.</span></span> <span data-ttu-id="e438d-113">Válassza ki a telepítést, melyeknél nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="e438d-113">Select the deployment that failed.</span></span>
   
    ![Központi telepítési állapota](./media/resource-manager-deployment-operations/select-deployment.png)
3. <span data-ttu-id="e438d-115">Válassza ki a hivatkozásra kattintva megtekintheti a központi telepítés sikertelenségének leírását.</span><span class="sxs-lookup"><span data-stu-id="e438d-115">Select the link to see a description of why the deployment failed.</span></span> <span data-ttu-id="e438d-116">Az alábbi ábrán a DNS-rekord nem egyedi.</span><span class="sxs-lookup"><span data-stu-id="e438d-116">In the image below, the DNS record is not unique.</span></span>  
   
    ![sikertelen központi telepítés megtekintése](./media/resource-manager-deployment-operations/view-error.png)
   
    <span data-ttu-id="e438d-118">Ez a hibaüzenet jelenik meg a hibaelhárítás elegendő kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e438d-118">This error message should be enough for you to begin troubleshooting.</span></span> <span data-ttu-id="e438d-119">Azonban ha kapcsolatos további részleteket kell mely feladatok befejeződtek, megtekintheti a műveleteket, ahogy az az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e438d-119">However, if you need more details about which tasks were completed, you can view the operations as shown in the following steps.</span></span>
4. <span data-ttu-id="e438d-120">Megtekintheti az összes üzembe helyezési műveleteinek a **telepítési** panelen.</span><span class="sxs-lookup"><span data-stu-id="e438d-120">You can view all the deployment operations in the **Deployment** blade.</span></span> <span data-ttu-id="e438d-121">Válassza ki a bármilyen műveletet további részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e438d-121">Select any operation to see more details.</span></span>
   
    ![műveletek megtekintése](./media/resource-manager-deployment-operations/view-operations.png)
   
    <span data-ttu-id="e438d-123">Ebben az esetben láthatja, hogy a tárfiókot, a virtuális hálózat és a rendelkezésre állási csoport sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="e438d-123">In this case, you see that the storage account, virtual network, and availability set were successfully created.</span></span> <span data-ttu-id="e438d-124">A nyilvános IP-címet nem sikerült, és más erőforrások nem megkísérelte.</span><span class="sxs-lookup"><span data-stu-id="e438d-124">The public IP address failed, and other resources were not attempted.</span></span>
5. <span data-ttu-id="e438d-125">Kiválasztásával megtekintheti a központi telepítés események **események**.</span><span class="sxs-lookup"><span data-stu-id="e438d-125">You can view events for the deployment by selecting **Events**.</span></span>
   
    ![események megtekintése](./media/resource-manager-deployment-operations/view-events.png)
6. <span data-ttu-id="e438d-127">A központi telepítés az eseményeket, és válassza ki a közül legalább egy további részleteket.</span><span class="sxs-lookup"><span data-stu-id="e438d-127">You see all the events for the deployment and select any one for more details.</span></span> <span data-ttu-id="e438d-128">Figyelje meg, túl a korrelációs azonosítót.</span><span class="sxs-lookup"><span data-stu-id="e438d-128">Notice too the correlation IDs.</span></span> <span data-ttu-id="e438d-129">Ez az érték akkor lehet hasznos, amikor olyan központi telepítés hibaelhárítása a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="e438d-129">This value can be helpful when working with technical support to troubleshoot a deployment.</span></span>
   
    ![események](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a><span data-ttu-id="e438d-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e438d-131">PowerShell</span></span>
1. <span data-ttu-id="e438d-132">Ahhoz, hogy a központi telepítés összesített állapotát, használja a **Get-AzureRmResourceGroupDeployment** parancsot.</span><span class="sxs-lookup"><span data-stu-id="e438d-132">To get the overall status of a deployment, use the **Get-AzureRmResourceGroupDeployment** command.</span></span> 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   <span data-ttu-id="e438d-133">Vagy csak a sikertelen telepítések az eredményeket szűrheti is.</span><span class="sxs-lookup"><span data-stu-id="e438d-133">Or, you can filter the results for only those deployments that have failed.</span></span>

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. <span data-ttu-id="e438d-134">Minden központi telepítési több műveletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e438d-134">Each deployment includes multiple operations.</span></span> <span data-ttu-id="e438d-135">Minden műveletet a telepítési folyamat azon lépése jelöli.</span><span class="sxs-lookup"><span data-stu-id="e438d-135">Each operation represents a step in the deployment process.</span></span> <span data-ttu-id="e438d-136">Annak megállapításához, hogy mi a probléma telepítés, általában szüksége üzembe helyezési műveleteinek kapcsolatos részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e438d-136">To discover what went wrong with a deployment, you usually need to see details about the deployment operations.</span></span> <span data-ttu-id="e438d-137">Láthatja, hogy a műveletek állapotának **Get-AzureRmResourceGroupDeploymentOperation**.</span><span class="sxs-lookup"><span data-stu-id="e438d-137">You can see the status of the operations with **Get-AzureRmResourceGroupDeploymentOperation**.</span></span>

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    <span data-ttu-id="e438d-138">Több művelet található a következő formátumban mindegyiknél visszaadó:</span><span class="sxs-lookup"><span data-stu-id="e438d-138">Which returns multiple operations with each one in the following format:</span></span>

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. <span data-ttu-id="e438d-139">Ahhoz, hogy további információt a sikertelen műveleteket, a műveletek tulajdonságainak lekérése **sikertelen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="e438d-139">To get more details about failed operations, retrieve the properties for operations with **Failed** state.</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    <span data-ttu-id="e438d-140">Amely adja vissza a sikertelen műveleteket a mindegyiknél a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="e438d-140">Which returns all the failed operations with each one in the following format:</span></span>

  ```powershell
  provisioningOperation : Create
  provisioningState     : Failed
  timestamp             : 2016-06-14T21:54:55.1468068Z
  duration              : PT3.1449887S
  trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
  serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
  statusCode            : BadRequest
  statusMessage         : @{error=}
  targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                          Microsoft.Network/publicIPAddresses/myPublicIP;
                          resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}
  ```

    <span data-ttu-id="e438d-141">Vegye figyelembe a serviceRequestId és a trackingId a művelethez.</span><span class="sxs-lookup"><span data-stu-id="e438d-141">Note the serviceRequestId and the trackingId for the operation.</span></span> <span data-ttu-id="e438d-142">A serviceRequestId akkor lehet hasznos, amikor olyan központi telepítés hibaelhárítása a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="e438d-142">The serviceRequestId can be helpful when working with technical support to troubleshoot a deployment.</span></span> <span data-ttu-id="e438d-143">Szüksége lesz a trackingId a következő lépésben egy adott művelethez összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="e438d-143">You will use the trackingId in the next step to focus on a particular operation.</span></span>
4. <span data-ttu-id="e438d-144">Ahhoz, hogy egy adott sikertelen művelettel állapotüzenetet, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e438d-144">To get the status message of a particular failed operation, use the following command:</span></span>

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    <span data-ttu-id="e438d-145">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="e438d-145">Which returns:</span></span>

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. <span data-ttu-id="e438d-146">Minden központi telepítési műveletet az Azure-ban kérés- és tartalmát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e438d-146">Every deployment operation in Azure includes request and response content.</span></span> <span data-ttu-id="e438d-147">A kérelem tartalma az Ön által küldött Azure üzembe helyezése során (például hozzon létre egy virtuális Gépet, az operációsrendszer-lemez és az egyéb erőforrások).</span><span class="sxs-lookup"><span data-stu-id="e438d-147">The request content is what you sent to Azure during deployment (for example, create a VM, OS disk, and other resources).</span></span> <span data-ttu-id="e438d-148">A válasz tartalma az Azure által küldött vissza a központi telepítési kérelemből.</span><span class="sxs-lookup"><span data-stu-id="e438d-148">The response content is what Azure sent back from your deployment request.</span></span> <span data-ttu-id="e438d-149">A telepítés során használhatja **DeploymentDebugLogLevel** paramenter adhatja meg, hogy a kérelem és/vagy a válasz a naplóba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="e438d-149">During deployment, you can use **DeploymentDebugLogLevel** paramenter to specify that the request and/or response are retained in the log.</span></span> 

  <span data-ttu-id="e438d-150">Ez az információ lekérése a naplót, és mentse helyileg a következő PowerShell-parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="e438d-150">You get that information from the log, and save it locally by using the following PowerShell commands:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a><span data-ttu-id="e438d-151">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e438d-151">Azure CLI</span></span>

1. <span data-ttu-id="e438d-152">Az alkalmazáspéldány általános állapotának lekérése a **azure-csoportok telepítési megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="e438d-152">Get the overall status of a deployment with the **azure group deployment show** command.</span></span>

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  <span data-ttu-id="e438d-153">A visszaadott értékek közül egy a **correlationId**.</span><span class="sxs-lookup"><span data-stu-id="e438d-153">One of the returned values is the **correlationId**.</span></span> <span data-ttu-id="e438d-154">Ez az érték a kapcsolódó események nyomon követésére szolgál, és hasznos lehet, amikor olyan központi telepítés hibaelhárítása a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="e438d-154">This value is used to track related events, and can be helpful when working with technical support to troubleshoot a deployment.</span></span>

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. <span data-ttu-id="e438d-155">A központi telepítés műveletek megjelenítéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="e438d-155">To see the operations for a deployment, use:</span></span>

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a><span data-ttu-id="e438d-156">REST</span><span class="sxs-lookup"><span data-stu-id="e438d-156">REST</span></span>

1. <span data-ttu-id="e438d-157">A központi telepítés adatainak beolvasása a [sablon-üzembehelyezés adatainak beolvasása](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="e438d-157">Get information about a deployment with the [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operation.</span></span>

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    <span data-ttu-id="e438d-158">A válaszban, vegye figyelembe különösen a **provisioningState**, **correlationId**, és **hiba** elemek.</span><span class="sxs-lookup"><span data-stu-id="e438d-158">In the response, note in particular the **provisioningState**, **correlationId**, and **error** elements.</span></span> <span data-ttu-id="e438d-159">A **correlationId** kapcsolódó események nyomon követésére szolgál, és hasznos lehet, amikor olyan központi telepítés hibaelhárítása a technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="e438d-159">The **correlationId** is used to track related events, and can be helpful when working with technical support to troubleshoot a deployment.</span></span>

  ```json
  { 
    ...
    "properties": {
      "provisioningState":"Failed",
      "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
      ...
      "error":{
        "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
        "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
      }  
    }
  }
  ```

2. <span data-ttu-id="e438d-160">Az üzembe helyezési műveletek adatainak beolvasása a [összes sablon telepítési művelet](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) műveletet.</span><span class="sxs-lookup"><span data-stu-id="e438d-160">Get information about deployment operations with the [List all template deployment operations](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operation.</span></span> 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    <span data-ttu-id="e438d-161">A válasz alapján megadva a kérelem és/vagy válasz információkat tartalmaz a **debugSetting** tulajdonság üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="e438d-161">The response includes request and/or response information based on what you specified in the **debugSetting** property during deployment.</span></span>

  ```json
  {
    ...
    "properties": 
    {
      ...
      "request":{
        "content":{
          "location":"West US",
          "properties":{
            "accountType": "Standard_LRS"
          }
        }
      },
      "response":{
        "content":{
          "error":{
            "message":"Conflict","code":"Conflict"
          }
        }
      }
    }
  }
  ```


## <a name="next-steps"></a><span data-ttu-id="e438d-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e438d-162">Next steps</span></span>
* <span data-ttu-id="e438d-163">Segítség az adott telepítési hibáinak megoldása: [gyakori hibák feloldása, amikor erőforrásokat üzembe helyezi az Azure-bA az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e438d-163">For help with resolving particular deployment errors, see [Resolve common errors when deploying resources to Azure with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="e438d-164">A tevékenység-naplók segítségével figyelheti a más típusú műveleteket, lásd: [tevékenységi naplóit Azure-erőforrások kezeléséhez megtekintése](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="e438d-164">To learn about using the activity logs to monitor other types of actions, see [View activity logs to manage Azure resources](resource-group-audit.md).</span></span>
* <span data-ttu-id="e438d-165">A telepítés előtt futtatnia kell az ellenőrzéséhez tekintse meg a [Azure Resource Manager sablonnal erőforráscsoport telepítése](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e438d-165">To validate your deployment before executing it, see [Deploy a resource group with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

