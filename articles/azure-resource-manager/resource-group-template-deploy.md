---
title: "Erőforrások az PowerShell és a sablon telepítése |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell használatával telepítse a erőforrások az Azure. Az erőforrások egy Resource Manager-sablonban vannak meghatározva."
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
ms.openlocfilehash: 5f395abf8ebdfbac18fd17d8183b392673e280ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a><span data-ttu-id="8885e-104">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="8885e-104">Deploy resources with Resource Manager templates and Azure PowerShell</span></span>

<span data-ttu-id="8885e-105">Ez a témakör ismerteti az Azure PowerShell használata a Resource Manager-sablonok az erőforrások telepítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="8885e-105">This topic explains how to use Azure PowerShell with Resource Manager templates to deploy your resources to Azure.</span></span> <span data-ttu-id="8885e-106">Ha nem ismeri a telepítésével kapcsolatos alapfogalmakat és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-106">If you are not familiar with the concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

<span data-ttu-id="8885e-107">A Resource Manager-sablon, azok a helyi fájl a számítógépre telepít, vagy egy külső egy például a GitHub-tárházban található fájl.</span><span class="sxs-lookup"><span data-stu-id="8885e-107">The Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="8885e-108">Ez a cikk központi telepítését a sablon érhető el a [mintasablon](#sample-template) szakasz, vagy mint [tárolási fiók sablon a Githubon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="8885e-108">The template you deploy in this article is available in the [Sample template](#sample-template) section, or as [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a><span data-ttu-id="8885e-109">A helyi gépről sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="8885e-109">Deploy a template from your local machine</span></span>

<span data-ttu-id="8885e-110">Ha erőforrásokat üzembe helyezi az Azure-ba, hogy:</span><span class="sxs-lookup"><span data-stu-id="8885e-110">When deploying resources to Azure, you:</span></span>

1. <span data-ttu-id="8885e-111">Jelentkezzen be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="8885e-111">Log in to your Azure account</span></span>
2. <span data-ttu-id="8885e-112">Hozzon létre egy erőforráscsoportot, amely a telepített erőforrások tárolójaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="8885e-112">Create a resource group that serves as the container for the deployed resources.</span></span> <span data-ttu-id="8885e-113">Az erőforráscsoport neve csak tartalmazhatnak alfanumerikus karaktereket, pontokat, aláhúzásjeleket, kötőjeleket és zárójeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="8885e-113">The name of the resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="8885e-114">Legfeljebb 90 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="8885e-114">It can be up to 90 characters.</span></span> <span data-ttu-id="8885e-115">Nem végződhet ponttal.</span><span class="sxs-lookup"><span data-stu-id="8885e-115">It cannot end in a period.</span></span>
3. <span data-ttu-id="8885e-116">Telepítse az erőforráscsoport a sablon, amely meghatározza az erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8885e-116">Deploy to the resource group the template that defines the resources to create</span></span>

<span data-ttu-id="8885e-117">A sablon tartalmazhat, amelyek segítségével testre szabhatja a központi telepítési paramétereit.</span><span class="sxs-lookup"><span data-stu-id="8885e-117">A template can include parameters that enable you to customize the deployment.</span></span> <span data-ttu-id="8885e-118">Biztosíthatja például is lefednek értékeket (például a fejlesztői, tesztelési és éles) egy adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="8885e-118">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="8885e-119">A minta sablon meghatározza a tárfiók SKU paraméter.</span><span class="sxs-lookup"><span data-stu-id="8885e-119">The sample template defines a parameter for the storage account SKU.</span></span>

<span data-ttu-id="8885e-120">Az alábbi példa létrehoz egy erőforráscsoport, és egy sablon, a helyi számítógépen telepíti:</span><span class="sxs-lookup"><span data-stu-id="8885e-120">The following example creates a resource group, and deploys a template from your local machine:</span></span>

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="8885e-121">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="8885e-121">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="8885e-122">A Befejezés után megjelenik egy üzenet, amely tartalmazza az eredmény:</span><span class="sxs-lookup"><span data-stu-id="8885e-122">When it finishes, you see a message that includes the result:</span></span>

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a><span data-ttu-id="8885e-123">Külső forrásból sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="8885e-123">Deploy a template from an external source</span></span>

<span data-ttu-id="8885e-124">Helyett Resource Manager-sablonok a helyi gépén, célszerű lehet külső helyen tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="8885e-124">Instead of storing Resource Manager templates on your local machine, you may prefer to store them in an external location.</span></span> <span data-ttu-id="8885e-125">A verziókövetési tárházat (például a Githubon) sablonok tárolhat.</span><span class="sxs-lookup"><span data-stu-id="8885e-125">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="8885e-126">Vagy tárolhatja őket egy Azure storage-fiók megosztott eléréséhez a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="8885e-126">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="8885e-127">Egy külső sablon történő üzembe helyezéséhez használjon a **TemplateUri** paraméter.</span><span class="sxs-lookup"><span data-stu-id="8885e-127">To deploy an external template, use the **TemplateUri** parameter.</span></span> <span data-ttu-id="8885e-128">A példában az URI segítségével telepítheti a minta-sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="8885e-128">Use the URI in the example to deploy the sample template from GitHub.</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

<span data-ttu-id="8885e-129">Az előző példában a sablont, amely a legtöbb környezetben működik, mivel a sablon nem tartalmaznia kell a bizalmas adatok nyilvánosan elérhető URI igényel.</span><span class="sxs-lookup"><span data-stu-id="8885e-129">The preceding example requires a publicly accessible URI for the template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="8885e-130">Meg kell adnia a bizalmas adatok (például egy rendszergazdai jelszó), ha egy biztonságos paraméterben adja át ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8885e-130">If you need to specify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="8885e-131">Azonban ha nem szeretné, hogy a sablon a nyilvánosan hozzáférhető, megvédheti azokat a személyes tárolót tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="8885e-131">However, if you do not want your template to be publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="8885e-132">A sablont, amely közös hozzáférésű jogosultságkód (SAS) jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-132">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>

## <a name="parameter-files"></a><span data-ttu-id="8885e-133">A paraméter fájlok</span><span class="sxs-lookup"><span data-stu-id="8885e-133">Parameter files</span></span>

<span data-ttu-id="8885e-134">Ahelyett, hogy a parancsfájl beágyazott értékeiként paraméterek átadása, előfordulhat, hogy ez egyszerűbbé teszi a paraméterek értékeit tartalmazó JSON-fájl használatára.</span><span class="sxs-lookup"><span data-stu-id="8885e-134">Rather than passing parameters as inline values in your script, you may find it easier to use a JSON file that contains the parameter values.</span></span> <span data-ttu-id="8885e-135">A paraméterfájl a következő formátumúnak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="8885e-135">The parameter file must be in the following format:</span></span>

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

<span data-ttu-id="8885e-136">Figyelje meg, hogy a Paraméterek szakaszban tartalmazza-e a paraméter neve, amely megfelel a sablonban (storageAccountType) meghatározott paraméter.</span><span class="sxs-lookup"><span data-stu-id="8885e-136">Notice that the parameters section includes a parameter name that matches the parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="8885e-137">A paraméterfájl a paraméter értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8885e-137">The parameter file contains a value for the parameter.</span></span> <span data-ttu-id="8885e-138">Ezt az értéket automatikusan kerülnek a sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="8885e-138">This value is automatically passed to the template during deployment.</span></span> <span data-ttu-id="8885e-139">Hozzon létre különböző telepítési forgatókönyvek esetén több paraméter fájlt, és akkor továbbítja a megfelelő paraméter fájlban.</span><span class="sxs-lookup"><span data-stu-id="8885e-139">You can create multiple parameter files for different deployment scenarios, and then pass in the appropriate parameter file.</span></span> 

<span data-ttu-id="8885e-140">Másolja át az előző példában, és mentse a fájlt `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="8885e-140">Copy the preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="8885e-141">A helyi paraméterfájl továbbítani, használja a **TemplateParameterFile** paraméter:</span><span class="sxs-lookup"><span data-stu-id="8885e-141">To pass a local parameter file, use the **TemplateParameterFile** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

<span data-ttu-id="8885e-142">Egy külső paraméterfájl továbbítani, használja a **TemplateParameterUri** paraméter:</span><span class="sxs-lookup"><span data-stu-id="8885e-142">To pass an external parameter file, use the **TemplateParameterUri** parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

<span data-ttu-id="8885e-143">Beágyazott paraméterek és a helyi paraméterfájl azonos központi telepítési művelet használható.</span><span class="sxs-lookup"><span data-stu-id="8885e-143">You can use inline parameters and a local parameter file in the same deployment operation.</span></span> <span data-ttu-id="8885e-144">Például a helyi paraméterfájl adja meg az egyes értékeket, és adja hozzá a más értékek beágyazott üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="8885e-144">For example, you can specify some values in the local parameter file and add other values inline during deployment.</span></span> <span data-ttu-id="8885e-145">Ha az egyik paraméter a helyi paraméterfájl és a beágyazott értékeket ad meg, a beágyazott elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="8885e-145">If you provide values for a parameter in both the local parameter file and inline, the inline value takes precedence.</span></span>

<span data-ttu-id="8885e-146">Azonban egy külső paraméterfájl használata esetén nem adható át más értékek vagy szövegközi vagy egy helyi fájlból.</span><span class="sxs-lookup"><span data-stu-id="8885e-146">However, when you use an external parameter file, you cannot pass other values either inline or from a local file.</span></span> <span data-ttu-id="8885e-147">A paraméterfájl a megadása a **TemplateParameterUri** paraméter, minden beágyazott paraméterek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="8885e-147">When you specify a parameter file in the **TemplateParameterUri** parameter, all inline parameters are ignored.</span></span> <span data-ttu-id="8885e-148">Adja meg az összes paraméter a külső fájlban.</span><span class="sxs-lookup"><span data-stu-id="8885e-148">Provide all parameter values in the external file.</span></span> <span data-ttu-id="8885e-149">Ha a sablont, amely nem adhat meg a paraméter fájl-és nagybetűket értéket tartalmaz, adja hozzá ezt az értéket a kulcstároló, vagy dinamikusan adjon meg az összes paraméter értékek beágyazott.</span><span class="sxs-lookup"><span data-stu-id="8885e-149">If your template includes a sensitive value that you cannot include in the parameter file, either add that value to a key vault, or dynamically provide all parameter values inline.</span></span>

<span data-ttu-id="8885e-150">Ha a sablon szerepel a PowerShell-parancs olyan paraméterre, amelynek a neve megegyezik a paraméterek egyikét, PowerShell eltéréseit a paraméter a sablonból a utótag **FromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="8885e-150">If your template includes a parameter with the same name as one of the parameters in the PowerShell command, PowerShell presents the parameter from your template with the postfix **FromTemplate**.</span></span> <span data-ttu-id="8885e-151">Például nevű paraméter **ResourceGroupName** a sablon ütközik a **ResourceGroupName** paramétere a [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="8885e-151">For example, a parameter named **ResourceGroupName** in your template conflicts with the **ResourceGroupName** parameter in the [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="8885e-152">Adjon meg egy értéket a rendszer kéri **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="8885e-152">You are prompted to provide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="8885e-153">Ez zavart ne általában a nem paraméterek telepítési műveleteihez használt paraméterek megegyező névvel.</span><span class="sxs-lookup"><span data-stu-id="8885e-153">In general, you should avoid this confusion by not naming parameters with the same name as parameters used for deployment operations.</span></span>

## <a name="test-a-template-deployment"></a><span data-ttu-id="8885e-154">A sablon üzemelő példány tesztelése</span><span class="sxs-lookup"><span data-stu-id="8885e-154">Test a template deployment</span></span>

<span data-ttu-id="8885e-155">Minden olyan erőforrásnál tényleges telepítése nélkül a sablonnal és paraméterfájlokkal értékek teszteléséhez [teszt-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span><span class="sxs-lookup"><span data-stu-id="8885e-155">To test your template and parameter values without actually deploying any resources, use [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment).</span></span> 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

<span data-ttu-id="8885e-156">Ha nincsenek hibák, a parancs a válaszra befejeződik.</span><span class="sxs-lookup"><span data-stu-id="8885e-156">If no errors are detected, the command finishes without a response.</span></span> <span data-ttu-id="8885e-157">Ha a rendszer hibát észlel, a parancs hibaüzenetet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="8885e-157">If an error is detected, the command returns an error message.</span></span> <span data-ttu-id="8885e-158">Például a tárfiók SKU, helytelen értéket átadni próbált a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="8885e-158">For example, attempting to pass an incorrect value for the storage account SKU, returns the following error:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

<span data-ttu-id="8885e-159">Ha a sablon szintaktikai hibát tartalmaz, a parancs nem tudta elemezni a sablon jelző hiba adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8885e-159">If your template has a syntax error, the command returns an error indicating it could not parse the template.</span></span> <span data-ttu-id="8885e-160">Az üzenet azt jelzi, a sor számának megjelenítése és elhelyezése az elemzési hiba.</span><span class="sxs-lookup"><span data-stu-id="8885e-160">The message indicates the line number and position of the parsing error.</span></span>

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="8885e-161">Teljes módot használja, használja a `Mode` paraméter:</span><span class="sxs-lookup"><span data-stu-id="8885e-161">To use complete mode, use the `Mode` parameter:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a><span data-ttu-id="8885e-162">Minta sablon</span><span class="sxs-lookup"><span data-stu-id="8885e-162">Sample template</span></span>

<span data-ttu-id="8885e-163">Ebben a témakörben szereplő példák a következő sablon használható.</span><span class="sxs-lookup"><span data-stu-id="8885e-163">The following template is used for the examples in this topic.</span></span> <span data-ttu-id="8885e-164">Másolja ki és mentse azt egy storage.json nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="8885e-164">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="8885e-165">Ez a sablon létrehozása ismertetése: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-165">To understand how to create this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="8885e-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8885e-166">Next steps</span></span>
* <span data-ttu-id="8885e-167">Ebben a cikkben szereplő példák erőforrások telepítése az alapértelmezett előfizetésében az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="8885e-167">The examples in this article deploy resources to a resource group in your default subscription.</span></span> <span data-ttu-id="8885e-168">Használjon másik előfizetést, lásd: [több Azure-előfizetések kezeléséhez](/powershell/azure/manage-subscriptions-azureps).</span><span class="sxs-lookup"><span data-stu-id="8885e-168">To use a different subscription, see [Manage multiple Azure subscriptions](/powershell/azure/manage-subscriptions-azureps).</span></span>
* <span data-ttu-id="8885e-169">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [Resource Manager sablon üzembe helyezési parancsfájl](resource-manager-samples-powershell-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-169">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-powershell-deploy.md).</span></span>
* <span data-ttu-id="8885e-170">Szeretné megtudni, hogyan adhat meg a paramétereket a sablonban, lásd: [megérteni a felépítését és Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-170">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="8885e-171">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-171">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="8885e-172">A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="8885e-172">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
* <span data-ttu-id="8885e-173">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="8885e-173">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

