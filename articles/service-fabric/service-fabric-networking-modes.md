---
title: "Az Azure Service Fabric tárolószolgáltatások hálózati módok konfigurálása |} Microsoft Docs"
description: "Útmutató a hálózati mód, amely támogatja az Azure Service Fabric beállítása."
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
ms.openlocfilehash: f792f9604a5d6e62551ed92c1049d6e2b4216417
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="service-fabric-container-networking-modes"></a><span data-ttu-id="ff0ad-103">A Service Fabric tároló hálózati módok</span><span class="sxs-lookup"><span data-stu-id="ff0ad-103">Service Fabric container networking modes</span></span>

<span data-ttu-id="ff0ad-104">A hálózati módban érhető el a Service Fabric-fürt tárolószolgáltatások az alapértelmezett érték a `nat` hálózati mód.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-104">The default networking mode offered in the Service Fabric cluster for container services is the `nat` networking mode.</span></span> <span data-ttu-id="ff0ad-105">Az a `nat` hálózati mód, hogy az azonos port eredményeket a központi telepítési hibái figyel egynél több tároló szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-105">With the `nat` networking mode, having more than one containers service listening to the same port results in deployment errors.</span></span> <span data-ttu-id="ff0ad-106">Rendszert futtató több szolgáltatási adott figyelési ugyanazt a portot, a Service Fabric támogatja a `open` hálózati mód (5.7-es vagy újabb verzió).</span><span class="sxs-lookup"><span data-stu-id="ff0ad-106">For running several services that listen on the same port, Service Fabric supports the `open` networking mode (version 5.7 or higher).</span></span> <span data-ttu-id="ff0ad-107">Az a `open` hálózati mód, minden tárolószolgáltatás beolvasása dinamikusan hozzárendelt IP-címnek belső így több szolgáltatás ugyanazt a portot figyeli.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-107">With the `open` networking mode, each container service gets a dynamically assigned IP address internally allowing multiple services to listen to the same port.</span></span>   

<span data-ttu-id="ff0ad-108">Így egyetlen szolgáltatástípus egy statikus végponttal, a szolgáltatás jegyzékben meghatározott, az új szolgáltatások előfordulhat, hogy hozható létre és használatával, a telepítési hibák nélkül törli a `open` hálózati mód.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-108">Thus, with a single service type with a static endpoint defined in the service manifest, new services may be created and deleted without deployment errors using the `open` networking mode.</span></span> <span data-ttu-id="ff0ad-109">Hasonlóképpen, egy használhatja ugyanazt `docker-compose.yml` több szolgáltatások létrehozásának statikus fájlt.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-109">Similarly, one can use the same `docker-compose.yml` file with static port mappings for creating multiple services.</span></span>

<span data-ttu-id="ff0ad-110">A dinamikusan kiosztott IP-használatával felderítéséhez a szolgáltatások esetén nem javasolt óta az IP-címet érintő módosításait a szolgáltatás újraindul, vagy egy másik csomópontjára helyezi át.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-110">Using the dynamically assigned IP to discover services is not advisable since the IP address changes when the service restarts or moves to another node.</span></span> <span data-ttu-id="ff0ad-111">Csak a **Service Fabric-szolgáltatás** vagy a **DNS-szolgáltatás** a szolgáltatás felderítése.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-111">Only use the **Service Fabric Naming Service**  or the **DNS Service** for service discovery.</span></span> 


> [!WARNING]
> <span data-ttu-id="ff0ad-112">Az Azure virtuális hálózat csak 4096 IP-címek összesen használható.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-112">Only a total of 4096 IPs are allowed per vNET in Azure.</span></span> <span data-ttu-id="ff0ad-113">Ebből kifolyólag a csomópontok száma és a tároló szolgáltatás példányainak száma összege (a `open` hálózati) legfeljebb 4096 történik egy Vneten belül.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-113">Thus, the sum of the number of nodes and the number of container service instances (with `open` networking) cannot exceed 4096 within a vNET.</span></span> <span data-ttu-id="ff0ad-114">Ilyen nagy sűrűségű forgatókönyvek esetén a `nat` hálózati mód használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-114">For such high-density scenarios, the `nat` networking mode is recommended.</span></span>
>

## <a name="setting-up-open-networking-mode"></a><span data-ttu-id="ff0ad-115">Nyissa meg a hálózati mód beállítása</span><span class="sxs-lookup"><span data-stu-id="ff0ad-115">Setting up open networking mode</span></span>

1. <span data-ttu-id="ff0ad-116">DNS-szolgáltatás és az IP-szolgáltató a engedélyezésével az Azure Resource Manager-sablon beállítása `fabricSettings`.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-116">Set up the Azure Resource Manager template by enabling DNS Service and the IP Provider under `fabricSettings`.</span></span> 

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

2. <span data-ttu-id="ff0ad-117">A hálózati profil szakasz beállítása lehetővé teszi több IP-címmel kell konfigurálni a fürt mindegyik csomópontján.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-117">Set up the network profile section to allow multiple IP addresses to be configured on each node of the cluster.</span></span> <span data-ttu-id="ff0ad-118">Az alábbi példa beállítja az öt IP-címek csomópontonként (így a minden egyes csomóponton portot öt szolgáltatáspéldány rendelkezhet) Windows Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-118">The following example sets up five IP addresses per node (thus you can have five service instances listening to the port on each node) for a Windows Service Fabric cluster.</span></span>

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

    <span data-ttu-id="ff0ad-119">Linux-fürtök esetén a további nyilvános IP-konfigurációt bekerül a kimenő kapcsolatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-119">For Linux clusters, an additional public IP configuration is added to allow outbound connectivity.</span></span> <span data-ttu-id="ff0ad-120">Az alábbi kódrészletben állít be egy Linux-fürt csomópontonként öt IP-címek:</span><span class="sxs-lookup"><span data-stu-id="ff0ad-120">The following snippet sets up five IP addresses per node for a Linux cluster:</span></span>

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

3. <span data-ttu-id="ff0ad-121">Csak Windows-fürtök esetén beállítása egy NSG-szabály az alábbi értékeket a vnet UDP/53-as port megnyitására:</span><span class="sxs-lookup"><span data-stu-id="ff0ad-121">For Windows clusters only, set up an NSG rule opening up port UDP/53 for the vNET with the following values:</span></span>

   | <span data-ttu-id="ff0ad-122">Prioritás</span><span class="sxs-lookup"><span data-stu-id="ff0ad-122">Priority</span></span> |    <span data-ttu-id="ff0ad-123">Név</span><span class="sxs-lookup"><span data-stu-id="ff0ad-123">Name</span></span>    |    <span data-ttu-id="ff0ad-124">Forrás</span><span class="sxs-lookup"><span data-stu-id="ff0ad-124">Source</span></span>      |  <span data-ttu-id="ff0ad-125">Cél</span><span class="sxs-lookup"><span data-stu-id="ff0ad-125">Destination</span></span>   |   <span data-ttu-id="ff0ad-126">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ff0ad-126">Service</span></span>    | <span data-ttu-id="ff0ad-127">Műveletek</span><span class="sxs-lookup"><span data-stu-id="ff0ad-127">Action</span></span> |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     <span data-ttu-id="ff0ad-128">2000</span><span class="sxs-lookup"><span data-stu-id="ff0ad-128">2000</span></span> | <span data-ttu-id="ff0ad-129">Custom_Dns</span><span class="sxs-lookup"><span data-stu-id="ff0ad-129">Custom_Dns</span></span> | <span data-ttu-id="ff0ad-130">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="ff0ad-130">VirtualNetwork</span></span> | <span data-ttu-id="ff0ad-131">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="ff0ad-131">VirtualNetwork</span></span> | <span data-ttu-id="ff0ad-132">DNS (UDP/53)</span><span class="sxs-lookup"><span data-stu-id="ff0ad-132">DNS (UDP/53)</span></span> | <span data-ttu-id="ff0ad-133">Engedélyezés</span><span class="sxs-lookup"><span data-stu-id="ff0ad-133">Allow</span></span>  |


4. <span data-ttu-id="ff0ad-134">Adja meg a hálózati mód az alkalmazásjegyzéket az egyes szolgáltatásokhoz `<NetworkConfig NetworkType="open">`.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-134">Specify the networking mode in the app manifest for each service `<NetworkConfig NetworkType="open">`.</span></span>  <span data-ttu-id="ff0ad-135">A mód `open` eredményezi, a szolgáltatás egy dedikált IP-cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-135">The mode `open` results in the service getting a dedicated IP address.</span></span> <span data-ttu-id="ff0ad-136">Ha egy mód nincs megadva, a basic alapértelmezett `nat` mód.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-136">If a mode isn't specified, it defaults to the basic `nat` mode.</span></span> <span data-ttu-id="ff0ad-137">Így az alábbi jegyzék `NodeContainerServicePackage1` és `NodeContainerServicePackage2` is ugyanazt a portot minden figyelése (mindkét szolgáltatás által figyelt `Endpoint1`).</span><span class="sxs-lookup"><span data-stu-id="ff0ad-137">Thus, in the following manifest example, `NodeContainerServicePackage1` and `NodeContainerServicePackage2` can each listen to the same port (both services are listening on `Endpoint1`).</span></span>

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
<span data-ttu-id="ff0ad-138">Ön szabadon kombinálhatók hálózati mód szolgáltatásban egy Windows-fürt számára az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-138">You can mix and match different networking modes across services within an application for a Windows cluster.</span></span> <span data-ttu-id="ff0ad-139">Ebből kifolyólag lehet néhány szolgáltatás `open` mód és az egyes `nat` hálózati mód.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-139">Thus, you can have some services on `open` mode and some on `nat` networking mode.</span></span> <span data-ttu-id="ff0ad-140">Ha a szolgáltatás beállítása a `nat`, a port a figyeléshez egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-140">When a service is configured with `nat`, the port it is listening to must be unique.</span></span> <span data-ttu-id="ff0ad-141">Különböző szolgáltatások hálózati módjainak keverése nem támogatott Linux fürtökön.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-141">Mixing networking modes for different services isn't supported on Linux clusters.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="ff0ad-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff0ad-142">Next steps</span></span>
<span data-ttu-id="ff0ad-143">Ebben a cikkben megtanulta, a Service Fabric által kínált módok hálózati.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-143">In this article, you learned about networking modes offered by Service Fabric.</span></span>  

* [<span data-ttu-id="ff0ad-144">A Service Fabric alkalmazásmodellt.</span><span class="sxs-lookup"><span data-stu-id="ff0ad-144">Service Fabric application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="ff0ad-145">A Service Fabric service manifest erőforrások</span><span class="sxs-lookup"><span data-stu-id="ff0ad-145">Service Fabric service manifest resources</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="ff0ad-146">A Service Fabric Windows Server 2016 egy Windows-tároló telepítése</span><span class="sxs-lookup"><span data-stu-id="ff0ad-146">Deploy a Windows container to Service Fabric on Windows Server 2016</span></span>](service-fabric-get-started-containers.md)
* [<span data-ttu-id="ff0ad-147">Telepítse a Service Fabric Linux egy Docker-tároló</span><span class="sxs-lookup"><span data-stu-id="ff0ad-147">Deploy a Docker container to Service Fabric on Linux</span></span>](service-fabric-get-started-containers-linux.md)
