---
title: "Azure CDN erőforrások állapotának aaaMonitor hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor hello az Azure CDN-erőforrások az Azure Resource Health állapotát."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 0a77e56d2fecae4bde6c83730c05375853a6638a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hello-health-of-azure-cdn-resources"></a>Hello Azure CDN erőforrások állapotának figyelése
  
Az Azure CDN Resource health része a [Azure-erőforrás állapotának](../resource-health/resource-health-overview.md).  Azure-erőforrás állapotának toomonitor hello állapotát CDN erőforrásokat használ, és fogadni végrehajthatóként útmutatást tootroubleshoot problémák.

>[!IMPORTANT] 
>Az Azure CDN-erőforrás állapota jelenleg csak számlák globális CDN kézbesítési és API-képességek hello állapotát.  Az Azure CDN-erőforrás állapota nem ellenőrzi az egyéni CDN-végpontokat.
>
>hello azt jelzi, hogy az adatcsatorna-Azure CDN erőforrás állapota mentése késleltetett too15 perc lehet.

## <a name="how-toofind-azure-cdn-resource-health"></a>Hogyan toofind Azure CDN erőforrás állapota

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour CDN-profilt.

2. Kattintson a hello **beállítások** gombra.

    ![Beállítások gomb](./media/cdn-resource-health/cdn-profile-settings.png)

3. A *támogatási + hibaelhárítási*, kattintson a **erőforrás állapota**.

    ![CDN-erőforrás állapota](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>Hello CDN erőforrás is található *erőforrás állapota* hello csempére *súgó + támogatás* panelen.  Gyorsan kaphat túl*súgó + támogatás* körben hello kattintva **?** hello jobb felső sarkában található hello portál.
>
> ![Súgó és támogatás](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Az Azure CDN-specifikus üzenetek

Állapotok kapcsolódó tooAzure CDN erőforrás állapota alatt található.

|Üzenet | Javasolt művelet |
|---|---|
|Előfordulhat, hogy leállt, eltávolítva, vagy nincs megfelelően konfigurálva a CDN-végpontok közül legalább egyet | Előfordulhat, hogy leállt, eltávolítva, vagy nincs megfelelően konfigurálva a CDN-végpontok közül legalább egyet.|
|Sajnáljuk, a hello CDN felügyeleti szolgáltatás jelenleg nem érhető el | Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz.|
|Sajnáljuk, a CDN-végpontokat negatív hatással lehet a folyamatban lévő problémák egy részét a CDN-szolgáltatók | Térjen vissza ide az állapotfrissítések; Hogyan használja a hello kapcsolatos problémák elhárítása eszköz toolearn tootest a forrás és a CDN-végpont; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz. |
|Sajnáljuk, a CDN-végpont konfigurációs módosítások tapasztal terjesztési késedelmeket | Térjen vissza ide az állapotfrissítések; Ha a konfigurációs módosításokat a rendszer nem propagálja teljesen várt hello időben, forduljon a támogatási szolgálathoz.|
|Sajnáljuk, betöltése a kiegészítő portálon hello problémák léptek | Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz.|
Sajnáljuk, a CDN-szolgáltatók némelyike problémák léptek | Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz. |

## <a name="next-steps"></a>Következő lépések

- [Az Azure-erőforrás állapota – áttekintés](../resource-health/resource-health-overview.md)
- [CDN-tömörítés elhárítása](./cdn-troubleshoot-compression.md)
- [404-es hibák elhárítása](./cdn-troubleshoot-endpoint.md)