---
title: "Az Azure AD Connect: Áteresztő hitelesítés – intelligens zárolás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure Active Directory (Azure AD) áteresztő hitelesítés védelmet nyújt a helyszíni fiókok hello felhőben találgatásos jelszó támadásoktól."
services: active-directory
keywords: "Az Azure AD Connect áteresztő hitelesítés, telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: b02e315c3cc3eae00ca6408d735a416f34c2cdc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Az Azure Active Directory áteresztő hitelesítés: Az intelligens zárolás

## <a name="overview"></a>Áttekintés

Az Azure AD találgatásos jelszó támadások ellen, és megakadályozza, hogy a valódi felhasználók az Office 365 és az SaaS-alkalmazások kívül zárás alatt. Ez a funkció hívása **intelligens zárolás**, akkor támogatott, ha átmenő hitelesítést, a bejelentkezési módszer használatát. Intelligens zárolás alapértelmezés szerint engedélyezve van az összes bérlőre vonatkozó és védeni, a felhasználói fiókok minden hello idő; nincs szükség tooturn van azt meg.

Intelligens zárolás működik, hogy nyomon követi a sikertelen bejelentkezési kísérletek, és egy bizonyos után **bejelentkezési próbálkozásra van lehetőségük**kezdődően egy **kizárás időtartama**. Hello támadó hello kizárás időtartama során a bejelentkezési kísérletek utasítja el. Ha hello támadás továbbra is fennáll, hello későbbi sikertelen bejelentkezési kísérletek hello kizárás időtartama leteltével fiókzárolási hosszabb időtartamokat eredményez.

>[!NOTE]
>hello alapértelmezett bejelentkezési próbálkozásra van lehetőségük, 10 sikertelen bejelentkezési kísérletek hello alapértelmezett kizárás időtartama érték 60 másodperc.

Intelligens zárolás is a bejelentkezések támadók és valódi felhasználók és a legtöbb esetben hello támadók csak zárolja a különböztet. Ez a funkció megakadályozza, hogy a támadók ártó zárolják a valódi felhasználók. Bejelentkezési viselkedés, a felhasználói eszközök & böngészők és egyéb jelekkel toodistinguish valódi felhasználók és a támadók között túli használjuk. Azt a rendszer folyamatosan javítása az algoritmusok.

Áteresztő hitelesítés továbbítja a jelszó érvényesítése kérelmek alakzatot a helyszíni Active Directory (AD), mert a felhasználók AD-fiókok zárolásának tooprevent támadók szüksége. Mivel a saját AD fiókzárolási házirendek (pontosabban [ **fiókzárolás küszöbértékénél** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) és [ **alaphelyzetbe fiók zárolása számláló után házirendek** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), tooconfigure az Azure AD bejelentkezési próbálkozásra van lehetőségük van szüksége, és kizárás időtartama értékek megfelelően kimenő hello felhőben támadások toofilter elérése előtti a helyszíni AD.

>[!NOTE]
>hello intelligens zárolás funkciót szabad és _a_ alapértelmezés szerint az összes ügyfél számára. Azonban az Azure AD bejelentkezési próbálkozásra van lehetőségük és fiókzárolási Duration típusú értékek Graph API-jával módosítása licencre van szüksége a bérlő toohave legalább egy Azure AD Premium P2. Nem kell az Azure AD Premium P2 licencek _felhasználónként_ tooget hello intelligens zárolás funkciót átmenő hitelesítéssel.

tooensure, hogy a felhasználók a helyszíni AD-fiókok védettek, tooensure van szüksége, amely:

1.  Az Azure AD bejelentkezési próbálkozásra van lehetőségük van _kevesebb_ mint AD a fiókzárolás küszöbértékénél. Azt javasoljuk, hogy hello értékeket állítsa be úgy, hogy a fiókzárolás küszöbértékénél AD meg legalább két vagy három alkalommal fordult elő az Azure AD bejelentkezési próbálkozásra van lehetőségük.
2.  Az Azure AD kizárás időtartama (másodpercben jelölt) _hosszabb_ mint AD meg alaphelyzetbe fiók zárolása számláló után (összességében percben megadva).

## <a name="verify-your-ad-account-lockout-policies"></a>Ellenőrizze a fiók zárolása AD házirendek

Az AD a fiókzárolás házirendek használata a következő utasításokat tooverify hello:

1.  Hello Csoportházirend kezelése eszköz megnyitásához.
2.  Hello csoportházirend szerkesztése, amely alkalmazott tooall felhasználók, például hello alapértelmezett tartományi házirendé.
3.  Keresse meg a tooComputer konfigurációja\Házirendek\A beállításai\Biztonsági beállítások\Fiókházirend\Fiókzárolási házirend.
4.  Ellenőrizze a fiókzárolás küszöbértékénél és alaphelyzetbe fiók zárolása számláló után értékeket.

![AD fiókzárolási házirendek](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-hello-graph-api-toomanage-your-tenants-smart-lockout-values"></a>A bérlő intelligens zárolás értékek hello Graph API toomanage használata

>[!IMPORTANT]
>Az Azure AD bejelentkezési próbálkozásra van lehetőségük és fiókzárolási Duration típusú értékek Graph API-jával módosítása az Azure AD Premium P2 szolgáltatása. Azt is igényli, toobe egy a bérlő globális rendszergazdája.

Használhat [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, állítsa be, és az Azure AD intelligens zárolás értékeinek frissítéséhez. Azonban programozott módon ezeket a műveleteket is van.

### <a name="read-smart-lockout-values"></a>Olvassa el az intelligens zárolás értékek

Kövesse ezeket a lépéseket tooread a bérlő intelligens zárolás értékeket:

1. A bérlő globális rendszergazdaként jelentkezzen be arra Graph Explorer. Ha a rendszer kéri, adjon hozzáférést a hello kért engedélyeket.
2. "Engedélyek módosítása" gombra, és válassza ki a hello "Directory.ReadWrite.All" engedéllyel.
3. Az alábbiak szerint konfigurálhatja hello Graph API-kérelem: verzió beállítása túl "Béta" típushoz túl "GET" és az URL-címe túl`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Kattintson a "Lekérdezés futtatása" toosee a bérlő intelligens zárolás értékeket. Ha a bérlő értékek előtt még nem állított, lásd: üres.

### <a name="set-smart-lockout-values"></a>Intelligens zárolás értékeinek megadása

Kövesse ezeket a lépéseket tooset a bérlő intelligens zárolás értékeit (csak az első alkalommal hello):

1. A bérlő globális rendszergazdaként jelentkezzen be arra Graph Explorer. Ha a rendszer kéri, adjon hozzáférést a hello kért engedélyeket.
2. "Engedélyek módosítása" gombra, és válassza ki a hello "Directory.ReadWrite.All" engedéllyel.
3. Az alábbiak szerint konfigurálhatja hello Graph API-kérelem: verzió beállítása túl "Béta" típushoz túl "POST" és az URL-címe túl`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Másolással illessze be a következő JSON-kérelmi hello "Kérelemtörzset" mezőbe hello. Hello intelligens zárolás értékeket szükség szerint módosítsa és használja egy véletlenszerű GUID Azonosítót `templateId`.
5. Kattintson a "Lekérdezés futtatása" tooset a bérlő intelligens zárolás értékeket.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Ha nem használ őket, meghagyhatja hello **BannedPasswordList** és **EnableBannedPasswordCheck** szerinti üres érték ("") és "false" kulcsattribútumokkal.

Győződjön meg arról, hogy beállította-e a bérlő intelligens zárolás értékek megfelelően [ezeket a lépéseket](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Intelligens zárolás értékek frissítése

Kövesse ezeket a lépéseket tooupdate a bérlő intelligens zárolás értékek (Ha be őket előtt):

1. A bérlő globális rendszergazdaként jelentkezzen be arra Graph Explorer. Ha a rendszer kéri, adjon hozzáférést a hello kért engedélyeket.
2. "Engedélyek módosítása" gombra, és válassza ki a hello "Directory.ReadWrite.All" engedéllyel.
3. [Kövesse ezeket a lépéseket tooread a bérlő intelligens zárolás értékek](#read-smart-lockout-values). Másolás hello `id` hello cikk "megjelenített"nevű, "PasswordRuleSettings" (GUID) értékét.
4. Hello Graph API-kérelem az alábbiak szerint konfigurálhatja: állítsa be a verzió túl "Béta" kéréstípus túl "Javítás" és az URL-cím túl`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -használata GUID Azonosítót a 3. lépés a hello `<id>`.
5. Másolással illessze be a következő JSON-kérelmi hello "Kérelemtörzset" mezőbe hello. Hello intelligens zárolás értékeket szükség szerint módosítható.
6. Kattintson a "Lekérdezés futtatása" tooupdate a bérlő intelligens zárolás értékeket.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Győződjön meg arról, hogy a bérlő intelligens zárolás értékek megfelelően frissítette [ezeket a lépéseket](#read-smart-lockout-values).

## <a name="next-steps"></a>Következő lépések
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.
