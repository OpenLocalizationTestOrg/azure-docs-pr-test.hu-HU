---
title: "bejelentkezés – phone aaaMicrosoft hitelesítő Azure és a Microsoft-fiókok |} Microsoft Docs"
description: "Használja a telefon toosign tooyour Microsoft-fiók helyett adja meg a jelszót. Ebben a cikkben megválaszolunk – gyakori kérdések a szolgáltatásról."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: a4911ff580b3ffa078299ad706d099330b75a2e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Jelentkezzen be a telefon, nem a jelszót

hello Microsoft Authenticator alkalmazás segít a fiókok biztonsága érdekében végezzen kétlépéses ellenőrzést, a jelszó megadása után. De tudja, hogy azt lecserélheti személyes Microsoft-fiókja jelszavát hello teljesen? 

Ez a szolgáltatás elérhető az iOS és Android-eszközök és személyes Microsoft-fiókkal rendelkező működik. 

## <a name="how-it-works"></a>Működés

Sokan hello Microsoft Authenticator alkalmazás használata a kétlépéses ellenőrzéshez tooyour Microsoft-fiókkal való bejelentkezést követően. Írja be a jelszavát, majd nyissa meg toohello app tooeither értesítés jóváhagyása vagy ellenőrző kód kérése. A telefon bejelentkezhet hagyja ki a hello jelszót, és tegye meg a személyazonosság igazolását a telefonján. Telefonos bejelentkezés is egy kétlépéses ellenőrzést, továbbra is van szüksége egy dolog tudja tooprovide és egy annyi teendője van, tooverify személyazonosságát. hello phone még mindig hello annyi teendője van, és a telefon PIN-kódot vagy biometrikus kulcs hello ismerjük. 

## <a name="how-tooget-started"></a>Hogyan tooget el

toosign tooyour személyes Microsoft-fiók a telefonja, kövesse az alábbi lépéseket: 

1. Engedélyezze a telefon bejelentkezhet a fiókjához. 

  - Ha nincs hello Microsoft Authenticator alkalmazást, telepítse, és adja hozzá a személyes Microsoft-fiókját a hello toohello lépések szerint [Microsoft Authenticator lap](microsoft-authenticator-app-how-to.md). Újonnan hozzáadott fiókok automatikusan engedélyezett, ezért most jó toogo.

  - Ha már használja a Microsoft Authenticator a kétlépéses ellenőrzéshez, a fiók hello alkalmazás kezdőlapja oldalon válassza ki és **telefonos bejelentkezés engedélyezése** hello legördülő menüből.

  >[!NOTE] 
  >tooprotect a fiókjához, kérjük, a PIN-kódot vagy biometrikus zárolást az eszközön. Ha a telefon zárolása feloldva, hello alkalmazás jelenik meg, egy kérelem kéri fel a zárolást tooset telefonos bejelentkezés engedélyezése előtt. 

3. Legtöbb lap, ahol általában írja be a Microsoft-fiókja jelszavát rendelkezik egy hivatkozást, amely szerint **alkalmazást használhat a megnyitáshoz**. Válassza ki a hivatkozás toosign be a telefonjára. 

4. Microsoft értesítési tooyour telefon küld. Hagyja jóvá a hello értesítési toosign tooyour fiók.   

## <a name="faq"></a>GYIK 

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Hogyan van bejelentkezni a biztonságosabb, mint a jelszó beírása telefonomat?  

Ma a legtöbben tooweb webhelyek vagy alkalmazások felhasználónévvel és jelszóval bejelentkezni.  Sajnos jelszavak gyakran elvesztése, ellopása, vagy kitalálni a támadók által. Hello Microsoft Authenticator alkalmazás toosign a beállításakor azt oldhatja fel a fiók telefonján kulcs létrehozása. Azt védeni, ezt a kulcsot a hello PIN-kódot vagy biometrikus már használható a telefonján.  A telefon jelentkezik, ha ezt a kulcsot nem használt, biztonságos és két személyazonosságát tényezők – hello tooprove phone magát, és a képes toounlock azt. 

hello használt a rendszer a Windows Hello és hello FIDO Alliance UAF specifikációk hasonló toohello kulcsok. Az adatok csak akkor életrajza tooprotect hello kulcs használt helyileg, és soha nem küldött, vagy felhőben tárolt, hello. 
 
### <a name="where-can-i-use-my-phone-tooreplace-my-password-and-where-would-i-still-need-hello-password"></a>Ha használható a telefon tooreplace jelszó, és ahol volna meg kell-e hello jelszót?  

Napjainkban hello phone bejelentkezési szolgáltatás csak működik együtt a webalkalmazások és szolgáltatások, amelyek személyes Microsoft-fiókok, iOS vagy Android-alkalmazásokat, amelyek személyes Microsoft-fiókkal, és egy személyes Microsoft-fiókot használó alkalmazások a Windows 10 szerint vannak kapcsolva. Amikor bejelentkezik a webhelyek vagy alkalmazások tooone, ahol általában megadhatja a jelszó hello oldalon kapcsolat van, amely szerint **alkalmazást használhat a megnyitáshoz**. 

Telefonos bejelentkezés nem lehet használt toounlock Windows-számítógép, XBOX vagy bármely Microsoft-alkalmazások, például Office-alkalmazások asztali verzióinak most. 
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Vált a kétlépéses ellenőrzést? Kell kikapcsolni a azt?   

Egyes esetekben. Folyamatosan bővítjük hello hatókörének telefonos bejelentkezés bővíteni, de most még vannak, amelyek nem támogatják a Microsoft-ökoszisztéma hello helyen. Ezen a helyen továbbra is használjuk kétlépéses ellenőrzés a biztonságos bejelentkezés. Emiatt nem, akkor ne kétlépéses ellenőrzés kikapcsolása a fiókjához. 
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-tooapprove-two-notifications"></a>Rendben Ha a fiók-e kapcsolva a kétlépéses ellenőrzést megtartásához kell tooapprove két értesítést?

Nem, nem. Bejelentkezés Microsoft-fiók a telefonhoz tooyour számít a kétlépéses ellenőrzést. Helyett írja be a jelszavát, majd egy értesítés jóváhagyása, a személyazonosság és ismerje meg, hogyan toounlock telefonja, és ezután jóváhagyhatja az értesítést. Nem fognak kapni egy második értesítési tooapprove.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Mi történik, ha elveszti a telefonszám, vagy nincs velem, hogyan lehet érhető-fiókom?  

Mindig kattinthat **használja helyette a jelszó** hello bejelentkezési oldal tooswitch a biztonsági toousing a jelszavát. Ne feledje, hogy használja a kétlépéses ellenőrzést, ha még mindig kell egy második módszer tooverify a bejelentkezés. Ezért határozottan javasoljuk, hogy rendelkezik-e további, naprakész biztonsági adatok fiókja toomake. A biztonsági adatokat a https://account.live.com/proofs/manage kezelheti. 
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-tooentering-my-password"></a>Hogyan állítsa le, ezzel a szolgáltatással és vissza tooentering jelszó?

Kattintson a **használja helyette a jelszó** bejelentkezést követően. Azt a legutóbbi választott megjegyezhető, és kínál, amely alapértelmezett hello legközelebb bejelentkezik. Legalább egyszer toogo hátsó toosigning be a telefon, kattintson a **alkalmazást használhat a megnyitáshoz**. 
 
### <a name="can-i-use-hello-app-toosign-in-tooall-my-accounts-with-microsoft"></a>Használhatok hello app toosign tooall a saját partnerek Microsoft?   
Ez a funkció jelenleg csak személyes Microsoft fiókokhoz érhető el. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Képes vagyok jelentkezzen be a számítógép a telefon?  
A számítógép bejelentkezni a Windows Hello Windows 10 használatát javasoljuk a oldallal, ujjlenyomat vagy PIN-kódot.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>I jelentkezhet be a Windows Phone?  
Most hogy nem fejleszt ezt a funkciót a Windows Phone Microsoft Authenticator hello. 

## <a name="next-steps"></a>Következő lépések
Ha még nem töltötte le hello Microsoft Authenticator alkalmazást, tekintse meg. hello alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), és telefonos bejelentkezés elérhető a Microsoft Authenticator alkalmazás hello [Android](http://go.microsoft.com/fwlink/?Linkid=825072) és [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Ha kérdései hello alkalmazással kapcsolatos általános, tekintse meg hello [Microsoft hitelesítő – gyakori kérdések](microsoft-authenticator-app-faq.md)
