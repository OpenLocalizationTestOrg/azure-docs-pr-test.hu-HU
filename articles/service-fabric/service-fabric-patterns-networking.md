---
title: "az Azure Service Fabric aaaNetworking minták |} Microsoft Docs"
description: "Általános hálózati mintákat ismerteti a Service Fabric és hogyan toocreate egy fürt Azure hálózati szolgáltatások segítségével."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2017
ms.author: ryanwi
ms.openlocfilehash: 5973e3f9917076c6a36e71443ec256e0f414ff87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-networking-patterns"></a>A Service Fabric hálózati minták
Az Azure Service Fabric-fürt integrálhatja más Azure hálózati szolgáltatásokkal. Ebben a cikkben megmutatjuk, hogyan toocreate fürtök, hogy a következő funkciók használata hello:

- [Meglévő virtuális hálózathoz vagy alhálózathoz](#existingvnet)
- [Statikus nyilvános IP-cím](#staticpublicip)
- [Csak belső terheléselosztó](#internallb)
- [Belső és külső terheléselosztó](#internalexternallb)

A Service Fabric-szabványos virtuálisgép-méretezési csoportban lévő fut. Olyan funkciót, melyekkel egy virtuálisgép-méretezési csoportban lévő, használhatja a Service Fabric-fürt. hálózati szakaszait hello hello Azure Resource Manager-sablonok a virtuálisgép-méretezési csoportok és a Service Fabric esetén azonosak. Miután telepítette a meglévő virtuális hálózat tooan,-e más könnyen tooincorporate hálózati funkciók, például Azure ExpressRoute, Azure VPN Gateway, a hálózati biztonsági csoporthoz és virtuális hálózati társviszony-létesítés.

A Service Fabric rendszer más hálózati szolgáltatások egyik tulajdonsága az egyedi. Hello [Azure-portálon](https://portal.azure.com) belsőleg használt hello Service Fabric erőforrás szolgáltató toocall tooa fürt tooget csomópontok és alkalmazásokkal kapcsolatos információkat. hello Service Fabric erőforrás-szolgáltató nyilvánosan elérhető befelé toohello HTTP átjáró port (19080, alapértelmezés szerint a port) hello felügyeleti végpont igényel. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hello felügyeleti végpont toomanage a fürt által használt. hello Service Fabric erőforrás-szolgáltató is használja ezt a fürtöt, az Azure-portálon hello toodisplay port tooquery információt. 

Port 19080 hello Service Fabric erőforrás-szolgáltató nem érhető el, ha egy üzenetet, például *csomópont nem található* megjelenik hello portálon, és megjelenik a a csomópont- és alkalmazás lista üres. Ha toosee lévő hello Azure-portálon, a terheléselosztó fel kell fednie egy nyilvános IP-címet, és a hálózati biztonsági csoport engedélyeznie kell a bejövő portot 19080 adatforgalmat. A telepítő nem felel meg a követelménynek, hello Azure-portálon nem jelennek meg a fürt hello állapotát.

## <a name="templates"></a>Sablonok

Az összes Service Fabric-sablonok vannak [egyik letöltendő fájl](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip). Meg kell tudni toodeploy hello sablont,-hello a következő PowerShell-parancsok használatával. Központi telepítése hello meglévő Azure-beli virtuális hálózatra-sablon vagy hello statikus nyilvános IP-sablont, elolvashatja hello [telepítő kezdeti](#initialsetup) című szakaszát.

<a id="initialsetup"></a>
## <a name="initial-setup"></a>Kezdeti telepítés

### <a name="existing-virtual-network"></a>Meglévő virtuális hálózat

A következő példa hello, először egy meglévő virtuális hálózat neve ExistingRG-vnet, a hello **ExistingRG** erőforráscsoportot. hello alhálózati alapértelmezett neve. Ezeket az alapértelmezett erőforrásokat hello Azure portál toocreate szabványos virtuális gép (VM) használatakor jönnek létre. Sikerült létrehozni a hello virtuális hálózati és alhálózati hello virtuális gép létrehozása nélkül, de hello fő célja egy fürt tooan meglévő virtuális hálózat hozzáadása tooprovide hálózati kapcsolat tooother virtuális gépeket. Virtuális gép létrehozása hello jó példán meglévő virtuális hálózat általában használatáról. Ha a Service Fabric-fürt használja csak a belső terheléselosztók, egy nyilvános IP-cím nélkül használhatja hello a virtuális gép és a nyilvános IP-cím, egy biztonságos *mezőben jump*.

### <a name="static-public-ip-address"></a>Statikus nyilvános IP-cím

Egy statikus nyilvános IP-cím általában egy dedikált erőforrás hello VM vagy a virtuális gépek hozzá van rendelve a külön-külön kezelhető. Még lett beállítva a dedikált hálózati erőforráscsoportban (fürterőforrás-csoportként megakadályozását tooin hello Service Fabric maga). Hozzon létre egy statikus nyilvános IP-címet a hello staticIP1 nevű azonos ExistingRG erőforráscsoport hello Azure-portálon vagy a PowerShell használatával:

```powershell
PS C:\Users\user> New-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG -Location westus -AllocationMethod Static -DomainNameLabel sfnetworking

Name                     : staticIP1
ResourceGroupName        : ExistingRG
Location                 : westus
Id                       : /subscriptions/1237f4d2-3dce-1236-ad95-123f764e7123/resourceGroups/ExistingRG/providers/Microsoft.Network/publicIPAddresses/staticIP1
Etag                     : W/"fc8b0c77-1f84-455d-9930-0404ebba1b64"
ResourceGuid             : 77c26c06-c0ae-496c-9231-b1a114e08824
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Static
IpAddress                : 40.83.182.110
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : null
DnsSettings              : {
                             "DomainNameLabel": "sfnetworking",
                             "Fqdn": "sfnetworking.westus.cloudapp.azure.com"
                           }
```

### <a name="service-fabric-template"></a>A Service Fabric-sablon

A cikkben szereplő példák hello hello Service Fabric template.json használjuk. Hello szabványos portál varázsló toodownload hello sablon hello portálról is használhat, a fürt létrehozása előtt. Is használhatja hello sablonok valamelyikét hello [sablon gyűjtemény](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), például a hello [öt csomópontból Service Fabric-fürt](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a>Meglévő virtuális hálózathoz vagy alhálózathoz

1. Hello alhálózati toohello paraméternév hello meglévő alhálózat módosítása, és adja hozzá a két új paramétereket tooreference hello meglévő virtuális hálózat:

    ```
        "subnet0Name": {
                "type": "string",
                "defaultValue": "default"
            },
            "existingVNetRGName": {
                "type": "string",
                "defaultValue": "ExistingRG"
            },

            "existingVNetName": {
                "type": "string",
                "defaultValue": "ExistingRG-vnet"
            },
            /*
            "subnet0Name": {
                "type": "string",
                "defaultValue": "Subnet-0"
            },
            "subnet0Prefix": {
                "type": "string",
                "defaultValue": "10.0.0.0/24"
            },*/
    ```


2. Változás hello `vnetID` változó toopoint toohello meglévő virtuális hálózat:

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. Távolítsa el `Microsoft.Network/virtualNetworks` az erőforrásokat, így Azure nem hozzon létre egy új virtuális hálózat:

    ```
    /*{
    "apiVersion": "[variables('vNetApiVersion')]",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('virtualNetworkName')]",
    "location": "[parameters('computeLocation')]",
    "properities": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
            {
                "name": "[parameters('subnet0Name')]",
                "properties": {
                    "addressPrefix": "[parameters('subnet0Prefix')]"
                }
            }
        ]
    },
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    }
    },*/
    ```

4. Hello hello virtuális hálózatban megjegyzésbe `dependsOn` attribútumának `Microsoft.Compute/virtualMachineScaleSets`, így nem függ egy új virtuális hálózat létrehozása:

    ```
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Computer/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType0Name')]",
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        /*"[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        */
        "[Concat('Microsoft.Storage/storageAccounts/', variables('uniqueStringArray0')[0])]",

    ```

5. Hello sablon üzembe helyezése:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    A központi telepítést követően tartalmaznia kell a virtuális hálózat hello új méretezési virtuális gépeket. hello virtuális gép méretezési készlet csomóponttípus hello meglévő virtuális hálózat és alhálózat kell megjelennie. Remote Desktop Protocol (RDP) tooaccess hello virtuális Gépet, amely már szerepel a virtuális hálózati hello is használhatja, és tooping hello új méretezési virtuális gépek:

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

Egy másik példa, lásd: [, amely nem adott tooService háló](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a>Statikus nyilvános IP-cím

1. Adja hozzá a statikus IP-erőforráscsoport nevét és teljes tartománynevét (FQDN) meglévő hello hello neve paramétereinek:

    ```
    "existingStaticIPResourceGroup": {
                "type": "string"
            },
            "existingStaticIPName": {
                "type": "string"
            },
            "existingStaticIPDnsFQDN": {
                "type": "string"
    }
    ```

2. Távolítsa el a hello `dnsName` paraméter. (hello statikus IP-cím már szerepel ilyen.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. Egy változó tooreference hello meglévő statikus IP-cím hozzáadása:

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. Távolítsa el `Microsoft.Network/publicIPAddresses` az erőforrásokat, így Azure nem hozzon létre egy új IP-cím:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

5. Hello IP-címet hello megjegyzésbe `dependsOn` attribútumának `Microsoft.Network/loadBalancers`, így nem függ egy új IP-cím létrehozása:

    ```
    "apiVersion": "[variables('lbIPApiVersion')]",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB', '-', parameters('clusterName'), '-', parameters('vmNodeType0Name'))]",
    "location": "[parameters('computeLocation')]",
    /*
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', concat(parameters('lbIPName'), '-', '0'))]"
    ], */
    "properties": {
    ```

6. A hello `Microsoft.Network/loadBalancers` erőforrás, a módosítás hello `publicIPAddress` eleme `frontendIPConfigurations` tooreference hello helyett egy újonnan létrehozott egy létező statikus IP-cím:

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                "publicIPAddress": {
                                    /*"id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"*/
                                    "id": "[variables('existingStaticIP')]"
                                }
                            }
                        }
                    ],
    ```

7. A hello `Microsoft.ServiceFabric/clusters` erőforrás, a módosítás `managementEndpoint` toohello hello statikus IP-cím DNS teljes Tartományneve. Ha egy biztonságos fürtöt használ, ellenőrizze, hogy megváltoztatja *http://* túl*https://*. (Vegye figyelembe, hogy ez a lépés csak tooService Fabric-fürtök vonatkozik-e. Ha egy virtuálisgép-méretezési csoport használja, kihagyhatja ezt a lépést.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. Hello sablon üzembe helyezése:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

Telepítés után ellenőrizheti, hogy a terheléselosztó-e a kötött toohello nyilvános statikus IP-cím a hello másik erőforráscsoportban. a Service Fabric ügyfél-csatlakozási végpont hello és [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) végpont pont toohello hello statikus IP-cím DNS teljes Tartományneve.

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a>Csak belső terheléselosztó

Ebben a forgatókönyvben egy csak belső terheléselosztó hello külső terheléselosztó hello alapértelmezett Service Fabric sablon lecseréli. Hello Azure-portál és a Service Fabric erőforrás-szolgáltató hello megvalósítását lásd: hello megelőző szakasz.

1. Távolítsa el a hello `dnsName` paraméter. (Nincs rá szükség.)

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. Szükség esetén egy statikus kiosztási módszerrel használatakor is hozzáadhat egy statikus IP-cím paraméter. Ha egy dinamikus elosztási módszert használ, nem kell toodo ezt a lépést.

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. Távolítsa el `Microsoft.Network/publicIPAddresses` az erőforrásokat, így Azure nem hozzon létre egy új IP-cím:

    ```
    /*
    {
        "apiVersion": "[variables('publicIPApiVersion')]",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[concat(parameters('lbIPName'),)'-', '0')]",
        "location": "[parameters('computeLocation')]",
        "properties": {
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsName')]"
            },
            "publicIPAllocationMethod": "Dynamic"        
        },
        "tags": {
            "resourceType": "Service Fabric",
            "clusterName": "[parameters('clusterName')]"
        }
    }, */
    ```

4. Távolítsa el a hello IP-cím `dependsOn` attribútumának `Microsoft.Network/loadBalancers`, így nem függ egy új IP-cím létrehozása. Adja hozzá a virtuális hálózati hello `dependsOn` attribútumon, mert hello terheléselosztó most hello virtuális hálózati alhálózat hello függ:

    ```
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'))]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /*"[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"*/
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
    ```

5. Hello terheléselosztójának módosítása `frontendIPConfigurations` beállítása a használatával egy `publicIPAddress`, toousing alhálózat és `privateIPAddress`. `privateIPAddress`egy előre meghatározott statikus belső IP-címet használja. dinamikus IP-címnek, toouse eltávolítása hello `privateIPAddress` elemet, és módosítsa `privateIPAllocationMethod` túl**dinamikus**.

    ```
                "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /*
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                } */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
    ```

6. A hello `Microsoft.ServiceFabric/clusters` erőforrás, a módosítás `managementEndpoint` toopoint toohello belső terheléselosztói címet. Ha biztonságos-fürtöt használ, ellenőrizze, hogy megváltoztatja *http://* túl*https://*. (Vegye figyelembe, hogy ez a lépés csak tooService Fabric-fürtök vonatkozik-e. Ha egy virtuálisgép-méretezési csoport használja, kihagyhatja ezt a lépést.)

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. Hello sablon üzembe helyezése:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

A központi telepítést követően a terheléselosztó hello titkos statikus 10.0.0.250 IP-címet használja. Ha egy másik gép ugyanazon virtuális hálózatban, elvégezheti a belső toohello [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) végpont. Vegye figyelembe, hogy csatlakozik-e tooone hello csomópontok hello terheléselosztó mögött.

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a>Belső és külső terheléselosztó

Ebben a forgatókönyvben hello meglévő egycsomópontos típus külső terheléselosztó kezdődnie, és adja hozzá a belső terheléselosztók hello az azonos csomóponttípus. A háttér-port csatolt tooa háttér-címkészlet csak tooa egyetlen terheléselosztóhoz rendelhetők hozzá. Válassza ki, mely terheléselosztót kapnak az alkalmazás portok, valamint mely terheléselosztót fog rendelkezni a felügyeleti végpontok (portok 19000 és 19080). Ha hello felügyeleti végpontok hello belső terheléselosztón, tartsa szem előtt tartva hello Service Fabric-erőforrás szolgáltató korlátozások hello cikkben korábban ismertetett. Hello példában használjuk hello felügyeleti végpontok hello külső terheléselosztó maradnak. Is adjon hozzá egy port 80 alkalmazás portot, és helyezze el hello belső terheléselosztót.

A két csomóponttípus fürtben egy csomópont típus hello külső terheléselosztóhoz. hello más típusú csomópont értéke hello belső terheléselosztón. két csomóponttípus fürt, a hello portál által létrehozott két csomóponttípus sablon (Ez a két terheléselosztók) toouse hello második load balancer tooan belső terheléselosztó váltani. További információkért lásd: hello [csak belső terheléselosztó](#internallb) szakasz.

1. Adja hozzá a hello statikus belső load balancer IP-cím paraméter. (Megjegyzések kapcsolódó toousing dinamikus IP-címnek, tekintse meg a cikk korábbi szakaszaiban.)

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. Adja hozzá az alkalmazás 80-as port paramétert.

3. tooadd belső hello meglévő változók, hálózati másolja és illessze be a, és adja hozzá "-Int" toohello nevét:

    ```
    /* Add internal load balancer networking variables */
            "lbID0-Int": "[resourceId('Microsoft.Network/loadBalancers', concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal'))]",
            "lbIPConfig0-Int": "[concat(variables('lbID0-Int'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
            "lbPoolID0-Int": "[concat(variables('lbID0-Int'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
            "lbProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricGatewayProbe')]",
            "lbHttpProbeID0-Int": "[concat(variables('lbID0-Int'),'/probes/FabricHttpGatewayProbe')]",
            "lbNatPoolID0-Int": "[concat(variables('lbID0-Int'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
            /* Internal load balancer networking variables end */
    ```

4. Ha először hello portál által létrehozott sablont, amely az alkalmazás 80-as portot használja, hello alapértelmezett portálsablon hozzáadja AppPort1 (80-as port) hello külső terheléselosztóhoz. Ebben az esetben távolítsa el a AppPort1 hello külső terheléselosztó `loadBalancingRules` és mintavételek menüpontban, így hozzáadhatja azt toohello belső terheléselosztó:

    ```
    "loadBalancingRules": [
        {
            "name": "LBHttpRule",
            "properties":{
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('nt0fabricHttpGatewayPort')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[variables('lbHttpProbeID0')]"
                },
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortLBRule1",
            "properties": {
                "backendAddressPool": {
                    "id": "[variables('lbPoolID0')]"
                },
                "backendPort": "[parameters('loadBalancedAppPort1')]",
                "enableFloatingIP": "false",
                "frontendIPConfiguration": {
                    "id": "[variables('lbIPConfig0')]"            
                },
                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                "idleTimeoutInMinutes": "5",
                "probe": {
                    "id": "[concate(variables('lbID0'), '/probes/AppPortProbe1')]"
                },
                "protocol": "tcp"
            }
        }*/

    ],
    "probes": [
        {
            "name": "FabricGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricTcpGatewayPort')]",
                "protocol": "tcp"
            }
        },
        {
            "name": "FabricHttpGatewayProbe",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('nt0fabricHttpGatewayPort')]",
                "protocol": "tcp"
            }
        } /* Remove AppPort1 from hello external load balancer.
        {
            "name": "AppPortProbe1",
            "properties": {
                "intervalInSeconds": 5,
                "numberOfProbes": 2,
                "port": "[parameters('loadBalancedAppPort1')]",
                "protocol": "tcp"
            }
        } */

    ],
    "inboundNatPools": [
    ```

5. Adja hozzá egy második `Microsoft.Network/loadBalancers` erőforrás. A jelek hasonló toohello belső terheléselosztó hello létrehozott [csak belső terheléselosztó](#internallb) szakaszában, de használja hello "-Int" terheléselosztó változók, és megvalósít csak hello alkalmazás 80-as porton. Ez eltávolítja `inboundNatPools`, nyilvános terheléselosztót hello tookeep RDP végpontja. RDP hello belső terheléselosztón, helyezze `inboundNatPools` hello külső load balancer toothis belső terheléselosztó:

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and hello "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" toohello name. */
                "name": "[concat('LB','-', parameters('clusterName'),'-',parameters('vmNodeType0Name'), '-Internal')]",
                "location": "[parameters('computeLocation')]",
                "dependsOn": [
                    /* Remove public IP dependsOn, add vnet dependsOn
                    "[concat('Microsoft.Network/publicIPAddresses/',concat(parameters('lbIPName'),'-','0'))]"
                    */
                    "[concat('Microsoft.Network/virtualNetworks/',parameters('virtualNetworkName'))]"
                ],
                "properties": {
                    "frontendIPConfigurations": [
                        {
                            "name": "LoadBalancerIPConfig",
                            "properties": {
                                /* Switch from Public tooPrivate IP address
                                */
                                "publicIPAddress": {
                                    "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(parameters('lbIPName'),'-','0'))]"
                                }
                                */
                                "subnet" :{
                                    "id": "[variables('subnet0Ref')]"
                                },
                                "privateIPAddress": "[parameters('internalLBAddress')]",
                                "privateIPAllocationMethod": "Static"
                            }
                        }
                    ],
                    "backendAddressPools": [
                        {
                            "name": "LoadBalancerBEAddressPool",
                            "properties": {}
                        }
                    ],
                    "loadBalancingRules": [
                        /* Add hello AppPort rule. Be sure tooreference hello "-Int" versions of backendAddressPool, frontendIPConfiguration, and hello probe variables. */
                        {
                            "name": "AppPortLBRule1",
                            "properties": {
                                "backendAddressPool": {
                                    "id": "[variables('lbPoolID0-Int')]"
                                },
                                "backendPort": "[parameters('loadBalancedAppPort1')]",
                                "enableFloatingIP": "false",
                                "frontendIPConfiguration": {
                                    "id": "[variables('lbIPConfig0-Int')]"
                                },
                                "frontendPort": "[parameters('loadBalancedAppPort1')]",
                                "idleTimeoutInMinutes": "5",
                                "probe": {
                                    "id": "[concat(variables('lbID0-Int'),'/probes/AppPortProbe1')]"
                                },
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "probes": [
                    /* Add hello probe for hello app port. */
                    {
                            "name": "AppPortProbe1",
                            "properties": {
                                "intervalInSeconds": 5,
                                "numberOfProbes": 2,
                                "port": "[parameters('loadBalancedAppPort1')]",
                                "protocol": "tcp"
                            }
                        }
                    ],
                    "inboundNatPools": [
                    ]
                },
                "tags": {
                    "resourceType": "Service Fabric",
                    "clusterName": "[parameters('clusterName')]"
                }
            },
    ```

6. A `networkProfile` a hello `Microsoft.Compute/virtualMachineScaleSets` erőforrás, hello belső háttér-címkészlet hozzáadása:

    ```
    "loadBalancerBackendAddressPools": [
                                                        {
                                                            "id": "[variables('lbPoolID0')]"
                                                        },
                                                        {
                                                            /* Add internal BE pool */
                                                            "id": "[variables('lbPoolID0-Int')]"
                                                        }
    ],
    ```

7. Hello sablon üzembe helyezése:

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

A központi telepítést követően megtekintheti a két terheléselosztók hello erőforráscsoportban. Ha tallózással hello terheléselosztók, láthatja a hello nyilvános IP-cím és a felügyeleti végpontok (portok 19000 és 19080) hozzárendelt toohello nyilvános IP-cím. Is láthatóvá hello statikus belső IP cím és az alkalmazás végpont (80-as port) hozzárendelt toohello belső terheléselosztó. Mindkét betölteni egy terheléselosztó használata hello ugyanazon virtuálisgép-méretezési csoport háttér-készlet.

## <a name="next-steps"></a>Következő lépések
[Fürt létrehozása](service-fabric-cluster-creation-via-arm.md)
