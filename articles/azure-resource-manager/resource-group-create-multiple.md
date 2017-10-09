---
title: "aaaDeploy Azure-erőforrások több példánya |} Microsoft Docs"
description: "Felhasználhatja a másolási művelet és az Azure Resource Manager sablon tooiterate tömbjei többször erőforrásokat üzembe helyezi."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2017
ms.author: tomfitz
ms.openlocfilehash: a3bd42f694053317c30b639c33dc4efae41a9a9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a><span data-ttu-id="a54e6-103">Egy erőforrás vagy egy tulajdonság az Azure Resource Manager sablonokban több példányának telepítése</span><span class="sxs-lookup"><span data-stu-id="a54e6-103">Deploy multiple instances of a resource or property in Azure Resource Manager templates</span></span>
<span data-ttu-id="a54e6-104">Ez a témakör bemutatja, hogyan tooiterate az Azure Resource Manager sablon toocreate az erőforrás több példányát, vagy az erőforrás-tulajdonságok több példánya.</span><span class="sxs-lookup"><span data-stu-id="a54e6-104">This topic shows you how tooiterate in your Azure Resource Manager template toocreate multiple instances of a resource, or multiple instances of a property on a resource.</span></span>

<span data-ttu-id="a54e6-105">Ha tooadd logika tooyour sablont, amely lehetővé teszi a toospecify e erőforrás van telepítve, lásd: [feltételesen telepíteni az erőforrás](#conditionally-deploy-resource).</span><span class="sxs-lookup"><span data-stu-id="a54e6-105">If you need tooadd logic tooyour template that enables you toospecify whether a resource is deployed, see [Conditionally deploy resource](#conditionally-deploy-resource).</span></span>

## <a name="resource-iteration"></a><span data-ttu-id="a54e6-106">Erőforrás iterációs</span><span class="sxs-lookup"><span data-stu-id="a54e6-106">Resource iteration</span></span>
<span data-ttu-id="a54e6-107">toocreate erőforrástípus, több példányát adja hozzá a `copy` elem toohello erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="a54e6-107">toocreate multiple instances of a resource type, add a `copy` element toohello resource type.</span></span> <span data-ttu-id="a54e6-108">Hello másolási elemben adja meg az ismétlés és nevezze el a hurok hello számát.</span><span class="sxs-lookup"><span data-stu-id="a54e6-108">In hello copy element, you specify hello number of iterations and a name for this loop.</span></span> <span data-ttu-id="a54e6-109">hello count értékének pozitív egész számnak kell lennie, és nem haladhatja meg a 800.</span><span class="sxs-lookup"><span data-stu-id="a54e6-109">hello count value must be a positive integer and cannot exceed 800.</span></span> <span data-ttu-id="a54e6-110">Erőforrás-kezelő hello erőforrások párhuzamosan hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a54e6-110">Resource Manager creates hello resources in parallel.</span></span> <span data-ttu-id="a54e6-111">Hello sorrendben, amelyben létre, ezért nem garantált.</span><span class="sxs-lookup"><span data-stu-id="a54e6-111">Therefore, hello order in which they are created is not guaranteed.</span></span> <span data-ttu-id="a54e6-112">többször is toocreate erőforrások sorrendben, lásd: [soros másolási](#serial-copy).</span><span class="sxs-lookup"><span data-stu-id="a54e6-112">toocreate iterated resources in sequence, see [Serial copy](#serial-copy).</span></span> 

<span data-ttu-id="a54e6-113">hello erőforrás toocreate több alkalommal hajtja végre a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="a54e6-113">hello resource toocreate multiple times takes hello following format:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

<span data-ttu-id="a54e6-114">Figyelje meg, hogy hello minden erőforrás nevét tartalmazza hello `copyIndex()` függvénynek, amely hello aktuális iterációs hello hurok adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a54e6-114">Notice that hello name of each resource includes hello `copyIndex()` function, which returns hello current iteration in hello loop.</span></span> <span data-ttu-id="a54e6-115">`copyIndex()`értéke nulla.</span><span class="sxs-lookup"><span data-stu-id="a54e6-115">`copyIndex()` is zero-based.</span></span> <span data-ttu-id="a54e6-116">Igen, a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="a54e6-116">So, hello following example:</span></span>

```json
"name": "[concat('storage', copyIndex())]",
```

<span data-ttu-id="a54e6-117">Hozza létre ezeket a neveket:</span><span class="sxs-lookup"><span data-stu-id="a54e6-117">Creates these names:</span></span>

* <span data-ttu-id="a54e6-118">storage0</span><span class="sxs-lookup"><span data-stu-id="a54e6-118">storage0</span></span>
* <span data-ttu-id="a54e6-119">storage1</span><span class="sxs-lookup"><span data-stu-id="a54e6-119">storage1</span></span>
* <span data-ttu-id="a54e6-120">storage2.</span><span class="sxs-lookup"><span data-stu-id="a54e6-120">storage2.</span></span>

<span data-ttu-id="a54e6-121">toooffset hello súgóindex-értéket, akkor is adjon át egy értéket hello copyIndex() függvény.</span><span class="sxs-lookup"><span data-stu-id="a54e6-121">toooffset hello index value, you can pass a value in hello copyIndex() function.</span></span> <span data-ttu-id="a54e6-122">hello tooperform ismétlések száma továbbra is megadott hello másolási elem, de copyIndex hello értékének ellensúlyozza hello megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="a54e6-122">hello number of iterations tooperform is still specified in hello copy element, but hello value of copyIndex is offset by hello specified value.</span></span> <span data-ttu-id="a54e6-123">Igen, a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="a54e6-123">So, hello following example:</span></span>

```json
"name": "[concat('storage', copyIndex(1))]",
```

<span data-ttu-id="a54e6-124">Hozza létre ezeket a neveket:</span><span class="sxs-lookup"><span data-stu-id="a54e6-124">Creates these names:</span></span>

* <span data-ttu-id="a54e6-125">storage1</span><span class="sxs-lookup"><span data-stu-id="a54e6-125">storage1</span></span>
* <span data-ttu-id="a54e6-126">storage2</span><span class="sxs-lookup"><span data-stu-id="a54e6-126">storage2</span></span>
* <span data-ttu-id="a54e6-127">storage3</span><span class="sxs-lookup"><span data-stu-id="a54e6-127">storage3</span></span>

<span data-ttu-id="a54e6-128">hello másolási művelet akkor hasznos, ha olyan tömb, mert az Ön is iterációt hello tömb egyes elemei.</span><span class="sxs-lookup"><span data-stu-id="a54e6-128">hello copy operation is helpful when working with arrays because you can iterate through each element in hello array.</span></span> <span data-ttu-id="a54e6-129">Használjon hello `length` hello tömb toospecify hello száma iteráció során, a függvény és `copyIndex` tooretrieve hello aktuális index hello tömbben.</span><span class="sxs-lookup"><span data-stu-id="a54e6-129">Use hello `length` function on hello array toospecify hello count for iterations, and `copyIndex` tooretrieve hello current index in hello array.</span></span> <span data-ttu-id="a54e6-130">Igen, a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="a54e6-130">So, hello following example:</span></span>

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

<span data-ttu-id="a54e6-131">Hozza létre ezeket a neveket:</span><span class="sxs-lookup"><span data-stu-id="a54e6-131">Creates these names:</span></span>

* <span data-ttu-id="a54e6-132">storagecontoso</span><span class="sxs-lookup"><span data-stu-id="a54e6-132">storagecontoso</span></span>
* <span data-ttu-id="a54e6-133">storagefabrikam</span><span class="sxs-lookup"><span data-stu-id="a54e6-133">storagefabrikam</span></span>
* <span data-ttu-id="a54e6-134">storagecoho</span><span class="sxs-lookup"><span data-stu-id="a54e6-134">storagecoho</span></span>

## <a name="serial-copy"></a><span data-ttu-id="a54e6-135">Soros másolása</span><span class="sxs-lookup"><span data-stu-id="a54e6-135">Serial copy</span></span>

<span data-ttu-id="a54e6-136">Hello másolási elem toocreate használatakor erőforrástípus, erőforrás-kezelő, alapértelmezés szerint több példányát telepíti azokat a példányokat párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="a54e6-136">When you use hello copy element toocreate multiple instances of a resource type, Resource Manager, by default, deploys those instances in parallel.</span></span> <span data-ttu-id="a54e6-137">Azonban érdemes lehet toospecify adott hello feladatütemezési erőforrások telepítése.</span><span class="sxs-lookup"><span data-stu-id="a54e6-137">However, you may want toospecify that hello resources are deployed in sequence.</span></span> <span data-ttu-id="a54e6-138">Például amikor frissíti az éles környezetben, érdemes lehet toostagger hello frissíti, hogy csak bizonyos számú egyszerre frissülnek.</span><span class="sxs-lookup"><span data-stu-id="a54e6-138">For example, when updating a production environment, you may want toostagger hello updates so only a certain number are updated at any one time.</span></span>

<span data-ttu-id="a54e6-139">A Resource Manager biztosít, amelyek lehetővé teszik, hogy tooserially tulajdonságok hello másolási elem több példányok telepítése.</span><span class="sxs-lookup"><span data-stu-id="a54e6-139">Resource Manager provides properties on hello copy element that enable you tooserially deploy multiple instances.</span></span> <span data-ttu-id="a54e6-140">Hello másolási elem, állítson be `mode` túl**soros** és `batchSize` példányok toodeploy egyszerre toohello száma.</span><span class="sxs-lookup"><span data-stu-id="a54e6-140">In hello copy element, set `mode` too**serial** and `batchSize` toohello number of instances toodeploy at a time.</span></span> <span data-ttu-id="a54e6-141">A soros üzemmódban erőforrás-kezelő függőséget hoz létre a korábbi hello hurok-példány, nem indul el egy kötegelt hello előző köteg befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="a54e6-141">With serial mode, Resource Manager creates a dependency on earlier instances in hello loop, so it does not start one batch until hello previous batch completes.</span></span>

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

<span data-ttu-id="a54e6-142">hello mód tulajdonságot is fogad **párhuzamos**, amely hello alapértelmezett értéke.</span><span class="sxs-lookup"><span data-stu-id="a54e6-142">hello mode property also accepts **parallel**, which is hello default value.</span></span>

<span data-ttu-id="a54e6-143">tootest soros másolási tényleges erőforrások létrehozása nélkül használja hello sablont, amely üres beágyazott sablonok telepíti a következő:</span><span class="sxs-lookup"><span data-stu-id="a54e6-143">tootest serial copy without creating actual resources, use hello following template that deploys empty nested templates:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

<span data-ttu-id="a54e6-144">Figyelje meg, hogy a beágyazott központi telepítések hello feldolgozása a hello üzembe helyezési előzményeket, sorrendben történik.</span><span class="sxs-lookup"><span data-stu-id="a54e6-144">In hello deployment history, notice that hello nested deployments are processed in sequence.</span></span>

![soros központi telepítés](./media/resource-group-create-multiple/serial-copy.png)

<span data-ttu-id="a54e6-146">Több reális forgatókönyvek esetében a következő példa hello beágyazott sablonból Linux virtuális gép egyszerre két példányt központilag telepíti:</span><span class="sxs-lookup"><span data-stu-id="a54e6-146">For a more realistic scenario, hello following example deploys two instances at a time of a Linux VM from a nested template:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for hello Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for hello Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for hello Public IP used tooaccess hello Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="property-iteration"></a><span data-ttu-id="a54e6-147">Tulajdonság iterációs</span><span class="sxs-lookup"><span data-stu-id="a54e6-147">Property iteration</span></span>

<span data-ttu-id="a54e6-148">toocreate több érték tartozik egy erőforrás-tulajdonságok hozzáadása egy `copy` tömb hello tulajdonságok elemben.</span><span class="sxs-lookup"><span data-stu-id="a54e6-148">toocreate multiple values for a property on a resource, add a `copy` array in hello properties element.</span></span> <span data-ttu-id="a54e6-149">A tömb olyan objektumokat tartalmaz, és minden objektum rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="a54e6-149">This array contains objects, and each object has hello following properties:</span></span>

* <span data-ttu-id="a54e6-150">név – hello hello tulajdonság toocreate a több értékei</span><span class="sxs-lookup"><span data-stu-id="a54e6-150">name - hello name of hello property toocreate multiple values for</span></span>
* <span data-ttu-id="a54e6-151">megadott számú - hello értékek toocreate</span><span class="sxs-lookup"><span data-stu-id="a54e6-151">count - hello number of values toocreate</span></span>
* <span data-ttu-id="a54e6-152">bemenet – hello értékek tooassign toohello tulajdonságot tartalmazó objektum</span><span class="sxs-lookup"><span data-stu-id="a54e6-152">input - an object that contains hello values tooassign toohello property</span></span>  

<span data-ttu-id="a54e6-153">a következő példa azt mutatja meg hogyan hello tooapply `copy` toohello dataDisks tulajdonság egy virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="a54e6-153">hello following example shows how tooapply `copy` toohello dataDisks property on a virtual machine:</span></span>

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

<span data-ttu-id="a54e6-154">Figyelje meg, hogy használatakor `copyIndex` egy tulajdonság iterációs belül meg kell adnia hello iterációs hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a54e6-154">Notice that when using `copyIndex` inside a property iteration, you must provide hello name of hello iteration.</span></span> <span data-ttu-id="a54e6-155">Nincs tooprovide hello neve erőforrás iteráció használata esetén.</span><span class="sxs-lookup"><span data-stu-id="a54e6-155">You do not have tooprovide hello name when used with resource iteration.</span></span>

<span data-ttu-id="a54e6-156">Erőforrás-kezelő bővíti hello `copy` tömb üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="a54e6-156">Resource Manager expands hello `copy` array during deployment.</span></span> <span data-ttu-id="a54e6-157">hello neve hello tömb hello tulajdonság hello neve lesz.</span><span class="sxs-lookup"><span data-stu-id="a54e6-157">hello name of hello array becomes hello name of hello property.</span></span> <span data-ttu-id="a54e6-158">hello bemeneti értékek hello objektumtulajdonságokat válik.</span><span class="sxs-lookup"><span data-stu-id="a54e6-158">hello input values become hello object properties.</span></span> <span data-ttu-id="a54e6-159">telepített hello sablon válik:</span><span class="sxs-lookup"><span data-stu-id="a54e6-159">hello deployed template becomes:</span></span>

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

<span data-ttu-id="a54e6-160">Erőforrás- és tulajdonság iterációs együtt használható.</span><span class="sxs-lookup"><span data-stu-id="a54e6-160">You can use resource and property iteration together.</span></span> <span data-ttu-id="a54e6-161">Hivatkozás hello tulajdonság iterációs név szerint.</span><span class="sxs-lookup"><span data-stu-id="a54e6-161">Reference hello property iteration by name.</span></span>

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

<span data-ttu-id="a54e6-162">Csak akkor szerepelhet egy másolás elem hello tulajdonságai között az egyes erőforrások.</span><span class="sxs-lookup"><span data-stu-id="a54e6-162">You can only include one copy element in hello properties for each resource.</span></span> <span data-ttu-id="a54e6-163">egy iteráció hurok egynél több tulajdonság toospecify több objektum hello másolási tömb határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a54e6-163">toospecify an iteration loop for more than one property, define multiple objects in hello copy array.</span></span> <span data-ttu-id="a54e6-164">Az egyes objektumok külön-külön többször is van.</span><span class="sxs-lookup"><span data-stu-id="a54e6-164">Each object is iterated separately.</span></span> <span data-ttu-id="a54e6-165">Például toocreate több példányát is hello `frontendIPConfigurations` tulajdonság és hello `loadBalancingRules` tulajdonság a terheléselosztóhoz, egyetlen másolatának elemben adja meg mindkét objektumok:</span><span class="sxs-lookup"><span data-stu-id="a54e6-165">For example, toocreate multiple instances of both hello `frontendIPConfigurations` property and hello `loadBalancingRules` property on a load balancer, define both objects in a single copy element:</span></span> 

```json
{
    "name": "[variables('loadBalancerName')]",
    "type": "Microsoft.Network/loadBalancers",
    "properties": {
        "copy": [
          {
              "name": "frontendIPConfigurations",
              "count": 2,
              "input": {
                  "name": "[concat('loadBalancerFrontEnd', copyIndex('frontendIPConfigurations', 1))]",
                  "properties": {
                      "publicIPAddress": {
                          "id": "[variables(concat('publicIPAddressID', copyIndex('frontendIPConfigurations', 1)))]"
                      }
                  }
              }
          },
          {
              "name": "loadBalancingRules",
              "count": 2,
              "input": {
                  "name": "[concat('LBRuleForVIP', copyIndex('loadBalancingRules', 1))]",
                  "properties": {
                      "frontendIPConfiguration": {
                          "id": "[variables(concat('frontEndIPConfigID', copyIndex('loadBalancingRules', 1)))]"
                      },
                      "backendAddressPool": {
                          "id": "[variables('lbBackendPoolID')]"
                      },
                      "protocol": "tcp",
                      "frontendPort": "[variables(concat('frontEndPort' copyIndex('loadBalancingRules', 1))]",
                      "backendPort": "[variables(concat('backEndPort' copyIndex('loadBalancingRules', 1))]",
                      "probe": {
                          "id": "[variables('lbProbeID')]"
                      }
                  }
              }
          }
        ],
        ...
    }
}
```

## <a name="depend-on-resources-in-a-loop"></a><span data-ttu-id="a54e6-166">Ismétlődő erőforrásokat függ</span><span class="sxs-lookup"><span data-stu-id="a54e6-166">Depend on resources in a loop</span></span>
<span data-ttu-id="a54e6-167">Megadja, hogy egy erőforrás használatával központilag telepítenek után egy másik erőforrás hello `dependsOn` elemet.</span><span class="sxs-lookup"><span data-stu-id="a54e6-167">You specify that a resource is deployed after another resource by using hello `dependsOn` element.</span></span> <span data-ttu-id="a54e6-168">toodeploy egy erőforrást, amelyek elengedhetetlenek az hello erőforrások gyűjteménye, ismétlődő, adja meg a hello másolási ciklust hello dependsOn elemben hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a54e6-168">toodeploy a resource that depends on hello collection of resources in a loop, provide hello name of hello copy loop in hello dependsOn element.</span></span> <span data-ttu-id="a54e6-169">hello a következő példa bemutatja, hogyan toodeploy üzembe helyezése előtt három tárfiókok hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a54e6-169">hello following example shows how toodeploy three storage accounts before deploying hello Virtual Machine.</span></span> <span data-ttu-id="a54e6-170">hello teljes virtuálisgép-definíció nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a54e6-170">hello full Virtual Machine definition is not shown.</span></span> <span data-ttu-id="a54e6-171">Figyelje meg, hogy hello másolási elemnek túl FormatName`storagecopy` és hello dependsOn elem a virtuális gépek hello is értéke túl`storagecopy`.</span><span class="sxs-lookup"><span data-stu-id="a54e6-171">Notice that hello copy element has name set too`storagecopy` and hello dependsOn element for hello Virtual Machines is also set too`storagecopy`.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a><span data-ttu-id="a54e6-172">Hozzon létre egy gyermek erőforrás több példánya</span><span class="sxs-lookup"><span data-stu-id="a54e6-172">Create multiple instances of a child resource</span></span>
<span data-ttu-id="a54e6-173">A másolási ciklust a gyermek-erőforrások esetében nem használható.</span><span class="sxs-lookup"><span data-stu-id="a54e6-173">You cannot use a copy loop for a child resource.</span></span> <span data-ttu-id="a54e6-174">a erőforrása, amely általában meghatározni, több példánya ágyazva egy másik erőforrás toocreate, kell hozzon létre, hogy az erőforrás egy legfelső szintű erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a54e6-174">toocreate multiple instances of a resource that you typically define as nested within another resource, you must instead create that resource as a top-level resource.</span></span> <span data-ttu-id="a54e6-175">Hello kapcsolatban van-e hello szülő erőforrás hello típusa és neve tulajdonságainál megadhatja.</span><span class="sxs-lookup"><span data-stu-id="a54e6-175">You define hello relationship with hello parent resource through hello type and name properties.</span></span>

<span data-ttu-id="a54e6-176">Tegyük fel, hogy egy adat-előállító belül gyermek erőforrásként általában meghatározása a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="a54e6-176">For example, suppose you typically define a dataset as a child resource within a data factory.</span></span>

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

<span data-ttu-id="a54e6-177">toocreate adatkészletek, több példánya helyezze hello adat-előállító kívül.</span><span class="sxs-lookup"><span data-stu-id="a54e6-177">toocreate multiple instances of data sets, move it outside of hello data factory.</span></span> <span data-ttu-id="a54e6-178">hello dataset hello ugyanaz, mint hello adat-előállító szinten kell lennie, de továbbra is hello adat-előállító gyermek erőforrása.</span><span class="sxs-lookup"><span data-stu-id="a54e6-178">hello dataset must be at hello same level as hello data factory, but it is still a child resource of hello data factory.</span></span> <span data-ttu-id="a54e6-179">Megőrizheti a hello kapcsolat adatkészlet és adat-előállító hello típusa és neve tulajdonságai között.</span><span class="sxs-lookup"><span data-stu-id="a54e6-179">You preserve hello relationship between data set and data factory through hello type and name properties.</span></span> <span data-ttu-id="a54e6-180">Mivel már nem lehet következtetni hello sablonban helyéről, meg kell adnia teljesen minősített hello típus hello formátumban: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span><span class="sxs-lookup"><span data-stu-id="a54e6-180">Since type can no longer be inferred from its position in hello template, you must provide hello fully qualified type in hello format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span></span>

<span data-ttu-id="a54e6-181">egy szülő-gyermek kapcsolatban hello adat-előállító példányának tooestablish nevezze el a hello adatkészlet hello szülő erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="a54e6-181">tooestablish a parent/child relationship with an instance of hello data factory, provide a name for hello data set that includes hello parent resource name.</span></span> <span data-ttu-id="a54e6-182">Hello formátum használata: `{parent-resource-name}/{child-resource-name}`.</span><span class="sxs-lookup"><span data-stu-id="a54e6-182">Use hello format: `{parent-resource-name}/{child-resource-name}`.</span></span>  

<span data-ttu-id="a54e6-183">a következő példa azt mutatja be hello megvalósítási hello:</span><span class="sxs-lookup"><span data-stu-id="a54e6-183">hello following example shows hello implementation:</span></span>

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="conditionally-deploy-resource"></a><span data-ttu-id="a54e6-184">Erőforrás feltételesen telepítése</span><span class="sxs-lookup"><span data-stu-id="a54e6-184">Conditionally deploy resource</span></span>

<span data-ttu-id="a54e6-185">toospecify akár egy erőforráshoz van telepítve, használja az hello `condition` elemet.</span><span class="sxs-lookup"><span data-stu-id="a54e6-185">toospecify whether a resource is deployed, use hello `condition` element.</span></span> <span data-ttu-id="a54e6-186">hello érték ehhez az elemhez oldja fel a tootrue vagy HAMIS eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="a54e6-186">hello value for this element resolves tootrue or false.</span></span> <span data-ttu-id="a54e6-187">Hello értéke true, ha telepítve van a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a54e6-187">When hello value is true, hello resource is deployed.</span></span> <span data-ttu-id="a54e6-188">Ha hello értéke HAMIS, hello erőforrás nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="a54e6-188">When hello value is false, hello resource is not deployed.</span></span> <span data-ttu-id="a54e6-189">Például toospecify egy új tárfiókot van telepítve, vagy egy meglévő tárfiók használata esetén használja:</span><span class="sxs-lookup"><span data-stu-id="a54e6-189">For example, toospecify whether a new storage account is deployed or an existing storage account is used, use:</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="a54e6-190">Például egy új vagy meglévő erőforrást használ, tekintse meg a [új vagy meglévő feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="a54e6-190">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="a54e6-191">Például a jelszóval vagy SSH-kulcs toodeploy virtuális gép, [felhasználónév és SSH feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="a54e6-191">For an example of using a password or SSH key toodeploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a54e6-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a54e6-192">Next steps</span></span>
* <span data-ttu-id="a54e6-193">Ha azt szeretné, hogy a sablon hello szakaszai toolearn, [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a54e6-193">If you want toolearn about hello sections of a template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="a54e6-194">toolearn hogyan toodeploy a sablont, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="a54e6-194">toolearn how toodeploy your template, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>

