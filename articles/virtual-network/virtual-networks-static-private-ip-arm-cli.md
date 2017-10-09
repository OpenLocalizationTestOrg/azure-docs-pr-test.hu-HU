---
title: "virtuális gépek - Azure CLI 2.0 aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek hello Azure parancssori felület (CLI) 2.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0e278e6ac63c0cda061cf70ab0edfaff5491c03b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-20"></a><span data-ttu-id="0ffd4-103">Hello Azure CLI 2.0 használatával virtuális gépek magánhálózati IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0ffd4-103">Configure private IP addresses for a virtual machine using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="0ffd4-104">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="0ffd4-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="0ffd4-105">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="0ffd4-106">[Az Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="0ffd4-106">[Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="0ffd4-107">[Az Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="0ffd4-107">[Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="0ffd4-108">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="0ffd4-109">Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd4-109">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> <span data-ttu-id="0ffd4-110">minta hello Azure CLI 2.0 az alábbi parancsok már létrehozott egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-110">hello sample Azure CLI 2.0 commands below expect a simple environment already created.</span></span> <span data-ttu-id="0ffd4-111">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0ffd4-111">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-cli.md).</span></span>

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="0ffd4-112">Adjon meg egy statikus magánhálózati IP-cím, amikor a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0ffd4-112">Specify a static private IP address when creating a VM</span></span>

<span data-ttu-id="0ffd4-113">toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-113">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="0ffd4-114">Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="0ffd4-114">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="0ffd4-115">Hozzon létre egy nyilvános IP-cím hello VM hello [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-115">Create a public IP for hello VM with hello [az network public-ip create](/cli/azure/network/public-ip#create) command.</span></span> <span data-ttu-id="0ffd4-116">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-116">hello list shown after hello output explains hello parameters used.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ffd4-117">Azt szeretné, vagy a környezettől függően toouse különböző érték is az argumentumok ezen és a későbbi lépésekben kell.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-117">You may want or need toouse different values for your arguments in this and subsequent steps, depending upon your environment.</span></span>
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    <span data-ttu-id="0ffd4-118">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-118">Expected output:</span></span>
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * <span data-ttu-id="0ffd4-119">`--resource-group`: A toocreate hello nyilvános IP-hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-119">`--resource-group`: Name of hello resource group in which toocreate hello public IP.</span></span>
   * <span data-ttu-id="0ffd4-120">`--name`: Hello nyilvános IP-cím neve.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-120">`--name`: Name of hello public IP.</span></span>
   * <span data-ttu-id="0ffd4-121">`--location`: Mely toocreate hello nyilvános IP-cím azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-121">`--location`: Azure region in which toocreate hello public IP.</span></span>

3. <span data-ttu-id="0ffd4-122">Hello futtatása [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create) parancs toocreate statikus magánhálózati IP-címe a hálózati Adapterhez.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-122">Run hello [az network nic create](/cli/azure/network/nic#create) command toocreate a NIC with a static private IP.</span></span> <span data-ttu-id="0ffd4-123">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-123">hello list shown after hello output explains hello parameters used.</span></span> 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="0ffd4-124">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-124">Expected output:</span></span>
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    <span data-ttu-id="0ffd4-125">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-125">Parameters:</span></span>

    * <span data-ttu-id="0ffd4-126">`--private-ip-address`: Saját IP-cím statikus hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-126">`--private-ip-address`: Static private IP address for hello NIC.</span></span>
    * <span data-ttu-id="0ffd4-127">`--vnet-name`: Name hello vnet mely toocreate a hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-127">`--vnet-name`: Name of hello VNet in wihch toocreate hello NIC.</span></span>
    * <span data-ttu-id="0ffd4-128">`--subnet`: Mely toocreate hello hálózati hello alhálózatának nevét</span><span class="sxs-lookup"><span data-stu-id="0ffd4-128">`--subnet`: Name of hello subnet in which toocreate hello NIC.</span></span>

4. <span data-ttu-id="0ffd4-129">Futtassa a hello [azure virtuális gép létrehozása](/cli/azure/vm/nic#create) parancs toocreate hello hello nyilvános IP-cím és a hálózati adapter használó virtuális gépek a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-129">Run hello [azure vm create](/cli/azure/vm/nic#create) command toocreate hello VM using hello public IP and NIC created above.</span></span> <span data-ttu-id="0ffd4-130">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-130">hello list shown after hello output explains hello parameters used.</span></span>
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    <span data-ttu-id="0ffd4-131">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-131">Expected output:</span></span>
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   <span data-ttu-id="0ffd4-132">Alapszintű hello más paramétereket [az virtuális gép létrehozása](/cli/azure/vm#create) paraméterek.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-132">Parameters other than hello basic [az vm create](/cli/azure/vm#create) parameters.</span></span>

   * <span data-ttu-id="0ffd4-133">`--nics`: Hello NIC toowhich hello virtuális gép neve van csatolva.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-133">`--nics`: Name of hello NIC toowhich hello VM is attached.</span></span>
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="0ffd4-134">Statikus magánhálózati IP-címadatok a virtuális gép beolvasása</span><span class="sxs-lookup"><span data-stu-id="0ffd4-134">Retrieve static private IP address information for a VM</span></span>

<span data-ttu-id="0ffd4-135">tooview hello statikus magánhálózati IP-cím létrehozott, futtassa a következő parancs az Azure parancssori felület hello, és tekintse meg az értékeket hello *privát IP-foglalási-metódus* és *magánhálózati IP-cím*:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-135">tooview hello static private IP address that you created, run hello following Azure CLI command and observe hello values for *Private IP alloc-method* and *Private IP address*:</span></span>

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

<span data-ttu-id="0ffd4-136">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-136">Expected output:</span></span>

```json
"192.168.1.101"
```

<span data-ttu-id="0ffd4-137">toodisplay kifejezetten hello ezt a virtuális Gépet, a hálózati adapter lekérdezés hello hello NIC az adott IP-információit:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-137">toodisplay hello specific IP information of hello NIC for that VM, query hello NIC specifically:</span></span>

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

<span data-ttu-id="0ffd4-138">hello eredménye a következőhöz hasonlóan:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-138">hello output is something like:</span></span>

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="0ffd4-139">A statikus magánhálózati IP-cím eltávolítása a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="0ffd4-139">Remove a static private IP address from a VM</span></span>

<span data-ttu-id="0ffd4-140">A resource manager üzembe helyezések hozzá statikus magánhálózati IP-cím nem távolítható el az Azure parancssori felület a hálózati Adapterhez.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-140">You cannot remove a static private IP address from a NIC in Azure CLI for resource manager deployments.</span></span> <span data-ttu-id="0ffd4-141">A következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-141">You must:</span></span>
- <span data-ttu-id="0ffd4-142">Hozzon létre egy új hálózati Adaptert, amely egy dinamikus IP-cím használja</span><span class="sxs-lookup"><span data-stu-id="0ffd4-142">Create a new NIC that uses a dynamic IP</span></span>
- <span data-ttu-id="0ffd4-143">A hálózati adapter hello be VM hello hello az újonnan létrehozott hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-143">Set hello NIC on hello VM do hello newly created NIC.</span></span> 

<span data-ttu-id="0ffd4-144">a virtuális gép használt a fenti hello parancsok hello NIC toochange hello kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-144">toochange hello NIC for hello VM used in hello commands above, follow hello steps below.</span></span>

1. <span data-ttu-id="0ffd4-145">Futtassa a hello **azure-hálózat hálózati adapter létrehozása** toocreate parancsot egy új hálózati Adaptert dinamikus IP-lefoglalást használatával új IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-145">Run hello **azure network nic create** command toocreate a new NIC using dynamic IP allocation with a new IP address.</span></span> <span data-ttu-id="0ffd4-146">Vegye figyelembe, hogy a megadott IP-cím, mert hello elosztási módszert **dinamikus**.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-146">Note that because no IP address is specified, hello allocation method is **Dynamic**.</span></span>

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    <span data-ttu-id="0ffd4-147">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-147">Expected output:</span></span>

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. <span data-ttu-id="0ffd4-148">Futtassa a hello **azure virtuálisgép-készlet** parancs toochange hello hello virtuális gép által használt hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-148">Run hello **azure vm set** command toochange hello NIC used by hello VM.</span></span>
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    <span data-ttu-id="0ffd4-149">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="0ffd4-149">Expected output:</span></span>
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > <span data-ttu-id="0ffd4-150">Ha hello virtuális gép elég nagy toohave egynél több hálózati adapter, futtassa a hello **azure-hálózat hálózati törlése** parancs toodelete hello régi hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-150">If hello VM is large enough toohave more than one NIC, run hello **azure network nic delete** command toodelete hello old NIC.</span></span>
   
## <a name="next-steps"></a><span data-ttu-id="0ffd4-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ffd4-151">Next steps</span></span>
* <span data-ttu-id="0ffd4-152">További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="0ffd4-153">További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="0ffd4-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="0ffd4-154">Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ffd4-154">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

