---
title: "aaaNo felhasználók folyamatban van a kiépített tooan az Azure AD-gyűjtemény alkalmazás |} Microsoft Docs"
description: "Hogyan tootroubleshoot kapcsolatos gyakori hibák tapasztalt felhasználók jelennek meg nem jelenik meg az Azure AD egy gyűjtemény alkalmazás már konfigurálta a felhasználók átadása az Azure ad-vel"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: asteen
ms.openlocfilehash: 4d9693a202ed657e1de5571b50e5d499bef1bb3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="no-users-are-being-provisioned-tooan-azure-ad-gallery-application"></a>Egy felhasználó sem folyamatban van a kiépített tooan az Azure AD-gyűjtemény alkalmazás

Egyszer automatikus kiépítés konfigurálták az alkalmazáshoz (beleértve az ellenőrzése, hogy hello app megadott hitelesítő adatok, tooAzure AD tooconnect toohello app érvényesek). Majd a felhasználók és/vagy csoportok kiosztott toohello alkalmazást, és határozza meg a következő dolgot hello:

-   Amelyek felhasználókat és csoportokat **hozzárendelt** toohello alkalmazás. A hozzárendelés további információkért lásd: [hozzárendelése egy felhasználóhoz vagy csoporthoz tooan vállalati alkalmazást az Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   E **attribútum-hozzárendelések** az Azure AD toohello alkalmazásból engedélyezett és konfigurált toosync attribútum. Attribútum-leképezésekhez további információkért lásd: [testreszabása felhasználói kiépítés attribútum-leképezésekhez az SaaS-alkalmazásokhoz az Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Van-e egy **tartalmazó szűrő** jelen van, hogy felhasználók egyedi attribútum értékek alapján. A helyezése hatókörszűrőkkel további információkért lásd: [alkalmazások Attribútumalapú üzembe helyezése hatókörszűrőkkel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Tartják be, hogy felhasználók nem alatt törlődnek, ha további részleteket a hello vizsgálati naplóit Azure AD-ben, és keresse meg a naplóbejegyzések adott felhasználó.

kiépítés naplók hello hello hello Azure portálon érhetők el **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásnév\] &gt; vizsgálati naplókban**fülre. Szűrő hello bejelentkezik hello **fiók** kategória tooonly, tekintse meg az adott alkalmazáshoz események kiépítés hello. Felhasználók hello "egyező ID" attribútum-leképezésekhez hello számukra konfigurált alapján is kereshet. Például, ha hello "egyszerű felhasználónév" vagy "e-mail cím" hello Azure AD oldalon attribútum megfelelő hello, konfigurálta, és nem kiépítés hello felhasználó érték "audrey@contoso.com". Kereshet az hello a naplókban talál "audrey@contoso.com", és tekintse át ezt követően eredményt adott vissza.

kiépítés naplózási hello naplózza az összes hello hello létesítési szolgáltatás, így az Azure AD-hez hozzárendelt felhasználók hatókörébe kiépítés, azoknak a felhasználóknak hello megléte hello cél alkalmazás lekérdezése, hello összehasonlításával lekérdezése által végrehajtott műveletek rekord felhasználói objektumok hello rendszer között. Majd adja hozzá, frissíteni vagy hello felhasználói fiók letiltása hello célrendszerben hello összehasonlítás alapján.

## <a name="general-problem-areas-with-provisioning-tooconsider"></a>Általános problémás területek tooconsider szolgáltatáskiépítéssel

Az alábbiakban felsoroljuk a hello részletezhető Ha egy ötletet, ahol általános problématerületeket toostart.

* [Szolgáltatás kiépítését nem jelenik meg toostart](#provisioning-service-does-not-appear-to-start)
* [Naplók mondja ki a felhasználók figyelmen hagyja, és nincs telepítve, akkor is, ha hozzárendeli őket](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-toostart"></a>Szolgáltatás kiépítését nem jelenik meg toostart

Ha hello **kiépítési állapot** toobe **a** a hello **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásneve\] &gt;Kiépítési** hello Azure-portálon szakasza. Azonban nem más állapot részletei jelennek meg a lap után további Újratölti valószínű, hogy hello szolgáltatás fut, de a kezdeti szinkronizálás még nem fejeződött be. Ellenőrizze a hello **naplók** toodetermine a fent leírt milyen műveletek hello szolgáltatás működik-e, és ha hiba történik.

>[!NOTE]
>Egy kezdeti szinkronizálást is igénybe vehet 20 perc tooseveral óra, hello Azure AD-címtár és a felhasználók kialakítási hatókörében hello száma hello méretétől függően. Hello kezdeti szinkronizálás után az ezt követő szinkronizálások gyorsabb, mint hello szolgáltatás kiépítését tárolja, amelyek megfelelnek a hello állapot mindkét rendszer hello kezdeti szinkronizálás után vízjelek. Ez javítja ezt követő szinkronizálások teljesítményét.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Naplók mondja ki a felhasználók figyelmen hagyja, és nincs telepítve, akkor is, ha hozzárendeli őket

A felhasználó megjelenik, mint "kimarad a" hello naplókat, esetén nagyon fontos tooread hello kiterjesztett hello napló üzenet toodetermine hello OK részleteit. Az alábbiakban gyakori okok és megoldások:

-   **Hatókörként szűrő konfigurációja** **, amely van kiszűrése hello felhasználói egy attribútum-érték alapján**. A helyezése hatókörszűrőkkel további információkért lásd: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **"nem hatékony joga" Hello felhasználó.** Ha hibaüzenet jelenik meg, azért, mert probléma hello Azure AD-ben tárolt felhasználói hozzárendelési bejegyzés. toofix a probléma megszüntetése felhasználói (vagy a csoport) hello hello alkalmazásból, és újból rendelje hozzá újra. A hozzárendelés további információkért lásd: <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Egy kötelező attribútum hiányzik vagy nem ki van töltve egy felhasználó.** Ha kiépítés beállítása tooreview egy fontos tooconsider hello attribútum-leképezésekhez és munkafolyamatok, amelyek meghatározzák, milyen felhasználói (vagy a csoport) tulajdonságok folyamata az Azure AD toohello alkalmazásból és konfigurálása. Ez magában foglalja, hello "egyező tulajdonság" beállítása használt toouniquely kell azonosítani és felel meg a felhasználók/csoportok hello két rendszerek között. További információ a fontos folyamatban: [testreszabása felhasználói kiépítés attribútum-leképezésekhez az SaaS-alkalmazásokhoz az Azure Active Directoryban](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Azon csoportok attribútum:** hello telepítése csoport név és csoportosítási részleteket, továbbá toohello tagokat, ha egyes alkalmazások esetében támogatott. Engedélyezheti vagy letilthatja ezt a funkciót engedélyezésével vagy letiltásával hello **leképezési** a csoport objektumainak hello látható **kiépítési** fülre. Ha csoportok kiépítés engedélyezve van, ne feledje tooreview hello attribútum hozzárendelések tooensure egy megfelelő mező használatos "Azonosítóval egyező" hello. Ez lehet hello megjelenítési nevet vagy e-mail címét), hello csoportot és annak tagjait nem lehet biztosított Ha hello megfelelő tulajdonság értéke üres vagy nem ki van töltve a csoport az Azure ad-ben.

## <a name="next-steps"></a>Következő lépések
[Azure AD Connect szinkronizálása: Understanding deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md)

