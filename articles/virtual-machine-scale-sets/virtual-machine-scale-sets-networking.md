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
# <a name="networking-for-azure-virtual-machine-scale-sets"></a><span data-ttu-id="9732c-103">Azure-beli virtuálisgép-méretezési csoportok hálózatkezelése</span><span class="sxs-lookup"><span data-stu-id="9732c-103">Networking for Azure virtual machine scale sets</span></span>

<span data-ttu-id="9732c-104">Amikor telepít egy Azure virtuálisgép-méretezési hello portálon keresztül beállítani, bizonyos hálózati tulajdonságainak vannak alapértelmezésnek megfelelően, például rendelkező Azure terheléselosztó bejövő forgalmat kezelő NAT-szabályok.</span><span class="sxs-lookup"><span data-stu-id="9732c-104">When you deploy an Azure virtual machine scale set through hello portal, certain network properties are defaulted, for example an Azure Load Balancer with inbound NAT rules.</span></span> <span data-ttu-id="9732c-105">Ez a cikk ismerteti, hogyan néhány hello fejlettebb hálózatkezelési szolgáltatásaival, amely konfigurálható toouse állítja be.</span><span class="sxs-lookup"><span data-stu-id="9732c-105">This article describes how toouse some of hello more advanced networking features that you can configure with scale sets.</span></span>

<span data-ttu-id="9732c-106">Konfigurálhatja az Azure Resource Manager-sablonokkal a cikkben szereplő hello funkciók.</span><span class="sxs-lookup"><span data-stu-id="9732c-106">You can configure all of hello features covered in this article using Azure Resource Manager templates.</span></span> <span data-ttu-id="9732c-107">Egyes szolgáltatások esetében az Azure CLI-hez és PowerShellhez is találhat példákat.</span><span class="sxs-lookup"><span data-stu-id="9732c-107">Azure CLI and PowerShell examples are also included for selected features.</span></span> <span data-ttu-id="9732c-108">Használja a parancssori felület 2.10-es vagy újabb, illetve a PowerShell 4.2.0-s vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="9732c-108">Use CLI 2.10, and PowerShell 4.2.0 or later.</span></span>

## <a name="accelerated-networking"></a><span data-ttu-id="9732c-109">Gyorsított hálózatkezelés</span><span class="sxs-lookup"><span data-stu-id="9732c-109">Accelerated Networking</span></span>
<span data-ttu-id="9732c-110">Azure [az elérését gyorsítja fel hálózati](../virtual-network/virtual-network-create-vm-accelerated-networking.md) egygyökerű i/o-virtualizálás (SR-IOV) tooa virtuális gép engedélyezésével javítja a hálózati teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="9732c-110">Azure [Accelerated Networking](../virtual-network/virtual-network-create-vm-accelerated-networking.md) improves  network performance by enabling single root I/O virtualization (SR-IOV) tooa virtual machine.</span></span> <span data-ttu-id="9732c-111">toouse elérését gyorsítja fel, a méretezési készlet hálózati, állítsa be a enableAcceleratedNetworking túl**igaz** a méretezési készletet networkinterfaceconfigurations tulajdonságok beállításai.</span><span class="sxs-lookup"><span data-stu-id="9732c-111">toouse accelerated networking with scale sets, set enableAcceleratedNetworking too**true** in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="9732c-112">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-112">For example:</span></span>
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

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a><span data-ttu-id="9732c-113">Már létező Azure Load Balancerre hivatkozó méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9732c-113">Create a scale set that references an existing Azure Load Balancer</span></span>
<span data-ttu-id="9732c-114">Amikor egy méretezési hello Azure-portál használatával hoz létre, új terheléselosztó legtöbb konfigurációs beállítás jön létre.</span><span class="sxs-lookup"><span data-stu-id="9732c-114">When a scale set is created using hello Azure portal, a new load balancer is created for most configuration options.</span></span> <span data-ttu-id="9732c-115">Ha a meglévő terheléselosztó hoz létre egy méretezési, amelyekre szüksége van a tooreference, ehhez parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="9732c-115">If you create a scale set, which needs tooreference an existing load balancer, you can do this using CLI.</span></span> <span data-ttu-id="9732c-116">a következő példa parancsfájl hello új terheléselosztó létrehozása, és létrehoz egy méretezési csoport, amely hivatkozik rá:</span><span class="sxs-lookup"><span data-stu-id="9732c-116">hello following example script creates a load balancer and then creates a scale set, which references it:</span></span>
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a><span data-ttu-id="9732c-117">Konfigurálható DNS-beállítások</span><span class="sxs-lookup"><span data-stu-id="9732c-117">Configurable DNS Settings</span></span>
<span data-ttu-id="9732c-118">Alapértelmezés szerint méretezési csoportok hello adott DNS-beállítások hello VNET és alhálózat, a létrehozásuk igénybe.</span><span class="sxs-lookup"><span data-stu-id="9732c-118">By default, scale sets take on hello specific DNS settings of hello VNET and subnet they were created in.</span></span> <span data-ttu-id="9732c-119">Ugyanakkor a méretezési készletben közvetlenül hello DNS-beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="9732c-119">You can however, configure hello DNS settings for a scale set directly.</span></span>
~
### <a name="creating-a-scale-set-with-configurable-dns-servers"></a><span data-ttu-id="9732c-120">Konfigurálható DNS-kiszolgálókkal rendelkező méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9732c-120">Creating a scale set with configurable DNS servers</span></span>
<span data-ttu-id="9732c-121">a méretezési CLI 2.0 használatával egyéni DNS-konfiguráció toocreate hozzáadása hello **--dns-kiszolgálók** argumentum toohello **vmss létrehozása** parancsot, és szóköz elválasztott IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="9732c-121">toocreate a scale set with a custom DNS configuration using CLI 2.0, add hello **--dns-servers** argument toohello **vmss create** command, followed by space separated server ip addresses.</span></span> <span data-ttu-id="9732c-122">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-122">For example:</span></span>
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
<span data-ttu-id="9732c-123">tooconfigure egyéni DNS-kiszolgálók az Azure-sablon alapján, adjon hozzá egy dnsSettings tulajdonság toohello méretezési networkinterfaceconfigurations tulajdonságok szakasz.</span><span class="sxs-lookup"><span data-stu-id="9732c-123">tooconfigure custom DNS servers in an Azure template, add a dnsSettings property toohello scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="9732c-124">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-124">For example:</span></span>
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a><span data-ttu-id="9732c-125">Konfigurálható virtuálisgép-tartománynevekkel rendelkező méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="9732c-125">Creating a scale set with configurable virtual machine domain names</span></span>
<span data-ttu-id="9732c-126">egy egyéni DNS-név CLI 2.0 használó virtuális gépek méretezési toocreate hozzáadása hello **– virtuálisgép-tartománynév** argumentum toohello **vmss létrehozása** parancsot, utána pedig egy karakterlánc, amely hello tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="9732c-126">toocreate a scale set with a custom DNS name for virtual machines using CLI 2.0, add hello **--vm-domain-name** argument toohello **vmss create** command, followed by a string representing hello domain name.</span></span>

<span data-ttu-id="9732c-127">Azure-sablon alapján, tooset hello tartománynév hozzáadása egy **dnsSettings** tulajdonság toohello méretezési **networkinterfaceconfigurations tulajdonságok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9732c-127">tooset hello domain name in an Azure template, add a **dnsSettings** property toohello scale set **networkInterfaceConfigurations** section.</span></span> <span data-ttu-id="9732c-128">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-128">For example:</span></span>

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

<span data-ttu-id="9732c-129">a következő képernyő hello hello kimenete, egy különálló virtuális gép DNS-névvel lenne:</span><span class="sxs-lookup"><span data-stu-id="9732c-129">hello output, for an individual virtual machine dns name would be in hello following form:</span></span> 
```
<vm><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="public-ipv4-per-virtual-machine"></a><span data-ttu-id="9732c-130">Nyilvános IPv4-cím virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="9732c-130">Public IPv4 per virtual machine</span></span>
<span data-ttu-id="9732c-131">Az Azure méretezési csoportok virtuális gépeinek általában nincs szükségük saját nyilvános IP-címre.</span><span class="sxs-lookup"><span data-stu-id="9732c-131">In general, Azure scale set virtual machines do not require their own public IP addresses.</span></span> <span data-ttu-id="9732c-132">A legtöbb esetben célszerű gazdaságos, és biztonságos tooassociate egy nyilvános IP cím tooa terhelés terheléselosztót vagy tooan különálló virtuális gép (más néven a jumpbox), amely ezután továbbítja a bejövő kapcsolatok tooscale beállított virtuális gépek, (például keresztül igény szerint bejövő NAT-szabályok).</span><span class="sxs-lookup"><span data-stu-id="9732c-132">For most scenarios, it is more economical and secure tooassociate a public IP address tooa load balancer or tooan individual virtual machine (aka a jumpbox), which then routes incoming connections tooscale set virtual machines as needed (for example, through inbound NAT rules).</span></span>

<span data-ttu-id="9732c-133">Azonban bizonyos esetekben szükséges-méretezési csoport virtuális gépek toohave a saját nyilvános IP-cím címek.</span><span class="sxs-lookup"><span data-stu-id="9732c-133">However, some scenarios do require scale set virtual machines toohave their own public IP addresses.</span></span> <span data-ttu-id="9732c-134">Példa játékok, amikor a konzol kell toomake egy közvetlen kapcsolat tooa felhő virtuális gépet, mely játék fizikai feldolgozási végez műveletet.</span><span class="sxs-lookup"><span data-stu-id="9732c-134">An example is gaming, where a console needs toomake a direct connection tooa cloud virtual machine, which is doing game physics processing.</span></span> <span data-ttu-id="9732c-135">Egy másik példa: Ha a virtuális gépek toomake külső kapcsolatok tooone kell egy másik egy elosztott adatbázist régiók között.</span><span class="sxs-lookup"><span data-stu-id="9732c-135">Another example is where virtual machines need toomake external connections tooone another across regions in a distributed database.</span></span>

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a><span data-ttu-id="9732c-136">Méretezési csoport létrehozása úgy, hogy minden virtuális gép saját IP-címmel rendelkezzen</span><span class="sxs-lookup"><span data-stu-id="9732c-136">Creating a scale set with public IP per virtual machine</span></span>
<span data-ttu-id="9732c-137">egy nyilvános IP cím tooeach rendelkező virtuális gép CLI 2.0 rendelő méretezési toocreate hozzáadása hello **– nyilvános-ip-– a virtuális gépenkénti** paraméter toohello **vmss létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9732c-137">toocreate a scale set that assigns a public IP address tooeach virtual machine with CLI 2.0, add hello **--public-ip-per-vm** parameter toohello **vmss create** command.</span></span> 

<span data-ttu-id="9732c-138">toocreate egy méretezési készletben, Azure-sablon alapján, ellenőrizze, hogy hello API hello Microsoft.Compute/virtualMachineScaleSets erőforrás verziója legalább **2017-03-30**, és adja hozzá a **publicIpAddressConfiguration**JSON tulajdonság toohello méretezési IP-konfigurációk szakasz.</span><span class="sxs-lookup"><span data-stu-id="9732c-138">toocreate a scale set using an Azure template, make sure hello API version of hello Microsoft.Compute/virtualMachineScaleSets resource is at least **2017-03-30**, and add a **publicIpAddressConfiguration** JSON property toohello scale set ipConfigurations section.</span></span> <span data-ttu-id="9732c-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-139">For example:</span></span>

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
<span data-ttu-id="9732c-140">Példasablon: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)</span><span class="sxs-lookup"><span data-stu-id="9732c-140">Example template: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)</span></span>

### <a name="querying-hello-public-ip-addresses-of-hello-virtual-machines-in-a-scale-set"></a><span data-ttu-id="9732c-141">A méretezési hello virtuális gépek címeinek beállítása hello nyilvános IP-cím lekérdezése</span><span class="sxs-lookup"><span data-stu-id="9732c-141">Querying hello public IP addresses of hello virtual machines in a scale set</span></span>
<span data-ttu-id="9732c-142">toolist hello nyilvános IP-címek hozzárendelve tooscale beállított virtuális gépek CLI 2.0 használatával, használja a hello **az vmss lista-példány-nyilvános-IP-címek** parancsot.</span><span class="sxs-lookup"><span data-stu-id="9732c-142">toolist hello public IP addresses assigned tooscale set virtual machines using CLI 2.0, use hello **az vmss list-instance-public-ips** command.</span></span>

<span data-ttu-id="9732c-143">a méretezési toolist PowerShell-lel nyilvános IP-címek, hello használata _Get-AzureRmPublicIpAddress_ parancsot.</span><span class="sxs-lookup"><span data-stu-id="9732c-143">toolist scale set public IP addresses using PowerShell, use hello _Get-AzureRmPublicIpAddress_ command.</span></span> <span data-ttu-id="9732c-144">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-144">For example:</span></span>
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -VirtualMachineScaleSetName myvmss
```

<span data-ttu-id="9732c-145">Is lekérdezés hello nyilvános IP-címek közvetlenül Vezérlőpultjának hello nyilvános IP-címének konfigurációja hello erőforrás-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9732c-145">You can also query hello public IP addresses by referencing hello resource id of hello public IP address configuration directly.</span></span> <span data-ttu-id="9732c-146">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-146">For example:</span></span>
```PowerShell
PS C:\> Get-AzureRmPublicIpAddress -ResourceGroupName myrg -Name myvmsspip
```

<span data-ttu-id="9732c-147">tooquery hello nyilvános IP-címek hozzárendelve tooscale beállított virtuális gépek hello [Azure erőforrás-kezelő](https://resources.azure.com), vagy a verziójával Azure REST API hello **2017-03-30** vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="9732c-147">tooquery hello public IP addresses assigned tooscale set virtual machines using hello [Azure Resource Explorer](https://resources.azure.com), or hello Azure REST API with version **2017-03-30** or higher.</span></span>

<span data-ttu-id="9732c-148">Tekintse meg a méretezési készletben, hello erőforrás-kezelő használatával tooview nyilvános IP-címek hello **publicipaddresses** a méretezési szakasza.</span><span class="sxs-lookup"><span data-stu-id="9732c-148">tooview public IP addresses for a scale set using hello Resource Explorer, look at hello **publicipaddresses** section under your scale set.</span></span> <span data-ttu-id="9732c-149">Például: https://resources.azure.com/subscriptions/_saját_előfizetési_azonosító_/resourceGroups/_saját_erőforráscsoport_/providers/Microsoft.Compute/virtualMachineScaleSets/_saját_vmss_/publicipaddresses</span><span class="sxs-lookup"><span data-stu-id="9732c-149">For example: https://resources.azure.com/subscriptions/_your_sub_id_/resourceGroups/_your_rg_/providers/Microsoft.Compute/virtualMachineScaleSets/_your_vmss_/publicipaddresses</span></span>

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

<span data-ttu-id="9732c-150">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="9732c-150">Example output:</span></span>
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

## <a name="multiple-ip-addresses-per-nic"></a><span data-ttu-id="9732c-151">Több IP-cím hálózati adapterenként</span><span class="sxs-lookup"><span data-stu-id="9732c-151">Multiple IP addresses per NIC</span></span>
<span data-ttu-id="9732c-152">Minden hálózati adapter Virtuálisgép-méretezési csoportban lévő lehet vele társított egy vagy több IP-konfigurációk tooa csatolni.</span><span class="sxs-lookup"><span data-stu-id="9732c-152">Every NIC attached tooa VM in a scale set can have one or more IP configurations associated with it.</span></span> <span data-ttu-id="9732c-153">Az egyes konfigurációkhoz egy magánhálózati IP-cím van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="9732c-153">Each configuration is assigned one private IP address.</span></span> <span data-ttu-id="9732c-154">Az egyes konfigurációkhoz egy nyilvános IP-cím erőforrás is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="9732c-154">Each configuration may also have one public IP address resource associated with it.</span></span> <span data-ttu-id="9732c-155">hány IP-címek is toounderstand rendelhető tooa hálózati adapter, és használhatja az Azure-előfizetéssel, hogy hány nyilvános IP-címek túl[Azure korlátozását](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).</span><span class="sxs-lookup"><span data-stu-id="9732c-155">toounderstand how many IP addresses can be assigned tooa NIC, and how many public IP addresses you can use in an Azure subscription, refer too[Azure limits](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).</span></span>

## <a name="multiple-nics-per-virtual-machine"></a><span data-ttu-id="9732c-156">Több hálózati adapter virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="9732c-156">Multiple NICs per virtual machine</span></span>
<span data-ttu-id="9732c-157">Akkor is fel too8 hálózati adapterek, virtuális gépenként, attól függően, hogy a gép méretét.</span><span class="sxs-lookup"><span data-stu-id="9732c-157">You can have up too8 NICs per virtual machine, depending on machine size.</span></span> <span data-ttu-id="9732c-158">hello maximális hálózati adapterek száma gépenként érhető el hello [virtuális gép mérete cikk](../virtual-machines/windows/sizes.md).</span><span class="sxs-lookup"><span data-stu-id="9732c-158">hello maximum number of NICs per machine is available in hello [VM size article](../virtual-machines/windows/sizes.md).</span></span> <span data-ttu-id="9732c-159">hello alábbi lehet például egy méretezési több hálózati adapter bejegyzést, és több nyilvános IP-címek egy virtuális gép hálózati profil:</span><span class="sxs-lookup"><span data-stu-id="9732c-159">hello following example is a scale set network profile showing multiple NIC entries, and multiple public IPs per virtual machine:</span></span>
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

## <a name="nsg-per-scale-set"></a><span data-ttu-id="9732c-160">Hálózati biztonsági csoportok virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="9732c-160">NSG per scale set</span></span>
<span data-ttu-id="9732c-161">Hálózati biztonsági csoport közvetlen tooa méretezési, adja hozzá egy hivatkozást toohello hálózati illesztő konfigurációs szakasz hello skála virtuális gép beállításainak megadása lehet alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="9732c-161">Network Security Groups can be applied directly tooa scale set, by adding a reference toohello network interface configuration section of hello scale set virtual machine properties.</span></span>

<span data-ttu-id="9732c-162">Példa:</span><span class="sxs-lookup"><span data-stu-id="9732c-162">For example:</span></span> 
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

## <a name="next-steps"></a><span data-ttu-id="9732c-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9732c-163">Next steps</span></span>
<span data-ttu-id="9732c-164">Egy Azure virtuális hálózatot kapcsolatos további információkért tekintse meg túl[ebben a dokumentációban](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9732c-164">For more information about Azure virtual networks, refer too[this documentation](../virtual-network/virtual-networks-overview.md).</span></span>
