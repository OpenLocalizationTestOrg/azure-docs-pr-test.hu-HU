## <a name="traffic-manager-profile"></a><span data-ttu-id="05b58-101">Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="05b58-101">Traffic Manager Profile</span></span>
<span data-ttu-id="05b58-102">A TRAFFIC manager és a gyermek végpont útválasztási tooendpoints DNS az Azure-ban, és az Azure-on kívüli engedélyezése</span><span class="sxs-lookup"><span data-stu-id="05b58-102">Traffic manager and its child endpoint resource enable DNS routing tooendpoints in Azure and outside of Azure.</span></span> <span data-ttu-id="05b58-103">Ilyen forgalomeloszlás, útválasztási házirend metódusok szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="05b58-103">Such traffic distribution is governed by routing  policy methods.</span></span> <span data-ttu-id="05b58-104">A TRAFFIC manager is lehetővé teszi a végpont állapotfigyelő toobe figyeli, és megfelelő módon használják fel forgalmat a végpontok hello állapotának alapján.</span><span class="sxs-lookup"><span data-stu-id="05b58-104">Traffic manager also allows endpoint health toobe monitored, and traffic diverted appropriately based on hello health of an endpoint.</span></span> 

| <span data-ttu-id="05b58-105">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="05b58-105">Property</span></span> | <span data-ttu-id="05b58-106">Leírás</span><span class="sxs-lookup"><span data-stu-id="05b58-106">Description</span></span> |
| --- | --- |
| <span data-ttu-id="05b58-107">**trafficRoutingMethod**</span><span class="sxs-lookup"><span data-stu-id="05b58-107">**trafficRoutingMethod**</span></span> |<span data-ttu-id="05b58-108">a lehetséges értékek: *teljesítmény*, *Weighted*, és *prioritása*</span><span class="sxs-lookup"><span data-stu-id="05b58-108">possible values are *Performance*, *Weighted*, and *Priority*</span></span> |
| <span data-ttu-id="05b58-109">**dnsConfig**</span><span class="sxs-lookup"><span data-stu-id="05b58-109">**dnsConfig**</span></span> |<span data-ttu-id="05b58-110">Hello-profil teljes Tartományneve</span><span class="sxs-lookup"><span data-stu-id="05b58-110">FQDN for hello profile</span></span> |
| <span data-ttu-id="05b58-111">**Protocol (Protokoll)**</span><span class="sxs-lookup"><span data-stu-id="05b58-111">**Protocol**</span></span> |<span data-ttu-id="05b58-112">figyelés a protokollt, a lehetséges értékek: *HTTP* és *HTTPS*</span><span class="sxs-lookup"><span data-stu-id="05b58-112">monitoring protocol, possible values are *HTTP* and *HTTPS*</span></span> |
| <span data-ttu-id="05b58-113">**Port**</span><span class="sxs-lookup"><span data-stu-id="05b58-113">**Port**</span></span> |<span data-ttu-id="05b58-114">figyelési port</span><span class="sxs-lookup"><span data-stu-id="05b58-114">monitoring port</span></span> |
| <span data-ttu-id="05b58-115">**Elérési út**</span><span class="sxs-lookup"><span data-stu-id="05b58-115">**Path**</span></span> |<span data-ttu-id="05b58-116">figyelési elérési út</span><span class="sxs-lookup"><span data-stu-id="05b58-116">monitoring path</span></span> |
| <span data-ttu-id="05b58-117">**Végpontok**</span><span class="sxs-lookup"><span data-stu-id="05b58-117">**Endpoints**</span></span> |<span data-ttu-id="05b58-118">végpont erőforrások tárolója</span><span class="sxs-lookup"><span data-stu-id="05b58-118">container for endpoint resources</span></span> |

### <a name="endpoint"></a><span data-ttu-id="05b58-119">Végpont</span><span class="sxs-lookup"><span data-stu-id="05b58-119">Endpoint</span></span>
<span data-ttu-id="05b58-120">A végpont a Traffic Manager-profil gyermek erőforrása.</span><span class="sxs-lookup"><span data-stu-id="05b58-120">An endpoint is a child resource of a Traffic Manager Profile.</span></span> <span data-ttu-id="05b58-121">Azt jelenti, hogy egy szolgáltatás, vagy webes végpont toowhich felhasználói adatforgalom elosztása konfigurált hello házirend alapján virtualizál hello Traffic Manager-profil erőforrás.</span><span class="sxs-lookup"><span data-stu-id="05b58-121">It represents a service or web endpoint toowhich user traffic is distributed based on hello configured policy in hello Traffic Manager Profile resource.</span></span> 

| <span data-ttu-id="05b58-122">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="05b58-122">Property</span></span> | <span data-ttu-id="05b58-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="05b58-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="05b58-124">**Típus**</span><span class="sxs-lookup"><span data-stu-id="05b58-124">**Type**</span></span> |<span data-ttu-id="05b58-125">hello típus hello végpont, a lehetséges értékek: *Azure végpont*, *külső végpont*, és *beágyazott végpont*</span><span class="sxs-lookup"><span data-stu-id="05b58-125">hello type of hello endpoint, possible values are *Azure End point*, *External Endpoint*, and  *Nested Endpoint*</span></span> |
| <span data-ttu-id="05b58-126">**targetresourceid azonosítója**</span><span class="sxs-lookup"><span data-stu-id="05b58-126">**targetResourceId**</span></span> |<span data-ttu-id="05b58-127">egy szolgáltatás vagy webes végpont nyilvános IP-címe.</span><span class="sxs-lookup"><span data-stu-id="05b58-127">public IP address of a service or web endpoint.</span></span> <span data-ttu-id="05b58-128">Ez lehet egy Azure-bA vagy külső végpont.</span><span class="sxs-lookup"><span data-stu-id="05b58-128">This can be an Azure or external endpoint.</span></span> |
| <span data-ttu-id="05b58-129">**Súlyozás**</span><span class="sxs-lookup"><span data-stu-id="05b58-129">**Weight**</span></span> |<span data-ttu-id="05b58-130">végpont súly használt forgalom kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="05b58-130">endpoint weight used in traffic management.</span></span> |
| <span data-ttu-id="05b58-131">**Priority (Prioritás)**</span><span class="sxs-lookup"><span data-stu-id="05b58-131">**Priority**</span></span> |<span data-ttu-id="05b58-132">hello a végponthoz, a feladatátvételi művelet használt toodefine prioritása</span><span class="sxs-lookup"><span data-stu-id="05b58-132">priority of hello endpoint, used toodefine a failover action</span></span> |

<span data-ttu-id="05b58-133">A Traffic Manager minta Json formátumban:</span><span class="sxs-lookup"><span data-stu-id="05b58-133">Sample of Traffic Manager in Json format:</span></span> 

        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }


## <a name="additional-resources"></a><span data-ttu-id="05b58-134">További források</span><span class="sxs-lookup"><span data-stu-id="05b58-134">Additional resources</span></span>
<span data-ttu-id="05b58-135">Olvasási [REST API-dokumentáció a Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="05b58-135">Read [REST API documentation for Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) for more information.</span></span>

