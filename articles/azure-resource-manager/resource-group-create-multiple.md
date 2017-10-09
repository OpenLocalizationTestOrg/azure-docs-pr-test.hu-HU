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
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Egy erőforrás vagy egy tulajdonság az Azure Resource Manager sablonokban több példányának telepítése
Ez a témakör bemutatja, hogyan tooiterate az Azure Resource Manager sablon toocreate az erőforrás több példányát, vagy az erőforrás-tulajdonságok több példánya.

Ha tooadd logika tooyour sablont, amely lehetővé teszi a toospecify e erőforrás van telepítve, lásd: [feltételesen telepíteni az erőforrás](#conditionally-deploy-resource).

## <a name="resource-iteration"></a>Erőforrás iterációs
toocreate erőforrástípus, több példányát adja hozzá a `copy` elem toohello erőforrástípus. Hello másolási elemben adja meg az ismétlés és nevezze el a hurok hello számát. hello count értékének pozitív egész számnak kell lennie, és nem haladhatja meg a 800. Erőforrás-kezelő hello erőforrások párhuzamosan hoz létre. Hello sorrendben, amelyben létre, ezért nem garantált. többször is toocreate erőforrások sorrendben, lásd: [soros másolási](#serial-copy). 

hello erőforrás toocreate több alkalommal hajtja végre a következő formátumban hello:

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

Figyelje meg, hogy hello minden erőforrás nevét tartalmazza hello `copyIndex()` függvénynek, amely hello aktuális iterációs hello hurok adja vissza. `copyIndex()`értéke nulla. Igen, a következő példa hello:

```json
"name": "[concat('storage', copyIndex())]",
```

Hozza létre ezeket a neveket:

* storage0
* storage1
* storage2.

toooffset hello súgóindex-értéket, akkor is adjon át egy értéket hello copyIndex() függvény. hello tooperform ismétlések száma továbbra is megadott hello másolási elem, de copyIndex hello értékének ellensúlyozza hello megadott értéket. Igen, a következő példa hello:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Hozza létre ezeket a neveket:

* storage1
* storage2
* storage3

hello másolási művelet akkor hasznos, ha olyan tömb, mert az Ön is iterációt hello tömb egyes elemei. Használjon hello `length` hello tömb toospecify hello száma iteráció során, a függvény és `copyIndex` tooretrieve hello aktuális index hello tömbben. Igen, a következő példa hello:

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

Hozza létre ezeket a neveket:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Soros másolása

Hello másolási elem toocreate használatakor erőforrástípus, erőforrás-kezelő, alapértelmezés szerint több példányát telepíti azokat a példányokat párhuzamosan. Azonban érdemes lehet toospecify adott hello feladatütemezési erőforrások telepítése. Például amikor frissíti az éles környezetben, érdemes lehet toostagger hello frissíti, hogy csak bizonyos számú egyszerre frissülnek.

A Resource Manager biztosít, amelyek lehetővé teszik, hogy tooserially tulajdonságok hello másolási elem több példányok telepítése. Hello másolási elem, állítson be `mode` túl**soros** és `batchSize` példányok toodeploy egyszerre toohello száma. A soros üzemmódban erőforrás-kezelő függőséget hoz létre a korábbi hello hurok-példány, nem indul el egy kötegelt hello előző köteg befejezéséig.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

hello mód tulajdonságot is fogad **párhuzamos**, amely hello alapértelmezett értéke.

tootest soros másolási tényleges erőforrások létrehozása nélkül használja hello sablont, amely üres beágyazott sablonok telepíti a következő:

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

Figyelje meg, hogy a beágyazott központi telepítések hello feldolgozása a hello üzembe helyezési előzményeket, sorrendben történik.

![soros központi telepítés](./media/resource-group-create-multiple/serial-copy.png)

Több reális forgatókönyvek esetében a következő példa hello beágyazott sablonból Linux virtuális gép egyszerre két példányt központilag telepíti:

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

## <a name="property-iteration"></a>Tulajdonság iterációs

toocreate több érték tartozik egy erőforrás-tulajdonságok hozzáadása egy `copy` tömb hello tulajdonságok elemben. A tömb olyan objektumokat tartalmaz, és minden objektum rendelkezik hello következő tulajdonságai:

* név – hello hello tulajdonság toocreate a több értékei
* megadott számú - hello értékek toocreate
* bemenet – hello értékek tooassign toohello tulajdonságot tartalmazó objektum  

a következő példa azt mutatja meg hogyan hello tooapply `copy` toohello dataDisks tulajdonság egy virtuális gépen:

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

Figyelje meg, hogy használatakor `copyIndex` egy tulajdonság iterációs belül meg kell adnia hello iterációs hello nevét. Nincs tooprovide hello neve erőforrás iteráció használata esetén.

Erőforrás-kezelő bővíti hello `copy` tömb üzembe helyezése során. hello neve hello tömb hello tulajdonság hello neve lesz. hello bemeneti értékek hello objektumtulajdonságokat válik. telepített hello sablon válik:

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

Erőforrás- és tulajdonság iterációs együtt használható. Hivatkozás hello tulajdonság iterációs név szerint.

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

Csak akkor szerepelhet egy másolás elem hello tulajdonságai között az egyes erőforrások. egy iteráció hurok egynél több tulajdonság toospecify több objektum hello másolási tömb határozza meg. Az egyes objektumok külön-külön többször is van. Például toocreate több példányát is hello `frontendIPConfigurations` tulajdonság és hello `loadBalancingRules` tulajdonság a terheléselosztóhoz, egyetlen másolatának elemben adja meg mindkét objektumok: 

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

## <a name="depend-on-resources-in-a-loop"></a>Ismétlődő erőforrásokat függ
Megadja, hogy egy erőforrás használatával központilag telepítenek után egy másik erőforrás hello `dependsOn` elemet. toodeploy egy erőforrást, amelyek elengedhetetlenek az hello erőforrások gyűjteménye, ismétlődő, adja meg a hello másolási ciklust hello dependsOn elemben hello nevét. hello a következő példa bemutatja, hogyan toodeploy üzembe helyezése előtt három tárfiókok hello virtuális gép. hello teljes virtuálisgép-definíció nem jelenik meg. Figyelje meg, hogy hello másolási elemnek túl FormatName`storagecopy` és hello dependsOn elem a virtuális gépek hello is értéke túl`storagecopy`.

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

## <a name="create-multiple-instances-of-a-child-resource"></a>Hozzon létre egy gyermek erőforrás több példánya
A másolási ciklust a gyermek-erőforrások esetében nem használható. a erőforrása, amely általában meghatározni, több példánya ágyazva egy másik erőforrás toocreate, kell hozzon létre, hogy az erőforrás egy legfelső szintű erőforrás. Hello kapcsolatban van-e hello szülő erőforrás hello típusa és neve tulajdonságainál megadhatja.

Tegyük fel, hogy egy adat-előállító belül gyermek erőforrásként általában meghatározása a DataSet adatkészlet.

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

toocreate adatkészletek, több példánya helyezze hello adat-előállító kívül. hello dataset hello ugyanaz, mint hello adat-előállító szinten kell lennie, de továbbra is hello adat-előállító gyermek erőforrása. Megőrizheti a hello kapcsolat adatkészlet és adat-előállító hello típusa és neve tulajdonságai között. Mivel már nem lehet következtetni hello sablonban helyéről, meg kell adnia teljesen minősített hello típus hello formátumban: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

egy szülő-gyermek kapcsolatban hello adat-előállító példányának tooestablish nevezze el a hello adatkészlet hello szülő erőforrás nevét. Hello formátum használata: `{parent-resource-name}/{child-resource-name}`.  

a következő példa azt mutatja be hello megvalósítási hello:

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

## <a name="conditionally-deploy-resource"></a>Erőforrás feltételesen telepítése

toospecify akár egy erőforráshoz van telepítve, használja az hello `condition` elemet. hello érték ehhez az elemhez oldja fel a tootrue vagy HAMIS eredményt ad. Hello értéke true, ha telepítve van a hello erőforrás. Ha hello értéke HAMIS, hello erőforrás nincs telepítve. Például toospecify egy új tárfiókot van telepítve, vagy egy meglévő tárfiók használata esetén használja:

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

Például egy új vagy meglévő erőforrást használ, tekintse meg a [új vagy meglévő feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Például a jelszóval vagy SSH-kulcs toodeploy virtuális gép, [felhasználónév és SSH feltétel sablon](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="next-steps"></a>Következő lépések
* Ha azt szeretné, hogy a sablon hello szakaszai toolearn, [Azure Resource Manager sablonok készítése](resource-group-authoring-templates.md).
* toolearn hogyan toodeploy a sablont, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

