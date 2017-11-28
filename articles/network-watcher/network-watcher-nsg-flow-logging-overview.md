---
title: "Hálózati biztonsági csoportok Azure hálózati figyelőt a naplózás folyamata bemutatása |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan NSG folyamata naplók Azure hálózati figyelőt szolgáltatása"
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
ms.openlocfilehash: b7a9162d6c6219b6b1c51a49cd34b9616e9d3e8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a><span data-ttu-id="c4824-103">A hálózati biztonsági csoportok folyamata naplózási bemutatása</span><span class="sxs-lookup"><span data-stu-id="c4824-103">Introduction to flow logging for Network Security Groups</span></span>

<span data-ttu-id="c4824-104">Hálózati biztonsági csoport folyamat egyik funkciója, amely lehetővé teszi, hogy tekintse meg a hálózati biztonsági csoporton keresztül bemenő és kimenő IP-forgalom hálózati figyelőt feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="c4824-104">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="c4824-105">A folyamat naplók json formátumban vannak megírva, és a kimenő és bejövő forgalom alapon egy szabályt, a hálózati adapter a folyamat vonatkozik, a folyamat (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), 5 rekordos információk megjelenítése, és ha a forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="c4824-105">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

![Attribútumfolyam naplók – áttekintés][1]

<span data-ttu-id="c4824-107">Attribútumfolyam naplózza a cél hálózati biztonsági csoportok, amíg azok nem azonos a többi naplófájlt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c4824-107">While flow logs target Network Security Groups, they are not displayed the same as the other logs.</span></span> <span data-ttu-id="c4824-108">Attribútumfolyam naplófájljainak tárolása csak egy tárfiókon belül, majd a naplózás elérési út a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="c4824-108">Flow logs are stored only within a storage account and following the logging path as shown in the following example:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="c4824-109">Az azonos megőrzési házirendek, a többi naplófájlt látott folyamata naplók vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="c4824-109">The same retention policies as seen on other logs apply to flow logs.</span></span> <span data-ttu-id="c4824-110">Naplók rendelkezik egy megőrzési házirend – 365 nap beállítható az 1 nap.</span><span class="sxs-lookup"><span data-stu-id="c4824-110">Logs have a retention policy that can be set from 1 day to 365 days.</span></span> <span data-ttu-id="c4824-111">Ha nincs beállítva adatmegőrzési szabály, a naplók megőrzése korlátlan időre szól.</span><span class="sxs-lookup"><span data-stu-id="c4824-111">If a retention policy is not set, the logs are maintained forever.</span></span>

## <a name="log-file"></a><span data-ttu-id="c4824-112">Naplófájl</span><span class="sxs-lookup"><span data-stu-id="c4824-112">Log file</span></span>

<span data-ttu-id="c4824-113">Attribútumfolyam naplók több tulajdonságokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c4824-113">Flow logs have multiple properties.</span></span> <span data-ttu-id="c4824-114">Az alábbi lista a NSG folyamata napló belül visszaadott tulajdonságainak listája:</span><span class="sxs-lookup"><span data-stu-id="c4824-114">The following list is a listing of the properties that are returned within the NSG flow log:</span></span>

* <span data-ttu-id="c4824-115">**idő** - idő, amikor az esemény</span><span class="sxs-lookup"><span data-stu-id="c4824-115">**time** - Time when the event was logged</span></span>
* <span data-ttu-id="c4824-116">**Rendszerazonosító** -hálózati biztonsági csoport erőforrás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="c4824-116">**systemId** - Network Security Group resource Id.</span></span>
* <span data-ttu-id="c4824-117">**kategória** -kategória az eseményről, a rendszer mindig kell NetworkSecurityGroupFlowEvent</span><span class="sxs-lookup"><span data-stu-id="c4824-117">**category** - The category of the event, this is always be NetworkSecurityGroupFlowEvent</span></span>
* <span data-ttu-id="c4824-118">**ResourceId** -erőforrás-azonosító, az NSG</span><span class="sxs-lookup"><span data-stu-id="c4824-118">**resourceid** - The resource Id of the NSG</span></span>
* <span data-ttu-id="c4824-119">**operationName** -mindig NetworkSecurityGroupFlowEvents</span><span class="sxs-lookup"><span data-stu-id="c4824-119">**operationName** - Always NetworkSecurityGroupFlowEvents</span></span>
* <span data-ttu-id="c4824-120">**Tulajdonságok** -áramlását tulajdonságainak gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="c4824-120">**properties** - A collection of properties of the flow</span></span>
    * <span data-ttu-id="c4824-121">**Verzió** -az egymást követő esemény séma verziószáma</span><span class="sxs-lookup"><span data-stu-id="c4824-121">**Version** - Version number of the Flow Log event schema</span></span>
    * <span data-ttu-id="c4824-122">**adatfolyamok** -adatfolyamok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="c4824-122">**flows** - A collection of flows.</span></span> <span data-ttu-id="c4824-123">Ez a tulajdonság különböző szabályok több bejegyzést tartalmaz</span><span class="sxs-lookup"><span data-stu-id="c4824-123">This property has multiple entries for different rules</span></span>
        * <span data-ttu-id="c4824-124">**a szabály** -a adatfolyamok találhatók a szabály</span><span class="sxs-lookup"><span data-stu-id="c4824-124">**rule** - Rule for which the flows are listed</span></span>
            * <span data-ttu-id="c4824-125">**adatfolyamok** -adatfolyamok gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="c4824-125">**flows** - a collection of flows</span></span>
                * <span data-ttu-id="c4824-126">**Mac** – a hálózati Adaptert a virtuális gép, ahol a folyamat gyűjtése történt a MAC-címe</span><span class="sxs-lookup"><span data-stu-id="c4824-126">**mac** - The MAC address of the NIC for the VM where the flow was collected</span></span>
                * <span data-ttu-id="c4824-127">**flowTuples** -karakterlánc, amely a folyamat rekord vesszővel tagolt formátumú több tulajdonságait tartalmazza</span><span class="sxs-lookup"><span data-stu-id="c4824-127">**flowTuples** - A string that contains multiple properties for the flow tuple in comma-separated format</span></span>
                    * <span data-ttu-id="c4824-128">**Időbélyeg** -ezt az értéket van, akkor, ha a folyamat történt UNIX EPOCH formátumban</span><span class="sxs-lookup"><span data-stu-id="c4824-128">**Time Stamp** - This value is the time stamp of when the flow occurred in UNIX EPOCH format</span></span>
                    * <span data-ttu-id="c4824-129">**Forrás IP-címe** -a forrás IP-címe</span><span class="sxs-lookup"><span data-stu-id="c4824-129">**Source IP** - The source IP</span></span>
                    * <span data-ttu-id="c4824-130">**Cél IP** -a cél IP-címe</span><span class="sxs-lookup"><span data-stu-id="c4824-130">**Destination IP** - The destination IP</span></span>
                    * <span data-ttu-id="c4824-131">**Forrásport** -forrásport</span><span class="sxs-lookup"><span data-stu-id="c4824-131">**Source Port** - The source port</span></span>
                    * <span data-ttu-id="c4824-132">**Célport** -Port cél</span><span class="sxs-lookup"><span data-stu-id="c4824-132">**Destination Port** - The destination Port</span></span>
                    * <span data-ttu-id="c4824-133">**Protokoll** -megadott protokollal a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="c4824-133">**Protocol** - The protocol of the flow.</span></span> <span data-ttu-id="c4824-134">Érvényes értékek a következők **T** TCP-hez és **U** UDP-hez</span><span class="sxs-lookup"><span data-stu-id="c4824-134">Valid values are **T** for TCP and **U** for UDP</span></span>
                    * <span data-ttu-id="c4824-135">**Forgalom bonyolódjon** -a forgalom áramlását irányát.</span><span class="sxs-lookup"><span data-stu-id="c4824-135">**Traffic Flow** - The direction of the traffic flow.</span></span> <span data-ttu-id="c4824-136">Érvényes értékek a következők **I** bejövő és **O** a kimenő.</span><span class="sxs-lookup"><span data-stu-id="c4824-136">Valid values are **I** for inbound and **O** for outbound.</span></span>
                    * <span data-ttu-id="c4824-137">**Forgalom** – akár forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="c4824-137">**Traffic** - Whether traffic was allowed or denied.</span></span> <span data-ttu-id="c4824-138">Érvényes értékek a következők **A** engedélyezett, és **D** a megtagadva.</span><span class="sxs-lookup"><span data-stu-id="c4824-138">Valid values are **A** for allowed and **D** for denied.</span></span>


<span data-ttu-id="c4824-139">Egy folyamat napló példát a következő:</span><span class="sxs-lookup"><span data-stu-id="c4824-139">The following is an example of a Flow log.</span></span> <span data-ttu-id="c4824-140">Ahogy látja, kövesse az előző szakaszban leírt keresésitulajdonság-lista több rekordot.</span><span class="sxs-lookup"><span data-stu-id="c4824-140">As you can see there are multiple records that follow the property list described in the preceding section.</span></span> 

> [!NOTE]
> <span data-ttu-id="c4824-141">A flowTuples tulajdonság értékei egy vesszővel elválasztott listában.</span><span class="sxs-lookup"><span data-stu-id="c4824-141">Values in the flowTuples property are a comma-separated list.</span></span>
 
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

## <a name="next-steps"></a><span data-ttu-id="c4824-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4824-142">Next steps</span></span>

<span data-ttu-id="c4824-143">Attribútumfolyam naplók engedélyezése ellátogatva [naplózás engedélyezésével Flow](network-watcher-nsg-flow-logging-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c4824-143">Learn how to enable Flow logs by visiting [Enabling Flow logging](network-watcher-nsg-flow-logging-portal.md).</span></span>

<span data-ttu-id="c4824-144">Látogasson el az NSG-naplózás megismerése [elemzések a hálózati biztonsági csoportokkal (NSG-k) jelentkezzen](../virtual-network/virtual-network-nsg-manage-log.md).</span><span class="sxs-lookup"><span data-stu-id="c4824-144">Learn about NSG logging by visiting [Log analytics for network security groups (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md).</span></span>

<span data-ttu-id="c4824-145">Megtudhatja, ha a forgalom engedélyezett vagy megtagadott a virtuális gép ellátogatva [ellenőrizze forgalmat az IP-adatfolyam ellenőrzése](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c4824-145">Find out if traffic is allowed or denied on a VM by visiting [Verify traffic with IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

