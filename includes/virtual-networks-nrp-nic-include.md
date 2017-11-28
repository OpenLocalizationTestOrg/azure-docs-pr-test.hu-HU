## <a name="nic"></a><span data-ttu-id="2109b-101">A HÁLÓZATI ADAPTER</span><span class="sxs-lookup"><span data-stu-id="2109b-101">NIC</span></span>
<span data-ttu-id="2109b-102">Kártya (NIC) erőforrás hálózati illesztő hálózati kapcsolatot biztosít annak VNet az erőforrás egy létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2109b-102">A network interface card (NIC) resource provides network connectivity to an existing subnet in a VNet resource.</span></span> <span data-ttu-id="2109b-103">Bár létrehozhat egy hálózati adapter nem önálló objektumként, rendelje hozzá azt egy másik objektum ténylegesen összekapcsolhatóság kell.</span><span class="sxs-lookup"><span data-stu-id="2109b-103">Although you can create a NIC as a stand alone object, you need to associate it to another object to actually provide connectivity.</span></span> <span data-ttu-id="2109b-104">Egy hálózati Adaptert egy virtuális gép csatlakozni egy alhálózatot, egy nyilvános IP-címet vagy egy terheléselosztó használható.</span><span class="sxs-lookup"><span data-stu-id="2109b-104">A NIC can be used to connect a VM to a subnet, a public IP address, or a load balancer.</span></span>  

| <span data-ttu-id="2109b-105">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2109b-105">Property</span></span> | <span data-ttu-id="2109b-106">Leírás</span><span class="sxs-lookup"><span data-stu-id="2109b-106">Description</span></span> | <span data-ttu-id="2109b-107">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="2109b-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2109b-108">**virtualMachine**</span><span class="sxs-lookup"><span data-stu-id="2109b-108">**virtualMachine**</span></span> |<span data-ttu-id="2109b-109">Virtuális gép hálózati adapter társítva van.</span><span class="sxs-lookup"><span data-stu-id="2109b-109">VM the NIC is associated with.</span></span> |<span data-ttu-id="2109b-110">/Subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1</span><span class="sxs-lookup"><span data-stu-id="2109b-110">/subscriptions/{guid}/../Microsoft.Compute/virtualMachines/vm1</span></span> |
| <span data-ttu-id="2109b-111">**macAddress**</span><span class="sxs-lookup"><span data-stu-id="2109b-111">**macAddress**</span></span> |<span data-ttu-id="2109b-112">A hálózati adapter MAC-címe</span><span class="sxs-lookup"><span data-stu-id="2109b-112">MAC address for the NIC</span></span> |<span data-ttu-id="2109b-113">4 és 30 közötti értéket</span><span class="sxs-lookup"><span data-stu-id="2109b-113">any value between 4 and 30</span></span> |
| <span data-ttu-id="2109b-114">**hálózati biztonsági csoporthoz tartozik**</span><span class="sxs-lookup"><span data-stu-id="2109b-114">**networkSecurityGroup**</span></span> |<span data-ttu-id="2109b-115">A hálózati adapterhez társított NSG-t</span><span class="sxs-lookup"><span data-stu-id="2109b-115">NSG associated to the NIC</span></span> |<span data-ttu-id="2109b-116">/Subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1</span><span class="sxs-lookup"><span data-stu-id="2109b-116">/subscriptions/{guid}/../Microsoft.Network/networkSecurityGroups/myNSG1</span></span> |
| <span data-ttu-id="2109b-117">**dnsSettings**</span><span class="sxs-lookup"><span data-stu-id="2109b-117">**dnsSettings**</span></span> |<span data-ttu-id="2109b-118">A hálózati adapter DNS-beállítások</span><span class="sxs-lookup"><span data-stu-id="2109b-118">DNS settings for the NIC</span></span> |<span data-ttu-id="2109b-119">Lásd: [PIP](#Public-IP-address)</span><span class="sxs-lookup"><span data-stu-id="2109b-119">see [PIP](#Public-IP-address)</span></span> |

<span data-ttu-id="2109b-120">A hálózati kártyát, vagy a hálózati adapter, jelöli meg a hálózati adaptert egy virtuális gép (VM) társítható.</span><span class="sxs-lookup"><span data-stu-id="2109b-120">A Network Interface Card, or NIC, represents a network interface that can be associated to a virtual machine (VM).</span></span> <span data-ttu-id="2109b-121">A virtuális gép egy vagy több hálózati adapterrel rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="2109b-121">A VM can have one or more NICs.</span></span>

![A hálózati adapter által a egyetlen virtuális gép](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a><span data-ttu-id="2109b-123">IP-konfigurációk</span><span class="sxs-lookup"><span data-stu-id="2109b-123">IP configurations</span></span>
<span data-ttu-id="2109b-124">Hálózati adapterei nevű gyermekobjektum **IP-konfigurációk** tartalmaz a következő tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="2109b-124">NICs have a child object named **ipConfigurations** containing the following properties:</span></span>

| <span data-ttu-id="2109b-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2109b-125">Property</span></span> | <span data-ttu-id="2109b-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="2109b-126">Description</span></span> | <span data-ttu-id="2109b-127">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="2109b-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2109b-128">**alhálózat**</span><span class="sxs-lookup"><span data-stu-id="2109b-128">**subnet**</span></span> |<span data-ttu-id="2109b-129">Alhálózatot a hálózati adapter az onnected.</span><span class="sxs-lookup"><span data-stu-id="2109b-129">Subnet the NIC is onnected to.</span></span> |<span data-ttu-id="2109b-130">/Subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1</span><span class="sxs-lookup"><span data-stu-id="2109b-130">/subscriptions/{guid}/../Microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1</span></span> |
| <span data-ttu-id="2109b-131">**privateipaddress tulajdonságot**</span><span class="sxs-lookup"><span data-stu-id="2109b-131">**privateIPAddress**</span></span> |<span data-ttu-id="2109b-132">A hálózati kártyának az alhálózat IP-cím</span><span class="sxs-lookup"><span data-stu-id="2109b-132">IP address for the NIC in the subnet</span></span> |<span data-ttu-id="2109b-133">10.0.0.8</span><span class="sxs-lookup"><span data-stu-id="2109b-133">10.0.0.8</span></span> |
| <span data-ttu-id="2109b-134">**címkiosztási**</span><span class="sxs-lookup"><span data-stu-id="2109b-134">**privateIPAllocationMethod**</span></span> |<span data-ttu-id="2109b-135">IP-címkiosztási módszerét</span><span class="sxs-lookup"><span data-stu-id="2109b-135">IP allocation method</span></span> |<span data-ttu-id="2109b-136">Dinamikus vagy statikus</span><span class="sxs-lookup"><span data-stu-id="2109b-136">Dynamic or Static</span></span> |
| <span data-ttu-id="2109b-137">**enableIPForwarding**</span><span class="sxs-lookup"><span data-stu-id="2109b-137">**enableIPForwarding**</span></span> |<span data-ttu-id="2109b-138">Hogy a hálózati adapter is használható Útválasztás</span><span class="sxs-lookup"><span data-stu-id="2109b-138">Whether the NIC can be used for routing</span></span> |<span data-ttu-id="2109b-139">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="2109b-139">true or false</span></span> |
| <span data-ttu-id="2109b-140">**elsődleges**</span><span class="sxs-lookup"><span data-stu-id="2109b-140">**primary**</span></span> |<span data-ttu-id="2109b-141">A hálózati adapter-e a virtuális gép az elsődleges hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="2109b-141">Whether the NIC is the primary NIC for the VM</span></span> |<span data-ttu-id="2109b-142">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="2109b-142">true or false</span></span> |
| <span data-ttu-id="2109b-143">**Nyilvános**</span><span class="sxs-lookup"><span data-stu-id="2109b-143">**publicIPAddress**</span></span> |<span data-ttu-id="2109b-144">A PIP társított hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="2109b-144">PIP associated with the NIC</span></span> |<span data-ttu-id="2109b-145">Lásd: [DNS-beállítások](#DNS-settings)</span><span class="sxs-lookup"><span data-stu-id="2109b-145">see [DNS Settings](#DNS-settings)</span></span> |
| <span data-ttu-id="2109b-146">**konfigurációja terheléselosztói Háttércímkészletet**</span><span class="sxs-lookup"><span data-stu-id="2109b-146">**loadBalancerBackendAddressPools**</span></span> |<span data-ttu-id="2109b-147">Biztonsági end címkészlet társítva van a hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="2109b-147">Back end address pools the NIC is associated with</span></span> | |
| <span data-ttu-id="2109b-148">**loadBalancerInboundNatRules**</span><span class="sxs-lookup"><span data-stu-id="2109b-148">**loadBalancerInboundNatRules**</span></span> |<span data-ttu-id="2109b-149">A bejövő terhelés terheléselosztó NAT-szabályok a hálózati adapter társítva</span><span class="sxs-lookup"><span data-stu-id="2109b-149">Inbound load balancer NAT rules the NIC is associated with</span></span> | |

<span data-ttu-id="2109b-150">A minta nyilvános IP-cím JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="2109b-150">Sample public IP address in JSON format:</span></span>

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a><span data-ttu-id="2109b-151">További források</span><span class="sxs-lookup"><span data-stu-id="2109b-151">Additional resources</span></span>
* <span data-ttu-id="2109b-152">Olvassa el a [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163579.aspx) a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="2109b-152">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163579.aspx) for NICs.</span></span>

