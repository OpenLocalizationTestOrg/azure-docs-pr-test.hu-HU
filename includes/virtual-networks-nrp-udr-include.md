## <a name="route-tables"></a><span data-ttu-id="03141-101">Az útvonaltáblák</span><span class="sxs-lookup"><span data-stu-id="03141-101">Route tables</span></span>
<span data-ttu-id="03141-102">Útválasztási táblázat erőforrások hogyan a forgalom belül az Azure-infrastruktúra meghatározásához használt útvonalak tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="03141-102">Route table resources contains routes used to define how traffic flows within your Azure infrastructure.</span></span> <span data-ttu-id="03141-103">Felhasználó által megadott útvonalakat (UDR) segítségével az összes forgalom küldése az adott alhálózat virtuális készülékre, például egy tűzfal vagy behatolás észlelési rendszert (Azonosítók).</span><span class="sxs-lookup"><span data-stu-id="03141-103">You can use user defined routes (UDR) to send all traffic from a given subnet to a virtual appliance, such as a firewall or intrusion detection system (IDS).</span></span> <span data-ttu-id="03141-104">Az alhálózatokhoz egy útválasztási táblázatot lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="03141-104">You can associate a route table to subnets.</span></span> 

<span data-ttu-id="03141-105">Az útvonaltáblák az alábbi tulajdonságokat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="03141-105">Route tables contain the following properties.</span></span>

| <span data-ttu-id="03141-106">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="03141-106">Property</span></span> | <span data-ttu-id="03141-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="03141-107">Description</span></span> | <span data-ttu-id="03141-108">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="03141-108">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="03141-109">**útvonalak**</span><span class="sxs-lookup"><span data-stu-id="03141-109">**routes**</span></span> |<span data-ttu-id="03141-110">Felhasználó által megadott útvonalak útvonaltábla</span><span class="sxs-lookup"><span data-stu-id="03141-110">Collection of user defined routes in the route table</span></span> |<span data-ttu-id="03141-111">Lásd: [felhasználó által megadott útvonalak](#User-defined-routes)</span><span class="sxs-lookup"><span data-stu-id="03141-111">see [user defined routes](#User-defined-routes)</span></span> |
| <span data-ttu-id="03141-112">**alhálózatok**</span><span class="sxs-lookup"><span data-stu-id="03141-112">**subnets**</span></span> |<span data-ttu-id="03141-113">Az útvonaltábla vonatkozik, az alhálózatok gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="03141-113">Collection of subnets the route table is applied to</span></span> |<span data-ttu-id="03141-114">Lásd: [alhálózatok](#Subnets)</span><span class="sxs-lookup"><span data-stu-id="03141-114">see [subnets](#Subnets)</span></span> |

### <a name="user-defined-routes"></a><span data-ttu-id="03141-115">Felhasználó által definiált útvonalak</span><span class="sxs-lookup"><span data-stu-id="03141-115">User defined routes</span></span>
<span data-ttu-id="03141-116">Létrehozhat udr-EK adhatja meg, ha forgalmat kell küldeni, a cél címe alapján.</span><span class="sxs-lookup"><span data-stu-id="03141-116">You can create UDRs to specify where traffic should be sent to, based on its destination address.</span></span> <span data-ttu-id="03141-117">Az eltolásokat tekintheti útvonal az alapértelmezett átjáró definíciófrissítések a hálózati csomagok célcím alapján.</span><span class="sxs-lookup"><span data-stu-id="03141-117">You can think of a route as the default gateway definition based on the destination address of a network packet.</span></span>

<span data-ttu-id="03141-118">Udr-EK az alábbi tulajdonságokat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="03141-118">UDRs contain the following properties.</span></span> 

| <span data-ttu-id="03141-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="03141-119">Property</span></span> | <span data-ttu-id="03141-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="03141-120">Description</span></span> | <span data-ttu-id="03141-121">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="03141-121">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="03141-122">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="03141-122">**addressPrefix**</span></span> |<span data-ttu-id="03141-123">Cím előtagján vagy a teljes IP-címet a cél</span><span class="sxs-lookup"><span data-stu-id="03141-123">Address prefix, or full IP address for the destination</span></span> |<span data-ttu-id="03141-124">192.168.1.0/24, 192.168.1.101</span><span class="sxs-lookup"><span data-stu-id="03141-124">192.168.1.0/24, 192.168.1.101</span></span> |
| <span data-ttu-id="03141-125">**nexthoptype elem**</span><span class="sxs-lookup"><span data-stu-id="03141-125">**nextHopType**</span></span> |<span data-ttu-id="03141-126">A forgalom kapnak eszköz típusa</span><span class="sxs-lookup"><span data-stu-id="03141-126">Type of device the traffic will be sent to</span></span> |<span data-ttu-id="03141-127">VirtualAppliance, VPN-átjárót, az Internet</span><span class="sxs-lookup"><span data-stu-id="03141-127">VirtualAppliance, VPN Gateway, Internet</span></span> |
| <span data-ttu-id="03141-128">**nexthopipaddress eleme**</span><span class="sxs-lookup"><span data-stu-id="03141-128">**nextHopIpAddress**</span></span> |<span data-ttu-id="03141-129">A következő ugrás IP-címe</span><span class="sxs-lookup"><span data-stu-id="03141-129">IP address for the next hop</span></span> |<span data-ttu-id="03141-130">192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="03141-130">192.168.1.4</span></span> |

<span data-ttu-id="03141-131">A minta útvonaltábla JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="03141-131">Sample route table in JSON format:</span></span>

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a><span data-ttu-id="03141-132">További források</span><span class="sxs-lookup"><span data-stu-id="03141-132">Additional resources</span></span>
* <span data-ttu-id="03141-133">További információk [udr-EK](../articles/virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="03141-133">Get more information about [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).</span></span>
* <span data-ttu-id="03141-134">Olvassa el a [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt502549.aspx) az útválasztási táblázatokat.</span><span class="sxs-lookup"><span data-stu-id="03141-134">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) for route tables.</span></span>
* <span data-ttu-id="03141-135">Olvassa el a [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt502539.aspx) felhasználó által megadott útvonalak (udr-EK).</span><span class="sxs-lookup"><span data-stu-id="03141-135">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) for user defined routes (UDRs).</span></span>

