---
title: "aaaLDAP hitelesítés és az Azure MFA kiszolgáló |} Microsoft Docs"
description: "Ez az LDAP-hitelesítés és az Azure multi-factor Authentication kiszolgáló üzembe helyezése segít hello Azure multi-factor Authentication hitelesítési lapot."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 17a26b57dbf6afa2fcfdb3d19c5b5ba2987a9f79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-hitelesítés és az Azure multi-factor Authentication kiszolgáló
Alapértelmezés szerint hello Azure multi-factor Authentication kiszolgálón konfigurált tooimport vagy felhasználók Active Directory szinkronizálását. Azt azonban nem konfigurált toobind toodifferent LDAP-címtárak esetén például ADAM-címtárhoz vagy megadott Active Directory-tartományvezérlőhöz. Ha csatlakoztatott tooa directory LDAP keresztül, hello Azure multi-factor Authentication kiszolgáló működhet, és az LDAP-proxy tooperform hitelesítéseket. Azt is lehetővé teszi hello használata LDAP-kötés RADIUS-célként, előhitelesítés során a felhasználók az IIS-hitelesítést, vagy az elsődleges hitelesítéshez hello Azure multi-factor Authentication felhasználói portálon.

Azure multi-factor Authentication LDAP-proxyként, toouse beszúrása hello Azure multi-factor Authentication kiszolgáló hello LDAP-ügyfél (például a VPN-készülék, az alkalmazás) és az LDAP-címtárkiszolgálóhoz hello között. hello Azure multi-factor Authentication kiszolgáló a hello ügyfélkiszolgálókkal, mind a hello LDAP-címtárhoz konfigurált toocommunicate kell lennie. Ebben a konfigurációban a hello Azure multi-factor Authentication kiszolgáló LDAP kérések ügyfél kiszolgálók és alkalmazások fogad, és továbbítja őket a toohello cél LDAP directory toovalidate hello elsődleges hitelesítő adatait. Ha az LDAP-címtár hello hello elsődleges hitelesítő adatok érvényesítése, Azure multi-factor Authentication második azonosító ellenőrzi és elküld egy választ hátsó toohello LDAP-ügyfél. hello teljes hitelesítés sikeres, csak akkor, ha a hello LDAP server-hitelesítés és a hello második lépés ellenőrzése sikertelen.

## <a name="configure-ldap-authentication"></a>LDAP-hitelesítés konfigurálása
tooconfigure LDAP-hitelesítés, telepítés hello Azure multi-factor Authentication kiszolgáló a Windows server. A következő eljárás hello használata:

### <a name="add-an-ldap-client"></a>LDAP-ügyfél hozzáadása

1. Hello Azure multi-factor Authentication kiszolgáló válassza ki a hello LDAP-hitelesítés ikonra a bal oldali menü hello.
2. Ellenőrizze a hello **LDAP-hitelesítés engedélyezése** jelölőnégyzetet.

   ![LDAP-hitelesítés](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)

3. Hello ügyfelek lapon módosítsa a hello TCP-port és az SSL-port lehetőséget, ha hello Azure multi-factor Authentication LDAP szolgáltatás toonon szabványnak portok toolisten az LDAP-kérésekhez kell kötni.
4. Ha azt tervezi, hogy a hello ügyfél toohello Azure multi-factor Authentication kiszolgáló LDAPS toouse, SSL-tanúsítványt kell telepíteni hello MFA kiszolgáló ugyanarra a kiszolgálóra. Kattintson a **Tallózás** következő toohello SSL tanúsítvány mezőbe, és válassza ki a tanúsítvány toouse hello biztonságos kapcsolat.
5. Kattintson az **Add** (Hozzáadás) parancsra.
6. A hello LDAP-ügyfél hozzáadása párbeszédpanelen adja meg a hello IP-cím hello átjárókészülék, a kiszolgáló vagy az alkalmazás, amely hitelesíti a toohello kiszolgáló és az alkalmazás neve (nem kötelező). hello alkalmazásnév Azure multi-factor Authentication-jelentésekben jelenik meg, és előfordulhat, hogy SMS vagy mobilalkalmazás hitelesítési üzenet jelenjen meg.
7. Ellenőrizze a hello **Azure multi-factor Authentication felhasználói egyeztetés megkövetelése** mezőben, ha az összes felhasználót törölték, vagy importálja a hello kiszolgáló és a tulajdonos tootwo lépés ellenőrzése. Ha a felhasználók jelentős számú nem még importálva lettek hello kiszolgálón és/vagy nem érvényes a kétlépéses ellenőrzést, hagyja a mezőt hello nincs bejelölve. Hello MFA kiszolgáló súgófájlban további információért tekintse meg ezt a szolgáltatást.

Ismételje meg a lépéseket tooadd további LDAP-ügyfél.

### <a name="configure-hello-ldap-directory-connection"></a>Hello LDAP-címtár kapcsolat konfigurálása

Ha hello Azure multi-factor Authentication konfigurált tooreceive LDAP-hitelesítés, azt kell proxy e hitelesítések toohello LDAP-címtár. Ezért hello cél lap csak jelenít meg egyetlen szürkén jelenik meg a beállítás toouse egy LDAP-tárolóhoz.

1. tooconfigure hello LDAP-címtár kapcsolatot, kattintson a hello **címtár-integráció** ikonra.
2. Hello beállítások lapon válassza ki a hello **adott LDAP-konfiguráció használata** választógombot.
3. Válassza a **Szerkesztés…** elemet.
4. A hello LDAP-konfiguráció szerkesztése párbeszédpanelen hello mezőket a hello szükséges adatokat tooconnect toohello LDAP-címtár feltöltéséhez. Hello mezők leírását hello Azure multi-factor Authentication kiszolgáló súgófájl szerepelnek.

    ![Címtár-integráció](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)

5. Hello LDAP-kapcsolat tesztelése gombra kattintva hello **teszt** gombra.
6. Ha hello LDAP kapcsolat ellenőrzése sikeres volt, kattintson a hello **OK** gombra.
7. Kattintson a hello **szűrők** külön-külön hello kiszolgáló, előre konfigurált tooload tárolók, biztonsági csoportok és felhasználók Active Directoryból. Ha kötés tooa másik LDAP-címtárhoz, valószínűleg szüksége tooedit hello szűrők jelenik meg. Kattintson a hello **súgó** szűrők további információ hivatkozásra.
8. Kattintson a hello **attribútumok** külön-külön hello kiszolgáló előre konfigurált toomap attribútumok az Active Directoryból.
9. Ha tooa különböző LDAP könyvtár vagy toochange hello előre megadott attribútum-leképezésekhez most kötést, kattintson a **szerkesztése...**
10. Hello attribútumok szerkesztése párbeszédpanelen módosítsa a hello LDAP attribútum-leképezésekhez a címtárban. Attribútumnevek írt, vagy kiválasztott hello kattintva **...** következő tooeach mező gombra. Kattintson a hello **súgó** hivatkozás a további információkra attribútumok.
11. Kattintson a hello **OK** gombra.
12. Kattintson a hello **vállalati beállítások** ikonra, és jelölje be hello **felhasználónév-feloldás** fülre.
13. Ha tooActive Directory kapcsolat egy tartományhoz csatlakoztatott kiszolgáló esetén hagyja hello **Windows biztonsági azonosítók (SID) az egyező felhasználónevekhez** kijelölt választógombot. Ellenkező esetben válassza a hello **LDAP egyedi azonosító attribútum használata a felhasználónevek egyeztetéséhez** választógombot. 

Ha hello **LDAP egyedi azonosító attribútum használata a felhasználónevek egyeztetéséhez** választógomb meg van adva, hello Azure multi-factor Authentication kiszolgáló megkísérli tooresolve minden egyes username tooa szereplő egyedi azonosítóra hello LDAP-címtárhoz. Egy LDAP-keresés történik hello felhasználónév a címtár-integráció hello meghatározott attribútumok -> attribútumok lapon. A felhasználók hitelesítése, hello felhasználónév feloldása toohello hello LDAP-címtárban szereplő egyedi azonosítóra. egyező hello felhasználó hello Azure multi-factor Authentication adatfájlban hello egyedi azonosítót kell használni. Ez lehetővé teszi a nem betűérzékeny összehasonlítást, és a hosszú és rövid felhasználónév-formátumokat.

Miután elvégezte ezeket a lépéseket, hello MFA kiszolgáló LDAP hozzáférési kérelmek hello hello konfigurált portokat a beállított figyeli az ügyfelek és tevékenységéért, azok proxy toohello LDAP-címtár hitelesítési kérelmek.

## <a name="configure-ldap-client"></a>LDAP-ügyfél konfigurálása
tooconfigure hello LDAP-ügyfél, használja a hello irányelveket:

* Konfigurálja a készülék, a kiszolgáló vagy az alkalmazás tooauthenticate Azure multi-factor Authentication kiszolgáló LDAP toohello keresztül, az LDAP-címtárral, mintha. Használjon hello azonos beállításokat, hogy szokásos módon használhatja tooconnect közvetlenül tooyour LDAP-címtárhoz, kivéve a hello kiszolgáló neve vagy IP-címet, amely lesz az Azure multi-factor Authentication kiszolgáló hello.
* Hello LDAP időtúllépés too30-60 másodperc konfigurálja úgy, hogy az LDAP-címtár hello idő toovalidate hello felhasználó hitelesítő adatait, hello második lépés ellenőrzés, a válasz fogadása és toohello LDAP hozzáférési kérelem válaszol.
* LDAPS használatakor hello berendezés vagy a kiszolgáló hello egy LDAP-lekérdezés végrehajtása meg kell bíznia hello SSL-tanúsítvány hello Azure multi-factor Authentication kiszolgáló telepíthető.

