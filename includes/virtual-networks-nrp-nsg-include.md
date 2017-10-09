## <a name="network-security-group"></a><span data-ttu-id="2291d-101">Hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="2291d-101">Network Security Group</span></span>
<span data-ttu-id="2291d-102">Az NSG-erőforrás lehetővé teszi a munkaterhelések biztonsági határ hello létrehozását, implementálásával engedélyezése, és megtagadási szabályoknak.</span><span class="sxs-lookup"><span data-stu-id="2291d-102">An NSG resource enables hello creation of security boundary for workloads, by implementing allow and deny rules.</span></span> <span data-ttu-id="2291d-103">Ezek a szabályok lehetnek tooa virtuális gép, egy hálózati adapter vagy alhálózat alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="2291d-103">Such rules can be applied tooa VM, a NIC, or a subnet.</span></span>

| <span data-ttu-id="2291d-104">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2291d-104">Property</span></span> | <span data-ttu-id="2291d-105">Leírás</span><span class="sxs-lookup"><span data-stu-id="2291d-105">Description</span></span> | <span data-ttu-id="2291d-106">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="2291d-106">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2291d-107">**alhálózatok**</span><span class="sxs-lookup"><span data-stu-id="2291d-107">**subnets**</span></span> |<span data-ttu-id="2291d-108">NSG vonatkozik, az alhálózati azonosítók hello listája.</span><span class="sxs-lookup"><span data-stu-id="2291d-108">List of subnet ids hello NSG is applied to.</span></span> |<span data-ttu-id="2291d-109">/Subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/Providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/FrontEnd</span><span class="sxs-lookup"><span data-stu-id="2291d-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd</span></span> |
| <span data-ttu-id="2291d-110">**securityRules**</span><span class="sxs-lookup"><span data-stu-id="2291d-110">**securityRules**</span></span> |<span data-ttu-id="2291d-111">Hello NSG alkotó biztonsági szabályok listája</span><span class="sxs-lookup"><span data-stu-id="2291d-111">List of security rules that make up hello NSG</span></span> |<span data-ttu-id="2291d-112">Lásd: [biztonsági szabály](#Security-rule) alatt</span><span class="sxs-lookup"><span data-stu-id="2291d-112">See [Security rule](#Security-rule) below</span></span> |
| <span data-ttu-id="2291d-113">**defaultSecurityRules**</span><span class="sxs-lookup"><span data-stu-id="2291d-113">**defaultSecurityRules**</span></span> |<span data-ttu-id="2291d-114">Minden NSG található alapértelmezett biztonsági szabályok listája</span><span class="sxs-lookup"><span data-stu-id="2291d-114">List of default security rules present in every NSG</span></span> |<span data-ttu-id="2291d-115">Lásd: [alapértelmezett biztonsági szabályok](#Default-security-rules) alatt</span><span class="sxs-lookup"><span data-stu-id="2291d-115">See [Default security rules](#Default-security-rules) below</span></span> |

* <span data-ttu-id="2291d-116">**A biztonsági szabály** -egy NSG rendelkezhet több biztonsági szabály lett meghatározva.</span><span class="sxs-lookup"><span data-stu-id="2291d-116">**Security rule** - An NSG can have multiple security rules defined.</span></span> <span data-ttu-id="2291d-117">Minden egyes szabály engedélyezheti vagy tagadhatja különböző típusú forgalom.</span><span class="sxs-lookup"><span data-stu-id="2291d-117">Each rule can allow or deny different types of traffic.</span></span>

### <a name="security-rule"></a><span data-ttu-id="2291d-118">A biztonsági szabály</span><span class="sxs-lookup"><span data-stu-id="2291d-118">Security rule</span></span>
<span data-ttu-id="2291d-119">A szabály egy NSG-t az alábbi hello tulajdonságokat tartalmazó gyermek erőforrása.</span><span class="sxs-lookup"><span data-stu-id="2291d-119">A security rule is a child resource of an NSG containing hello properties below.</span></span>

| <span data-ttu-id="2291d-120">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2291d-120">Property</span></span> | <span data-ttu-id="2291d-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="2291d-121">Description</span></span> | <span data-ttu-id="2291d-122">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="2291d-122">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2291d-123">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="2291d-123">**description**</span></span> |<span data-ttu-id="2291d-124">Hello szabály leírása</span><span class="sxs-lookup"><span data-stu-id="2291d-124">Description for hello rule</span></span> |<span data-ttu-id="2291d-125">Bejövő adatforgalom engedélyezésére X alhálózatban lévő virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="2291d-125">Allow inbound traffic for all VMs in subnet X</span></span> |
| <span data-ttu-id="2291d-126">**protokoll**</span><span class="sxs-lookup"><span data-stu-id="2291d-126">**protocol**</span></span> |<span data-ttu-id="2291d-127">Protokoll toomatch hello szabály</span><span class="sxs-lookup"><span data-stu-id="2291d-127">Protocol toomatch for hello rule</span></span> |<span data-ttu-id="2291d-128">TCP, UDP vagy *</span><span class="sxs-lookup"><span data-stu-id="2291d-128">TCP, UDP, or *</span></span> |
| <span data-ttu-id="2291d-129">**sourcePortRange**</span><span class="sxs-lookup"><span data-stu-id="2291d-129">**sourcePortRange**</span></span> |<span data-ttu-id="2291d-130">Source port tartomány toomatch hello szabály</span><span class="sxs-lookup"><span data-stu-id="2291d-130">Source port range toomatch for hello rule</span></span> |<span data-ttu-id="2291d-131">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="2291d-131">80, 100-200, *</span></span> |
| <span data-ttu-id="2291d-132">**destinationPortRange**</span><span class="sxs-lookup"><span data-stu-id="2291d-132">**destinationPortRange**</span></span> |<span data-ttu-id="2291d-133">Cél port tartomány toomatch hello szabály</span><span class="sxs-lookup"><span data-stu-id="2291d-133">Destination port range toomatch for hello rule</span></span> |<span data-ttu-id="2291d-134">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="2291d-134">80, 100-200, *</span></span> |
| <span data-ttu-id="2291d-135">**sourceAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="2291d-135">**sourceAddressPrefix**</span></span> |<span data-ttu-id="2291d-136">Forrás cím előtag toomatch hello szabály</span><span class="sxs-lookup"><span data-stu-id="2291d-136">Source address prefix toomatch for hello rule</span></span> |<span data-ttu-id="2291d-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="2291d-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="2291d-138">**destinationAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="2291d-138">**destinationAddressPrefix**</span></span> |<span data-ttu-id="2291d-139">Rendeltetési cím előtag toomatch hello szabály</span><span class="sxs-lookup"><span data-stu-id="2291d-139">Destination address prefix toomatch for hello rule</span></span> |<span data-ttu-id="2291d-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="2291d-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="2291d-141">**iránya**</span><span class="sxs-lookup"><span data-stu-id="2291d-141">**direction**</span></span> |<span data-ttu-id="2291d-142">Forgalom toomatch hello szabály irányának</span><span class="sxs-lookup"><span data-stu-id="2291d-142">Direction of traffic toomatch for hello rule</span></span> |<span data-ttu-id="2291d-143">bejövő vagy kimenő</span><span class="sxs-lookup"><span data-stu-id="2291d-143">inbound or outbound</span></span> |
| <span data-ttu-id="2291d-144">**prioritás**</span><span class="sxs-lookup"><span data-stu-id="2291d-144">**priority**</span></span> |<span data-ttu-id="2291d-145">Hello szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="2291d-145">Priority for hello rule.</span></span> <span data-ttu-id="2291d-146">Szabályok prioritás szerinti sorrendben ellenőrzi, a szabály vonatkozik, ha nincsenek további szabályok kiértékelésének egyeztetéséhez.</span><span class="sxs-lookup"><span data-stu-id="2291d-146">Rules are checked int he order of priority, once a rule applies, no more rules are tested for matching.</span></span> |<span data-ttu-id="2291d-147">10, 100, 65000</span><span class="sxs-lookup"><span data-stu-id="2291d-147">10, 100, 65000</span></span> |
| <span data-ttu-id="2291d-148">**hozzáférés**</span><span class="sxs-lookup"><span data-stu-id="2291d-148">**access**</span></span> |<span data-ttu-id="2291d-149">Ha hello szabálynak hozzáférés tooapply típusa</span><span class="sxs-lookup"><span data-stu-id="2291d-149">Type of access tooapply if hello rule matches</span></span> |<span data-ttu-id="2291d-150">engedélyezés vagy megtagadás</span><span class="sxs-lookup"><span data-stu-id="2291d-150">allow or deny</span></span> |

<span data-ttu-id="2291d-151">NSG a JSON formátum. minta:</span><span class="sxs-lookup"><span data-stu-id="2291d-151">Sample NSG in JSON format:</span></span>

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a><span data-ttu-id="2291d-152">Alapértelmezett biztonsági szabályok</span><span class="sxs-lookup"><span data-stu-id="2291d-152">Default security rules</span></span>

<span data-ttu-id="2291d-153">Alapértelmezett biztonsági szabály rendelkezik hello ugyanazok a tulajdonságok a biztonsági szabályok érhető el.</span><span class="sxs-lookup"><span data-stu-id="2291d-153">Default security rules have hello same properties available in security rules.</span></span> <span data-ttu-id="2291d-154">Hálózati kapcsolat tooprovide alkalmazott NSG-k toothem rendelkező erőforrások között vannak ilyenek.</span><span class="sxs-lookup"><span data-stu-id="2291d-154">They exist tooprovide basic connectivity between resources that have NSGs applied toothem.</span></span> <span data-ttu-id="2291d-155">Ellenőrizze, hogy tudja, amely [alapértelmezett biztonsági szabályok](../articles/virtual-network/virtual-networks-nsg.md#default-rules) létezik.</span><span class="sxs-lookup"><span data-stu-id="2291d-155">Make sure you know which [default security rules](../articles/virtual-network/virtual-networks-nsg.md#default-rules) exist.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2291d-156">További források</span><span class="sxs-lookup"><span data-stu-id="2291d-156">Additional resources</span></span>
* <span data-ttu-id="2291d-157">További információk [NSG-k](../articles/virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="2291d-157">Get more information about [NSGs](../articles/virtual-network/virtual-networks-nsg.md).</span></span>
* <span data-ttu-id="2291d-158">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163615.aspx) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="2291d-158">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) for NSGs.</span></span>
* <span data-ttu-id="2291d-159">Olvasási hello [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163580.aspx) a biztonsági szabályok.</span><span class="sxs-lookup"><span data-stu-id="2291d-159">Read hello [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) for security rules.</span></span>
