---
title: "felhasználók átadásához tooan az Azure AD-gyűjtemény alkalmazás konfigurálása aaaProblem |} Microsoft Docs"
description: "Hogyan tootroubleshoot kapcsolatos gyakori hibák tapasztalt felhasználók átadásához már szerepel a hello Azure AD Application Gallery tooan alkalmazás konfigurálása"
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
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 19b12be019b05faecef74c6f92a9de03b52a5ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-user-provisioning-tooan-azure-ad-gallery-application"></a>Probléma a felhasználók átadásához tooan az Azure AD-gyűjtemény alkalmazás konfigurálása

Konfigurálás [automatikus felhasználólétesítés](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) alkalmazás (ha támogatott), szükség van-e meghatározott utasításokat követi tooprepare hello alkalmazás automatikus üzembe helyezéséhez. Majd hello Azure portál tooconfigure hello kiépítése szolgáltatáshoz toosynchronize felhasználói fiókok toohello alkalmazás is használhatja.

Mindig el keresése hello beállítása útmutató adott toosetting be az alkalmazás telepítése. Hajtsa végre ezeket a lépéseket tooconfigure mindkét hello alkalmazást, és az Azure AD toocreate hello létesítési kapcsolat. Egy alkalmazás bemutatók felsorolása található [oktatóanyagok listáját hogyan tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list).

## <a name="how-toosee-if-provisioning-is-working"></a>Ha kiépítés toosee alakulását 

Ha konfigurálva van a hello szolgáltatást, hello művelet hello szolgáltatás legtöbb betekintést megállapítható két helyen:

-   **A naplók** – hello hello szolgáltatás kiépítését által végrehajtott összes hello operations logs naplórekordot átadása, beleértve az Azure ad-val lekérdezése hozzárendelt felhasználók hatókörébe történő üzembe helyezéséhez. Lekérdezés hello cél app azoknak a felhasználóknak, hello felhasználói objektumok között hello rendszer összehasonlításával hello meglétét. Majd adja hozzá, frissíteni vagy hello felhasználói fiók letiltása hello célrendszerben hello összehasonlítás alapján. kiépítés naplók hello hello hello Azure portálon érhetők el **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásnév\] &gt; vizsgálati naplókban**fülre. Szűrő hello bejelentkezik hello **fiók** kategória tooonly, tekintse meg az adott alkalmazáshoz események kiépítés hello.

-   **Előkészítési állapotát –** hello utolsó kiépítés összegzését futtatni egy adott alkalmazás látható a hello **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásnév\] &gt; Kiépítés** szakasz hello szolgáltatás beállításait az üdvözlő képernyőt hello alján. Ez a szakasz foglalja hány felhasználók (és/vagy csoportok) jelenleg folyamatban szinkronizálják egymást között hello két rendszer, és ha hiba történik. Hiba legutolsó részletes adatai hello naplók találhatók. Vegye figyelembe, hogy a létesítési állapot hello nem tölthető fel, amíg egy teljes kezdeti szinkronizálása befejeződött az Azure AD között és hello alkalmazást.

## <a name="general-problem-areas-with-provisioning-tooconsider"></a>Általános problémás területek tooconsider szolgáltatáskiépítéssel

Az alábbiakban felsoroljuk a hello részletezhető Ha egy ötletet, ahol általános problématerületeket toostart.

* [Szolgáltatás kiépítését nem jelenik meg toostart](#provisioning-service-does-not-appear-to-start)
* [Nem lehet menteni a konfigurációs miatt tooapp hitelesítő adatok nem működik](#can’t-save-configuration-due-to-app-credentials-not-working)
* [Naplók fel felhasználókat "kihagyva", és nincs telepítve, akkor is, ha hozzárendeli őket](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-toostart"></a>Szolgáltatás kiépítését nem jelenik meg toostart

Ha hello **kiépítési állapot** toobe **a** a hello **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásneve\] &gt;Kiépítési** hello Azure-portálon szakasza. Azonban nincs más állapot részletei jelennek meg, hogy a lap után további Újratölti. Valószínű, hogy hello szolgáltatás fut, de a kezdeti szinkronizálás még nem fejeződött be. Ellenőrizze a hello **naplók** toodetermine a fent leírt milyen műveletek hello szolgáltatás működik-e, és ha hiba történik.

>[!NOTE]
>Egy kezdeti szinkronizálást is igénybe vehet 20 perc tooseveral óra, hello Azure AD-címtár és a felhasználók kialakítási hatókörében hello száma hello méretétől függően. Ezt követő szinkronizálások hello kezdeti szinkronizálás után lehet gyorsabb, mint hello szolgáltatás kiépítését, amelyek megfelelnek a hello állapot mindkét rendszer hello a kezdeti szinkronizálás ezt követő szinkronizálások teljesítményének javítása után vízjelek tárolja.
>
>

## <a name="cant-save-configuration-due-tooapp-credentials-not-working"></a>Nem lehet menteni a konfigurációs miatt tooapp hitelesítő adatok nem működik

Ahhoz, hogy a létesítési toowork az Azure AD érvényes hitelesítő adatokat, amelyek lehetővé teszik az alkalmazás által biztosított API tooconnect tooa felhasználókezelés van szükség. Ha ezeket a hitelesítő adatokat nem működik, vagy nem tudja, Nyugat-afrikai vannak, tekintse át ezt az alkalmazást, a fentiekben ismertetett beállításának hello oktatóanyag.

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Naplók mondja ki a felhasználók figyelmen hagyja, és nincs telepítve, akkor is, ha hozzárendeli őket

A felhasználó megjelenik, mint "kimarad a" hello naplókat, esetén nagyon fontos tooread hello kiterjesztett hello napló üzenet toodetermine hello OK részleteit. Az alábbiakban gyakori okok és megoldások:

-   **Hatókörként szűrő konfigurációja** **, amely van kiszűrése hello felhasználói egy attribútum-érték alapján**. A helyezése hatókörszűrőkkel további információkért lásd: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **"nem hatékony joga" Hello felhasználó.** Ha hibaüzenet jelenik meg, azért, mert probléma hello Azure AD-ben tárolt felhasználói hozzárendelési bejegyzés. toofix a probléma megszüntetése felhasználói (vagy a csoport) hello hello alkalmazásból, és újból rendelje hozzá újra. A hozzárendelés további információkért lásd: <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Egy kötelező attribútum hiányzik vagy nem ki van töltve egy felhasználó.** Ha kiépítés beállítása tooreview egy fontos tooconsider hello attribútum-leképezésekhez és munkafolyamatok, amelyek meghatározzák, milyen felhasználói (vagy a csoport) tulajdonságok folyamata az Azure AD toohello alkalmazásból és konfigurálása. Ez magában foglalja, hello "egyező tulajdonság" beállítása használt toouniquely kell azonosítani és felel meg a felhasználók/csoportok hello két rendszerek között. További információ a fontos folyamatban: <https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings>.

   * **Azon csoportok attribútum:** hello telepítése csoport név és csoportosítási részleteket, továbbá toohello tagokat, ha egyes alkalmazások esetében támogatott. Engedélyezheti vagy letilthatja ezt a funkciót engedélyezésével vagy letiltásával hello **leképezési** a csoport objektumainak hello látható **kiépítési** fülre. Ha csoportok kiépítés engedélyezve van, ne feledje tooreview hello attribútum hozzárendelések tooensure egy megfelelő mező használatos "Azonosítóval egyező" hello. Ez lehet hello megjelenítési nevet vagy e-mail címét), hello csoportot és annak tagjait nem lehet biztosított Ha hello megfelelő tulajdonság értéke üres vagy nem ki van töltve a csoport az Azure ad-ben.

#<a name="next-steps"></a>Következő lépések
[Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához](active-directory-saas-app-provisioning.md)
