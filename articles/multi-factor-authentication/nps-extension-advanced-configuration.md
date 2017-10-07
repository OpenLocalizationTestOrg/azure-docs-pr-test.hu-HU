---
title: "Azure MFA NPS bővítmény aaaConfigure hello |} Microsoft Docs"
description: "Hello NPS-bővítményének telepítése után ezeket a lépéseket használhatja a Speciális konfiguráció például IP engedélyezése és az egyszerű Felhasználónevük cseréje."
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
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: c3aed077b23c95f874861eb00c8e6dca668329c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-options-for-hello-nps-extension-for-multi-factor-authentication"></a>Speciális konfigurációs beállítások a hálózati házirend-kiszolgáló bővítmény hello a többtényezős hitelesítés

Hálózati házirend-kiszolgáló (NPS) bővítmény hello a felhőalapú Azure multi-factor Authentication szolgáltatások átnyúlik a helyszíni infrastruktúrával. Ez a cikk feltételezi, hogy már rendelkezik a hello bővítmények vannak telepítve, és most szeretné, hogy hogyan toocustomize hello bővítmény meg kell tooknow. 

## <a name="alternate-login-id"></a>Másodlagos bejelentkezési Azonosítóval

Mivel a hálózati házirend-kiszolgáló bővítmény hello tooboth csatlakozik, a helyszíni és felhőalapú könyvtárak, ahol a helyszíni egyszerű felhasználónév (UPN) nem egyezik a hello nevek hello felhőben problémát előforduló. toosolve probléma használható alternatív bejelentkezési azonosítókat. 

Belül hello NPS-bővítmény az Active Directory-attribútum toobe, használja a hello egyszerű Felhasználónevük helyett az Azure multi-factor Authentication is kijelölhet. Ez lehetővé teszi, hogy Ön tooprotect a helyszíni erőforrásokat a kétlépéses ellenőrzéshez használttal a helyszíni UPN-EK módosítása nélkül. 

tooconfigure másodlagos bejelentkezési azonosítókat, nyissa meg túl`HKLM\SOFTWARE\Microsoft\AzureMfa` és a következő beállításazonosítókat hello szerkesztése:

| Név | Típus | Alapértelmezett érték | Leírás |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | Karakterlánc | üres | Jelölje ki a megjeleníteni kívánt toouse hello egyszerű Felhasználónevük helyett az Active Directory-attribútum neve hello. Ez az attribútum van megadva hello AlternateLoginId attribútumaként. Ha ez a beállításazonosító értéke tooa [érvényes Active Directory-attribútumot](https://msdn.microsoft.com/library/ms675090.aspx) (a példában, levelezési vagy displayName), majd hello attribútum értéke helyett hello felhasználói UPN-hitelesítéshez használt. Ha ez a beállításazonosító nem üres, vagy nincs konfigurálva, majd AlternateLoginId le van tiltva, és hello felhasználói UPN-hitelesítéshez használt. |
| LDAP_FORCE_GLOBAL_CATALOG | Logikai érték | False (Hamis) | Use a jelző tooforce hello globális katalógus az LDAP-keresésekhez AlternateLoginId keresésekor. A tartományvezérlő beállítása a globális katalógus vegye hello AlternateLoginId attribútum toohello globális katalógus, és engedélyeznie kell ezt a jelzőt. <br><br> Ha LDAP_LOOKUP_FORESTS van konfigurálva (nem üres), **Ez a jelző IGAZ van kényszerítve**, függetlenül attól, hello értékének hello beállításjegyzék-beállítást. Ebben az esetben hello hálózati házirend-kiszolgáló bővítmény hello globális katalógus toobe hello AlternateLoginId attribútum az egyes erdőkhöz konfigurálva van szükség. |
| LDAP_LOOKUP_FORESTS | Karakterlánc | üres | Erdők toosearch pontosvesszővel elválasztott listáját tartalmazzák. Például *contoso.com;foobar.com*. Ha ez a beállításazonosító van beállítva, a hálózati házirend-kiszolgáló bővítmény hello ismételt, amelyben szereplő és hello első sikeres AlternateLoginId értékét adja vissza hello sorrendben keres a összes hello erdőben. Ha ez a beállításazonosító nincs konfigurálva, a hello AlternateLoginId keresési zárt toohello aktuális tartományban.|

tootroubleshoot problémák másodlagos bejelentkezési azonosítókat, használja a javasolt lépéseket a hello [másodlagos bejelentkezési azonosító hibák](multi-factor-authentication-nps-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP-kivételek

Ha szeretne toomonitor kiszolgáló rendelkezésre állása, például ha egy terheléselosztó azok a kiszolgálók ellenőrizze fut-e a munkaterhelések, elküldése előtt nem szeretné, hogy ezen ellenőrzések toobe igazolási kérések blokkolja. Ehelyett hozzon létre, amelyek biztosan szolgáltatásfiókok által használt IP-címek listáját, és tiltsa le a multi-factor Authentication követelményeinek, hogy a lista. 

túl nyissa meg az IP-engedélyezési lista tooconfigure`HKLM\SOFTWARE\Microsoft\AzureMfa` és beállításazonosítót a következő hello konfigurálása: 

| Név | Típus | Alapértelmezett érték | Leírás |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | Karakterlánc | üres | Adjon meg egy IP-címek pontosvesszővel elválasztott listája. Gépek, ahol kérések érkeznek, mint hello NAS/VPN-kiszolgáló hello IP-címét tartalmazza. IP-címtartományok olyan alhálózatok nem támogatottak. <br><br> Például *10.0.0.1;10.0.0.2;10.0.0.3*.

Ha a kérelem érkezik hello engedélyezési lista megtalálható IP-címről, kétlépéses ellenőrzés kimarad. hello IP-engedélyezési lista az összehasonlított toohello IP-cím hello megadott *ratNASIPAddress* attribútumának hello RADIUS-kérelmeket. Ha RADIUS kérelem érkezik hello ratNASIPAddress attribútum nélkül, a rendszer naplózza hello figyelmeztetés a következő: "P_WHITE_LIST_WARNING::IP engedélyezett van mellőzve forrás IP-cím a RADIUS-kérelmet NasIpAddress attribútum hiányzik."

## <a name="next-steps"></a>Következő lépések

[Hárítsa el a hálózati házirend-kiszolgáló bővítmény hello hibaüzeneteket az Azure multi-factor Authentication](multi-factor-authentication-nps-errors.md)
