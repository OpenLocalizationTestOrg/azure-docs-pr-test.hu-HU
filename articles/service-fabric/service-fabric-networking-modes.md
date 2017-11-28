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
# <a name="service-fabric-container-networking-modes"></a><span data-ttu-id="4ecf6-103">A Service Fabric tároló hálózati módok</span><span class="sxs-lookup"><span data-stu-id="4ecf6-103">Service Fabric container networking modes</span></span>

<span data-ttu-id="4ecf6-104">hello alapértelmezett hálózati módban érhető el hello Service Fabric fürt tárolószolgáltatások hello `nat` hálózati mód.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-104">hello default networking mode offered in hello Service Fabric cluster for container services is hello `nat` networking mode.</span></span> <span data-ttu-id="4ecf6-105">A hello `nat` mód, egynél több tárolók szolgáltatás figyelő toohello rendelkező hálózat azonos port telepítési hibákat eredményez.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-105">With hello `nat` networking mode, having more than one containers service listening toohello same port results in deployment errors.</span></span> <span data-ttu-id="4ecf6-106">Számos szolgáltatás ugyanazt a portot, a Service Fabric támogatja hello figyelő futtatásához hello `open` hálózati mód (5.7-es vagy újabb verzió).</span><span class="sxs-lookup"><span data-stu-id="4ecf6-106">For running several services that listen on hello same port, Service Fabric supports hello `open` networking mode (version 5.7 or higher).</span></span> <span data-ttu-id="4ecf6-107">A hello `open` hálózati mód, minden tárolószolgáltatás beolvasása dinamikusan hozzárendelt IP-címnek belső így több szolgáltatások toolisten toohello ugyanazt a portot.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-107">With hello `open` networking mode, each container service gets a dynamically assigned IP address internally allowing multiple services toolisten toohello same port.</span></span>   

<span data-ttu-id="4ecf6-108">Így egyetlen szolgáltatástípus egy statikus végponttal hello szolgáltatás jegyzékben meghatározott, az új szolgáltatások előfordulhat, hogy hozható létre és hello felmerülő telepítési hibákat nélkül törli `open` hálózati mód.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-108">Thus, with a single service type with a static endpoint defined in hello service manifest, new services may be created and deleted without deployment errors using hello `open` networking mode.</span></span> <span data-ttu-id="4ecf6-109">Ehhez hasonlóan egy használható hello azonos `docker-compose.yml` több szolgáltatások létrehozásának statikus fájlt.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-109">Similarly, one can use hello same `docker-compose.yml` file with static port mappings for creating multiple services.</span></span>

<span data-ttu-id="4ecf6-110">Dinamikusan kiosztott IP toodiscover szolgáltatások hello használata esetén nem ajánlott óta hello IP-cím módosításainak hello szolgáltatás újraindítja vagy áthelyezi tooanother csomópont.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-110">Using hello dynamically assigned IP toodiscover services is not advisable since hello IP address changes when hello service restarts or moves tooanother node.</span></span> <span data-ttu-id="4ecf6-111">Csak a hello használata **Service Fabric-szolgáltatás** vagy hello **DNS-szolgáltatás** a szolgáltatás felderítése.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-111">Only use hello **Service Fabric Naming Service**  or hello **DNS Service** for service discovery.</span></span> 


> [!WARNING]
> <span data-ttu-id="4ecf6-112">Az Azure virtuális hálózat csak 4096 IP-címek összesen használható.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-112">Only a total of 4096 IPs are allowed per vNET in Azure.</span></span> <span data-ttu-id="4ecf6-113">Ebből kifolyólag hello összege hello csomópontok száma és a tároló szolgáltatáspéldány hello száma (a `open` hálózati) legfeljebb 4096 történik egy Vneten belül.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-113">Thus, hello sum of hello number of nodes and hello number of container service instances (with `open` networking) cannot exceed 4096 within a vNET.</span></span> <span data-ttu-id="4ecf6-114">Ilyen nagy sűrűségű forgatókönyvek hello `nat` hálózati mód használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-114">For such high-density scenarios, hello `nat` networking mode is recommended.</span></span>
>

## <a name="setting-up-open-networking-mode"></a><span data-ttu-id="4ecf6-115">Nyissa meg a hálózati mód beállítása</span><span class="sxs-lookup"><span data-stu-id="4ecf6-115">Setting up open networking mode</span></span>

1. <span data-ttu-id="4ecf6-116">Hello Azure Resource Manager-sablon beállítása a DNS-szolgáltatás engedélyezésével és az IP-szolgáltató hello `fabricSettings`.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-116">Set up hello Azure Resource Manager template by enabling DNS Service and hello IP Provider under `fabricSettings`.</span></span> 

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

2. <span data-ttu-id="4ecf6-117">Állítson be hello hálózati profil szakasz tooallow több IP-címek toobe hello fürt mindegyik csomópontján konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-117">Set up hello network profile section tooallow multiple IP addresses toobe configured on each node of hello cluster.</span></span> <span data-ttu-id="4ecf6-118">hello alábbi példa beállítja az egyes csomópontok öt IP-címek (így a figyelést toohello port minden csomóponton öt szolgáltatáspéldány lehet) a Windows Service Fabric-fürt számára.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-118">hello following example sets up five IP addresses per node (thus you can have five service instances listening toohello port on each node) for a Windows Service Fabric cluster.</span></span>

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

    <span data-ttu-id="4ecf6-119">Linux-fürtök esetén további nyilvános IP-konfigurációt tooallow kimenő kapcsolat hozzá.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-119">For Linux clusters, an additional public IP configuration is added tooallow outbound connectivity.</span></span> <span data-ttu-id="4ecf6-120">hello következő kódrészlettel állít be egy Linux-fürt csomópontonként öt IP-címek:</span><span class="sxs-lookup"><span data-stu-id="4ecf6-120">hello following snippet sets up five IP addresses per node for a Linux cluster:</span></span>

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

3. <span data-ttu-id="4ecf6-121">A fürtök csak, állítsa be az NSG Windows hello vnet UDP/53-as port nyitása a következő értékek hello szabály:</span><span class="sxs-lookup"><span data-stu-id="4ecf6-121">For Windows clusters only, set up an NSG rule opening up port UDP/53 for hello vNET with hello following values:</span></span>

   | <span data-ttu-id="4ecf6-122">Prioritás</span><span class="sxs-lookup"><span data-stu-id="4ecf6-122">Priority</span></span> |    <span data-ttu-id="4ecf6-123">Név</span><span class="sxs-lookup"><span data-stu-id="4ecf6-123">Name</span></span>    |    <span data-ttu-id="4ecf6-124">Forrás</span><span class="sxs-lookup"><span data-stu-id="4ecf6-124">Source</span></span>      |  <span data-ttu-id="4ecf6-125">Cél</span><span class="sxs-lookup"><span data-stu-id="4ecf6-125">Destination</span></span>   |   <span data-ttu-id="4ecf6-126">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4ecf6-126">Service</span></span>    | <span data-ttu-id="4ecf6-127">Műveletek</span><span class="sxs-lookup"><span data-stu-id="4ecf6-127">Action</span></span> |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     <span data-ttu-id="4ecf6-128">2000</span><span class="sxs-lookup"><span data-stu-id="4ecf6-128">2000</span></span> | <span data-ttu-id="4ecf6-129">Custom_Dns</span><span class="sxs-lookup"><span data-stu-id="4ecf6-129">Custom_Dns</span></span> | <span data-ttu-id="4ecf6-130">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="4ecf6-130">VirtualNetwork</span></span> | <span data-ttu-id="4ecf6-131">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="4ecf6-131">VirtualNetwork</span></span> | <span data-ttu-id="4ecf6-132">DNS (UDP/53)</span><span class="sxs-lookup"><span data-stu-id="4ecf6-132">DNS (UDP/53)</span></span> | <span data-ttu-id="4ecf6-133">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="4ecf6-133">Allow</span></span>  |


4. <span data-ttu-id="4ecf6-134">Adja meg, az alkalmazás-jegyzékfájl hello minden egyes szolgáltatás hello hálózati módban `<NetworkConfig NetworkType="open">`.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-134">Specify hello networking mode in hello app manifest for each service `<NetworkConfig NetworkType="open">`.</span></span>  <span data-ttu-id="4ecf6-135">hello mód `open` hello szolgáltatás egy dedikált IP-cím beolvasása eredményez.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-135">hello mode `open` results in hello service getting a dedicated IP address.</span></span> <span data-ttu-id="4ecf6-136">Ha egy mód nincs megadva, alapértelmezés szerint alapvető toohello `nat` mód.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-136">If a mode isn't specified, it defaults toohello basic `nat` mode.</span></span> <span data-ttu-id="4ecf6-137">Ebből kifolyólag a jegyzék a példában a következő hello `NodeContainerServicePackage1` és `NodeContainerServicePackage2` minden figyelési toohello is ugyanazt a portot (mindkét szolgáltatás által figyelt `Endpoint1`).</span><span class="sxs-lookup"><span data-stu-id="4ecf6-137">Thus, in hello following manifest example, `NodeContainerServicePackage1` and `NodeContainerServicePackage2` can each listen toohello same port (both services are listening on `Endpoint1`).</span></span>

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
<span data-ttu-id="4ecf6-138">Ön szabadon kombinálhatók hálózati mód szolgáltatásban egy Windows-fürt számára az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-138">You can mix and match different networking modes across services within an application for a Windows cluster.</span></span> <span data-ttu-id="4ecf6-139">Ebből kifolyólag lehet néhány szolgáltatás `open` mód és az egyes `nat` hálózati mód.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-139">Thus, you can have some services on `open` mode and some on `nat` networking mode.</span></span> <span data-ttu-id="4ecf6-140">Ha a szolgáltatás beállítása a `nat`, hello port figyelő toomust egyedinek lennie.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-140">When a service is configured with `nat`, hello port it is listening toomust be unique.</span></span> <span data-ttu-id="4ecf6-141">Különböző szolgáltatások hálózati módjainak keverése nem támogatott Linux fürtökön.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-141">Mixing networking modes for different services isn't supported on Linux clusters.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="4ecf6-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ecf6-142">Next steps</span></span>
<span data-ttu-id="4ecf6-143">Ebben a cikkben megtanulta, a Service Fabric által kínált módok hálózati.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-143">In this article, you learned about networking modes offered by Service Fabric.</span></span>  

* [<span data-ttu-id="4ecf6-144">A Service Fabric alkalmazásmodellt.</span><span class="sxs-lookup"><span data-stu-id="4ecf6-144">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="4ecf6-145">A Service Fabric service manifest erőforrások</span><span class="sxs-lookup"><span data-stu-id="4ecf6-145">Service Fabric service manifest resources</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="4ecf6-146">Egy Windows-tároló tooService Windows Server 2016 háló telepítése</span><span class="sxs-lookup"><span data-stu-id="4ecf6-146">Deploy a Windows container tooService Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="4ecf6-147">Egy Docker-tároló tooService Linux háló telepítése</span><span class="sxs-lookup"><span data-stu-id="4ecf6-147">Deploy a Docker container tooService Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
