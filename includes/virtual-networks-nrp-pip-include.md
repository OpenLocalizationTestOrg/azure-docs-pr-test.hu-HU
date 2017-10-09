## <a name="public-ip-address"></a><span data-ttu-id="969f0-101">Nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="969f0-101">Public IP address</span></span>
<span data-ttu-id="969f0-102">Egy nyilvános IP-cím erőforrás vagy egy fenntartott vagy dinamikus internetes IP-címet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="969f0-102">A public IP address resource provides either a reserved or dynamic Internet facing IP address.</span></span> <span data-ttu-id="969f0-103">Bár létrehozhat egy nyilvános IP-cím megegyezik egy önálló, tooassociate kell azt tooanother objektum tooactually hello címet használja.</span><span class="sxs-lookup"><span data-stu-id="969f0-103">Although you can create a public IP address as a stand alone object, you need tooassociate it tooanother object tooactually use hello address.</span></span> <span data-ttu-id="969f0-104">A nyilvános IP cím tooa terheléselosztó, Alkalmazásátjáró vagy egy hálózati adapter tooprovide Internet access toothose erőforrásokat lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="969f0-104">You can associate a public IP address tooa load balancer, application  gateway, or a NIC tooprovide Internet access toothose resources.</span></span>  

| <span data-ttu-id="969f0-105">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="969f0-105">Property</span></span> | <span data-ttu-id="969f0-106">Leírás</span><span class="sxs-lookup"><span data-stu-id="969f0-106">Description</span></span> | <span data-ttu-id="969f0-107">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="969f0-107">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="969f0-108">**publicIPAllocationMethod**</span><span class="sxs-lookup"><span data-stu-id="969f0-108">**publicIPAllocationMethod**</span></span> |<span data-ttu-id="969f0-109">Meghatározza, hogy az IP-cím hello *statikus* vagy *dinamikus*.</span><span class="sxs-lookup"><span data-stu-id="969f0-109">Defines if hello IP address is *static* or *dynamic*.</span></span> |<span data-ttu-id="969f0-110">statikus, dinamikus</span><span class="sxs-lookup"><span data-stu-id="969f0-110">static, dynamic</span></span> |
| <span data-ttu-id="969f0-111">**idleTimeoutInMinutes**</span><span class="sxs-lookup"><span data-stu-id="969f0-111">**idleTimeoutInMinutes**</span></span> |<span data-ttu-id="969f0-112">Meghatározza a hello üresjárati időkorlátot túllépő, 4 perces alapértelmezett értékkel.</span><span class="sxs-lookup"><span data-stu-id="969f0-112">Defines hello idle time out, with a default value of 4 minutes.</span></span> <span data-ttu-id="969f0-113">Ha egy adott munkamenethez nincs további csomagok ezen időn belül érkezik, hello munkamenet megszakítása.</span><span class="sxs-lookup"><span data-stu-id="969f0-113">If no more packets for a given session is received within this time, hello session is terminated.</span></span> |<span data-ttu-id="969f0-114">4 és 30 közötti értéket</span><span class="sxs-lookup"><span data-stu-id="969f0-114">any value between 4 and 30</span></span> |
| <span data-ttu-id="969f0-115">**IP-cím**</span><span class="sxs-lookup"><span data-stu-id="969f0-115">**ipAddress**</span></span> |<span data-ttu-id="969f0-116">IP-cím hozzárendelése tooobject.</span><span class="sxs-lookup"><span data-stu-id="969f0-116">IP address assigned tooobject.</span></span> <span data-ttu-id="969f0-117">Ez a tulajdonság csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="969f0-117">This is a read-only property.</span></span> |<span data-ttu-id="969f0-118">104.42.233.77</span><span class="sxs-lookup"><span data-stu-id="969f0-118">104.42.233.77</span></span> |

### <a name="dns-settings"></a><span data-ttu-id="969f0-119">DNS-beállítások</span><span class="sxs-lookup"><span data-stu-id="969f0-119">DNS settings</span></span>
<span data-ttu-id="969f0-120">Nyilvános IP-címek rendelkezik nevű gyermekobjektum **dnsSettings** tartalmazó hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="969f0-120">Public IP addresses have a child object named **dnsSettings** containing hello following properties:</span></span>

| <span data-ttu-id="969f0-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="969f0-121">Property</span></span> | <span data-ttu-id="969f0-122">Leírás</span><span class="sxs-lookup"><span data-stu-id="969f0-122">Description</span></span> | <span data-ttu-id="969f0-123">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="969f0-123">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="969f0-124">**domainNameLabel**</span><span class="sxs-lookup"><span data-stu-id="969f0-124">**domainNameLabel**</span></span> |<span data-ttu-id="969f0-125">Nevű a névfeloldáshoz használnak.</span><span class="sxs-lookup"><span data-stu-id="969f0-125">Host named used for name resolution.</span></span> |<span data-ttu-id="969f0-126">a webszolgáltatáshoz, ftp, vm1</span><span class="sxs-lookup"><span data-stu-id="969f0-126">www, ftp, vm1</span></span> |
| <span data-ttu-id="969f0-127">**teljesen minősített tartományneve**</span><span class="sxs-lookup"><span data-stu-id="969f0-127">**fqdn**</span></span> |<span data-ttu-id="969f0-128">Teljesen minősített neve hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="969f0-128">Fully qualified name for hello public IP.</span></span> |<span data-ttu-id="969f0-129">www.westus.cloudapp.Azure.com</span><span class="sxs-lookup"><span data-stu-id="969f0-129">www.westus.cloudapp.azure.com</span></span> |
| <span data-ttu-id="969f0-130">**reverseFqdn**</span><span class="sxs-lookup"><span data-stu-id="969f0-130">**reverseFqdn**</span></span> |<span data-ttu-id="969f0-131">Teljesen minősített tartománynevét, amely feloldja toohello IP-cím és DNS PTR-rekordot, regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="969f0-131">Fully qualified domain name that resolves toohello IP address and is registered in DNS as a PTR record.</span></span> |<span data-ttu-id="969f0-132">www.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="969f0-132">www.contoso.com.</span></span> |

<span data-ttu-id="969f0-133">A minta nyilvános IP-cím JSON formátumban:</span><span class="sxs-lookup"><span data-stu-id="969f0-133">Sample public IP address in JSON format:</span></span>

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a><span data-ttu-id="969f0-134">További források</span><span class="sxs-lookup"><span data-stu-id="969f0-134">Additional resources</span></span>
* <span data-ttu-id="969f0-135">További információk [nyilvános IP-címek](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="969f0-135">Get more information about [public IP addresses](../articles/virtual-network/virtual-networks-reserved-public-ip.md).</span></span>
* <span data-ttu-id="969f0-136">További tudnivalók [szintű nyilvános IP-címek példány](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="969f0-136">Learn about [instance level public IP addresses](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).</span></span>
* <span data-ttu-id="969f0-137">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163638.aspx) nyilvános IP-címek.</span><span class="sxs-lookup"><span data-stu-id="969f0-137">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163638.aspx) for public IP addresses.</span></span>

