---
title: "Regisztrációs portál Súgó-témaköröket aaaApp |} Microsoft Docs"
description: "Hello Microsoft app regisztrációs portál különféle funkcióinak leírása."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 3eb17b629577446a336152799497e7d980fb825d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-registration-reference"></a>Alkalmazás regisztrálása referencia
Ez a dokumentum nyújt a környezet és a Microsoft App regisztrációs portál hello található különböző szolgáltatások leírása [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Saját alkalmazások
Ez a lista azokat az alkalmazásokat regisztrált hello Azure AD v2.0-végponttal való használatra.  Ezek az alkalmazások a Microsoft-fiók és a munkahelyi vagy iskolai fiókok az Azure Active Directory mindkét személyes fiókkal rendelkező felhasználók hello képességét toosign rendelkezik.  toolearn hello Azure AD v2.0-végponttól kapcsolatos további információkért tekintse meg a [v2.0 áttekintése](active-directory-appmodel-v2-overview.md).  Ezek az alkalmazások akkor is használt toointegrate hello Microsoft fiók hitelesítési végponttal `https://login.live.com`.

## <a name="live-sdk-applications"></a>Élő SDK-alkalmazásokra
Ez a lista azokat az alkalmazásokat, kizárólag a Microsoft-fiókkal történő használathoz regisztrált.  Nincsenek engedélyezve az Azure Active Directoryval legyen használható.  Ez az olyan alkalmazásokat, amelyek volt korábban regisztrálva hello msa-t a fejlesztői portálon, a talál `https://account.live.com/developers/applications`.  A korábban elvégzett összes függvények `https://account.live.com/developers/applications` most már az új portálon hajtható végre `https://apps.dev.microsoft.com`.  Ha a Microsoft-fiók alkalmazásokkal kapcsolatos további kérdése van, lépjen kapcsolatba velünk a következő címen.

## <a name="application-secrets"></a>Alkalmazás titkos kulcsok
Alkalmazás titkokat hitelesítő adatokat, amelyek lehetővé teszik az alkalmazás tooperform megbízható [ügyfél-hitelesítés](http://tools.ietf.org/html/rfc6749#section-2.3) az Azure ad-val.  Az OAuth és az OpenID Connect, egy alkalmazás titkos kulcsok leggyakrabban hivatkozott tooas egy `client_secret`.  A hello v2.0 protokoll bármely alkalmazás, amely fogad egy webes megcímezhető helyen biztonsági jogkivonatot (használatával egy `https` séma) kell használnia egy alkalmazás titkos tooidentify maga tooAzure AD visszaváltás a biztonsági jogkivonat alapján.  Továbbá, hogy az eszközön recieves jogkivonatok használatával egy alkalmazás titkos tooperform ügyfél-hitelesítéshez, toodiscourage kell tiltani a natív ügyfél hello nem biztonságos környezetben titkos kulcsok tárolására.

Minden alkalmazás tartalmazhat két érvényes alkalmazás titkok álljon időben.  Két titkos kulcsok fölött, hello ablilty tooperform rendszeres kulcsváltás e az alkalmazás teljes környezetben van.  Miután áttelepítette az alkalmazás tooa új titkos kulcs hello a teljes, hello régi titkos kulcs törlése, és egy új kiépítéséhez.

Jelenleg csak kétféle típusú alkalmazás titkos kulcsok hello app regisztrációs portálon engedélyezett.  Kiválasztása **új jelszó létrehozása** fogja létrehozni, és egy közös titkos kulcs tárolása hello megfelelő adattára, amely az alkalmazásban használható.  Kiválasztása **létre új kulcspár** hoz létre egy új nyilvános és titkos kulcsból álló kulcspárt, amely letölthető, és ügyfél-hitelesítési tooAzure AD használatos.

## <a name="profile"></a>Profil
alkalmazás-regisztrálási portál hello hello-profil szakasz lehet használt toocustomize hello bejelentkezési oldal az alkalmazáshoz.  Jelenleg a hello bejelentkezési lap alkalmazás embléma, a szolgáltatás URL-CÍMÉT, és az adatvédelmi nyilatkozatot feltételeket módosíthatja.  hello embléma képnek kell lennie átlátszó 48 x 48 vagy 50 x 50 képpontos GIF, PNG vagy JPEG fájlban 15 KB-os vagy kisebb.  Cserélje ki hello értékek és a bejelentkezési oldal eredő megtekintésre hello!

## <a name="live-sdk-support"></a>Élő SDK-támogatás
Ha engedélyezi a "Live SDK támogatása", bármely alkalmazás titkos kulcsok létrehozása kiépítendő hello Azure AD és a Microsoft Account adatait tárolja.  Ez lehetővé teszi az alkalmazás toointegrate közvetlenül az hello Microsoft Account szolgáltatás (login.live.com).  Ha a toobuild közvetlenül (megakadályozását toousing hello Azure AD v2.0-végponttól) a Microsoft Account az alkalmazást, meg kell győződnie arról, hogy engedélyezve van-e az élő támogatása az SDK-ban.

Élő SDK-támogatás letiltása biztosítja, hogy hello alkalmazás titkos kulcs csak írt hello Azure AD adattárba.  hello Azure AD-adattár, amelyek lehetővé teszik, hogy toomeet vállalati szintű szabályzat bizonyos szabványok, például a FISMA megfelelőségi magában foglalja.  Ha élő SDK támogatását engedélyezi, az alkalmazás nem érhetik el az egyes ezeknek a szabványoknak való megfelelés.

Ha csak toouse hello Azure AD v2.0-végpontra, Live SDK támogatási biztonságosan letilthatja.

