---
title: "a PowerShell és a sablon aaaDeploy erőforrások |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell toodeploy egy erőforrások tooAzure használja. a Resource Manager-sablon hello erőforrások vannak definiálva."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 41506811ba3c2ea5df6313db70978ade50f71161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a><span data-ttu-id="1e2da-104">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="1e2da-104">Deploy resources with Resource Manager templates and Azure PowerShell</span></span>

<span data-ttu-id="1e2da-105">Ez a témakör azt ismerteti, hogyan toouse Azure PowerShell, a Resource Manager sablonok toodeploy az erőforrások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1e2da-105">This topic explains how toouse Azure PowerShell with Resource Manager templates toodeploy your resources tooAzure.</span></span> <span data-ttu-id="1e2da-106">Ha nem ismeri a hello kapcsolatos alapfogalmakat üzembe helyezése és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-106">If you are not familiar with hello concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

<span data-ttu-id="1e2da-107">azok a helyi fájl a számítógépre telepít hello Resource Manager-sablon, vagy egy külső egy például a GitHub-tárházban található fájl.</span><span class="sxs-lookup"><span data-stu-id="1e2da-107">hello Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="1e2da-108">Ebben a cikkben telepít hello sablon érhető el hello [mintasablon](#sample-template) szakasz, vagy mint [tárolási fiók sablon a Githubon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="1e2da-108">hello template you deploy in this article is available in hello [Sample template](#sample-template) section, or as [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a><span data-ttu-id="1e2da-109">A helyi gépről sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="1e2da-109">Deploy a template from your local machine</span></span>

<span data-ttu-id="1e2da-110">Erőforrások tooAzure telepítésekor meg:</span><span class="sxs-lookup"><span data-stu-id="1e2da-110">When deploying resources tooAzure, you:</span></span>

1. <span data-ttu-id="1e2da-111">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="1e2da-111">Log in tooyour Azure account</span></span>
2. <span data-ttu-id="1e2da-112">Hozzon létre egy erőforráscsoportot, amely hello telepített erőforrások hello tárolóként szolgál.</span><span class="sxs-lookup"><span data-stu-id="1e2da-112">Create a resource group that serves as hello container for hello deployed resources.</span></span> <span data-ttu-id="1e2da-113">hello erőforráscsoport nevét hello tartalmazhatnak alfanumerikus karaktereket, pontokat, aláhúzásjeleket, kötőjeleket és zárójeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1e2da-113">hello name of hello resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="1e2da-114">Másolatot too90 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="1e2da-114">It can be up too90 characters.</span></span> <span data-ttu-id="1e2da-115">Nem végződhet ponttal.</span><span class="sxs-lookup"><span data-stu-id="1e2da-115">It cannot end in a period.</span></span>
3. <span data-ttu-id="1e2da-116">Toohello erőforrás csoport hello sablont, amely meghatározza a hello erőforrások toocreate telepítése</span><span class="sxs-lookup"><span data-stu-id="1e2da-116">Deploy toohello resource group hello template that defines hello resources toocreate</span></span>

<span data-ttu-id="1e2da-117">A sablon tartalmazhat, amelyek lehetővé teszik toocustomize hello telepítési paramétereit.</span><span class="sxs-lookup"><span data-stu-id="1e2da-117">A template can include parameters that enable you toocustomize hello deployment.</span></span> <span data-ttu-id="1e2da-118">Biztosíthatja például is lefednek értékeket (például a fejlesztői, tesztelési és éles) egy adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="1e2da-118">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="1e2da-119">hello mintasablon hello tárfiók SKU paraméter határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1e2da-119">hello sample template defines a parameter for hello storage account SKU.</span></span>

<span data-ttu-id="1e2da-120">a következő példa hello hoz létre egy erőforráscsoportot, és egy sablon, a helyi számítógépen telepíti:</span><span class="sxs-lookup"><span data-stu-id="1e2da-120">hello following example creates a resource group, and deploys a template from your local machine:</span></span>

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="1e2da-121">hello központi telepítés is igénybe vehet néhány percet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="1e2da-121">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="1e2da-122">A Befejezés után megjelenik egy üzenet, amely tartalmazza az hello eredmény:</span><span class="sxs-lookup"><span data-stu-id="1e2da-122">When it finishes, you see a message that includes hello result:</span></span>

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a><span data-ttu-id="1e2da-123">Külső forrásból sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="1e2da-123">Deploy a template from an external source</span></span>

<span data-ttu-id="1e2da-124">Toostore célszerű helyett Resource Manager-sablonok a helyi számítógépen, a külső helyre.</span><span class="sxs-lookup"><span data-stu-id="1e2da-124">Instead of storing Resource Manager templates on your local machine, you may prefer toostore them in an external location.</span></span> <span data-ttu-id="1e2da-125">A verziókövetési tárházat (például a Githubon) sablonok tárolhat.</span><span class="sxs-lookup"><span data-stu-id="1e2da-125">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="1e2da-126">Vagy tárolhatja őket egy Azure storage-fiók megosztott eléréséhez a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="1e2da-126">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="1e2da-127">egy külső sablon toodeploy hello használata **TemplateUri** paraméter.</span><span class="sxs-lookup"><span data-stu-id="1e2da-127">toodeploy an external template, use hello **TemplateUri** parameter.</span></span> <span data-ttu-id="1e2da-128">Hello URI hello példa toodeploy hello minta sablont a Githubból a használata.</span><span class="sxs-lookup"><span data-stu-id="1e2da-128">Use hello URI in hello example toodeploy hello sample template from GitHub.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

<span data-ttu-id="1e2da-129">hello előző példa kell rendelkeznie a nyilvánosan elérhető URI hello sablon, amely a legtöbb környezetben működik, mivel a sablon nem érzékeny adatot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="1e2da-129">hello preceding example requires a publicly accessible URI for hello template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="1e2da-130">Ha toospecify bizalmas adatok (például egy rendszergazdai jelszó) van szüksége, adja át ezt az értéket egy biztonságos paraméterben.</span><span class="sxs-lookup"><span data-stu-id="1e2da-130">If you need toospecify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="1e2da-131">Azonban ha nem szeretné, hogy a sablon toobe nyilvánosan elérhető, megvédheti azokat a személyes tárolót tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="1e2da-131">However, if you do not want your template toobe publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="1e2da-132">A sablont, amely közös hozzáférésű jogosultságkód (SAS) jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-132">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>

## <a name="parameter-files"></a><span data-ttu-id="1e2da-133">A paraméter fájlok</span><span class="sxs-lookup"><span data-stu-id="1e2da-133">Parameter files</span></span>

<span data-ttu-id="1e2da-134">Ahelyett, hogy a parancsfájl beágyazott értékeiként paraméterek átadása, azt tapasztalhatja, könnyebben toouse hello paraméterértékek tartalmazó JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="1e2da-134">Rather than passing parameters as inline values in your script, you may find it easier toouse a JSON file that contains hello parameter values.</span></span> <span data-ttu-id="1e2da-135">hello paraméterfájl kell hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="1e2da-135">hello parameter file must be in hello following format:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

<span data-ttu-id="1e2da-136">Figyelje meg, hogy hello paraméterek szakaszban tartalmazza-e a paraméter neve, amely megfelel a sablonban (storageAccountType) meghatározott hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="1e2da-136">Notice that hello parameters section includes a parameter name that matches hello parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="1e2da-137">hello paraméterfájl hello paraméter értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1e2da-137">hello parameter file contains a value for hello parameter.</span></span> <span data-ttu-id="1e2da-138">Ezt az értéket automatikusan átadódik toohello sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="1e2da-138">This value is automatically passed toohello template during deployment.</span></span> <span data-ttu-id="1e2da-139">Hozzon létre különböző telepítési forgatókönyvek esetén több paraméter fájlt, és akkor továbbítja a hello megfelelő paraméter fájlban.</span><span class="sxs-lookup"><span data-stu-id="1e2da-139">You can create multiple parameter files for different deployment scenarios, and then pass in hello appropriate parameter file.</span></span> 

<span data-ttu-id="1e2da-140">Példa megelőző hello másolja, majd mentse a fájlt `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="1e2da-140">Copy hello preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="1e2da-141">a helyi paraméterfájl toopass hello használata **TemplateParameterFile** paraméter:</span><span class="sxs-lookup"><span data-stu-id="1e2da-141">toopass a local parameter file, use hello **TemplateParameterFile** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

<span data-ttu-id="1e2da-142">toopass külső paraméterfájl használata hello **TemplateParameterUri** paraméter:</span><span class="sxs-lookup"><span data-stu-id="1e2da-142">toopass an external parameter file, use hello **TemplateParameterUri** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

<span data-ttu-id="1e2da-143">Beágyazott paraméterek használhatók, és egy helyi paraméter fájlt hello ugyanazt a telepítési műveletek.</span><span class="sxs-lookup"><span data-stu-id="1e2da-143">You can use inline parameters and a local parameter file in hello same deployment operation.</span></span> <span data-ttu-id="1e2da-144">Például adja meg az egyes értékeket hello helyi paraméterfájl és egyéb értékek beágyazott hozzáadása a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="1e2da-144">For example, you can specify some values in hello local parameter file and add other values inline during deployment.</span></span> <span data-ttu-id="1e2da-145">Ha értékeket ad meg az egyik paraméter a helyi paraméterfájl hello és a beágyazott, hello beágyazott elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="1e2da-145">If you provide values for a parameter in both hello local parameter file and inline, hello inline value takes precedence.</span></span>

<span data-ttu-id="1e2da-146">Azonban egy külső paraméterfájl használata esetén nem adható át más értékek vagy szövegközi vagy egy helyi fájlból.</span><span class="sxs-lookup"><span data-stu-id="1e2da-146">However, when you use an external parameter file, you cannot pass other values either inline or from a local file.</span></span> <span data-ttu-id="1e2da-147">Ha megadja a paraméterfájl hello **TemplateParameterUri** paraméter, minden beágyazott paraméterek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="1e2da-147">When you specify a parameter file in hello **TemplateParameterUri** parameter, all inline parameters are ignored.</span></span> <span data-ttu-id="1e2da-148">Adja meg az összes paraméter hello külső fájlban.</span><span class="sxs-lookup"><span data-stu-id="1e2da-148">Provide all parameter values in hello external file.</span></span> <span data-ttu-id="1e2da-149">Ha a sablont, amely nem adhat meg hello paraméterfájl kényes értéket tartalmaz, adja hozzá az adott érték tooa kulcstároló, vagy dinamikusan adjon meg az összes paraméter értékek beágyazott.</span><span class="sxs-lookup"><span data-stu-id="1e2da-149">If your template includes a sensitive value that you cannot include in hello parameter file, either add that value tooa key vault, or dynamically provide all parameter values inline.</span></span>

<span data-ttu-id="1e2da-150">Ha a sablon azonos nevet hello paraméterek hello PowerShell-parancsot a rendelkezésre álló hello paraméterrel tartalmaz, PowerShell hello paraméter a sablon alapján megadja a hello utótag **FromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="1e2da-150">If your template includes a parameter with hello same name as one of hello parameters in hello PowerShell command, PowerShell presents hello parameter from your template with hello postfix **FromTemplate**.</span></span> <span data-ttu-id="1e2da-151">Például nevű paraméter **ResourceGroupName** a sablon ütközéseket hello **ResourceGroupName** hello paraméterének [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1e2da-151">For example, a parameter named **ResourceGroupName** in your template conflicts with hello **ResourceGroupName** parameter in hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="1e2da-152">Felszólító tooprovide értéket **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="1e2da-152">You are prompted tooprovide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="1e2da-153">Ez zavart ne általában a nem azonos nevet telepítési műveleteihez használt paraméterekként hello paramétereket.</span><span class="sxs-lookup"><span data-stu-id="1e2da-153">In general, you should avoid this confusion by not naming parameters with hello same name as parameters used for deployment operations.</span></span>

## <a name="test-a-template-deployment"></a><span data-ttu-id="1e2da-154">A sablon üzemelő példány tesztelése</span><span class="sxs-lookup"><span data-stu-id="1e2da-154">Test a template deployment</span></span>

<span data-ttu-id="1e2da-155">tootest erőforrásokat, tényleges telepítése nélkül a sablonnal és paraméterfájlokkal értékeket használja [teszt-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span><span class="sxs-lookup"><span data-stu-id="1e2da-155">tootest your template and parameter values without actually deploying any resources, use [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span></span> 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="1e2da-156">Ha nincsenek hibák, a hello parancs befejezi a válaszra.</span><span class="sxs-lookup"><span data-stu-id="1e2da-156">If no errors are detected, hello command finishes without a response.</span></span> <span data-ttu-id="1e2da-157">Ha a rendszer hibát észlel, hello parancs hibaüzenetet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1e2da-157">If an error is detected, hello command returns an error message.</span></span> <span data-ttu-id="1e2da-158">Például kísérlet toopass hello tárfiók SKU, helytelen értéket adja vissza hello hiba a következő:</span><span class="sxs-lookup"><span data-stu-id="1e2da-158">For example, attempting toopass an incorrect value for hello storage account SKU, returns hello following error:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'hello provided value 'badSku' for hello template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. hello parameter value is not part of hello allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

<span data-ttu-id="1e2da-159">Ha a sablon szintaktikai hibát tartalmaz, a hello parancs nem tudta elemezni a hello sablon jelző hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1e2da-159">If your template has a syntax error, hello command returns an error indicating it could not parse hello template.</span></span> <span data-ttu-id="1e2da-160">hello az üzenet azt jelzi, hello számát és a feldolgozási hiba hello pozícióját.</span><span class="sxs-lookup"><span data-stu-id="1e2da-160">hello message indicates hello line number and position of hello parsing error.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="1e2da-161">toouse teljes módban használja hello `Mode` paraméter:</span><span class="sxs-lookup"><span data-stu-id="1e2da-161">toouse complete mode, use hello `Mode` parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a><span data-ttu-id="1e2da-162">Minta sablon</span><span class="sxs-lookup"><span data-stu-id="1e2da-162">Sample template</span></span>

<span data-ttu-id="1e2da-163">hello következő sablon használható hello példák ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="1e2da-163">hello following template is used for hello examples in this topic.</span></span> <span data-ttu-id="1e2da-164">Másolja ki és mentse azt egy storage.json nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="1e2da-164">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="1e2da-165">toounderstand hogyan toocreate ezen sablon esetén lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-165">toounderstand how toocreate this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="1e2da-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e2da-166">Next steps</span></span>
* <span data-ttu-id="1e2da-167">a cikkben szereplő példák hello erőforrások alapértelmezett előfizetése tooa erőforráscsoport telepítése.</span><span class="sxs-lookup"><span data-stu-id="1e2da-167">hello examples in this article deploy resources tooa resource group in your default subscription.</span></span> <span data-ttu-id="1e2da-168">toouse egy másik előfizetést, lásd: [több Azure-előfizetések kezeléséhez](/powershell/azure/manage-subscriptions-azureps).</span><span class="sxs-lookup"><span data-stu-id="1e2da-168">toouse a different subscription, see [Manage multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps).</span></span>
* <span data-ttu-id="1e2da-169">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [Resource Manager sablon üzembe helyezési parancsfájl](resource-manager-samples-powershell-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-169">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-powershell-deploy.md).</span></span>
* <span data-ttu-id="1e2da-170">Hogyan toodefine paramétereket a sablonban: toounderstand [hello struktúra és az Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-170">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1e2da-171">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-171">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="1e2da-172">A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-172">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="1e2da-173">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1e2da-173">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

