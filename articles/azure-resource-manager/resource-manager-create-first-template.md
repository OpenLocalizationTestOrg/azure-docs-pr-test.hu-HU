---
title: "aaaCreate első Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "Egy részletes útmutató toocreating az első Azure Resource Manager-sablon. Az bemutatja, hogyan toouse hello tárolási fiók toocreate hello sablon hivatkozása."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 92e6d6bb7094fe0e4537ee080704967862804bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a><span data-ttu-id="b4a84-104">Az első Azure Resource Manager-sablon létrehozása ás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b4a84-104">Create and deploy your first Azure Resource Manager template</span></span>
<span data-ttu-id="b4a84-105">Ez a témakör bemutatja, hogyan hello az első Azure Resource Manager-sablonok létrehozásának lépésein.</span><span class="sxs-lookup"><span data-stu-id="b4a84-105">This topic walks you through hello steps of creating your first Azure Resource Manager template.</span></span> <span data-ttu-id="b4a84-106">Resource Manager-sablonok JSON-fájlokat, amelyek meghatározzák a megoldáshoz szükséges toodeploy hello erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b4a84-106">Resource Manager templates are JSON files that define hello resources you need toodeploy for your solution.</span></span> <span data-ttu-id="b4a84-107">toounderstand hello fogalmak társított telepítése és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b4a84-107">toounderstand hello concepts associated with deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span> <span data-ttu-id="b4a84-108">Ha meglévő erőforrásokat, és szeretné, hogy az adott erőforrás tooget egy sablon, lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="b4a84-108">If you have existing resources and want tooget a template for those resources, see [Export an Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

<span data-ttu-id="b4a84-109">toocreate és ellenőrzés sablonok, egy JSON-szerkesztővel kell.</span><span class="sxs-lookup"><span data-stu-id="b4a84-109">toocreate and revise templates, you need a JSON editor.</span></span> <span data-ttu-id="b4a84-110">A [Visual Studio Code](https://code.visualstudio.com/) egy könnyen használható, nyílt forráskódú, platformfüggetlen kódszerkesztő.</span><span class="sxs-lookup"><span data-stu-id="b4a84-110">[Visual Studio Code](https://code.visualstudio.com/) is a lightweight, open-source, cross-platform code editor.</span></span> <span data-ttu-id="b4a84-111">A Resource Manager-sablonok létrehozásához kifejezetten javasoljuk a Visual Studio Code használatát.</span><span class="sxs-lookup"><span data-stu-id="b4a84-111">We strongly recommend using Visual Studio Code for creating Resource Manager templates.</span></span> <span data-ttu-id="b4a84-112">Ez a témakör a VS Code használatát feltételezi, de használhat egyéb JSON-szerkesztőt is (pl. Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="b4a84-112">This topic assumes you are using VS Code; however, if you have another JSON editor (like Visual Studio), you can use that editor.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4a84-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b4a84-113">Prerequisites</span></span>

* <span data-ttu-id="b4a84-114">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b4a84-114">Visual Studio Code.</span></span> <span data-ttu-id="b4a84-115">Ha szükséges, a következő címről telepíthető: [https://code.visualstudio.com/](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b4a84-115">If needed, install it from [https://code.visualstudio.com/](https://code.visualstudio.com/).</span></span>
* <span data-ttu-id="b4a84-116">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b4a84-116">An Azure subscription.</span></span> <span data-ttu-id="b4a84-117">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="b4a84-117">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="create-template"></a><span data-ttu-id="b4a84-118">Sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4a84-118">Create template</span></span>

<span data-ttu-id="b4a84-119">Kezdjük azokkal, amelyek a tárfiók-előfizetés tooyour telepíti egyszerű sablonokat.</span><span class="sxs-lookup"><span data-stu-id="b4a84-119">Let's start with a simple template that deploys a storage account tooyour subscription.</span></span>

1. <span data-ttu-id="b4a84-120">Válassza a **File** (Fájl) > **New File** (Új fájl) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b4a84-120">Select **File** > **New File**.</span></span> 

   ![Új fájl](./media/resource-manager-create-first-template/new-file.png)

2. <span data-ttu-id="b4a84-122">Másolja és illessze be a fájlba a JSON-szintaxis következő hello:</span><span class="sxs-lookup"><span data-stu-id="b4a84-122">Copy and paste hello following JSON syntax into your file:</span></span>

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   <span data-ttu-id="b4a84-123">Tárfiókneveket rendelkezik, akkor tegye őket nehéz tooset több korlátozások.</span><span class="sxs-lookup"><span data-stu-id="b4a84-123">Storage account names have several restrictions that make them difficult tooset.</span></span> <span data-ttu-id="b4a84-124">hello nevének kell lennie 3 és 24 karakter hosszúságú, használja csak a számokat és kisbetűket tartalmazhatnak, és egyedi.</span><span class="sxs-lookup"><span data-stu-id="b4a84-124">hello name must be between 3 and 24 characters in length, use only numbers and lower-case letters, and be unique.</span></span> <span data-ttu-id="b4a84-125">hello előző sablon által hello [uniqueString](resource-group-template-functions-string.md#uniquestring) toogenerate kivonatot működik.</span><span class="sxs-lookup"><span data-stu-id="b4a84-125">hello preceding template uses hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function toogenerate a hash value.</span></span> <span data-ttu-id="b4a84-126">toogive Ez a kivonat értéke nagyobb, ami azt jelenti, hello előtag hozzáadja *tárolási*.</span><span class="sxs-lookup"><span data-stu-id="b4a84-126">toogive this hash value more meaning, it adds hello prefix *storage*.</span></span> 

3. <span data-ttu-id="b4a84-127">Mentse a fájlt **azuredeploy.json** tooa helyi mappát.</span><span class="sxs-lookup"><span data-stu-id="b4a84-127">Save this file as **azuredeploy.json** tooa local folder.</span></span>

   ![Sablon mentése](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a><span data-ttu-id="b4a84-129">Sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b4a84-129">Deploy template</span></span>

<span data-ttu-id="b4a84-130">Akkor van kész toodeploy ezt a sablont.</span><span class="sxs-lookup"><span data-stu-id="b4a84-130">You are ready toodeploy this template.</span></span> <span data-ttu-id="b4a84-131">Használhatja a PowerShell vagy az Azure CLI toocreate egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b4a84-131">You use either PowerShell or Azure CLI toocreate a resource group.</span></span> <span data-ttu-id="b4a84-132">Ezután telepítsen egy tárolási fiók toothat erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b4a84-132">Then, you deploy a storage account toothat resource group.</span></span>

* <span data-ttu-id="b4a84-133">PowerShell használja a következő parancsok hello sablon tartalmazó hello mappából hello:</span><span class="sxs-lookup"><span data-stu-id="b4a84-133">For PowerShell, use hello following commands from hello folder containing hello template:</span></span>

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* <span data-ttu-id="b4a84-134">Az Azure CLI helyi telepítés esetén parancsok hello sablon tartalmazó hello mappából a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="b4a84-134">For a local installation of Azure CLI, use hello following commands from hello folder containing hello template:</span></span>

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

<span data-ttu-id="b4a84-135">Központi telepítés befejezése után a storage-fiók létezik-e hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b4a84-135">When deployment finishes, your storage account exists in hello resource group.</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="b4a84-136">Sablon üzembe helyezése a Cloud Shellből</span><span class="sxs-lookup"><span data-stu-id="b4a84-136">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="b4a84-137">Használhat [felhő rendszerhéj](../cloud-shell/overview.md) toorun hello Azure parancssori felület parancsai a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4a84-137">You can use [Cloud Shell](../cloud-shell/overview.md) toorun hello Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="b4a84-138">Azonban Ön először be kell tölteni a sablon hello fájlmegosztás be a felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="b4a84-138">However, you must first load your template into hello file share for your Cloud Shell.</span></span> <span data-ttu-id="b4a84-139">Ha még nem használta a Cloud Shellt, a telepítésével kapcsolatban lásd [Az Azure Cloud Shell áttekintése](../cloud-shell/overview.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="b4a84-139">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="b4a84-140">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b4a84-140">Log in toohello [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="b4a84-141">Válassza ki a Cloud Shell-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b4a84-141">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="b4a84-142">hello minta nem `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="b4a84-142">hello name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Erőforráscsoport kiválasztása](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. <span data-ttu-id="b4a84-144">A felhő rendszerhéj hello storage-fiók kiválasztása</span><span class="sxs-lookup"><span data-stu-id="b4a84-144">Select hello storage account for your Cloud Shell.</span></span>

   ![Adattároló fiók kiválasztása](./media/resource-manager-create-first-template/select-storage.png)

4. <span data-ttu-id="b4a84-146">Válassza a **Files** (Fájlok) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b4a84-146">Select **Files**.</span></span>

   ![Fájlok kiválasztása](./media/resource-manager-create-first-template/select-files.png)

5. <span data-ttu-id="b4a84-148">Válassza ki a hello fájlmegosztás felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="b4a84-148">Select hello file share for Cloud Shell.</span></span> <span data-ttu-id="b4a84-149">hello minta nem `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="b4a84-149">hello name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Fájlmegosztás kiválasztása](./media/resource-manager-create-first-template/select-file-share.png)

6. <span data-ttu-id="b4a84-151">Válassza az **Add directory** (Könyvtár hozzáadása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b4a84-151">Select **Add directory**.</span></span>

   ![Könyvtár hozzáadása](./media/resource-manager-create-first-template/select-add-directory.png)

7. <span data-ttu-id="b4a84-153">Nevezze el a könyvtárat **templates** (sablonok) néven, és válassza az **Okay** (OK) gombot.</span><span class="sxs-lookup"><span data-stu-id="b4a84-153">Name it **templates**, and select **Okay**.</span></span>

   ![Könyvtár elnevezése](./media/resource-manager-create-first-template/name-templates.png)

8. <span data-ttu-id="b4a84-155">Jelölje ki az új könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="b4a84-155">Select your new directory.</span></span>

   ![Könyvtár kijelölése](./media/resource-manager-create-first-template/select-templates.png)

9. <span data-ttu-id="b4a84-157">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b4a84-157">Select **Upload**.</span></span>

   ![Feltöltés kiválasztása](./media/resource-manager-create-first-template/select-upload.png)

10. <span data-ttu-id="b4a84-159">Keresse meg és töltse fel a sablont.</span><span class="sxs-lookup"><span data-stu-id="b4a84-159">Find and upload your template.</span></span>

   ![Fájl feltöltése](./media/resource-manager-create-first-template/upload-files.png)

11. <span data-ttu-id="b4a84-161">Nyissa meg hello kérdés.</span><span class="sxs-lookup"><span data-stu-id="b4a84-161">Open hello prompt.</span></span>

   ![Cloud Shell megnyitása](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. <span data-ttu-id="b4a84-163">Adja meg a következő parancsok hello felhő rendszerhéj hello:</span><span class="sxs-lookup"><span data-stu-id="b4a84-163">Enter hello following commands in hello Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

<span data-ttu-id="b4a84-164">Központi telepítés befejezése után a storage-fiók létezik-e hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="b4a84-164">When deployment finishes, your storage account exists in hello resource group.</span></span>

## <a name="customize-hello-template"></a><span data-ttu-id="b4a84-165">Hello sablon testreszabása</span><span class="sxs-lookup"><span data-stu-id="b4a84-165">Customize hello template</span></span>

<span data-ttu-id="b4a84-166">hello sablon remekül működik, de nincs rugalmas.</span><span class="sxs-lookup"><span data-stu-id="b4a84-166">hello template works fine, but it is not flexible.</span></span> <span data-ttu-id="b4a84-167">A helyileg redundáns tárolás tooSouth USA középső RÉGIÓJA mindig telepíti.</span><span class="sxs-lookup"><span data-stu-id="b4a84-167">It always deploys a locally redundant storage tooSouth Central US.</span></span> <span data-ttu-id="b4a84-168">hello értéke mindig *tárolási* kivonatot követ.</span><span class="sxs-lookup"><span data-stu-id="b4a84-168">hello name is always *storage* followed by a hash value.</span></span> <span data-ttu-id="b4a84-169">hello sablonnal különböző helyzetek kezelésére, tooenable paraméterek toohello sablon hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="b4a84-169">tooenable using hello template for different scenarios, add parameters toohello template.</span></span>

<span data-ttu-id="b4a84-170">hello következő példa bemutatja hello paraméterek szakaszban két paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="b4a84-170">hello following example shows hello parameters section with two parameters.</span></span> <span data-ttu-id="b4a84-171">az első paraméter hello `storageSKU` lehetővé teszi a redundancia toospecify hello típusú.</span><span class="sxs-lookup"><span data-stu-id="b4a84-171">hello first parameter `storageSKU` enables you toospecify hello type of redundancy.</span></span> <span data-ttu-id="b4a84-172">Hello értékek, amelyek érvényesek a tárfiók toovalues átadhatók korlátozza.</span><span class="sxs-lookup"><span data-stu-id="b4a84-172">It limits hello values you can pass in toovalues that are valid for a storage account.</span></span> <span data-ttu-id="b4a84-173">Emellett egy alapértelmezett értéket is megad.</span><span class="sxs-lookup"><span data-stu-id="b4a84-173">It also specifies a default value.</span></span> <span data-ttu-id="b4a84-174">a második paraméter hello `storageNamePrefix` van beállítva tooallow legfeljebb 11 karakter.</span><span class="sxs-lookup"><span data-stu-id="b4a84-174">hello second parameter `storageNamePrefix` is set tooallow a maximum of 11 characters.</span></span> <span data-ttu-id="b4a84-175">A paraméter megad egy alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="b4a84-175">It specifies a default value.</span></span>

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "hello type of replication toouse for hello storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

<span data-ttu-id="b4a84-176">Hello a változók szakaszban nevű változó hozzáadása `storageName`.</span><span class="sxs-lookup"><span data-stu-id="b4a84-176">In hello variables section, add a variable named `storageName`.</span></span> <span data-ttu-id="b4a84-177">Hello előtag a hello paraméter és érték egy a hello egyesít [uniqueString](resource-group-template-functions-string.md#uniquestring) függvény.</span><span class="sxs-lookup"><span data-stu-id="b4a84-177">It combines hello prefix value from hello parameters and a hash value from hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span> <span data-ttu-id="b4a84-178">Hello használ [toLower](resource-group-template-functions-string.md#tolower) tooconvert összes karakter toolowercase működik.</span><span class="sxs-lookup"><span data-stu-id="b4a84-178">It uses hello [toLower](resource-group-template-functions-string.md#tolower) function tooconvert all characters toolowercase.</span></span>

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

<span data-ttu-id="b4a84-179">toouse ezeket az új értékeket a tárfiók módosítása hello erőforrás-definícióban:</span><span class="sxs-lookup"><span data-stu-id="b4a84-179">toouse these new values for your storage account, change hello resource definition:</span></span>

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

<span data-ttu-id="b4a84-180">Figyelje meg, hogy hello toohello változó hozzáadott most beállítva hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="b4a84-180">Notice that hello name of hello storage account is now set toohello variable that you added.</span></span> <span data-ttu-id="b4a84-181">hello termékváltozat hello paraméter értékének toohello van beállítva.</span><span class="sxs-lookup"><span data-stu-id="b4a84-181">hello SKU name is set toohello value of hello parameter.</span></span> <span data-ttu-id="b4a84-182">hello hely hello hello erőforráscsoport és ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="b4a84-182">hello location is set hello same location as hello resource group.</span></span>

<span data-ttu-id="b4a84-183">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4a84-183">Save your file.</span></span> 

<span data-ttu-id="b4a84-184">Ebben a cikkben hello lépések végrehajtását követően a sablon most néz ki:</span><span class="sxs-lookup"><span data-stu-id="b4a84-184">After completing hello steps in this article, your template now looks like:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "hello type of replication toouse for hello storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a><span data-ttu-id="b4a84-185">Sablon ismételt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="b4a84-185">Redeploy template</span></span>

<span data-ttu-id="b4a84-186">Telepítse újra a hello sablon, eltérő értékű.</span><span class="sxs-lookup"><span data-stu-id="b4a84-186">Redeploy hello template with different values.</span></span>

<span data-ttu-id="b4a84-187">PowerShell esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4a84-187">For PowerShell, use:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

<span data-ttu-id="b4a84-188">Azure CLI esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4a84-188">For Azure CLI, use:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

<span data-ttu-id="b4a84-189">A felhő rendszerhéj hello töltse fel a módosított sablon toohello fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="b4a84-189">For hello Cloud Shell, upload your changed template toohello file share.</span></span> <span data-ttu-id="b4a84-190">Hello meglévő fájl felülírásához.</span><span class="sxs-lookup"><span data-stu-id="b4a84-190">Overwrite hello existing file.</span></span> <span data-ttu-id="b4a84-191">Ezután használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b4a84-191">Then, use hello following command:</span></span>

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a><span data-ttu-id="b4a84-192">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b4a84-192">Clean up resources</span></span>

<span data-ttu-id="b4a84-193">Ha már nincs szükség, hello erőforráscsoport törlésével telepített hello erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="b4a84-193">When no longer needed, clean up hello resources you deployed by deleting hello resource group.</span></span>

<span data-ttu-id="b4a84-194">PowerShell esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4a84-194">For PowerShell, use:</span></span>

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

<span data-ttu-id="b4a84-195">Azure CLI esetén használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4a84-195">For Azure CLI, use:</span></span>

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a><span data-ttu-id="b4a84-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4a84-196">Next steps</span></span>
* <span data-ttu-id="b4a84-197">toolearn hello a sablonok struktúrájával kapcsolatos, kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b4a84-197">toolearn more about hello structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b4a84-198">toolearn egy tárfiókot, hello tulajdonságainak kapcsolatban lásd: [tárfiókok sablonra való hivatkozást](/azure/templates/microsoft.storage/storageaccounts).</span><span class="sxs-lookup"><span data-stu-id="b4a84-198">toolearn about hello properties for a storage account, see [storage accounts template reference](/azure/templates/microsoft.storage/storageaccounts).</span></span>
* <span data-ttu-id="b4a84-199">tooview teljes sablonok számos különböző típusú megoldások, lásd: hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="b4a84-199">tooview complete templates for many different types of solutions, see hello [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
