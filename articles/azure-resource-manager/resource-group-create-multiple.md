---
title: "Azure-erőforrások több példányának telepítése |} Microsoft Docs"
description: "Másolási művelet és a tömbök használata Azure Resource Manager sablon felépítésének több alkalommal, amikor erőforrásokat üzembe helyezi."
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
ms.openlocfilehash: ed8e3081d2b2e07938d7cf3aa5f95f6dde81bc66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a><span data-ttu-id="3a9b2-103">Egy erőforrás vagy egy tulajdonság az Azure Resource Manager sablonokban több példányának telepítése</span><span class="sxs-lookup"><span data-stu-id="3a9b2-103">Deploy multiple instances of a resource or property in Azure Resource Manager templates</span></span>
<span data-ttu-id="3a9b2-104">Ez a témakör bemutatja, hogyan felépítésének erőforrás több példányát, vagy egy tulajdonság több példányát erőforrás létrehozása az Azure Resource Manager sablonban.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-104">This topic shows you how to iterate in your Azure Resource Manager template to create multiple instances of a resource, or multiple instances of a property on a resource.</span></span>

<span data-ttu-id="3a9b2-105">Ha szeretné logika hozzáadása a sablont, amely lehetővé teszi, hogy adja meg, hogy telepítve van-e a erőforrás című [feltételesen telepíteni az erőforrás](#conditionally-deploy-resource).</span><span class="sxs-lookup"><span data-stu-id="3a9b2-105">If you need to add logic to your template that enables you to specify whether a resource is deployed, see [Conditionally deploy resource](#conditionally-deploy-resource).</span></span>

## <a name="resource-iteration"></a><span data-ttu-id="3a9b2-106">Erőforrás iterációs</span><span class="sxs-lookup"><span data-stu-id="3a9b2-106">Resource iteration</span></span>
<span data-ttu-id="3a9b2-107">Az erőforrástípus több példányt létrehozni, vegye fel a `copy` elemben, amely az erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-107">To create multiple instances of a resource type, add a `copy` element to the resource type.</span></span> <span data-ttu-id="3a9b2-108">A másolási elemben adja meg a számát ismétlési és ez a ciklus nevét.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-108">In the copy element, you specify the number of iterations and a name for this loop.</span></span> <span data-ttu-id="3a9b2-109">A count értékének pozitív egész számnak kell lennie, és nem haladhatja meg a 800.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-109">The count value must be a positive integer and cannot exceed 800.</span></span> <span data-ttu-id="3a9b2-110">Erőforrás-kezelő párhuzamosan hoz létre az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-110">Resource Manager creates the resources in parallel.</span></span> <span data-ttu-id="3a9b2-111">A sorrend, amelyben létre, ezért nem garantált.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-111">Therefore, the order in which they are created is not guaranteed.</span></span> <span data-ttu-id="3a9b2-112">Feladatütemezési többször is erőforrások létrehozásához lásd: [soros másolási](#serial-copy).</span><span class="sxs-lookup"><span data-stu-id="3a9b2-112">To create iterated resources in sequence, see [Serial copy](#serial-copy).</span></span> 

<span data-ttu-id="3a9b2-113">Az erőforrás létrehozása több alkalommal hajtja végre a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-113">The resource to create multiple times takes the following format:</span></span>

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

<span data-ttu-id="3a9b2-114">Figyelje meg, hogy mindegyik erőforrás nevét tartalmazza a `copyIndex()` függvénynek, amely az aktuális iteráció adja meg a hurok.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-114">Notice that the name of each resource includes the `copyIndex()` function, which returns the current iteration in the loop.</span></span> <span data-ttu-id="3a9b2-115">`copyIndex()`értéke nulla.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-115">`copyIndex()` is zero-based.</span></span> <span data-ttu-id="3a9b2-116">Így, az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-116">So, the following example:</span></span>

```json
"name": "[concat('storage', copyIndex())]",
```

<span data-ttu-id="3a9b2-117">Hozza létre ezeket a neveket:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-117">Creates these names:</span></span>

* <span data-ttu-id="3a9b2-118">storage0</span><span class="sxs-lookup"><span data-stu-id="3a9b2-118">storage0</span></span>
* <span data-ttu-id="3a9b2-119">storage1</span><span class="sxs-lookup"><span data-stu-id="3a9b2-119">storage1</span></span>
* <span data-ttu-id="3a9b2-120">storage2.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-120">storage2.</span></span>

<span data-ttu-id="3a9b2-121">Eltolás az értéket, adjon át egy értéket a copyIndex() függvény.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-121">To offset the index value, you can pass a value in the copyIndex() function.</span></span> <span data-ttu-id="3a9b2-122">Végrehajtásához az ismétlések száma továbbra is a másolási elem van megadva, de copyIndex értékének ellensúlyozza a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-122">The number of iterations to perform is still specified in the copy element, but the value of copyIndex is offset by the specified value.</span></span> <span data-ttu-id="3a9b2-123">Így, az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-123">So, the following example:</span></span>

```json
"name": "[concat('storage', copyIndex(1))]",
```

<span data-ttu-id="3a9b2-124">Hozza létre ezeket a neveket:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-124">Creates these names:</span></span>

* <span data-ttu-id="3a9b2-125">storage1</span><span class="sxs-lookup"><span data-stu-id="3a9b2-125">storage1</span></span>
* <span data-ttu-id="3a9b2-126">storage2</span><span class="sxs-lookup"><span data-stu-id="3a9b2-126">storage2</span></span>
* <span data-ttu-id="3a9b2-127">storage3</span><span class="sxs-lookup"><span data-stu-id="3a9b2-127">storage3</span></span>

<span data-ttu-id="3a9b2-128">A másolási művelet akkor hasznos, ha olyan tömb, mert az Ön is iterációt a tömb egyes elemei.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-128">The copy operation is helpful when working with arrays because you can iterate through each element in the array.</span></span> <span data-ttu-id="3a9b2-129">Használja a `length` a tömbben, az ismétlések számát adja meg a függvény és `copyIndex` az aktuális index a tömb beolvasása.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-129">Use the `length` function on the array to specify the count for iterations, and `copyIndex` to retrieve the current index in the array.</span></span> <span data-ttu-id="3a9b2-130">Így, az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-130">So, the following example:</span></span>

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

<span data-ttu-id="3a9b2-131">Hozza létre ezeket a neveket:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-131">Creates these names:</span></span>

* <span data-ttu-id="3a9b2-132">storagecontoso</span><span class="sxs-lookup"><span data-stu-id="3a9b2-132">storagecontoso</span></span>
* <span data-ttu-id="3a9b2-133">storagefabrikam</span><span class="sxs-lookup"><span data-stu-id="3a9b2-133">storagefabrikam</span></span>
* <span data-ttu-id="3a9b2-134">storagecoho</span><span class="sxs-lookup"><span data-stu-id="3a9b2-134">storagecoho</span></span>

## <a name="serial-copy"></a><span data-ttu-id="3a9b2-135">Soros másolása</span><span class="sxs-lookup"><span data-stu-id="3a9b2-135">Serial copy</span></span>

<span data-ttu-id="3a9b2-136">Használatakor a másolási elem az erőforrástípus, erőforrás-kezelő, alapértelmezés szerint több példány létrehozásához telepíti azokat a példányokat párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-136">When you use the copy element to create multiple instances of a resource type, Resource Manager, by default, deploys those instances in parallel.</span></span> <span data-ttu-id="3a9b2-137">Azonban érdemes lehet adja meg, hogy az erőforrások telepítése feladatütemezési.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-137">However, you may want to specify that the resources are deployed in sequence.</span></span> <span data-ttu-id="3a9b2-138">Például egy éles környezetben frissítésekor érdemes szakaszosan, a frissítések csak egy adott értéket egyszerre frissülnek.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-138">For example, when updating a production environment, you may want to stagger the updates so only a certain number are updated at any one time.</span></span>

<span data-ttu-id="3a9b2-139">A Resource Manager tulajdonságok biztosít, amelyek lehetővé teszik több példány Feladattervek telepítendő példány elemen.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-139">Resource Manager provides properties on the copy element that enable you to serially deploy multiple instances.</span></span> <span data-ttu-id="3a9b2-140">A másolási elem beállítása `mode` való **soros** és `batchSize` egyszerre telepítendő példányok száma.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-140">In the copy element, set `mode` to **serial** and `batchSize` to the number of instances to deploy at a time.</span></span> <span data-ttu-id="3a9b2-141">A soros üzemmódban erőforrás-kezelő függőséget hoz létre a korábbi példánya a hurok, nem indul el egy kötegben csak az előző köteg befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-141">With serial mode, Resource Manager creates a dependency on earlier instances in the loop, so it does not start one batch until the previous batch completes.</span></span>

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

<span data-ttu-id="3a9b2-142">A mód tulajdonságot is fogad **párhuzamos**, az alapértelmezett érték.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-142">The mode property also accepts **parallel**, which is the default value.</span></span>

<span data-ttu-id="3a9b2-143">Tényleges erőforrások létrehozása nélkül soros példány teszteléséhez a következő sablon használata, amely üres beágyazott sablonok telepíti:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-143">To test serial copy without creating actual resources, use the following template that deploys empty nested templates:</span></span>

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

<span data-ttu-id="3a9b2-144">A központi telepítés előzményei figyelje meg, hogy a beágyazott központi telepítések feldolgozása sorrendben.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-144">In the deployment history, notice that the nested deployments are processed in sequence.</span></span>

![soros központi telepítés](./media/resource-group-create-multiple/serial-copy.png)

<span data-ttu-id="3a9b2-146">Modell forgatókönyvek esetében a következő példa egy beágyazott sablonból Linux virtuális gép egyszerre két példányt központilag telepíti:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-146">For a more realistic scenario, the following example deploys two instances at a time of a Linux VM from a nested template:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
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
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
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

## <a name="property-iteration"></a><span data-ttu-id="3a9b2-147">Tulajdonság iterációs</span><span class="sxs-lookup"><span data-stu-id="3a9b2-147">Property iteration</span></span>

<span data-ttu-id="3a9b2-148">A tulajdonsághoz több érték létrehozására egy erőforrás hozzáadása egy `copy` tömb a Tulajdonságok elemben.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-148">To create multiple values for a property on a resource, add a `copy` array in the properties element.</span></span> <span data-ttu-id="3a9b2-149">A tömb olyan objektumokat tartalmaz, és az egyes objektumok tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-149">This array contains objects, and each object has the following properties:</span></span>

* <span data-ttu-id="3a9b2-150">név - létrehozása több értéket a tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="3a9b2-150">name - the name of the property to create multiple values for</span></span>
* <span data-ttu-id="3a9b2-151">szám - létrehozásához értékek száma</span><span class="sxs-lookup"><span data-stu-id="3a9b2-151">count - the number of values to create</span></span>
* <span data-ttu-id="3a9b2-152">bemenet – olyan objektum, amely a tulajdonság hozzárendelése értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-152">input - an object that contains the values to assign to the property</span></span>  

<span data-ttu-id="3a9b2-153">A következő példa bemutatja, hogyan alkalmazandó `copy` dataDisks tulajdonság egy virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-153">The following example shows how to apply `copy` to the dataDisks property on a virtual machine:</span></span>

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

<span data-ttu-id="3a9b2-154">Figyelje meg, hogy használatakor `copyIndex` egy tulajdonság iterációs belül meg kell adnia a iterációs nevét.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-154">Notice that when using `copyIndex` inside a property iteration, you must provide the name of the iteration.</span></span> <span data-ttu-id="3a9b2-155">Nem kell adnia a nevét, és erőforrás-ismétlés használatakor.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-155">You do not have to provide the name when used with resource iteration.</span></span>

<span data-ttu-id="3a9b2-156">Erőforrás-kezelő bontja ki a `copy` tömb üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-156">Resource Manager expands the `copy` array during deployment.</span></span> <span data-ttu-id="3a9b2-157">A tömb neve lesz a tulajdonságnevet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-157">The name of the array becomes the name of the property.</span></span> <span data-ttu-id="3a9b2-158">A bemeneti értékek lesz az objektum tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-158">The input values become the object properties.</span></span> <span data-ttu-id="3a9b2-159">A telepített sablon válik:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-159">The deployed template becomes:</span></span>

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

<span data-ttu-id="3a9b2-160">Erőforrás- és tulajdonság iterációs együtt használható.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-160">You can use resource and property iteration together.</span></span> <span data-ttu-id="3a9b2-161">Hivatkozás a tulajdonság iterációs név szerint.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-161">Reference the property iteration by name.</span></span>

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

<span data-ttu-id="3a9b2-162">Csak akkor szerepelhet egy másolás elem tulajdonságai között az egyes erőforrások.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-162">You can only include one copy element in the properties for each resource.</span></span> <span data-ttu-id="3a9b2-163">Egy iteráció hurok egynél több tulajdonság megadásához ad meg a másolási tömb több objektum.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-163">To specify an iteration loop for more than one property, define multiple objects in the copy array.</span></span> <span data-ttu-id="3a9b2-164">Az egyes objektumok külön-külön többször is van.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-164">Each object is iterated separately.</span></span> <span data-ttu-id="3a9b2-165">Ahhoz például, hogy hozzon létre több példányát is a `frontendIPConfigurations` tulajdonság és a `loadBalancingRules` tulajdonság a terheléselosztóhoz, egyetlen másolatának elemben adja meg mindkét objektumok:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-165">For example, to create multiple instances of both the `frontendIPConfigurations` property and the `loadBalancingRules` property on a load balancer, define both objects in a single copy element:</span></span> 

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

## <a name="depend-on-resources-in-a-loop"></a><span data-ttu-id="3a9b2-166">Ismétlődő erőforrásokat függ</span><span class="sxs-lookup"><span data-stu-id="3a9b2-166">Depend on resources in a loop</span></span>
<span data-ttu-id="3a9b2-167">Megadja, hogy egy erőforrás által központilag telepített után egy másik erőforrás használja a `dependsOn` elemet.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-167">You specify that a resource is deployed after another resource by using the `dependsOn` element.</span></span> <span data-ttu-id="3a9b2-168">Egy erőforrást, amelyek elengedhetetlenek az ismétlődő források központi telepítéséhez adja meg a másolási ciklust a dependsOn elem nevét.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-168">To deploy a resource that depends on the collection of resources in a loop, provide the name of the copy loop in the dependsOn element.</span></span> <span data-ttu-id="3a9b2-169">A következő példa bemutatja, hogyan három storage-fiókok telepítése a virtuális gép üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-169">The following example shows how to deploy three storage accounts before deploying the Virtual Machine.</span></span> <span data-ttu-id="3a9b2-170">A teljes virtuálisgép-definíció nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-170">The full Virtual Machine definition is not shown.</span></span> <span data-ttu-id="3a9b2-171">Figyelje meg, hogy rendelkezik-e a másolási elem name tulajdonsága `storagecopy` , és a dependsOn elem a virtuális gépek is `storagecopy`.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-171">Notice that the copy element has name set to `storagecopy` and the dependsOn element for the Virtual Machines is also set to `storagecopy`.</span></span>

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

## <a name="create-multiple-instances-of-a-child-resource"></a><span data-ttu-id="3a9b2-172">Hozzon létre egy gyermek erőforrás több példánya</span><span class="sxs-lookup"><span data-stu-id="3a9b2-172">Create multiple instances of a child resource</span></span>
<span data-ttu-id="3a9b2-173">A másolási ciklust a gyermek-erőforrások esetében nem használható.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-173">You cannot use a copy loop for a child resource.</span></span> <span data-ttu-id="3a9b2-174">Hozzon létre egy erőforrást, amely általában határozza meg, mint a beágyazott belül egy másik erőforrás több példánya, ehelyett hozzon létre, hogy erőforrást egy legfelső szintű erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-174">To create multiple instances of a resource that you typically define as nested within another resource, you must instead create that resource as a top-level resource.</span></span> <span data-ttu-id="3a9b2-175">Megadhatja a kapcsolat a szülő erőforrás típusa és neve tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-175">You define the relationship with the parent resource through the type and name properties.</span></span>

<span data-ttu-id="3a9b2-176">Tegyük fel, hogy egy adat-előállító belül gyermek erőforrásként általában meghatározása a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-176">For example, suppose you typically define a dataset as a child resource within a data factory.</span></span>

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

<span data-ttu-id="3a9b2-177">Adatkészletek több példány létrehozásához helyezze az adat-előállítóban kívül.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-177">To create multiple instances of data sets, move it outside of the data factory.</span></span> <span data-ttu-id="3a9b2-178">A dataset adat-előállító ugyanazon a szinten kell lennie, de továbbra is az adat-előállítóban gyermek erőforrása.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-178">The dataset must be at the same level as the data factory, but it is still a child resource of the data factory.</span></span> <span data-ttu-id="3a9b2-179">Megőrizheti a adatkészlet és adat-előállító típusa és neve tulajdonságai közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-179">You preserve the relationship between data set and data factory through the type and name properties.</span></span> <span data-ttu-id="3a9b2-180">Óta már nem lehet következtetni a sablonban helyéről, meg kell adnia a teljesen minősített típus a formátumban: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-180">Since type can no longer be inferred from its position in the template, you must provide the fully qualified type in the format: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.</span></span>

<span data-ttu-id="3a9b2-181">Az adat-előállítóban példányának a szülő-gyermek kapcsolat létrehozására, nevezze el a készlet, amely a szülő erőforrás nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-181">To establish a parent/child relationship with an instance of the data factory, provide a name for the data set that includes the parent resource name.</span></span> <span data-ttu-id="3a9b2-182">A formátumot használja: `{parent-resource-name}/{child-resource-name}`.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-182">Use the format: `{parent-resource-name}/{child-resource-name}`.</span></span>  

<span data-ttu-id="3a9b2-183">Az alábbi példa megvalósítását mutatja be:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-183">The following example shows the implementation:</span></span>

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

## <a name="conditionally-deploy-resource"></a><span data-ttu-id="3a9b2-184">Erőforrás feltételesen telepítése</span><span class="sxs-lookup"><span data-stu-id="3a9b2-184">Conditionally deploy resource</span></span>

<span data-ttu-id="3a9b2-185">Adja meg, hogy telepítve van-e a erőforrás, a `condition` elemet.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-185">To specify whether a resource is deployed, use the `condition` element.</span></span> <span data-ttu-id="3a9b2-186">Ez az elem értéke IGAZ vagy hamis oldja fel.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-186">The value for this element resolves to true or false.</span></span> <span data-ttu-id="3a9b2-187">Értéke true, ha az erőforrás van telepítve.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-187">When the value is true, the resource is deployed.</span></span> <span data-ttu-id="3a9b2-188">Ha értéke HAMIS, az erőforrás nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="3a9b2-188">When the value is false, the resource is not deployed.</span></span> <span data-ttu-id="3a9b2-189">Például adja meg, hogy egy új tárfiókot telepítve van, vagy egy meglévő tárfiókot használja, használja:</span><span class="sxs-lookup"><span data-stu-id="3a9b2-189">For example, to specify whether a new storage account is deployed or an existing storage account is used, use:</span></span>

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

<span data-ttu-id="3a9b2-190">Például egy új vagy meglévő erőforrást használ, tekintse meg a [új vagy meglévő feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="3a9b2-190">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="3a9b2-191">Például egy jelszó vagy SSH-kulcs használatával a virtuális gép telepítése, lásd: [felhasználónév és SSH feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="3a9b2-191">For an example of using a password or SSH key to deploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a9b2-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a9b2-192">Next steps</span></span>
* <span data-ttu-id="3a9b2-193">Ha azt szeretné, további információt a szakaszok egy sablon, lásd: [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3a9b2-193">If you want to learn about the sections of a template, see [Authoring Azure Resource Manager Templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="3a9b2-194">A sablon telepítéséhez, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="3a9b2-194">To learn how to deploy your template, see [Deploy an application with Azure Resource Manager Template](resource-group-template-deploy.md).</span></span>

