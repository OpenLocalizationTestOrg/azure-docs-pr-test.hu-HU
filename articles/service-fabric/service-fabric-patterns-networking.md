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
# <a name="service-fabric-networking-patterns"></a><span data-ttu-id="ab880-103">A Service Fabric hálózati minták</span><span class="sxs-lookup"><span data-stu-id="ab880-103">Service Fabric networking patterns</span></span>
<span data-ttu-id="ab880-104">Az Azure Service Fabric-fürt integrálhatja más Azure hálózati szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="ab880-104">You can integrate your Azure Service Fabric cluster with other Azure networking features.</span></span> <span data-ttu-id="ab880-105">Ebben a cikkben megmutatjuk, hogyan toocreate fürtök, hogy a következő funkciók használata hello:</span><span class="sxs-lookup"><span data-stu-id="ab880-105">In this article, we show you how toocreate clusters that use hello following features:</span></span>

- [<span data-ttu-id="ab880-106">Meglévő virtuális hálózathoz vagy alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="ab880-106">Existing virtual network or subnet</span></span>](#existingvnet)
- [<span data-ttu-id="ab880-107">Statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="ab880-107">Static public IP address</span></span>](#staticpublicip)
- [<span data-ttu-id="ab880-108">Csak belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ab880-108">Internal-only load balancer</span></span>](#internallb)
- [<span data-ttu-id="ab880-109">Belső és külső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ab880-109">Internal and external load balancer</span></span>](#internalexternallb)

<span data-ttu-id="ab880-110">A Service Fabric-szabványos virtuálisgép-méretezési csoportban lévő fut.</span><span class="sxs-lookup"><span data-stu-id="ab880-110">Service Fabric runs in a standard virtual machine scale set.</span></span> <span data-ttu-id="ab880-111">Olyan funkciót, melyekkel egy virtuálisgép-méretezési csoportban lévő, használhatja a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="ab880-111">Any functionality that you can use in a virtual machine scale set, you can use with a Service Fabric cluster.</span></span> <span data-ttu-id="ab880-112">hálózati szakaszait hello hello Azure Resource Manager-sablonok a virtuálisgép-méretezési csoportok és a Service Fabric esetén azonosak.</span><span class="sxs-lookup"><span data-stu-id="ab880-112">hello networking sections of hello Azure Resource Manager templates for virtual machine scale sets and Service Fabric are identical.</span></span> <span data-ttu-id="ab880-113">Miután telepítette a meglévő virtuális hálózat tooan,-e más könnyen tooincorporate hálózati funkciók, például Azure ExpressRoute, Azure VPN Gateway, a hálózati biztonsági csoporthoz és virtuális hálózati társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="ab880-113">After you deploy tooan existing virtual network, it's easy tooincorporate other networking features, like Azure ExpressRoute, Azure VPN Gateway, a network security group, and virtual network peering.</span></span>

<span data-ttu-id="ab880-114">A Service Fabric rendszer más hálózati szolgáltatások egyik tulajdonsága az egyedi.</span><span class="sxs-lookup"><span data-stu-id="ab880-114">Service Fabric is unique from other networking features in one aspect.</span></span> <span data-ttu-id="ab880-115">Hello [Azure-portálon](https://portal.azure.com) belsőleg használt hello Service Fabric erőforrás szolgáltató toocall tooa fürt tooget csomópontok és alkalmazásokkal kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="ab880-115">hello [Azure portal](https://portal.azure.com) internally uses hello Service Fabric resource provider toocall tooa cluster tooget information about nodes and applications.</span></span> <span data-ttu-id="ab880-116">hello Service Fabric erőforrás-szolgáltató nyilvánosan elérhető befelé toohello HTTP átjáró port (19080, alapértelmezés szerint a port) hello felügyeleti végpont igényel.</span><span class="sxs-lookup"><span data-stu-id="ab880-116">hello Service Fabric resource provider requires publicly accessible inbound access toohello HTTP gateway port (port 19080, by default) on hello management endpoint.</span></span> <span data-ttu-id="ab880-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) hello felügyeleti végpont toomanage a fürt által használt.</span><span class="sxs-lookup"><span data-stu-id="ab880-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) uses hello management endpoint toomanage your cluster.</span></span> <span data-ttu-id="ab880-118">hello Service Fabric erőforrás-szolgáltató is használja ezt a fürtöt, az Azure-portálon hello toodisplay port tooquery információt.</span><span class="sxs-lookup"><span data-stu-id="ab880-118">hello Service Fabric resource provider also uses this port tooquery information about your cluster, toodisplay in hello Azure portal.</span></span> 

<span data-ttu-id="ab880-119">Port 19080 hello Service Fabric erőforrás-szolgáltató nem érhető el, ha egy üzenetet, például *csomópont nem található* megjelenik hello portálon, és megjelenik a a csomópont- és alkalmazás lista üres.</span><span class="sxs-lookup"><span data-stu-id="ab880-119">If port 19080 is not accessible from hello Service Fabric resource provider, a message like *Nodes Not Found* appears in hello portal, and your node and application list appears empty.</span></span> <span data-ttu-id="ab880-120">Ha toosee lévő hello Azure-portálon, a terheléselosztó fel kell fednie egy nyilvános IP-címet, és a hálózati biztonsági csoport engedélyeznie kell a bejövő portot 19080 adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="ab880-120">If you want toosee your cluster in hello Azure portal, your load balancer must expose a public IP address, and your network security group must allow incoming port 19080 traffic.</span></span> <span data-ttu-id="ab880-121">A telepítő nem felel meg a követelménynek, hello Azure-portálon nem jelennek meg a fürt hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="ab880-121">If your setup does not meet these requirements, hello Azure portal does not display hello status of your cluster.</span></span>

## <a name="templates"></a><span data-ttu-id="ab880-122">Sablonok</span><span class="sxs-lookup"><span data-stu-id="ab880-122">Templates</span></span>

<span data-ttu-id="ab880-123">Az összes Service Fabric-sablonok vannak [egyik letöltendő fájl](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span><span class="sxs-lookup"><span data-stu-id="ab880-123">All Service Fabric templates are in [one download file](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span></span> <span data-ttu-id="ab880-124">Meg kell tudni toodeploy hello sablont,-hello a következő PowerShell-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="ab880-124">You should be able toodeploy hello templates as-is by using hello following PowerShell commands.</span></span> <span data-ttu-id="ab880-125">Központi telepítése hello meglévő Azure-beli virtuális hálózatra-sablon vagy hello statikus nyilvános IP-sablont, elolvashatja hello [telepítő kezdeti](#initialsetup) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="ab880-125">If you are deploying hello existing Azure Virtual Network template or hello static public IP template, first read hello [Initial setup](#initialsetup) section of this article.</span></span>

<a id="initialsetup"></a>
## <a name="initial-setup"></a><span data-ttu-id="ab880-126">Kezdeti telepítés</span><span class="sxs-lookup"><span data-stu-id="ab880-126">Initial setup</span></span>

### <a name="existing-virtual-network"></a><span data-ttu-id="ab880-127">Meglévő virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="ab880-127">Existing virtual network</span></span>

<span data-ttu-id="ab880-128">A következő példa hello, először egy meglévő virtuális hálózat neve ExistingRG-vnet, a hello **ExistingRG** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ab880-128">In hello following example, we start with an existing virtual network named ExistingRG-vnet, in hello **ExistingRG** resource group.</span></span> <span data-ttu-id="ab880-129">hello alhálózati alapértelmezett neve.</span><span class="sxs-lookup"><span data-stu-id="ab880-129">hello subnet is named default.</span></span> <span data-ttu-id="ab880-130">Ezeket az alapértelmezett erőforrásokat hello Azure portál toocreate szabványos virtuális gép (VM) használatakor jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ab880-130">These default resources are created when you use hello Azure portal toocreate a standard virtual machine (VM).</span></span> <span data-ttu-id="ab880-131">Sikerült létrehozni a hello virtuális hálózati és alhálózati hello virtuális gép létrehozása nélkül, de hello fő célja egy fürt tooan meglévő virtuális hálózat hozzáadása tooprovide hálózati kapcsolat tooother virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="ab880-131">You could create hello virtual network and subnet without creating hello VM, but hello main goal of adding a cluster tooan existing virtual network is tooprovide network connectivity tooother VMs.</span></span> <span data-ttu-id="ab880-132">Virtuális gép létrehozása hello jó példán meglévő virtuális hálózat általában használatáról.</span><span class="sxs-lookup"><span data-stu-id="ab880-132">Creating hello VM gives a good example of how an existing virtual network typically is used.</span></span> <span data-ttu-id="ab880-133">Ha a Service Fabric-fürt használja csak a belső terheléselosztók, egy nyilvános IP-cím nélkül használhatja hello a virtuális gép és a nyilvános IP-cím, egy biztonságos *mezőben jump*.</span><span class="sxs-lookup"><span data-stu-id="ab880-133">If your Service Fabric cluster uses only an internal load balancer, without a public IP address, you can use hello VM and its public IP as a secure *jump box*.</span></span>

### <a name="static-public-ip-address"></a><span data-ttu-id="ab880-134">Statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="ab880-134">Static public IP address</span></span>

<span data-ttu-id="ab880-135">Egy statikus nyilvános IP-cím általában egy dedikált erőforrás hello VM vagy a virtuális gépek hozzá van rendelve a külön-külön kezelhető.</span><span class="sxs-lookup"><span data-stu-id="ab880-135">A static public IP address generally is a dedicated resource that's managed separately from hello VM or VMs it's assigned to.</span></span> <span data-ttu-id="ab880-136">Még lett beállítva a dedikált hálózati erőforráscsoportban (fürterőforrás-csoportként megakadályozását tooin hello Service Fabric maga).</span><span class="sxs-lookup"><span data-stu-id="ab880-136">It's provisioned in a dedicated networking resource group (as opposed tooin hello Service Fabric cluster resource group itself).</span></span> <span data-ttu-id="ab880-137">Hozzon létre egy statikus nyilvános IP-címet a hello staticIP1 nevű azonos ExistingRG erőforráscsoport hello Azure-portálon vagy a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="ab880-137">Create a static public IP address named staticIP1 in hello same ExistingRG resource group, either in hello Azure portal or by using PowerShell:</span></span>

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

### <a name="service-fabric-template"></a><span data-ttu-id="ab880-138">A Service Fabric-sablon</span><span class="sxs-lookup"><span data-stu-id="ab880-138">Service Fabric template</span></span>

<span data-ttu-id="ab880-139">A cikkben szereplő példák hello hello Service Fabric template.json használjuk.</span><span class="sxs-lookup"><span data-stu-id="ab880-139">In hello examples in this article, we use hello Service Fabric template.json.</span></span> <span data-ttu-id="ab880-140">Hello szabványos portál varázsló toodownload hello sablon hello portálról is használhat, a fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="ab880-140">You can use hello standard portal wizard toodownload hello template from hello portal before you create a cluster.</span></span> <span data-ttu-id="ab880-141">Is használhatja hello sablonok valamelyikét hello [sablon gyűjtemény](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), például a hello [öt csomópontból Service Fabric-fürt](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span><span class="sxs-lookup"><span data-stu-id="ab880-141">You also can use one of hello templates in hello [template gallery](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), like hello [five-node Service Fabric cluster](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span></span>

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a><span data-ttu-id="ab880-142">Meglévő virtuális hálózathoz vagy alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="ab880-142">Existing virtual network or subnet</span></span>

1. <span data-ttu-id="ab880-143">Hello alhálózati toohello paraméternév hello meglévő alhálózat módosítása, és adja hozzá a két új paramétereket tooreference hello meglévő virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="ab880-143">Change hello subnet parameter toohello name of hello existing subnet, and then add two new parameters tooreference hello existing virtual network:</span></span>

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


2. <span data-ttu-id="ab880-144">Változás hello `vnetID` változó toopoint toohello meglévő virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="ab880-144">Change hello `vnetID` variable toopoint toohello existing virtual network:</span></span>

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. <span data-ttu-id="ab880-145">Távolítsa el `Microsoft.Network/virtualNetworks` az erőforrásokat, így Azure nem hozzon létre egy új virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="ab880-145">Remove `Microsoft.Network/virtualNetworks` from your resources, so Azure does not create a new virtual network:</span></span>

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

4. <span data-ttu-id="ab880-146">Hello hello virtuális hálózatban megjegyzésbe `dependsOn` attribútumának `Microsoft.Compute/virtualMachineScaleSets`, így nem függ egy új virtuális hálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ab880-146">Comment out hello virtual network from hello `dependsOn` attribute of `Microsoft.Compute/virtualMachineScaleSets`, so you don't depend on creating a new virtual network:</span></span>

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

5. <span data-ttu-id="ab880-147">Hello sablon üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="ab880-147">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    <span data-ttu-id="ab880-148">A központi telepítést követően tartalmaznia kell a virtuális hálózat hello új méretezési virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="ab880-148">After deployment, your virtual network should include hello new scale set VMs.</span></span> <span data-ttu-id="ab880-149">hello virtuális gép méretezési készlet csomóponttípus hello meglévő virtuális hálózat és alhálózat kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="ab880-149">hello virtual machine scale set node type should show hello existing virtual network and subnet.</span></span> <span data-ttu-id="ab880-150">Remote Desktop Protocol (RDP) tooaccess hello virtuális Gépet, amely már szerepel a virtuális hálózati hello is használhatja, és tooping hello új méretezési virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="ab880-150">You also can use Remote Desktop Protocol (RDP) tooaccess hello VM that was already in hello virtual network, and tooping hello new scale set VMs:</span></span>

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

<span data-ttu-id="ab880-151">Egy másik példa, lásd: [, amely nem adott tooService háló](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="ab880-151">For another example, see [one that is not specific tooService Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span></span>


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a><span data-ttu-id="ab880-152">Statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="ab880-152">Static public IP address</span></span>

1. <span data-ttu-id="ab880-153">Adja hozzá a statikus IP-erőforráscsoport nevét és teljes tartománynevét (FQDN) meglévő hello hello neve paramétereinek:</span><span class="sxs-lookup"><span data-stu-id="ab880-153">Add parameters for hello name of hello existing static IP resource group, name, and fully qualified domain name (FQDN):</span></span>

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

2. <span data-ttu-id="ab880-154">Távolítsa el a hello `dnsName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="ab880-154">Remove hello `dnsName` parameter.</span></span> <span data-ttu-id="ab880-155">(hello statikus IP-cím már szerepel ilyen.)</span><span class="sxs-lookup"><span data-stu-id="ab880-155">(hello static IP address already has one.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. <span data-ttu-id="ab880-156">Egy változó tooreference hello meglévő statikus IP-cím hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ab880-156">Add a variable tooreference hello existing static IP address:</span></span>

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. <span data-ttu-id="ab880-157">Távolítsa el `Microsoft.Network/publicIPAddresses` az erőforrásokat, így Azure nem hozzon létre egy új IP-cím:</span><span class="sxs-lookup"><span data-stu-id="ab880-157">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

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

5. <span data-ttu-id="ab880-158">Hello IP-címet hello megjegyzésbe `dependsOn` attribútumának `Microsoft.Network/loadBalancers`, így nem függ egy új IP-cím létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ab880-158">Comment out hello IP address from hello `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address:</span></span>

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

6. <span data-ttu-id="ab880-159">A hello `Microsoft.Network/loadBalancers` erőforrás, a módosítás hello `publicIPAddress` eleme `frontendIPConfigurations` tooreference hello helyett egy újonnan létrehozott egy létező statikus IP-cím:</span><span class="sxs-lookup"><span data-stu-id="ab880-159">In hello `Microsoft.Network/loadBalancers` resource, change hello `publicIPAddress` element of `frontendIPConfigurations` tooreference hello existing static IP address instead of a newly created one:</span></span>

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

7. <span data-ttu-id="ab880-160">A hello `Microsoft.ServiceFabric/clusters` erőforrás, a módosítás `managementEndpoint` toohello hello statikus IP-cím DNS teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="ab880-160">In hello `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` toohello DNS FQDN of hello static IP address.</span></span> <span data-ttu-id="ab880-161">Ha egy biztonságos fürtöt használ, ellenőrizze, hogy megváltoztatja *http://* túl*https://*.</span><span class="sxs-lookup"><span data-stu-id="ab880-161">If you are using a secure cluster, make sure you change *http://* too*https://*.</span></span> <span data-ttu-id="ab880-162">(Vegye figyelembe, hogy ez a lépés csak tooService Fabric-fürtök vonatkozik-e.</span><span class="sxs-lookup"><span data-stu-id="ab880-162">(Note that this step applies only tooService Fabric clusters.</span></span> <span data-ttu-id="ab880-163">Ha egy virtuálisgép-méretezési csoport használja, kihagyhatja ezt a lépést.)</span><span class="sxs-lookup"><span data-stu-id="ab880-163">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. <span data-ttu-id="ab880-164">Hello sablon üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="ab880-164">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

<span data-ttu-id="ab880-165">Telepítés után ellenőrizheti, hogy a terheléselosztó-e a kötött toohello nyilvános statikus IP-cím a hello másik erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ab880-165">After deployment, you can see that your load balancer is bound toohello public static IP address from hello other resource group.</span></span> <span data-ttu-id="ab880-166">a Service Fabric ügyfél-csatlakozási végpont hello és [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) végpont pont toohello hello statikus IP-cím DNS teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="ab880-166">hello Service Fabric client connection endpoint and [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint point toohello DNS FQDN of hello static IP address.</span></span>

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a><span data-ttu-id="ab880-167">Csak belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ab880-167">Internal-only load balancer</span></span>

<span data-ttu-id="ab880-168">Ebben a forgatókönyvben egy csak belső terheléselosztó hello külső terheléselosztó hello alapértelmezett Service Fabric sablon lecseréli.</span><span class="sxs-lookup"><span data-stu-id="ab880-168">This scenario replaces hello external load balancer in hello default Service Fabric template with an internal-only load balancer.</span></span> <span data-ttu-id="ab880-169">Hello Azure-portál és a Service Fabric erőforrás-szolgáltató hello megvalósítását lásd: hello megelőző szakasz.</span><span class="sxs-lookup"><span data-stu-id="ab880-169">For implications for hello Azure portal and for hello Service Fabric resource provider, see hello preceding section.</span></span>

1. <span data-ttu-id="ab880-170">Távolítsa el a hello `dnsName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="ab880-170">Remove hello `dnsName` parameter.</span></span> <span data-ttu-id="ab880-171">(Nincs rá szükség.)</span><span class="sxs-lookup"><span data-stu-id="ab880-171">(It's not needed.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. <span data-ttu-id="ab880-172">Szükség esetén egy statikus kiosztási módszerrel használatakor is hozzáadhat egy statikus IP-cím paraméter.</span><span class="sxs-lookup"><span data-stu-id="ab880-172">Optionally, if you use a static allocation method, you can add a static IP address parameter.</span></span> <span data-ttu-id="ab880-173">Ha egy dinamikus elosztási módszert használ, nem kell toodo ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="ab880-173">If you use a dynamic allocation method, you do not need toodo this step.</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. <span data-ttu-id="ab880-174">Távolítsa el `Microsoft.Network/publicIPAddresses` az erőforrásokat, így Azure nem hozzon létre egy új IP-cím:</span><span class="sxs-lookup"><span data-stu-id="ab880-174">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

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

4. <span data-ttu-id="ab880-175">Távolítsa el a hello IP-cím `dependsOn` attribútumának `Microsoft.Network/loadBalancers`, így nem függ egy új IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ab880-175">Remove hello IP address `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address.</span></span> <span data-ttu-id="ab880-176">Adja hozzá a virtuális hálózati hello `dependsOn` attribútumon, mert hello terheléselosztó most hello virtuális hálózati alhálózat hello függ:</span><span class="sxs-lookup"><span data-stu-id="ab880-176">Add hello virtual network `dependsOn` attribute because hello load balancer now depends on hello subnet from hello virtual network:</span></span>

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

5. <span data-ttu-id="ab880-177">Hello terheléselosztójának módosítása `frontendIPConfigurations` beállítása a használatával egy `publicIPAddress`, toousing alhálózat és `privateIPAddress`.</span><span class="sxs-lookup"><span data-stu-id="ab880-177">Change hello load balancer's `frontendIPConfigurations` setting from using a `publicIPAddress`, toousing a subnet and `privateIPAddress`.</span></span> <span data-ttu-id="ab880-178">`privateIPAddress`egy előre meghatározott statikus belső IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="ab880-178">`privateIPAddress` uses a predefined static internal IP address.</span></span> <span data-ttu-id="ab880-179">dinamikus IP-címnek, toouse eltávolítása hello `privateIPAddress` elemet, és módosítsa `privateIPAllocationMethod` túl**dinamikus**.</span><span class="sxs-lookup"><span data-stu-id="ab880-179">toouse a dynamic IP address, remove hello `privateIPAddress` element, and then change `privateIPAllocationMethod` too**Dynamic**.</span></span>

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

6. <span data-ttu-id="ab880-180">A hello `Microsoft.ServiceFabric/clusters` erőforrás, a módosítás `managementEndpoint` toopoint toohello belső terheléselosztói címet.</span><span class="sxs-lookup"><span data-stu-id="ab880-180">In hello `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` toopoint toohello internal load balancer address.</span></span> <span data-ttu-id="ab880-181">Ha biztonságos-fürtöt használ, ellenőrizze, hogy megváltoztatja *http://* túl*https://*.</span><span class="sxs-lookup"><span data-stu-id="ab880-181">If you use a secure cluster, make sure you change *http://* too*https://*.</span></span> <span data-ttu-id="ab880-182">(Vegye figyelembe, hogy ez a lépés csak tooService Fabric-fürtök vonatkozik-e.</span><span class="sxs-lookup"><span data-stu-id="ab880-182">(Note that this step applies only tooService Fabric clusters.</span></span> <span data-ttu-id="ab880-183">Ha egy virtuálisgép-méretezési csoport használja, kihagyhatja ezt a lépést.)</span><span class="sxs-lookup"><span data-stu-id="ab880-183">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. <span data-ttu-id="ab880-184">Hello sablon üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="ab880-184">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

<span data-ttu-id="ab880-185">A központi telepítést követően a terheléselosztó hello titkos statikus 10.0.0.250 IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="ab880-185">After deployment, your load balancer uses hello private static 10.0.0.250 IP address.</span></span> <span data-ttu-id="ab880-186">Ha egy másik gép ugyanazon virtuális hálózatban, elvégezheti a belső toohello [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) végpont.</span><span class="sxs-lookup"><span data-stu-id="ab880-186">If you have another machine in that same virtual network, you can go toohello internal [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint.</span></span> <span data-ttu-id="ab880-187">Vegye figyelembe, hogy csatlakozik-e tooone hello csomópontok hello terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="ab880-187">Note that it connects tooone of hello nodes behind hello load balancer.</span></span>

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a><span data-ttu-id="ab880-188">Belső és külső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ab880-188">Internal and external load balancer</span></span>

<span data-ttu-id="ab880-189">Ebben a forgatókönyvben hello meglévő egycsomópontos típus külső terheléselosztó kezdődnie, és adja hozzá a belső terheléselosztók hello az azonos csomóponttípus.</span><span class="sxs-lookup"><span data-stu-id="ab880-189">In this scenario, you start with hello existing single-node type external load balancer, and add an internal load balancer for hello same node type.</span></span> <span data-ttu-id="ab880-190">A háttér-port csatolt tooa háttér-címkészlet csak tooa egyetlen terheléselosztóhoz rendelhetők hozzá.</span><span class="sxs-lookup"><span data-stu-id="ab880-190">A back-end port attached tooa back-end address pool can be assigned only tooa single load balancer.</span></span> <span data-ttu-id="ab880-191">Válassza ki, mely terheléselosztót kapnak az alkalmazás portok, valamint mely terheléselosztót fog rendelkezni a felügyeleti végpontok (portok 19000 és 19080).</span><span class="sxs-lookup"><span data-stu-id="ab880-191">Choose which load balancer will have your application ports, and which load balancer will have your management endpoints (ports 19000 and 19080).</span></span> <span data-ttu-id="ab880-192">Ha hello felügyeleti végpontok hello belső terheléselosztón, tartsa szem előtt tartva hello Service Fabric-erőforrás szolgáltató korlátozások hello cikkben korábban ismertetett.</span><span class="sxs-lookup"><span data-stu-id="ab880-192">If you put hello management endpoints on hello internal load balancer, keep in mind hello Service Fabric resource provider restrictions discussed earlier in hello article.</span></span> <span data-ttu-id="ab880-193">Hello példában használjuk hello felügyeleti végpontok hello külső terheléselosztó maradnak.</span><span class="sxs-lookup"><span data-stu-id="ab880-193">In hello example we use, hello management endpoints remain on hello external load balancer.</span></span> <span data-ttu-id="ab880-194">Is adjon hozzá egy port 80 alkalmazás portot, és helyezze el hello belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="ab880-194">You also add a port 80 application port, and place it on hello internal load balancer.</span></span>

<span data-ttu-id="ab880-195">A két csomóponttípus fürtben egy csomópont típus hello külső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="ab880-195">In a two-node-type cluster, one node type is on hello external load balancer.</span></span> <span data-ttu-id="ab880-196">hello más típusú csomópont értéke hello belső terheléselosztón.</span><span class="sxs-lookup"><span data-stu-id="ab880-196">hello other node type is on hello internal load balancer.</span></span> <span data-ttu-id="ab880-197">két csomóponttípus fürt, a hello portál által létrehozott két csomóponttípus sablon (Ez a két terheléselosztók) toouse hello második load balancer tooan belső terheléselosztó váltani.</span><span class="sxs-lookup"><span data-stu-id="ab880-197">toouse a two-node-type cluster, in hello portal-created two-node-type template (which comes with two load balancers), switch hello second load balancer tooan internal load balancer.</span></span> <span data-ttu-id="ab880-198">További információkért lásd: hello [csak belső terheléselosztó](#internallb) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ab880-198">For more information, see hello [Internal-only load balancer](#internallb) section.</span></span>

1. <span data-ttu-id="ab880-199">Adja hozzá a hello statikus belső load balancer IP-cím paraméter.</span><span class="sxs-lookup"><span data-stu-id="ab880-199">Add hello static internal load balancer IP address parameter.</span></span> <span data-ttu-id="ab880-200">(Megjegyzések kapcsolódó toousing dinamikus IP-címnek, tekintse meg a cikk korábbi szakaszaiban.)</span><span class="sxs-lookup"><span data-stu-id="ab880-200">(For notes related toousing a dynamic IP address, see earlier sections of this article.)</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. <span data-ttu-id="ab880-201">Adja hozzá az alkalmazás 80-as port paramétert.</span><span class="sxs-lookup"><span data-stu-id="ab880-201">Add an application port 80 parameter.</span></span>

3. <span data-ttu-id="ab880-202">tooadd belső hello meglévő változók, hálózati másolja és illessze be a, és adja hozzá "-Int" toohello nevét:</span><span class="sxs-lookup"><span data-stu-id="ab880-202">tooadd internal versions of hello existing networking variables, copy and paste them, and add "-Int" toohello name:</span></span>

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

4. <span data-ttu-id="ab880-203">Ha először hello portál által létrehozott sablont, amely az alkalmazás 80-as portot használja, hello alapértelmezett portálsablon hozzáadja AppPort1 (80-as port) hello külső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="ab880-203">If you start with hello portal-generated template that uses application port 80, hello default portal template adds AppPort1 (port 80) on hello external load balancer.</span></span> <span data-ttu-id="ab880-204">Ebben az esetben távolítsa el a AppPort1 hello külső terheléselosztó `loadBalancingRules` és mintavételek menüpontban, így hozzáadhatja azt toohello belső terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="ab880-204">In this case, remove AppPort1 from hello external load balancer `loadBalancingRules` and probes, so you can add it toohello internal load balancer:</span></span>

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

5. <span data-ttu-id="ab880-205">Adja hozzá egy második `Microsoft.Network/loadBalancers` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ab880-205">Add a second `Microsoft.Network/loadBalancers` resource.</span></span> <span data-ttu-id="ab880-206">A jelek hasonló toohello belső terheléselosztó hello létrehozott [csak belső terheléselosztó](#internallb) szakaszában, de használja hello "-Int" terheléselosztó változók, és megvalósít csak hello alkalmazás 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="ab880-206">It looks similar toohello internal load balancer created in hello [Internal-only load balancer](#internallb) section, but it uses hello "-Int" load balancer variables, and implements only hello application port 80.</span></span> <span data-ttu-id="ab880-207">Ez eltávolítja `inboundNatPools`, nyilvános terheléselosztót hello tookeep RDP végpontja.</span><span class="sxs-lookup"><span data-stu-id="ab880-207">This also removes `inboundNatPools`, tookeep RDP endpoints on hello public load balancer.</span></span> <span data-ttu-id="ab880-208">RDP hello belső terheléselosztón, helyezze `inboundNatPools` hello külső load balancer toothis belső terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="ab880-208">If you want RDP on hello internal load balancer, move `inboundNatPools` from hello external load balancer toothis internal load balancer:</span></span>

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

6. <span data-ttu-id="ab880-209">A `networkProfile` a hello `Microsoft.Compute/virtualMachineScaleSets` erőforrás, hello belső háttér-címkészlet hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ab880-209">In `networkProfile` for hello `Microsoft.Compute/virtualMachineScaleSets` resource, add hello internal back-end address pool:</span></span>

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

7. <span data-ttu-id="ab880-210">Hello sablon üzembe helyezése:</span><span class="sxs-lookup"><span data-stu-id="ab880-210">Deploy hello template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

<span data-ttu-id="ab880-211">A központi telepítést követően megtekintheti a két terheléselosztók hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ab880-211">After deployment, you can see two load balancers in hello resource group.</span></span> <span data-ttu-id="ab880-212">Ha tallózással hello terheléselosztók, láthatja a hello nyilvános IP-cím és a felügyeleti végpontok (portok 19000 és 19080) hozzárendelt toohello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="ab880-212">If you browse hello load balancers, you can see hello public IP address and management endpoints (ports 19000 and 19080) assigned toohello public IP address.</span></span> <span data-ttu-id="ab880-213">Is láthatóvá hello statikus belső IP cím és az alkalmazás végpont (80-as port) hozzárendelt toohello belső terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="ab880-213">You also can see hello static internal IP address and application endpoint (port 80) assigned toohello internal load balancer.</span></span> <span data-ttu-id="ab880-214">Mindkét betölteni egy terheléselosztó használata hello ugyanazon virtuálisgép-méretezési csoport háttér-készlet.</span><span class="sxs-lookup"><span data-stu-id="ab880-214">Both load balancers use hello same virtual machine scale set back-end pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab880-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab880-215">Next steps</span></span>
[<span data-ttu-id="ab880-216">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab880-216">Create a cluster</span></span>](service-fabric-cluster-creation-via-arm.md)
