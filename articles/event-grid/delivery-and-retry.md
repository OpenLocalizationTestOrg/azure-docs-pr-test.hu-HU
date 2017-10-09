---
title: "aaaAzure esemény rács kézbesítési, és próbálkozzon újra"
description: "Ismerteti, hogyan nyújt az Azure esemény rács a eseményeket, és hogyan kezeli az kézbesítetlen üzenetek."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/11/2017
ms.author: darosa
ms.openlocfilehash: 874b3bf8892fbf803ef40f29d0ec10eb50150916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-grid-message-delivery-and-retry"></a><span data-ttu-id="0a33d-103">Esemény rács üzenetkézbesítést, és próbálkozzon újra</span><span class="sxs-lookup"><span data-stu-id="0a33d-103">Event Grid message delivery and retry</span></span> 

<span data-ttu-id="0a33d-104">Ez a cikk ismerteti, miként kezeli az Azure esemény rács a kézbesítési nem vonatkozik, az eseményeket.</span><span class="sxs-lookup"><span data-stu-id="0a33d-104">This article describes how Azure Event Grid handles events when delivery is not acknowledged.</span></span>

<span data-ttu-id="0a33d-105">Esemény rács tartós kézbesítési biztosít.</span><span class="sxs-lookup"><span data-stu-id="0a33d-105">Event Grid provides durable delivery.</span></span> <span data-ttu-id="0a33d-106">Minden üzenet legalább egyszer az egyes előfizetésekhez biztosítja.</span><span class="sxs-lookup"><span data-stu-id="0a33d-106">It delivers each message at least once for each subscription.</span></span> <span data-ttu-id="0a33d-107">Események azonnal elküldi a rendszer minden egyes előfizetés regisztrálva toohello webhook.</span><span class="sxs-lookup"><span data-stu-id="0a33d-107">Events are sent toohello registered webhook of each subscription immediately.</span></span> <span data-ttu-id="0a33d-108">Ha egy webhook nem átvételét esemény hello első szállítási 60 másodpercen belül kísérlet, esemény rács újrapróbálja hello esemény kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="0a33d-108">If a webhook does not acknowledge receipt of an event within 60 seconds of hello first delivery attempt, Event Grid retries delivery of hello event.</span></span>

## <a name="message-delivery-status"></a><span data-ttu-id="0a33d-109">Üzenet kézbesítési állapota</span><span class="sxs-lookup"><span data-stu-id="0a33d-109">Message delivery status</span></span>

<span data-ttu-id="0a33d-110">Esemény rács használja az események HTTP válasz kódok tooacknowledge fogadását.</span><span class="sxs-lookup"><span data-stu-id="0a33d-110">Event Grid uses HTTP response codes tooacknowledge receipt of events.</span></span> 

### <a name="success-codes"></a><span data-ttu-id="0a33d-111">Sikeres kódok</span><span class="sxs-lookup"><span data-stu-id="0a33d-111">Success codes</span></span>

<span data-ttu-id="0a33d-112">hello következő HTTP-válaszkódot azt jelzi, hogy az esemény szállított sikeresen tooyour webhook.</span><span class="sxs-lookup"><span data-stu-id="0a33d-112">hello following HTTP response codes indicate that an event has been delivered successfully tooyour webhook.</span></span> <span data-ttu-id="0a33d-113">Esemény rács úgy ítéli meg kézbesítési befejeződött.</span><span class="sxs-lookup"><span data-stu-id="0a33d-113">Event Grid considers delivery complete.</span></span>

- <span data-ttu-id="0a33d-114">200 OK</span><span class="sxs-lookup"><span data-stu-id="0a33d-114">200 OK</span></span>
- <span data-ttu-id="0a33d-115">202 elfogadott</span><span class="sxs-lookup"><span data-stu-id="0a33d-115">202 Accepted</span></span>

### <a name="failure-codes"></a><span data-ttu-id="0a33d-116">Hibát kódok</span><span class="sxs-lookup"><span data-stu-id="0a33d-116">Failure codes</span></span>

<span data-ttu-id="0a33d-117">a következő HTTP-válaszkódot hello azt jelzi, hogy az esemény kézbesítési tett kísérlet meghiúsult.</span><span class="sxs-lookup"><span data-stu-id="0a33d-117">hello following HTTP response codes indicate that an event delivery attempt failed.</span></span> <span data-ttu-id="0a33d-118">Esemény rács újrapróbálkozik toosend hello esemény.</span><span class="sxs-lookup"><span data-stu-id="0a33d-118">Event Grid tries again toosend hello event.</span></span> 

- <span data-ttu-id="0a33d-119">400 Hibás kérés</span><span class="sxs-lookup"><span data-stu-id="0a33d-119">400 Bad Request</span></span>
- <span data-ttu-id="0a33d-120">401 nem engedélyezett</span><span class="sxs-lookup"><span data-stu-id="0a33d-120">401 Unauthorized</span></span>
- <span data-ttu-id="0a33d-121">404 – Nem található</span><span class="sxs-lookup"><span data-stu-id="0a33d-121">404 Not Found</span></span>
- <span data-ttu-id="0a33d-122">408 kérelmi időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="0a33d-122">408 Request timeout</span></span>
- <span data-ttu-id="0a33d-123">414 URI túl hosszú</span><span class="sxs-lookup"><span data-stu-id="0a33d-123">414 URI Too Long</span></span>
- <span data-ttu-id="0a33d-124">500 belső kiszolgálóhiba</span><span class="sxs-lookup"><span data-stu-id="0a33d-124">500 Internal Server Error</span></span>
- <span data-ttu-id="0a33d-125">503-as szolgáltatás nem érhető el</span><span class="sxs-lookup"><span data-stu-id="0a33d-125">503 Service Unavailable</span></span>
- <span data-ttu-id="0a33d-126">504-es számú átjáró időtúllépése</span><span class="sxs-lookup"><span data-stu-id="0a33d-126">504 Gateway Timeout</span></span>

<span data-ttu-id="0a33d-127">Bármely más válaszkód vagy hiányoznak a választ egy hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="0a33d-127">Any other response code or a lack of a response indicates a failure.</span></span> <span data-ttu-id="0a33d-128">Esemény rács kézbesítési próbálkozások.</span><span class="sxs-lookup"><span data-stu-id="0a33d-128">Event Grid retries delivery.</span></span> 

## <a name="retry-intervals"></a><span data-ttu-id="0a33d-129">Újrapróbálkozási időközök</span><span class="sxs-lookup"><span data-stu-id="0a33d-129">Retry intervals</span></span>

<span data-ttu-id="0a33d-130">Esemény rács az exponenciális leállítási újrapróbálkozási házirend esemény kézbesítési használ.</span><span class="sxs-lookup"><span data-stu-id="0a33d-130">Event Grid uses an exponential backoff retry policy for event delivery.</span></span> <span data-ttu-id="0a33d-131">A webhook nem válaszol vagy hibakódot ad vissza, ha az esemény rács újrapróbálkozik a következő ütemezés hello kézbesítési:</span><span class="sxs-lookup"><span data-stu-id="0a33d-131">If your webhook does not respond or returns a failure code, Event Grid retries delivery on hello following schedule:</span></span>

1. <span data-ttu-id="0a33d-132">10 másodperc</span><span class="sxs-lookup"><span data-stu-id="0a33d-132">10 seconds</span></span>
2. <span data-ttu-id="0a33d-133">30 másodperc</span><span class="sxs-lookup"><span data-stu-id="0a33d-133">30 seconds</span></span>
3. <span data-ttu-id="0a33d-134">1 perc</span><span class="sxs-lookup"><span data-stu-id="0a33d-134">1 minute</span></span>
4. <span data-ttu-id="0a33d-135">5 perc</span><span class="sxs-lookup"><span data-stu-id="0a33d-135">5 minutes</span></span>
5. <span data-ttu-id="0a33d-136">10 perc</span><span class="sxs-lookup"><span data-stu-id="0a33d-136">10 minutes</span></span>
6. <span data-ttu-id="0a33d-137">30 perc</span><span class="sxs-lookup"><span data-stu-id="0a33d-137">30 minutes</span></span>
7. <span data-ttu-id="0a33d-138">1 óra</span><span class="sxs-lookup"><span data-stu-id="0a33d-138">1 hour</span></span>

<span data-ttu-id="0a33d-139">Esemény rács hozzáadja a kisméretű véletlenszerű tooall újrapróbálkozási időközönként.</span><span class="sxs-lookup"><span data-stu-id="0a33d-139">Event Grid adds a small randomization tooall retry intervals.</span></span>

## <a name="retry-duration"></a><span data-ttu-id="0a33d-140">Ismételje meg az időtartama</span><span class="sxs-lookup"><span data-stu-id="0a33d-140">Retry duration</span></span>

<span data-ttu-id="0a33d-141">Hello előzetes Azure esemény rács összes eseményt, amely nem érkeznek meg két órán belül lejár.</span><span class="sxs-lookup"><span data-stu-id="0a33d-141">During hello preview, Azure Event Grid expires all events that are not delivered within two hours.</span></span> <span data-ttu-id="0a33d-142">Általánosan rendelkezésre álló verzió előtt a most nagyobb too24 óra lesz.</span><span class="sxs-lookup"><span data-stu-id="0a33d-142">Before General Availability, this time will be increased too24 hours.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0a33d-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a33d-143">Next steps</span></span>

* <span data-ttu-id="0a33d-144">Egy bevezető tooEvent rács, lásd: [esemény rács](overview.md).</span><span class="sxs-lookup"><span data-stu-id="0a33d-144">For an introduction tooEvent Grid, see [About Event Grid](overview.md).</span></span>
* <span data-ttu-id="0a33d-145">tooquickly esemény rács használatának megkezdésében című [Azure esemény rácshoz hozza létre és útvonal egyéni események](custom-event-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="0a33d-145">tooquickly get started using Event Grid, see [Create and route custom events with Azure Event Grid](custom-event-quickstart.md).</span></span>
