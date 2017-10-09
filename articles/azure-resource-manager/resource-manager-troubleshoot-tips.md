---
title: "az Azure-telepítés hibák aaaUnderstand |} Microsoft Docs"
description: "Ismerteti, hogyan toolearn Azure telepítési hibái."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "központi telepítési hiba, az azure-telepítés tooazure központi telepítése"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: a335e121e9b908a763374907e34b1f6e823d6e96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-deployment-errors"></a><span data-ttu-id="49f6a-104">Az Azure-telepítés hibák megismerése</span><span class="sxs-lookup"><span data-stu-id="49f6a-104">Understand Azure deployment errors</span></span>
<span data-ttu-id="49f6a-105">Ez a témakör ismerteti a telepítési hibákat, és hogyan felderíthetők a hibával kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="49f6a-105">This topic describes deployment errors and how you can discover more information about an error.</span></span> <span data-ttu-id="49f6a-106">A megoldások toocommon telepítési hibákat, tekintse meg a [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="49f6a-106">For resolutions toocommon deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="two-types-of-errors"></a><span data-ttu-id="49f6a-107">Két típusú hibák</span><span class="sxs-lookup"><span data-stu-id="49f6a-107">Two types of errors</span></span>
<span data-ttu-id="49f6a-108">A hibák fogadhat két típusa van:</span><span class="sxs-lookup"><span data-stu-id="49f6a-108">There are two types of errors you can receive:</span></span>

* <span data-ttu-id="49f6a-109">Érvényesítési hiba</span><span class="sxs-lookup"><span data-stu-id="49f6a-109">validation errors</span></span>
* <span data-ttu-id="49f6a-110">Telepítési hibák</span><span class="sxs-lookup"><span data-stu-id="49f6a-110">deployment errors</span></span>

<span data-ttu-id="49f6a-111">a következő kép hello hello tevékenységnapló-előfizetéshez tartozó jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="49f6a-111">hello following image shows hello activity log for a subscription.</span></span> <span data-ttu-id="49f6a-112">Azt jelenti, hogy a két üzemelő példány.</span><span class="sxs-lookup"><span data-stu-id="49f6a-112">It represents two deployments.</span></span> <span data-ttu-id="49f6a-113">Egy központi telepítési hello sablon ellenőrzése nem sikerült (**ellenőrzése**), és nem haladt.</span><span class="sxs-lookup"><span data-stu-id="49f6a-113">In one deployment, hello template failed validation (**Validate**) and did not proceed.</span></span> <span data-ttu-id="49f6a-114">A hello másik üzemelő példány, hello sablon érvényesítési átadott, de nem sikerült hello erőforrások létrehozásakor (**írási központi telepítések**).</span><span class="sxs-lookup"><span data-stu-id="49f6a-114">In hello other deployment, hello template passed validation but failed when creating hello resources (**Write Deployments**).</span></span> 

![hibakód megjelenítése](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

<span data-ttu-id="49f6a-116">Érvényesítési hibák merülnek fel a szolgáltatásokat, amelyek a központi telepítés előtt meg lehet határozni.</span><span class="sxs-lookup"><span data-stu-id="49f6a-116">Validation errors arise from scenarios that can be determined before deployment.</span></span> <span data-ttu-id="49f6a-117">Szintaktikai hibákat a sablont, vagy túllépné az előfizetés kvóták közben toodeploy erőforrások tartoznak.</span><span class="sxs-lookup"><span data-stu-id="49f6a-117">They include syntax errors in your template, or trying toodeploy resources that would exceed your subscription quotas.</span></span> <span data-ttu-id="49f6a-118">Telepítési hibák feltételek hello telepítési folyamat során előforduló merülhetnek fel.</span><span class="sxs-lookup"><span data-stu-id="49f6a-118">Deployment errors arise from conditions that occur during hello deployment process.</span></span> <span data-ttu-id="49f6a-119">Ezek tartalmazzák próbált tooaccess párhuzamosan telepített erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49f6a-119">They include trying tooaccess a resource that is being deployed in parallel.</span></span>

<span data-ttu-id="49f6a-120">Mindkét típusú hibákat visszatérési tootroubleshoot hello telepítési használt hibakódot.</span><span class="sxs-lookup"><span data-stu-id="49f6a-120">Both types of errors return an error code that you use tootroubleshoot hello deployment.</span></span> <span data-ttu-id="49f6a-121">Mindkét típusú hibák jelennek meg hello [tevékenységnapló](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="49f6a-121">Both types of errors appear in hello [activity log](resource-group-audit.md).</span></span> <span data-ttu-id="49f6a-122">Azonban érvényesítési hiba jelenik meg a telepítési előzményeket mert hello telepítési soha nem indult el.</span><span class="sxs-lookup"><span data-stu-id="49f6a-122">However, validation errors do not appear in your deployment history because hello deployment never started.</span></span>

## <a name="determine-error-code"></a><span data-ttu-id="49f6a-123">Határozza meg a hiba kódja</span><span class="sxs-lookup"><span data-stu-id="49f6a-123">Determine error code</span></span>

<span data-ttu-id="49f6a-124">Megismerheti a hiba bármikor hello hibaüzenet és hello hibakód.</span><span class="sxs-lookup"><span data-stu-id="49f6a-124">You can learn about an error by looking at hello error message and hello error code.</span></span> <span data-ttu-id="49f6a-125">Hello [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md) cikkben megoldások hibakóddal leírni.</span><span class="sxs-lookup"><span data-stu-id="49f6a-125">hello [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md) article lists resolutions by error code.</span></span> <span data-ttu-id="49f6a-126">Ez a témakör bemutatja, hogyan toouse hello Azure portál toodiscover hello hibakód.</span><span class="sxs-lookup"><span data-stu-id="49f6a-126">This topic shows how toouse hello Azure portal toodiscover hello error code.</span></span>

### <a name="validation-errors"></a><span data-ttu-id="49f6a-127">Érvényesítési hiba</span><span class="sxs-lookup"><span data-stu-id="49f6a-127">Validation errors</span></span>

<span data-ttu-id="49f6a-128">Hello portálon keresztül telepítésekor érvényesítési hiba láthatja az értékek elküldése után.</span><span class="sxs-lookup"><span data-stu-id="49f6a-128">When deploying through hello portal, you see a validation error after submitting your values.</span></span>

![portál érvényesítési hibaüzenet megjelenítése](./media/resource-manager-troubleshoot-tips/validation-error.png)

<span data-ttu-id="49f6a-130">Válassza ki a üdvözlőüzenetére további részleteket.</span><span class="sxs-lookup"><span data-stu-id="49f6a-130">Select hello message for more details.</span></span> <span data-ttu-id="49f6a-131">A kép a következő hello, tekintse meg az **InvalidTemplateDeployment** hiba, és egy házirend jelző üzenet blokkolva központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="49f6a-131">In hello following image, you see an **InvalidTemplateDeployment** error and a message that indicates a policy blocked deployment.</span></span>

![érvényesítési részletek megjelenítése](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a><span data-ttu-id="49f6a-133">Telepítési hibák</span><span class="sxs-lookup"><span data-stu-id="49f6a-133">Deployment errors</span></span>

<span data-ttu-id="49f6a-134">Amikor a hello művelet ellenőrzése sikeres, de a telepítés során nem sikerül, hello értesítések hello hiba látható.</span><span class="sxs-lookup"><span data-stu-id="49f6a-134">When hello operation passes validation, but fails during deployment, you see hello error in hello notifications.</span></span> <span data-ttu-id="49f6a-135">Válassza ki a hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="49f6a-135">Select hello notification.</span></span>

![értesítési hiba](./media/resource-manager-troubleshoot-tips/notification.png)

<span data-ttu-id="49f6a-137">Megjelenik a hello fejlesztésével kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="49f6a-137">You see more details about hello deployment.</span></span> <span data-ttu-id="49f6a-138">Válassza ki a hello beállítás toofind hello hibával kapcsolatban további információt.</span><span class="sxs-lookup"><span data-stu-id="49f6a-138">Select hello option toofind more information about hello error.</span></span>

![központi telepítése nem sikerült](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

<span data-ttu-id="49f6a-140">Hello üzenet és a hiba hibakódok láthatja.</span><span class="sxs-lookup"><span data-stu-id="49f6a-140">You see hello error message and error codes.</span></span> <span data-ttu-id="49f6a-141">Figyelje meg, hogy van két hibakódok.</span><span class="sxs-lookup"><span data-stu-id="49f6a-141">Notice there are two error codes.</span></span> <span data-ttu-id="49f6a-142">első hibakód hello (**"deploymentfailed"**) egy általános hiba, amely nem biztosít hello adatokat meg kell toosolve hello hiba.</span><span class="sxs-lookup"><span data-stu-id="49f6a-142">hello first error code (**DeploymentFailed**) is a general error that does not provide hello details you need toosolve hello error.</span></span> <span data-ttu-id="49f6a-143">második hibakód hello (**StorageAccountNotFound**) részletesen bemutatja a hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="49f6a-143">hello second error code (**StorageAccountNotFound**) provides hello details you need.</span></span> 

![Hiba legutolsó részletes adatai](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a><span data-ttu-id="49f6a-145">A hibakeresési naplózást engedélyező</span><span class="sxs-lookup"><span data-stu-id="49f6a-145">Enable debug logging</span></span>
<span data-ttu-id="49f6a-146">Néha kell hello kérelem-válasz toodiscover további információt a probléma okát.</span><span class="sxs-lookup"><span data-stu-id="49f6a-146">Sometimes you need more information about hello request and response toodiscover what went wrong.</span></span> <span data-ttu-id="49f6a-147">PowerShell vagy Azure CLI segítségével kérheti, hogy további információkat kerül a központi telepítés során.</span><span class="sxs-lookup"><span data-stu-id="49f6a-147">By using PowerShell or Azure CLI, you can request that additional information is logged during a deployment.</span></span>

- <span data-ttu-id="49f6a-148">PowerShell</span><span class="sxs-lookup"><span data-stu-id="49f6a-148">PowerShell</span></span>

   <span data-ttu-id="49f6a-149">A PowerShellben, állítsa be a hello **DeploymentDebugLogLevel** paraméter tooAll, ResponseContent vagy RequestContent.</span><span class="sxs-lookup"><span data-stu-id="49f6a-149">In PowerShell, set hello **DeploymentDebugLogLevel** parameter tooAll, ResponseContent, or RequestContent.</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   <span data-ttu-id="49f6a-150">Vizsgálja meg a hello kérés tartalma a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="49f6a-150">Examine hello request content with hello following cmdlet:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   <span data-ttu-id="49f6a-151">Vagy a tartalom válasz hello:</span><span class="sxs-lookup"><span data-stu-id="49f6a-151">Or, hello response content with:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   <span data-ttu-id="49f6a-152">Ezen információk segítségével meghatározhatja, hogy hello sablonban lévő érték helytelen beállítása.</span><span class="sxs-lookup"><span data-stu-id="49f6a-152">This information can help you determine whether a value in hello template is being incorrectly set.</span></span>

- <span data-ttu-id="49f6a-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="49f6a-153">Azure CLI</span></span>

   <span data-ttu-id="49f6a-154">Vizsgálja meg a hello üzembe helyezési műveleteinek a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="49f6a-154">Examine hello deployment operations with hello following command:</span></span>

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- <span data-ttu-id="49f6a-155">Beágyazott sablon</span><span class="sxs-lookup"><span data-stu-id="49f6a-155">Nested template</span></span>

   <span data-ttu-id="49f6a-156">egy beágyazott sablon használata hello toolog hibakeresési információ **debugSetting** elemet.</span><span class="sxs-lookup"><span data-stu-id="49f6a-156">toolog debug information for a nested template, use hello **debugSetting** element.</span></span>

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a><span data-ttu-id="49f6a-157">Hibaelhárítási-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="49f6a-157">Create a troubleshooting template</span></span>
<span data-ttu-id="49f6a-158">Bizonyos esetekben hello legegyszerűbb módja tootroubleshoot a sablon tootest része.</span><span class="sxs-lookup"><span data-stu-id="49f6a-158">In some cases, hello easiest way tootroubleshoot your template is tootest parts of it.</span></span> <span data-ttu-id="49f6a-159">Létrehozhat egy egyszerűsített sablont, amely lehetővé teszi toofocus hello részre, amelyekről hello hiba okozza.</span><span class="sxs-lookup"><span data-stu-id="49f6a-159">You can create a simplified template that enables you toofocus on hello part that you believe is causing hello error.</span></span> <span data-ttu-id="49f6a-160">Tegyük fel, hogy hiba azért kapta, való hivatkozáskor erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49f6a-160">For example, suppose you are receiving an error when referencing a resource.</span></span> <span data-ttu-id="49f6a-161">Ahelyett, hogy egy teljes sablon foglalkozik, hozzon létre egy sablont, amely a problémát okozó hello részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="49f6a-161">Rather than dealing with an entire template, create a template that returns hello part that may be causing your problem.</span></span> <span data-ttu-id="49f6a-162">Ez segít meghatározni, hogy átadott hello jobb paraméterek, a sablon függvénnyel megfelelően, és hello erőforrás lekérése várt.</span><span class="sxs-lookup"><span data-stu-id="49f6a-162">It can help you determine whether you are passing in hello right parameters, using template functions correctly, and getting hello resource you expect.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

<span data-ttu-id="49f6a-163">Vagy tegyük fel, hogy a központi telepítési hibái, amelyekről tooincorrectly függőségek beállítása kapcsolódó áll kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="49f6a-163">Or, suppose you are encountering deployment errors that you believe are related tooincorrectly set dependencies.</span></span> <span data-ttu-id="49f6a-164">Tesztelje a sablon ossza egyszerűsített sablonok.</span><span class="sxs-lookup"><span data-stu-id="49f6a-164">Test your template by breaking it into simplified templates.</span></span> <span data-ttu-id="49f6a-165">Először hozzon létre egy sablont, amely csak egyetlen erőforrás (például egy SQL Server) telepít.</span><span class="sxs-lookup"><span data-stu-id="49f6a-165">First, create a template that deploys only a single resource (like a SQL Server).</span></span> <span data-ttu-id="49f6a-166">Ha meggyőződött arról, hogy helyesen definiálva erőforrást is tartalmaz, amelyektől függ (például egy SQL-adatbázis) erőforrás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="49f6a-166">When you are sure you have that resource correctly defined, add a resource that depends on it (like a SQL Database).</span></span> <span data-ttu-id="49f6a-167">Ha két erőforrások helyesen definiálva van, adja hozzá a más függő erőforrások (például naplózási házirendek).</span><span class="sxs-lookup"><span data-stu-id="49f6a-167">When you have those two resources correctly defined, add other dependent resources (like auditing policies).</span></span> <span data-ttu-id="49f6a-168">Minden egyes próbatelepítés Between hello erőforrás csoport toomake meg arról, hogy Ön megfelelően tesztelési hello függőségek törlése.</span><span class="sxs-lookup"><span data-stu-id="49f6a-168">In between each test deployment, delete hello resource group toomake sure you adequately testing hello dependencies.</span></span> 

## <a name="check-deployment-sequence"></a><span data-ttu-id="49f6a-169">Feladatütemezési központi telepítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="49f6a-169">Check deployment sequence</span></span>

<span data-ttu-id="49f6a-170">Sok központi telepítési hiba fordulhat elő, amikor egy váratlan sorozat erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="49f6a-170">Many deployment errors happen when resources are deployed in an unexpected sequence.</span></span> <span data-ttu-id="49f6a-171">Ezeket a hibákat okozhat, ha a függőségek nem megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="49f6a-171">These errors arise when dependencies are not correctly set.</span></span> <span data-ttu-id="49f6a-172">Hiányzik egy szükséges függőséget, amikor egy erőforrást próbál toouse értékét, egy másik erőforrás, de más hello még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="49f6a-172">When you are missing a needed dependency, one resource attempts toouse a value for another resource but hello other does not yet exist.</span></span> <span data-ttu-id="49f6a-173">Hibaüzenet arról, hogy egy erőforrás nem található.</span><span class="sxs-lookup"><span data-stu-id="49f6a-173">You get an error stating that a resource is not found.</span></span> <span data-ttu-id="49f6a-174">Hiba az ilyen típusú felmerülhet időnként mivel hello központi telepítéskor az egyes erőforrások változhat.</span><span class="sxs-lookup"><span data-stu-id="49f6a-174">You may encounter this type of error intermittently because hello deployment time for each resource can vary.</span></span> <span data-ttu-id="49f6a-175">Például az első kísérlet toodeploy az erőforrások sikeres lesz, mert egy szükséges erőforrás véletlenszerűen idő alatt fejeződik be.</span><span class="sxs-lookup"><span data-stu-id="49f6a-175">For example, your first attempt toodeploy your resources succeeds because a required resource randomly completes in time.</span></span> <span data-ttu-id="49f6a-176">A második próbálkozás azonban sikertelen lesz, mert hello szükséges erőforrás nem fejeződött be időben.</span><span class="sxs-lookup"><span data-stu-id="49f6a-176">However, your second attempt fails because hello required resource did not complete in time.</span></span> 

<span data-ttu-id="49f6a-177">De azt szeretné, hogy tooavoid beállítás függőségek esetén nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="49f6a-177">But, you want tooavoid setting dependencies that are not needed.</span></span> <span data-ttu-id="49f6a-178">Ha szükségtelen függőségek, erőforrások, amelyek nincsenek a párhuzamos telepített egymástól függenek, hogy meghosszabbítása hello telepítési hello időtartama.</span><span class="sxs-lookup"><span data-stu-id="49f6a-178">When you have unnecessary dependencies, you prolong hello duration of hello deployment by preventing resources that are not dependent on each other from being deployed in parallel.</span></span> <span data-ttu-id="49f6a-179">Ezenkívül létrehozhat, amelyek blokkolják az hello telepítési körkörös függőségek.</span><span class="sxs-lookup"><span data-stu-id="49f6a-179">In addition, you may create circular dependencies that block hello deployment.</span></span> <span data-ttu-id="49f6a-180">Hello [hivatkozás](resource-group-template-functions-resource.md#reference) függvény hoz létre egy implicit függőség hello hivatkozott erőforrás hello erőforrás telepítésekor ugyanazt a sablont.</span><span class="sxs-lookup"><span data-stu-id="49f6a-180">hello [reference](resource-group-template-functions-resource.md#reference) function creates an implicit dependency on hello referenced resource, when that resource is deployed in hello same template.</span></span> <span data-ttu-id="49f6a-181">Ezért, akkor elképzelhető, hogy több mint hello megadott hello függőségek **dependsOn** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="49f6a-181">Therefore, you may have more dependencies than hello dependencies specified in hello **dependsOn** property.</span></span> <span data-ttu-id="49f6a-182">Hello [resourceId](resource-group-template-functions-resource.md#resourceid) függvény létrehozása egy implicit függőség, vagy nem ellenőrzi, hogy létezik-e hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49f6a-182">hello [resourceId](resource-group-template-functions-resource.md#resourceid) function does not create an implicit dependency or validate that hello resource exists.</span></span>

<span data-ttu-id="49f6a-183">A függőségi problémákat tapasztal, meg kell toogain betekintést erőforrások telepítése hello sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="49f6a-183">When you encounter dependency problems, you need toogain insight into hello order of resource deployment.</span></span> <span data-ttu-id="49f6a-184">telepítési műveletek tooview hello sorrendje:</span><span class="sxs-lookup"><span data-stu-id="49f6a-184">tooview hello order of deployment operations:</span></span>

1. <span data-ttu-id="49f6a-185">Válassza ki a hello üzembe helyezési előzményeket az erőforráscsoport számára.</span><span class="sxs-lookup"><span data-stu-id="49f6a-185">Select hello deployment history for your resource group.</span></span>

   ![Válassza ki a központi telepítés előzményei](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. <span data-ttu-id="49f6a-187">Jelölje ki a telepítést az hello előzményeket, és válassza ki **események**.</span><span class="sxs-lookup"><span data-stu-id="49f6a-187">Select a deployment from hello history, and select **Events**.</span></span>

   ![Válassza ki a központi telepítési eseményeket](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. <span data-ttu-id="49f6a-189">Vizsgálja meg az egyes erőforrások események sorozatát hello.</span><span class="sxs-lookup"><span data-stu-id="49f6a-189">Examine hello sequence of events for each resource.</span></span> <span data-ttu-id="49f6a-190">Nagy figyelmet toohello állapota minden egyes művelet.</span><span class="sxs-lookup"><span data-stu-id="49f6a-190">Pay attention toohello status of each operation.</span></span> <span data-ttu-id="49f6a-191">Például hello következő kép bemutatja, amelynek a telepítése három storage-fiókok párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="49f6a-191">For example, hello following image shows three storage accounts that deployed in parallel.</span></span> <span data-ttu-id="49f6a-192">Figyelje meg, hogy három tárfiókok hello: hello indulnak el egyszerre.</span><span class="sxs-lookup"><span data-stu-id="49f6a-192">Notice that hello three storage accounts are started at hello same time.</span></span>

   ![Párhuzamos üzembe helyezés](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   <span data-ttu-id="49f6a-194">hello következő kép bemutatja a három tárfiókok nem telepített párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="49f6a-194">hello next image shows three storage accounts that are not deployed in parallel.</span></span> <span data-ttu-id="49f6a-195">hello második tárfiók hello első tárfiókon, valamint hello harmadik tárfiók hello második tárfiók függ.</span><span class="sxs-lookup"><span data-stu-id="49f6a-195">hello second storage account depends on hello first storage account, and hello third storage account depends on hello second storage account.</span></span> <span data-ttu-id="49f6a-196">Ezért hello első tárfiók elkezdődött, elfogadott, és befejeződött hello mellett megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="49f6a-196">Therefore, hello first storage account is started, accepted, and completed before hello next is started.</span></span>

   ![szekvenciális központi telepítés](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

<span data-ttu-id="49f6a-198">Még a bonyolultabb esetek is használhat ugyanazon módszer toodiscover hello, ha a központi telepítés elkezdődött és befejeződött az egyes erőforrások.</span><span class="sxs-lookup"><span data-stu-id="49f6a-198">Even for more complicated scenarios, you can use hello same technique toodiscover when deployment is started and completed for each resource.</span></span> <span data-ttu-id="49f6a-199">Nézze át a központi telepítési események toosee, ha hello feladatütemezési különbözik attól a teheti meg.</span><span class="sxs-lookup"><span data-stu-id="49f6a-199">Look through your deployment events toosee if hello sequence is different than you would expect.</span></span> <span data-ttu-id="49f6a-200">Ha igen, újra kiértékelje hello függőségek ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="49f6a-200">If so, reevaluate hello dependencies for this resource.</span></span>

<span data-ttu-id="49f6a-201">Erőforrás-kezelő körkörös függőségi viszony azonosítja a sablon érvényesítése során.</span><span class="sxs-lookup"><span data-stu-id="49f6a-201">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="49f6a-202">Egy hibaüzenet arról, hogy kifejezetten körkörös függőség létezik adja vissza.</span><span class="sxs-lookup"><span data-stu-id="49f6a-202">It returns an error message that specifically states a circular dependency exists.</span></span> <span data-ttu-id="49f6a-203">Körkörös függőség toosolve:</span><span class="sxs-lookup"><span data-stu-id="49f6a-203">toosolve a circular dependency:</span></span>

1. <span data-ttu-id="49f6a-204">A sablonban meghatározott hello körkörös függőség hello erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="49f6a-204">In your template, find hello resource identified in hello circular dependency.</span></span> 
2. <span data-ttu-id="49f6a-205">Az adott erőforráshoz, vizsgálja meg a hello **dependsOn** tulajdonság és bármely használatát hello **hivatkozás** erőforrások attól függ, toosee működik.</span><span class="sxs-lookup"><span data-stu-id="49f6a-205">For that resource, examine hello **dependsOn** property and any uses of hello **reference** function toosee which resources it depends on.</span></span> 
3. <span data-ttu-id="49f6a-206">Vizsgálja meg az ezen erőforrások toosee, mely erőforrásokat függenek.</span><span class="sxs-lookup"><span data-stu-id="49f6a-206">Examine those resources toosee which resources they depend on.</span></span> <span data-ttu-id="49f6a-207">Hajtsa végre a hello függőségek meg, amikor egy erőforrást, amely hello eredeti erőforrás függ.</span><span class="sxs-lookup"><span data-stu-id="49f6a-207">Follow hello dependencies until you notice a resource that depends on hello original resource.</span></span>
5. <span data-ttu-id="49f6a-208">Hello erőforrások hello körkörös függőség részt, gondosan vizsgálja meg minden használatát hello **dependsOn** tulajdonság tooidentify esetén nem szükséges összes függőséget.</span><span class="sxs-lookup"><span data-stu-id="49f6a-208">For hello resources involved in hello circular dependency, carefully examine all uses of hello **dependsOn** property tooidentify any dependencies that are not needed.</span></span> <span data-ttu-id="49f6a-209">Távolítsa el ezeket a függőségeket.</span><span class="sxs-lookup"><span data-stu-id="49f6a-209">Remove those dependencies.</span></span> <span data-ttu-id="49f6a-210">Ha biztos abban, hogy a függőség van szükség, távolítsa el azt.</span><span class="sxs-lookup"><span data-stu-id="49f6a-210">If you are unsure that a dependency is needed, try removing it.</span></span> 
6. <span data-ttu-id="49f6a-211">Hello sablon újratelepítése.</span><span class="sxs-lookup"><span data-stu-id="49f6a-211">Redeploy hello template.</span></span>

<span data-ttu-id="49f6a-212">Értékek eltávolítása hello **dependsOn** tulajdonság hibákat eredményezhet, hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="49f6a-212">Removing values from hello **dependsOn** property can cause errors when you deploy hello template.</span></span> <span data-ttu-id="49f6a-213">Ha hibát észlel, vegye fel a hello függőségi vissza hello sablonba.</span><span class="sxs-lookup"><span data-stu-id="49f6a-213">If you encounter an error, add hello dependency back into hello template.</span></span> 

<span data-ttu-id="49f6a-214">Ha ezt a megközelítést nem oldja meg a hello körkörös függőséget, fontolja meg a központi telepítés logikája a gyermekszintű erőforrása (például a bővítmények vagy konfigurációs beállítások).</span><span class="sxs-lookup"><span data-stu-id="49f6a-214">If that approach does not solve hello circular dependency, consider moving part of your deployment logic into child resources (such as extensions or configuration settings).</span></span> <span data-ttu-id="49f6a-215">E gyermek erőforrások toodeploy konfigurálása után hello körkörös függőség hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="49f6a-215">Configure those child resources toodeploy after hello resources involved in hello circular dependency.</span></span> <span data-ttu-id="49f6a-216">Tegyük fel például, két olyan virtuális gépet telepít, de a tulajdonságok az egyes más toohello hivatkozó meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="49f6a-216">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer toohello other.</span></span> <span data-ttu-id="49f6a-217">A sorrend hello telepíthetné őket:</span><span class="sxs-lookup"><span data-stu-id="49f6a-217">You can deploy them in hello following order:</span></span>

1. <span data-ttu-id="49f6a-218">vm1</span><span class="sxs-lookup"><span data-stu-id="49f6a-218">vm1</span></span>
2. <span data-ttu-id="49f6a-219">vm2 virtuális gépnek</span><span class="sxs-lookup"><span data-stu-id="49f6a-219">vm2</span></span>
3. <span data-ttu-id="49f6a-220">Vm1 kiterjesztés vm1 és vm2 virtuális gépnek függ.</span><span class="sxs-lookup"><span data-stu-id="49f6a-220">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="49f6a-221">hello bővítmény, amely azt lekérése vm2 virtuális gépnek vm1 értékek beállítása.</span><span class="sxs-lookup"><span data-stu-id="49f6a-221">hello extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="49f6a-222">Bővítmény vm2 virtuális gépnek a vm1 és vm2 virtuális gépnek függ.</span><span class="sxs-lookup"><span data-stu-id="49f6a-222">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="49f6a-223">hello bővítmény értékek beállítása, amely azt lekérése vm1 vm2 virtuális gépnek.</span><span class="sxs-lookup"><span data-stu-id="49f6a-223">hello extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="49f6a-224">ugyanezt a megközelítést akkor működik, ha az App Service apps hello.</span><span class="sxs-lookup"><span data-stu-id="49f6a-224">hello same approach works for App Service apps.</span></span> <span data-ttu-id="49f6a-225">Fontolja meg konfigurációs értékeket az alkalmazás-erőforrást hello gyermek erőforrása.</span><span class="sxs-lookup"><span data-stu-id="49f6a-225">Consider moving configuration values into a child resource of hello app resource.</span></span> <span data-ttu-id="49f6a-226">Két webes alkalmazásokat telepíthet a sorrend hello:</span><span class="sxs-lookup"><span data-stu-id="49f6a-226">You can deploy two web apps in hello following order:</span></span>

1. <span data-ttu-id="49f6a-227">webapp1</span><span class="sxs-lookup"><span data-stu-id="49f6a-227">webapp1</span></span>
2. <span data-ttu-id="49f6a-228">webapp2</span><span class="sxs-lookup"><span data-stu-id="49f6a-228">webapp2</span></span>
3. <span data-ttu-id="49f6a-229">config webapp1 webapp1 és webapp2 függ.</span><span class="sxs-lookup"><span data-stu-id="49f6a-229">config for webapp1 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="49f6a-230">Alkalmazásbeállítások webapp2 értékeivel tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="49f6a-230">It contains app settings with values from webapp2.</span></span>
4. <span data-ttu-id="49f6a-231">config webapp2 webapp1 és webapp2 függ.</span><span class="sxs-lookup"><span data-stu-id="49f6a-231">config for webapp2 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="49f6a-232">Alkalmazásbeállítások webapp1 értékeivel tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="49f6a-232">It contains app settings with values from webapp1.</span></span>


## <a name="next-steps"></a><span data-ttu-id="49f6a-233">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49f6a-233">Next steps</span></span>
* <span data-ttu-id="49f6a-234">A megoldások toocommon telepítési hibákat, tekintse meg a [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="49f6a-234">For resolutions toocommon deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="49f6a-235">toolearn kapcsolatos naplózási műveletek, lásd: [naplózási műveletek a Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="49f6a-235">toolearn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="49f6a-236">toolearn kapcsolatos műveletek toodetermine hello hibák központi telepítése során lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="49f6a-236">toolearn about actions toodetermine hello errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
