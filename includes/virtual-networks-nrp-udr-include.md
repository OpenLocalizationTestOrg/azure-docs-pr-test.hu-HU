## <a name="route-tables"></a><span data-ttu-id="27966-101">Az útvonaltáblák</span><span class="sxs-lookup"><span data-stu-id="27966-101">Route tables</span></span>
<span data-ttu-id="27966-102">Útválasztási táblázat erőforrások hogyan a forgalom az Azure-infrastruktúra belül használt útvonalak toodefine tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="27966-102">Route table resources contains routes used toodefine how traffic flows within your Azure infrastructure.</span></span> <span data-ttu-id="27966-103">Felhasználó által definiált útvonalak (UDR) toosend használhatja egy adott alhálózaton tooa virtuális készüléknek, például egy tűzfal vagy behatolás észlelési rendszert (Azonosítók) származó összes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="27966-103">You can use user defined routes (UDR) toosend all traffic from a given subnet tooa virtual appliance, such as a firewall or intrusion detection system (IDS).</span></span> <span data-ttu-id="27966-104">Egy útvonal tábla toosubnets lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="27966-104">You can associate a route table toosubnets.</span></span> 

<span data-ttu-id="27966-105">Az útvonaltáblák hello következő tulajdonságai tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="27966-105">Route tables contain hello following properties.</span></span>

| <span data-ttu-id="27966-106">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="27966-106">Property</span></span> | <span data-ttu-id="27966-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="27966-107">Description</span></span> | <span data-ttu-id="27966-108">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="27966-108">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="27966-109">**útvonalak**</span><span class="sxs-lookup"><span data-stu-id="27966-109">**routes**</span></span> |<span data-ttu-id="27966-110">Felhasználó által megadott útvonalak hello útvonaltábla</span><span class="sxs-lookup"><span data-stu-id="27966-110">Collection of user defined routes in hello route table</span></span> |<span data-ttu-id="27966-111">Lásd: [felhasználó által megadott útvonalak](#User-defined-routes)</span><span class="sxs-lookup"><span data-stu-id="27966-111">see [user defined routes](#User-defined-routes)</span></span> |
| <span data-ttu-id="27966-112">**alhálózatok**</span><span class="sxs-lookup"><span data-stu-id="27966-112">**subnets**</span></span> |<span data-ttu-id="27966-113">Alhálózatok hello útvonaltábla gyűjteménye túl vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="27966-113">Collection of subnets hello route table is applied too</span></span>|<span data-ttu-id="27966-114">Lásd: [alhálózatok](#Subnets)</span><span class="sxs-lookup"><span data-stu-id="27966-114">see [subnets](#Subnets)</span></span> |

### <a name="user-defined-routes"></a><span data-ttu-id="27966-115">Felhasználó által definiált útvonalak</span><span class="sxs-lookup"><span data-stu-id="27966-115">User defined routes</span></span>
<span data-ttu-id="27966-116">Létrehozhat udr-EK toospecify, ahol forgalmat kell küldeni, a cél címe alapján.</span><span class="sxs-lookup"><span data-stu-id="27966-116">You can create UDRs toospecify where traffic should be sent to, based on its destination address.</span></span> <span data-ttu-id="27966-117">Az eltolásokat tekintheti útvonal hello alapértelmezett átjáró definíciófrissítések a hálózati csomagok hello célcím alapján.</span><span class="sxs-lookup"><span data-stu-id="27966-117">You can think of a route as hello default gateway definition based on hello destination address of a network packet.</span></span>

<span data-ttu-id="27966-118">Udr-EK hello következő tulajdonságai tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="27966-118">UDRs contain hello following properties.</span></span> 

| <span data-ttu-id="27966-119">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="27966-119">Property</span></span> | <span data-ttu-id="27966-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="27966-120">Description</span></span> | <span data-ttu-id="27966-121">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="27966-121">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="27966-122">**addressPrefix**</span><span class="sxs-lookup"><span data-stu-id="27966-122">**addressPrefix**</span></span> |<span data-ttu-id="27966-123">Cím előtagján vagy a teljes IP-cím hello cél</span><span class="sxs-lookup"><span data-stu-id="27966-123">Address prefix, or full IP address for hello destination</span></span> |<span data-ttu-id="27966-124">192.168.1.0/24, 192.168.1.101</span><span class="sxs-lookup"><span data-stu-id="27966-124">192.168.1.0/24, 192.168.1.101</span></span> |
| <span data-ttu-id="27966-125">**nexthoptype elem**</span><span class="sxs-lookup"><span data-stu-id="27966-125">**nextHopType**</span></span> |<span data-ttu-id="27966-126">Eszköz hello forgalomtípushoz túl küld</span><span class="sxs-lookup"><span data-stu-id="27966-126">Type of device hello traffic will be sent too</span></span>|<span data-ttu-id="27966-127">VirtualAppliance, VPN-átjárót, az Internet</span><span class="sxs-lookup"><span data-stu-id="27966-127">VirtualAppliance, VPN Gateway, Internet</span></span> |
| <span data-ttu-id="27966-128">**nexthopipaddress eleme**</span><span class="sxs-lookup"><span data-stu-id="27966-128">**nextHopIpAddress**</span></span> |<span data-ttu-id="27966-129">Hello következő ugrás IP-címe</span><span class="sxs-lookup"><span data-stu-id="27966-129">IP address for hello next hop</span></span> |<span data-ttu-id="27966-130">192.168.1.4</span><span class="sxs-lookup"><span data-stu-id="27966-130">192.168.1.4</span></span> |

<span data-ttu-id="27966-131">A minta útvonaltábla JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="27966-131">Sample route table in JSON format:</span></span>

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

### <a name="additional-resources"></a><span data-ttu-id="27966-132">További források</span><span class="sxs-lookup"><span data-stu-id="27966-132">Additional resources</span></span>
* <span data-ttu-id="27966-133">További információk [udr-EK](../articles/virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27966-133">Get more information about [UDRs](../articles/virtual-network/virtual-networks-udr-overview.md).</span></span>
* <span data-ttu-id="27966-134">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt502549.aspx) az útválasztási táblázatokat.</span><span class="sxs-lookup"><span data-stu-id="27966-134">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502549.aspx) for route tables.</span></span>
* <span data-ttu-id="27966-135">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt502539.aspx) felhasználó által megadott útvonalak (udr-EK).</span><span class="sxs-lookup"><span data-stu-id="27966-135">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt502539.aspx) for user defined routes (UDRs).</span></span>

