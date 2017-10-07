---
title: "Linux virtuálisgép-méretezési csoportok aaaAutoscale |} Microsoft Docs"
description: "A Linux virtuális gépek méretezési csoportján Azure parancssori felület használatával automatikus skálázás beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 4352b94ec2973c37bc5616e3be25bd0c9442632b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a>Automatikus méretezése a virtuálisgép-méretezési csoportban lévő Linux-gépek
Virtuálisgép-méretezési csoportok könnyítheti meg toodeploy, és egyetlen egységként azonos virtuális gépek kezeléséhez. Méretezési csoportok hyperscale alkalmazások jól méretezhető és testre szabható számítási rétegét adja meg, és a Windows platform-lemezképek, Linux platformon képek, egyéni lemezképek és bővítmények támogatják. több, lásd: toolearn [virtuális gépek méretezési készletek áttekintése](virtual-machine-scale-sets-overview.md).

Az oktatóanyag bemutatja, hogyan toocreate terjedő skálán állítsa be a Linux virtuális gépek hello Ubuntu Linux legújabb verzióját használja. hello oktatóanyag is bemutatja, hogyan tooautomatically méretezési hello gépek hello beállítása. Állítsa be, és állítsa be az Azure Resource Manager-sablon létrehozása és telepítése az Azure parancssori felület használatával méretezhetővé hello méretezési hoz létre. A sablonokról további információkat az [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) (Azure Resource Manager-sablonok készítése) című témakörben talál. toolearn méretezési készlet automatikus skálázás kapcsolatos további információkért lásd: [automatikus skálázás és virtuálisgép-skálázási készletekben](virtual-machine-scale-sets-autoscale-overview.md).

Ebben az oktatóanyagban hello következő telepít erőforrások és bővítmények:

* Microsoft.Storage/storageAccounts
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Erőforrás-kezelő erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).

Mielőtt hozzáfogna ebben az oktatóanyagban hello lépésben [hello Azure parancssori felület telepítése](../cli-install-nodejs.md).

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a>1. lépés: Az erőforráscsoportot és a storage-fiók létrehozása

1. **Jelentkezzen be Azure tooMicrosoft**  
A parancssori felület (Bash, terminál, parancssor), a kapcsoló tooResource kezelő módra, majd [jelentkezzen be a munkahelyi vagy iskolai azonosítóját](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Hajtsa végre az interaktív bejelentkezési élmény tooyour Azure-fiók hello kér.

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > Ha rendelkezik munkahelyi vagy iskolai azonosítója, és nem kéttényezős hitelesítést, használja `azure login -u` a hello azonosító toolog a egy interaktív munkamenet nélkül. Ha nem rendelkezik munkahelyi vagy iskolai azonosítója, akkor [munkahelyi vagy iskolai azonosító létrehozása a személyes Microsoft-fiókhoz tartozó](../active-directory/active-directory-users-create-azure-portal.md).
    
2. **Hozzon létre egy erőforráscsoportot**  
Összes erőforrást erőforráscsoportban telepített tooa kell lennie. Ebben az oktatóanyagban nevezze el hello erőforráscsoportot **vmsstest1**.
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. **A storage-fiók üzembe helyezés hello új erőforráscsoport**  
Ez a tárfiók hello sablon tárolására. Hozzon létre egy tárfiókot, nevű **vmsstestsa**.
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-hello-template"></a>2. lépés: Hello sablon létrehozása
Az Azure Resource Manager-sablon, toodeploy lehetővé teszi, és Azure-erőforrások együtt kezelheti a JSON-leírásuk hello erőforrások és a társított telepítési paramétereket.

1. A kedvenc szerkesztő hozzon létre VMSSTemplate.json hello fájlt, és hello kezdeti JSON struktúrában toosupport hello sablon hozzáadása.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Paraméterek nem mindig szükséges, de biztosítanak egy módon tooinput értékek hello sablon telepítésekor. Adja hozzá ezeket a paramétereket, hogy hozzáadta a toohello sablon hello paraméterek szülő elem alatt.

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * Hello külön virtuális gépre, amelyik használt tooaccess hello gépek hello méretezési csoportban lévő nevét.
   * Hello sablon tárolására hello storage-fiók nevét.
   * hello méretezési csoportban lévő virtuális gépek tooinitially példányainak száma hello hozzon létre.
   * A név és jelszó hello rendszergazdai fiókjának hello virtuális gépeken.
   * Állítsa be a nevének előtagját toosupport hello méretezési létrehozott hello erőforrások.

3. A sablon toospecify értékek, amelyek gyakran változhat a változók használhatók, vagy toobe igénylő értékek paraméterértékek kombinációja alapján létrehozott. Adja hozzá ezeket a változókat, hogy hozzáadta a toohello sablon hello változók szülő elem alatt.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * DNS-nevek hello által használt hálózati illesztőt.
   * hello IP-címet nevének és a virtuális hálózati hello és-alhálózatok előtagok.
   * hello nevek és hello virtuális hálózat, azonosítók betölteni a terheléselosztó és a hálózati adapterek.
   * Hello fiókok hello gépek hello méretezési csoportban lévő társított tárfiókok neve.
   * Hello hello virtuális gépeken telepített diagnosztika extension beállításait. Hello diagnosztika bővítmény kapcsolatos további információkért lásd: [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Vegyen fel hello tárolási fiók erőforrást toohello sablon hozzáadásának hello erőforrások szülő elem alatt. Ez a sablon használ egy hurok toocreate hello öt tárfiókok ajánlott hello operációs rendszer lemezek és diagnosztikai adatok tárolására. E fiókok csoportja too100 virtuális gépek, amelyek hello aktuális maximális méretezési csoportban lévő képes támogatni. Minden tárfiók megadott hello paraméterek hello sablon hello utótaggal együtt hello változók definiált levél jelzéssel neve.
   
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Hello virtuális hálózati erőforrás hozzáadása. További információkért lásd: [hálózati erőforrás-szolgáltató](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Adja hozzá a hello nyilvános IP-cím erőforrás hello által használt terheléselosztó és a hálózati kapcsolat betöltése.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. Adja hozzá a hello méretezési készlet által használt hello terheléselosztó erőforrást. További információkért lásd: [Azure Resource Manager támogatása a terheléselosztó](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 22
            }
          }
        ]
      }
    },
    ```

8. Vegyen fel hello hálózati illesztő erőforrást hello különálló virtuális gép által használt. Méretezési csoportban lévő gépek nem nyilvános IP-címnek keresztül érhető el, mert egy különálló virtuális gép jön létre hello azonos virtuális hálózati tooremotely hozzáférés hello gépek.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. Adja hozzá azonos hálózati hello méretezési beállított hello hello különálló virtuális gép.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. Hello virtuálisgép-méretezési csoport erőforrás hozzáadása, és adja meg a hello diagnosztika-bővítménnyel, amely hello méretezési csoportban lévő összes virtuális gép telepítve van. Ehhez az erőforráshoz hello beállításainak jelentős része hasonlóak a hello virtuálisgép-erőforrást. hello fő különbségek a következők: hello kapacitás elem hello méretezési csoportban lévő virtuális gépek hello számát megadó és upgradePolicy, amely meghatározza, hogyan frissítések válnak, toovirtual gépek. hello méretezési készlet nem jön létre az összes hello tárfiók létrehozásáig hello dependsOn elemmel megadottak szerint.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Vegyen fel hello autoscaleSettings erőforrást, amely meghatározza, hogyan hello méretezési processzor kihasználtsága hello gépek hello beállítása alapján állítja be.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor\\PercentProcessorTime",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```
    
    Ebben az oktatóanyagban fontosak a ezeket az értékeket:
    
    * **metricName**  
    Ez az érték azonos a hello wadperfcounter változóban meghatározott hello teljesítményszámláló van hello. Használja ezt a változót, hello diagnosztika bővítmény gyűjt hello **Processor\PercentProcessorTime** számláló.
    
    * **metricResourceUri**  
    Ez az érték hello erőforrás-azonosítót a virtuálisgép-méretezési csoport hello.
    
    * **időkeretben vannak**  
    Ez az érték hello metrikák összegyűjtött hello granularitása. A sablon értéke tooone perc.
    
    * **statisztika**  
    Ez az érték határozza meg, hogyan hello adatok gyűjtése le kombinált tooaccommodate hello automatikus skálázás művelet. hello lehetséges értékek a következők: átlagos, Min, Max. Ez a sablon a hello átlagos teljes CPU-használat hello virtuális gépek gyűjti.

    * **Az időtartomány értékének**  
    Ez az érték, amely hello amelyben példány adatgyűjtés. 5 perc és 12 óra között kell lennie.
    
    * **timeAggregation**  
    az érték szabja meg, hogyan hello adatokat gyűjtött össze kell vonni adott idő alatt. hello alapértelmezett érték átlaga. hello lehetséges értékek a következők: átlagos, Minimum, Maximum, utolsó, összesen, száma.
    
    * **operátor**  
    Ez az érték használt toocompare hello metrika adatok és hello küszöbérték, hello operátor. hello lehetséges értékek a következők: egyenlő NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **küszöbérték**  
    Ez az érték hello skálázási műveletet váltja ki. Ez a sablon a gépeket adnak toohello méretezési készletben, ha hello átlagos CPU-használat hello készlet gépek között több mint 50 %.
    
    * **iránya**  
    Ez az érték határozza meg a hello hello küszöbérték érhető el, amikor végrehajtandó műveletet. hello lehetséges értékei növekedése vagy csökkenése. Ez a sablon a hello hello méretezési csoportban lévő virtuális gépek száma nő, amikor több mint 50 %-a meghatározott hello időkerete hello küszöbértéket.

    * **típusa**  
    Ez az érték egy végrehajtandó műveletet kell végrehajtani, és be kell állítani tooChangeCount hello típusú.
    
    * **érték**  
    Ez az érték hello hozzáadásakor vagy eltávolításakor hello méretezési csoport a virtuális gépek száma. Ez az érték 1 vagy nagyobb lehet. hello alapértelmezett értéke 1. A sablonban lévő hello méretezési gépek hello száma növekszik helyezésekor által beállított 1 hello küszöbérték elérése.

    * **cooldown**  
    Ez az érték érték hello idő toowait óta hello utolsó méretezési művelet előtt hello a következő lépéseket. Ez az érték egy percet, és egy hét között kell lennie.

12. Hello sablonfájl mentési.    

## <a name="step-3-upload-hello-template-toostorage"></a>3. lépés: Hello sablon toostorage feltöltése
hello sablon mindaddig, amíg hello nevét és az 1. lépésben létrehozott hello tárfiók elsődleges kulcs ismeri fel kell tölteni.

1. A parancssori felület (Bash, terminál, parancssor) futtassa az alábbi parancsokat tooset hello környezeti változók szükséges tooaccess hello storage-fiók:

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    Hello kulcs kaphat hello kulcs ikonra kattintva, amikor megtekintik hello tárolási fiók erőforrás hello Azure-portálon. Ha egy Windows parancssori ablakba, írja be a **beállítása** exportálása helyett.

2. Az hello sablon tárolására alkalmas hello tároló létrehozása.
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. Töltse fel a hello sablon fájl toohello új tárolót.
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-hello-template"></a>4. lépés: Hello sablon üzembe helyezése
Most, hogy létrehozta a hello sablon, megkezdheti a hello erőforrásokat üzembe helyezi. A parancs toostart hello folyamat használja:

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

Amikor lenyomja az adja meg, a hozzárendelt hello változók felszólító tooprovide értékei lesznek. Adja meg ezeket az értékeket:

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

Mintegy 15 percre leáll az összes hello erőforrások toosuccessfully telepíteni kell vennie.

> [!NOTE]
> Hello portal képességét toodeploy hello erőforrásokat is használhatja. A hivatkozás: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>


## <a name="step-5-monitor-resources"></a>5. lépés: A figyelő erőforrások
Virtuálisgép-méretezési csoportok ezekkel a módszerekkel kapcsolatos információkat kaphat:

* Azure-portálon hello – jelenleg korlátozott mennyiségű információt hello portál használatával kaphat.

* Hello [Azure erőforrás-kezelő](https://resources.azure.com/) -Ez az eszköz felderítését lehetővé tevő hello aktuális állapotát, a méretezési legjobb hello. Hajtsa végre ezt az elérési utat, és megtekintheti az hello példányait tartalmazó nézetet hello méretezési beállítása létrehozott:
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* Az Azure CLI - használja a parancs tooget néhány információt:

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* Csatlakoztassa a toohello jumpbox virtuális gépet, ugyanúgy, mint bármely más gép üzembe helyezése, és ezután távolról elérheti hello virtuális gépek hello méretezési készlet toomonitor az egyes folyamatok.

> [!NOTE]
> A teljes REST API-t a méretezési készlet vonatkozó adatok találhatók [virtuálisgép-skálázási készletekben](https://msdn.microsoft.com/library/mt589023.aspx).

## <a name="step-6-remove-hello-resources"></a>6. lépés: Hello erőforrások eltávolítása
Mivel van szó, az Azure-ban használt erőforrásokhoz, még mindig egy célszerű toodelete erőforrásokat, amelyek már nem szükséges. Nincs szükség a toodelete az egyes erőforrások elkülönítve egy erőforráscsoportot. Hello erőforráscsoport törlése és a hozzá tartozó összes erőforrásnak automatikusan törlődnek.

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a>Következő lépések
* Példák figyelési szolgáltatásokat az Azure-figyelő található [figyelő Azure platformfüggetlen parancssori felület gyors start minták](../monitoring-and-diagnostics/insights-cli-samples.md)
* További tudnivalók az értesítési szolgáltatások [automatikus skálázási műveletek toosend e-mailek és a webhook riasztási értesítések használata az Azure-figyelő](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Ismerje meg, hogyan túl[használata auditnaplókat toosend e-mailek és a webhook riasztási értesítések az Azure-figyelő](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
* Tekintse meg a hello [automatikus skálázás bemutató alkalmazás Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sablont, amely beállítja a Python/bottle app tooexercise hello az automatikus skálázás működésének virtuálisgép-skálázási készletekben.

