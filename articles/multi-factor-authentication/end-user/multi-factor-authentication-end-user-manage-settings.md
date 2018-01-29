---
title: "A kétlépéses ellenőrzés beállításait kezelése |} Microsoft Docs"
description: "Kezelése, beleértve a kapcsolattartási adatok módosítása vagy az eszközök konfigurálása Azure multi-factor Authentication használatát."
services: multi-factor-authentication
keywords: "többtényezős hitelesítés ügyfél, hitelesítési probléma korrelációs azonosító"
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 8d84574283aa0c94ce303b0a7e3bde335c0eb2b8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a>A kétlépéses ellenőrzést beállításainak kezelése
Ez a cikk a kétlépéses ellenőrzés vagy többtényezős hitelesítési beállításainak frissítése kapcsolatos kérdésekre. Ha a fiókkal történő bejelentkezés problémát tapasztal, tekintse meg a [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md) hibaelhárítási segítséget.

## <a name="where-to-find-the-settings-page"></a>Hol található a beállítások lap
Attól függően, hogy vállalata hogyan Azure multi-factor Authentication beállítása van néhány olyan helyen, ahol módosíthatók a beállítások például a telefonszámát.

Ha a vállalat támogatja küldött egy adott URL-címet vagy lépéseket kezelése a kétlépéses ellenőrzést, kövesse ezeket az utasításokat. Ellenkező esetben az alábbi utasításokat a Mindenki más kell működnie. Ha kövesse az alábbi lépéseket, de nem jelenik meg a ugyanazokkal a beállításokkal, ez azt jelenti, hogy a munkahelyi vagy iskolai testreszabott saját portálon. Kérje meg a rendszergazdát az Azure multi-factor Authentication portálon mutató hivatkozás.

1. Jelentkezzen be [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Jelölje ki a fiók nevét a jobb felső sarkában, majd válasszon **profil**.  
3. Válassza ki **további biztonsági ellenőrzés**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. A további biztonsági ellenőrzési lapot betölti a beállításokkal.

    ![Proofup](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Szeretném a telefonszám módosítása, vagy egy másodlagos szám hozzáadása
Fontos konfigurálása egy másodlagos hitelesítéshez használni kívánt telefonszámot.  Mivel az elsődleges telefonszám és a mobilalkalmazás valószínűleg ugyanazt a telefonján, a másodlagos telefonszám nem visszaszerezni a fiókot, ha telefonja elveszett vagy ellopták lesz az egyetlen lehetőség.

> [!NOTE]
> Ha nem fér hozzá az elsődleges telefonszám, és nem a fiókjához első segítségre van szüksége, tekintse meg a súgótémakörök [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md).  

**Az elsődleges telefonszám módosítása:**  

1. A további biztonsági ellenőrzési lapot jelölje ki a szövegmezőben az aktuális telefonszámmal rendelkező és szerkesztheti az új telefonszámot.  
2. Kattintson a **Mentés** gombra.  
3. Ha ez a szám, amelyet a kedvenc hitelesítési lehetőségnek használja, akkor ellenőrizze az új szám előtt mentse azt.  

**Egy másodlagos telefonszámot hozzáadása:**  

1. A további biztonsági ellenőrzési lapon jelölje be a jelölőnégyzetet a **másik hitelesítési phone.**  
2. A mezőben adja meg a másodlagos telefonszámát.  
3. Válassza ki **mentése** és a módosítások befejezte volna.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>A megbízhatóként megjelölt eszközön újra kétlépéses ellenőrzés megkövetelése

A szervezet beállításoktól függően előfordulhat, hogy a jelölőnégyzet, amely szerint "Ne jelenjen meg többé a **X** nap" a böngésző kétlépéses ellenőrzés végrehajtásakor. Ha ezt a jelölőnégyzetet és majd az eszköz elvesztése vagy gondolja, hogy a fiók biztonsága sérül, az összes eszközre kell visszaállítani a kétlépéses ellenőrzést.

1. A további biztonsági ellenőrzési lapot, jelölje ki **visszaállítási többtényezős hitelesítést a korábban megbízhatónak minősített eszközökön**.
2. A következő bejelentkezéskor bármilyen eszközön, a rendszer kéri a kétlépéses ellenőrzés.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Hogyan Microsoft Authenticator tisztítása a régi eszközről és helyezze át egy újat?
Távolítsa el az alkalmazást az eszközről, vagy állítsa alaphelyzetbe az eszközt, ha nem távolítja el az aktiválást a háttérben. További információkért lásd: [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Következő lépések
* Hibaelhárítási tippeket és súgó [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md)
* Állítson be [alkalmazásjelszók](multi-factor-authentication-end-user-app-passwords.md) alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést.
