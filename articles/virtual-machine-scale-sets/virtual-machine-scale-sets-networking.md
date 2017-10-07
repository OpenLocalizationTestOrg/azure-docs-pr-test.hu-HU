---
title: "az Azure virtuálisgép-méretezési csoportok aaaNetworking |} Microsoft Docs"
description: "Hálózati tulajdonságok konfigurálása Azure-beli virtuálisgép-méretezési csoportok esetében."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: guybo
ms.openlocfilehash: ef3f0cfe648d2195c051a73987e654f0e15d13bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Azure-beli virtuálisgép-méretezési csoportok hálózatkezelése

Amikor telepít egy Azure virtuálisgép-méretezési hello portálon keresztül beállítani, bizonyos hálózati tulajdonságainak vannak alapértelmezésnek megfelelően, például rendelkező Azure terheléselosztó bejövő forgalmat kezelő NAT-szabályok. Ez a cikk ismerteti, hogyan néhány hello fejlettebb hálózatkezelési szolgáltatásaival, amely konfigurálható toouse állítja be.

Konfigurálhatja az Azure Resource Manager-sablonokkal a cikkben szereplő hello funkciók. Egyes szolgáltatások esetében az Azure CLI-hez és PowerShellhez is találhat példákat. Használja a parancssori felület 2.10-es vagy újabb, illetve a PowerShell 4.2.0-s vagy újabb verzióját.

## <a name="accelerated-networking"></a>Gyorsított hálózatkezelés
Azure [az elérését gyorsítja fel hálózati](../virtual-network/virtual-network-create-vm-accelerated-networking.md) egygyökerű i/o-virtualizálás (SR-IOV) tooa virtuális gép engedélyezésével javítja a hálózati teljesítmény. toouse elérését gyorsítja fel, a méretezési készlet hálózati, állítsa be a enableAcceleratedNetworking túl**igaz** a méretezési készletet networkinterfaceconfigurations tulajdonságok beállításai. Példa:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Már létező Azure Load Balancerre hivatkozó méretezési csoport létrehozása
Amikor egy méretezési hello Azure-portál használatával hoz létre, új terheléselosztó legtöbb konfigurációs beállítás jön létre. Ha a meglévő terheléselosztó hoz létre egy méretezési, amelyekre szüksége van a tooreference, ehhez parancssori felület használatával. a következő példa parancsfájl hello új terheléselosztó létrehozása, és létrehoz egy méretezési csoport, amely hivatkozik rá:
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a>Konfigurálható DNS-beállítások
Alapértelmezés szerint méretezési csoportok hello adott DNS-beállítások hello VNET és alhálózat, a létrehozásuk igénybe. Ugyanakkor a méretezési készletben közvetlenül hello DNS-beállításainak konfigurálása.
~
### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Konfigurálható DNS-kiszolgálókkal rendelkező méretezési csoport létrehozása
a méretezési CLI 2.0 használatával egyéni DNS-konfiguráció toocreate hozzáadása hello **--dns-kiszolgálók** argumentum toohello **vmss létrehozása** parancsot, és szóköz elválasztott IP-címeket. Példa:
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
tooconfigure egyéni DNS-kiszolgálók az Azure-sablon alapján, adjon hozzá egy dnsSettings tulajdonság toohello méretezési networkinterfaceconfigurations tulajdonságok szakasz. Példa:
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Konfigurálható virtuálisgép-tartománynevekkel rendelkező méretezési csoport létrehozása
egy egyéni DNS-név CLI 2.0 használó virtuális gépek méretezési toocreate hozzáadása hello **– virtuálisgép-tartománynév** argumentum toohello **vmss létrehozása** parancsot, utána pedig egy karakterlánc, amely hello tartomány nevét.

Azure-sablon alapján, tooset hello tartománynév hozzáadása egy **dnsSettings** tulajdonság toohello méretezési **networkinterfaceconfigurations tulajdonságok** szakasz. Példa:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

a következő képernyő hello hello kimenete, egy különálló virtuális gép DNS-névvel lenne: 
```
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a>Nyilvános IPv4-cím virtuális gépenként
Az Azure méretezési csoportok virtuális gépeinek általában nincs szükségük saját nyilvános IP-címre. A legtöbb esetben célszerű gazdaságos, és biztonságos tooassociate egy nyilvános IP cím tooa terhelés terheléselosztót vagy tooan különálló virtuális gép (más néven a jumpbox), amely ezután továbbítja a bejövő kapcsolatok tooscale beállított virtuális gépek, (például keresztül igény szerint bejövő NAT-szabályok).

Azonban bizonyos esetekben szükséges-méretezési csoport virtuális gépek toohave a saját nyilvános IP-cím címek. Példa játékok, amikor a konzol kell toomake egy közvetlen kapcsolat tooa felhő virtuális gépet, mely játék fizikai feldolgozási végez műveletet. Egy másik példa: Ha a virtuális gépek toomake külső kapcsolatok tooone kell egy másik egy elosztott adatbázist régiók között.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Méretezési csoport létrehozása úgy, hogy minden virtuális gép saját IP-címmel rendelkezzen
egy nyilvános IP cím tooeach rendelkező virtuális gép CLI 2.0 rendelő méretezési toocreate hozzáadása hello **– nyilvános-ip-– a virtuális gépenkénti** paraméter toohello **vmss létrehozása** parancsot. 

toocreate egy méretezési készletben, Azure-sablon alapján, ellenőrizze, hogy hello API hello Microsoft.Compute/virtualMachineScaleSets erőforrás verziója legalább **2017-03-30**, és adja hozzá a **publicIpAddressConfiguration**JSON tulajdonság toohello méretezési IP-konfigurációk szakasz. Példa:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
Példasablon: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-hello-public-ip-addresses-of-hello-virtual-machines-in-a-scale-set"></a>A méretezési hello virtuális gépek címeinek beállítása hello nyilvános IP-cím lekérdezése
toolist hello nyilvános IP-címek hozzárendelve tooscale beállított virtuális gépek CLI 2.0 használatával, használja a hello **az vmss lista-példány-nyilvános-IP-címek** parancsot.

a méretezési toolist PowerShell-lel nyilvános IP-címek, hello használata _Get-AzureRmPublicIpAddress_ parancsot. Példa:
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

Is lekérdezés hello nyilvános IP-címek közvetlenül Vezérlőpultjának hello nyilvános IP-címének konfigurációja hello erőforrás-azonosítója. Példa:
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

tooquery hello nyilvános IP-címek hozzárendelve tooscale beállított virtuális gépek hello [Azure erőforrás-kezelő](https://resources.azure.com), vagy a verziójával Azure REST API hello **2017-03-30** vagy újabb verzióját.

Tekintse meg a méretezési készletben, hello erőforrás-kezelő használatával tooview nyilvános IP-címek hello **publicipaddresses** a méretezési szakasza. Például: https://resources.azure.com/subscriptions/_saját_előfizetési_azonosító_/resourceGroups/_saját_erőforráscsoport_/providers/Microsoft.Compute/virtualMachineScaleSets/_saját_vmss_/publicipaddresses

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Példa a kimenetre:
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Több IP-cím hálózati adapterenként
Minden hálózati adapter Virtuálisgép-méretezési csoportban lévő lehet vele társított egy vagy több IP-konfigurációk tooa csatolni. Az egyes konfigurációkhoz egy magánhálózati IP-cím van hozzárendelve. Az egyes konfigurációkhoz egy nyilvános IP-cím erőforrás is hozzárendelhető. hány IP-címek is toounderstand rendelhető tooa hálózati adapter, és használhatja az Azure-előfizetéssel, hogy hány nyilvános IP-címek túl[Azure korlátozását](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Több hálózati adapter virtuális gépenként
Akkor is fel too8 hálózati adapterek, virtuális gépenként, attól függően, hogy a gép méretét. hello maximális hálózati adapterek száma gépenként érhető el hello [virtuális gép mérete cikk](../virtual-machines/windows/sizes.md). hello alábbi lehet például egy méretezési több hálózati adapter bejegyzést, és több nyilvános IP-címek egy virtuális gép hálózati profil:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a>Hálózati biztonsági csoportok virtuális gépenként
Hálózati biztonsági csoport közvetlen tooa méretezési, adja hozzá egy hivatkozást toohello hálózati illesztő konfigurációs szakasz hello skála virtuális gép beállításainak megadása lehet alkalmazni.

Példa: 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Következő lépések
Egy Azure virtuális hálózatot kapcsolatos további információkért tekintse meg túl[ebben a dokumentációban](../virtual-network/virtual-networks-overview.md).
