---
title: "aaaManage a kétlépéses ellenőrzés beállításait |} Microsoft Docs"
description: "Kezelése, beleértve a kapcsolattartási adatok módosítása vagy az eszközök konfigurálása Azure multi-factor Authentication használatát."
services: multi-factor-authentication
keywords: "többtényezős hitelesítés ügyfél, hitelesítési probléma korrelációs azonosító"
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 2c974b08c584943f3c5a6b9bf16497d1706e8329
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a>A kétlépéses ellenőrzést beállításainak kezelése
Ez a cikk kapcsolatos kérdésekre ad választ tooupdate beállítások kétlépéses ellenőrzés vagy a multi-factor Authentication hitelesítéshez. Ha bejelentkezik tooyour fiók problémát tapasztal, tekintse meg a túl[problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md) hibaelhárítási segítséget.

## <a name="where-toofind-hello-settings-page"></a>Ha toofind hello beállítások lap
Attól függően, hogy vállalata hogyan Azure multi-factor Authentication beállítása van néhány olyan helyen, ahol módosíthatók a beállítások például a telefonszámát.

Ha a vállalat informatikai rendszergazdája egy adott URL-cím vagy lépéseket toomanage kétlépéses ellenőrzés kiküldött, kövesse ezeket az utasításokat. Ellenkező esetben az utasításoknak hello a Mindenki más kell működnie. Ha kövesse az alábbi lépéseket, de nem jelenik meg hello ugyanazokkal a beállításokkal, amely azt jelenti, hogy a munkahelyi vagy iskolai testreszabott saját portálon. Kérje meg a rendszergazdát a hello hivatkozás tooyour Azure multi-factor Authentication portálon.

1. Jelentkezzen be a túl[https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Hello jobb felső jelölje be a fiók nevére, majd válassza ki **profil**.  
3. Válassza ki **további biztonsági ellenőrzés**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. hello további biztonsági ellenőrzési lapot betölti a beállításokkal.

    ![Proofup](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-toochange-my-phone-number-or-add-a-secondary-number"></a>Toochange telefonszámmal, és nem sikerül egy másodlagos szám hozzáadása
Fontos tooconfigure egy másodlagos hitelesítéshez használni kívánt telefonszámot is.  Mivel az elsődleges telefonszám száma és a mobilalkalmazás vannak valószínűleg a hello azonos telefon, hello másodlagos telefonszám hello csak úgy fogja tudni tooget visszaállítása fiókjába Ha telefonja elveszett vagy ellopták.

> [!NOTE]
> Ha nem rendelkezik hozzáférési tooyour elsődleges telefonszámot, és tooyour fiók első segítségre van szüksége, tekintse meg a súgótémakörök [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md).  

**toochange az elsődleges telefonszám:**  

1. Hello további biztonsági ellenőrzési lapot jelölje ki a hello szövegmezőbe az aktuális telefonszámát és szerkesztheti az új telefonszámot.  
2. Kattintson a **Mentés** gombra.  
3. Ez a kedvenc hitelesítési lehetőségnek a használt hello számát, lehetősége van tooverify hello új szám előtt mentse azt.  

**egy másodlagos telefonszámot tooadd:**  

1. Az oldalon hello további biztonsági ellenőrzési jelölőnégyzetet hello mellett túl**másik hitelesítési phone.**  
2. Hello mezőben adja meg a másodlagos telefonszámát.  
3. Válassza ki **mentése** és a módosítások befejezte volna.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>A megbízhatóként megjelölt eszközön újra kétlépéses ellenőrzés megkövetelése

A szervezet beállításoktól függően előfordulhat, hogy a jelölőnégyzet, amely szerint "Ne jelenjen meg többé a **X** nap" a böngésző kétlépéses ellenőrzés végrehajtásakor. Ha ezt a jelölőnégyzetet és majd az eszköz elvesztése vagy gondolja, hogy a fiók biztonsága sérül, az eszközök kétlépéses ellenőrzés tooall kell visszaállítani. 

1. Hello további biztonsági ellenőrzési lapot, jelölje ki **visszaállítási többtényezős hitelesítést a korábban megbízhatónak minősített eszközökön**.
2. hello bejelentkezik bármilyen eszközön, amikor legközelebb lesz felszólító tooperform kétlépéses ellenőrzést. 

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-tooa-new-one"></a>Hogyan Microsoft Authenticator tisztítása a régi eszközről és helyezze át a tooa újat?
Hello alkalmazást az eszközről, vagy alaphelyzetbe állítása hello eszköz eltávolításakor hello aktiválási hello háttérben futó nem távolítja el. További információkért lásd: [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Következő lépések
* Hibaelhárítási tippeket és súgó [problémák adódtak a kétlépéses ellenőrzéshez használttal](multi-factor-authentication-end-user-troubleshoot.md)
* Állítson be [alkalmazásjelszók](multi-factor-authentication-end-user-app-passwords.md) alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést.
