---
title: "Erőforrások az Azure CLI és a sablon telepítése |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület használatával egy erőforrások telepítése az Azure-bA. Az erőforrások egy Resource Manager-sablonban vannak meghatározva."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 4f1d5f4cc48470f8906edb28628006dd1996bd3a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a><span data-ttu-id="80c53-104">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure parancssori felületével</span><span class="sxs-lookup"><span data-stu-id="80c53-104">Deploy resources with Resource Manager templates and Azure CLI</span></span>

<span data-ttu-id="80c53-105">Ez a témakör azt ismerteti, hogy az erőforrások telepítése Azure Resource Manager-sablonok Azure CLI 2.0 használata.</span><span class="sxs-lookup"><span data-stu-id="80c53-105">This topic explains how to use Azure CLI 2.0 with Resource Manager templates to deploy your resources to Azure.</span></span> <span data-ttu-id="80c53-106">Ha nem ismeri a telepítésével kapcsolatos alapfogalmakat és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-106">If you are not familiar with the concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>  

<span data-ttu-id="80c53-107">A Resource Manager-sablon, azok a helyi fájl a számítógépre telepít, vagy egy külső egy például a GitHub-tárházban található fájl.</span><span class="sxs-lookup"><span data-stu-id="80c53-107">The Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="80c53-108">Ez a cikk központi telepítését a sablon érhető el a [mintasablon](#sample-template) szakasz, vagy a regisztrációja, mivel egy [tárolási fiók sablon a Githubon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="80c53-108">The template you deploy in this article is available in the [Sample template](#sample-template) section, or as a [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

<span data-ttu-id="80c53-109">Ha nincs telepítve az Azure parancssori felület, használhatja a [felhő rendszerhéj](#deploy-template-from-cloud-shell).</span><span class="sxs-lookup"><span data-stu-id="80c53-109">If you do not have Azure CLI installed, you can use the [Cloud Shell](#deploy-template-from-cloud-shell).</span></span>

## <a name="deploy-local-template"></a><span data-ttu-id="80c53-110">Helyi sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="80c53-110">Deploy local template</span></span>

<span data-ttu-id="80c53-111">Ha erőforrásokat üzembe helyezi az Azure-ba, hogy:</span><span class="sxs-lookup"><span data-stu-id="80c53-111">When deploying resources to Azure, you:</span></span>

1. <span data-ttu-id="80c53-112">Jelentkezzen be az Azure-fiókjával</span><span class="sxs-lookup"><span data-stu-id="80c53-112">Log in to your Azure account</span></span>
2. <span data-ttu-id="80c53-113">Hozzon létre egy erőforráscsoportot, amely a telepített erőforrások tárolójaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="80c53-113">Create a resource group that serves as the container for the deployed resources.</span></span> <span data-ttu-id="80c53-114">Az erőforráscsoport neve csak tartalmazhatnak alfanumerikus karaktereket, pontokat, aláhúzásjeleket, kötőjeleket és zárójeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="80c53-114">The name of the resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="80c53-115">Legfeljebb 90 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="80c53-115">It can be up to 90 characters.</span></span> <span data-ttu-id="80c53-116">Nem végződhet ponttal.</span><span class="sxs-lookup"><span data-stu-id="80c53-116">It cannot end in a period.</span></span>
3. <span data-ttu-id="80c53-117">Telepítse az erőforráscsoport a sablon, amely meghatározza az erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="80c53-117">Deploy to the resource group the template that defines the resources to create</span></span>

<span data-ttu-id="80c53-118">A sablon tartalmazhat, amelyek segítségével testre szabhatja a központi telepítési paramétereit.</span><span class="sxs-lookup"><span data-stu-id="80c53-118">A template can include parameters that enable you to customize the deployment.</span></span> <span data-ttu-id="80c53-119">Biztosíthatja például is lefednek értékeket (például a fejlesztői, tesztelési és éles) egy adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="80c53-119">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="80c53-120">A minta sablon meghatározza a tárfiók SKU paraméter.</span><span class="sxs-lookup"><span data-stu-id="80c53-120">The sample template defines a parameter for the storage account SKU.</span></span> 

<span data-ttu-id="80c53-121">Az alábbi példa létrehoz egy erőforráscsoport, és egy sablon, a helyi számítógépen telepíti:</span><span class="sxs-lookup"><span data-stu-id="80c53-121">The following example creates a resource group, and deploys a template from your local machine:</span></span>

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="80c53-122">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="80c53-122">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="80c53-123">A Befejezés után megjelenik egy üzenet, amely tartalmazza az eredmény:</span><span class="sxs-lookup"><span data-stu-id="80c53-123">When it finishes, you see a message that includes the result:</span></span>

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a><span data-ttu-id="80c53-124">Külső sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="80c53-124">Deploy external template</span></span>

<span data-ttu-id="80c53-125">Helyett Resource Manager-sablonok a helyi gépén, célszerű lehet külső helyen tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="80c53-125">Instead of storing Resource Manager templates on your local machine, you may prefer to store them in an external location.</span></span> <span data-ttu-id="80c53-126">A verziókövetési tárházat (például a Githubon) sablonok tárolhat.</span><span class="sxs-lookup"><span data-stu-id="80c53-126">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="80c53-127">Vagy tárolhatja őket egy Azure storage-fiók megosztott eléréséhez a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="80c53-127">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="80c53-128">Egy külső sablon történő üzembe helyezéséhez használjon a **sablon-uri** paraméter.</span><span class="sxs-lookup"><span data-stu-id="80c53-128">To deploy an external template, use the **template-uri** parameter.</span></span> <span data-ttu-id="80c53-129">A példában az URI segítségével telepítheti a minta-sablont a Githubból.</span><span class="sxs-lookup"><span data-stu-id="80c53-129">Use the URI in the example to deploy the sample template from GitHub.</span></span>
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="80c53-130">Az előző példában a sablont, amely a legtöbb környezetben működik, mivel a sablon nem tartalmaznia kell a bizalmas adatok nyilvánosan elérhető URI igényel.</span><span class="sxs-lookup"><span data-stu-id="80c53-130">The preceding example requires a publicly accessible URI for the template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="80c53-131">Meg kell adnia a bizalmas adatok (például egy rendszergazdai jelszó), ha egy biztonságos paraméterben adja át ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="80c53-131">If you need to specify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="80c53-132">Azonban ha nem szeretné, hogy a sablon a nyilvánosan hozzáférhető, megvédheti azokat a személyes tárolót tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="80c53-132">However, if you do not want your template to be publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="80c53-133">A sablont, amely közös hozzáférésű jogosultságkód (SAS) jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-133">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="80c53-134">Sablon üzembe helyezése a Cloud Shellből</span><span class="sxs-lookup"><span data-stu-id="80c53-134">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="80c53-135">Az Azure CLI-parancsokat a [Cloud Shell](../cloud-shell/overview.md) használatával is futtathatja a sablon üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="80c53-135">You can use [Cloud Shell](../cloud-shell/overview.md) to run the Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="80c53-136">Ehhez azonban először be kell töltenie a sablont a Cloud Shell fájlmegosztásába.</span><span class="sxs-lookup"><span data-stu-id="80c53-136">However, you must first load your template into the file share for your Cloud Shell.</span></span> <span data-ttu-id="80c53-137">Ha még nem használta a Cloud Shellt, a telepítésével kapcsolatban lásd [Az Azure Cloud Shell áttekintése](../cloud-shell/overview.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="80c53-137">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="80c53-138">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="80c53-138">Log in to the [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="80c53-139">Válassza ki a Cloud Shell-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="80c53-139">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="80c53-140">A névminta a következő: `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="80c53-140">The name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Erőforráscsoport kiválasztása](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. <span data-ttu-id="80c53-142">Válassza ki a Cloud Shell tárfiókját.</span><span class="sxs-lookup"><span data-stu-id="80c53-142">Select the storage account for your Cloud Shell.</span></span>

   ![Adattároló fiók kiválasztása](./media/resource-group-template-deploy-cli/select-storage.png)

4. <span data-ttu-id="80c53-144">Válassza a **Files** (Fájlok) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="80c53-144">Select **Files**.</span></span>

   ![Fájlok kiválasztása](./media/resource-group-template-deploy-cli/select-files.png)

5. <span data-ttu-id="80c53-146">Válassza ki a Cloud Shell fájlmegosztását.</span><span class="sxs-lookup"><span data-stu-id="80c53-146">Select the file share for Cloud Shell.</span></span> <span data-ttu-id="80c53-147">A névminta a következő: `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="80c53-147">The name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Fájlmegosztás kiválasztása](./media/resource-group-template-deploy-cli/select-file-share.png)

6. <span data-ttu-id="80c53-149">Válassza az **Add directory** (Könyvtár hozzáadása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="80c53-149">Select **Add directory**.</span></span>

   ![Könyvtár hozzáadása](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. <span data-ttu-id="80c53-151">Nevezze el a könyvtárat **templates** (sablonok) néven, és válassza az **Okay** (OK) gombot.</span><span class="sxs-lookup"><span data-stu-id="80c53-151">Name it **templates**, and select **Okay**.</span></span>

   ![Könyvtár elnevezése](./media/resource-group-template-deploy-cli/name-templates.png)

8. <span data-ttu-id="80c53-153">Jelölje ki az új könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="80c53-153">Select your new directory.</span></span>

   ![Könyvtár kijelölése](./media/resource-group-template-deploy-cli/select-templates.png)

9. <span data-ttu-id="80c53-155">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="80c53-155">Select **Upload**.</span></span>

   ![Feltöltés kiválasztása](./media/resource-group-template-deploy-cli/select-upload.png)

10. <span data-ttu-id="80c53-157">Keresse meg és töltse fel a sablont.</span><span class="sxs-lookup"><span data-stu-id="80c53-157">Find and upload your template.</span></span>

   ![Fájl feltöltése](./media/resource-group-template-deploy-cli/upload-files.png)

11. <span data-ttu-id="80c53-159">Nyissa meg a parancssort.</span><span class="sxs-lookup"><span data-stu-id="80c53-159">Open the prompt.</span></span>

   ![Cloud Shell megnyitása](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. <span data-ttu-id="80c53-161">Írja be a következő parancsokat a Cloud Shellbe:</span><span class="sxs-lookup"><span data-stu-id="80c53-161">Enter the following commands in the Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a><span data-ttu-id="80c53-162">A paraméter fájlok</span><span class="sxs-lookup"><span data-stu-id="80c53-162">Parameter files</span></span>

<span data-ttu-id="80c53-163">Ahelyett, hogy a parancsfájl beágyazott értékeiként paraméterek átadása, előfordulhat, hogy ez egyszerűbbé teszi a paraméterek értékeit tartalmazó JSON-fájl használatára.</span><span class="sxs-lookup"><span data-stu-id="80c53-163">Rather than passing parameters as inline values in your script, you may find it easier to use a JSON file that contains the parameter values.</span></span> <span data-ttu-id="80c53-164">A paraméterfájl a következő formátumúnak kell lennie:</span><span class="sxs-lookup"><span data-stu-id="80c53-164">The parameter file must be in the following format:</span></span>

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

<span data-ttu-id="80c53-165">Figyelje meg, hogy a Paraméterek szakaszban tartalmazza-e a paraméter neve, amely megfelel a sablonban (storageAccountType) meghatározott paraméter.</span><span class="sxs-lookup"><span data-stu-id="80c53-165">Notice that the parameters section includes a parameter name that matches the parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="80c53-166">A paraméterfájl a paraméter értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="80c53-166">The parameter file contains a value for the parameter.</span></span> <span data-ttu-id="80c53-167">Ezt az értéket automatikusan kerülnek a sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="80c53-167">This value is automatically passed to the template during deployment.</span></span> <span data-ttu-id="80c53-168">Hozzon létre különböző telepítési forgatókönyvek esetén több paraméter fájlt, és akkor továbbítja a megfelelő paraméter fájlban.</span><span class="sxs-lookup"><span data-stu-id="80c53-168">You can create multiple parameter files for different deployment scenarios, and then pass in the appropriate parameter file.</span></span> 

<span data-ttu-id="80c53-169">Másolja át az előző példában, és mentse a fájlt `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="80c53-169">Copy the preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="80c53-170">A helyi paraméterfájl továbbítani, használja `@` storage.parameters.json nevű helyi fájl megadását.</span><span class="sxs-lookup"><span data-stu-id="80c53-170">To pass a local parameter file, use `@` to specify a local file named storage.parameters.json.</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a><span data-ttu-id="80c53-171">A sablon üzemelő példány tesztelése</span><span class="sxs-lookup"><span data-stu-id="80c53-171">Test a template deployment</span></span>

<span data-ttu-id="80c53-172">Minden olyan erőforrásnál tényleges telepítése nélkül a sablonnal és paraméterfájlokkal értékek teszteléséhez [az csoport központi telepítésének ellenőrzése](/cli/azure/group/deployment#validate).</span><span class="sxs-lookup"><span data-stu-id="80c53-172">To test your template and parameter values without actually deploying any resources, use [az group deployment validate](/cli/azure/group/deployment#validate).</span></span> 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

<span data-ttu-id="80c53-173">Ha nincsenek hibák, a parancs a teszttelepítés információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="80c53-173">If no errors are detected, the command returns information about the test deployment.</span></span> <span data-ttu-id="80c53-174">Különösen figyelje meg, hogy a **hiba** értéke null.</span><span class="sxs-lookup"><span data-stu-id="80c53-174">In particular, notice that the **error** value is null.</span></span>

```azurecli
{
  "error": null,
  "properties": {
      ...
```

<span data-ttu-id="80c53-175">Ha a rendszer hibát észlel, a parancs hibaüzenetet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="80c53-175">If an error is detected, the command returns an error message.</span></span> <span data-ttu-id="80c53-176">Például a tárfiók SKU, helytelen értéket átadni próbált a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="80c53-176">For example, attempting to pass an incorrect value for the storage account SKU, returns the following error:</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

<span data-ttu-id="80c53-177">Ha a sablon szintaktikai hibát tartalmaz, a parancs nem tudta elemezni a sablon jelző hiba adja vissza.</span><span class="sxs-lookup"><span data-stu-id="80c53-177">If your template has a syntax error, the command returns an error indicating it could not parse the template.</span></span> <span data-ttu-id="80c53-178">Az üzenet azt jelzi, a sor számának megjelenítése és elhelyezése az elemzési hiba.</span><span class="sxs-lookup"><span data-stu-id="80c53-178">The message indicates the line number and position of the parsing error.</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

<span data-ttu-id="80c53-179">Teljes módot használja, használja a `mode` paraméter:</span><span class="sxs-lookup"><span data-stu-id="80c53-179">To use complete mode, use the `mode` parameter:</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a><span data-ttu-id="80c53-180">Minta sablon</span><span class="sxs-lookup"><span data-stu-id="80c53-180">Sample template</span></span>

<span data-ttu-id="80c53-181">Ebben a témakörben szereplő példák a következő sablon használható.</span><span class="sxs-lookup"><span data-stu-id="80c53-181">The following template is used for the examples in this topic.</span></span> <span data-ttu-id="80c53-182">Másolja ki és mentse azt egy storage.json nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="80c53-182">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="80c53-183">Ez a sablon létrehozása ismertetése: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-183">To understand how to create this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="80c53-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80c53-184">Next steps</span></span>
* <span data-ttu-id="80c53-185">Ebben a cikkben szereplő példák erőforrások telepítése az alapértelmezett előfizetésében az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="80c53-185">The examples in this article deploy resources to a resource group in your default subscription.</span></span> <span data-ttu-id="80c53-186">Használjon másik előfizetést, lásd: [több Azure-előfizetések kezeléséhez](/cli/azure/manage-azure-subscriptions-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="80c53-186">To use a different subscription, see [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>
* <span data-ttu-id="80c53-187">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [Resource Manager sablon üzembe helyezési parancsfájl](resource-manager-samples-cli-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-187">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-cli-deploy.md).</span></span>
* <span data-ttu-id="80c53-188">Szeretné megtudni, hogyan adhat meg a paramétereket a sablonban, lásd: [megérteni a felépítését és Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-188">To understand how to define parameters in your template, see [Understand the structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="80c53-189">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-189">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="80c53-190">A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="80c53-190">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>
* <span data-ttu-id="80c53-191">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="80c53-191">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
