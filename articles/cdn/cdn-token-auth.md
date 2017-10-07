---
title: "aaaSecuring Azure CDN eszközök token hitelesítéssel |} Microsoft Docs"
description: "Jogkivonat hitelesítési toosecure hozzáférés tooyour Azure CDN eszközök használatával."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: 5865bcb8eed7ced834970d52d30136252039265f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Tokent használó hitelesítés az Azure CDN-eszközök védelme

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Áttekintés

Jogkivonat hitelesítési egy olyan mechanizmus, amely lehetővé teszi a kiszolgáló eszközök toounauthorized ügyfelek tooprevent Azure CDN szolgáltatás használata.  Ez általában történik tooprevent "hotlinking" tartalom, ahol egy másik webhelyre, gyakran egy üzenet üzenőfalon, használja az eszközök engedély Allowed értékű.  Ez hatással lehetnek a továbbítási költségeit. Azáltal, hogy a CDN ezt a funkciót, CDN peremhálózati előtt hello tartalomtovábbítás kapcsolódási kérések lesz hitelesítve. 

## <a name="how-it-works"></a>Működés

Jogkivonat hitelesítési ellenőrzi a kérelmeket a token értékét hello kérelmező kódolt információt tartalmazó kérelmek toocontain megkövetelésével. a megbízható helyek által generált. Tartalmat csak szolgáltató toorequester amikor hello kódolású adatokat igazodhat hello követelmények, ellenkező esetben kérelmeket a rendszer megtagadja. Egy vagy több, az alábbi paraméterekkel hello követelmény állíthat be.

- Ország: engedélyezi vagy megtagadja a hozzáférést meghatározott országokból származó kérelmek.  [Érvénytelen országhívó számokat listája.](https://msdn.microsoft.com/library/mt761717.aspx) 
- URL-címe: csak a megadott eszköz vagy az elérési út toorequest engedélyezése.  
- Állomás: engedélyezi vagy megtagadja a hozzáférést a kérelmek hello kérelem fejlécében megadott állomás használatát.
- Hivatkozó: engedélyezi vagy megtagadja a megadott hivatkozó toorequest.
- IP-cím: engedélyezése csak a megadott IP-cím vagy IP-alhálózat származó kérelmek.
- Protokoll: engedélyezi, vagy blokk-kéréseket hello protokollon alapuló használt toorequest hello tartalom.
- Lejárati idő: rendelje hozzá a dátum és idő, hogy a hivatkozás csak érvényes marad egy korlátozott ideig időszak tooensure.

Tekintse meg mindegyik paraméter részletes konfigurációs példát.

## <a name="reference-architecture"></a>Referenciaarchitektúra

Olvassa el a referencia-architektúrában, hogyan kell beállítani a CDN toowork a webalkalmazással tokent használó hitelesítés.

![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>CDN-végpont logika jogkivonatok érvényesség-ellenőrzése
    
Ez a diagram bemutatja, miként Azure CDN ellenőrzi az ügyfél kérésében Ha tokent használó hitelesítés konfigurálva van a CDN-végponthoz.

![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Token hitelesítés beállítása

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour CDN-profilt, és kattintson a hello **kezelése** gomb toolaunch hello kiegészítő portálon.

    ![CDN-profil panelje kezelése gomb](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Vigye **HTTP nagy**, és kattintson a **jogkivonat hitelesítési** a hello menü. Titkosítási kulcs és a titkosítási paraméterek ezen a lapon fog beállítani.

    1. Adjon meg egy egyedi titkosítási kulcsot a **elsődleges kulcs**.  Adjon meg egy másikat a **kulcs biztonsági mentése**

        ![CDN-jogkivonat hitelesítési telepítési kulcs](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. A titkosítási eszközével titkosítási paraméterek beállítása (engedélyezi vagy megtagadja a kérelmet az alapján a lejárati idő, ország, hivatkozó, protokoll, ügyfél IP. Használható bármilyen kombinációja.)

        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - EK-lejár: hozzárendel egy megadott időszak után a jogkivonat lejárati időt. Miután a rendszer megtagadja hello lejárati idő küldött kérelmeket. Ezt a paramétert használ Unix időbélyeg (1/1/1970 szabványos epoch óta eltelt percek alapján 00:00:00 GMT. Használható online eszközök tooprovide átalakítás téli idő és a Unix időpont között.)  Például, ha be szeretné tooset hello token toobe lejárt, 12, 31/2016 12:00:00 GMT, hello Unix idő: 1483185600 használja a következő:
    
        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - EK URL-cím engedélyezése: lehetővé teszi tootailor jogkivonatok tooa adott eszköz vagy az elérési út. Korlátozza a hozzáférést toorequests megadott relatív elérési úttal rendelkező start URL-cím. Mindegyik elérési út vesszővel elválasztva több elérési utat adjon meg. URL-címei kis-és nagybetűket. Attól függően, hogy hello követelmény állíthat be eltérő érték tooprovide eltérő szintű hozzáférési. Az alábbiakban néhány forgatókönyv:
        
            Ha egy URL-cím: http://www.mydomain.com/pictures/city/strasbourg.png. Lásd: a bemeneti érték "", és ennek megfelelően. a hozzáférési szint

            1. Adjon meg értéket "/": minden kérelemhez engedélyezett lesz
            2. Adjon meg értéket "/ képek": lehetővé teszi a kérelem a következő összes hello
            
                - http://www.mydomain.com/Pictures.png
                - http://www.mydomain.com/Pictures/City/Strasbourg.png
                - http://www.mydomain.com/picturesnew/City/strasbourgh.png
            3. Adjon meg értéket "/ képek /": csak a /pictures/ lesz engedélyezett kérelmek
            4. Adjon meg értéket "/ pictures/city/strasbourg.png": csak az eszköz lehetővé teszi, hogy kérelem
    
        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - EK ország engedélyezése: csak lehetővé teszi, hogy egy vagy több megadott országokból kérelmekkel. Más országokból kérelmekkel elutasításra kerülne. Használja az ország kód tooset hello paramétereket, és minden egyes országhívó szám vesszővel elválasztva. Például ha azt szeretné, hogy az Amerikai Egyesült Államokban és Franciaország tooallow hozzáférés, adjon meg VELÜNK, FR hello oszlop szerint alatt.  
        
        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - EK ország elutasítás: egy vagy több megadott országokból származó megtagadja. Más országokból kérelmekkel engedélyezett lesz. Használja az ország kód tooset hello paramétereket, és minden egyes országhívó szám vesszővel elválasztva. Például ha azt szeretné, hogy az Amerikai Egyesült Államokban és Franciaország toodeny hozzáférés, adjon meg VELÜNK, FR hello oszlopban.
    
        - EK ref engedélyezése: csak a megadott hivatkozó lehetővé teszi a kérelmek. A hivatkozó hello URL-címet, amelyre a társított toohello erőforrás kért weblap hello azonosítja. hello hivatkozó paraméterérték hello protokoll nem tartalmazhatja. Az állomásnév és/vagy egy adott elérési utat adott állomásnév a szövegszerkesztőben. Több hivatkozó kérelmei belül mindegyiknél vesszővel elválasztva egyetlen paramétert is hozzáadhat. Ha hivatkozó értéket adta, de hello hivatkozó adatok küldése nem hello kérelem miatt toosome konfigurációját, ezek a kérelmek elutasításra kerülne alapértelmezés szerint. Hozzárendelhet "Hiányzó" vagy üres értéket hello paraméter tooallow ezeket a kéréseket a hivatkozó adatok hiányoznak. Is "*. consoto.com" tooallow consoto.com az összes altartomány.  Ha azt szeretné, hogy a www.consoto.com, consoto2.com és az üres vagy hiányzó reffers erquests altartományokkal érkező kéréseket tooallow hozzáférés, például adjon meg értéket az alábbi:
        
        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - EK ref elutasítás: a megadott hivatkozó megtagadja. Tekintse meg a toodetails és példa "EK-ref-engedélyezése" paraméterben.
         
        - EK protokoll engedélyezése: csak lehetővé teszi, hogy a megadott protokoll érkező kérelmeket. Például a http vagy https.
        
        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - EK protokoll elutasítás: a megadott protokoll megtagadja. Például a http vagy https.
    
        - EK-ügyfélip: korlátozza a hozzáférést toospecified kérelmező IP-címet. IPV4 és IPV6 használata támogatott. Megadhat egy kérelemhez IP-cím vagy IP-alhálózat.
            
        ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. A token hello visszafejtés eszközzel tesztelheti.

    4. Testre szabhatja hello válaszának típusa, amelyet az eredmény toouser amikor kérelmet a rendszer megtagadja. Alapértelmezés szerint 403 használjuk.

3. Most kattintson **szabálymotor** lap **HTTP nagy**. Lesz a lap toodefine elérési utak tooapply hello funkcióval, hello jogkivonat hitelesítési szolgáltatás engedélyezése és engedélyezheti a további jogkivonat hitelesítési kapcsolatos képességeit.

    - "IF" oszlop toodefine eszköz vagy az elérési út, amelyet az tooapply tokent használó hitelesítés használata. 
    - Kattintson a "Token Auth" tooadd hello szolgáltatás legördülő tooenable token hitelesítés alól.
        
    ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. A hello **szabálymotor** lap, van néhány további képességeket engedélyezheti.
    
    - A token hitelesítés megtagadása kód: meghatározza, hogy hello típusú válaszban visszaadott toouser során a rendszer megtagadja a kérelmet. Itt beállított szabályok felülírják hello Megtagadás beállítása hello jogkivonat hitelesítési lapon.
    - Jogkivonat hitelesítési figyelmen kívül hagyja: meghatározza, hogy a használt URL-cím toovalidate token és a nagybetűk között legyen.
    - Jogkivonat hitelesítési paraméter: hello megjelenítő karakterlánc-paramétert a kért URL-cím hello jogkivonat hitelesítési lekérdezés átnevezése. 
        
    ![CDN-profil panelje kezelése gomb](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Testre szabhatja a jogkivonatot, amely az alkalmazás által generált jogkivonat-alapú hitelesítési szolgáltatások lexikális eleme. Forráskódját itt is elérhetők a [GitHub](https://github.com/VerizonDigital/ectoken).
Rendelkezésre álló nyelvek:
    
    - C#
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Az Azure CDN szolgáltatásai és szolgáltató díjszabása

Lásd: hello [CDN áttekintésével](cdn-overview.md) témakör.
