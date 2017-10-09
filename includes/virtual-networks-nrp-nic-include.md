## <a name="nic"></a><span data-ttu-id="a87c2-101">A HÁLÓZATI ADAPTER</span><span class="sxs-lookup"><span data-stu-id="a87c2-101">NIC</span></span>
<span data-ttu-id="a87c2-102">Kártya (NIC) erőforrás hálózati illesztő hálózati kapcsolat tooan meglévő alhálózat virtuális hálózat az erőforrás biztosít.</span><span class="sxs-lookup"><span data-stu-id="a87c2-102">A network interface card (NIC) resource provides network connectivity tooan existing subnet in a VNet resource.</span></span> <span data-ttu-id="a87c2-103">Bár létrehozhat egy hálózati adapter nem önálló objektumként, tooassociate kell azt tooanother objektum tooactually biztosít kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a87c2-103">Although you can create a NIC as a stand alone object, you need tooassociate it tooanother object tooactually provide connectivity.</span></span> <span data-ttu-id="a87c2-104">A hálózati adapter által használt tooconnect tooa alhálózatot, egy nyilvános IP-címet vagy egy terheléselosztó lehet.</span><span class="sxs-lookup"><span data-stu-id="a87c2-104">A NIC can be used tooconnect a VM tooa subnet, a public IP address, or a load balancer.</span></span>  

| <span data-ttu-id="a87c2-105">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a87c2-105">Property</span></span> | <span data-ttu-id="a87c2-106">Leírás</span><span class="sxs-lookup"><span data-stu-id="a87c2-106">Description</span></span> | <span data-ttu-id="a87c2-107">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="a87c2-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a87c2-108">**virtualMachine**</span><span class="sxs-lookup"><span data-stu-id="a87c2-108">**virtualMachine**</span></span> |<span data-ttu-id="a87c2-109">Virtuális gép hello hálózati adapter társítva van.</span><span class="sxs-lookup"><span data-stu-id="a87c2-109">VM hello NIC is associated with.</span></span> |<span data-ttu-id="a87c2-110">/Subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1</span><span class="sxs-lookup"><span data-stu-id="a87c2-110">/subscriptions/{guid}/../Microsoft.Compute/virtualMachines/vm1</span></span> |
| <span data-ttu-id="a87c2-111">**macAddress**</span><span class="sxs-lookup"><span data-stu-id="a87c2-111">**macAddress**</span></span> |<span data-ttu-id="a87c2-112">Hello hálózati adapter MAC-címe</span><span class="sxs-lookup"><span data-stu-id="a87c2-112">MAC address for hello NIC</span></span> |<span data-ttu-id="a87c2-113">4 és 30 közötti értéket</span><span class="sxs-lookup"><span data-stu-id="a87c2-113">any value between 4 and 30</span></span> |
| <span data-ttu-id="a87c2-114">**hálózati biztonsági csoporthoz tartozik**</span><span class="sxs-lookup"><span data-stu-id="a87c2-114">**networkSecurityGroup**</span></span> |<span data-ttu-id="a87c2-115">Társított NSG-t toohello hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="a87c2-115">NSG associated toohello NIC</span></span> |<span data-ttu-id="a87c2-116">/Subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1</span><span class="sxs-lookup"><span data-stu-id="a87c2-116">/subscriptions/{guid}/../Microsoft.Network/networkSecurityGroups/myNSG1</span></span> |
| <span data-ttu-id="a87c2-117">**dnsSettings**</span><span class="sxs-lookup"><span data-stu-id="a87c2-117">**dnsSettings**</span></span> |<span data-ttu-id="a87c2-118">Hello hálózati adapter DNS-beállításait</span><span class="sxs-lookup"><span data-stu-id="a87c2-118">DNS settings for hello NIC</span></span> |<span data-ttu-id="a87c2-119">Lásd: [PIP](#Public-IP-address)</span><span class="sxs-lookup"><span data-stu-id="a87c2-119">see [PIP](#Public-IP-address)</span></span> |

<span data-ttu-id="a87c2-120">A hálózati kártyát, vagy a hálózati adapter, jelöli meg a hálózati adaptert, amely társított tooa virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="a87c2-120">A Network Interface Card, or NIC, represents a network interface that can be associated tooa virtual machine (VM).</span></span> <span data-ttu-id="a87c2-121">A virtuális gép egy vagy több hálózati adapterrel rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="a87c2-121">A VM can have one or more NICs.</span></span>

![A hálózati adapter által a egyetlen virtuális gép](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a><span data-ttu-id="a87c2-123">IP-konfigurációk</span><span class="sxs-lookup"><span data-stu-id="a87c2-123">IP configurations</span></span>
<span data-ttu-id="a87c2-124">Hálózati adapterei nevű gyermekobjektum **IP-konfigurációk** tartalmazó hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="a87c2-124">NICs have a child object named **ipConfigurations** containing hello following properties:</span></span>

| <span data-ttu-id="a87c2-125">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a87c2-125">Property</span></span> | <span data-ttu-id="a87c2-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="a87c2-126">Description</span></span> | <span data-ttu-id="a87c2-127">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="a87c2-127">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a87c2-128">**alhálózat**</span><span class="sxs-lookup"><span data-stu-id="a87c2-128">**subnet**</span></span> |<span data-ttu-id="a87c2-129">Alhálózati hello NIC való onnected.</span><span class="sxs-lookup"><span data-stu-id="a87c2-129">Subnet hello NIC is onnected to.</span></span> |<span data-ttu-id="a87c2-130">/Subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/Subnets/mysub1</span><span class="sxs-lookup"><span data-stu-id="a87c2-130">/subscriptions/{guid}/../Microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1</span></span> |
| <span data-ttu-id="a87c2-131">**privateipaddress tulajdonságot**</span><span class="sxs-lookup"><span data-stu-id="a87c2-131">**privateIPAddress**</span></span> |<span data-ttu-id="a87c2-132">Hello NIC hello alhálózat IP-címe</span><span class="sxs-lookup"><span data-stu-id="a87c2-132">IP address for hello NIC in hello subnet</span></span> |<span data-ttu-id="a87c2-133">10.0.0.8</span><span class="sxs-lookup"><span data-stu-id="a87c2-133">10.0.0.8</span></span> |
| <span data-ttu-id="a87c2-134">**címkiosztási**</span><span class="sxs-lookup"><span data-stu-id="a87c2-134">**privateIPAllocationMethod**</span></span> |<span data-ttu-id="a87c2-135">IP-címkiosztási módszerét</span><span class="sxs-lookup"><span data-stu-id="a87c2-135">IP allocation method</span></span> |<span data-ttu-id="a87c2-136">Dinamikus vagy statikus</span><span class="sxs-lookup"><span data-stu-id="a87c2-136">Dynamic or Static</span></span> |
| <span data-ttu-id="a87c2-137">**enableIPForwarding**</span><span class="sxs-lookup"><span data-stu-id="a87c2-137">**enableIPForwarding**</span></span> |<span data-ttu-id="a87c2-138">E hello hálózati adapter is használható Útválasztás</span><span class="sxs-lookup"><span data-stu-id="a87c2-138">Whether hello NIC can be used for routing</span></span> |<span data-ttu-id="a87c2-139">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="a87c2-139">true or false</span></span> |
| <span data-ttu-id="a87c2-140">**elsődleges**</span><span class="sxs-lookup"><span data-stu-id="a87c2-140">**primary**</span></span> |<span data-ttu-id="a87c2-141">-E a hálózati adapter hello hello elsődleges hálózati hello méretű VM</span><span class="sxs-lookup"><span data-stu-id="a87c2-141">Whether hello NIC is hello primary NIC for hello VM</span></span> |<span data-ttu-id="a87c2-142">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="a87c2-142">true or false</span></span> |
| <span data-ttu-id="a87c2-143">**Nyilvános**</span><span class="sxs-lookup"><span data-stu-id="a87c2-143">**publicIPAddress**</span></span> |<span data-ttu-id="a87c2-144">A PIP társított hálózati adapter hello</span><span class="sxs-lookup"><span data-stu-id="a87c2-144">PIP associated with hello NIC</span></span> |<span data-ttu-id="a87c2-145">Lásd: [DNS-beállítások](#DNS-settings)</span><span class="sxs-lookup"><span data-stu-id="a87c2-145">see [DNS Settings](#DNS-settings)</span></span> |
| <span data-ttu-id="a87c2-146">**konfigurációja terheléselosztói Háttércímkészletet**</span><span class="sxs-lookup"><span data-stu-id="a87c2-146">**loadBalancerBackendAddressPools**</span></span> |<span data-ttu-id="a87c2-147">Biztonsági másolatot a záró cím készletek hello hálózati adapter társítva</span><span class="sxs-lookup"><span data-stu-id="a87c2-147">Back end address pools hello NIC is associated with</span></span> | |
| <span data-ttu-id="a87c2-148">**loadBalancerInboundNatRules**</span><span class="sxs-lookup"><span data-stu-id="a87c2-148">**loadBalancerInboundNatRules**</span></span> |<span data-ttu-id="a87c2-149">A bejövő terhelés terheléselosztó NAT szabályok hello hálózati adapter társítva</span><span class="sxs-lookup"><span data-stu-id="a87c2-149">Inbound load balancer NAT rules hello NIC is associated with</span></span> | |

<span data-ttu-id="a87c2-150">A minta nyilvános IP-cím JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="a87c2-150">Sample public IP address in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="a87c2-151">További források</span><span class="sxs-lookup"><span data-stu-id="a87c2-151">Additional resources</span></span>
* <span data-ttu-id="a87c2-152">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163579.aspx) a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="a87c2-152">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163579.aspx) for NICs.</span></span>

