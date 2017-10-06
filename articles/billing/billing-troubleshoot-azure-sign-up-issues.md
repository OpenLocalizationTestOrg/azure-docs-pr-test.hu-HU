---
title: "aaaTroubleshoot Azure regisztrációs problémák |} Microsoft Docs"
description: "Ismerteti, hogyan tootroubleshoot néhány gyakori Azure regisztráljon problémákat."
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
ms.openlocfilehash: ee649bfc3be7ba1fe2dd863fac09e1c2311d835b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-sign-up-issues-for-azure"></a>Regisztrációs problémák elhárítása az Azure-bA
Ha nem regisztrál Azure, hello tippek használható ez a cikk tootroubleshoot gyakori problémákat. Ha a probléma lehet a hitelkártya során az előfizetési, lásd: [a bankkártyával vagy hitelkártya utasít: Azure-előfizetési](billing-credit-card-fails-during-azure-sign-up.md). Ha Azure-fiókkal rendelkezik, de nem tud bejelentkezni, lásd: [I nem tud bejelentkezni, toomanage az Azure-előfizetésem](billing-cannot-login-subscription.md).

## <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Folyamatjelző sáv lefagy "Kártya által identitás ellenőrzése" szakaszában

toocomplete hello identitás-ellenőrzés kártya által, külső cookie-k engedélyezni kell a böngésző.

![Képernyőfelvétel a hello kártya szakaszhoz regisztráció során függő identitás-ellenőrzés](./media/billing-troubleshoot-azure-sign-up-issues/identity-verification-hangs.PNG)

A következő lépéseket tooupdate hello a böngésző cookie-beállításokat használja.

1. Ha Chrome használata esetén nyissa meg túl**beállítások** > **megjelenítése speciális beállítások** > **adatvédelmi** > **tartalom beállítások**. Törölje a jelet **külső cookie-k blokkolását, és a helyadatok**.
2. Ha Edge használata esetén nyissa meg túl**beállítások** > **nézet speciális beállítások** > **cookie-k**. Válassza ki **nem blokkolja a cookie-k**.
3. Frissítse a hello Azure-előfizetés lapján, és ellenőrizze a hello probléma megoldódott.
4. Ha hello frissítési nem hello probléma megoldására, lépjen ki, és indítsa újra a böngészőt, majd próbálkozzon újra.

## <a name="credit-card-form-doesnt-support-my-billing-address"></a>Hitelkártya űrlap nem támogatja a számlázási címét
A számlázási címe kell hello kiválasztott hello országban toobe **az Ön adatai** szakasz. Gondoskodjon róla, hogy hello megfelelő országot.

## <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Nem szöveges üzenetek vagy hívások során előfizetési cím ellenőrzése
Bár ez általában sokkal gyorsabb, ellenőrző kód toobe kézbesíteni toofour percet igénybe vehet. az ellenőrzéshez meg hello telefonszám hello fiók a telefonszám nem tárolja.

Az alábbiakban néhány további tipp:
* Egy VOIP telefonszámot hello phone ellenőrzési folyamat nem használható.
* Ellenőrizze hello telefonszám ad meg, beleértve hello legördülő menüből kiválasztott hello országhívószám.
* Ha a telefon nem fogad szöveges üzeneteket (SMS), próbálja meg hello **hívást kérek** lehetőséget.
* Győződjön meg arról, hogy a telefon fogadhat hívást vagy SMS-üzenetek alapján Egyesült államokbeli telefonszámok.

Amikor hello szöveges üzenetet vagy a telefonhívás, adja meg a kapott kódot hello szövegmezőbe.

## <a name="credit-card-declined-or-not-accepted"></a>Hitelkártya elutasította, vagy nem fogadható el.
Az Azure-előfizetések fizetési virtuális vagy előre vagy követel kártyák nem elfogadott. milyen hiba okozhatja a kártya toobe elutasítva, toosee lásd: [a bankkártyával vagy hitelkártya utasít: Azure-előfizetési](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="free-trial-is-not-available"></a>"Ingyenes nem érhető el"
Használta már Azure-előfizetéssel az elmúlt hello? hello Azure használati szerződés ingyenes próba aktiválás csak az új tooAzure olyan felhasználói korlátozza. Ha rendelkezik Azure-előfizetés semmilyen más típusú, nem aktiválható, egy ingyenes próbaverzióra. Fontolja meg a regisztrációt a [használatalapú fizetés előfizetés](https://azure.microsoft.com/offers/ms-azr-0003p/).

## <a name="i-saw-a-charge-on-my-free-trial-account"></a>A felhasználói fiókhoz ingyenes díj I látott
Láthatja, hogy egy kis ellenőrzési tartsa a hitelkártya-fiókjában, a regisztrációt követően, amely too5 3 nappal törlődnek. Ha tudja kapcsolatos költségek kezelésére, tudjon meg többet az [váratlan költségek megakadályozza](https://docs.microsoft.com/azure/billing/billing-getting-started).

## <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Nem lehet aktiválni az MSDN, BizSpark, BizSparkPlus vagy MPN Azure juttatás terv
Győződjön meg arról, hogy hello jobb bejelentkezés használata hitelesítő adatokat. Ellenőrizze a hello juttatás program toomake, hogy, hogy Ön jogosult. 

* MSDN
  * Ellenőrizze a jogosultsági állapotát a [MSDN fióklapját](https://msdn.microsoft.com/subscriptions/manage/default.aspx).
  * Ha az állapot nem ellenőrizhető, forduljon a hello [MSDN előfizetések ügyfélszolgálati Központjaink](https://msdn.microsoft.com/subscriptions/contactus.aspx)
* BizSpark
  * Jelentkezzen be toohello [BizSpark portal](https://www.microsoft.com/bizspark/default.aspx#start-two) és a jogosultsági állapotának ellenőrzése a BizSpark és BizSpark plusz.
  * Ha az állapot nem ellenőrizhető, akkor [segítség: hello BizSpark fórumok](http://aka.ms/bzforums).
* MPN
  * Jelentkezzen be toohello [MPN portal](https://mspartner.microsoft.com/en/us/Pages/Locale.aspx) és a jogosultsági állapotának ellenőrzése. Ha rendelkezik a megfelelő hello [felhő Platform szakértelem](https://mspartner.microsoft.com/en/us/pages/membership/cloud-platform-competency.aspx), előfordulhat, hogy jogosult a további előnyökkel is jár.
  * Ha az állapot nem ellenőrizhető, forduljon a [MPN támogatási](https://mspartner.microsoft.com/en/us/Pages/Support/Premium/contact-support.aspx).

## <a name="cant-activate-new-azure-in-open-subscription"></a>Új Megnyitás az Azure-előfizetés nem lehet aktiválni.
egy nyitott az Azure-előfizetés toocreate, rendelkeznie kell legalább egy Azure a nyitott token társított tooit érvényes Online szolgáltatás aktiválási (OSA) kulcsot. Ha nem rendelkezik egy OSA kulccsal, lépjen kapcsolatba Microsoft Partners egyik felsorolt [Microsoft Pinpoint](http://pinpoint.microsoft.com/).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.
