## <a name="azure-dns"></a><span data-ttu-id="c55a9-101">Azure DNS</span><span class="sxs-lookup"><span data-stu-id="c55a9-101">Azure DNS</span></span>
<span data-ttu-id="c55a9-102">Az Azure DNS egy olyan üzemeltetési szolgáltatás DNS-tartományok, biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával.</span><span class="sxs-lookup"><span data-stu-id="c55a9-102">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span>

| <span data-ttu-id="c55a9-103">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c55a9-103">Property</span></span> | <span data-ttu-id="c55a9-104">Leírás</span><span class="sxs-lookup"><span data-stu-id="c55a9-104">Description</span></span> | <span data-ttu-id="c55a9-105">A minta értéke</span><span class="sxs-lookup"><span data-stu-id="c55a9-105">Sample Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c55a9-106">**DNSzones**</span><span class="sxs-lookup"><span data-stu-id="c55a9-106">**DNSzones**</span></span> |<span data-ttu-id="c55a9-107">Tartományi zóna adatainak állomás DNS-rekordok az adott tartományban</span><span class="sxs-lookup"><span data-stu-id="c55a9-107">Domain zone information to host DNS records of a particular domain</span></span> |<span data-ttu-id="c55a9-108">/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com "</span><span class="sxs-lookup"><span data-stu-id="c55a9-108">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com"</span></span> |

### <a name="dns-record-sets"></a><span data-ttu-id="c55a9-109">DNS-rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="c55a9-109">DNS record sets</span></span>
<span data-ttu-id="c55a9-110">DNS-zónák nevű rekordkészlet gyermekobjektum rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c55a9-110">DNS zones have a child object named record set.</span></span> <span data-ttu-id="c55a9-111">Rekordhalmazok kivételt jelentenek a DNS-zónák típusonként állomásrekordokat gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="c55a9-111">Record sets are a collection of host records by type for a DNS zone.</span></span> <span data-ttu-id="c55a9-112">Típusok A, AAAA, CNAME, MX, NS, SOA, SRV és TXT.</span><span class="sxs-lookup"><span data-stu-id="c55a9-112">Record types are A, AAAA, CNAME, MX, NS, SOA,SRV and TXT.</span></span>

| <span data-ttu-id="c55a9-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c55a9-113">Property</span></span> | <span data-ttu-id="c55a9-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="c55a9-114">Description</span></span> | <span data-ttu-id="c55a9-115">Mintaérték</span><span class="sxs-lookup"><span data-stu-id="c55a9-115">Sample value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c55a9-116">A</span><span class="sxs-lookup"><span data-stu-id="c55a9-116">A</span></span> |<span data-ttu-id="c55a9-117">IPv4-rekord típusa</span><span class="sxs-lookup"><span data-stu-id="c55a9-117">IPv4 record type</span></span> |<span data-ttu-id="c55a9-118">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/A/www</span><span class="sxs-lookup"><span data-stu-id="c55a9-118">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www</span></span> |
| <span data-ttu-id="c55a9-119">AAAA</span><span class="sxs-lookup"><span data-stu-id="c55a9-119">AAAA</span></span> |<span data-ttu-id="c55a9-120">IPv6-rekord típusa</span><span class="sxs-lookup"><span data-stu-id="c55a9-120">IPv6 record type</span></span> |<span data-ttu-id="c55a9-121">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span><span class="sxs-lookup"><span data-stu-id="c55a9-121">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span></span> |
| <span data-ttu-id="c55a9-122">CNAME</span><span class="sxs-lookup"><span data-stu-id="c55a9-122">CNAME</span></span> |<span data-ttu-id="c55a9-123">kanonikus név rekordtípus <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="c55a9-123">canonical name record type <sup>1</sup></span></span> |<span data-ttu-id="c55a9-124">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span><span class="sxs-lookup"><span data-stu-id="c55a9-124">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span></span> |
| <span data-ttu-id="c55a9-125">MX</span><span class="sxs-lookup"><span data-stu-id="c55a9-125">MX</span></span> |<span data-ttu-id="c55a9-126">mail bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="c55a9-126">mail record type</span></span> |<span data-ttu-id="c55a9-127">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span><span class="sxs-lookup"><span data-stu-id="c55a9-127">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span></span> |
| <span data-ttu-id="c55a9-128">NS</span><span class="sxs-lookup"><span data-stu-id="c55a9-128">NS</span></span> |<span data-ttu-id="c55a9-129">server type nevű</span><span class="sxs-lookup"><span data-stu-id="c55a9-129">name server record type</span></span> |<span data-ttu-id="c55a9-130">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/NS/</span><span class="sxs-lookup"><span data-stu-id="c55a9-130">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/</span></span> |
| <span data-ttu-id="c55a9-131">SOA</span><span class="sxs-lookup"><span data-stu-id="c55a9-131">SOA</span></span> |<span data-ttu-id="c55a9-132">Szolgáltató rekordtípus kezdetét <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="c55a9-132">Start of Authority record type <sup>2</sup></span></span> |<span data-ttu-id="c55a9-133">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SOA</span><span class="sxs-lookup"><span data-stu-id="c55a9-133">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA</span></span> |
| <span data-ttu-id="c55a9-134">SRV</span><span class="sxs-lookup"><span data-stu-id="c55a9-134">SRV</span></span> |<span data-ttu-id="c55a9-135">szolgáltatás bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="c55a9-135">service record type</span></span> |<span data-ttu-id="c55a9-136">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SRV</span><span class="sxs-lookup"><span data-stu-id="c55a9-136">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV</span></span> |

<span data-ttu-id="c55a9-137"><sup>1</sup> csak lehetővé teszi, hogy egy érték / rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="c55a9-137"><sup>1</sup> only allows one value per record set.</span></span>

<span data-ttu-id="c55a9-138"><sup>2</sup> csak lehetővé teszi, hogy egy rekordtípus SOA / DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="c55a9-138"><sup>2</sup> only allows one record type SOA per DNS zone.</span></span> 

<span data-ttu-id="c55a9-139">Minta DNS-zóna Json formátumban:</span><span class="sxs-lookup"><span data-stu-id="c55a9-139">Sample of DNS zone in Json format:</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "The name of the DNS zone to be created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "The name of the DNS record to be created.  The name is relative to the zone, not the FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a><span data-ttu-id="c55a9-140">További források</span><span class="sxs-lookup"><span data-stu-id="c55a9-140">Additional resources</span></span>
<span data-ttu-id="c55a9-141">Olvassa el a [DNS-zónák REST API dokumentációja ](https://msdn.microsoft.com/library/azure/mt130626.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="c55a9-141">Read the [REST API documentation for DNS zones ](https://msdn.microsoft.com/library/azure/mt130626.aspx) for more information.</span></span>

<span data-ttu-id="c55a9-142">Olvassa el a [DNS-rekordhalmazok REST API dokumentációja](https://msdn.microsoft.com/library/azure/mt130627.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="c55a9-142">Read the [REST API documentation for DNS record sets](https://msdn.microsoft.com/library/azure/mt130627.aspx) for more information.</span></span>

