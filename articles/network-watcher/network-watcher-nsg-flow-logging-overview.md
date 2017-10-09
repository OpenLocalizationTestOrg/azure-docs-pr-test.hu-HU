---
title: "a hálózati biztonsági csoportok az Azure hálózati figyelőt aaaIntroduction tooflow naplózási |} Microsoft Docs"
description: "Ez a lap azt ismerteti, hogyan toouse NSG folyamata naplózza az Azure hálózati figyelőt szolgáltatása"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: da85e946147b14717144cb47d1c742057c6dfa24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooflow-logging-for-network-security-groups"></a><span data-ttu-id="46978-103">Hálózati biztonsági csoportok tooflow naplózását bemutatása</span><span class="sxs-lookup"><span data-stu-id="46978-103">Introduction tooflow logging for Network Security Groups</span></span>

<span data-ttu-id="46978-104">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="46978-104">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="46978-105">A folyamat naplók json formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.</span><span class="sxs-lookup"><span data-stu-id="46978-105">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

![Attribútumfolyam naplók – áttekintés][1]

<span data-ttu-id="46978-107">Attribútumfolyam naplózza a cél hálózati biztonsági csoportok, amíg azok nem jelennek meg hello ugyanaz, mint a hello további naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="46978-107">While flow logs target Network Security Groups, they are not displayed hello same as hello other logs.</span></span> <span data-ttu-id="46978-108">Csak egy tárfiók és a következő hello naplózási elérési út belül folyamata naplófájljainak tárolása a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="46978-108">Flow logs are stored only within a storage account and following hello logging path as shown in hello following example:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="46978-109">hello ugyanaz, mint a többi naplófájlt adatmegőrzési alkalmazni a naplókat tooflow.</span><span class="sxs-lookup"><span data-stu-id="46978-109">hello same retention policies as seen on other logs apply tooflow logs.</span></span> <span data-ttu-id="46978-110">Naplók esetében 1 nap too365 nap beállítható adatmegőrzési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="46978-110">Logs have a retention policy that can be set from 1 day too365 days.</span></span> <span data-ttu-id="46978-111">Ha nincs megadva adatmegőrzési, hello naplók végtelen karbantartása.</span><span class="sxs-lookup"><span data-stu-id="46978-111">If a retention policy is not set, hello logs are maintained forever.</span></span>

## <a name="log-file"></a><span data-ttu-id="46978-112">Naplófájl</span><span class="sxs-lookup"><span data-stu-id="46978-112">Log file</span></span>

<span data-ttu-id="46978-113">Attribútumfolyam naplók több tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="46978-113">Flow logs have multiple properties.</span></span> <span data-ttu-id="46978-114">hello alábbi lista a következő hello tulajdonságok belül hello NSG folyamata napló visszaadott lista tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="46978-114">hello following list is a listing of hello properties that are returned within hello NSG flow log:</span></span>

* <span data-ttu-id="46978-115">**idő** – hello esemény naplózásának ideje</span><span class="sxs-lookup"><span data-stu-id="46978-115">**time** - Time when hello event was logged</span></span>
* <span data-ttu-id="46978-116">**Rendszerazonosító** -hálózati biztonsági csoport erőforrás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="46978-116">**systemId** - Network Security Group resource Id.</span></span>
* <span data-ttu-id="46978-117">**kategória** -hello kategória hello esemény, a rendszer mindig kell NetworkSecurityGroupFlowEvent</span><span class="sxs-lookup"><span data-stu-id="46978-117">**category** - hello category of hello event, this is always be NetworkSecurityGroupFlowEvent</span></span>
* <span data-ttu-id="46978-118">**ResourceId** -erőforrás-azonosítót az hello NSG hello</span><span class="sxs-lookup"><span data-stu-id="46978-118">**resourceid** - hello resource Id of hello NSG</span></span>
* <span data-ttu-id="46978-119">**operationName** -mindig NetworkSecurityGroupFlowEvents</span><span class="sxs-lookup"><span data-stu-id="46978-119">**operationName** - Always NetworkSecurityGroupFlowEvents</span></span>
* <span data-ttu-id="46978-120">**Tulajdonságok** -hello folyamat tulajdonságainak gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="46978-120">**properties** - A collection of properties of hello flow</span></span>
    * <span data-ttu-id="46978-121">**Verzió** -hello egymást követő esemény séma verziószáma</span><span class="sxs-lookup"><span data-stu-id="46978-121">**Version** - Version number of hello Flow Log event schema</span></span>
    * <span data-ttu-id="46978-122">**adatfolyamok** -adatfolyamok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="46978-122">**flows** - A collection of flows.</span></span> <span data-ttu-id="46978-123">Ez a tulajdonság különböző szabályok több bejegyzést tartalmaz</span><span class="sxs-lookup"><span data-stu-id="46978-123">This property has multiple entries for different rules</span></span>
        * <span data-ttu-id="46978-124">**a szabály** -mely hello adatfolyamok megjelennek szabály</span><span class="sxs-lookup"><span data-stu-id="46978-124">**rule** - Rule for which hello flows are listed</span></span>
            * <span data-ttu-id="46978-125">**adatfolyamok** -adatfolyamok gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="46978-125">**flows** - a collection of flows</span></span>
                * <span data-ttu-id="46978-126">**Mac** -hello hello NIC hello VM, ahol hello folyamata gyűjtése történt a MAC-címe</span><span class="sxs-lookup"><span data-stu-id="46978-126">**mac** - hello MAC address of hello NIC for hello VM where hello flow was collected</span></span>
                * <span data-ttu-id="46978-127">**flowTuples** -hello folyamata rekordot vesszővel tagolt formátumú több tulajdonságait tartalmazó karakterlánc</span><span class="sxs-lookup"><span data-stu-id="46978-127">**flowTuples** - A string that contains multiple properties for hello flow tuple in comma-separated format</span></span>
                    * <span data-ttu-id="46978-128">**Időbélyeg** -Ez az érték esetén időbélyegzőjét hello hello folyamat történt UNIX EPOCH formátumban</span><span class="sxs-lookup"><span data-stu-id="46978-128">**Time Stamp** - This value is hello time stamp of when hello flow occurred in UNIX EPOCH format</span></span>
                    * <span data-ttu-id="46978-129">**Forrás IP-címe** – hello forrás IP-címe</span><span class="sxs-lookup"><span data-stu-id="46978-129">**Source IP** - hello source IP</span></span>
                    * <span data-ttu-id="46978-130">**Cél IP** -hello cél IP-címe</span><span class="sxs-lookup"><span data-stu-id="46978-130">**Destination IP** - hello destination IP</span></span>
                    * <span data-ttu-id="46978-131">**Forrásport** – hello forrásport</span><span class="sxs-lookup"><span data-stu-id="46978-131">**Source Port** - hello source port</span></span>
                    * <span data-ttu-id="46978-132">**Célport** -hello célport</span><span class="sxs-lookup"><span data-stu-id="46978-132">**Destination Port** - hello destination Port</span></span>
                    * <span data-ttu-id="46978-133">**Protokoll** -protokoll hello áramlás hello.</span><span class="sxs-lookup"><span data-stu-id="46978-133">**Protocol** - hello protocol of hello flow.</span></span> <span data-ttu-id="46978-134">Érvényes értékek a következők **T** TCP-hez és **U** UDP-hez</span><span class="sxs-lookup"><span data-stu-id="46978-134">Valid values are **T** for TCP and **U** for UDP</span></span>
                    * <span data-ttu-id="46978-135">**Forgalom bonyolódjon** -hello hello adatforgalmat irányát.</span><span class="sxs-lookup"><span data-stu-id="46978-135">**Traffic Flow** - hello direction of hello traffic flow.</span></span> <span data-ttu-id="46978-136">Érvényes értékek a következők **I** bejövő és **O** a kimenő.</span><span class="sxs-lookup"><span data-stu-id="46978-136">Valid values are **I** for inbound and **O** for outbound.</span></span>
                    * <span data-ttu-id="46978-137">**Forgalom** – akár forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="46978-137">**Traffic** - Whether traffic was allowed or denied.</span></span> <span data-ttu-id="46978-138">Érvényes értékek a következők **A** engedélyezett, és **D** a megtagadva.</span><span class="sxs-lookup"><span data-stu-id="46978-138">Valid values are **A** for allowed and **D** for denied.</span></span>


<span data-ttu-id="46978-139">hello az alábbiakban látható egy példa egy folyamat napló.</span><span class="sxs-lookup"><span data-stu-id="46978-139">hello following is an example of a Flow log.</span></span> <span data-ttu-id="46978-140">Az alábbi hello előző szakaszban leírt hello keresésitulajdonság-lista több rekordot látható.</span><span class="sxs-lookup"><span data-stu-id="46978-140">As you can see there are multiple records that follow hello property list described in hello preceding section.</span></span> 

> [!NOTE]
> <span data-ttu-id="46978-141">Hello flowTuples tulajdonság értékei egy vesszővel elválasztott listában.</span><span class="sxs-lookup"><span data-stu-id="46978-141">Values in hello flowTuples property are a comma-separated list.</span></span>
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a><span data-ttu-id="46978-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46978-142">Next steps</span></span>

<span data-ttu-id="46978-143">Ismerje meg, hogy miként naplózza az tooenable folyamat ellátogatva [naplózás engedélyezésével Flow](network-watcher-nsg-flow-logging-portal.md).</span><span class="sxs-lookup"><span data-stu-id="46978-143">Learn how tooenable Flow logs by visiting [Enabling Flow logging](network-watcher-nsg-flow-logging-portal.md).</span></span>

<span data-ttu-id="46978-144">Látogasson el az NSG-naplózás megismerése [elemzések a hálózati biztonsági csoportokkal (NSG-k) jelentkezzen](../virtual-network/virtual-network-nsg-manage-log.md).</span><span class="sxs-lookup"><span data-stu-id="46978-144">Learn about NSG logging by visiting [Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md).</span></span>

<span data-ttu-id="46978-145">Megtudhatja, ha a forgalom engedélyezett vagy megtagadott a virtuális gép ellátogatva [ellenőrizze forgalmat az IP-adatfolyam ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="46978-145">Find out if traffic is allowed or denied on a VM by visiting [Verify traffic with IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

