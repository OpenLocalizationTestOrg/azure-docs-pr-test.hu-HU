---
title: "az Azure-telepítés aaaLink sablonok |} Microsoft Docs"
description: "Ismerteti, hogyan toouse társított sablonok az Azure Resource Manager sablon toocreate moduláris sablon megoldást. Bemutatja, hogyan toopass paraméterek értékét, adja meg a paraméter-fájlt, és dinamikusan létrejön az URL-címeket."
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
ms.openlocfilehash: b935b1810db5ce894d009403cd4bb945cab34ba7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a><span data-ttu-id="1176c-104">Azure-erőforrások telepítésekor kapcsolt sablonok használata</span><span class="sxs-lookup"><span data-stu-id="1176c-104">Using linked templates when deploying Azure resources</span></span>
<span data-ttu-id="1176c-105">A belül egy Azure Resource Manager-sablon, társíthatja tooanother sablont, amely lehetővé teszi a toodecompose célzott konkrét, Célspecifikus sablonok készletére a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="1176c-105">From within one Azure Resource Manager template, you can link tooanother template, which enables you toodecompose your deployment into a set of targeted, purpose-specific templates.</span></span> <span data-ttu-id="1176c-106">Csakúgy, mint egy alkalmazás több kód osztályokba decomposing, a felbontás ellen tesztelése, újbóli és olvashatóság előnyt kínál.</span><span class="sxs-lookup"><span data-stu-id="1176c-106">As with decomposing an application into several code classes, decomposition provides benefits in terms of testing, reuse, and readability.</span></span>  

<span data-ttu-id="1176c-107">Paraméterek átadhatók egy fő sablont tooa csatolt sablonból, és ezeket a paramétereket közvetlenül hozzárendelhető tooparameters vagy hello hívja a sablon által elérhetővé tett változók.</span><span class="sxs-lookup"><span data-stu-id="1176c-107">You can pass parameters from a main template tooa linked template, and those parameters can directly map tooparameters or variables exposed by hello calling template.</span></span> <span data-ttu-id="1176c-108">hello csatolt sablon is eltelhet egy kimeneti változó hátsó toohello Forrássablon, sablonok közötti kétirányú adatcsere engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1176c-108">hello linked template can also pass an output variable back toohello source template, enabling a two-way data exchange between templates.</span></span>

## <a name="linking-tooa-template"></a><span data-ttu-id="1176c-109">Hivatkozási tooa sablon</span><span class="sxs-lookup"><span data-stu-id="1176c-109">Linking tooa template</span></span>
<span data-ttu-id="1176c-110">Létrehozhat egy hivatkozást hello fő sablon található központi telepítési erőforráshoz pontok toohello csatolt sablon hozzáadásával két sablonok között.</span><span class="sxs-lookup"><span data-stu-id="1176c-110">You create a link between two templates by adding a deployment resource within hello main template that points toohello linked template.</span></span> <span data-ttu-id="1176c-111">Beállíthatja a hello **templateLink** tulajdonság toohello hello csatolt sablon URI.</span><span class="sxs-lookup"><span data-stu-id="1176c-111">You set hello **templateLink** property toohello URI of hello linked template.</span></span> <span data-ttu-id="1176c-112">Hello csatolt sablon közvetlenül a sablonban vagy egy paraméterfájl biztosítható a paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="1176c-112">You can provide parameter values for hello linked template directly in your template or in a parameter file.</span></span> <span data-ttu-id="1176c-113">hello alábbi példában hello **paraméterek** tulajdonság toospecify közvetlenül a paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="1176c-113">hello following example uses hello **parameters** property toospecify a parameter value directly.</span></span>

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

<span data-ttu-id="1176c-114">Más típusú erőforrások, például a hello csatolt sablon és egyéb erőforrások közti függőségeket is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="1176c-114">Like other resource types, you can set dependencies between hello linked template and other resources.</span></span> <span data-ttu-id="1176c-115">Ezért ha más erőforrásokhoz szükséges hello csatolt sablonból egy kimeneti értéket, biztosíthatja, hello csatolt sablon előtt történik.</span><span class="sxs-lookup"><span data-stu-id="1176c-115">Therefore, when other resources require an output value from hello linked template, you can make sure hello linked template is deployed before them.</span></span> <span data-ttu-id="1176c-116">Vagy hello csatolt sablon más erőforrások támaszkodik, ha biztos lehet benne, más erőforrások telepítése előtt hello csatolt sablont.</span><span class="sxs-lookup"><span data-stu-id="1176c-116">Or, when hello linked template relies on other resources, you can make sure other resources are deployed before hello linked template.</span></span> <span data-ttu-id="1176c-117">Csatolt sablonból értéket kérheti le a hello a következő szintaxist:</span><span class="sxs-lookup"><span data-stu-id="1176c-117">You can retrieve a value from a linked template with hello following syntax:</span></span>

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

<span data-ttu-id="1176c-118">Erőforrás-kezelő szolgáltatás hello képes tooaccess hello csatolt sablon kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1176c-118">hello Resource Manager service must be able tooaccess hello linked template.</span></span> <span data-ttu-id="1176c-119">Egy helyi fájl vagy a fájl, amely csak akkor érhető el a helyi hálózaton hello csatolt sablon nem adható meg.</span><span class="sxs-lookup"><span data-stu-id="1176c-119">You cannot specify a local file or a file that is only available on your local network for hello linked template.</span></span> <span data-ttu-id="1176c-120">Csak adja meg, amely tartalmazza az vagy URI érték **http** vagy **https**.</span><span class="sxs-lookup"><span data-stu-id="1176c-120">You can only provide a URI value that includes either **http** or **https**.</span></span> <span data-ttu-id="1176c-121">Egy elem tooplace egy tárfiókot, és használni a csatolt sablon hello URI, hogy az elem, például a következő példa hello látható:</span><span class="sxs-lookup"><span data-stu-id="1176c-121">One option is tooplace your linked template in a storage account, and use hello URI for that item, such as shown in hello following example:</span></span>

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

<span data-ttu-id="1176c-122">Bár a hello csatolt sablon külsőleg elérhetőnek kell lennie, nem kell toobe általánosan elérhető toohello nyilvános.</span><span class="sxs-lookup"><span data-stu-id="1176c-122">Although hello linked template must be externally available, it does not need toobe generally available toohello public.</span></span> <span data-ttu-id="1176c-123">A sablon tooa titkos tárfiókja, amely elérhető tooonly hello tárfiók tulajdonosa adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="1176c-123">You can add your template tooa private storage account that is accessible tooonly hello storage account owner.</span></span> <span data-ttu-id="1176c-124">Ezután létrehozhat egy közös hozzáférésű jogosultságkód (SAS) token tooenable hozzáférés üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="1176c-124">Then, you create a shared access signature (SAS) token tooenable access during deployment.</span></span> <span data-ttu-id="1176c-125">A SAS-token toohello URI, hello csatolt sablon hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1176c-125">You add that SAS token toohello URI for hello linked template.</span></span> <span data-ttu-id="1176c-126">A sablont a storage-fiók beállítása és SAS-token létrehozása lépéseiért lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md) vagy [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1176c-126">For steps on setting up a template in a storage account and generating a SAS token, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md) or [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> 

<span data-ttu-id="1176c-127">a következő példa hello szülő sablon hivatkozások tooanother sablon jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1176c-127">hello following example shows a parent template that links tooanother template.</span></span> <span data-ttu-id="1176c-128">hello csatolt sablon paraméterként átadott SAS-jogkivonat segítségével érhető el.</span><span class="sxs-lookup"><span data-stu-id="1176c-128">hello linked template is accessed with a SAS token that is passed in as a parameter.</span></span>

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

<span data-ttu-id="1176c-129">Annak ellenére, hogy hello token érték az átadott egy biztonságos karakterláncot, hello hello csatolt sablon, többek között a hello SAS-jogkivonat URI naplózott hello üzembe helyezési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="1176c-129">Even though hello token is passed in as a secure string, hello URI of hello linked template, including hello SAS token, is logged in hello deployment operations.</span></span> <span data-ttu-id="1176c-130">toolimit kapta, beállíthatja a hello jogkivonat egy lejárati idejét.</span><span class="sxs-lookup"><span data-stu-id="1176c-130">toolimit exposure, set an expiration for hello token.</span></span>

<span data-ttu-id="1176c-131">Erőforrás-kezelő ennek egy külön központi telepítés minden egyes csatolt sablon kezeli.</span><span class="sxs-lookup"><span data-stu-id="1176c-131">Resource Manager handles each linked template as a separate deployment.</span></span> <span data-ttu-id="1176c-132">Hello üzembe helyezési előzményeket hello erőforráscsoport külön központi telepítéseinek hello szülő és a beágyazott sablonok látható.</span><span class="sxs-lookup"><span data-stu-id="1176c-132">In hello deployment history for hello resource group, you see separate deployments for hello parent and nested templates.</span></span>

![telepítési előzmények](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-tooa-parameter-file"></a><span data-ttu-id="1176c-134">Tooa paraméter fájl csatolása</span><span class="sxs-lookup"><span data-stu-id="1176c-134">Linking tooa parameter file</span></span>
<span data-ttu-id="1176c-135">hello következő példában hello **parametersLink** tulajdonság toolink tooa paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="1176c-135">hello next example uses hello **parametersLink** property toolink tooa parameter file.</span></span>

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

<span data-ttu-id="1176c-136">hello URI érték hello csatolt paraméterfájl nem lehet egy helyi fájl, és tartalmaznia kell vagy **http** vagy **https**.</span><span class="sxs-lookup"><span data-stu-id="1176c-136">hello URI value for hello linked parameter file cannot be a local file, and must include either **http** or **https**.</span></span> <span data-ttu-id="1176c-137">hello paraméterfájl keresztül egy SAS-tokennel korlátozott tooaccess is lehet.</span><span class="sxs-lookup"><span data-stu-id="1176c-137">hello parameter file can also be limited tooaccess through a SAS token.</span></span>

## <a name="using-variables-toolink-templates"></a><span data-ttu-id="1176c-138">Változók toolink sablonokkal</span><span class="sxs-lookup"><span data-stu-id="1176c-138">Using variables toolink templates</span></span>
<span data-ttu-id="1176c-139">hello előző példák azt szemléltették URL-cím értékeit kódolt hello sablon hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1176c-139">hello previous examples showed hard-coded URL values for hello template links.</span></span> <span data-ttu-id="1176c-140">Ez a módszer egy egyszerű sablon esetében is működik, de nem működik jól, ha nagy számú moduláris sablonok használata.</span><span class="sxs-lookup"><span data-stu-id="1176c-140">This approach might work for a simple template but it does not work well when working with a large set of modular templates.</span></span> <span data-ttu-id="1176c-141">Ehelyett statikus változó, amely tárolja az alap URL-cím hello fő sablon létrehozása és majd hozható létre dinamikusan URL-címek kapcsolódó hello sablonok az alap URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1176c-141">Instead, you can create a static variable that stores a base URL for hello main template and then dynamically create URLs for hello linked templates from that base URL.</span></span> <span data-ttu-id="1176c-142">hello előnye, hogy ez a megközelítés ez is könnyen áthelyezése vagy elágazás hello sablon, mivel csak szüksége toochange hello statikus változó hello fő sablonban.</span><span class="sxs-lookup"><span data-stu-id="1176c-142">hello benefit of this approach is you can easily move or fork hello template because you only need toochange hello static variable in hello main template.</span></span> <span data-ttu-id="1176c-143">hello fő sablon hello megfelelő URI-k teljes hello kiválasztott sablon továbbítja.</span><span class="sxs-lookup"><span data-stu-id="1176c-143">hello main template passes hello correct URIs throughout hello decomposed template.</span></span>

<span data-ttu-id="1176c-144">hello következő példa bemutatja, hogyan toouse egy alap URL-cím toocreate két URL-címet, a kapcsolódó sablonok (**sharedTemplateUrl** és **vmTemplate**).</span><span class="sxs-lookup"><span data-stu-id="1176c-144">hello following example shows how toouse a base URL toocreate two URLs for linked templates (**sharedTemplateUrl** and **vmTemplate**).</span></span> 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

<span data-ttu-id="1176c-145">Is [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello hello aktuális sablon alap URL-címet, és egyéb sablonok a hello tooget hello URL-címet használja ugyanazt a helyet.</span><span class="sxs-lookup"><span data-stu-id="1176c-145">You can also use [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello base URL for hello current template, and use that tooget hello URL for other templates in hello same location.</span></span> <span data-ttu-id="1176c-146">Ez a módszer akkor hasznos, ha a sablon helye megváltozik (lehet, hogy esedékes tooversioning) vagy a kívánt tooavoid rögzített kódolási hello sablonfájl URL-címeit.</span><span class="sxs-lookup"><span data-stu-id="1176c-146">This approach is useful if your template location changes (maybe due tooversioning) or you want tooavoid hard coding URLs in hello template file.</span></span> 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a><span data-ttu-id="1176c-147">Teljes példa</span><span class="sxs-lookup"><span data-stu-id="1176c-147">Complete example</span></span>
<span data-ttu-id="1176c-148">a következő példa sablonok hello megjelenítése kapcsolt sablonok tooillustrate egyszerűsített elrendezésének hello fogalmak számos ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="1176c-148">hello following example templates show a simplified arrangement of linked templates tooillustrate several of hello concepts in this article.</span></span> <span data-ttu-id="1176c-149">Azt feltételezi, hogy hello sablonok toohello ugyanabban a tárolóban, hozzáférésű tárfiókokban ki van kapcsolva lettek hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="1176c-149">It assumes hello templates have been added toohello same container in a storage account with public access turned off.</span></span> <span data-ttu-id="1176c-150">hello csatolt sablon érték hátsó toohello fő sablont továbbítja a hello **kimenete** szakasz.</span><span class="sxs-lookup"><span data-stu-id="1176c-150">hello linked template passes a value back toohello main template in hello **outputs** section.</span></span>

<span data-ttu-id="1176c-151">Hello **parent.json** fájl áll:</span><span class="sxs-lookup"><span data-stu-id="1176c-151">hello **parent.json** file consists of:</span></span>

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

<span data-ttu-id="1176c-152">Hello **helloworld.json** fájl áll:</span><span class="sxs-lookup"><span data-stu-id="1176c-152">hello **helloworld.json** file consists of:</span></span>

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

<span data-ttu-id="1176c-153">PowerShell, a szolgáltatáshitelesítést egy token hello tároló, és léptethet érvénybe hello sablon is van:</span><span class="sxs-lookup"><span data-stu-id="1176c-153">In PowerShell, you get a token for hello container and deploy hello templates with:</span></span>

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

<span data-ttu-id="1176c-154">Az Azure CLI 2.0 szolgáltatáshitelesítést egy token hello tároló, és a következő kód hello hello sablonok telepítése:</span><span class="sxs-lookup"><span data-stu-id="1176c-154">In Azure CLI 2.0, you get a token for hello container and deploy hello templates with hello following code:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1176c-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1176c-155">Next steps</span></span>
* <span data-ttu-id="1176c-156">toolearn hello telepítési ahhoz, hogy az erőforrások meghatározása hello kapcsolatban lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md)</span><span class="sxs-lookup"><span data-stu-id="1176c-156">toolearn about hello defining hello deployment order for your resources, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md)</span></span>
* <span data-ttu-id="1176c-157">toolearn hogyan toodefine egy erőforrás de hozzon létre több példányát, lásd: [erőforrások több példányát az Azure Resource Manager létrehozása](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="1176c-157">toolearn how toodefine one resource but create many instances of it, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>

