---
title: "Azure Service Fabric hálózatkezelés mintái |} Microsoft Docs"
description: "A Service Fabric és a fürt létrehozása az Azure hálózati szolgáltatások segítségével közös hálózati mintákat ismerteti."
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
ms.openlocfilehash: 126637002b24391058fb702227a570aa0b58c1d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="service-fabric-networking-patterns"></a><span data-ttu-id="2bbce-103">A Service Fabric hálózati minták</span><span class="sxs-lookup"><span data-stu-id="2bbce-103">Service Fabric networking patterns</span></span>
<span data-ttu-id="2bbce-104">Az Azure Service Fabric-fürt integrálhatja más Azure hálózati szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="2bbce-104">You can integrate your Azure Service Fabric cluster with other Azure networking features.</span></span> <span data-ttu-id="2bbce-105">Ebben a cikkben azt mutatja be az alábbi szolgáltatásokat használó fürtök létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="2bbce-105">In this article, we show you how to create clusters that use the following features:</span></span>

- [<span data-ttu-id="2bbce-106">Meglévő virtuális hálózathoz vagy alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="2bbce-106">Existing virtual network or subnet</span></span>](#existingvnet)
- [<span data-ttu-id="2bbce-107">Statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="2bbce-107">Static public IP address</span></span>](#staticpublicip)
- [<span data-ttu-id="2bbce-108">Csak belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2bbce-108">Internal-only load balancer</span></span>](#internallb)
- [<span data-ttu-id="2bbce-109">Belső és külső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2bbce-109">Internal and external load balancer</span></span>](#internalexternallb)

<span data-ttu-id="2bbce-110">A Service Fabric-szabványos virtuálisgép-méretezési csoportban lévő fut.</span><span class="sxs-lookup"><span data-stu-id="2bbce-110">Service Fabric runs in a standard virtual machine scale set.</span></span> <span data-ttu-id="2bbce-111">Olyan funkciót, melyekkel egy virtuálisgép-méretezési csoportban lévő, használhatja a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="2bbce-111">Any functionality that you can use in a virtual machine scale set, you can use with a Service Fabric cluster.</span></span> <span data-ttu-id="2bbce-112">Az Azure Resource Manager-sablonok a virtuálisgép-méretezési csoportok és a Service Fabric hálózati szakasza esetén azonosak.</span><span class="sxs-lookup"><span data-stu-id="2bbce-112">The networking sections of the Azure Resource Manager templates for virtual machine scale sets and Service Fabric are identical.</span></span> <span data-ttu-id="2bbce-113">Miután telepít egy meglévő virtuális hálózatot, is könnyen más hálózati funkciókat, például Azure ExpressRoute, Azure VPN Gateway, a hálózati biztonsági csoporthoz és virtuális hálózati társviszony-létesítés tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="2bbce-113">After you deploy to an existing virtual network, it's easy to incorporate other networking features, like Azure ExpressRoute, Azure VPN Gateway, a network security group, and virtual network peering.</span></span>

<span data-ttu-id="2bbce-114">A Service Fabric rendszer más hálózati szolgáltatások egyik tulajdonsága az egyedi.</span><span class="sxs-lookup"><span data-stu-id="2bbce-114">Service Fabric is unique from other networking features in one aspect.</span></span> <span data-ttu-id="2bbce-115">A [Azure-portálon](https://portal.azure.com) belső módon használja a Service Fabric erőforrás-szolgáltató hívása fürthöz csomópontokat és alkalmazások adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2bbce-115">The [Azure portal](https://portal.azure.com) internally uses the Service Fabric resource provider to call to a cluster to get information about nodes and applications.</span></span> <span data-ttu-id="2bbce-116">A Service Fabric erőforrás-szolgáltató nyilvánosan elérhető bejövő kell elérnie a felügyeleti végpont HTTP-átjáró port (19080, alapértelmezés szerint a port).</span><span class="sxs-lookup"><span data-stu-id="2bbce-116">The Service Fabric resource provider requires publicly accessible inbound access to the HTTP gateway port (port 19080, by default) on the management endpoint.</span></span> <span data-ttu-id="2bbce-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) használ a felügyeleti végpont a fürt kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2bbce-117">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) uses the management endpoint to manage your cluster.</span></span> <span data-ttu-id="2bbce-118">A Service Fabric erőforrás-szolgáltató ezen a porton lekérdezés adatait a fürt is használja az Azure portálon megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2bbce-118">The Service Fabric resource provider also uses this port to query information about your cluster, to display in the Azure portal.</span></span> 

<span data-ttu-id="2bbce-119">Ha port 19080 nem érhető el a Service Fabric erőforrás-szolgáltató, egy üzenet, például *csomópont nem található* megjelenik a portálon, és megjelenik a a csomópont- és alkalmazás listája üres.</span><span class="sxs-lookup"><span data-stu-id="2bbce-119">If port 19080 is not accessible from the Service Fabric resource provider, a message like *Nodes Not Found* appears in the portal, and your node and application list appears empty.</span></span> <span data-ttu-id="2bbce-120">Ha azt szeretné, hogy a fürt az Azure portálon, a terheléselosztó fel kell fednie egy nyilvános IP-címet, és a hálózati biztonsági csoport engedélyeznie kell a bejövő portot 19080 adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="2bbce-120">If you want to see your cluster in the Azure portal, your load balancer must expose a public IP address, and your network security group must allow incoming port 19080 traffic.</span></span> <span data-ttu-id="2bbce-121">A telepítő nem felel meg a követelménynek, ha az Azure-portál nem jelenik meg a fürt állapota.</span><span class="sxs-lookup"><span data-stu-id="2bbce-121">If your setup does not meet these requirements, the Azure portal does not display the status of your cluster.</span></span>

## <a name="templates"></a><span data-ttu-id="2bbce-122">Sablonok</span><span class="sxs-lookup"><span data-stu-id="2bbce-122">Templates</span></span>

<span data-ttu-id="2bbce-123">Az összes Service Fabric-sablonok vannak [egyik letöltendő fájl](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span><span class="sxs-lookup"><span data-stu-id="2bbce-123">All Service Fabric templates are in [one download file](https://msdnshared.blob.core.windows.net/media/2016/10/SF_Networking_Templates.zip).</span></span> <span data-ttu-id="2bbce-124">Meg kell telepíteni, a sablonok – a következő PowerShell-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="2bbce-124">You should be able to deploy the templates as-is by using the following PowerShell commands.</span></span> <span data-ttu-id="2bbce-125">Ha telepíti a meglévő Azure virtuális hálózat vagy a statikus nyilvános IP-sablont, először olvassa el a [telepítő kezdeti](#initialsetup) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="2bbce-125">If you are deploying the existing Azure Virtual Network template or the static public IP template, first read the [Initial setup](#initialsetup) section of this article.</span></span>

<a id="initialsetup"></a>
## <a name="initial-setup"></a><span data-ttu-id="2bbce-126">Kezdeti telepítés</span><span class="sxs-lookup"><span data-stu-id="2bbce-126">Initial setup</span></span>

### <a name="existing-virtual-network"></a><span data-ttu-id="2bbce-127">Meglévő virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="2bbce-127">Existing virtual network</span></span>

<span data-ttu-id="2bbce-128">A következő példában először nevű ExistingRG-hálózatok, a meglévő virtuális hálózat a **ExistingRG** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2bbce-128">In the following example, we start with an existing virtual network named ExistingRG-vnet, in the **ExistingRG** resource group.</span></span> <span data-ttu-id="2bbce-129">Az alhálózat neve alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="2bbce-129">The subnet is named default.</span></span> <span data-ttu-id="2bbce-130">Ezek az alapértelmezett erőforrások jönnek létre, ha az Azure-portál használatával hozzon létre egy szabványos virtuális gépet (VM).</span><span class="sxs-lookup"><span data-stu-id="2bbce-130">These default resources are created when you use the Azure portal to create a standard virtual machine (VM).</span></span> <span data-ttu-id="2bbce-131">Létrehozhatja a virtuális hálózati és alhálózati nélkül a virtuális gép létrehozása, de a fő céllal, a fürt hozzáadása egy meglévő virtuális hálózathoz hálózati kapcsolatot biztosít a többi virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="2bbce-131">You could create the virtual network and subnet without creating the VM, but the main goal of adding a cluster to an existing virtual network is to provide network connectivity to other VMs.</span></span> <span data-ttu-id="2bbce-132">A virtuális gép létrehozása biztosít a meglévő virtuális hálózat általában használatáról jó példa.</span><span class="sxs-lookup"><span data-stu-id="2bbce-132">Creating the VM gives a good example of how an existing virtual network typically is used.</span></span> <span data-ttu-id="2bbce-133">Ha a Service Fabric-fürt csak egy belső elosztott terhelésű, a nyilvános IP-cím nélkül is használhatja a virtuális gép és a nyilvános IP-cím a biztonságos *mezőben jump*.</span><span class="sxs-lookup"><span data-stu-id="2bbce-133">If your Service Fabric cluster uses only an internal load balancer, without a public IP address, you can use the VM and its public IP as a secure *jump box*.</span></span>

### <a name="static-public-ip-address"></a><span data-ttu-id="2bbce-134">Statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="2bbce-134">Static public IP address</span></span>

<span data-ttu-id="2bbce-135">Egy statikus nyilvános IP-cím általában egy dedikált erőforrást, amely a virtuális gép vagy a virtuális gépek hozzá van rendelve külön kezelnek.</span><span class="sxs-lookup"><span data-stu-id="2bbce-135">A static public IP address generally is a dedicated resource that's managed separately from the VM or VMs it's assigned to.</span></span> <span data-ttu-id="2bbce-136">Azt ki van építve a dedikált hálózati erőforráscsoportban (azaz a Service Fabric-fürt erőforrás csoportosítás maga nem).</span><span class="sxs-lookup"><span data-stu-id="2bbce-136">It's provisioned in a dedicated networking resource group (as opposed to in the Service Fabric cluster resource group itself).</span></span> <span data-ttu-id="2bbce-137">Hozzon létre egy statikus nyilvános IP-cím nevű staticIP1 azonos ExistingRG az erőforráscsoporthoz tartozik, az Azure portálon vagy a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="2bbce-137">Create a static public IP address named staticIP1 in the same ExistingRG resource group, either in the Azure portal or by using PowerShell:</span></span>

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

### <a name="service-fabric-template"></a><span data-ttu-id="2bbce-138">A Service Fabric-sablon</span><span class="sxs-lookup"><span data-stu-id="2bbce-138">Service Fabric template</span></span>

<span data-ttu-id="2bbce-139">A cikkben szereplő példák használjuk a Service Fabric template.json.</span><span class="sxs-lookup"><span data-stu-id="2bbce-139">In the examples in this article, we use the Service Fabric template.json.</span></span> <span data-ttu-id="2bbce-140">A szabványos portál varázsló segítségével a sablon letöltéséről a portálon, a fürt létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="2bbce-140">You can use the standard portal wizard to download the template from the portal before you create a cluster.</span></span> <span data-ttu-id="2bbce-141">Is használhatja a sablont a [sablon gyűjtemény](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), például a [öt csomópontból Service Fabric-fürt](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span><span class="sxs-lookup"><span data-stu-id="2bbce-141">You also can use one of the templates in the [template gallery](https://azure.microsoft.com/en-us/documentation/templates/?term=service+fabric), like the [five-node Service Fabric cluster](https://azure.microsoft.com/en-us/documentation/templates/service-fabric-unsecure-cluster-5-node-1-nodetype/).</span></span>

<a id="existingvnet"></a>
## <a name="existing-virtual-network-or-subnet"></a><span data-ttu-id="2bbce-142">Meglévő virtuális hálózathoz vagy alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="2bbce-142">Existing virtual network or subnet</span></span>

1. <span data-ttu-id="2bbce-143">Módosítsa a alhálózati paramétert a meglévő alhálózat nevét, és adja hozzá a két új paramétert a meglévő virtuális hálózat hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="2bbce-143">Change the subnet parameter to the name of the existing subnet, and then add two new parameters to reference the existing virtual network:</span></span>

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


2. <span data-ttu-id="2bbce-144">Módosítsa a `vnetID` változó, mutasson a meglévő virtuális hálózathoz:</span><span class="sxs-lookup"><span data-stu-id="2bbce-144">Change the `vnetID` variable to point to the existing virtual network:</span></span>

    ```
            /*old "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",*/
            "vnetID": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingVNetRGName'), '/providers/Microsoft.Network/virtualNetworks/', parameters('existingVNetName'))]",
    ```

3. <span data-ttu-id="2bbce-145">Távolítsa el `Microsoft.Network/virtualNetworks` az erőforrásokat, így Azure nem hozzon létre egy új virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="2bbce-145">Remove `Microsoft.Network/virtualNetworks` from your resources, so Azure does not create a new virtual network:</span></span>

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

4. <span data-ttu-id="2bbce-146">A virtuális hálózaton megjegyzésbe a `dependsOn` attribútumának `Microsoft.Compute/virtualMachineScaleSets`, így nem függ egy új virtuális hálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2bbce-146">Comment out the virtual network from the `dependsOn` attribute of `Microsoft.Compute/virtualMachineScaleSets`, so you don't depend on creating a new virtual network:</span></span>

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

5. <span data-ttu-id="2bbce-147">A sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="2bbce-147">Deploy the template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingexistingvnet -Location westus
    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingexistingvnet -TemplateFile C:\SFSamples\Final\template\_existingvnet.json
    ```

    <span data-ttu-id="2bbce-148">A központi telepítést követően a virtuális hálózat tartalmaznia kell az új virtuális gépek méretezési csoportjának virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="2bbce-148">After deployment, your virtual network should include the new scale set VMs.</span></span> <span data-ttu-id="2bbce-149">A virtuális gép méretezési készlet csomóponttípus meg kell jelennie a meglévő virtuális hálózat és alhálózat.</span><span class="sxs-lookup"><span data-stu-id="2bbce-149">The virtual machine scale set node type should show the existing virtual network and subnet.</span></span> <span data-ttu-id="2bbce-150">Is használhatja Remote Desktop Protocol (RDP) a virtuális Gépet, amely már szerepel a virtuális hálózat eléréséhez, és Pingelje meg az új állítsa be a virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="2bbce-150">You also can use Remote Desktop Protocol (RDP) to access the VM that was already in the virtual network, and to ping the new scale set VMs:</span></span>

    ```
    C:>\Users\users>ping 10.0.0.5 -n 1
    C:>\Users\users>ping NOde1000000 -n 1
    ```

<span data-ttu-id="2bbce-151">Egy másik példa, lásd: [, amely nem a Service Fabric jellemző](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="2bbce-151">For another example, see [one that is not specific to Service Fabric](https://github.com/gbowerman/azure-myriad/tree/master/existing-vnet).</span></span>


<a id="staticpublicip"></a>
## <a name="static-public-ip-address"></a><span data-ttu-id="2bbce-152">Statikus nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="2bbce-152">Static public IP address</span></span>

1. <span data-ttu-id="2bbce-153">Adja hozzá a statikus IP meglévő erőforráscsoport neve, a neve és a teljesen minősített tartománynevét (FQDN) paramétereinek:</span><span class="sxs-lookup"><span data-stu-id="2bbce-153">Add parameters for the name of the existing static IP resource group, name, and fully qualified domain name (FQDN):</span></span>

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

2. <span data-ttu-id="2bbce-154">Távolítsa el a `dnsName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2bbce-154">Remove the `dnsName` parameter.</span></span> <span data-ttu-id="2bbce-155">(A statikus IP-cím már szerepel ilyen.)</span><span class="sxs-lookup"><span data-stu-id="2bbce-155">(The static IP address already has one.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

3. <span data-ttu-id="2bbce-156">Egy változó való hivatkozáshoz a meglévő statikus IP-cím hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="2bbce-156">Add a variable to reference the existing static IP address:</span></span>

    ```
    "existingStaticIP": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', parameters('existingStaticIPResourceGroup'), '/providers/Microsoft.Network/publicIPAddresses/', parameters('existingStaticIPName'))]",
    ```

4. <span data-ttu-id="2bbce-157">Távolítsa el `Microsoft.Network/publicIPAddresses` az erőforrásokat, így Azure nem hozzon létre egy új IP-cím:</span><span class="sxs-lookup"><span data-stu-id="2bbce-157">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

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

5. <span data-ttu-id="2bbce-158">Az IP-cím a megjegyzésbe a `dependsOn` attribútumának `Microsoft.Network/loadBalancers`, így nem függ egy új IP-cím létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2bbce-158">Comment out the IP address from the `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address:</span></span>

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

6. <span data-ttu-id="2bbce-159">Az a `Microsoft.Network/loadBalancers` erőforrás, módosítsa a `publicIPAddress` eleme `frontendIPConfigurations` való hivatkozáshoz helyett egy újonnan létrehozott egy létező statikus IP-cím:</span><span class="sxs-lookup"><span data-stu-id="2bbce-159">In the `Microsoft.Network/loadBalancers` resource, change the `publicIPAddress` element of `frontendIPConfigurations` to reference the existing static IP address instead of a newly created one:</span></span>

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

7. <span data-ttu-id="2bbce-160">Az a `Microsoft.ServiceFabric/clusters` erőforrás, a módosítás `managementEndpoint` a statikus IP-címet a DNS teljes Tartományneve számára.</span><span class="sxs-lookup"><span data-stu-id="2bbce-160">In the `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` to the DNS FQDN of the static IP address.</span></span> <span data-ttu-id="2bbce-161">Ha egy biztonságos fürtöt használ, ellenőrizze, hogy megváltoztatja *http://* való *https://*.</span><span class="sxs-lookup"><span data-stu-id="2bbce-161">If you are using a secure cluster, make sure you change *http://* to *https://*.</span></span> <span data-ttu-id="2bbce-162">(Vegye figyelembe, hogy ez a lépés csak a Service Fabric-fürtök vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2bbce-162">(Note that this step applies only to Service Fabric clusters.</span></span> <span data-ttu-id="2bbce-163">Ha egy virtuálisgép-méretezési csoport használja, kihagyhatja ezt a lépést.)</span><span class="sxs-lookup"><span data-stu-id="2bbce-163">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',parameters('existingStaticIPDnsFQDN'),':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

8. <span data-ttu-id="2bbce-164">A sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="2bbce-164">Deploy the template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkingstaticip -Location westus

    $staticip = Get-AzureRmPublicIpAddress -Name staticIP1 -ResourceGroupName ExistingRG

    $staticip

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkingstaticip -TemplateFile C:\SFSamples\Final\template\_staticip.json -existingStaticIPResourceGroup $staticip.ResourceGroupName -existingStaticIPName $staticip.Name -existingStaticIPDnsFQDN $staticip.DnsSettings.Fqdn
    ```

<span data-ttu-id="2bbce-165">A központi telepítést követően láthatja, hogy a terheléselosztó van kötve a nyilvános statikus IP-cím az egyéb erőforráscsoportból.</span><span class="sxs-lookup"><span data-stu-id="2bbce-165">After deployment, you can see that your load balancer is bound to the public static IP address from the other resource group.</span></span> <span data-ttu-id="2bbce-166">A Service Fabric ügyfél-csatlakozási végpont és [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) végpont pontot a DNS FQDN a statikus IP-cím.</span><span class="sxs-lookup"><span data-stu-id="2bbce-166">The Service Fabric client connection endpoint and [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint point to the DNS FQDN of the static IP address.</span></span>

<a id="internallb"></a>
## <a name="internal-only-load-balancer"></a><span data-ttu-id="2bbce-167">Csak belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2bbce-167">Internal-only load balancer</span></span>

<span data-ttu-id="2bbce-168">Ebben a forgatókönyvben a külső terheléselosztóhoz, a Service Fabric alapértelmezett sablon lecseréli egy csak belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="2bbce-168">This scenario replaces the external load balancer in the default Service Fabric template with an internal-only load balancer.</span></span> <span data-ttu-id="2bbce-169">Az Azure-portál és a Service Fabric erőforrás-szolgáltató megvalósítását lásd: az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2bbce-169">For implications for the Azure portal and for the Service Fabric resource provider, see the preceding section.</span></span>

1. <span data-ttu-id="2bbce-170">Távolítsa el a `dnsName` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2bbce-170">Remove the `dnsName` parameter.</span></span> <span data-ttu-id="2bbce-171">(Nincs rá szükség.)</span><span class="sxs-lookup"><span data-stu-id="2bbce-171">(It's not needed.)</span></span>

    ```
    /*
    "dnsName": {
        "type": "string"
    },
    */
    ```

2. <span data-ttu-id="2bbce-172">Szükség esetén egy statikus kiosztási módszerrel használatakor is hozzáadhat egy statikus IP-cím paraméter.</span><span class="sxs-lookup"><span data-stu-id="2bbce-172">Optionally, if you use a static allocation method, you can add a static IP address parameter.</span></span> <span data-ttu-id="2bbce-173">Ha egy dinamikus elosztási módszert használ, nem szükséges ehhez a lépéshez.</span><span class="sxs-lookup"><span data-stu-id="2bbce-173">If you use a dynamic allocation method, you do not need to do this step.</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

3. <span data-ttu-id="2bbce-174">Távolítsa el `Microsoft.Network/publicIPAddresses` az erőforrásokat, így Azure nem hozzon létre egy új IP-cím:</span><span class="sxs-lookup"><span data-stu-id="2bbce-174">Remove `Microsoft.Network/publicIPAddresses` from your resources, so Azure does not create a new IP address:</span></span>

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

4. <span data-ttu-id="2bbce-175">Távolítsa el az IP-cím `dependsOn` attribútumának `Microsoft.Network/loadBalancers`, így nem függ egy új IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2bbce-175">Remove the IP address `dependsOn` attribute of `Microsoft.Network/loadBalancers`, so you don't depend on creating a new IP address.</span></span> <span data-ttu-id="2bbce-176">Adja hozzá a virtuális hálózatot `dependsOn` attribútumon, mert a terheléselosztó most a virtuális hálózati alhálózat függ:</span><span class="sxs-lookup"><span data-stu-id="2bbce-176">Add the virtual network `dependsOn` attribute because the load balancer now depends on the subnet from the virtual network:</span></span>

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

5. <span data-ttu-id="2bbce-177">Módosítsa a terheléselosztó `frontendIPConfigurations` beállítása a használatával egy `publicIPAddress`, alhálózat használatát és `privateIPAddress`.</span><span class="sxs-lookup"><span data-stu-id="2bbce-177">Change the load balancer's `frontendIPConfigurations` setting from using a `publicIPAddress`, to using a subnet and `privateIPAddress`.</span></span> <span data-ttu-id="2bbce-178">`privateIPAddress`egy előre meghatározott statikus belső IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="2bbce-178">`privateIPAddress` uses a predefined static internal IP address.</span></span> <span data-ttu-id="2bbce-179">A dinamikus IP-cím használatára, távolítsa el a `privateIPAddress` elemet, és módosítsa `privateIPAllocationMethod` való **dinamikus**.</span><span class="sxs-lookup"><span data-stu-id="2bbce-179">To use a dynamic IP address, remove the `privateIPAddress` element, and then change `privateIPAllocationMethod` to **Dynamic**.</span></span>

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

6. <span data-ttu-id="2bbce-180">Az a `Microsoft.ServiceFabric/clusters` erőforrás, a módosítás `managementEndpoint` úgy, hogy a belső terheléselosztó-címén mutasson.</span><span class="sxs-lookup"><span data-stu-id="2bbce-180">In the `Microsoft.ServiceFabric/clusters` resource, change `managementEndpoint` to point to the internal load balancer address.</span></span> <span data-ttu-id="2bbce-181">Ha biztonságos-fürtöt használ, ellenőrizze, hogy megváltoztatja *http://* való *https://*.</span><span class="sxs-lookup"><span data-stu-id="2bbce-181">If you use a secure cluster, make sure you change *http://* to *https://*.</span></span> <span data-ttu-id="2bbce-182">(Vegye figyelembe, hogy ez a lépés csak a Service Fabric-fürtök vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2bbce-182">(Note that this step applies only to Service Fabric clusters.</span></span> <span data-ttu-id="2bbce-183">Ha egy virtuálisgép-méretezési csoport használja, kihagyhatja ezt a lépést.)</span><span class="sxs-lookup"><span data-stu-id="2bbce-183">If you are using a virtual machine scale set, skip this step.)</span></span>

    ```
                    "fabricSettings": [],
                    /*"managementEndpoint": "[concat('http://',reference(concat(parameters('lbIPName'),'-','0')).dnsSettings.fqdn,':',parameters('nt0fabricHttpGatewayPort'))]",*/
                    "managementEndpoint": "[concat('http://',reference(variables('lbID0')).frontEndIPConfigurations[0].properties.privateIPAddress,':',parameters('nt0fabricHttpGatewayPort'))]",
    ```

7. <span data-ttu-id="2bbce-184">A sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="2bbce-184">Deploy the template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternallb -TemplateFile C:\SFSamples\Final\template\_internalonlyLB.json
    ```

<span data-ttu-id="2bbce-185">A központi telepítést követően a terheléselosztó a statikus 10.0.0.250 magánhálózati IP-címet használ.</span><span class="sxs-lookup"><span data-stu-id="2bbce-185">After deployment, your load balancer uses the private static 10.0.0.250 IP address.</span></span> <span data-ttu-id="2bbce-186">Ha egy másik gép ugyanazon virtuális hálózatban, lépjen a belső [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) végpont.</span><span class="sxs-lookup"><span data-stu-id="2bbce-186">If you have another machine in that same virtual network, you can go to the internal [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) endpoint.</span></span> <span data-ttu-id="2bbce-187">Figyelje meg, hogy a terheléselosztó mögött a csomópontok egyikét csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="2bbce-187">Note that it connects to one of the nodes behind the load balancer.</span></span>

<a id="internalexternallb"></a>
## <a name="internal-and-external-load-balancer"></a><span data-ttu-id="2bbce-188">Belső és külső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2bbce-188">Internal and external load balancer</span></span>

<span data-ttu-id="2bbce-189">Ebben a forgatókönyvben a meglévő egycsomópontos típus külső terheléselosztó kezdődnie, és vegye fel a csomópont ugyanolyan belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="2bbce-189">In this scenario, you start with the existing single-node type external load balancer, and add an internal load balancer for the same node type.</span></span> <span data-ttu-id="2bbce-190">A háttér-port, egy háttér címkészletet csatolva csak egyetlen terheléselosztót is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="2bbce-190">A back-end port attached to a back-end address pool can be assigned only to a single load balancer.</span></span> <span data-ttu-id="2bbce-191">Válassza ki, mely terheléselosztót kapnak az alkalmazás portok, valamint mely terheléselosztót fog rendelkezni a felügyeleti végpontok (portok 19000 és 19080).</span><span class="sxs-lookup"><span data-stu-id="2bbce-191">Choose which load balancer will have your application ports, and which load balancer will have your management endpoints (ports 19000 and 19080).</span></span> <span data-ttu-id="2bbce-192">Ha a felügyeleti végpontok a belső terheléselosztón, tartsa szem előtt a Service Fabric-erőforrás a cikkben korábban tárgyalt szolgáltató korlátozások.</span><span class="sxs-lookup"><span data-stu-id="2bbce-192">If you put the management endpoints on the internal load balancer, keep in mind the Service Fabric resource provider restrictions discussed earlier in the article.</span></span> <span data-ttu-id="2bbce-193">A példában használjuk, a felügyeleti végpontok maradnak a külső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="2bbce-193">In the example we use, the management endpoints remain on the external load balancer.</span></span> <span data-ttu-id="2bbce-194">Emellett adjon hozzá egy port 80 alkalmazás portot, és helyezze el a belső terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2bbce-194">You also add a port 80 application port, and place it on the internal load balancer.</span></span>

<span data-ttu-id="2bbce-195">A két csomóponttípus fürtben egy csomópont típus a külső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="2bbce-195">In a two-node-type cluster, one node type is on the external load balancer.</span></span> <span data-ttu-id="2bbce-196">A második csomópont típus esetén a belső terheléselosztón.</span><span class="sxs-lookup"><span data-stu-id="2bbce-196">The other node type is on the internal load balancer.</span></span> <span data-ttu-id="2bbce-197">A két csomóponttípus-fürtöt használ, a portál által létrehozott két csomóponttípus sablonban (Ez a két terheléselosztók), a második terheléselosztó átvált belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="2bbce-197">To use a two-node-type cluster, in the portal-created two-node-type template (which comes with two load balancers), switch the second load balancer to an internal load balancer.</span></span> <span data-ttu-id="2bbce-198">További információkért lásd: a [csak belső terheléselosztó](#internallb) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2bbce-198">For more information, see the [Internal-only load balancer](#internallb) section.</span></span>

1. <span data-ttu-id="2bbce-199">Adja hozzá a statikus belső load balancer IP-cím paraméter.</span><span class="sxs-lookup"><span data-stu-id="2bbce-199">Add the static internal load balancer IP address parameter.</span></span> <span data-ttu-id="2bbce-200">(Egy dinamikus IP-cím használatához kapcsolódó megjegyzések, tekintse meg a cikk korábbi szakaszaiban.)</span><span class="sxs-lookup"><span data-stu-id="2bbce-200">(For notes related to using a dynamic IP address, see earlier sections of this article.)</span></span>

    ```
            "internalLBAddress": {
                "type": "string",
                "defaultValue": "10.0.0.250"
            }
    ```

2. <span data-ttu-id="2bbce-201">Adja hozzá az alkalmazás 80-as port paramétert.</span><span class="sxs-lookup"><span data-stu-id="2bbce-201">Add an application port 80 parameter.</span></span>

3. <span data-ttu-id="2bbce-202">Belső a meglévő hálózati változók, másolja és illessze be a hozzáadni, majd adja meg a "-Int" nevét:</span><span class="sxs-lookup"><span data-stu-id="2bbce-202">To add internal versions of the existing networking variables, copy and paste them, and add "-Int" to the name:</span></span>

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

4. <span data-ttu-id="2bbce-203">Ha először a portál által létrehozott sablon, amely alkalmazás 80-as portot használja, a portál alapértelmezett sablon hozzáadása AppPort1 (80-as port) a külső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="2bbce-203">If you start with the portal-generated template that uses application port 80, the default portal template adds AppPort1 (port 80) on the external load balancer.</span></span> <span data-ttu-id="2bbce-204">Ebben az esetben távolítsa el a külső terheléselosztó AppPort1 `loadBalancingRules` és mintavételek menüpontban, így adhat hozzá a belső terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="2bbce-204">In this case, remove AppPort1 from the external load balancer `loadBalancingRules` and probes, so you can add it to the internal load balancer:</span></span>

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
        } /* Remove AppPort1 from the external load balancer.
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
        } /* Remove AppPort1 from the external load balancer.
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

5. <span data-ttu-id="2bbce-205">Adja hozzá egy második `Microsoft.Network/loadBalancers` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2bbce-205">Add a second `Microsoft.Network/loadBalancers` resource.</span></span> <span data-ttu-id="2bbce-206">A belső terheléselosztó létrehozott hasonló a [csak belső terheléselosztó](#internallb) szakaszában, de használja a "-Int" terheléselosztó változók betölteni, és megvalósítja az csak az alkalmazás 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="2bbce-206">It looks similar to the internal load balancer created in the [Internal-only load balancer](#internallb) section, but it uses the "-Int" load balancer variables, and implements only the application port 80.</span></span> <span data-ttu-id="2bbce-207">Ez eltávolítja `inboundNatPools`, hogy továbbra is RDP-végpontot a nyilvános terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2bbce-207">This also removes `inboundNatPools`, to keep RDP endpoints on the public load balancer.</span></span> <span data-ttu-id="2bbce-208">Az RDP a belső terheléselosztón, helyezze `inboundNatPools` a külső terheléselosztóhoz, a belső terheléselosztóhoz:</span><span class="sxs-lookup"><span data-stu-id="2bbce-208">If you want RDP on the internal load balancer, move `inboundNatPools` from the external load balancer to this internal load balancer:</span></span>

    ```
            /* Add a second load balancer, configured with a static privateIPAddress and the "-Int" load balancer variables. */
            {
                "apiVersion": "[variables('lbApiVersion')]",
                "type": "Microsoft.Network/loadBalancers",
                /* Add "-Internal" to the name. */
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
                                /* Switch from Public to Private IP address
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
                        /* Add the AppPort rule. Be sure to reference the "-Int" versions of backendAddressPool, frontendIPConfiguration, and the probe variables. */
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
                    /* Add the probe for the app port. */
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

6. <span data-ttu-id="2bbce-209">A `networkProfile` a a `Microsoft.Compute/virtualMachineScaleSets` erőforrás, a belső háttér-címkészlet hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="2bbce-209">In `networkProfile` for the `Microsoft.Compute/virtualMachineScaleSets` resource, add the internal back-end address pool:</span></span>

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

7. <span data-ttu-id="2bbce-210">A sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="2bbce-210">Deploy the template:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name sfnetworkinginternalexternallb -Location westus

    New-AzureRmResourceGroupDeployment -Name deployment -ResourceGroupName sfnetworkinginternalexternallb -TemplateFile C:\SFSamples\Final\template\_internalexternalLB.json
    ```

<span data-ttu-id="2bbce-211">A központi telepítést követően az erőforráscsoportban két terheléselosztók tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2bbce-211">After deployment, you can see two load balancers in the resource group.</span></span> <span data-ttu-id="2bbce-212">Ha tallózással azokat a terheléselosztókat, megtekintheti a nyilvános IP cím és felügyeleti végpontok (19000 és 19080 portok) a nyilvános IP-címet hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="2bbce-212">If you browse the load balancers, you can see the public IP address and management endpoints (ports 19000 and 19080) assigned to the public IP address.</span></span> <span data-ttu-id="2bbce-213">A statikus belső IP-cím és az alkalmazás végponton (80-as port) a belső terheléselosztó rendelt is látható.</span><span class="sxs-lookup"><span data-stu-id="2bbce-213">You also can see the static internal IP address and application endpoint (port 80) assigned to the internal load balancer.</span></span> <span data-ttu-id="2bbce-214">Mindkét terheléselosztók használja a virtuális gép méretezési készlet háttér-készlethez.</span><span class="sxs-lookup"><span data-stu-id="2bbce-214">Both load balancers use the same virtual machine scale set back-end pool.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bbce-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2bbce-215">Next steps</span></span>
[<span data-ttu-id="2bbce-216">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bbce-216">Create a cluster</span></span>](service-fabric-cluster-creation-via-arm.md)
