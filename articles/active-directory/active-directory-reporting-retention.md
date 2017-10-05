---
title: "Az Azure Active Directory-jelentés adatmegőrzési |} Microsoft Docs"
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
ms.openlocfilehash: ffeba8a6c32a21c75af21f948bbd6ea88dd9278c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-report-retention-policies"></a>Az Azure Active Directory-jelentés adatmegőrzési szabályai


Ez a témakör a kérdésekre adott válaszok a leggyakrabban használt a különböző tevékenység jelentések az Azure Active Directoryban az adatok megőrzése mellett. 

**K: hogyan férhetnek indítása tevékenység adatok gyűjtését?**

**V:**

| Az Azure AD Edition | Gyűjtemény indítása |
| :--              | :--   |
| Prémium szintű Azure AD P1 <br /> Prémium szintű Azure AD P2 | Ha a regisztráláskor az előfizetéshez |
| Azure AD Free | Az első megnyitásakor a [Azure Active Directory panel](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) , vagy használja a [reporting API-k](https://aka.ms/aadreports)  |

---
**K: mikor érhető el a tevékenységek adatai az Azure-portálon?**

**V:**

- **Azonnal** – Ha már dolgozott jelentéseket a klasszikus Azure portálon
- **2 órán belül** – Ha nem kapcsolta reporting klasszikus Azure portálon

---
**K: hogyan férhetnek a gyűjtemény biztonsági jelek lépések?**  

**V:** a biztonsági jelek, az adatgyűjtési folyamat kezdődik, amikor Ön részt a Identity Protection-központtól használja. 


---
**K: mennyi ideig az összegyűjtött adatok tárolják?**

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