---
title: "a REST API-t és a sablon aaaDeploy erőforrások |} Microsoft Docs"
description: "Azure Resource Manager és a Resource Manager REST API toodeploy egy erőforrások tooAzure használja. a Resource Manager-sablon hello erőforrások vannak definiálva."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 347ad3bdb604429e7291297d448688204af69b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Manager REST API-val
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portál](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Ez a cikk azt ismerteti, hogyan toouse hello Resource Manager REST API-t a Resource Manager sablonok toodeploy az erőforrások tooAzure.  

> [!TIP]
> A hibakeresés központi telepítése során hibát talál segítséget:
> 
> * [Üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md) toolearn kapcsolatos információk segítségével a hiba elhárítása
> * [Gyakori hibák elhárítása erőforrások tooAzure az Azure Resource Manager telepítésekor](resource-manager-common-deployment-errors.md) toolearn hogyan tooresolve gyakori telepítési hibák
> 
> 

A sablon lehet egy helyi fájl vagy a külső fájlra, amely egy URI keresztül érhető el. A sablon olyan tárfiókban helyezkedik el, ha korlátozni a hozzáférést toohello sablon, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során.

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-hello-rest-api"></a>Hello REST API-t üzembe helyezéséhez
1. Állítsa be [közös paraméterek és fejlécek](https://docs.microsoft.com/rest/api/index), beleértve a hitelesítési tokenek.
2. Ha nem rendelkezik egy meglévő erőforráscsoportot, hozzon létre egy erőforráscsoportot. Adja meg az előfizetés-azonosító, hello új erőforráscsoportot, és a helyet, a megoldáshoz szükséges hello nevét. További információkért lásd: [hozzon létre egy erőforráscsoportot](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. A telepítés előtt futtatnia kell a hello futtatásával érvényesíteni [egy sablon telepítésének ellenőrzése](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) műveletet. Hello központi telepítés tesztelésekor meg paramétereket pontosan ugyanúgy hello deployment (lásd a következő lépésben hello) végrehajtása közben.
4. A központi telepítés létrehozása. Adja meg az előfizetés-Azonosítóval, a hello hello erőforráscsoport nevét, a hello telepítési hello neve és a hivatkozás tooyour sablont. Hello sablon fájllal kapcsolatos információkért lásd: [paraméterfájl](#parameter-file). További információ a hello REST API toocreate erőforráscsoport: [hozzon létre egy sablon-üzembehelyezés](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Értesítés hello **mód** értéke túl**növekményes**. a teljes telepítési toorun beállítása **mód** túl**Complete**. Ügyeljen arra, hogy ha hello teljes módot, ha véletlenül is törli a sablonban lévő erőforrásokhoz.
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      Ha azt szeretné, toolog válasz tartalmat, a kérés tartalma vagy mindkettőt, **debugSetting** hello kérelemben.
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      Beállíthatja a tárolási fiók toouse egy közös hozzáférésű jogosultságkód (SAS) jogkivonatot. További információkért lásd: [egy közös hozzáférésű Jogosultságkód hozzáférést delegálása](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).
5. Hello sablon-üzembehelyezés hello állapotának beolvasása. További információkért lásd: [sablon-üzembehelyezés adatainak beolvasása](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a>Paraméterfájl

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Következő lépések
* toolearn aszinkron REST műveleteinek, kezelésével kapcsolatban lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).
* Például egy keresztül hello .NET ügyféloldali kódtár erőforrásokat üzembe helyezi, lásd: [központi telepítése a .NET-kódtárakra és egy sablon használatával erőforrások](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).
* A megoldás toodifferent környezetei telepítésével kapcsolatos útmutatásért lásd: [a Microsoft Azure-ban fejlesztési és tesztkörnyezetek](solution-dev-test-environments.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

