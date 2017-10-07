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
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Az Azure Resource Manager központi telepítési műveletek megtekintése


Hello műveleteket a központi telepítés hello Azure-portálon keresztül tekintheti meg. Előfordulhat, hogy tervezheti meg hello műveletek megtekintése hiba beérkezése után üzembe helyezése során, ez a cikk foglalkozik, amelyek nem tudták műveletek megtekintése. hello portál egy felületet biztosít, amely lehetővé teszi a tooeasily keresés hello hibákat, és határozza meg a lehetséges javításokat.

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a>Portál
toosee hello telepítési műveleteit, a lépéseket követve hello használata:

1. Hello erőforráscsoport hello telepítési részt figyelje meg hello hello utolsó központi telepítésének állapotát tartalmazza. Kiválaszthatja a állapot tooget további részleteket.
   
    ![Központi telepítési állapota](./media/resource-manager-deployment-operations/deployment-status.png)
2. Hello legutóbbi központi telepítés előzményei láthatja. Válassza ki a sikertelen hello telepítést.
   
    ![Központi telepítési állapota](./media/resource-manager-deployment-operations/select-deployment.png)
3. Válassza ki a hello hivatkozás toosee miért leírása hello központi telepítése nem sikerült. A hello alábbi rendszerképek közül hello DNS-rekord nem egyedi.  
   
    ![sikertelen központi telepítés megtekintése](./media/resource-manager-deployment-operations/view-error.png)
   
    Ez a hibaüzenet kell lennie ahhoz, akkor a hibaelhárítás toobegin. Azonban ha olvashat arról, hogy mely feladatok befejeződtek, megtekintheti hello műveletek ahogy az alábbi lépésekkel hello.
4. Összes hello telepítési művelet megtekintheti a hello **telepítési** panelen. Válassza ki a bármilyen műveletet toosee további részleteket.
   
    ![műveletek megtekintése](./media/resource-manager-deployment-operations/view-operations.png)
   
    Ebben az esetben láthatja, hogy hello tárfiókot, a virtuális hálózat és a rendelkezésre állási csoport sikeresen létrehozva. hello nyilvános IP-címet nem sikerült, és más erőforrások nem megkísérelte.
5. Kiválasztásával megtekintheti a hello telepítéshez események **események**.
   
    ![események megtekintése](./media/resource-manager-deployment-operations/view-events.png)
6. Hello központi telepítés összes hello eseményeket, és válassza ki a közül legalább egy további részleteket. Figyelje meg a korrelációs azonosító túl hello. Ez az érték akkor lehet hasznos, amikor a központi telepítés a technikai támogatási szolgálathoz tootroubleshoot olyan.
   
    ![események](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. tooget hello összesített állapotát a központi telepítés használata hello **Get-AzureRmResourceGroupDeployment** parancsot. 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Vagy csak a sikertelen telepítések hello eredményeket szűrheti is.

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. Minden központi telepítési több műveletet tartalmaz. Egyes műveletek hello telepítési folyamat azon lépése jelöli. hibás telepítés okát toodiscover, általában szükséges hello üzembe helyezési műveletek toosee adatait. Hello műveletek hello állapotát megtekintheti **Get-AzureRmResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Több művelet található a következő formátum hello mindegyiknél visszaadó:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. tooget további információt a sikertelen műveleteket műveletek hello tulajdonságainak lekérése **sikertelen** állapotát.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Összes sikertelen műveleteket hello minden valamelyik a következő formátum hello visszaadó:

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

    Megjegyzés: hello serviceRequestId és hello trackingId hello a művelethez. hello serviceRequestId akkor lehet hasznos, amikor a központi telepítés a technikai támogatási szolgálathoz tootroubleshoot olyan. A következő lépés toofocus hello egy adott művelethez a hello trackingId fogja használni.
4. tooget hello állapotüzenetét egy adott sikertelen művelettel, a következő parancs használata hello:

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Amely adja vissza:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. Minden központi telepítési műveletet az Azure-ban kérés- és tartalmát tartalmazza. hello kérelem tartalma az Ön által küldött tooAzure üzembe helyezése során (például hozzon létre egy virtuális Gépet, az operációsrendszer-lemez és az egyéb erőforrások). hello válasz tartalma az Azure által küldött vissza a központi telepítési kérelemből. A telepítés során használhatja **DeploymentDebugLogLevel** paramenter toospecify, hogy a kérés vagy válasz hello hello naplóba kerülnek. 

  Ezt az információt lekérni hello naplót, és mentse helyileg a hello a következő PowerShell-parancsok használatával:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure CLI

1. Első hello a központi telepítés általános állapota a hello **azure-csoportok telepítési megjelenítése** parancsot.

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  Visszaadott értékek hello egyik hello **correlationId**. Ez az érték használható tootrack kapcsolatos eseményeket, és az is lehet hasznos, ha használata a technikai támogatási szolgálathoz tootroubleshoot a központi telepítés.

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. toosee hello műveletek üzembe helyezés esetén használja:

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a>REST

1. A központi telepítés hello adatainak beolvasása [sablon-üzembehelyezés adatainak beolvasása](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) műveletet.

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    Hello válaszként, vegye figyelembe különösen hello **provisioningState**, **correlationId**, és **hiba** elemek. Hello **correlationId** használt tootrack kapcsolatos eseményeket, és az is lehet hasznos, ha használata a technikai támogatási szolgálathoz tootroubleshoot a központi telepítés.

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

2. Telepítési műveletek hello adatainak beolvasása [összes sablon telepítési művelet](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) műveletet. 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    hello válasz tartalmazza a kérés vagy válasz információk alapján hello megadott **debugSetting** tulajdonság üzembe helyezése során.

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


## <a name="next-steps"></a>Következő lépések
* Segítség az adott telepítési hibáinak megoldása: [gyakori hibák feloldása, amikor üzembe helyezése az Azure Resource Manager erőforrások tooAzure](resource-manager-common-deployment-errors.md).
* hello tevékenység használatáról toolearn naplózza toomonitor más típusú műveleteket című [tevékenység megtekintése naplózza toomanage Azure erőforrások](resource-group-audit.md).
* toovalidate a központi telepítés előtt futtatnia kell, lásd: [Azure Resource Manager sablonnal erőforráscsoport telepítése](resource-group-template-deploy.md).

