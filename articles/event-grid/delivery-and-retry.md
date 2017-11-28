---
title: "Az Azure Event rács kézbesítési, és próbálkozzon újra"
description: "Ismerteti, hogyan nyújt az Azure esemény rács a eseményeket, és hogyan kezeli az kézbesítetlen üzenetek."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: e0f8afdfd84ea3c0c061459c27da285f6ae8957e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="event-grid-message-delivery-and-retry"></a><span data-ttu-id="31246-103">Esemény rács üzenetkézbesítést, és próbálkozzon újra</span><span class="sxs-lookup"><span data-stu-id="31246-103">Event Grid message delivery and retry</span></span> 

<span data-ttu-id="31246-104">Ez a cikk ismerteti, miként kezeli az Azure esemény rács a kézbesítési nem vonatkozik, az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="31246-104">This article describes how Azure Event Grid handles events when delivery is not acknowledged.</span></span>

<span data-ttu-id="31246-105">Esemény rács tartós kézbesítési biztosít.</span><span class="sxs-lookup"><span data-stu-id="31246-105">Event Grid provides durable delivery.</span></span> <span data-ttu-id="31246-106">Minden üzenet legalább egyszer az egyes előfizetésekhez biztosítja.</span><span class="sxs-lookup"><span data-stu-id="31246-106">It delivers each message at least once for each subscription.</span></span> <span data-ttu-id="31246-107">Az események küldhetők az egyes előfizetések regisztrált webhook azonnal.</span><span class="sxs-lookup"><span data-stu-id="31246-107">Events are sent to the registered webhook of each subscription immediately.</span></span> <span data-ttu-id="31246-108">A webhook nem átvételét egy eseményt az első kézbesítési kísérlet 60 másodpercen belül, ha az esemény rács kézbesítési esemény újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="31246-108">If a webhook does not acknowledge receipt of an event within 60 seconds of the first delivery attempt, Event Grid retries delivery of the event.</span></span>

## <a name="message-delivery-status"></a><span data-ttu-id="31246-109">Üzenet kézbesítési állapota</span><span class="sxs-lookup"><span data-stu-id="31246-109">Message delivery status</span></span>

<span data-ttu-id="31246-110">Esemény rács események nyugtázta használja a HTTP válaszkódot.</span><span class="sxs-lookup"><span data-stu-id="31246-110">Event Grid uses HTTP response codes to acknowledge receipt of events.</span></span> 

### <a name="success-codes"></a><span data-ttu-id="31246-111">Sikeres kódok</span><span class="sxs-lookup"><span data-stu-id="31246-111">Success codes</span></span>

<span data-ttu-id="31246-112">A következő HTTP-válaszkódot azt jelzi, hogy az esemény kézbesítése megtörtént sikeresen a webhook.</span><span class="sxs-lookup"><span data-stu-id="31246-112">The following HTTP response codes indicate that an event has been delivered successfully to your webhook.</span></span> <span data-ttu-id="31246-113">Esemény rács úgy ítéli meg kézbesítési befejeződött.</span><span class="sxs-lookup"><span data-stu-id="31246-113">Event Grid considers delivery complete.</span></span>

- <span data-ttu-id="31246-114">200 OK</span><span class="sxs-lookup"><span data-stu-id="31246-114">200 OK</span></span>
- <span data-ttu-id="31246-115">202 elfogadott</span><span class="sxs-lookup"><span data-stu-id="31246-115">202 Accepted</span></span>

### <a name="failure-codes"></a><span data-ttu-id="31246-116">Hibát kódok</span><span class="sxs-lookup"><span data-stu-id="31246-116">Failure codes</span></span>

<span data-ttu-id="31246-117">A következő HTTP-válaszkódot azt jelzi, hogy az esemény kézbesítési tett kísérlet meghiúsult.</span><span class="sxs-lookup"><span data-stu-id="31246-117">The following HTTP response codes indicate that an event delivery attempt failed.</span></span> <span data-ttu-id="31246-118">Esemény rács megpróbálja újra elküldeni az esemény.</span><span class="sxs-lookup"><span data-stu-id="31246-118">Event Grid tries again to send the event.</span></span> 

- <span data-ttu-id="31246-119">400 Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="31246-119">400 Bad Request</span></span>
- <span data-ttu-id="31246-120">401 nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="31246-120">401 Unauthorized</span></span>
- <span data-ttu-id="31246-121">404 – Nem található</span><span class="sxs-lookup"><span data-stu-id="31246-121">404 Not Found</span></span>
- <span data-ttu-id="31246-122">408 kérelmi időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="31246-122">408 Request timeout</span></span>
- <span data-ttu-id="31246-123">414 URI túl hosszú</span><span class="sxs-lookup"><span data-stu-id="31246-123">414 URI Too Long</span></span>
- <span data-ttu-id="31246-124">500 belső kiszolgálóhiba</span><span class="sxs-lookup"><span data-stu-id="31246-124">500 Internal Server Error</span></span>
- <span data-ttu-id="31246-125">503-as szolgáltatás nem érhető el</span><span class="sxs-lookup"><span data-stu-id="31246-125">503 Service Unavailable</span></span>
- <span data-ttu-id="31246-126">504-es számú átjáró időtúllépése</span><span class="sxs-lookup"><span data-stu-id="31246-126">504 Gateway Timeout</span></span>

<span data-ttu-id="31246-127">Bármely más válaszkód vagy hiányoznak a választ egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="31246-127">Any other response code or a lack of a response indicates a failure.</span></span> <span data-ttu-id="31246-128">Esemény rács kézbesítési próbálkozások.</span><span class="sxs-lookup"><span data-stu-id="31246-128">Event Grid retries delivery.</span></span> 

## <a name="retry-intervals"></a><span data-ttu-id="31246-129">Újrapróbálkozási időközök</span><span class="sxs-lookup"><span data-stu-id="31246-129">Retry intervals</span></span>

<span data-ttu-id="31246-130">Esemény rács az exponenciális leállítási újrapróbálkozási házirend esemény kézbesítési használ.</span><span class="sxs-lookup"><span data-stu-id="31246-130">Event Grid uses an exponential backoff retry policy for event delivery.</span></span> <span data-ttu-id="31246-131">A webhook nem válaszol vagy hibakódot ad vissza, ha az esemény rács újrapróbálja kézbesítése a következő ütemezés szerint:</span><span class="sxs-lookup"><span data-stu-id="31246-131">If your webhook does not respond or returns a failure code, Event Grid retries delivery on the following schedule:</span></span>

1. <span data-ttu-id="31246-132">10 másodperc</span><span class="sxs-lookup"><span data-stu-id="31246-132">10 seconds</span></span>
2. <span data-ttu-id="31246-133">30 másodperc</span><span class="sxs-lookup"><span data-stu-id="31246-133">30 seconds</span></span>
3. <span data-ttu-id="31246-134">1 perc</span><span class="sxs-lookup"><span data-stu-id="31246-134">1 minute</span></span>
4. <span data-ttu-id="31246-135">5 perc</span><span class="sxs-lookup"><span data-stu-id="31246-135">5 minutes</span></span>
5. <span data-ttu-id="31246-136">10 perc</span><span class="sxs-lookup"><span data-stu-id="31246-136">10 minutes</span></span>
6. <span data-ttu-id="31246-137">30 perc</span><span class="sxs-lookup"><span data-stu-id="31246-137">30 minutes</span></span>
7. <span data-ttu-id="31246-138">1 óra</span><span class="sxs-lookup"><span data-stu-id="31246-138">1 hour</span></span>

<span data-ttu-id="31246-139">Esemény rács intervallumokban újra egy kis véletlenszerű hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="31246-139">Event Grid adds a small randomization to all retry intervals.</span></span>

## <a name="retry-duration"></a><span data-ttu-id="31246-140">Ismételje meg az időtartama</span><span class="sxs-lookup"><span data-stu-id="31246-140">Retry duration</span></span>

<span data-ttu-id="31246-141">Az előzetes Azure esemény rács összes eseményt, amely nem érkeznek meg két órán belül lejár.</span><span class="sxs-lookup"><span data-stu-id="31246-141">During the preview, Azure Event Grid expires all events that are not delivered within two hours.</span></span> <span data-ttu-id="31246-142">Általános rendelkezésre állását, mielőtt most 24 órára növekszik.</span><span class="sxs-lookup"><span data-stu-id="31246-142">Before General Availability, this time will be increased to 24 hours.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="31246-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="31246-143">Next steps</span></span>

* <span data-ttu-id="31246-144">Esemény rácshoz ismertetőért lásd: [esemény rács](overview.md).</span><span class="sxs-lookup"><span data-stu-id="31246-144">For an introduction to Event Grid, see [About Event Grid](overview.md).</span></span>
* <span data-ttu-id="31246-145">Ha gyorsan esemény rács segítségével, lásd: [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="31246-145">To quickly get started using Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>