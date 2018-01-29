---
title: "Az Azure API management-házirendet a minta - képességeket adhat a háttérszolgáltatáshoz |} Microsoft Docs"
description: "Az Azure API management házirend minta - képességeket adhat a háttérszolgáltatáshoz mutatja be. Például fogadja el a szélességi és hosszúsági a időjárási előrejelzés API helyett a hely nevét."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: d500df0f0e48134ac9c1397795d65706d2e56ff9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/04/2017
---
# <a name="add-capabilities-to-a-backend-service"></a>-Képességeket adhat a háttérszolgáltatáshoz

Ez a cikk bemutatja az Azure API management házirend minta, amely bemutatja, hogyan kell háttérszolgáltatás-képességeket adhat. Például fogadja el a szélességi és hosszúsági a időjárási előrejelzés API helyett a hely nevét. Beállíthatja vagy szerkesztheti a házirendet kódot, kövesse a témakörben ismertetett [beállítása és módosítása a házirend](../set-edit-policies.md). További példák, olvassa el [házirend minták](../policy-samples.md).

## <a name="policy"></a>Szabályzat

Illessze be a kódot a **bejövő** blokkot.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Következő lépések

További tudnivalók APIM házirendek:

+ [Átalakítási házirendek](../api-management-transformation-policies.md)
+ [Házirend-minták](../policy-samples.md)

