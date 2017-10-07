---
title: "az Active Directory jelentéskészítési késések aaaAzure |} Microsoft Docs"
description: "További tudnivalók: hello mennyi ideig tart a jelentési események tooshow mentése az Azure-portálon"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 9b88958d-94a2-4f4b-a18c-616f0617a24e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi;dhanyahk
ms.reviewer: dhanyahk
ms.openlocfilehash: eee959331262ba59b313dd038cb54699dbef48a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-latencies"></a>Azure Active Directory jelentéskészítés késések

A [reporting](active-directory-preview-explainer.md) hello Azure Active Directory, az beszerzése toodetermine hogyan működik a környezetében szükséges összes hello információkat. a jelentéskészítési adatok tooshow fel a hello Azure-portálon is várakozási időt hello mennyisége. 

Ez a témakör felsorolja hello késési adatok hello jelentéskészítési kategóriából hello Azure-portálon. 


## <a name="activity-reports"></a>Tevékenységjelentések

Nincsenek Tevékenységjelentés két terület:

- **Bejelentkezési tevékenységek** – kezelt alkalmazások és a felhasználói bejelentkezési tevékenységek hello használatával kapcsolatos információért
- **Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan

a következő táblázat hello hello késési adatok Tevékenységjelentések sorolja fel.

| Jelentés | Minimális | Átlagos | Maximális |
| :-- | --- | --- | --- |
| Naplók             | 30 perc  | 45 percben | 1 óra     |
| Bejelentkezések               | 15 perc  | 15 perc | 2 óra *   |

>[!NOTE]
> Néhány bejelentkezések tevékenységre vonatkozó adatok az örökölt office-alkalmazások érkező hello adatok tooshow végzi a jelentéseket a too8 óráig is eltarthat. 


## <a name="security-reports"></a>Biztonsági jelentések

Nincsenek biztonsági reporting két terület:

- **Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója. 
- **Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága. 

a következő táblázat hello hello késési adatok biztonsági jelentések sorolja fel.

| Jelentés | Minimális | Átlagos | Maximális |
| :-- | --- | --- | --- |
| Érintett felhasználók          | 5 perc   | 15 perc  | 2 óra  |
| Kockázatos bejelentkezések         | 5 perc   | 15 perc  | 2 óra  |

## <a name="risk-events"></a>Kockázati események

Az Azure Active Directory adaptív gépi tanulási a algoritmusok és heurisztikus toodetect gyanús műveleteket kapcsolódó tooyour felhasználói fiókokat használ. Minden észlelt gyanús művelet egy rekord hívott kockázat esemény van tárolva.

a következő táblázat hello hello késési adatok kockázati események sorolja fel.

| Jelentés | Minimális | Átlagos | Maximális |
| :-- | --- | --- | --- |
| Névtelen IP-címről történő bejelentkezések |5 perc |15 perc |2 óra |
| Ismeretlen helyekről történt bejelentkezések |5 perc |15 perc |2 óra |
| Felhasználók, akiknek kiszivárogtak a hitelesítő adatai |2 óra |4 óra |8 óra |
| Lehetetlen odautazás tooatypical helyek |5 perc |1 óra |8 óra  |
| Bejelentkezések fertőzött eszközökről |2 óra |4 óra |8 óra  |
| Bejelentkezések gyanús tevékenységeket mutató IP-címekkel |2 óra |4 óra |8 óra  |



## <a name="next-steps"></a>Következő lépések

Ha azt szeretné, hogy az Azure-portálon hello hello tevékenység jelentésekkel kapcsolatos további tooknow, lásd:

- [Bejelentkezési tevékenység jelentések hello Azure Active Directory portálon](active-directory-reporting-activity-sign-ins.md)
- [Naplózási Tevékenységjelentések hello Azure Active Directory portálon](active-directory-reporting-activity-audit-logs.md)

Ha azt szeretné, hogy az Azure-portálon hello hello biztonsági jelentésekkel kapcsolatos további tooknow, lásd:

- [A kockázat biztonsági jelentés hello Azure Active Directory portálon felhasználója](active-directory-reporting-security-user-at-risk.md)
- [Kockázatos bejelentkezések jelentés hello Azure Active Directory portálon](active-directory-reporting-security-risky-sign-ins.md)

Ha azt szeretné, hogy a kockázati eseményekről további tooknow, [Azure Active Directory kockázati események](active-directory-reporting-risk-events.md).
