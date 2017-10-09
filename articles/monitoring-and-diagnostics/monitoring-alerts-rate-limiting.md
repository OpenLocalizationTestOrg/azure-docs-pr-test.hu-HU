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
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Értékelje a bejegyzéseket az SMS-üzenetek, e-mailek és webhook korlátozása
Az értesítések, amelyek akkor fordul elő, ha túl sok értesítések küldése általában olyan tooa adott telefonszámát vagy e-mail címét felfüggesztés sebességkorlátozást. Sebességkorlátozást biztosítja, hogy riasztások kezelhető és hajtható végre.

hello szabályok az SMS- és e-mailek: hello azonos. hello arány korlát küszöbértéke:

 - **SMS**: 10 üzenetek egy óra alatt.
 - **E-mailek**: 100 üzenetek egy óra alatt.

## <a name="rate-limit-rules"></a>Sebesség korlát szabályok
- Egy adott telefonszámát vagy e-mail sebessége korlátozott, ha hello küszöbérték által engedélyezettnél több üzenetet kapott.
- Telefonos vagy e-mailek sok előfizetésekhez művelet csoportok része lehet. Minden előfizetésekhez sebességkorlátozást vonatkozik. Amint hello küszöbérték elérésekor vonatkozik, akkor is, ha több előfizetéssel az üzenetküldés.  
- Ha telefonos vagy e-mailek sebessége korlátozott, egy további értesítést küld toocommunicate hello sebességkorlátozást. hello állapotokat, ha hello értékelje korlátozza az értesítési lejár.

## <a name="rate-limit-of-webhooks"></a>A webhook sávszélesség-korlátjának ##
Nincs webhookok a helyen nincs sebességével.

## <a name="next-steps"></a>Következő lépések ##
* További információ [SMS riasztási viselkedés](monitoring-sms-alert-behavior.md).
* Lekérése egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan tooreceive riasztásokat.  
* Ismerje meg, hogyan túl[riasztások konfigurálása, ha az állapotfigyelő szolgáltatáshoz értesítést visszaküldi](monitoring-activity-log-alerts-on-service-notifications.md).
