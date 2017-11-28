---
title: "aaaAzure IoT Hub IP-kapcsolat szűrők |} Microsoft Docs"
description: "Hogyan toouse az adott IP-címről tooblock kapcsolatok szűrési IP-címeket tooyour Azure IoT-központ. Letilthatja egyes érkező kapcsolatokat vagy IP-címek tartományát."
services: iot-hub
documentationcenter: 
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: f833eac3-5b5f-46a7-a47b-f4f6fc927f3f
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: boltean
ms.openlocfilehash: 45e5906a494561b6108895d86d6a04fc3b52b8fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ip-filters"></a><span data-ttu-id="a6a61-104">IP-szűrők használata</span><span class="sxs-lookup"><span data-stu-id="a6a61-104">Use IP filters</span></span>

<span data-ttu-id="a6a61-105">A biztonság az Azure IoT Hub alapján IoT megoldások fontos eleme.</span><span class="sxs-lookup"><span data-stu-id="a6a61-105">Security is an important aspect of any IoT solution based on Azure IoT Hub.</span></span> <span data-ttu-id="a6a61-106">Néha szüksége tooexplicitly hello IP-címet, amelyből az eszközök csatlakozni tud megadni a biztonsági beállítások részeként.</span><span class="sxs-lookup"><span data-stu-id="a6a61-106">Sometimes you need tooexplicitly specify hello IP addresses from which devices can connect as part of your security configuration.</span></span> <span data-ttu-id="a6a61-107">Hello _IP-szűrő_ funkciója lehetővé teszi elutasítása, vagy meghatározott IPv4-címeket érkező forgalmat fogad tooconfigure szabályait.</span><span class="sxs-lookup"><span data-stu-id="a6a61-107">hello _IP filter_ feature enables you tooconfigure rules for rejecting or accepting traffic from specific IPv4 addresses.</span></span>

## <a name="when-toouse"></a><span data-ttu-id="a6a61-108">Ha toouse</span><span class="sxs-lookup"><span data-stu-id="a6a61-108">When toouse</span></span>

<span data-ttu-id="a6a61-109">Nincsenek két adott használati esetek hasznos tooblock hello IoT-központok végpontjai bizonyos IP-címek esetén:</span><span class="sxs-lookup"><span data-stu-id="a6a61-109">There are two specific use-cases when it is useful tooblock hello IoT Hub endpoints for certain IP addresses:</span></span>

- <span data-ttu-id="a6a61-110">Az IoT hub kell forgalom fogadására csak a megadott IP-címről, és minden más elutasítása.</span><span class="sxs-lookup"><span data-stu-id="a6a61-110">Your IoT hub should receive traffic only from a specified range of IP addresses and reject everything else.</span></span> <span data-ttu-id="a6a61-111">Az IoT hub való használata esetén például [Azure Express Route] toocreate magánhálózati kapcsolatok az IoT-központ és a helyszíni infrastruktúra között.</span><span class="sxs-lookup"><span data-stu-id="a6a61-111">For example, you are using your IoT hub with [Azure Express Route] toocreate private connections between an IoT hub and your on-premises infrastructure.</span></span>
- <span data-ttu-id="a6a61-112">Hello IoT hub-rendszergazda által azonosított, gyanús IP-címekről tooreject forgalom van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a6a61-112">You need tooreject traffic from IP addresses that have been identified as suspicious by hello IoT hub administrator.</span></span>

## <a name="how-filter-rules-are-applied"></a><span data-ttu-id="a6a61-113">Állapotszűrő szabályok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a6a61-113">How filter rules are applied</span></span>

<span data-ttu-id="a6a61-114">IoT Hub szolgáltatási szint hello hello IP Állapotszűrő szabályok érvényesek.</span><span class="sxs-lookup"><span data-stu-id="a6a61-114">hello IP filter rules are applied at hello IoT Hub service level.</span></span> <span data-ttu-id="a6a61-115">Ezért hello IP Állapotszűrő szabályok alkalmazása tooall kapcsolatok az eszközökről és a háttér-alkalmazások bármely támogatott protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="a6a61-115">Therefore hello IP filter rules apply tooall connections from devices and back-end apps using any supported protocol.</span></span>

<span data-ttu-id="a6a61-116">A kapcsolódási kísérlet egy IP-címről, amely megfelel egy rejecting IP szabályt az IoT hub kap egy jogosulatlan 401-es állapotkód és a leírást.</span><span class="sxs-lookup"><span data-stu-id="a6a61-116">Any connection attempt from an IP address that matches a rejecting IP rule in your IoT hub receives an unauthorized 401 status code and description.</span></span> <span data-ttu-id="a6a61-117">hello válaszüzenetet nem említi hello IP szabály.</span><span class="sxs-lookup"><span data-stu-id="a6a61-117">hello response message does not mention hello IP rule.</span></span>

## <a name="default-setting"></a><span data-ttu-id="a6a61-118">Alapértelmezett beállítás</span><span class="sxs-lookup"><span data-stu-id="a6a61-118">Default setting</span></span>

<span data-ttu-id="a6a61-119">Alapértelmezés szerint hello **IP-szűrő** hello portálon az IoT-központ a rács értéke üres.</span><span class="sxs-lookup"><span data-stu-id="a6a61-119">By default, hello **IP Filter** grid in hello portal for an IoT hub is empty.</span></span> <span data-ttu-id="a6a61-120">Ez az alapértelmezett beállítás azt jelenti, hogy a központ kapcsolatokat fogad IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="a6a61-120">This default setting means that your hub accepts connections any IP address.</span></span> <span data-ttu-id="a6a61-121">Az alapértelmezett érték egyenértékű tooa szabály, amely fogadja a hello 0.0.0.0/0 IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="a6a61-121">This default setting is equivalent tooa rule that accepts hello 0.0.0.0/0 IP address range.</span></span>

![Az IoT-központ alapértelmezett IP-szűrési beállítások][img-ip-filter-default]

## <a name="add-or-edit-an-ip-filter-rule"></a><span data-ttu-id="a6a61-123">Az IP-szűrési szabály felvétele vagy szerkesztése</span><span class="sxs-lookup"><span data-stu-id="a6a61-123">Add or edit an IP filter rule</span></span>

<span data-ttu-id="a6a61-124">Amikor egy IP-szűrő szabályt, a következő értékek hello kéri:</span><span class="sxs-lookup"><span data-stu-id="a6a61-124">When you add an IP filter rule, you are prompted for hello following values:</span></span>

- <span data-ttu-id="a6a61-125">Egy **IP-szűrés szabálynév** , amely másolatot too128 karakter egyedi, nem betűérzékeny, alfanumerikus karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a6a61-125">An **IP filter rule name** that must be a unique, case-insensitive, alphanumeric string up too128 characters long.</span></span> <span data-ttu-id="a6a61-126">Csak ASCII 7 bites hello alfanumerikus karakterek plusz `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` elfogadottak.</span><span class="sxs-lookup"><span data-stu-id="a6a61-126">Only hello ASCII 7-bit alphanumeric characters plus `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` are accepted.</span></span>
- <span data-ttu-id="a6a61-127">Válassza ki a **elutasítása** vagy **fogadja el** , hello **művelet** hello IP-szűrési szabály.</span><span class="sxs-lookup"><span data-stu-id="a6a61-127">Select a **reject** or **accept** as hello **action** for hello IP filter rule.</span></span>
- <span data-ttu-id="a6a61-128">Adjon meg egy IPv4-cím vagy IP-címek a CIDR jelölésrendszer blokkban.</span><span class="sxs-lookup"><span data-stu-id="a6a61-128">Provide a single IPv4 address or a block of IP addresses in CIDR notation.</span></span> <span data-ttu-id="a6a61-129">Például a CIDR jelölésrendszer 192.168.100.0/22 jelöli hello 1024 IPv4-címek 192.168.100.0 too192.168.103.255.</span><span class="sxs-lookup"><span data-stu-id="a6a61-129">For example, in CIDR notation 192.168.100.0/22 represents hello 1024 IPv4 addresses from 192.168.100.0 too192.168.103.255.</span></span>

![Adja hozzá az IP szűrési szabály tooan IoT-központ][img-ip-filter-add-rule]

<span data-ttu-id="a6a61-131">Hello szabály mentése után megjelenik egy figyelmeztetés értesíti, hogy hello frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="a6a61-131">After you save hello rule, you see an alert notifying you that hello update is in progress.</span></span>

![Értesítés egy IP-szűrési szabály mentése][img-ip-filter-save-new-rule]

<span data-ttu-id="a6a61-133">Hello **Hozzáadás** lehetőség le van tiltva, ha eléri hello legfeljebb 10 IP-szűrő szabályokban.</span><span class="sxs-lookup"><span data-stu-id="a6a61-133">hello **Add** option is disabled when you reach hello maximum of 10 IP filter rules.</span></span>

<span data-ttu-id="a6a61-134">Meglévő szabály hello sor hello szabályt tartalmazó duplán kattintva módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a6a61-134">You can edit an existing rule by double-clicking hello row that contains hello rule.</span></span>

> [!NOTE]
> <span data-ttu-id="a6a61-135">Elutasítása IP címeket így előfordulhat, hogy más Azure-szolgáltatások (például az Azure Stream Analytics, Azure virtuális gépek vagy hello eszköz Explorer hello portálon) hello IoT-központ való interakció.</span><span class="sxs-lookup"><span data-stu-id="a6a61-135">Rejecting IP addresses can prevent other Azure Services (such as Azure Stream Analytics, Azure Virtual Machines, or hello Device Explorer in hello portal) from interacting with hello IoT hub.</span></span>

> [!WARNING]
> <span data-ttu-id="a6a61-136">IP-szűrés engedélyezve van az IoT-központ tooread üzeneteit Azure Stream Analytics (ASA) használja, ha használja hello Event Hub-kompatibilis nevét és az IoT-központ végpontjának a hello ASA kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a6a61-136">If you use Azure Stream Analytics (ASA) tooread messages from an IoT hub with IP filtering enabled, use hello Event Hub-compatible name and endpoint of your IoT Hub in hello ASA connection string.</span></span>

## <a name="delete-an-ip-filter-rule"></a><span data-ttu-id="a6a61-137">Az IP-szűrési szabály törlése</span><span class="sxs-lookup"><span data-stu-id="a6a61-137">Delete an IP filter rule</span></span>

<span data-ttu-id="a6a61-138">toodelete egy IP-szűrő szabályt, jelöljön ki egy vagy több szabály hello rács kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="a6a61-138">toodelete an IP filter rule, select one or more rules in hello grid and click **Delete**.</span></span>

![Az IoT-központ IP-szűrési szabály törlése][img-ip-filter-delete-rule]

## <a name="ip-filter-rule-evaluation"></a><span data-ttu-id="a6a61-140">IP-szűrési szabály értékelése</span><span class="sxs-lookup"><span data-stu-id="a6a61-140">IP filter rule evaluation</span></span>

<span data-ttu-id="a6a61-141">IP-szűrési szabályokat a rendszer sorrendben alkalmazza, és hello első szabály, hogy megfelel hello IP-címét határozza meg a hello fogadja el vagy utasítsa el a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a6a61-141">IP filter rules are applied in order and hello first rule that matches hello IP address determines hello accept or reject action.</span></span>

<span data-ttu-id="a6a61-142">Például ha szeretné, hogy hello tartomány 192.168.100.0/22 tooaccept címeit, és utasítsa el minden más, hello első szabály hello rácsban hello cím tartomány 192.168.100.0/22 kell fogadnia.</span><span class="sxs-lookup"><span data-stu-id="a6a61-142">For example, if you want tooaccept addresses in hello range 192.168.100.0/22 and reject everything else, hello first rule in hello grid should accept hello address range 192.168.100.0/22.</span></span> <span data-ttu-id="a6a61-143">hello közvetkező szabályának hello tartomány 0.0.0.0/0 használatával kell utasítania összes címet.</span><span class="sxs-lookup"><span data-stu-id="a6a61-143">hello next rule should reject all addresses by using hello range 0.0.0.0/0.</span></span>

<span data-ttu-id="a6a61-144">Az IP-Állapotszűrő szabályok hello rácsban hello sorrendjének módosításához kattintson a hello függőleges három pont sor hello elején, majd húzza használatával, és dobja el.</span><span class="sxs-lookup"><span data-stu-id="a6a61-144">You can change hello order of your IP filter rules in hello grid by clicking hello three vertical dots at hello start of a row and using drag and drop.</span></span>

<span data-ttu-id="a6a61-145">toosave az új IP-szűrés rendelés szabály, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="a6a61-145">toosave your new IP filter rule order, click **Save**.</span></span>

![Az IoT-központ IP-szűrési szabályokat hello sorrendjének módosítása][img-ip-filter-rule-order]

## <a name="next-steps"></a><span data-ttu-id="a6a61-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6a61-147">Next steps</span></span>

<span data-ttu-id="a6a61-148">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="a6a61-148">toofurther explore hello capabilities of IoT Hub, see:</span></span>

- <span data-ttu-id="a6a61-149">[Figyelési műveletek][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="a6a61-149">[Operations monitoring][lnk-monitor]</span></span>
- <span data-ttu-id="a6a61-150">[Az IoT-központ metrikák][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="a6a61-150">[IoT Hub metrics][lnk-metrics]</span></span>

<!-- Images -->
[img-ip-filter-default]: ./media/iot-hub-ip-filtering/ip-filter-default.png
[img-ip-filter-add-rule]: ./media/iot-hub-ip-filtering/ip-filter-add-rule.png
[img-ip-filter-save-new-rule]: ./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png
[img-ip-filter-delete-rule]: ./media/iot-hub-ip-filtering/ip-filter-delete-rule.png
[img-ip-filter-rule-order]: ./media/iot-hub-ip-filtering/ip-filter-rule-order.png


<!-- Links -->

[IoT Hub developer guide]: iot-hub-devguide.md
[Azure Express Route]:  https://azure.microsoft.com/en-us/documentation/articles/expressroute-faqs/#supported-services

[lnk-monitor]: iot-hub-operations-monitoring.md
[lnk-metrics]: iot-hub-metrics.md