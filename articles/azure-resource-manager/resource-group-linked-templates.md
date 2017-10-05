---
title: "Hivatkozás a sablonokat az Azure-telepítés |} Microsoft Docs"
description: "Ismerteti az Azure Resource Manager sablon kapcsolt sablonok segítségével moduláris sablon megoldás létrehozása. Bemutatja, hogyan továbbítsa a paraméterértéket, adja meg a paraméter fájlt, és dinamikusan létrehozott URL-címeket."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a><span data-ttu-id="6ef5b-104">Azure-erőforrások telepítésekor kapcsolt sablonok használata</span><span class="sxs-lookup"><span data-stu-id="6ef5b-104">Using linked templates when deploying Azure resources</span></span>
<span data-ttu-id="6ef5b-105">A belül egy Azure Resource Manager sablon hozzákapcsolhatja egy másik sablont, amely lehetővé teszi, hogy a központi telepítés egy felbontani megcélzott, cél-specifikus sablonok.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-105">From within one Azure Resource Manager template, you can link to another template, which enables you to decompose your deployment into a set of targeted, purpose-specific templates.</span></span> <span data-ttu-id="6ef5b-106">Csakúgy, mint egy alkalmazás több kód osztályokba decomposing, a felbontás ellen tesztelése, újbóli és olvashatóság előnyt kínál.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-106">As with decomposing an application into several code classes, decomposition provides benefits in terms of testing, reuse, and readability.</span></span>  

<span data-ttu-id="6ef5b-107">Átadhatók fő sablonból paraméterek csatolt sablont, és ezeket a paramétereket közvetlenül hozzárendelhető paraméterek és változók jelennek meg, ha a hívó sablont.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-107">You can pass parameters from a main template to a linked template, and those parameters can directly map to parameters or variables exposed by the calling template.</span></span> <span data-ttu-id="6ef5b-108">A csatolt sablont is eltelhet egy kimeneti változó vissza a Forrássablon a sablonok között kétirányú adatcsere engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-108">The linked template can also pass an output variable back to the source template, enabling a two-way data exchange between templates.</span></span>

## <a name="linking-to-a-template"></a><span data-ttu-id="6ef5b-109">A sablon csatolása</span><span class="sxs-lookup"><span data-stu-id="6ef5b-109">Linking to a template</span></span>
<span data-ttu-id="6ef5b-110">Létrehozhat egy hivatkozást a fő sablont, amely a csatolt sablon mutat található központi telepítési erőforráshoz hozzáadásával két sablonok között.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-110">You create a link between two templates by adding a deployment resource within the main template that points to the linked template.</span></span> <span data-ttu-id="6ef5b-111">Beállíthatja a **templateLink** URI-azonosítója a csatolt sablon tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-111">You set the **templateLink** property to the URI of the linked template.</span></span> <span data-ttu-id="6ef5b-112">A csatolt sablon közvetlenül a sablonban vagy egy paraméterfájl biztosítható a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-112">You can provide parameter values for the linked template directly in your template or in a parameter file.</span></span> <span data-ttu-id="6ef5b-113">Az alábbi példában a **paraméterek** tulajdonságot közvetlenül adja meg a paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-113">The following example uses the **parameters** property to specify a parameter value directly.</span></span>

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

<span data-ttu-id="6ef5b-114">Más típusú erőforrások, például a csatolt sablont és egyéb erőforrások közti függőségeket is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-114">Like other resource types, you can set dependencies between the linked template and other resources.</span></span> <span data-ttu-id="6ef5b-115">Ezért ha más erőforrásokhoz szükséges egy kimeneti értéket, a csatolt sablonból, biztosíthatja, a csatolt sablon előtt történik.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-115">Therefore, when other resources require an output value from the linked template, you can make sure the linked template is deployed before them.</span></span> <span data-ttu-id="6ef5b-116">Vagy a csatolt sablon más erőforrások támaszkodik, ha biztos lehet benne, más erőforrások telepítése előtt a csatolt sablont.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-116">Or, when the linked template relies on other resources, you can make sure other resources are deployed before the linked template.</span></span> <span data-ttu-id="6ef5b-117">Egy érték beolvasható egy csatolt sablon a következő szintaxissal:</span><span class="sxs-lookup"><span data-stu-id="6ef5b-117">You can retrieve a value from a linked template with the following syntax:</span></span>

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

<span data-ttu-id="6ef5b-118">Az erőforrás-kezelő szolgáltatás eléréséhez a csatolt sablon képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-118">The Resource Manager service must be able to access the linked template.</span></span> <span data-ttu-id="6ef5b-119">Nem adhat meg egy helyi fájl vagy a fájl, amely csak akkor érhető el a csatolt sablon a helyi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-119">You cannot specify a local file or a file that is only available on your local network for the linked template.</span></span> <span data-ttu-id="6ef5b-120">Csak adja meg, amely tartalmazza az vagy URI érték **http** vagy **https**.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-120">You can only provide a URI value that includes either **http** or **https**.</span></span> <span data-ttu-id="6ef5b-121">Egy elem helyezze el a csatolt sablon egy tárfiókot, és az URI használata, hogy az elem, például az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="6ef5b-121">One option is to place your linked template in a storage account, and use the URI for that item, such as shown in the following example:</span></span>

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

<span data-ttu-id="6ef5b-122">Bár a csatolt sablon külsőleg elérhetőnek kell lennie, nem kell lennie a nyilvános általánosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-122">Although the linked template must be externally available, it does not need to be generally available to the public.</span></span> <span data-ttu-id="6ef5b-123">A sablon a személyes storage-fiók, amely csak a fiók tulajdonosa számára hozzáférhető is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-123">You can add your template to a private storage account that is accessible to only the storage account owner.</span></span> <span data-ttu-id="6ef5b-124">Ezután hozzon létre egy közös hozzáférésű jogosultságkód (SAS) token hozzáférés engedélyezése a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-124">Then, you create a shared access signature (SAS) token to enable access during deployment.</span></span> <span data-ttu-id="6ef5b-125">A SAS-token hozzáadása az URI a csatolt sablon.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-125">You add that SAS token to the URI for the linked template.</span></span> <span data-ttu-id="6ef5b-126">A sablont a storage-fiók beállítása és SAS-token létrehozása lépéseiért lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md) vagy [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="6ef5b-126">For steps on setting up a template in a storage account and generating a SAS token, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md) or [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> 

<span data-ttu-id="6ef5b-127">A következő példa bemutatja a szülő sablon egy másik sablon mutató.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-127">The following example shows a parent template that links to another template.</span></span> <span data-ttu-id="6ef5b-128">A csatolt sablon paraméterként átadott SAS-jogkivonat segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-128">The linked template is accessed with a SAS token that is passed in as a parameter.</span></span>

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

<span data-ttu-id="6ef5b-129">Annak ellenére, hogy a jogkivonat érték az átadott egy biztonságos karakterláncot, URI-azonosítója a csatolt sablon, beleértve a SAS-jogkivonat a telepítési műveleteket rögzíti.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-129">Even though the token is passed in as a secure string, the URI of the linked template, including the SAS token, is logged in the deployment operations.</span></span> <span data-ttu-id="6ef5b-130">Korlátozható a támadóknak, beállíthatja a egy lejárati idejét, a jogkivonat esetében.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-130">To limit exposure, set an expiration for the token.</span></span>

<span data-ttu-id="6ef5b-131">Erőforrás-kezelő ennek egy külön központi telepítés minden egyes csatolt sablon kezeli.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-131">Resource Manager handles each linked template as a separate deployment.</span></span> <span data-ttu-id="6ef5b-132">A központi telepítési előzmények ahhoz az erőforráscsoporthoz tekintse meg a szülő és a beágyazott sablonok külön központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-132">In the deployment history for the resource group, you see separate deployments for the parent and nested templates.</span></span>

![telepítési előzmények](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a><span data-ttu-id="6ef5b-134">A paraméterfájl csatolása</span><span class="sxs-lookup"><span data-stu-id="6ef5b-134">Linking to a parameter file</span></span>
<span data-ttu-id="6ef5b-135">A következő példában a **parametersLink** tulajdonság egy paraméter fájlra való hivatkozáshoz.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-135">The next example uses the **parametersLink** property to link to a parameter file.</span></span>

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

<span data-ttu-id="6ef5b-136">A csatolt paraméterfájl URI értéke nem lehet egy helyi fájl, és tartalmaznia kell vagy **http** vagy **https**.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-136">The URI value for the linked parameter file cannot be a local file, and must include either **http** or **https**.</span></span> <span data-ttu-id="6ef5b-137">A paraméterfájl is lehet korlátozni a SAS-jogkivonat-en keresztüli hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-137">The parameter file can also be limited to access through a SAS token.</span></span>

## <a name="using-variables-to-link-templates"></a><span data-ttu-id="6ef5b-138">Változók használata sablonok</span><span class="sxs-lookup"><span data-stu-id="6ef5b-138">Using variables to link templates</span></span>
<span data-ttu-id="6ef5b-139">Az előző példák azt szemléltették, hogy a sablon hivatkozások kódolt URL-cím értékeket.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-139">The previous examples showed hard-coded URL values for the template links.</span></span> <span data-ttu-id="6ef5b-140">Ez a módszer egy egyszerű sablon esetében is működik, de nem működik jól, ha nagy számú moduláris sablonok használata.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-140">This approach might work for a simple template but it does not work well when working with a large set of modular templates.</span></span> <span data-ttu-id="6ef5b-141">Ehelyett hozzon létre egy statikus változó, amely tárolja a fő sablon alap URL-címet, és majd hozható létre dinamikusan URL-címeket az alap URL-címet a kapcsolt sablonok.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-141">Instead, you can create a static variable that stores a base URL for the main template and then dynamically create URLs for the linked templates from that base URL.</span></span> <span data-ttu-id="6ef5b-142">Ez a megközelítés előnye, egyszerűen áthelyezheti vagy oszthatja ketté a sablont, mert csak módosítani szeretné a statikus változó a fő sablonban.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-142">The benefit of this approach is you can easily move or fork the template because you only need to change the static variable in the main template.</span></span> <span data-ttu-id="6ef5b-143">A fő sablont a megfelelő URI-k teljes lebontott sablon továbbítja.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-143">The main template passes the correct URIs throughout the decomposed template.</span></span>

<span data-ttu-id="6ef5b-144">A következő példa bemutatja, hogyan két URL-címéből kapcsolt sablonok létrehozásához használja az alap URL-cím (**sharedTemplateUrl** és **vmTemplate**).</span><span class="sxs-lookup"><span data-stu-id="6ef5b-144">The following example shows how to use a base URL to create two URLs for linked templates (**sharedTemplateUrl** and **vmTemplate**).</span></span> 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

<span data-ttu-id="6ef5b-145">Is [deployment()](resource-group-template-functions-deployment.md#deployment) az alap URL-CÍMÉT az aktuális sablon, és azt használja az URL-cím lekérésére más sablonok ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-145">You can also use [deployment()](resource-group-template-functions-deployment.md#deployment) to get the base URL for the current template, and use that to get the URL for other templates in the same location.</span></span> <span data-ttu-id="6ef5b-146">Ez a módszer akkor hasznos, ha a sablon helye megváltozik (lehet, hogy miatt versioning), vagy el szeretné kerülni a merevlemez kódolási URL-címek a sablon fájlban.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-146">This approach is useful if your template location changes (maybe due to versioning) or you want to avoid hard coding URLs in the template file.</span></span> 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a><span data-ttu-id="6ef5b-147">Teljes példa</span><span class="sxs-lookup"><span data-stu-id="6ef5b-147">Complete example</span></span>
<span data-ttu-id="6ef5b-148">Az alábbi példa sablonok kapcsolt sablonok egyszerűsített elrendezésének, több cikkben fogalmak szemléltetésére megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-148">The following example templates show a simplified arrangement of linked templates to illustrate several of the concepts in this article.</span></span> <span data-ttu-id="6ef5b-149">Azt feltételezi, hogy a sablonok lettek hozzáadva a tárfiók ugyanabban a tárolóban, hozzáférésű ki van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-149">It assumes the templates have been added to the same container in a storage account with public access turned off.</span></span> <span data-ttu-id="6ef5b-150">A csatolt sablon értéket átadja vissza a fő-sablon a **kimenete** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ef5b-150">The linked template passes a value back to the main template in the **outputs** section.</span></span>

<span data-ttu-id="6ef5b-151">A **parent.json** fájl áll:</span><span class="sxs-lookup"><span data-stu-id="6ef5b-151">The **parent.json** file consists of:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

<span data-ttu-id="6ef5b-152">A **helloworld.json** fájl áll:</span><span class="sxs-lookup"><span data-stu-id="6ef5b-152">The **helloworld.json** file consists of:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

<span data-ttu-id="6ef5b-153">PowerShell, a szolgáltatáshitelesítést egy token ahhoz a tárolóhoz, és telepítse központilag a sablon is van:</span><span class="sxs-lookup"><span data-stu-id="6ef5b-153">In PowerShell, you get a token for the container and deploy the templates with:</span></span>

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

<span data-ttu-id="6ef5b-154">Az Azure CLI 2.0 szolgáltatáshitelesítést egy token ahhoz a tárolóhoz, és telepítse központilag a sablonok a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="6ef5b-154">In Azure CLI 2.0, you get a token for the container and deploy the templates with the following code:</span></span>

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a><span data-ttu-id="6ef5b-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ef5b-155">Next steps</span></span>
* <span data-ttu-id="6ef5b-156">A telepítési sorrendet, az erőforrások meghatározása, lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="6ef5b-156">To learn about the defining the deployment order for your resources, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md)</span></span>
* <span data-ttu-id="6ef5b-157">Adja meg egy erőforrást, de több példányát létrehozni, lásd: [erőforrások több példányát az Azure Resource Manager létrehozása](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="6ef5b-157">To learn how to define one resource but create many instances of it, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>

