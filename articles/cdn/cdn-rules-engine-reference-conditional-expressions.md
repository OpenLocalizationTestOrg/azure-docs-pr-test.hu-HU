---
title: "aaaAzure CDN szabályok motor feltételes kifejezések |} Microsoft Docs"
description: "Az Azure CDN referenciadokumentációt szabályok motor egyezés feltételek és a szolgáltatások."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: rli
ms.openlocfilehash: 39d0754c34a577f77ca87b6fd92e2b6a9e4ff8fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cdn-rules-engine-conditional-expressions"></a>Az Azure CDN-szabályok a feltételes kifejezések motor
Ez a témakör részletesen ismerteti a feltételes kifejezések hello az Azure Content Delivery Network (CDN) [szabálymotor](cdn-rules-engine.md).

hello első szabály része hello feltételes kifejezés.

A feltételes kifejezés | Leírás
-----------------------|-------------
HA | Egy IF kifejezés mindig egy szabály következő utasításnak elsőként hello részét képezi. Mint minden más feltételes kifejezés ez IF utasítást kapcsolódó egyezést kell lennie. Ha nincsenek további feltételes kifejezések meghatározása a megfelelés, amelyeknek teljesülniük kell ahhoz funkciókat esetleg alkalmazott tooa kérelem hello feltételnek határozza meg.
HA | ÉS IF kifejezés csak a következő típusú feltételes kifejezések: Ha, és ha hello után adhatók hozzá. Azt jelzi, hogy van-e egy másik feltétel, amelyeknek teljesülniük kell hello kezdeti IF utasítást.
MÁSKÜLÖNBEN HA| MÁS IF kifejezés határozza meg a szolgáltatások adott toothis más IF utasítást készlete előtt teljesítendő alternatív feltételt. az ELSE IF utasítást hello jelenléte azt jelzi, hogy hello előző utasítás hello végéhez. hello csak a feltételes kifejezés, amely után az ELSE IF utasítást egy másik más IF utasítást kell elhelyezni. Ez azt jelenti, hogy az ELSE IF utasítást csak az használt toospecify egyetlen további feltétel teljesül toobe rendelkező lehet.

**Példa**: ![CDN feltétel felel meg.](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Egy későbbi szabály felülbírálhatják az előző szabályok által megadott hello műveletek. Példa: Általános szabály bérlőkulcshoz kapcsolódó összes kérelem jogkivonat-alapú hitelesítéssel. Egy másik szabály lehet létrehozni közvetlenül alatta toomake bizonyos típusú kérelmet egy kivételt.

### <a name="next-steps"></a>Következő lépések
* [Az Azure CDN áttekintése](cdn-overview.md)
* [Szabályok motor referencia](cdn-rules-engine-reference.md)
* [Szabályok motor egyezés feltételek](cdn-rules-engine-reference-match-conditions.md)
* [Szabályok adatbázismotor-szolgáltatások](cdn-rules-engine-reference-features.md)
* [Alapértelmezett HTTP működés használata hello szabályok felülbírálása](cdn-rules-engine.md)
