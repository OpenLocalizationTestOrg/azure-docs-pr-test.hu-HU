---
title: "az Active Directory-jelentés adatmegőrzési aaaAzure |} Microsoft Docs"
description: "A jelentés adatokat az Azure Active Directoryban adatmegőrzési"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c46a68198cb34e9c92662b2f8461010745392c04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Az Azure Active Directory-jelentés adatmegőrzési szabályai


Ez a témakör a válaszok toohello leggyakoribb kérdések hello különböző tevékenység jelentések az Azure Active Directoryban hello adatok megőrzése mellett. 

**K: hogyan férhetnek hello adatgyűjtés tevékenység elindult?**

**V:**

| Az Azure AD Edition | Gyűjtemény indítása |
| :--              | :--   |
| Prémium szintű Azure AD P1 <br /> Prémium szintű Azure AD P2 | Ha a regisztráláskor az előfizetéshez |
| Azure AD Free | hello első megnyitásakor hello [Azure Active Directory panel](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) , vagy használjon hello [reporting API-k](https://aka.ms/aadreports)  |

---
**K: mikor érhető el a tevékenységek adatai hello Azure-portálon?**

**V:**

- **Azonnal** – Ha már dolgozott jelentésekkel hello a klasszikus Azure portálon
- **2 órán belül** – Ha még nem bekapcsolta a jelentés a klasszikus Azure portálon hello

---
**K: hogyan férhetnek lépések biztonsági jelek hello gyűjtemény?**  

**V:** biztonsági jelek, az adatgyűjtési folyamat hello kezdődik, amikor Ön részt toouse hello Identity Protection-központtól. 


---
**K: mennyi ideig hello összegyűjtött adatok tárolják?**

**V:**

**Tevékenységjelentések**    

| Jelentés                 | Azure AD Free | Prémium szintű Azure AD P1 | Prémium szintű Azure AD P2 |
| :--                    | :--           | :--                 | :--                 |
| Címtárnaplózás        | 7 nap        | 30 nap             | 30 nap             |
| Bejelentkezési tevékenységek       | 7 nap        | 30 nap             | 30 nap             |

**Biztonsági jelek**

| Jelentés         | Azure AD Free | Prémium szintű Azure AD P1 | Prémium szintű Azure AD P2 |
| :--            | :--           | :--                 | :--                 |
| Érintett felhasználók  | 7 nap        | 30 nap             | 90 nap             |
| Kockázatos bejelentkezések | 7 nap        | 30 nap             | 90 nap             |

---