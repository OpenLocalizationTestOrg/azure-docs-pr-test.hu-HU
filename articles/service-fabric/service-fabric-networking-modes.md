---
title: "Azure Service Fabric tárolószolgáltatások módjainak hálózati aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan toosetup hello hálózati mód választható, hogy Azure Service Fabric támogatja."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 5c5dd4c590c7698a947503cbe8ef66ff7d6b503a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-container-networking-modes"></a>A Service Fabric tároló hálózati módok

hello alapértelmezett hálózati módban érhető el hello Service Fabric fürt tárolószolgáltatások hello `nat` hálózati mód. A hello `nat` mód, egynél több tárolók szolgáltatás figyelő toohello rendelkező hálózat azonos port telepítési hibákat eredményez. Számos szolgáltatás ugyanazt a portot, a Service Fabric támogatja hello figyelő futtatásához hello `open` hálózati mód (5.7-es vagy újabb verzió). A hello `open` hálózati mód, minden tárolószolgáltatás beolvasása dinamikusan hozzárendelt IP-címnek belső így több szolgáltatások toolisten toohello ugyanazt a portot.   

Így egyetlen szolgáltatástípus egy statikus végponttal hello szolgáltatás jegyzékben meghatározott, az új szolgáltatások előfordulhat, hogy hozható létre és hello felmerülő telepítési hibákat nélkül törli `open` hálózati mód. Ehhez hasonlóan egy használható hello azonos `docker-compose.yml` több szolgáltatások létrehozásának statikus fájlt.

Dinamikusan kiosztott IP toodiscover szolgáltatások hello használata esetén nem ajánlott óta hello IP-cím módosításainak hello szolgáltatás újraindítja vagy áthelyezi tooanother csomópont. Csak a hello használata **Service Fabric-szolgáltatás** vagy hello **DNS-szolgáltatás** a szolgáltatás felderítése. 


> [!WARNING]
> Az Azure virtuális hálózat csak 4096 IP-címek összesen használható. Ebből kifolyólag hello összege hello csomópontok száma és a tároló szolgáltatáspéldány hello száma (a `open` hálózati) legfeljebb 4096 történik egy Vneten belül. Ilyen nagy sűrűségű forgatókönyvek hello `nat` hálózati mód használata ajánlott.
>

## <a name="setting-up-open-networking-mode"></a>Nyissa meg a hálózati mód beállítása

1. Hello Azure Resource Manager-sablon beállítása a DNS-szolgáltatás engedélyezésével és az IP-szolgáltató hello `fabricSettings`. 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. Állítson be hello hálózati profil szakasz tooallow több IP-címek toobe hello fürt mindegyik csomópontján konfigurálva. hello alábbi példa beállítja az egyes csomópontok öt IP-címek (így a figyelést toohello port minden csomóponton öt szolgáltatáspéldány lehet) a Windows Service Fabric-fürt számára.

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
    ```

    Linux-fürtök esetén további nyilvános IP-konfigurációt tooallow kimenő kapcsolat hozzá. hello következő kódrészlettel állít be egy Linux-fürt csomópontonként öt IP-címek:

    ```json
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "publicipaddressconfiguration": {
                              "name": "devpub",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 1)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 2)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 3)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 4)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "publicipaddressconfiguration": {
                              "name": "[concat('devpub', 5)]",
                              "properties": {
                                "idleTimeoutInMinutes": 15
                              }
                            },
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
    ```

3. A fürtök csak, állítsa be az NSG Windows hello vnet UDP/53-as port nyitása a következő értékek hello szabály:

   | Prioritás |    Név    |    Forrás      |  Cél   |   Szolgáltatás    | Műveletek |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     2000 | Custom_Dns | VirtualNetwork | VirtualNetwork | DNS (UDP/53) | Engedélyezés  |


4. Adja meg, az alkalmazás-jegyzékfájl hello minden egyes szolgáltatás hello hálózati módban `<NetworkConfig NetworkType="open">`.  hello mód `open` hello szolgáltatás egy dedikált IP-cím beolvasása eredményez. Ha egy mód nincs megadva, alapértelmezés szerint alapvető toohello `nat` mód. Ebből kifolyólag a jegyzék a példában a következő hello `NodeContainerServicePackage1` és `NodeContainerServicePackage2` minden figyelési toohello is ugyanazt a portot (mindkét szolgáltatás által figyelt `Endpoint1`).

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="open"/>
           <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="open"/>
            <PortBinding ContainerPort="8910" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
Ön szabadon kombinálhatók hálózati mód szolgáltatásban egy Windows-fürt számára az alkalmazáson belül. Ebből kifolyólag lehet néhány szolgáltatás `open` mód és az egyes `nat` hálózati mód. Ha a szolgáltatás beállítása a `nat`, hello port figyelő toomust egyedinek lennie. Különböző szolgáltatások hálózati módjainak keverése nem támogatott Linux fürtökön. 


## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, a Service Fabric által kínált módok hálózati.  

* [A Service Fabric alkalmazásmodellt.](service-fabric-application-model.md)
* [A Service Fabric service manifest erőforrások](service-fabric-application-model.md)
* [Egy Windows-tároló tooService Windows Server 2016 háló telepítése](service-fabric-get-started-containers.md)
* [Egy Docker-tároló tooService Linux háló telepítése](service-fabric-get-started-containers-linux.md)
