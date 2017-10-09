---
title: "aaaBest eljárások létrehozásához a Resource Manager-sablonok |} Microsoft Docs"
description: "Az Azure Resource Manager-sablonok egyszerűsítésére irányelveket."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: ec9bbe218c4f2c6a92ca44b5e9c9c71029e22151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a><span data-ttu-id="43171-103">Ajánlott eljárások az Azure Resource Manager-sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="43171-103">Best practices for creating Azure Resource Manager templates</span></span>
<span data-ttu-id="43171-104">Iránymutatás segítséget nyújtanak az Azure Resource Manager-sablonok, amelyek megbízható és könnyű toouse létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="43171-104">These guidelines can help you create Azure Resource Manager templates that are reliable and easy toouse.</span></span> <span data-ttu-id="43171-105">hello irányelvek csak javaslatok.</span><span class="sxs-lookup"><span data-stu-id="43171-105">hello guidelines are only suggestions.</span></span> <span data-ttu-id="43171-106">Nincsenek követelmények, kivéve, ha az áttelepítés előtt feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="43171-106">They are not requirements, except where noted.</span></span> <span data-ttu-id="43171-107">Adott esetben előfordulhat, hogy egy hello egyik változata a következő módszerekkel vagy példák.</span><span class="sxs-lookup"><span data-stu-id="43171-107">Your scenario might require a variation of one of hello following approaches or examples.</span></span>

## <a name="resource-names"></a><span data-ttu-id="43171-108">Erőforrások neve</span><span class="sxs-lookup"><span data-stu-id="43171-108">Resource names</span></span>
<span data-ttu-id="43171-109">Általában három típusú erőforrásnevek a Resource Manager használata:</span><span class="sxs-lookup"><span data-stu-id="43171-109">Generally, you work with three types of resource names in Resource Manager:</span></span>

* <span data-ttu-id="43171-110">Erőforrás neve, amely egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43171-110">Resource names that must be unique.</span></span>
* <span data-ttu-id="43171-111">Erőforrás neve nem egyedi toobe szükséges, de úgy dönt, hogy a nevet, amely segítségével azonosíthatja környezetben alapuló erőforrás tooprovide.</span><span class="sxs-lookup"><span data-stu-id="43171-111">Resource names that are not required toobe unique, but you choose tooprovide a name that can help you identify a resource based on context.</span></span>
* <span data-ttu-id="43171-112">Erőforrás neve, amely lehet általános.</span><span class="sxs-lookup"><span data-stu-id="43171-112">Resource names that can be generic.</span></span>

 <span data-ttu-id="43171-113">Erőforrás neve vonatkozó megkötésekkel kapcsolatos további információkért lásd: [Azure-erőforrások elnevezési szabályai ajánlott](../guidance/guidance-naming-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="43171-113">For information about resource name restrictions, see [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md).</span></span>

### <a name="unique-resource-names"></a><span data-ttu-id="43171-114">Egyedi erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="43171-114">Unique resource names</span></span>
<span data-ttu-id="43171-115">Egy adat-hozzáférési végponttal rendelkezik erőforrás típussal egyedi erőforrás nevét kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="43171-115">You must provide a unique resource name for any resource type that has a data access endpoint.</span></span> <span data-ttu-id="43171-116">Néhány gyakori erőforrástípusok esetében, amelyek egyedi nevet kell megadni a következők:</span><span class="sxs-lookup"><span data-stu-id="43171-116">Some common resource types that require a unique name include:</span></span>

* <span data-ttu-id="43171-117">Az Azure Storage<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="43171-117">Azure Storage<sup>1</sup></span></span> 
* <span data-ttu-id="43171-118">Web Apps funkció az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="43171-118">Web Apps feature of Azure App Service</span></span>
* <span data-ttu-id="43171-119">SQL Server</span><span class="sxs-lookup"><span data-stu-id="43171-119">SQL Server</span></span>
* <span data-ttu-id="43171-120">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="43171-120">Azure Key Vault</span></span>
* <span data-ttu-id="43171-121">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="43171-121">Azure Redis Cache</span></span>
* <span data-ttu-id="43171-122">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="43171-122">Azure Batch</span></span>
* <span data-ttu-id="43171-123">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="43171-123">Azure Traffic Manager</span></span>
* <span data-ttu-id="43171-124">Azure Search</span><span class="sxs-lookup"><span data-stu-id="43171-124">Azure Search</span></span>
* <span data-ttu-id="43171-125">Az Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="43171-125">Azure HDInsight</span></span>

<span data-ttu-id="43171-126"><sup>1</sup> tárfiókneveket is kisbetűnek kell lennie, 24 karakter vagy kevesebb, és nem rendelkezik a kötőjel.</span><span class="sxs-lookup"><span data-stu-id="43171-126"><sup>1</sup> Storage account names also must be lowercase, 24 characters or less, and not have any hyphens.</span></span>

<span data-ttu-id="43171-127">Egy paramétert az erőforrás nevét adja meg, ha hello erőforrás telepítésekor meg kell adnia egy egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="43171-127">If you provide a parameter for a resource name, you must provide a unique name when you deploy hello resource.</span></span> <span data-ttu-id="43171-128">Másik lehetőségként létrehozhat hello használó változó [uniqueString()](resource-group-template-functions-string.md#uniquestring) toogenerate nevét működik.</span><span class="sxs-lookup"><span data-stu-id="43171-128">Optionally, you can create a variable that uses hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) function toogenerate a name.</span></span> 

<span data-ttu-id="43171-129">Azt is előfordulhat, hogy szeretné tooadd előtag vagy utótag toohello **uniqueString** eredménye.</span><span class="sxs-lookup"><span data-stu-id="43171-129">You also might want tooadd a prefix or suffix toohello **uniqueString** result.</span></span> <span data-ttu-id="43171-130">Módosítása hello egyedi név segítségével további hello erőforrástípus neve alapján hello könnyebb azonosításához.</span><span class="sxs-lookup"><span data-stu-id="43171-130">Modifying hello unique name can help you more easily identify hello resource type from hello name.</span></span> <span data-ttu-id="43171-131">Például egy egyedi nevet a tárfiók hozhat létre a következő változó hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="43171-131">For example, you can generate a unique name for a storage account by using hello following variable:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a><span data-ttu-id="43171-132">Az azonosításhoz erőforrásnevek</span><span class="sxs-lookup"><span data-stu-id="43171-132">Resource names for identification</span></span>
<span data-ttu-id="43171-133">Egyes erőforrástípusok esetében érdemes tooname, de a nevek nem rendelkezik egyedi toobe.</span><span class="sxs-lookup"><span data-stu-id="43171-133">Some resource types you might want tooname, but their names do not have toobe unique.</span></span> <span data-ttu-id="43171-134">Ezen erőforrás esetében adja meg a nevet, amely azonosítja az erőforrás-környezet hello és a hello erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="43171-134">For these resource types, you can provide a name that identifies both hello resource context and hello resource type.</span></span> <span data-ttu-id="43171-135">Adjon meg egy leíró nevet, hogy könnyebb legyen azonosítani hello erőforrás tartozó erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="43171-135">Provide a descriptive name that helps you identify hello resource in a list of resources.</span></span> <span data-ttu-id="43171-136">Ha toouse különböző központi telepítésénél a különböző erőforrás nevét, a paraméter használható hello nevét:</span><span class="sxs-lookup"><span data-stu-id="43171-136">If you need toouse a different resource name for different deployments, you can use a parameter for hello name:</span></span>

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "hello name of hello VM toocreate."
        }
    }
}
```

<span data-ttu-id="43171-137">Toopass egy nevet a telepítés során nem szükséges, ha egy változót is használhatja:</span><span class="sxs-lookup"><span data-stu-id="43171-137">If you do not need toopass in a name during deployment, you can use a variable:</span></span> 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

<span data-ttu-id="43171-138">A kódolt érték is használható:</span><span class="sxs-lookup"><span data-stu-id="43171-138">You also can use a hard-coded value:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a><span data-ttu-id="43171-139">Általános erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="43171-139">Generic resource names</span></span>
<span data-ttu-id="43171-140">Erőforrás esetében, amelyek többnyire keresztül érhető el egy másik erőforráscsoportban általános neve nem változtatható hello sablonban is használhatja.</span><span class="sxs-lookup"><span data-stu-id="43171-140">For resource types that you mostly access through a different resource, you can use a generic name that is hard-coded in hello template.</span></span> <span data-ttu-id="43171-141">Például beállíthatja egy szabványos, általános nevet tűzfalszabályok SQL-kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="43171-141">For example, you can set a standard, generic name for firewall rules on a SQL server:</span></span>

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a><span data-ttu-id="43171-142">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="43171-142">Parameters</span></span>
<span data-ttu-id="43171-143">hello alábbi információ segítségével esetleg megállapítható paraméterek használata:</span><span class="sxs-lookup"><span data-stu-id="43171-143">hello following information can be helpful when you work with parameters:</span></span>

* <span data-ttu-id="43171-144">Minimalizálása érdekében a paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="43171-144">Minimize your use of parameters.</span></span> <span data-ttu-id="43171-145">Amikor csak lehetséges, használjon egy változó vagy konstansérték.</span><span class="sxs-lookup"><span data-stu-id="43171-145">Whenever possible, use a variable or a literal value.</span></span> <span data-ttu-id="43171-146">Paraméterek csak ezek helyzetekben használhatja:</span><span class="sxs-lookup"><span data-stu-id="43171-146">Use parameters only for these scenarios:</span></span>
   
   * <span data-ttu-id="43171-147">A beállítások, amelyet az toouse változatait tooenvironment (SKU, méret, kapacitás) szerint.</span><span class="sxs-lookup"><span data-stu-id="43171-147">Settings that you want toouse variations of according tooenvironment (SKU, size, capacity).</span></span>
   * <span data-ttu-id="43171-148">Erőforrás nevét, amelyet az toospecify könnyen azonosításához.</span><span class="sxs-lookup"><span data-stu-id="43171-148">Resource names that you want toospecify for easy identification.</span></span>
   * <span data-ttu-id="43171-149">Az értékeket, hogy a gyakran használt toocomplete más feladatokat (például egy rendszergazda felhasználójának neve).</span><span class="sxs-lookup"><span data-stu-id="43171-149">Values that you use frequently toocomplete other tasks (such as an admin user name).</span></span>
   * <span data-ttu-id="43171-150">A titkos kulcsokat (például a jelszavak).</span><span class="sxs-lookup"><span data-stu-id="43171-150">Secrets (such as passwords).</span></span>
   * <span data-ttu-id="43171-151">hello száma vagy értékek toouse erőforrástípus több példánya létrehozásakor tömbje.</span><span class="sxs-lookup"><span data-stu-id="43171-151">hello number or array of values toouse when you create multiple instances of a resource type.</span></span>
* <span data-ttu-id="43171-152">-És nagybetűhasználattal teve a paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="43171-152">Use camel case for parameter names.</span></span>
* <span data-ttu-id="43171-153">Adjon meg egy leírást, minden paraméter hello metaadatokban:</span><span class="sxs-lookup"><span data-stu-id="43171-153">Provide a description of every parameter in hello metadata:</span></span>

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "hello type of hello new storage account created toostore hello VM disks."
           }
       }
   }
   ```

* <span data-ttu-id="43171-154">Adja meg (kivéve a jelszavak és SSH-kulcsok) paraméterek alapértelmezett értéke:</span><span class="sxs-lookup"><span data-stu-id="43171-154">Define default values for parameters (except for passwords and SSH keys):</span></span>
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "hello type of hello new storage account created toostore hello VM disks."
            }
        }
   }
   ```

* <span data-ttu-id="43171-155">Használjon **SecureString** a jelszavak és a titkos kulcsok:</span><span class="sxs-lookup"><span data-stu-id="43171-155">Use **SecureString** for all passwords and secrets:</span></span> 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "hello value of hello secret toostore in hello vault."
           }
       }
   }
   ```

* <span data-ttu-id="43171-156">Amikor csak lehetséges, ne használjon egy paraméter toospecify helyre.</span><span class="sxs-lookup"><span data-stu-id="43171-156">Whenever possible, don't use a parameter toospecify location.</span></span> <span data-ttu-id="43171-157">Ehelyett használja a hello **hely** hello erőforráscsoport tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="43171-157">Instead, use hello **location** property of hello resource group.</span></span> <span data-ttu-id="43171-158">Hello segítségével **resourceGroup () .location** az erőforrások kifejezését erőforrások hello sablon telepítése hello hello erőforráscsoport és ugyanazon a helyen:</span><span class="sxs-lookup"><span data-stu-id="43171-158">By using hello **resourceGroup().location** expression for all your resources, resources in hello template are deployed in hello same location as hello resource group:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   <span data-ttu-id="43171-159">Ha egy erőforrás típusa támogatott helyek csak korlátozott számú, érdemes toospecify hello sablonban közvetlenül egy érvényes helyet.</span><span class="sxs-lookup"><span data-stu-id="43171-159">If a resource type is supported in only a limited number of locations, you might want toospecify a valid location directly in hello template.</span></span> <span data-ttu-id="43171-160">Használata esetén egy **hely** paraméter, a lehető legnagyobb mértékben paraméterérték megosztása a hello valószínűleg toobe erőforrásokhoz ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="43171-160">If you must use a **location** parameter, share that parameter value as much as possible with resources that are likely toobe in hello same location.</span></span> <span data-ttu-id="43171-161">Ez minimalizálja a hello szám, ahányszor a felhasználó felkérést kap tooprovide helyére vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="43171-161">This minimizes hello number of times users are asked tooprovide location information.</span></span>
* <span data-ttu-id="43171-162">Kerülje a paraméter vagy változó hello API-verzió erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="43171-162">Avoid using a parameter or variable for hello API version for a resource type.</span></span> <span data-ttu-id="43171-163">Erőforrás-tulajdonságok és értékek alapján verziószáma változhat.</span><span class="sxs-lookup"><span data-stu-id="43171-163">Resource properties and values can vary by version number.</span></span> <span data-ttu-id="43171-164">A kód szerkesztése az IntelliSense nem határozható meg, hogy megfelelő sémát hello hello API-verzió beállítása tooa paraméter vagy változó.</span><span class="sxs-lookup"><span data-stu-id="43171-164">IntelliSense in a code editor cannot determine hello correct schema when hello API version is set tooa parameter or variable.</span></span> <span data-ttu-id="43171-165">Ehelyett kódolnia hello hello sablonban API-verzió.</span><span class="sxs-lookup"><span data-stu-id="43171-165">Instead, hard-code hello API version in hello template.</span></span>

## <a name="variables"></a><span data-ttu-id="43171-166">Változók</span><span class="sxs-lookup"><span data-stu-id="43171-166">Variables</span></span>
<span data-ttu-id="43171-167">hello alábbi információ segítségével esetleg megállapítható változók használata:</span><span class="sxs-lookup"><span data-stu-id="43171-167">hello following information can be helpful when you work with variables:</span></span>

* <span data-ttu-id="43171-168">Változók használata, toouse csak egyszer kell a sablonban lévő érték.</span><span class="sxs-lookup"><span data-stu-id="43171-168">Use variables for values that you need toouse more than once in a template.</span></span> <span data-ttu-id="43171-169">Ha az érték csak egyszer legyen használva, kódolt értéket a sablon könnyebb tooread lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="43171-169">If a value is used only once, a hard-coded value makes your template easier tooread.</span></span>
* <span data-ttu-id="43171-170">Nem használhat hello [hivatkozás](resource-group-template-functions-resource.md#reference) hello függvényt **változók** hello sablon szakasza.</span><span class="sxs-lookup"><span data-stu-id="43171-170">You cannot use hello [reference](resource-group-template-functions-resource.md#reference) function in hello **variables** section of hello template.</span></span> <span data-ttu-id="43171-171">Hello **hivatkozás** függvény hello erőforrás futásidejű állapot az értékét osztályból származik.</span><span class="sxs-lookup"><span data-stu-id="43171-171">hello **reference** function derives its value from hello resource's runtime state.</span></span> <span data-ttu-id="43171-172">Azonban a változók feloldva hello kezdeti hello sablon elemzése során.</span><span class="sxs-lookup"><span data-stu-id="43171-172">However, variables are resolved during hello initial parsing of hello template.</span></span> <span data-ttu-id="43171-173">Értékek, amelyek hello kell összeállítani **hivatkozás** függvény közvetlenül a hello **erőforrások** vagy **kimenete** hello sablon szakasza.</span><span class="sxs-lookup"><span data-stu-id="43171-173">Construct values that need hello **reference** function directly in hello **resources** or **outputs** section of hello template.</span></span>
* <span data-ttu-id="43171-174">Az erőforrás nevét, amely egyedinek kell lennie, a változókat tartalmazhat [erőforrásnevek](#resource-names).</span><span class="sxs-lookup"><span data-stu-id="43171-174">Include variables for resource names that must be unique, as described in [Resource names](#resource-names).</span></span>
* <span data-ttu-id="43171-175">Változók összetett objektumokba csoportosíthatja.</span><span class="sxs-lookup"><span data-stu-id="43171-175">You can group variables into complex objects.</span></span> <span data-ttu-id="43171-176">Használjon hello **variable.subentry** formázása tooreference egy összetett objektum közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="43171-176">Use hello **variable.subentry** format tooreference a value from a complex object.</span></span> <span data-ttu-id="43171-177">Változók segítségével nyomon követheti a kapcsolódó változók.</span><span class="sxs-lookup"><span data-stu-id="43171-177">Grouping variables can help you track related variables.</span></span> <span data-ttu-id="43171-178">Ez növeli a hello sablon olvashatóság érdekében is.</span><span class="sxs-lookup"><span data-stu-id="43171-178">It also improves readability of hello template.</span></span> <span data-ttu-id="43171-179">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="43171-179">Here's an example:</span></span>
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > <span data-ttu-id="43171-180">Egy összetett objektum nem tartalmazhat egy kifejezés, amely egy összetett objektumot egy értéket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="43171-180">A complex object cannot contain an expression that references a value from a complex object.</span></span> <span data-ttu-id="43171-181">Külön változó erre a célra.</span><span class="sxs-lookup"><span data-stu-id="43171-181">Define a separate variable for this purpose.</span></span>
   > 
   > 
   
     <span data-ttu-id="43171-182">Speciális összetett objektumok használatával változókként, tekintse meg a [megosztani az Azure Resource Manager sablonokban állapot](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="43171-182">For advanced examples of using complex objects as variables, see [Share state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="resources"></a><span data-ttu-id="43171-183">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="43171-183">Resources</span></span>
<span data-ttu-id="43171-184">hello alábbi információ segítségével esetleg megállapítható erőforrások használatakor:</span><span class="sxs-lookup"><span data-stu-id="43171-184">hello following information can be helpful when you work with resources:</span></span>

* <span data-ttu-id="43171-185">toohelp más közreműködők megértéséhez hello erőforrás hello célját, adja meg **megjegyzések** hello sablonban az egyes erőforrások:</span><span class="sxs-lookup"><span data-stu-id="43171-185">toohelp other contributors understand hello purpose of hello resource, specify **comments** for each resource in hello template:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used toostore hello VM disks.",
         ...
     }
   ]
   ```

* <span data-ttu-id="43171-186">Címkék tooadd metaadatok tooresources is használhatja.</span><span class="sxs-lookup"><span data-stu-id="43171-186">You can use tags tooadd metadata tooresources.</span></span> <span data-ttu-id="43171-187">Az erőforrások metaadatok tooadd adatait használja.</span><span class="sxs-lookup"><span data-stu-id="43171-187">Use metadata tooadd information about your resources.</span></span> <span data-ttu-id="43171-188">Például hozzáadhat metaadatok toorecord számlázási tudnivalókról erőforrás.</span><span class="sxs-lookup"><span data-stu-id="43171-188">For example, you can add metadata toorecord billing details for a resource.</span></span> <span data-ttu-id="43171-189">További információkért lásd: [Using címkéket tooorganize az Azure-erőforrások](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="43171-189">For more information, see [Using tags tooorganize your Azure resources](resource-group-using-tags.md).</span></span>
* <span data-ttu-id="43171-190">Ha egy *nyilvános végpontot* a sablonban (például egy Azure Blob storage nyilvános végpontot), *do nem rögzített kód* névtér hello.</span><span class="sxs-lookup"><span data-stu-id="43171-190">If you use a *public endpoint* in your template (such as an Azure Blob storage public endpoint), *do not hard-code* hello namespace.</span></span> <span data-ttu-id="43171-191">Használjon hello **hivatkozás** függvény toodynamically hello névtér beolvasása.</span><span class="sxs-lookup"><span data-stu-id="43171-191">Use hello **reference** function toodynamically retrieve hello namespace.</span></span> <span data-ttu-id="43171-192">Ez a megközelítés toodeploy hello sablon toodifferent nyilvános névtér-környezetekben hello végpont hello sablonban manuális módosítása nélkül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="43171-192">You can use this approach toodeploy hello template toodifferent public namespace environments without manually changing hello endpoint in hello template.</span></span> <span data-ttu-id="43171-193">Állítsa be a hello API-verzió toohello hello tárfiókot használja a sablon azonos verziójú:</span><span class="sxs-lookup"><span data-stu-id="43171-193">Set hello API version toohello same version that you are using for hello storage account in your template:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="43171-194">Ha az hello tárfiók hello ugyanazt a sablont, amely hoz létre, nem kell toospecify hello szolgáltatójának névtere amikor hello erőforrás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="43171-194">If hello storage account is deployed in hello same template that you are creating, you do not need toospecify hello provider namespace when you reference hello resource.</span></span> <span data-ttu-id="43171-195">Ez az hello egyszerűsített szintaxist:</span><span class="sxs-lookup"><span data-stu-id="43171-195">This is hello simplified syntax:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="43171-196">Ha a sablont más értékek, amelyek a konfigurált toouse nyilvános névtér, ezek helyett tooreflect hello azonos **hivatkozás** függvény.</span><span class="sxs-lookup"><span data-stu-id="43171-196">If you have other values in your template that are configured toouse a public namespace, change these values tooreflect hello same **reference** function.</span></span> <span data-ttu-id="43171-197">Például beállíthatja a hello **storageUri** hello virtuális gép diagnosztikai profiljának tulajdonsága:</span><span class="sxs-lookup"><span data-stu-id="43171-197">For example, you can set hello **storageUri** property of hello virtual machine diagnostics profile:</span></span>
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   <span data-ttu-id="43171-198">Meglévő tárfiókot, amely egy másik erőforráscsoportban található is hivatkozhat:</span><span class="sxs-lookup"><span data-stu-id="43171-198">You also can reference an existing storage account that is in a different resource group:</span></span>

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* <span data-ttu-id="43171-199">Rendelje hozzá a nyilvános IP címek tooa virtuális gép csak akkor, ha egy alkalmazás írja elő.</span><span class="sxs-lookup"><span data-stu-id="43171-199">Assign public IP addresses tooa virtual machine only when an application requires it.</span></span> <span data-ttu-id="43171-200">tooconnect tooa virtuális gép (VM) hibakereséshez vagy felügyeleti vagy felügyeleti célokra, használja a bejövő NAT-szabályok, a virtuális hálózati átjáró vagy egy jumpbox.</span><span class="sxs-lookup"><span data-stu-id="43171-200">tooconnect tooa virtual machine (VM) for debugging, or for management or administrative purposes, use inbound NAT rules, a virtual network gateway, or a jumpbox.</span></span>
   
     <span data-ttu-id="43171-201">Csatlakozás toovirtual gépek kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="43171-201">For more information about connecting toovirtual machines, see:</span></span>
   
   * [<span data-ttu-id="43171-202">Futtassa a virtuális gépek Azure-ban N szintű architektúrája</span><span class="sxs-lookup"><span data-stu-id="43171-202">Run VMs for an N-tier architecture in Azure</span></span>](../guidance/guidance-compute-n-tier-vm.md)
   * [<span data-ttu-id="43171-203">A WinRM-hozzáférés beállítása az Azure Resource Manager virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="43171-203">Set up WinRM access for VMs in Azure Resource Manager</span></span>](../virtual-machines/windows/winrm.md)
   * [<span data-ttu-id="43171-204">Külső hozzáférés tooyour VM engedélyezése hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="43171-204">Allow external access tooyour VM by using hello Azure portal</span></span>](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [<span data-ttu-id="43171-205">Külső hozzáférés tooyour VM engedélyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="43171-205">Allow external access tooyour VM by using PowerShell</span></span>](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [<span data-ttu-id="43171-206">Külső hozzáférés tooyour Linux virtuális gép engedélyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="43171-206">Allow external access tooyour Linux VM by using Azure CLI</span></span>](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* <span data-ttu-id="43171-207">Hello **domainNameLabel** nyilvános IP-címekhez tulajdonságnak egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43171-207">hello **domainNameLabel** property for public IP addresses must be unique.</span></span> <span data-ttu-id="43171-208">Hello **domainNameLabel** értéket kell 3 és 63 karakter között lehet, és kövesse a reguláris kifejezés által meghatározott hello szabályok: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span><span class="sxs-lookup"><span data-stu-id="43171-208">hello **domainNameLabel** value must be between 3 and 63 characters long, and follow hello rules specified by this regular expression: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span></span> <span data-ttu-id="43171-209">Mivel hello **uniqueString** függvény karakterláncot hoz létre, amely 13 karakterig, hello **dnsPrefixString** paraméter korlátozott too50 karakterek:</span><span class="sxs-lookup"><span data-stu-id="43171-209">Because hello **uniqueString** function generates a string that is 13 characters long, hello **dnsPrefixString** parameter is limited too50 characters:</span></span>

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "hello DNS label for hello public IP address. It must be lowercase. It should match hello following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* <span data-ttu-id="43171-210">A jelszó tooa egyéni parancsprogramok futtatására szolgáló bővítmény hozzáadásakor használja hello **commandToExecute** hello tulajdonság **protectedSettings** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="43171-210">When you add a password tooa custom script extension, use hello **commandToExecute** property in hello **protectedSettings** property:</span></span>
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > <span data-ttu-id="43171-211">tooensure, amelyek a titkos kulcsok titkosított, amikor azok paraméterek tooVMs és bővítmények át lettek adva, használja a hello **protectedSettings** hello vonatkozó bővítmények tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="43171-211">tooensure that secrets are encrypted when they are passed as parameters tooVMs and extensions, use hello **protectedSettings** property of hello relevant extensions.</span></span>
   > 
   > 

## <a name="outputs"></a><span data-ttu-id="43171-212">kimenetek</span><span class="sxs-lookup"><span data-stu-id="43171-212">Outputs</span></span>
<span data-ttu-id="43171-213">Ha egy sablon toocreate nyilvános IP-címeket használ, egy **kimenete** szakasz adatait hello IP-cím és hello teljesen minősített tartománynevét (FQDN) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="43171-213">If you use a template toocreate public IP addresses, include an **outputs** section that returns details of hello IP address and hello fully qualified domain name (FQDN).</span></span> <span data-ttu-id="43171-214">A telepítés utáni kimeneti értékek tooeasily lekérése adatait nyilvános IP-címek és a teljes tartománynevek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="43171-214">You can use output values tooeasily retrieve details about public IP addresses and FQDNs after deployment.</span></span> <span data-ttu-id="43171-215">Ha hello erőforrás hivatkozik, amellyel toocreate hello API verzióját használja azt:</span><span class="sxs-lookup"><span data-stu-id="43171-215">When you reference hello resource, use hello API version that you used toocreate it:</span></span> 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a><span data-ttu-id="43171-216">Egy sablon és a beágyazott sablonok</span><span class="sxs-lookup"><span data-stu-id="43171-216">Single template vs. nested templates</span></span>
<span data-ttu-id="43171-217">toodeploy a megoldás több beágyazott sablonok használhatja ugyanazt a sablont vagy a fő sablont.</span><span class="sxs-lookup"><span data-stu-id="43171-217">toodeploy your solution, you can use either a single template or a main template with multiple nested templates.</span></span> <span data-ttu-id="43171-218">Beágyazott sablonok olyan közös speciális forgatókönyvekhez.</span><span class="sxs-lookup"><span data-stu-id="43171-218">Nested templates are common for more advanced scenarios.</span></span> <span data-ttu-id="43171-219">Egy beágyazott sablon ad meg a következő előnyöket hello használata:</span><span class="sxs-lookup"><span data-stu-id="43171-219">Using a nested template gives you hello following advantages:</span></span>

* <span data-ttu-id="43171-220">A célként megadott összetevők megoldás is lebontva.</span><span class="sxs-lookup"><span data-stu-id="43171-220">You can break down a solution into targeted components.</span></span>
* <span data-ttu-id="43171-221">Eltérő fő sablonok beágyazott sablonok is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="43171-221">You can reuse nested templates with different main templates.</span></span>

<span data-ttu-id="43171-222">Ha úgy dönt, hogy a beágyazott toouse sablonok, a következő irányelveket hello segítségével a sablon tervezési szabványosítására.</span><span class="sxs-lookup"><span data-stu-id="43171-222">If you choose toouse nested templates, hello following guidelines can help you standardize your template design.</span></span> <span data-ttu-id="43171-223">Ezeket az irányelveket alapuló [tervezése során az Azure Resource Manager-sablonok minták](best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="43171-223">These guidelines are based on [patterns for designing Azure Resource Manager templates](best-practices-resource-manager-design-templates.md).</span></span> <span data-ttu-id="43171-224">Azt javasoljuk, amely rendelkezik a következő sablonok hello:</span><span class="sxs-lookup"><span data-stu-id="43171-224">We recommend a design that has hello following templates:</span></span>

* <span data-ttu-id="43171-225">**Fő sablon** (azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="43171-225">**Main template** (azuredeploy.json).</span></span> <span data-ttu-id="43171-226">Ezzel az hello bemeneti paraméterek.</span><span class="sxs-lookup"><span data-stu-id="43171-226">Use for hello input parameters.</span></span>
* <span data-ttu-id="43171-227">**Megosztott erőforrások sablon**.</span><span class="sxs-lookup"><span data-stu-id="43171-227">**Shared resources template**.</span></span> <span data-ttu-id="43171-228">Használjon toodeploy megosztott erőforrásokat, amelyek más erőforrások (például: virtuális hálózat és a rendelkezésre állási készlet).</span><span class="sxs-lookup"><span data-stu-id="43171-228">Use toodeploy shared resources that all other resources use (for example, virtual network and availability sets).</span></span> <span data-ttu-id="43171-229">Használjon hello **dependsOn** kifejezés tooensure, hogy ez a sablon olyan sablon előtt van-e telepíteni.</span><span class="sxs-lookup"><span data-stu-id="43171-229">Use hello **dependsOn** expression tooensure that this template is deployed before other templates.</span></span>
* <span data-ttu-id="43171-230">**Nem kötelező erőforrások sablon**.</span><span class="sxs-lookup"><span data-stu-id="43171-230">**Optional resources template**.</span></span> <span data-ttu-id="43171-231">Használjon tooconditionally erőforrások (például jumpbox) paraméter alapján telepítheti.</span><span class="sxs-lookup"><span data-stu-id="43171-231">Use tooconditionally deploy resources based on a parameter (for example, a jumpbox).</span></span>
* <span data-ttu-id="43171-232">**Tag erőforrások sablon**.</span><span class="sxs-lookup"><span data-stu-id="43171-232">**Member resources template**.</span></span> <span data-ttu-id="43171-233">Minden belül egy alkalmazás réteget példánytípust saját konfigurációval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="43171-233">Each instance type within an application tier has its own configuration.</span></span> <span data-ttu-id="43171-234">A réteg belül különböző példány típust határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="43171-234">Within a tier, you can define different instance types.</span></span> <span data-ttu-id="43171-235">(Például hello első példányát egy fürtöt hoz létre, és a további példányt toohello meglévő fürt adja hozzá.) Minden példány rendelkezik saját központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="43171-235">(For example, hello first instance creates a cluster, and additional instances are added toohello existing cluster.) Each instance type has its own deployment template.</span></span>
* <span data-ttu-id="43171-236">**Parancsfájlok**.</span><span class="sxs-lookup"><span data-stu-id="43171-236">**Scripts**.</span></span> <span data-ttu-id="43171-237">Széles körben újrafelhasználható parancsfájlok alkalmazhatók minden példány típusa (például inicializálása és formázása további lemezek).</span><span class="sxs-lookup"><span data-stu-id="43171-237">Widely reusable scripts are applicable for each instance type (for example, initialize and format additional disks).</span></span> <span data-ttu-id="43171-238">Egy adott testreszabási célra létrehozott egyéni parancsfájlok eltérőek, attól függően hello sablonpéldány típusúvá.</span><span class="sxs-lookup"><span data-stu-id="43171-238">Custom scripts that you create for a specific customization purpose are different, based on hello instance type.</span></span>

![Beágyazott sablon](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

<span data-ttu-id="43171-240">További információkért lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="43171-240">For more information, see [Use linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="conditionally-link-toonested-templates"></a><span data-ttu-id="43171-241">Feltételes hivatkozás toonested sablonok</span><span class="sxs-lookup"><span data-stu-id="43171-241">Conditionally link toonested templates</span></span>
<span data-ttu-id="43171-242">A paraméter tooconditionally hivatkozás toonested sablonokat használhat.</span><span class="sxs-lookup"><span data-stu-id="43171-242">You can use a parameter tooconditionally link toonested templates.</span></span> <span data-ttu-id="43171-243">hello paraméter hello URI hello sablon része lesz:</span><span class="sxs-lookup"><span data-stu-id="43171-243">hello parameter becomes part of hello URI for hello template:</span></span>

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a><span data-ttu-id="43171-244">Sablon formátumban</span><span class="sxs-lookup"><span data-stu-id="43171-244">Template format</span></span>
<span data-ttu-id="43171-245">Egy jó gyakorlat toopass a sablon egy JSON-érvényesítő keresztül.</span><span class="sxs-lookup"><span data-stu-id="43171-245">It's a good practice toopass your template through a JSON validator.</span></span> <span data-ttu-id="43171-246">Egy érvényesítő segítségével távolítsa el a felesleges vessző, zárójelek és zárójelek, üzembe helyezése során hibát okozhat.</span><span class="sxs-lookup"><span data-stu-id="43171-246">A validator can help you remove extraneous commas, parentheses, and brackets that might cause an error during deployment.</span></span> <span data-ttu-id="43171-247">Próbálja [JSONLint](http://jsonlint.com/) vagy a kedvenc linter csomag szerkesztésével környezet (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="43171-247">Try [JSONLint](http://jsonlint.com/) or a linter package for your favorite editing environment (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span></span>

<span data-ttu-id="43171-248">Egyúttal egy jó ötlet tooformat a JSON jobb olvashatóság érdekében.</span><span class="sxs-lookup"><span data-stu-id="43171-248">It's also a good idea tooformat your JSON for better readability.</span></span> <span data-ttu-id="43171-249">A helyi szerkesztő JSON formázó csomag használható.</span><span class="sxs-lookup"><span data-stu-id="43171-249">You can use a JSON formatter package for your local editor.</span></span> <span data-ttu-id="43171-250">A Visual Studio tooformat hello dokumentum, nyomja le az **Ctrl + K, Ctrl + D**.</span><span class="sxs-lookup"><span data-stu-id="43171-250">In Visual Studio, tooformat hello document, press **Ctrl+K, Ctrl+D**.</span></span> <span data-ttu-id="43171-251">A Visual Studio Code, nyomja le a **Alt + Shift + F**.</span><span class="sxs-lookup"><span data-stu-id="43171-251">In Visual Studio Code, press **Alt+Shift+F**.</span></span> <span data-ttu-id="43171-252">Ha a helyi szerkesztő nem formázza hello dokumentum, egy [online formázó](https://www.bing.com/search?q=json+formatter).</span><span class="sxs-lookup"><span data-stu-id="43171-252">If your local editor doesn't format hello document, you can use an [online formatter](https://www.bing.com/search?q=json+formatter).</span></span>

## <a name="next-steps"></a><span data-ttu-id="43171-253">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43171-253">Next steps</span></span>
* <span data-ttu-id="43171-254">A megoldás a virtuális gépek újratervezni a útmutatóért lásd: [futtassa egy Windows virtuális Gépet az Azure-ban](../guidance/guidance-compute-single-vm.md) és [Linux virtuális gép futtatása az Azure-ban](../guidance/guidance-compute-single-vm-linux.md).</span><span class="sxs-lookup"><span data-stu-id="43171-254">For guidance on architecting your solution for virtual machines, see [Run a Windows VM in Azure](../guidance/guidance-compute-single-vm.md) and [Run a Linux VM in Azure](../guidance/guidance-compute-single-vm-linux.md).</span></span>
* <span data-ttu-id="43171-255">A storage-fiók beállításával kapcsolatos útmutatásért lásd: [Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="43171-255">For guidance on setting up a storage account, see [Azure Storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>
* <span data-ttu-id="43171-256">arról, hogyan vállalati erőforrás-kezelő tooeffectively toolearn-előfizetések kezelése című [Azure enterprise scaffold: részletes utasításokkal megadott előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="43171-256">toolearn about how an enterprise can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold: Prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

