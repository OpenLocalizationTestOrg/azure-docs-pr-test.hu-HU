---
title: "SMS, e-mailek és webhookokkal korlátozása aaaRate |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure korlátozza művelet csoportból lehetséges SMS, e-mailek vagy webhook értesítések hello száma."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 1cd08a5b982c82bb02e0bf93451aa1fcd9bc34af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a><span data-ttu-id="cb40a-103">Értékelje a bejegyzéseket az SMS-üzenetek, e-mailek és webhook korlátozása</span><span class="sxs-lookup"><span data-stu-id="cb40a-103">Rate limiting for SMS messages, emails, and webhook posts</span></span>
<span data-ttu-id="cb40a-104">Az értesítések, amelyek akkor fordul elő, ha túl sok értesítések küldése általában olyan tooa adott telefonszámát vagy e-mail címét felfüggesztés sebességkorlátozást.</span><span class="sxs-lookup"><span data-stu-id="cb40a-104">Rate limiting is a suspension of notifications that occurs when too many notifications are sent tooa particular phone number or email address.</span></span> <span data-ttu-id="cb40a-105">Sebességkorlátozást biztosítja, hogy riasztások kezelhető és hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="cb40a-105">Rate limiting ensures that alerts are manageable and actionable.</span></span>

<span data-ttu-id="cb40a-106">hello szabályok az SMS- és e-mailek: hello azonos.</span><span class="sxs-lookup"><span data-stu-id="cb40a-106">hello rules for SMS and email are hello same.</span></span> <span data-ttu-id="cb40a-107">hello arány korlát küszöbértéke:</span><span class="sxs-lookup"><span data-stu-id="cb40a-107">hello rate limit threshold is:</span></span>

 - <span data-ttu-id="cb40a-108">**SMS**: 10 üzenetek egy óra alatt.</span><span class="sxs-lookup"><span data-stu-id="cb40a-108">**SMS**: 10 messages in an hour.</span></span>
 - <span data-ttu-id="cb40a-109">**E-mailek**: 100 üzenetek egy óra alatt.</span><span class="sxs-lookup"><span data-stu-id="cb40a-109">**Email**: 100 messages in an hour.</span></span>

## <a name="rate-limit-rules"></a><span data-ttu-id="cb40a-110">Sebesség korlát szabályok</span><span class="sxs-lookup"><span data-stu-id="cb40a-110">Rate limit rules</span></span>
- <span data-ttu-id="cb40a-111">Egy adott telefonszámát vagy e-mail sebessége korlátozott, ha hello küszöbérték által engedélyezettnél több üzenetet kapott.</span><span class="sxs-lookup"><span data-stu-id="cb40a-111">A particular phone number or email is rate limited when it receives more messages than hello threshold allows.</span></span>
- <span data-ttu-id="cb40a-112">Telefonos vagy e-mailek sok előfizetésekhez művelet csoportok része lehet.</span><span class="sxs-lookup"><span data-stu-id="cb40a-112">A phone number or email can be part of action groups across many subscriptions.</span></span> <span data-ttu-id="cb40a-113">Minden előfizetésekhez sebességkorlátozást vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="cb40a-113">Rate limiting applies across all subscriptions.</span></span> <span data-ttu-id="cb40a-114">Amint hello küszöbérték elérésekor vonatkozik, akkor is, ha több előfizetéssel az üzenetküldés.</span><span class="sxs-lookup"><span data-stu-id="cb40a-114">It applies as soon as hello threshold is reached, even if messages are sent from multiple subscriptions.</span></span>  
- <span data-ttu-id="cb40a-115">Ha telefonos vagy e-mailek sebessége korlátozott, egy további értesítést küld toocommunicate hello sebességkorlátozást.</span><span class="sxs-lookup"><span data-stu-id="cb40a-115">When a phone number or email is rate limited, an additional notification is sent toocommunicate hello rate limiting.</span></span> <span data-ttu-id="cb40a-116">hello állapotokat, ha hello értékelje korlátozza az értesítési lejár.</span><span class="sxs-lookup"><span data-stu-id="cb40a-116">hello notification states when hello rate limiting expires.</span></span>

## <a name="rate-limit-of-webhooks"></a><span data-ttu-id="cb40a-117">A webhook sávszélesség-korlátjának</span><span class="sxs-lookup"><span data-stu-id="cb40a-117">Rate limit of webhooks</span></span> ##
<span data-ttu-id="cb40a-118">Nincs webhookok a helyen nincs sebességével.</span><span class="sxs-lookup"><span data-stu-id="cb40a-118">There is no rate limiting in place for webhooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb40a-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb40a-119">Next steps</span></span> ##
* <span data-ttu-id="cb40a-120">További információ [SMS riasztási viselkedés](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="cb40a-120">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>
* <span data-ttu-id="cb40a-121">Lekérése egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan tooreceive riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="cb40a-121">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span>  
* <span data-ttu-id="cb40a-122">Ismerje meg, hogyan túl[riasztások konfigurálása, ha az állapotfigyelő szolgáltatáshoz értesítést visszaküldi](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="cb40a-122">Learn how too[configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
