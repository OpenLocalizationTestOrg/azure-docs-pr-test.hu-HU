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
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a><span data-ttu-id="bf350-104">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure Manager REST API-val</span><span class="sxs-lookup"><span data-stu-id="bf350-104">Deploy resources with Resource Manager templates and Resource Manager REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bf350-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf350-105">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="bf350-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="bf350-106">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="bf350-107">Portál</span><span class="sxs-lookup"><span data-stu-id="bf350-107">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="bf350-108">REST API</span><span class="sxs-lookup"><span data-stu-id="bf350-108">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="bf350-109">Ez a cikk azt ismerteti, hogyan toouse hello Resource Manager REST API-t a Resource Manager sablonok toodeploy az erőforrások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="bf350-109">This article explains how toouse hello Resource Manager REST API with Resource Manager templates toodeploy your resources tooAzure.</span></span>  

> [!TIP]
> <span data-ttu-id="bf350-110">A hibakeresés központi telepítése során hibát talál segítséget:</span><span class="sxs-lookup"><span data-stu-id="bf350-110">For help with debugging an error during deployment, see:</span></span>
> 
> * <span data-ttu-id="bf350-111">[Üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md) toolearn kapcsolatos információk segítségével a hiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="bf350-111">[View deployment operations](resource-manager-deployment-operations.md) toolearn about getting information that helps you troubleshoot your error</span></span>
> * <span data-ttu-id="bf350-112">[Gyakori hibák elhárítása erőforrások tooAzure az Azure Resource Manager telepítésekor](resource-manager-common-deployment-errors.md) toolearn hogyan tooresolve gyakori telepítési hibák</span><span class="sxs-lookup"><span data-stu-id="bf350-112">[Troubleshoot common errors when deploying resources tooAzure with Azure Resource Manager](resource-manager-common-deployment-errors.md) toolearn how tooresolve common deployment errors</span></span>
> 
> 

<span data-ttu-id="bf350-113">A sablon lehet egy helyi fájl vagy a külső fájlra, amely egy URI keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="bf350-113">Your template can be either a local file or an external file that is available through a URI.</span></span> <span data-ttu-id="bf350-114">A sablon olyan tárfiókban helyezkedik el, ha korlátozni a hozzáférést toohello sablon, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="bf350-114">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span>

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-hello-rest-api"></a><span data-ttu-id="bf350-115">Hello REST API-t üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="bf350-115">Deploy with hello REST API</span></span>
1. <span data-ttu-id="bf350-116">Állítsa be [közös paraméterek és fejlécek](https://docs.microsoft.com/rest/api/index), beleértve a hitelesítési tokenek.</span><span class="sxs-lookup"><span data-stu-id="bf350-116">Set [common parameters and headers](https://docs.microsoft.com/rest/api/index), including authentication tokens.</span></span>
2. <span data-ttu-id="bf350-117">Ha nem rendelkezik egy meglévő erőforráscsoportot, hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="bf350-117">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="bf350-118">Adja meg az előfizetés-azonosító, hello új erőforráscsoportot, és a helyet, a megoldáshoz szükséges hello nevét.</span><span class="sxs-lookup"><span data-stu-id="bf350-118">Provide your subscription ID, hello name of hello new resource group, and location that you need for your solution.</span></span> <span data-ttu-id="bf350-119">További információkért lásd: [hozzon létre egy erőforráscsoportot](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="bf350-119">For more information, see [Create a resource group](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span></span>
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. <span data-ttu-id="bf350-120">A telepítés előtt futtatnia kell a hello futtatásával érvényesíteni [egy sablon telepítésének ellenőrzése](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) műveletet.</span><span class="sxs-lookup"><span data-stu-id="bf350-120">Validate your deployment before executing it by running hello [Validate a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operation.</span></span> <span data-ttu-id="bf350-121">Hello központi telepítés tesztelésekor meg paramétereket pontosan ugyanúgy hello deployment (lásd a következő lépésben hello) végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="bf350-121">When testing hello deployment, provide parameters exactly as you would when executing hello deployment (shown in hello next step).</span></span>
4. <span data-ttu-id="bf350-122">A központi telepítés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bf350-122">Create a deployment.</span></span> <span data-ttu-id="bf350-123">Adja meg az előfizetés-Azonosítóval, a hello hello erőforráscsoport nevét, a hello telepítési hello neve és a hivatkozás tooyour sablont.</span><span class="sxs-lookup"><span data-stu-id="bf350-123">Provide your subscription ID, hello name of hello resource group, hello name of hello deployment, and a link tooyour template.</span></span> <span data-ttu-id="bf350-124">Hello sablon fájllal kapcsolatos információkért lásd: [paraméterfájl](#parameter-file).</span><span class="sxs-lookup"><span data-stu-id="bf350-124">For information about hello template file, see [Parameter file](#parameter-file).</span></span> <span data-ttu-id="bf350-125">További információ a hello REST API toocreate erőforráscsoport: [hozzon létre egy sablon-üzembehelyezés](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="bf350-125">For more information about hello REST API toocreate a resource group, see [Create a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span></span> <span data-ttu-id="bf350-126">Értesítés hello **mód** értéke túl**növekményes**.</span><span class="sxs-lookup"><span data-stu-id="bf350-126">Notice hello **mode** is set too**Incremental**.</span></span> <span data-ttu-id="bf350-127">a teljes telepítési toorun beállítása **mód** túl**Complete**.</span><span class="sxs-lookup"><span data-stu-id="bf350-127">toorun a complete deployment, set **mode** too**Complete**.</span></span> <span data-ttu-id="bf350-128">Ügyeljen arra, hogy ha hello teljes módot, ha véletlenül is törli a sablonban lévő erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="bf350-128">Be careful when using hello complete mode as you can inadvertently delete resources that are not in your template.</span></span>
   
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
   
      <span data-ttu-id="bf350-129">Ha azt szeretné, toolog válasz tartalmat, a kérés tartalma vagy mindkettőt, **debugSetting** hello kérelemben.</span><span class="sxs-lookup"><span data-stu-id="bf350-129">If you want toolog response content, request content, or both, include **debugSetting** in hello request.</span></span>
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      <span data-ttu-id="bf350-130">Beállíthatja a tárolási fiók toouse egy közös hozzáférésű jogosultságkód (SAS) jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="bf350-130">You can set up your storage account toouse a shared access signature (SAS) token.</span></span> <span data-ttu-id="bf350-131">További információkért lásd: [egy közös hozzáférésű Jogosultságkód hozzáférést delegálása](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="bf350-131">For more information, see [Delegating Access with a Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span></span>
5. <span data-ttu-id="bf350-132">Hello sablon-üzembehelyezés hello állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bf350-132">Get hello status of hello template deployment.</span></span> <span data-ttu-id="bf350-133">További információkért lásd: [sablon-üzembehelyezés adatainak beolvasása](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span><span class="sxs-lookup"><span data-stu-id="bf350-133">For more information, see [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span></span>
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a><span data-ttu-id="bf350-134">Paraméterfájl</span><span class="sxs-lookup"><span data-stu-id="bf350-134">Parameter file</span></span>

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a><span data-ttu-id="bf350-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf350-135">Next steps</span></span>
* <span data-ttu-id="bf350-136">toolearn aszinkron REST műveleteinek, kezelésével kapcsolatban lásd: [nyomon követheti a aszinkron Azure műveleteket](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="bf350-136">toolearn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
* <span data-ttu-id="bf350-137">Például egy keresztül hello .NET ügyféloldali kódtár erőforrásokat üzembe helyezi, lásd: [központi telepítése a .NET-kódtárakra és egy sablon használatával erőforrások](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bf350-137">For an example of deploying resources through hello .NET client library, see [Deploy resources using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="bf350-138">a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="bf350-138">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="bf350-139">A megoldás toodifferent környezetei telepítésével kapcsolatos útmutatásért lásd: [a Microsoft Azure-ban fejlesztési és tesztkörnyezetek](solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="bf350-139">For guidance on deploying your solution toodifferent environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="bf350-140">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="bf350-140">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

