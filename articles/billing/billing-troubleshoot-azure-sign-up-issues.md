---
title: "Az Azure regisztrációs problémák elhárítása |} Microsoft Docs"
description: "Néhány gyakori Azure bejelentkezési elhárítását ismerteti problémák össze."
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: a0907da1-cb2d-41d1-a97f-43841fabe355
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 647509ea36e487aca5db661adb3268e845988f78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Regisztrációs problémák elhárítása az Azure-bA
Ha Ön Azure nem lehet regisztrálni, használja a tippek ebben a cikkben kapcsolatos gyakori hibák elhárítása. Ha a probléma lehet a hitelkártya során az előfizetési, lásd: [a bankkártyával vagy hitelkártya utasít: Azure-előfizetési](billing-credit-card-fails-during-azure-sign-up.md). Ha Azure-fiókkal rendelkezik, de nem tud bejelentkezni, lásd: [nem lehet bejelentkezni a kezelése az Azure-előfizetésem](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Folyamatjelző sáv lefagy "Kártya által identitás ellenőrzése" szakaszában

Az identitás igazolásához kártya által, külső cookie-k a böngésző engedélyezni kell.

![Képernyőkép a regisztráció során függő kártya szakaszhoz identitás-ellenőrzés](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

Az alábbi lépések segítségével frissítse a böngésző cookie-k beállításairól.

1. Ha Chrome használata esetén folytassa a **beállítások** > **megjelenítése speciális beállítások** > **adatvédelmi** > **beállításai**. Törölje a jelet **külső cookie-k blokkolását, és a helyadatok**.
2. Ha Edge használata esetén folytassa a **beállítások** > **nézet speciális beállítások** > **cookie-k**. Válassza ki **nem blokkolja a cookie-k**.
3. Frissítse az Azure-előfizetési oldalt, és ellenőrizze, hogy ha a probléma megoldódott.
4. Ha a frissítés nem tudja megoldani a problémát, lépjen ki, és indítsa újra a böngészőt, és próbálkozzon újra.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Hitelkártya űrlap nem támogatja a számlázási címét
A számlázási címe kell lennie a kiválasztott országban a **az Ön adatai** szakasz. Gondoskodjon róla, hogy a megfelelő országot.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Nem szöveges üzenetek vagy hívások során előfizetési cím ellenőrzése
Bár ez általában sokkal gyorsabb, a megerősítési kódot kézbesítendő legfeljebb négy percig is tarthat. A telefonszám az ellenőrzéshez meg egy számot a fiók nem tárolja.

Az alábbiakban néhány további tipp:
* A telefonos hitelesítési eljárás nem használható egy VOIP telefonszámot.
* Ellenőrizze a telefonszámot ad meg, beleértve az országkódot a legördülő menüből kiválasztott.
* Ha a telefon nem fogad szöveges üzeneteket (SMS), próbálja meg a **hívást kérek** lehetőséget.
* Győződjön meg arról, hogy a telefon fogadhat hívást vagy SMS-üzenetek alapján Egyesült államokbeli telefonszámok.

Amikor az SMS-üzenet vagy a telefonhívás, meg, amely megjelenik a szövegmezőben.

## <a name="credit-card-declined-or-not-accepted"></a>Hitelkártya elutasította, vagy nem fogadható el.
Az Azure-előfizetések fizetési virtuális vagy előre vagy követel kártyák nem elfogadott. Milyen hiba lehet elutasította a kártya okozhat, olvassa el [a bankkártyával vagy hitelkártya utasít: Azure-előfizetési](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>"Ingyenes nem érhető el"
Használta már a Azure-előfizetéssel az elmúlt? A használati feltételeket Azure szerződés ingyenes próba aktiválás csak az olyan új Azure felhasználói korlátozza. Ha rendelkezik Azure-előfizetés semmilyen más típusú, nem aktiválható, egy ingyenes próbaverzióra. Fontolja meg a regisztrációt a [használatalapú fizetés előfizetés](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>A felhasználói fiókhoz ingyenes díj I látott
Láthatja, hogy egy kis ellenőrzési tartsa a hitelkártya-fiókjában, a regisztrációt követően, amely 3-5 napon belül törlődnek. Ha tudja kapcsolatos költségek kezelésére, tudjon meg többet az [váratlan költségek megakadályozza](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Nem lehet aktiválni az MSDN, BizSpark, BizSparkPlus vagy MPN Azure juttatás terv
Győződjön meg arról, hogy használja a jobb oldali bejelentkezési hitelesítő adataival. Ellenőrizze a juttatás program győződjön meg arról, hogy Ön jogosult. 

* MSDN
  * Ellenőrizze a jogosultsági állapotát a [MSDN fióklapját](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Ha az állapot nem ellenőrizhető, lépjen kapcsolatba a [MSDN előfizetések ügyfélszolgálati Központjaink](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Jelentkezzen be a [BizSpark portal](https://www.microsoft.com/bizspark/default.aspx#start-two) és a jogosultsági állapotának ellenőrzése a BizSpark és BizSpark plusz.
  * Ha az állapot nem ellenőrizhető, akkor [segítség, a BizSpark fórumok](http://aka.ms/bzforums).
* MPN
  * Jelentkezzen be a [MPN portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) és a jogosultsági állapotának ellenőrzése. Ha rendelkezik a megfelelő [felhő Platform szakértelem](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), előfordulhat, hogy jogosult a további előnyökkel is jár.
  * Ha az állapot nem ellenőrizhető, forduljon a [MPN támogatási](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Új Megnyitás az Azure-előfizetés nem lehet aktiválni.
Egy megnyitott az Azure-előfizetés létrehozása érvényes Online szolgáltatás aktiválási (OSA) kulcs rendelkeznie kell legalább egy Azure a nyitott jogkivonatok társítva. Ha nem rendelkezik egy OSA kulccsal, lépjen kapcsolatba Microsoft Partners egyik felsorolt [Microsoft Pinpoint](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.
