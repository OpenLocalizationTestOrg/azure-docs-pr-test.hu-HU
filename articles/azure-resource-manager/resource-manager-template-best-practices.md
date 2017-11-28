---
title: "Ajánlott eljárások a Resource Manager-sablonok létrehozásához |} Microsoft Docs"
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
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a><span data-ttu-id="d2530-103">Ajánlott eljárások az Azure Resource Manager-sablonok létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2530-103">Best practices for creating Azure Resource Manager templates</span></span>
<span data-ttu-id="d2530-104">Iránymutatás segítséget nyújtanak az Azure Resource Manager-sablonok, amelyek könnyen használható és megbízható létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d2530-104">These guidelines can help you create Azure Resource Manager templates that are reliable and easy to use.</span></span> <span data-ttu-id="d2530-105">Az útmutató csak javaslatok.</span><span class="sxs-lookup"><span data-stu-id="d2530-105">The guidelines are only suggestions.</span></span> <span data-ttu-id="d2530-106">Nincsenek követelmények, kivéve, ha az áttelepítés előtt feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="d2530-106">They are not requirements, except where noted.</span></span> <span data-ttu-id="d2530-107">A forgatókönyv a következő módszerekkel és példák az egyik egy változata lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="d2530-107">Your scenario might require a variation of one of the following approaches or examples.</span></span>

## <a name="resource-names"></a><span data-ttu-id="d2530-108">Erőforrások neve</span><span class="sxs-lookup"><span data-stu-id="d2530-108">Resource names</span></span>
<span data-ttu-id="d2530-109">Általában három típusú erőforrásnevek a Resource Manager használata:</span><span class="sxs-lookup"><span data-stu-id="d2530-109">Generally, you work with three types of resource names in Resource Manager:</span></span>

* <span data-ttu-id="d2530-110">Erőforrás neve, amely egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d2530-110">Resource names that must be unique.</span></span>
* <span data-ttu-id="d2530-111">Erőforrás neve, amely nem kell egyedinek lennie, de válassza ki, amelyek segítenek azonosítani azokat a környezet alapján erőforrás nevének.</span><span class="sxs-lookup"><span data-stu-id="d2530-111">Resource names that are not required to be unique, but you choose to provide a name that can help you identify a resource based on context.</span></span>
* <span data-ttu-id="d2530-112">Erőforrás neve, amely lehet általános.</span><span class="sxs-lookup"><span data-stu-id="d2530-112">Resource names that can be generic.</span></span>

 <span data-ttu-id="d2530-113">Erőforrás neve vonatkozó megkötésekkel kapcsolatos további információkért lásd: [Azure-erőforrások elnevezési szabályai ajánlott](../guidance/guidance-naming-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-113">For information about resource name restrictions, see [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md).</span></span>

### <a name="unique-resource-names"></a><span data-ttu-id="d2530-114">Egyedi erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="d2530-114">Unique resource names</span></span>
<span data-ttu-id="d2530-115">Egy adat-hozzáférési végponttal rendelkezik erőforrás típussal egyedi erőforrás nevét kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="d2530-115">You must provide a unique resource name for any resource type that has a data access endpoint.</span></span> <span data-ttu-id="d2530-116">Néhány gyakori erőforrástípusok esetében, amelyek egyedi nevet kell megadni a következők:</span><span class="sxs-lookup"><span data-stu-id="d2530-116">Some common resource types that require a unique name include:</span></span>

* <span data-ttu-id="d2530-117">Az Azure Storage<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="d2530-117">Azure Storage<sup>1</sup></span></span> 
* <span data-ttu-id="d2530-118">Web Apps funkció az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="d2530-118">Web Apps feature of Azure App Service</span></span>
* <span data-ttu-id="d2530-119">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d2530-119">SQL Server</span></span>
* <span data-ttu-id="d2530-120">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d2530-120">Azure Key Vault</span></span>
* <span data-ttu-id="d2530-121">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="d2530-121">Azure Redis Cache</span></span>
* <span data-ttu-id="d2530-122">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d2530-122">Azure Batch</span></span>
* <span data-ttu-id="d2530-123">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="d2530-123">Azure Traffic Manager</span></span>
* <span data-ttu-id="d2530-124">Azure Search</span><span class="sxs-lookup"><span data-stu-id="d2530-124">Azure Search</span></span>
* <span data-ttu-id="d2530-125">Az Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2530-125">Azure HDInsight</span></span>

<span data-ttu-id="d2530-126"><sup>1</sup> tárfiókneveket is kisbetűnek kell lennie, 24 karakter vagy kevesebb, és nem rendelkezik a kötőjel.</span><span class="sxs-lookup"><span data-stu-id="d2530-126"><sup>1</sup> Storage account names also must be lowercase, 24 characters or less, and not have any hyphens.</span></span>

<span data-ttu-id="d2530-127">Egy paramétert az erőforrás nevét adja meg, ha az erőforrás telepítésekor meg kell adnia egy egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="d2530-127">If you provide a parameter for a resource name, you must provide a unique name when you deploy the resource.</span></span> <span data-ttu-id="d2530-128">Másik lehetőségként létrehozhat egy változó, amely használja a [uniqueString()](resource-group-template-functions-string.md#uniquestring) nevet generálni az adott függvényt.</span><span class="sxs-lookup"><span data-stu-id="d2530-128">Optionally, you can create a variable that uses the [uniqueString()](resource-group-template-functions-string.md#uniquestring) function to generate a name.</span></span> 

<span data-ttu-id="d2530-129">Érdemes azt is egy előtagot, vagy hogy utótag a **uniqueString** eredménye.</span><span class="sxs-lookup"><span data-stu-id="d2530-129">You also might want to add a prefix or suffix to the **uniqueString** result.</span></span> <span data-ttu-id="d2530-130">Az egyedi név módosítása segítségével további könnyen azonosíthatja az erőforrás típusa a neve.</span><span class="sxs-lookup"><span data-stu-id="d2530-130">Modifying the unique name can help you more easily identify the resource type from the name.</span></span> <span data-ttu-id="d2530-131">Például egy egyedi nevet a tárfiók hozhat létre a következő változó használatával:</span><span class="sxs-lookup"><span data-stu-id="d2530-131">For example, you can generate a unique name for a storage account by using the following variable:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a><span data-ttu-id="d2530-132">Az azonosításhoz erőforrásnevek</span><span class="sxs-lookup"><span data-stu-id="d2530-132">Resource names for identification</span></span>
<span data-ttu-id="d2530-133">Egyes erőforrástípusok esetében érdemes nevét, de a nevek nem rendelkeznek egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d2530-133">Some resource types you might want to name, but their names do not have to be unique.</span></span> <span data-ttu-id="d2530-134">Ezen erőforrás esetében adja meg a nevet, amely azonosítja az erőforrás-környezetben, mind az erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="d2530-134">For these resource types, you can provide a name that identifies both the resource context and the resource type.</span></span> <span data-ttu-id="d2530-135">Adjon meg egy leíró nevet, hogy könnyebb legyen azonosítani az erőforrás tartozó erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="d2530-135">Provide a descriptive name that helps you identify the resource in a list of resources.</span></span> <span data-ttu-id="d2530-136">Ha szeretné használni a különböző központi telepítésénél a különböző erőforrás nevét, egy paraméter használható nevét:</span><span class="sxs-lookup"><span data-stu-id="d2530-136">If you need to use a different resource name for different deployments, you can use a parameter for the name:</span></span>

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

<span data-ttu-id="d2530-137">Ha nem kell egy nevet a telepítés során adhat, egy változót is használhatja:</span><span class="sxs-lookup"><span data-stu-id="d2530-137">If you do not need to pass in a name during deployment, you can use a variable:</span></span> 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

<span data-ttu-id="d2530-138">A kódolt érték is használható:</span><span class="sxs-lookup"><span data-stu-id="d2530-138">You also can use a hard-coded value:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a><span data-ttu-id="d2530-139">Általános erőforrás neve</span><span class="sxs-lookup"><span data-stu-id="d2530-139">Generic resource names</span></span>
<span data-ttu-id="d2530-140">Erőforrás esetében, amelyek többnyire keresztül érhető el egy másik erőforráscsoportban használhatja az általános neve nem változtatható a sablonban.</span><span class="sxs-lookup"><span data-stu-id="d2530-140">For resource types that you mostly access through a different resource, you can use a generic name that is hard-coded in the template.</span></span> <span data-ttu-id="d2530-141">Például beállíthatja egy szabványos, általános nevet tűzfalszabályok SQL-kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="d2530-141">For example, you can set a standard, generic name for firewall rules on a SQL server:</span></span>

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a><span data-ttu-id="d2530-142">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="d2530-142">Parameters</span></span>
<span data-ttu-id="d2530-143">Az alábbi információ segítségével esetleg megállapítható, paraméterek használata:</span><span class="sxs-lookup"><span data-stu-id="d2530-143">The following information can be helpful when you work with parameters:</span></span>

* <span data-ttu-id="d2530-144">Minimalizálása érdekében a paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="d2530-144">Minimize your use of parameters.</span></span> <span data-ttu-id="d2530-145">Amikor csak lehetséges, használjon egy változó vagy konstansérték.</span><span class="sxs-lookup"><span data-stu-id="d2530-145">Whenever possible, use a variable or a literal value.</span></span> <span data-ttu-id="d2530-146">Paraméterek csak ezek helyzetekben használhatja:</span><span class="sxs-lookup"><span data-stu-id="d2530-146">Use parameters only for these scenarios:</span></span>
   
   * <span data-ttu-id="d2530-147">Környezet (SKU, méret, kapacitás) megfelelően változatait használni kívánt beállításokat.</span><span class="sxs-lookup"><span data-stu-id="d2530-147">Settings that you want to use variations of according to environment (SKU, size, capacity).</span></span>
   * <span data-ttu-id="d2530-148">Erőforrás neve, amely könnyebbé teszi a beazonosítást szeretne megadni.</span><span class="sxs-lookup"><span data-stu-id="d2530-148">Resource names that you want to specify for easy identification.</span></span>
   * <span data-ttu-id="d2530-149">(Például egy rendszergazda felhasználóneve) más feladatok elvégzéséhez gyakran használt értékek.</span><span class="sxs-lookup"><span data-stu-id="d2530-149">Values that you use frequently to complete other tasks (such as an admin user name).</span></span>
   * <span data-ttu-id="d2530-150">A titkos kulcsokat (például a jelszavak).</span><span class="sxs-lookup"><span data-stu-id="d2530-150">Secrets (such as passwords).</span></span>
   * <span data-ttu-id="d2530-151">A szám vagy tömb erőforrástípus több példánya létrehozásakor használandó.</span><span class="sxs-lookup"><span data-stu-id="d2530-151">The number or array of values to use when you create multiple instances of a resource type.</span></span>
* <span data-ttu-id="d2530-152">-És nagybetűhasználattal teve a paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="d2530-152">Use camel case for parameter names.</span></span>
* <span data-ttu-id="d2530-153">A metaadatokban minden paraméter leírását adhatja meg:</span><span class="sxs-lookup"><span data-stu-id="d2530-153">Provide a description of every parameter in the metadata:</span></span>

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* <span data-ttu-id="d2530-154">Adja meg (kivéve a jelszavak és SSH-kulcsok) paraméterek alapértelmezett értéke:</span><span class="sxs-lookup"><span data-stu-id="d2530-154">Define default values for parameters (except for passwords and SSH keys):</span></span>
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* <span data-ttu-id="d2530-155">Használjon **SecureString** a jelszavak és a titkos kulcsok:</span><span class="sxs-lookup"><span data-stu-id="d2530-155">Use **SecureString** for all passwords and secrets:</span></span> 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* <span data-ttu-id="d2530-156">Amikor csak lehetséges, nem egy paraméter segítségével adjon meg helyet.</span><span class="sxs-lookup"><span data-stu-id="d2530-156">Whenever possible, don't use a parameter to specify location.</span></span> <span data-ttu-id="d2530-157">Ehelyett használja a **hely** az erőforráscsoport tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d2530-157">Instead, use the **location** property of the resource group.</span></span> <span data-ttu-id="d2530-158">Használatával a **resourceGroup () .location** megadó kifejezést vár, a erőforrások a sablonban szereplő erőforrások vannak telepítve az erőforráscsoport és ugyanazon a helyen:</span><span class="sxs-lookup"><span data-stu-id="d2530-158">By using the **resourceGroup().location** expression for all your resources, resources in the template are deployed in the same location as the resource group:</span></span>
   
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
   
   <span data-ttu-id="d2530-159">Ha egy erőforrás típusa támogatott helyek csak korlátozott számú, érdemes adjon meg egy érvényes helyet közvetlenül a sablonban.</span><span class="sxs-lookup"><span data-stu-id="d2530-159">If a resource type is supported in only a limited number of locations, you might want to specify a valid location directly in the template.</span></span> <span data-ttu-id="d2530-160">Használata esetén egy **hely** paraméter, a lehető legnagyobb mértékben paraméterérték megosztása erőforrásokat, amelyek valószínűleg ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="d2530-160">If you must use a **location** parameter, share that parameter value as much as possible with resources that are likely to be in the same location.</span></span> <span data-ttu-id="d2530-161">Ez minimalizálja a szám, ahányszor a felhasználó felkérést kap arra, hogy helyére vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="d2530-161">This minimizes the number of times users are asked to provide location information.</span></span>
* <span data-ttu-id="d2530-162">Kerülje a paraméter vagy változó erőforrástípus API-verzió számára.</span><span class="sxs-lookup"><span data-stu-id="d2530-162">Avoid using a parameter or variable for the API version for a resource type.</span></span> <span data-ttu-id="d2530-163">Erőforrás-tulajdonságok és értékek alapján verziószáma változhat.</span><span class="sxs-lookup"><span data-stu-id="d2530-163">Resource properties and values can vary by version number.</span></span> <span data-ttu-id="d2530-164">A kód szerkesztése az IntelliSense nem határozható meg, hogy a megfelelő sémát az API-verzió beállítása egy paraméter vagy változó.</span><span class="sxs-lookup"><span data-stu-id="d2530-164">IntelliSense in a code editor cannot determine the correct schema when the API version is set to a parameter or variable.</span></span> <span data-ttu-id="d2530-165">Ehelyett a sablonban merevlemez-kódot az API-verzió.</span><span class="sxs-lookup"><span data-stu-id="d2530-165">Instead, hard-code the API version in the template.</span></span>

## <a name="variables"></a><span data-ttu-id="d2530-166">Változók</span><span class="sxs-lookup"><span data-stu-id="d2530-166">Variables</span></span>
<span data-ttu-id="d2530-167">Az alábbi információ segítségével esetleg megállapítható, változók használata:</span><span class="sxs-lookup"><span data-stu-id="d2530-167">The following information can be helpful when you work with variables:</span></span>

* <span data-ttu-id="d2530-168">Változók használata sablonban egynél többször használni kívánt értékeket.</span><span class="sxs-lookup"><span data-stu-id="d2530-168">Use variables for values that you need to use more than once in a template.</span></span> <span data-ttu-id="d2530-169">Ha az érték csak egyszer legyen használva, kódolt értéket a sablon olvashatóbbá teszi.</span><span class="sxs-lookup"><span data-stu-id="d2530-169">If a value is used only once, a hard-coded value makes your template easier to read.</span></span>
* <span data-ttu-id="d2530-170">Nem használhatja a [hivatkozás](resource-group-template-functions-resource.md#reference) működni a **változók** a sablon szakasza.</span><span class="sxs-lookup"><span data-stu-id="d2530-170">You cannot use the [reference](resource-group-template-functions-resource.md#reference) function in the **variables** section of the template.</span></span> <span data-ttu-id="d2530-171">A **hivatkozás** függvény az értékét az erőforrás futásidejű állapot osztályból származik.</span><span class="sxs-lookup"><span data-stu-id="d2530-171">The **reference** function derives its value from the resource's runtime state.</span></span> <span data-ttu-id="d2530-172">Azonban változók, megtörténik a sablon kezdeti elemzése során.</span><span class="sxs-lookup"><span data-stu-id="d2530-172">However, variables are resolved during the initial parsing of the template.</span></span> <span data-ttu-id="d2530-173">Szerkezet értékei, amelyen a kell a **hivatkozás** működéséhez közvetlenül a **erőforrások** vagy **kimenete** a sablon szakasza.</span><span class="sxs-lookup"><span data-stu-id="d2530-173">Construct values that need the **reference** function directly in the **resources** or **outputs** section of the template.</span></span>
* <span data-ttu-id="d2530-174">Az erőforrás nevét, amely egyedinek kell lennie, a változókat tartalmazhat [erőforrásnevek](#resource-names).</span><span class="sxs-lookup"><span data-stu-id="d2530-174">Include variables for resource names that must be unique, as described in [Resource names](#resource-names).</span></span>
* <span data-ttu-id="d2530-175">Változók összetett objektumokba csoportosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d2530-175">You can group variables into complex objects.</span></span> <span data-ttu-id="d2530-176">Használja a **variable.subentry** való hivatkozáshoz egy összetett objektum érték formátuma.</span><span class="sxs-lookup"><span data-stu-id="d2530-176">Use the **variable.subentry** format to reference a value from a complex object.</span></span> <span data-ttu-id="d2530-177">Változók segítségével nyomon követheti a kapcsolódó változók.</span><span class="sxs-lookup"><span data-stu-id="d2530-177">Grouping variables can help you track related variables.</span></span> <span data-ttu-id="d2530-178">Ez növeli a sablon olvashatóság érdekében is.</span><span class="sxs-lookup"><span data-stu-id="d2530-178">It also improves readability of the template.</span></span> <span data-ttu-id="d2530-179">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="d2530-179">Here's an example:</span></span>
   
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
   > <span data-ttu-id="d2530-180">Egy összetett objektum nem tartalmazhat egy kifejezés, amely egy összetett objektumot egy értéket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d2530-180">A complex object cannot contain an expression that references a value from a complex object.</span></span> <span data-ttu-id="d2530-181">Külön változó erre a célra.</span><span class="sxs-lookup"><span data-stu-id="d2530-181">Define a separate variable for this purpose.</span></span>
   > 
   > 
   
     <span data-ttu-id="d2530-182">Speciális összetett objektumok használatával változókként, tekintse meg a [megosztani az Azure Resource Manager sablonokban állapot](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-182">For advanced examples of using complex objects as variables, see [Share state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="resources"></a><span data-ttu-id="d2530-183">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="d2530-183">Resources</span></span>
<span data-ttu-id="d2530-184">A következő információkat az erőforrásokkal való munka során lehet hasznos:</span><span class="sxs-lookup"><span data-stu-id="d2530-184">The following information can be helpful when you work with resources:</span></span>

* <span data-ttu-id="d2530-185">Adjon meg más közreműködők a erőforrás megismerése érdekében **megjegyzések** a sablonban az egyes erőforrások:</span><span class="sxs-lookup"><span data-stu-id="d2530-185">To help other contributors understand the purpose of the resource, specify **comments** for each resource in the template:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* <span data-ttu-id="d2530-186">Címkék segítségével erőforrásokat ad hozzá metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="d2530-186">You can use tags to add metadata to resources.</span></span> <span data-ttu-id="d2530-187">Metaadatok használatával adhatja hozzá az erőforrások adatait.</span><span class="sxs-lookup"><span data-stu-id="d2530-187">Use metadata to add information about your resources.</span></span> <span data-ttu-id="d2530-188">Például metaadatokra ahhoz, hogy az erőforrás számlázási adatát is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="d2530-188">For example, you can add metadata to record billing details for a resource.</span></span> <span data-ttu-id="d2530-189">További információkért lásd: [az Azure-erőforrások rendszerezése címkék használatával](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-189">For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).</span></span>
* <span data-ttu-id="d2530-190">Ha egy *nyilvános végpontot* a sablonban (például egy Azure Blob storage nyilvános végpontot), *do nem rögzített kód* a névtér.</span><span class="sxs-lookup"><span data-stu-id="d2530-190">If you use a *public endpoint* in your template (such as an Azure Blob storage public endpoint), *do not hard-code* the namespace.</span></span> <span data-ttu-id="d2530-191">Használja a **hivatkozás** függvény dinamikusan beolvasni a névteret.</span><span class="sxs-lookup"><span data-stu-id="d2530-191">Use the **reference** function to dynamically retrieve the namespace.</span></span> <span data-ttu-id="d2530-192">A sablon telepítéséhez a különböző nyilvános névtér-környezetekben a végpont a sablonban manuális módosítása nélkül használhatja ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="d2530-192">You can use this approach to deploy the template to different public namespace environments without manually changing the endpoint in the template.</span></span> <span data-ttu-id="d2530-193">Állítsa be az API-verzió Ön a sablon a tárfiók által használt verziójával megegyező verzióra:</span><span class="sxs-lookup"><span data-stu-id="d2530-193">Set the API version to the same version that you are using for the storage account in your template:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="d2530-194">Ha ugyanazt a sablont hoz létre a tárfiók van telepítve, nem kell adja meg a szolgáltató névterének neve, amikor az erőforrás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d2530-194">If the storage account is deployed in the same template that you are creating, you do not need to specify the provider namespace when you reference the resource.</span></span> <span data-ttu-id="d2530-195">Ez az a egyszerűsített szintaxist:</span><span class="sxs-lookup"><span data-stu-id="d2530-195">This is the simplified syntax:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="d2530-196">Ha a sablont más értékek, amelyek egy nyilvános névtér használatára van konfigurálva, ezek helyett megfelelően azonos **hivatkozás** függvény.</span><span class="sxs-lookup"><span data-stu-id="d2530-196">If you have other values in your template that are configured to use a public namespace, change these values to reflect the same **reference** function.</span></span> <span data-ttu-id="d2530-197">Például beállíthatja a **storageUri** a virtuális gép diagnosztikai profiljának tulajdonsága:</span><span class="sxs-lookup"><span data-stu-id="d2530-197">For example, you can set the **storageUri** property of the virtual machine diagnostics profile:</span></span>
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   <span data-ttu-id="d2530-198">Meglévő tárfiókot, amely egy másik erőforráscsoportban található is hivatkozhat:</span><span class="sxs-lookup"><span data-stu-id="d2530-198">You also can reference an existing storage account that is in a different resource group:</span></span>

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* <span data-ttu-id="d2530-199">Nyilvános IP-címek hozzárendelése a virtuális gép csak akkor, ha egy alkalmazás írja elő.</span><span class="sxs-lookup"><span data-stu-id="d2530-199">Assign public IP addresses to a virtual machine only when an application requires it.</span></span> <span data-ttu-id="d2530-200">A virtuális gép (VM) hibakereséshez vagy felügyeleti vagy felügyeleti célokra való kapcsolódáshoz használja a bejövő NAT-szabályok, a virtuális hálózati átjáró vagy egy jumpbox.</span><span class="sxs-lookup"><span data-stu-id="d2530-200">To connect to a virtual machine (VM) for debugging, or for management or administrative purposes, use inbound NAT rules, a virtual network gateway, or a jumpbox.</span></span>
   
     <span data-ttu-id="d2530-201">Virtuális gépekhez való csatlakozás kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="d2530-201">For more information about connecting to virtual machines, see:</span></span>
   
   * [<span data-ttu-id="d2530-202">Futtassa a virtuális gépek Azure-ban N szintű architektúrája</span><span class="sxs-lookup"><span data-stu-id="d2530-202">Run VMs for an N-tier architecture in Azure</span></span>](../guidance/guidance-compute-n-tier-vm.md)
   * [<span data-ttu-id="d2530-203">A WinRM-hozzáférés beállítása az Azure Resource Manager virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="d2530-203">Set up WinRM access for VMs in Azure Resource Manager</span></span>](../virtual-machines/windows/winrm.md)
   * [<span data-ttu-id="d2530-204">A virtuális gép külső hozzáférés engedélyezése az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="d2530-204">Allow external access to your VM by using the Azure portal</span></span>](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [<span data-ttu-id="d2530-205">A virtuális gép külső hozzáférés engedélyezése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d2530-205">Allow external access to your VM by using PowerShell</span></span>](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [<span data-ttu-id="d2530-206">A Linux virtuális gép külső hozzáférés engedélyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d2530-206">Allow external access to your Linux VM by using Azure CLI</span></span>](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* <span data-ttu-id="d2530-207">A **domainNameLabel** nyilvános IP-címekhez tulajdonságnak egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d2530-207">The **domainNameLabel** property for public IP addresses must be unique.</span></span> <span data-ttu-id="d2530-208">A **domainNameLabel** érték 3 és 63 karakter között kell, és kövesse a reguláris kifejezés által meghatározott szabályok: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span><span class="sxs-lookup"><span data-stu-id="d2530-208">The **domainNameLabel** value must be between 3 and 63 characters long, and follow the rules specified by this regular expression: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span></span> <span data-ttu-id="d2530-209">Mivel a **uniqueString** függvény karakterláncot hoz létre, amely 13 karakterből áll, a **dnsPrefixString** paraméter értéke legfeljebb 50 karakter hosszú lehet:</span><span class="sxs-lookup"><span data-stu-id="d2530-209">Because the **uniqueString** function generates a string that is 13 characters long, the **dnsPrefixString** parameter is limited to 50 characters:</span></span>

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* <span data-ttu-id="d2530-210">Ha jelszót ad hozzá egy egyéni parancsprogramok futtatására szolgáló bővítmény, használja a **commandToExecute** tulajdonságot a **protectedSettings** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="d2530-210">When you add a password to a custom script extension, use the **commandToExecute** property in the **protectedSettings** property:</span></span>
   
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
   > <span data-ttu-id="d2530-211">Győződjön meg arról, hogy titkos kulcsok titkosítását, ha azok paraméterként virtuális gépek és bővítmények, használja a **protectedSettings** tulajdonsága a megfelelő bővítményeket.</span><span class="sxs-lookup"><span data-stu-id="d2530-211">To ensure that secrets are encrypted when they are passed as parameters to VMs and extensions, use the **protectedSettings** property of the relevant extensions.</span></span>
   > 
   > 

## <a name="outputs"></a><span data-ttu-id="d2530-212">kimenetek</span><span class="sxs-lookup"><span data-stu-id="d2530-212">Outputs</span></span>
<span data-ttu-id="d2530-213">Egy sablon használatával nyilvános IP-címek létrehozása, ha egy **kimenete** szakaszt, amely az IP-cím és a teljesen minősített tartománynevét (FQDN) adatait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d2530-213">If you use a template to create public IP addresses, include an **outputs** section that returns details of the IP address and the fully qualified domain name (FQDN).</span></span> <span data-ttu-id="d2530-214">Egyszerűen beolvashatók a telepítést követően nyilvános IP-címek és teljes tartománynevek kimeneti értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="d2530-214">You can use output values to easily retrieve details about public IP addresses and FQDNs after deployment.</span></span> <span data-ttu-id="d2530-215">Ha az erőforrás hivatkozik, használja a létrehozásához használt API-verzió:</span><span class="sxs-lookup"><span data-stu-id="d2530-215">When you reference the resource, use the API version that you used to create it:</span></span> 

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

## <a name="single-template-vs-nested-templates"></a><span data-ttu-id="d2530-216">Egy sablon és a beágyazott sablonok</span><span class="sxs-lookup"><span data-stu-id="d2530-216">Single template vs. nested templates</span></span>
<span data-ttu-id="d2530-217">A megoldás üzembe helyezéséhez, használhatja ugyanazt a sablont, vagy egy fő sablont több beágyazott sablonokkal.</span><span class="sxs-lookup"><span data-stu-id="d2530-217">To deploy your solution, you can use either a single template or a main template with multiple nested templates.</span></span> <span data-ttu-id="d2530-218">Beágyazott sablonok olyan közös speciális forgatókönyvekhez.</span><span class="sxs-lookup"><span data-stu-id="d2530-218">Nested templates are common for more advanced scenarios.</span></span> <span data-ttu-id="d2530-219">Beágyazott sablon használatával lehetővé teszi a következő előnyökkel jár:</span><span class="sxs-lookup"><span data-stu-id="d2530-219">Using a nested template gives you the following advantages:</span></span>

* <span data-ttu-id="d2530-220">A célként megadott összetevők megoldás is lebontva.</span><span class="sxs-lookup"><span data-stu-id="d2530-220">You can break down a solution into targeted components.</span></span>
* <span data-ttu-id="d2530-221">Eltérő fő sablonok beágyazott sablonok is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="d2530-221">You can reuse nested templates with different main templates.</span></span>

<span data-ttu-id="d2530-222">Ha a beágyazott sablonok használata mellett dönt, a következő iránymutatás segítséget nyújt a sablon tervezési szabványosítására.</span><span class="sxs-lookup"><span data-stu-id="d2530-222">If you choose to use nested templates, the following guidelines can help you standardize your template design.</span></span> <span data-ttu-id="d2530-223">Ezeket az irányelveket alapuló [tervezése során az Azure Resource Manager-sablonok minták](best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-223">These guidelines are based on [patterns for designing Azure Resource Manager templates](best-practices-resource-manager-design-templates.md).</span></span> <span data-ttu-id="d2530-224">Azt javasoljuk, amely rendelkezik a következő sablonokat:</span><span class="sxs-lookup"><span data-stu-id="d2530-224">We recommend a design that has the following templates:</span></span>

* <span data-ttu-id="d2530-225">**Fő sablon** (azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="d2530-225">**Main template** (azuredeploy.json).</span></span> <span data-ttu-id="d2530-226">A bemeneti paramétereket használni.</span><span class="sxs-lookup"><span data-stu-id="d2530-226">Use for the input parameters.</span></span>
* <span data-ttu-id="d2530-227">**Megosztott erőforrások sablon**.</span><span class="sxs-lookup"><span data-stu-id="d2530-227">**Shared resources template**.</span></span> <span data-ttu-id="d2530-228">Használatával történő telepítése a megosztott erőforrásokat, amelyek más erőforrások (például: virtuális hálózat és a rendelkezésre állási készlet).</span><span class="sxs-lookup"><span data-stu-id="d2530-228">Use to deploy shared resources that all other resources use (for example, virtual network and availability sets).</span></span> <span data-ttu-id="d2530-229">Használja a **dependsOn** kifejezés, győződjön meg arról, hogy ez a sablon olyan sablon előtt van-e telepíteni.</span><span class="sxs-lookup"><span data-stu-id="d2530-229">Use the **dependsOn** expression to ensure that this template is deployed before other templates.</span></span>
* <span data-ttu-id="d2530-230">**Nem kötelező erőforrások sablon**.</span><span class="sxs-lookup"><span data-stu-id="d2530-230">**Optional resources template**.</span></span> <span data-ttu-id="d2530-231">A paraméter (például jumpbox) alapján erőforrások feltételesen telepítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="d2530-231">Use to conditionally deploy resources based on a parameter (for example, a jumpbox).</span></span>
* <span data-ttu-id="d2530-232">**Tag erőforrások sablon**.</span><span class="sxs-lookup"><span data-stu-id="d2530-232">**Member resources template**.</span></span> <span data-ttu-id="d2530-233">Minden belül egy alkalmazás réteget példánytípust saját konfigurációval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d2530-233">Each instance type within an application tier has its own configuration.</span></span> <span data-ttu-id="d2530-234">A réteg belül különböző példány típust határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="d2530-234">Within a tier, you can define different instance types.</span></span> <span data-ttu-id="d2530-235">(Például először egy fürtöt hoz létre és további példányt adja hozzá a meglévő fürtből.) Minden példány rendelkezik saját központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="d2530-235">(For example, the first instance creates a cluster, and additional instances are added to the existing cluster.) Each instance type has its own deployment template.</span></span>
* <span data-ttu-id="d2530-236">**Parancsfájlok**.</span><span class="sxs-lookup"><span data-stu-id="d2530-236">**Scripts**.</span></span> <span data-ttu-id="d2530-237">Széles körben újrafelhasználható parancsfájlok alkalmazhatók minden példány típusa (például inicializálása és formázása további lemezek).</span><span class="sxs-lookup"><span data-stu-id="d2530-237">Widely reusable scripts are applicable for each instance type (for example, initialize and format additional disks).</span></span> <span data-ttu-id="d2530-238">Egy adott testreszabási célra létrehozott egyéni parancsfájlok eltérőek, a példány típusa alapján.</span><span class="sxs-lookup"><span data-stu-id="d2530-238">Custom scripts that you create for a specific customization purpose are different, based on the instance type.</span></span>

![Beágyazott sablon](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

<span data-ttu-id="d2530-240">További információkért lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-240">For more information, see [Use linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="conditionally-link-to-nested-templates"></a><span data-ttu-id="d2530-241">Beágyazott sablonok feltételesen csatolása</span><span class="sxs-lookup"><span data-stu-id="d2530-241">Conditionally link to nested templates</span></span>
<span data-ttu-id="d2530-242">A paraméter segítségével beágyazott sablonok feltételesen kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="d2530-242">You can use a parameter to conditionally link to nested templates.</span></span> <span data-ttu-id="d2530-243">A paraméter a sablon URI-JÁNAK részévé válik:</span><span class="sxs-lookup"><span data-stu-id="d2530-243">The parameter becomes part of the URI for the template:</span></span>

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

## <a name="template-format"></a><span data-ttu-id="d2530-244">Sablon formátumban</span><span class="sxs-lookup"><span data-stu-id="d2530-244">Template format</span></span>
<span data-ttu-id="d2530-245">Ajánlott a sablon egy JSON-érvényesítő keresztül továbbítja.</span><span class="sxs-lookup"><span data-stu-id="d2530-245">It's a good practice to pass your template through a JSON validator.</span></span> <span data-ttu-id="d2530-246">Egy érvényesítő segítségével távolítsa el a felesleges vessző, zárójelek és zárójelek, üzembe helyezése során hibát okozhat.</span><span class="sxs-lookup"><span data-stu-id="d2530-246">A validator can help you remove extraneous commas, parentheses, and brackets that might cause an error during deployment.</span></span> <span data-ttu-id="d2530-247">Próbálja [JSONLint](http://jsonlint.com/) vagy a kedvenc linter csomag szerkesztésével környezet (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="d2530-247">Try [JSONLint](http://jsonlint.com/) or a linter package for your favorite editing environment (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span></span>

<span data-ttu-id="d2530-248">Akkor is érdemes jobb olvashatóság érdekében a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="d2530-248">It's also a good idea to format your JSON for better readability.</span></span> <span data-ttu-id="d2530-249">A helyi szerkesztő JSON formázó csomag használható.</span><span class="sxs-lookup"><span data-stu-id="d2530-249">You can use a JSON formatter package for your local editor.</span></span> <span data-ttu-id="d2530-250">A Visual Studio, a dokumentum formázása, nyomja le az ENTER **Ctrl + K, Ctrl + D**.</span><span class="sxs-lookup"><span data-stu-id="d2530-250">In Visual Studio, to format the document, press **Ctrl+K, Ctrl+D**.</span></span> <span data-ttu-id="d2530-251">A Visual Studio Code, nyomja le a **Alt + Shift + F**.</span><span class="sxs-lookup"><span data-stu-id="d2530-251">In Visual Studio Code, press **Alt+Shift+F**.</span></span> <span data-ttu-id="d2530-252">Ha a helyi szerkesztő nem formázza a dokumentum, egy [online formázó](https://www.bing.com/search?q=json+formatter).</span><span class="sxs-lookup"><span data-stu-id="d2530-252">If your local editor doesn't format the document, you can use an [online formatter](https://www.bing.com/search?q=json+formatter).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2530-253">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2530-253">Next steps</span></span>
* <span data-ttu-id="d2530-254">A megoldás a virtuális gépek újratervezni a útmutatóért lásd: [futtassa egy Windows virtuális Gépet az Azure-ban](../guidance/guidance-compute-single-vm.md) és [Linux virtuális gép futtatása az Azure-ban](../guidance/guidance-compute-single-vm-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-254">For guidance on architecting your solution for virtual machines, see [Run a Windows VM in Azure](../guidance/guidance-compute-single-vm.md) and [Run a Linux VM in Azure](../guidance/guidance-compute-single-vm-linux.md).</span></span>
* <span data-ttu-id="d2530-255">A storage-fiók beállításával kapcsolatos útmutatásért lásd: [Azure Storage teljesítményére és méretezhetőségére ellenőrzőlista](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-255">For guidance on setting up a storage account, see [Azure Storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>
* <span data-ttu-id="d2530-256">Vállalati használatát erőforrás-kezelő hatékonyan kezelheti az előfizetéseket, lásd: [Azure enterprise scaffold: részletes utasításokkal megadott előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="d2530-256">To learn about how an enterprise can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold: Prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

