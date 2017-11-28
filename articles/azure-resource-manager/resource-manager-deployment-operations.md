---
title: "az Azure Resource Manager aaaDeployment műveletek |} Microsoft Docs"
description: "Ismerteti, hogyan tooview Azure Resource Manager üzembe helyezési műveleteket a hello portálon, a PowerShell, a Azure CLI és a REST API-t."
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
ms.openlocfilehash: ba4823ca73caca83dfc07c99d736344ef8b7b54d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a><span data-ttu-id="b4fe7-103">Az Azure Resource Manager központi telepítési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b4fe7-103">View deployment operations with Azure Resource Manager</span></span>


<span data-ttu-id="b4fe7-104">Hello műveleteket a központi telepítés hello Azure-portálon keresztül tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-104">You can view hello operations for a deployment through hello Azure portal.</span></span> <span data-ttu-id="b4fe7-105">Előfordulhat, hogy tervezheti meg hello műveletek megtekintése hiba beérkezése után üzembe helyezése során, ez a cikk foglalkozik, amelyek nem tudták műveletek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-105">You may be most interested in viewing hello operations when you have received an error during deployment so this article focuses on viewing operations that have failed.</span></span> <span data-ttu-id="b4fe7-106">hello portál egy felületet biztosít, amely lehetővé teszi a tooeasily keresés hello hibákat, és határozza meg a lehetséges javításokat.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-106">hello portal provides an interface that enables you tooeasily find hello errors and determine potential fixes.</span></span>

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a><span data-ttu-id="b4fe7-107">Portál</span><span class="sxs-lookup"><span data-stu-id="b4fe7-107">Portal</span></span>
<span data-ttu-id="b4fe7-108">toosee hello telepítési műveleteit, a lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-108">toosee hello deployment operations, use hello following steps:</span></span>

1. <span data-ttu-id="b4fe7-109">Hello erőforráscsoport hello telepítési részt figyelje meg hello hello utolsó központi telepítésének állapotát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-109">For hello resource group involved in hello deployment, notice hello status of hello last deployment.</span></span> <span data-ttu-id="b4fe7-110">Kiválaszthatja a állapot tooget további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-110">You can select this status tooget more details.</span></span>
   
    ![Központi telepítési állapota](./media/resource-manager-deployment-operations/deployment-status.png)
2. <span data-ttu-id="b4fe7-112">Hello legutóbbi központi telepítés előzményei láthatja.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-112">You see hello recent deployment history.</span></span> <span data-ttu-id="b4fe7-113">Válassza ki a sikertelen hello telepítést.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-113">Select hello deployment that failed.</span></span>
   
    ![Központi telepítési állapota](./media/resource-manager-deployment-operations/select-deployment.png)
3. <span data-ttu-id="b4fe7-115">Válassza ki a hello hivatkozás toosee miért leírása hello központi telepítése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-115">Select hello link toosee a description of why hello deployment failed.</span></span> <span data-ttu-id="b4fe7-116">A hello alábbi rendszerképek közül hello DNS-rekord nem egyedi.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-116">In hello image below, hello DNS record is not unique.</span></span>  
   
    ![sikertelen központi telepítés megtekintése](./media/resource-manager-deployment-operations/view-error.png)
   
    <span data-ttu-id="b4fe7-118">Ez a hibaüzenet kell lennie ahhoz, akkor a hibaelhárítás toobegin.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-118">This error message should be enough for you toobegin troubleshooting.</span></span> <span data-ttu-id="b4fe7-119">Azonban ha olvashat arról, hogy mely feladatok befejeződtek, megtekintheti hello műveletek ahogy az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-119">However, if you need more details about which tasks were completed, you can view hello operations as shown in hello following steps.</span></span>
4. <span data-ttu-id="b4fe7-120">Összes hello telepítési művelet megtekintheti a hello **telepítési** panelen.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-120">You can view all hello deployment operations in hello **Deployment** blade.</span></span> <span data-ttu-id="b4fe7-121">Válassza ki a bármilyen műveletet toosee további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-121">Select any operation toosee more details.</span></span>
   
    ![műveletek megtekintése](./media/resource-manager-deployment-operations/view-operations.png)
   
    <span data-ttu-id="b4fe7-123">Ebben az esetben láthatja, hogy hello tárfiókot, a virtuális hálózat és a rendelkezésre állási csoport sikeresen létrehozva.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-123">In this case, you see that hello storage account, virtual network, and availability set were successfully created.</span></span> <span data-ttu-id="b4fe7-124">hello nyilvános IP-címet nem sikerült, és más erőforrások nem megkísérelte.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-124">hello public IP address failed, and other resources were not attempted.</span></span>
5. <span data-ttu-id="b4fe7-125">Kiválasztásával megtekintheti a hello telepítéshez események **események**.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-125">You can view events for hello deployment by selecting **Events**.</span></span>
   
    ![események megtekintése](./media/resource-manager-deployment-operations/view-events.png)
6. <span data-ttu-id="b4fe7-127">Hello központi telepítés összes hello eseményeket, és válassza ki a közül legalább egy további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-127">You see all hello events for hello deployment and select any one for more details.</span></span> <span data-ttu-id="b4fe7-128">Figyelje meg a korrelációs azonosító túl hello.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-128">Notice too hello correlation IDs.</span></span> <span data-ttu-id="b4fe7-129">Ez az érték akkor lehet hasznos, amikor a központi telepítés a technikai támogatási szolgálathoz tootroubleshoot olyan.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-129">This value can be helpful when working with technical support tootroubleshoot a deployment.</span></span>
   
    ![események](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a><span data-ttu-id="b4fe7-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4fe7-131">PowerShell</span></span>
1. <span data-ttu-id="b4fe7-132">tooget hello összesített állapotát a központi telepítés használata hello **Get-AzureRmResourceGroupDeployment** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-132">tooget hello overall status of a deployment, use hello **Get-AzureRmResourceGroupDeployment** command.</span></span> 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   <span data-ttu-id="b4fe7-133">Vagy csak a sikertelen telepítések hello eredményeket szűrheti is.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-133">Or, you can filter hello results for only those deployments that have failed.</span></span>

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. <span data-ttu-id="b4fe7-134">Minden központi telepítési több műveletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-134">Each deployment includes multiple operations.</span></span> <span data-ttu-id="b4fe7-135">Egyes műveletek hello telepítési folyamat azon lépése jelöli.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-135">Each operation represents a step in hello deployment process.</span></span> <span data-ttu-id="b4fe7-136">hibás telepítés okát toodiscover, általában szükséges hello üzembe helyezési műveletek toosee adatait.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-136">toodiscover what went wrong with a deployment, you usually need toosee details about hello deployment operations.</span></span> <span data-ttu-id="b4fe7-137">Hello műveletek hello állapotát megtekintheti **Get-AzureRmResourceGroupDeploymentOperation**.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-137">You can see hello status of hello operations with **Get-AzureRmResourceGroupDeploymentOperation**.</span></span>

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    <span data-ttu-id="b4fe7-138">Több művelet található a következő formátum hello mindegyiknél visszaadó:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-138">Which returns multiple operations with each one in hello following format:</span></span>

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. <span data-ttu-id="b4fe7-139">tooget további információt a sikertelen műveleteket műveletek hello tulajdonságainak lekérése **sikertelen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-139">tooget more details about failed operations, retrieve hello properties for operations with **Failed** state.</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    <span data-ttu-id="b4fe7-140">Összes sikertelen műveleteket hello minden valamelyik a következő formátum hello visszaadó:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-140">Which returns all hello failed operations with each one in hello following format:</span></span>

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

    <span data-ttu-id="b4fe7-141">Megjegyzés: hello serviceRequestId és hello trackingId hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-141">Note hello serviceRequestId and hello trackingId for hello operation.</span></span> <span data-ttu-id="b4fe7-142">hello serviceRequestId akkor lehet hasznos, amikor a központi telepítés a technikai támogatási szolgálathoz tootroubleshoot olyan.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-142">hello serviceRequestId can be helpful when working with technical support tootroubleshoot a deployment.</span></span> <span data-ttu-id="b4fe7-143">A következő lépés toofocus hello egy adott művelethez a hello trackingId fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-143">You will use hello trackingId in hello next step toofocus on a particular operation.</span></span>
4. <span data-ttu-id="b4fe7-144">tooget hello állapotüzenetét egy adott sikertelen művelettel, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-144">tooget hello status message of a particular failed operation, use hello following command:</span></span>

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    <span data-ttu-id="b4fe7-145">Amely adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-145">Which returns:</span></span>

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. <span data-ttu-id="b4fe7-146">Minden központi telepítési műveletet az Azure-ban kérés- és tartalmát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-146">Every deployment operation in Azure includes request and response content.</span></span> <span data-ttu-id="b4fe7-147">hello kérelem tartalma az Ön által küldött tooAzure üzembe helyezése során (például hozzon létre egy virtuális Gépet, az operációsrendszer-lemez és az egyéb erőforrások).</span><span class="sxs-lookup"><span data-stu-id="b4fe7-147">hello request content is what you sent tooAzure during deployment (for example, create a VM, OS disk, and other resources).</span></span> <span data-ttu-id="b4fe7-148">hello válasz tartalma az Azure által küldött vissza a központi telepítési kérelemből.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-148">hello response content is what Azure sent back from your deployment request.</span></span> <span data-ttu-id="b4fe7-149">A telepítés során használhatja **DeploymentDebugLogLevel** paramenter toospecify, hogy a kérés vagy válasz hello hello naplóba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-149">During deployment, you can use **DeploymentDebugLogLevel** paramenter toospecify that hello request and/or response are retained in hello log.</span></span> 

  <span data-ttu-id="b4fe7-150">Ezt az információt lekérni hello naplót, és mentse helyileg a hello a következő PowerShell-parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-150">You get that information from hello log, and save it locally by using hello following PowerShell commands:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a><span data-ttu-id="b4fe7-151">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b4fe7-151">Azure CLI</span></span>

1. <span data-ttu-id="b4fe7-152">Első hello a központi telepítés általános állapota a hello **azure-csoportok telepítési megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-152">Get hello overall status of a deployment with hello **azure group deployment show** command.</span></span>

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  <span data-ttu-id="b4fe7-153">Visszaadott értékek hello egyik hello **correlationId**.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-153">One of hello returned values is hello **correlationId**.</span></span> <span data-ttu-id="b4fe7-154">Ez az érték használható tootrack kapcsolatos eseményeket, és az is lehet hasznos, ha használata a technikai támogatási szolgálathoz tootroubleshoot a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-154">This value is used tootrack related events, and can be helpful when working with technical support tootroubleshoot a deployment.</span></span>

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. <span data-ttu-id="b4fe7-155">toosee hello műveletek üzembe helyezés esetén használja:</span><span class="sxs-lookup"><span data-stu-id="b4fe7-155">toosee hello operations for a deployment, use:</span></span>

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a><span data-ttu-id="b4fe7-156">REST</span><span class="sxs-lookup"><span data-stu-id="b4fe7-156">REST</span></span>

1. <span data-ttu-id="b4fe7-157">A központi telepítés hello adatainak beolvasása [sablon-üzembehelyezés adatainak beolvasása](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-157">Get information about a deployment with hello [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operation.</span></span>

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    <span data-ttu-id="b4fe7-158">Hello válaszként, vegye figyelembe különösen hello **provisioningState**, **correlationId**, és **hiba** elemek.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-158">In hello response, note in particular hello **provisioningState**, **correlationId**, and **error** elements.</span></span> <span data-ttu-id="b4fe7-159">Hello **correlationId** használt tootrack kapcsolatos eseményeket, és az is lehet hasznos, ha használata a technikai támogatási szolgálathoz tootroubleshoot a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-159">hello **correlationId** is used tootrack related events, and can be helpful when working with technical support tootroubleshoot a deployment.</span></span>

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

2. <span data-ttu-id="b4fe7-160">Telepítési műveletek hello adatainak beolvasása [összes sablon telepítési művelet](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) műveletet.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-160">Get information about deployment operations with hello [List all template deployment operations](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operation.</span></span> 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    <span data-ttu-id="b4fe7-161">hello válasz tartalmazza a kérés vagy válasz információk alapján hello megadott **debugSetting** tulajdonság üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="b4fe7-161">hello response includes request and/or response information based on what you specified in hello **debugSetting** property during deployment.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="b4fe7-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4fe7-162">Next steps</span></span>
* <span data-ttu-id="b4fe7-163">Segítség az adott telepítési hibáinak megoldása: [gyakori hibák feloldása, amikor üzembe helyezése az Azure Resource Manager erőforrások tooAzure](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="b4fe7-163">For help with resolving particular deployment errors, see [Resolve common errors when deploying resources tooAzure with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="b4fe7-164">hello tevékenység használatáról toolearn naplózza toomonitor más típusú műveleteket című [tevékenység megtekintése naplózza toomanage Azure erőforrások](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="b4fe7-164">toolearn about using hello activity logs toomonitor other types of actions, see [View activity logs toomanage Azure resources](resource-group-audit.md).</span></span>
* <span data-ttu-id="b4fe7-165">toovalidate a központi telepítés előtt futtatnia kell, lásd: [Azure Resource Manager sablonnal erőforráscsoport telepítése](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b4fe7-165">toovalidate your deployment before executing it, see [Deploy a resource group with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

