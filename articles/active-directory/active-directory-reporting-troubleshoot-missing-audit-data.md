---
title: "Hibaelhárítás: Hiányzó adatok az Azure Active Directory tevékenységnaplójában | Microsoft Docs"
description: "Ez a dokumentum az Azure Active Directoryban elérhető különféle jelentéseket sorolja fel"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 55587fb4ff6d8a2130eb838782a65788fb2dd2b3
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/16/2018
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>Nem találom az Azure Active Directory-tevékenységnaplóban a végrehajtott műveleteket


## <a name="symptoms"></a>Probléma

Végrehajtottam bizonyos műveleteket az Azure Portalon, és arra számítottam, hogy látom a műveletek naplóit a `Activity logs > Audit Logs` panelen, de nem találtam meg őket.

 ![Jelentéskészítés](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>Ok

A műveletek nem jelennek meg azonnal a tevékenységnaplóban. 15 perctől akár egy óráig is eltarthat, hogy a művelet végrehajtása után megjelenjenek a naplók a portálon.

## <a name="resolution"></a>Megoldás:

Várja ki 15 perc és egy óra közötti időtartamot, hogy lássa, megjelennek-e a műveletek a naplóban. Ha még mindig nem látja őket, hozzon létre egy támogatási jegyet, és megvizsgáljuk az ügyet.


## <a name="next-steps"></a>További lépések
További információ: [Jelentéskészítés az Azure Active Directoryban – gyakori kérdések](active-directory-reporting-faq.md).

