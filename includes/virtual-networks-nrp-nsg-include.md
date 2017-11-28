## <a name="network-security-group"></a><span data-ttu-id="ad2eb-101">Hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="ad2eb-101">Network Security Group</span></span>
<span data-ttu-id="ad2eb-102">Az NSG-erőforrás létrehozását lehetővé tevő biztonsági határ munkaterhelésekhez, implementálásával engedélyezése, és megtagadási szabályoknak.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-102">An NSG resource enables the creation of security boundary for workloads, by implementing allow and deny rules.</span></span> <span data-ttu-id="ad2eb-103">Ezek a szabályok alkalmazhatók a virtuális gép, egy hálózati adapter vagy egy alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-103">Such rules can be applied to a VM, a NIC, or a subnet.</span></span>

| <span data-ttu-id="ad2eb-104">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ad2eb-104">Property</span></span> | <span data-ttu-id="ad2eb-105">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad2eb-105">Description</span></span> | <span data-ttu-id="ad2eb-106">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="ad2eb-106">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad2eb-107">**alhálózatok**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-107">**subnets**</span></span> |<span data-ttu-id="ad2eb-108">Az NSG vonatkozik, az alhálózati azonosítók listáját.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-108">List of subnet ids the NSG is applied to.</span></span> |<span data-ttu-id="ad2eb-109">/Subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/Providers/Microsoft.Network/virtualNetworks/TestVNet/Subnets/FrontEnd</span><span class="sxs-lookup"><span data-stu-id="ad2eb-109">/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd</span></span> |
| <span data-ttu-id="ad2eb-110">**securityRules**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-110">**securityRules**</span></span> |<span data-ttu-id="ad2eb-111">Az NSG alkotó biztonsági szabályok listája</span><span class="sxs-lookup"><span data-stu-id="ad2eb-111">List of security rules that make up the NSG</span></span> |<span data-ttu-id="ad2eb-112">Lásd: [biztonsági szabály](#Security-rule) alatt</span><span class="sxs-lookup"><span data-stu-id="ad2eb-112">See [Security rule](#Security-rule) below</span></span> |
| <span data-ttu-id="ad2eb-113">**defaultSecurityRules**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-113">**defaultSecurityRules**</span></span> |<span data-ttu-id="ad2eb-114">Minden NSG található alapértelmezett biztonsági szabályok listája</span><span class="sxs-lookup"><span data-stu-id="ad2eb-114">List of default security rules present in every NSG</span></span> |<span data-ttu-id="ad2eb-115">Lásd: [alapértelmezett biztonsági szabályok](#Default-security-rules) alatt</span><span class="sxs-lookup"><span data-stu-id="ad2eb-115">See [Default security rules](#Default-security-rules) below</span></span> |

* <span data-ttu-id="ad2eb-116">**A biztonsági szabály** -egy NSG rendelkezhet több biztonsági szabály lett meghatározva.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-116">**Security rule** - An NSG can have multiple security rules defined.</span></span> <span data-ttu-id="ad2eb-117">Minden egyes szabály engedélyezheti vagy tagadhatja különböző típusú forgalom.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-117">Each rule can allow or deny different types of traffic.</span></span>

### <a name="security-rule"></a><span data-ttu-id="ad2eb-118">A biztonsági szabály</span><span class="sxs-lookup"><span data-stu-id="ad2eb-118">Security rule</span></span>
<span data-ttu-id="ad2eb-119">A szabály egy NSG-t az alábbi tulajdonságait tartalmazó gyermek erőforrása.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-119">A security rule is a child resource of an NSG containing the properties below.</span></span>

| <span data-ttu-id="ad2eb-120">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ad2eb-120">Property</span></span> | <span data-ttu-id="ad2eb-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="ad2eb-121">Description</span></span> | <span data-ttu-id="ad2eb-122">Példaértékek</span><span class="sxs-lookup"><span data-stu-id="ad2eb-122">Sample values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ad2eb-123">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-123">**description**</span></span> |<span data-ttu-id="ad2eb-124">Egy leírást a szabályhoz</span><span class="sxs-lookup"><span data-stu-id="ad2eb-124">Description for the rule</span></span> |<span data-ttu-id="ad2eb-125">Bejövő adatforgalom engedélyezésére X alhálózatban lévő virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="ad2eb-125">Allow inbound traffic for all VMs in subnet X</span></span> |
| <span data-ttu-id="ad2eb-126">**protokoll**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-126">**protocol**</span></span> |<span data-ttu-id="ad2eb-127">A szabálynak megfelelő protokoll</span><span class="sxs-lookup"><span data-stu-id="ad2eb-127">Protocol to match for the rule</span></span> |<span data-ttu-id="ad2eb-128">TCP, UDP vagy *</span><span class="sxs-lookup"><span data-stu-id="ad2eb-128">TCP, UDP, or *</span></span> |
| <span data-ttu-id="ad2eb-129">**sourcePortRange**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-129">**sourcePortRange**</span></span> |<span data-ttu-id="ad2eb-130">A szabálynak megfelelő forrásporttartomány</span><span class="sxs-lookup"><span data-stu-id="ad2eb-130">Source port range to match for the rule</span></span> |<span data-ttu-id="ad2eb-131">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="ad2eb-131">80, 100-200, *</span></span> |
| <span data-ttu-id="ad2eb-132">**destinationPortRange**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-132">**destinationPortRange**</span></span> |<span data-ttu-id="ad2eb-133">A szabálynak megfelelő célporttartomány</span><span class="sxs-lookup"><span data-stu-id="ad2eb-133">Destination port range to match for the rule</span></span> |<span data-ttu-id="ad2eb-134">80, 100-200, *</span><span class="sxs-lookup"><span data-stu-id="ad2eb-134">80, 100-200, *</span></span> |
| <span data-ttu-id="ad2eb-135">**sourceAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-135">**sourceAddressPrefix**</span></span> |<span data-ttu-id="ad2eb-136">A szabálynak megfelelő forráscím-előtag</span><span class="sxs-lookup"><span data-stu-id="ad2eb-136">Source address prefix to match for the rule</span></span> |<span data-ttu-id="ad2eb-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="ad2eb-137">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="ad2eb-138">**destinationAddressPrefix**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-138">**destinationAddressPrefix**</span></span> |<span data-ttu-id="ad2eb-139">A szabálynak megfelelő célcím-előtag</span><span class="sxs-lookup"><span data-stu-id="ad2eb-139">Destination address prefix to match for the rule</span></span> |<span data-ttu-id="ad2eb-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="ad2eb-140">10.10.10.1, 10.10.10.0/24, VirtualNetwork</span></span> |
| <span data-ttu-id="ad2eb-141">**iránya**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-141">**direction**</span></span> |<span data-ttu-id="ad2eb-142">A forgalom szabálynak megfelelő iránya</span><span class="sxs-lookup"><span data-stu-id="ad2eb-142">Direction of traffic to match for the rule</span></span> |<span data-ttu-id="ad2eb-143">bejövő vagy kimenő</span><span class="sxs-lookup"><span data-stu-id="ad2eb-143">inbound or outbound</span></span> |
| <span data-ttu-id="ad2eb-144">**prioritás**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-144">**priority**</span></span> |<span data-ttu-id="ad2eb-145">A szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-145">Priority for the rule.</span></span> <span data-ttu-id="ad2eb-146">Szabályok prioritás szerinti sorrendben ellenőrzi, a szabály vonatkozik, ha nincsenek további szabályok kiértékelésének egyeztetéséhez.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-146">Rules are checked int he order of priority, once a rule applies, no more rules are tested for matching.</span></span> |<span data-ttu-id="ad2eb-147">10, 100, 65000</span><span class="sxs-lookup"><span data-stu-id="ad2eb-147">10, 100, 65000</span></span> |
| <span data-ttu-id="ad2eb-148">**hozzáférés**</span><span class="sxs-lookup"><span data-stu-id="ad2eb-148">**access**</span></span> |<span data-ttu-id="ad2eb-149">Az alkalmazandó hozzáférés típusa, ha a csomag megfelel a szabálynak</span><span class="sxs-lookup"><span data-stu-id="ad2eb-149">Type of access to apply if the rule matches</span></span> |<span data-ttu-id="ad2eb-150">engedélyezés vagy megtagadás</span><span class="sxs-lookup"><span data-stu-id="ad2eb-150">allow or deny</span></span> |

<span data-ttu-id="ad2eb-151">NSG a JSON formátum. minta:</span><span class="sxs-lookup"><span data-stu-id="ad2eb-151">Sample NSG in JSON format:</span></span>

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

### <a name="default-security-rules"></a><span data-ttu-id="ad2eb-152">Alapértelmezett szabályok</span><span class="sxs-lookup"><span data-stu-id="ad2eb-152">Default security rules</span></span>

<span data-ttu-id="ad2eb-153">Alapértelmezett szabályok érhető el a biztonsági szabályok azonos tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-153">Default security rules have the same properties available in security rules.</span></span> <span data-ttu-id="ad2eb-154">Adja meg, amelyek az NSG-k vonatkoznak annak biztosítása érdekében erőforrások közötti hálózati kapcsolat léteznek.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-154">They exist to provide basic connectivity between resources that have NSGs applied to them.</span></span> <span data-ttu-id="ad2eb-155">Ellenőrizze, hogy tudja, amely [alapértelmezett biztonsági szabályok](../articles/virtual-network/virtual-networks-nsg.md#default-rules) létezik.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-155">Make sure you know which [default security rules](../articles/virtual-network/virtual-networks-nsg.md#default-rules) exist.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="ad2eb-156">További források</span><span class="sxs-lookup"><span data-stu-id="ad2eb-156">Additional resources</span></span>
* <span data-ttu-id="ad2eb-157">További információk [NSG-k](../articles/virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="ad2eb-157">Get more information about [NSGs](../articles/virtual-network/virtual-networks-nsg.md).</span></span>
* <span data-ttu-id="ad2eb-158">Olvassa el a [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163615.aspx) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-158">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163615.aspx) for NSGs.</span></span>
* <span data-ttu-id="ad2eb-159">Olvassa el a [REST API referenciadokumentációt](https://msdn.microsoft.com/library/azure/mt163580.aspx) a biztonsági szabályok.</span><span class="sxs-lookup"><span data-stu-id="ad2eb-159">Read the [REST API reference documentation](https://msdn.microsoft.com/library/azure/mt163580.aspx) for security rules.</span></span>
