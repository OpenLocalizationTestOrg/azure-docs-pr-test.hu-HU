## <a name="azure-dns"></a><span data-ttu-id="851a4-101">Azure DNS</span><span class="sxs-lookup"><span data-stu-id="851a4-101">Azure DNS</span></span>
<span data-ttu-id="851a4-102">Az Azure DNS egy olyan üzemeltetési szolgáltatás DNS-tartományok, biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával.</span><span class="sxs-lookup"><span data-stu-id="851a4-102">Azure DNS is a hosting service for DNS domains, providing name resolution using Microsoft Azure infrastructure.</span></span>

| <span data-ttu-id="851a4-103">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="851a4-103">Property</span></span> | <span data-ttu-id="851a4-104">Leírás</span><span class="sxs-lookup"><span data-stu-id="851a4-104">Description</span></span> | <span data-ttu-id="851a4-105">A minta értéke</span><span class="sxs-lookup"><span data-stu-id="851a4-105">Sample Value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="851a4-106">**DNSzones**</span><span class="sxs-lookup"><span data-stu-id="851a4-106">**DNSzones**</span></span> |<span data-ttu-id="851a4-107">Tartomány zóna információk toohost DNS-rekordjait az adott tartományban</span><span class="sxs-lookup"><span data-stu-id="851a4-107">Domain zone information toohost DNS records of a particular domain</span></span> |<span data-ttu-id="851a4-108">/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com "</span><span class="sxs-lookup"><span data-stu-id="851a4-108">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com"</span></span> |

### <a name="dns-record-sets"></a><span data-ttu-id="851a4-109">DNS-rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="851a4-109">DNS record sets</span></span>
<span data-ttu-id="851a4-110">DNS-zónák nevű rekordkészlet gyermekobjektum rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="851a4-110">DNS zones have a child object named record set.</span></span> <span data-ttu-id="851a4-111">Rekordhalmazok kivételt jelentenek a DNS-zónák típusonként állomásrekordokat gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="851a4-111">Record sets are a collection of host records by type for a DNS zone.</span></span> <span data-ttu-id="851a4-112">Típusok A, AAAA, CNAME, MX, NS, SOA, SRV és TXT.</span><span class="sxs-lookup"><span data-stu-id="851a4-112">Record types are A, AAAA, CNAME, MX, NS, SOA,SRV and TXT.</span></span>

| <span data-ttu-id="851a4-113">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="851a4-113">Property</span></span> | <span data-ttu-id="851a4-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="851a4-114">Description</span></span> | <span data-ttu-id="851a4-115">Mintaérték</span><span class="sxs-lookup"><span data-stu-id="851a4-115">Sample value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="851a4-116">A</span><span class="sxs-lookup"><span data-stu-id="851a4-116">A</span></span> |<span data-ttu-id="851a4-117">IPv4-rekord típusa</span><span class="sxs-lookup"><span data-stu-id="851a4-117">IPv4 record type</span></span> |<span data-ttu-id="851a4-118">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/A/www</span><span class="sxs-lookup"><span data-stu-id="851a4-118">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www</span></span> |
| <span data-ttu-id="851a4-119">AAAA</span><span class="sxs-lookup"><span data-stu-id="851a4-119">AAAA</span></span> |<span data-ttu-id="851a4-120">IPv6-rekord típusa</span><span class="sxs-lookup"><span data-stu-id="851a4-120">IPv6 record type</span></span> |<span data-ttu-id="851a4-121">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span><span class="sxs-lookup"><span data-stu-id="851a4-121">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord</span></span> |
| <span data-ttu-id="851a4-122">CNAME</span><span class="sxs-lookup"><span data-stu-id="851a4-122">CNAME</span></span> |<span data-ttu-id="851a4-123">kanonikus név rekordtípus <sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="851a4-123">canonical name record type <sup>1</sup></span></span> |<span data-ttu-id="851a4-124">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span><span class="sxs-lookup"><span data-stu-id="851a4-124">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www</span></span> |
| <span data-ttu-id="851a4-125">MX</span><span class="sxs-lookup"><span data-stu-id="851a4-125">MX</span></span> |<span data-ttu-id="851a4-126">mail bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="851a4-126">mail record type</span></span> |<span data-ttu-id="851a4-127">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span><span class="sxs-lookup"><span data-stu-id="851a4-127">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/mail</span></span> |
| <span data-ttu-id="851a4-128">NS</span><span class="sxs-lookup"><span data-stu-id="851a4-128">NS</span></span> |<span data-ttu-id="851a4-129">server type nevű</span><span class="sxs-lookup"><span data-stu-id="851a4-129">name server record type</span></span> |<span data-ttu-id="851a4-130">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/NS/</span><span class="sxs-lookup"><span data-stu-id="851a4-130">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/</span></span> |
| <span data-ttu-id="851a4-131">SOA</span><span class="sxs-lookup"><span data-stu-id="851a4-131">SOA</span></span> |<span data-ttu-id="851a4-132">Szolgáltató rekordtípus kezdetét <sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="851a4-132">Start of Authority record type <sup>2</sup></span></span> |<span data-ttu-id="851a4-133">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SOA</span><span class="sxs-lookup"><span data-stu-id="851a4-133">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA</span></span> |
| <span data-ttu-id="851a4-134">SRV</span><span class="sxs-lookup"><span data-stu-id="851a4-134">SRV</span></span> |<span data-ttu-id="851a4-135">szolgáltatás bejegyzéstípus</span><span class="sxs-lookup"><span data-stu-id="851a4-135">service record type</span></span> |<span data-ttu-id="851a4-136">/Subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SRV</span><span class="sxs-lookup"><span data-stu-id="851a4-136">/subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV</span></span> |

<span data-ttu-id="851a4-137"><sup>1</sup> csak lehetővé teszi, hogy egy érték / rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="851a4-137"><sup>1</sup> only allows one value per record set.</span></span>

<span data-ttu-id="851a4-138"><sup>2</sup> csak lehetővé teszi, hogy egy rekordtípus SOA / DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="851a4-138"><sup>2</sup> only allows one record type SOA per DNS zone.</span></span> 

<span data-ttu-id="851a4-139">Minta DNS-zóna Json formátumban:</span><span class="sxs-lookup"><span data-stu-id="851a4-139">Sample of DNS zone in Json format:</span></span>

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "hello name of hello DNS zone toobe created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "hello name of hello DNS record toobe created.  hello name is relative toohello zone, not hello FQDN."
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

## <a name="additional-resources"></a><span data-ttu-id="851a4-140">További források</span><span class="sxs-lookup"><span data-stu-id="851a4-140">Additional resources</span></span>
<span data-ttu-id="851a4-141">Olvasási hello [DNS-zónák REST API dokumentációja ](https://msdn.microsoft.com/library/azure/mt130626.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="851a4-141">Read hello [REST API documentation for DNS zones ](https://msdn.microsoft.com/library/azure/mt130626.aspx) for more information.</span></span>

<span data-ttu-id="851a4-142">Olvasási hello [DNS-rekordhalmazok REST API dokumentációja](https://msdn.microsoft.com/library/azure/mt130627.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="851a4-142">Read hello [REST API documentation for DNS record sets](https://msdn.microsoft.com/library/azure/mt130627.aspx) for more information.</span></span>

