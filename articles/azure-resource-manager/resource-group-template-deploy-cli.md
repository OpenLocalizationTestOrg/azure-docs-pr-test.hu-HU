---
title: "az Azure CLI és sablon aaaDeploy erőforrások |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület toodeploy egy erőforrások tooAzure használja. a Resource Manager-sablon hello erőforrások vannak definiálva."
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
ms.openlocfilehash: 9f8bb9a8720399390a407030d2d32bcd97d32f13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a><span data-ttu-id="2adb7-104">Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure parancssori felületével</span><span class="sxs-lookup"><span data-stu-id="2adb7-104">Deploy resources with Resource Manager templates and Azure CLI</span></span>

<span data-ttu-id="2adb7-105">Ez a témakör azt ismerteti, hogyan toouse Azure CLI 2.0 a Resource Manager sablonok toodeploy az erőforrások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2adb7-105">This topic explains how toouse Azure CLI 2.0 with Resource Manager templates toodeploy your resources tooAzure.</span></span> <span data-ttu-id="2adb7-106">Ha nem ismeri a hello kapcsolatos alapfogalmakat üzembe helyezése és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-106">If you are not familiar with hello concepts of deploying and managing your Azure solutions, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>  

<span data-ttu-id="2adb7-107">azok a helyi fájl a számítógépre telepít hello Resource Manager-sablon, vagy egy külső egy például a GitHub-tárházban található fájl.</span><span class="sxs-lookup"><span data-stu-id="2adb7-107">hello Resource Manager template you deploy can either be a local file on your machine, or an external file that is located in a repository like GitHub.</span></span> <span data-ttu-id="2adb7-108">Ez a cikk központi telepítését hello sablon érhető el hello [mintasablon](#sample-template) szakasz, vagy regisztrációja, mivel egy [tárolási fiók sablon a Githubon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="2adb7-108">hello template you deploy in this article is available in hello [Sample template](#sample-template) section, or as a [storage account template in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).</span></span>

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

<span data-ttu-id="2adb7-109">Ha nincs telepítve az Azure parancssori felület, használhatja a hello [felhő rendszerhéj](#deploy-template-from-cloud-shell).</span><span class="sxs-lookup"><span data-stu-id="2adb7-109">If you do not have Azure CLI installed, you can use hello [Cloud Shell](#deploy-template-from-cloud-shell).</span></span>

## <a name="deploy-local-template"></a><span data-ttu-id="2adb7-110">Helyi sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="2adb7-110">Deploy local template</span></span>

<span data-ttu-id="2adb7-111">Erőforrások tooAzure telepítésekor meg:</span><span class="sxs-lookup"><span data-stu-id="2adb7-111">When deploying resources tooAzure, you:</span></span>

1. <span data-ttu-id="2adb7-112">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="2adb7-112">Log in tooyour Azure account</span></span>
2. <span data-ttu-id="2adb7-113">Hozzon létre egy erőforráscsoportot, amely hello telepített erőforrások hello tárolóként szolgál.</span><span class="sxs-lookup"><span data-stu-id="2adb7-113">Create a resource group that serves as hello container for hello deployed resources.</span></span> <span data-ttu-id="2adb7-114">hello erőforráscsoport nevét hello tartalmazhatnak alfanumerikus karaktereket, pontokat, aláhúzásjeleket, kötőjeleket és zárójeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="2adb7-114">hello name of hello resource group can only include alphanumeric characters, periods, underscores, hyphens, and parenthesis.</span></span> <span data-ttu-id="2adb7-115">Másolatot too90 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="2adb7-115">It can be up too90 characters.</span></span> <span data-ttu-id="2adb7-116">Nem végződhet ponttal.</span><span class="sxs-lookup"><span data-stu-id="2adb7-116">It cannot end in a period.</span></span>
3. <span data-ttu-id="2adb7-117">Toohello erőforrás csoport hello sablont, amely meghatározza a hello erőforrások toocreate telepítése</span><span class="sxs-lookup"><span data-stu-id="2adb7-117">Deploy toohello resource group hello template that defines hello resources toocreate</span></span>

<span data-ttu-id="2adb7-118">A sablon tartalmazhat, amelyek lehetővé teszik toocustomize hello telepítési paramétereit.</span><span class="sxs-lookup"><span data-stu-id="2adb7-118">A template can include parameters that enable you toocustomize hello deployment.</span></span> <span data-ttu-id="2adb7-119">Biztosíthatja például is lefednek értékeket (például a fejlesztői, tesztelési és éles) egy adott környezetben.</span><span class="sxs-lookup"><span data-stu-id="2adb7-119">For example, you can provide values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="2adb7-120">hello mintasablon hello tárfiók SKU paraméter határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2adb7-120">hello sample template defines a parameter for hello storage account SKU.</span></span> 

<span data-ttu-id="2adb7-121">a következő példa hello hoz létre egy erőforráscsoportot, és egy sablon, a helyi számítógépen telepíti:</span><span class="sxs-lookup"><span data-stu-id="2adb7-121">hello following example creates a resource group, and deploys a template from your local machine:</span></span>

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="2adb7-122">hello központi telepítés is igénybe vehet néhány percet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="2adb7-122">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="2adb7-123">A Befejezés után megjelenik egy üzenet, amely tartalmazza az hello eredmény:</span><span class="sxs-lookup"><span data-stu-id="2adb7-123">When it finishes, you see a message that includes hello result:</span></span>

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a><span data-ttu-id="2adb7-124">Külső sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="2adb7-124">Deploy external template</span></span>

<span data-ttu-id="2adb7-125">Toostore célszerű helyett Resource Manager-sablonok a helyi számítógépen, a külső helyre.</span><span class="sxs-lookup"><span data-stu-id="2adb7-125">Instead of storing Resource Manager templates on your local machine, you may prefer toostore them in an external location.</span></span> <span data-ttu-id="2adb7-126">A verziókövetési tárházat (például a Githubon) sablonok tárolhat.</span><span class="sxs-lookup"><span data-stu-id="2adb7-126">You can store templates in a source control repository (such as GitHub).</span></span> <span data-ttu-id="2adb7-127">Vagy tárolhatja őket egy Azure storage-fiók megosztott eléréséhez a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="2adb7-127">Or, you can store them in an Azure storage account for shared access in your organization.</span></span>

<span data-ttu-id="2adb7-128">egy külső sablon toodeploy hello használata **sablon-uri** paraméter.</span><span class="sxs-lookup"><span data-stu-id="2adb7-128">toodeploy an external template, use hello **template-uri** parameter.</span></span> <span data-ttu-id="2adb7-129">Hello URI hello példa toodeploy hello minta sablont a Githubból a használata.</span><span class="sxs-lookup"><span data-stu-id="2adb7-129">Use hello URI in hello example toodeploy hello sample template from GitHub.</span></span>
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

<span data-ttu-id="2adb7-130">hello előző példa kell rendelkeznie a nyilvánosan elérhető URI hello sablon, amely a legtöbb környezetben működik, mivel a sablon nem érzékeny adatot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="2adb7-130">hello preceding example requires a publicly accessible URI for hello template, which works for most scenarios because your template should not include sensitive data.</span></span> <span data-ttu-id="2adb7-131">Ha toospecify bizalmas adatok (például egy rendszergazdai jelszó) van szüksége, adja át ezt az értéket egy biztonságos paraméterben.</span><span class="sxs-lookup"><span data-stu-id="2adb7-131">If you need toospecify sensitive data (like an admin password), pass that value as a secure parameter.</span></span> <span data-ttu-id="2adb7-132">Azonban ha nem szeretné, hogy a sablon toobe nyilvánosan elérhető, megvédheti azokat a személyes tárolót tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="2adb7-132">However, if you do not want your template toobe publicly accessible, you can protect it by storing it in a private storage container.</span></span> <span data-ttu-id="2adb7-133">A sablont, amely közös hozzáférésű jogosultságkód (SAS) jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-133">For information about deploying a template that requires a shared access signature (SAS) token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>

## <a name="deploy-template-from-cloud-shell"></a><span data-ttu-id="2adb7-134">Sablon üzembe helyezése a Cloud Shellből</span><span class="sxs-lookup"><span data-stu-id="2adb7-134">Deploy template from Cloud Shell</span></span>

<span data-ttu-id="2adb7-135">Használhat [felhő rendszerhéj](../cloud-shell/overview.md) toorun hello Azure parancssori felület parancsai a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2adb7-135">You can use [Cloud Shell](../cloud-shell/overview.md) toorun hello Azure CLI commands for deploying your template.</span></span> <span data-ttu-id="2adb7-136">Azonban Ön először be kell tölteni a sablon hello fájlmegosztás be a felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="2adb7-136">However, you must first load your template into hello file share for your Cloud Shell.</span></span> <span data-ttu-id="2adb7-137">Ha még nem használta a Cloud Shellt, a telepítésével kapcsolatban lásd [Az Azure Cloud Shell áttekintése](../cloud-shell/overview.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="2adb7-137">If you have not used Cloud Shell, see [Overview of Azure Cloud Shell](../cloud-shell/overview.md) for information about setting it up.</span></span>

1. <span data-ttu-id="2adb7-138">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2adb7-138">Log in toohello [Azure portal](https://portal.azure.com).</span></span>   

2. <span data-ttu-id="2adb7-139">Válassza ki a Cloud Shell-erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2adb7-139">Select your Cloud Shell resource group.</span></span> <span data-ttu-id="2adb7-140">hello minta nem `cloud-shell-storage-<region>`.</span><span class="sxs-lookup"><span data-stu-id="2adb7-140">hello name pattern is `cloud-shell-storage-<region>`.</span></span>

   ![Erőforráscsoport kiválasztása](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. <span data-ttu-id="2adb7-142">A felhő rendszerhéj hello storage-fiók kiválasztása</span><span class="sxs-lookup"><span data-stu-id="2adb7-142">Select hello storage account for your Cloud Shell.</span></span>

   ![Adattároló fiók kiválasztása](./media/resource-group-template-deploy-cli/select-storage.png)

4. <span data-ttu-id="2adb7-144">Válassza a **Files** (Fájlok) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2adb7-144">Select **Files**.</span></span>

   ![Fájlok kiválasztása](./media/resource-group-template-deploy-cli/select-files.png)

5. <span data-ttu-id="2adb7-146">Válassza ki a hello fájlmegosztás felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="2adb7-146">Select hello file share for Cloud Shell.</span></span> <span data-ttu-id="2adb7-147">hello minta nem `cs-<user>-<domain>-com-<uniqueGuid>`.</span><span class="sxs-lookup"><span data-stu-id="2adb7-147">hello name pattern is `cs-<user>-<domain>-com-<uniqueGuid>`.</span></span>

   ![Fájlmegosztás kiválasztása](./media/resource-group-template-deploy-cli/select-file-share.png)

6. <span data-ttu-id="2adb7-149">Válassza az **Add directory** (Könyvtár hozzáadása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2adb7-149">Select **Add directory**.</span></span>

   ![Könyvtár hozzáadása](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. <span data-ttu-id="2adb7-151">Nevezze el a könyvtárat **templates** (sablonok) néven, és válassza az **Okay** (OK) gombot.</span><span class="sxs-lookup"><span data-stu-id="2adb7-151">Name it **templates**, and select **Okay**.</span></span>

   ![Könyvtár elnevezése](./media/resource-group-template-deploy-cli/name-templates.png)

8. <span data-ttu-id="2adb7-153">Jelölje ki az új könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="2adb7-153">Select your new directory.</span></span>

   ![Könyvtár kijelölése](./media/resource-group-template-deploy-cli/select-templates.png)

9. <span data-ttu-id="2adb7-155">Válassza a **Feltöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2adb7-155">Select **Upload**.</span></span>

   ![Feltöltés kiválasztása](./media/resource-group-template-deploy-cli/select-upload.png)

10. <span data-ttu-id="2adb7-157">Keresse meg és töltse fel a sablont.</span><span class="sxs-lookup"><span data-stu-id="2adb7-157">Find and upload your template.</span></span>

   ![Fájl feltöltése](./media/resource-group-template-deploy-cli/upload-files.png)

11. <span data-ttu-id="2adb7-159">Nyissa meg hello kérdés.</span><span class="sxs-lookup"><span data-stu-id="2adb7-159">Open hello prompt.</span></span>

   ![Cloud Shell megnyitása](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. <span data-ttu-id="2adb7-161">Adja meg a következő parancsok hello felhő rendszerhéj hello:</span><span class="sxs-lookup"><span data-stu-id="2adb7-161">Enter hello following commands in hello Cloud Shell:</span></span>

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a><span data-ttu-id="2adb7-162">A paraméter fájlok</span><span class="sxs-lookup"><span data-stu-id="2adb7-162">Parameter files</span></span>

<span data-ttu-id="2adb7-163">Ahelyett, hogy a parancsfájl beágyazott értékeiként paraméterek átadása, azt tapasztalhatja, könnyebben toouse hello paraméterértékek tartalmazó JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="2adb7-163">Rather than passing parameters as inline values in your script, you may find it easier toouse a JSON file that contains hello parameter values.</span></span> <span data-ttu-id="2adb7-164">hello paraméterfájl kell hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="2adb7-164">hello parameter file must be in hello following format:</span></span>

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

<span data-ttu-id="2adb7-165">Figyelje meg, hogy hello paraméterek szakaszban tartalmazza-e a paraméter neve, amely megfelel a sablonban (storageAccountType) meghatározott hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="2adb7-165">Notice that hello parameters section includes a parameter name that matches hello parameter defined in your template (storageAccountType).</span></span> <span data-ttu-id="2adb7-166">hello paraméterfájl hello paraméter értékét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2adb7-166">hello parameter file contains a value for hello parameter.</span></span> <span data-ttu-id="2adb7-167">Ezt az értéket automatikusan átadódik toohello sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="2adb7-167">This value is automatically passed toohello template during deployment.</span></span> <span data-ttu-id="2adb7-168">Hozzon létre különböző telepítési forgatókönyvek esetén több paraméter fájlt, és akkor továbbítja a hello megfelelő paraméter fájlban.</span><span class="sxs-lookup"><span data-stu-id="2adb7-168">You can create multiple parameter files for different deployment scenarios, and then pass in hello appropriate parameter file.</span></span> 

<span data-ttu-id="2adb7-169">Példa megelőző hello másolja, majd mentse a fájlt `storage.parameters.json`.</span><span class="sxs-lookup"><span data-stu-id="2adb7-169">Copy hello preceding example and save it as a file named `storage.parameters.json`.</span></span>

<span data-ttu-id="2adb7-170">a helyi paraméterfájl toopass használja `@` toospecify storage.parameters.json egy helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="2adb7-170">toopass a local parameter file, use `@` toospecify a local file named storage.parameters.json.</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a><span data-ttu-id="2adb7-171">A sablon üzemelő példány tesztelése</span><span class="sxs-lookup"><span data-stu-id="2adb7-171">Test a template deployment</span></span>

<span data-ttu-id="2adb7-172">tootest erőforrásokat, tényleges telepítése nélkül a sablonnal és paraméterfájlokkal értékeket használja [az csoport központi telepítésének ellenőrzése](/cli/azure/group/deployment#validate).</span><span class="sxs-lookup"><span data-stu-id="2adb7-172">tootest your template and parameter values without actually deploying any resources, use [az group deployment validate](/cli/azure/group/deployment#validate).</span></span> 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

<span data-ttu-id="2adb7-173">Ha nincsenek hibák, a hello parancs hello próbatelepítés információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2adb7-173">If no errors are detected, hello command returns information about hello test deployment.</span></span> <span data-ttu-id="2adb7-174">Különösen figyelje meg, hogy hello **hiba** értéke null.</span><span class="sxs-lookup"><span data-stu-id="2adb7-174">In particular, notice that hello **error** value is null.</span></span>

```azurecli
{
  "error": null,
  "properties": {
      ...
```

<span data-ttu-id="2adb7-175">Ha a rendszer hibát észlel, hello parancs hibaüzenetet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2adb7-175">If an error is detected, hello command returns an error message.</span></span> <span data-ttu-id="2adb7-176">Például kísérlet toopass hello tárfiók SKU, helytelen értéket adja vissza hello hiba a következő:</span><span class="sxs-lookup"><span data-stu-id="2adb7-176">For example, attempting toopass an incorrect value for hello storage account SKU, returns hello following error:</span></span>

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'hello provided value 'badSKU' for hello template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. hello parameter value is not part of hello allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

<span data-ttu-id="2adb7-177">Ha a sablon szintaktikai hibát tartalmaz, a hello parancs nem tudta elemezni a hello sablon jelző hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2adb7-177">If your template has a syntax error, hello command returns an error indicating it could not parse hello template.</span></span> <span data-ttu-id="2adb7-178">hello az üzenet azt jelzi, hello számát és a feldolgozási hiba hello pozícióját.</span><span class="sxs-lookup"><span data-stu-id="2adb7-178">hello message indicates hello line number and position of hello parsing error.</span></span>

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

<span data-ttu-id="2adb7-179">toouse teljes módban használja hello `mode` paraméter:</span><span class="sxs-lookup"><span data-stu-id="2adb7-179">toouse complete mode, use hello `mode` parameter:</span></span>

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a><span data-ttu-id="2adb7-180">Minta sablon</span><span class="sxs-lookup"><span data-stu-id="2adb7-180">Sample template</span></span>

<span data-ttu-id="2adb7-181">hello következő sablon használható hello példák ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="2adb7-181">hello following template is used for hello examples in this topic.</span></span> <span data-ttu-id="2adb7-182">Másolja ki és mentse azt egy storage.json nevű fájlba.</span><span class="sxs-lookup"><span data-stu-id="2adb7-182">Copy and save it as a file named storage.json.</span></span> <span data-ttu-id="2adb7-183">toounderstand hogyan toocreate ezen sablon esetén lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-183">toounderstand how toocreate this template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>  

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

## <a name="next-steps"></a><span data-ttu-id="2adb7-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2adb7-184">Next steps</span></span>
* <span data-ttu-id="2adb7-185">a cikkben szereplő példák hello erőforrások alapértelmezett előfizetése tooa erőforráscsoport telepítése.</span><span class="sxs-lookup"><span data-stu-id="2adb7-185">hello examples in this article deploy resources tooa resource group in your default subscription.</span></span> <span data-ttu-id="2adb7-186">toouse egy másik előfizetést, lásd: [több Azure-előfizetések kezeléséhez](/cli/azure/manage-azure-subscriptions-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2adb7-186">toouse a different subscription, see [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>
* <span data-ttu-id="2adb7-187">Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [Resource Manager sablon üzembe helyezési parancsfájl](resource-manager-samples-cli-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-187">For a complete sample script that deploys a template, see [Resource Manager template deployment script](resource-manager-samples-cli-deploy.md).</span></span>
* <span data-ttu-id="2adb7-188">Hogyan toodefine paramétereket a sablonban: toounderstand [hello struktúra és az Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-188">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="2adb7-189">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-189">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="2adb7-190">A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-cli-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-190">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-cli-sas-token.md).</span></span>
* <span data-ttu-id="2adb7-191">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="2adb7-191">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
