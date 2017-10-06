---
title: "aaaAzure Active Directory Reporting – gyakori kérdések |} Microsoft Docs"
description: "Az Azure Active Directory reporting gyakran ismételt kérdések."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: be65a05574ea3b5b190cd02a96b211c571ba70bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-faq"></a>Azure Active Directory jelentéskészítés – gyakori kérdések

A cikk a Microsoft Azure Active Directory reporting kapcsolatban felmerülő kérdések (GYIK) válaszok toofrequently tartalmaz.  
További részletekért lásd: [Azure Active Directory reporting](active-directory-reporting-azure-portal.md). 

**K: Mi hello adatok megőrzése (naplózási és bejelentkezések) tevékenységi naplóit hello Azure-portálon?** 

**V:** nyújtunk adatok 7 nap szabad ügyfeleink és tooan Azure AD Premium 1 vagy Premium 2 licenc váltásával, too30 nap fel az adatok eléréséhez. A megőrzési további részletekért lásd: [Azure Active Directory-jelentés adatmegőrzési szabályai](active-directory-reporting-retention.md)

--- 

**K: mennyi ideig tart amíg I a feladat befejezése után is látható hello tevékenységek adatai?**

**V:** tevékenység naplókat a 15 perc tooan óra közötti késéssel rendelkeznek. Bejelentkezési tevékenység naplók a legtöbb rekordok, és néhány rekordok too2 órában 15 perc közötti késéssel rendelkeznek.

---

**K: kell toobe hello Azure portál vagy tooget adatok hello API használatával bejelentkezik egy globális rendszergazdai toosee hello tevékenységet?**

**V.:** Nem. Akkor lehet egy **biztonsági olvasó**, egy **biztonsági rendszergazda** vagy egy **globális rendszergazda** toosee jelentéskészítéshez szükséges adatok az Azure portálon vagy keresztül hello API elérésével.

---

**K: kaphatok Office 365 műveletnapló adatai hello Azure-portálon keresztül?**

**V:** annak ellenére, hogy Office 365 tevékenységet és az Azure AD tevékenység naplók megosztási hello címtárerőforrásokkal, ha azt szeretné, amely teljes rálátást számos hello Office 365 tevékenységi naplóit, toohello Office 365 felügyeleti központban tooget Office 365 műveletnapló adatai kell lépjen.

---


**K: amelyek API-k használata Office 365 tevékenységi naplóit tooget információ?**

**V:** használata hello Office 365 felügyeleti API-k tooaccess hello [Office 365 tevékenység naplózza az API-n keresztül](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**K: hogyan sok rekord letölthető Azure-portálon?**

**V:** töltheti fel too120K bejegyzéseit hello Azure-portálon. hello rekordok alapján vannak rendezve *legutóbbi* és alapértelmezés szerint le hello legutóbbi 120-K rögzíti. 

---

**K: hogyan sok rekord tudja lekérdezni hello tevékenységek API keresztül?**

**V:** too1 millió rekordok kérdezhetők le (Ha nem adja meg hello top operátor, többsége hello rekord rendező legutóbbi). Ha hello "top" operátort használja, lekérheti too500K rekordok. Mintalekérdezések a módját a toouse hello API itt található [Itt](active-directory-reporting-api-getting-started.md).

---

**K: Hogyan tudom működtetni premium licenc?**

**V:** lásd [Ismerkedés az Azure Active Directory Premium](active-directory-get-started-premium.md) egy válasz toothis kérdés.

---

**K: hogyan hamarosan kell meg a tevékenységek adatok premium licenc beolvasása után?**

**V:** Ha szabad licenc tevékenységek az adatok már rendelkezik, akkor láthatja hello ugyanazokat az adatokat. Ha még nem rendelkezik az adatokat, majd tart egy vagy két napot.

---

**K: jelennek meg adatok előző hónap után egy Azure AD premium-licenc beolvasásakor?**

**V:** nemrég váltottunk tooa prémium verzió (beleértve a rendszer próbaverzióját), ha látható adatok mentése too7 nap kezdetben. Összesít adatokat, amikor látni fogja a másolatot too30 nap.

---

**K: van a kockázati események azonosító adatok védelmét, de nem látható az megfelelő bejelentkezés hello az összes bejelentkezéseket. Ez várható?**

**V:** Igen, Identity Protection kiértékeli minden hitelesítési forgalom kockázatát e ha interaktív vagy nem interaktív. Azonban minden bejelentkezések csak jelentésben csak hello interaktív bejelentkezést.

---

**K: hogyan töltheti hello Azure-portálon "Kockázat megjelölt felhasználók" jelentést?**

**V:** hello beállítás toodownload *felhasználók meg van jelölve, a kockázat* jelentés hamarosan megjelenik.

---

**K: Hogyan állapítható meg, hogy miért a bejelentkezés vagy a felhasználó lett megjelölve hello Azure-portálon a kockázatos?**

**V:** Premium edition ügyfelek talál további információt az alapul szolgáló kockázati események ehhez kattintson a "Felhasználók megjelölt kockázatot jelent a" hello felhasználó vagy a "Kockázatos sign-ins" hello kattintva hello. Szabad és alapvető edition ügyfelek le toosee hello at-risk felhasználók és a bejelentkezések nélkül hello az alapul szolgáló kockázati események adatait.

---

**K: hogyan kiszámítása hello bejelentkezéseket és kockázatos bejelentkezések jelentés IP-címek??**

**V:** IP-címek kiállított úgy, hogy nincs-e végleges kapcsolat közötti IP-cím és hello számítógép ezzel a címmel rendelkező fizikai helyét. Ez például a mobil szolgáltatók és a kiállító központi készletek IP-címek nagyon gyakran messze hol használják ténylegesen hello ügyféleszköz VPN tényezőktől bonyolítja. Megadott fenti hello, IP-cím tooa fizikai hely konvertálása a nyomkövetési adatokat, beállításjegyzék-adatok, fordított keresések és egyéb információk alapján a lehető legkedvezőbb módon. 

---
