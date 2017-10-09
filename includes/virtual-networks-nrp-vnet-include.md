## <a name="virtual-network"></a><span data-ttu-id="ec1e0-101">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="ec1e0-101">Virtual Network</span></span>
<span data-ttu-id="ec1e0-102">Virtuális hálózatok (VNET) és alhálózatok erőforrások segítségével határozza meg az Azure-ban futó munkaterhelések biztonsági korlátot.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-102">Virtual Networks (VNET) and subnets resources help define a security boundary for workloads running in Azure.</span></span> <span data-ttu-id="ec1e0-103">Egy VNet jellemzőek, címterekhez, meghatározott CIDR-blokkok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-103">A VNet is characterized by a collection of address spaces, defined as CIDR blocks.</span></span> 

> [!NOTE]
> <span data-ttu-id="ec1e0-104">A hálózati rendszergazdák jártas CIDR-formátumban.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-104">Network administrators are familiar with CIDR notation.</span></span> <span data-ttu-id="ec1e0-105">Ha nem ismeri a CIDR, [olvashat azokról bővebben](http://whatismyipaddress.com/cidr).</span><span class="sxs-lookup"><span data-stu-id="ec1e0-105">If you are not familiar with CIDR, [learn more about it](http://whatismyipaddress.com/cidr).</span></span>
> 
> 

![Virtuális hálózat, több alhálózattal](./media/resource-groups-networking/Figure4.png)

<span data-ttu-id="ec1e0-107">Vnetek hello következő tulajdonságai tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-107">VNets contain hello following properties.</span></span>

| <span data-ttu-id="ec1e0-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ec1e0-108">Property</span></span> | <span data-ttu-id="ec1e0-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="ec1e0-109">Description</span></span> | <span data-ttu-id="ec1e0-110">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="ec1e0-110">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec1e0-111">**Címtartományt**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-111">**addressSpace**</span></span> |<span data-ttu-id="ec1e0-112">A CIDR jelölésrendszer VNet hello alkotó címelőtagokat gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="ec1e0-112">Collection of address prefixes that make up hello VNet in CIDR notation</span></span> |<span data-ttu-id="ec1e0-113">192.168.0.0/16</span><span class="sxs-lookup"><span data-stu-id="ec1e0-113">192.168.0.0/16</span></span> |
| <span data-ttu-id="ec1e0-114">**alhálózatok**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-114">**subnets**</span></span> |<span data-ttu-id="ec1e0-115">Hello virtuális hálózatot alkotó alhálózatok gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="ec1e0-115">Collection of subnets that make up hello VNet</span></span> |<span data-ttu-id="ec1e0-116">Lásd: [alhálózatok](#Subnets) alatt.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-116">see [subnets](#Subnets) below.</span></span> |
| <span data-ttu-id="ec1e0-117">**IP-cím**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-117">**ipAddress**</span></span> |<span data-ttu-id="ec1e0-118">IP-cím hozzárendelése tooobject.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-118">IP address assigned tooobject.</span></span> <span data-ttu-id="ec1e0-119">Ez a tulajdonság csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-119">This is a read-only property.</span></span> |<span data-ttu-id="ec1e0-120">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="ec1e0-120">104.42.233.77</span></span> |

### <a name="subnets"></a><span data-ttu-id="ec1e0-121">Alhálózatok</span><span class="sxs-lookup"><span data-stu-id="ec1e0-121">Subnets</span></span>
<span data-ttu-id="ec1e0-122">Egy alhálózat egy Vnetet gyermek erőforrása, és segítségével meghatározhatja a címterek belül használja az IP-cím előtagokat CIDR-blokkja részeit.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-122">A subnet is a child resource of a VNet, and helps define segments of address spaces within a CIDR block, using IP address prefixes.</span></span> <span data-ttu-id="ec1e0-123">Hálózati adapter toosubnets, és a csatlakoztatott tooVMs biztosítható a kapcsolat a különböző munkaterhelések lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-123">NICs can be added toosubnets, and connected tooVMs, providing connectivity for various workloads.</span></span>

<span data-ttu-id="ec1e0-124">Alhálózatok hello következő tulajdonságai tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-124">Subnets contain hello following properties.</span></span> 

| <span data-ttu-id="ec1e0-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ec1e0-125">Property</span></span> | <span data-ttu-id="ec1e0-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="ec1e0-126">Description</span></span> | <span data-ttu-id="ec1e0-127">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="ec1e0-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec1e0-128">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-128">**addressPrefix**</span></span> |<span data-ttu-id="ec1e0-129">Egyetlen címelőtag fel hello alhálózat CIDR-jelöléssel</span><span class="sxs-lookup"><span data-stu-id="ec1e0-129">Single address prefix that make up hello subnet in CIDR notation</span></span> |<span data-ttu-id="ec1e0-130">192.168.1.0/24</span><span class="sxs-lookup"><span data-stu-id="ec1e0-130">192.168.1.0/24</span></span> |
| <span data-ttu-id="ec1e0-131">**hálózati biztonsági csoporthoz tartozik**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-131">**networkSecurityGroup**</span></span> |<span data-ttu-id="ec1e0-132">Alkalmazott NSG toohello alhálózati</span><span class="sxs-lookup"><span data-stu-id="ec1e0-132">NSG applied toohello subnet</span></span> |<span data-ttu-id="ec1e0-133">Lásd: [NSG-k](#Network-Security-Group)</span><span class="sxs-lookup"><span data-stu-id="ec1e0-133">see [NSGs](#Network-Security-Group)</span></span> |
| <span data-ttu-id="ec1e0-134">**Migrálták**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-134">**routeTable**</span></span> |<span data-ttu-id="ec1e0-135">Az útvonaltábla alkalmazott toohello alhálózati</span><span class="sxs-lookup"><span data-stu-id="ec1e0-135">Route table applied toohello subnet</span></span> |<span data-ttu-id="ec1e0-136">Lásd: [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="ec1e0-136">see [UDR](#Route-table)</span></span> |
| <span data-ttu-id="ec1e0-137">**IP-konfigurációk**</span><span class="sxs-lookup"><span data-stu-id="ec1e0-137">**ipConfigurations**</span></span> |<span data-ttu-id="ec1e0-138">Hálózati adapter csatlakoztatott toohello alhálózati által használt IP-konfigurációs objektumok gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="ec1e0-138">Collection of IP configruation objects used by NICs connected toohello subnet</span></span> |<span data-ttu-id="ec1e0-139">Lásd: [UDR](#Route-table)</span><span class="sxs-lookup"><span data-stu-id="ec1e0-139">see [UDR](#Route-table)</span></span> |

<span data-ttu-id="ec1e0-140">A minta VNet JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="ec1e0-140">Sample VNet in JSON format:</span></span>

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a><span data-ttu-id="ec1e0-141">További források</span><span class="sxs-lookup"><span data-stu-id="ec1e0-141">Additional resources</span></span>
* <span data-ttu-id="ec1e0-142">További információk [VNet](../articles/virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ec1e0-142">Get more information about [VNet](../articles/virtual-network/virtual-networks-overview.md).</span></span>
* <span data-ttu-id="ec1e0-143">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163650.aspx) tartozó Vnetek esetében.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-143">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) for VNets.</span></span>
* <span data-ttu-id="ec1e0-144">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163618.aspx) alhálózatok esetében.</span><span class="sxs-lookup"><span data-stu-id="ec1e0-144">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) for Subnets.</span></span>

