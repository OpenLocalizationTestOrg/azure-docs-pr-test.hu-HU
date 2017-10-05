---
title: "Az egyéni Azure CDN-tartomány HTTPS engedélyezése |} Microsoft Docs"
description: "Útmutató a HTTPS engedélyezése az Azure CDN-végpont egy egyéni tartomány."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Az egyéni Azure CDN-tartomány HTTPS engedélyezése

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Egyéni Azure CDN-tartomány HTTPS támogatása lehetővé teszi, hogy a tartalmak biztonságos saját tartománynév használatával, melyekkel biztonságosabbá teheti az átvitel során az adatok SSL használatával. A végpontok közötti munkafolyamat HTTPS engedélyezése az egyéni tartomány egyszerűsödött, egy kattintással engedélyezését, teljes és minden további költség nélkül.

Nagyon fontos adatvédelmi és a webes alkalmazások bizalmas adatok az átvitel alatt adatok integritásának biztosításához. A HTTPS protokoll használatával biztosítja, hogy a bizalmas adatok titkosítva van, amikor az interneten keresztül továbbítja azokat. Megbízható, hitelesítés és a webes alkalmazások megvédi a támadások biztosít. Azure CDN jelenleg a CDN-végpont támogatja a HTTPS PROTOKOLLT. Például, ha a CDN-végpont létrehozása az Azure CDN (pl. https://contoso.azureedge.net), HTTPS alapértelmezés szerint engedélyezve van. Most az egyéni tartomány HTTPS, engedélyezheti a biztonságos kézbesítés egy egyéni tartomány (pl. https://www.contoso.com) is. 

A legfőbb attribútumai a HTTPS-szolgáltatás a következők:

- További költség nélkül: a tanúsítvány-beszerzéssel vagy megújítása nem költségekkel és a HTTPS-forgalmat további költség nélkül. Ugyanúgy kell fizetnie GB kilépő a CDN.

- Egyszerű engedélyezése: egy kattintson kiépítés érhető el a [Azure-portálon](https://portal.azure.com). REST API vagy egyéb fejlesztői eszközök segítségével is engedélyezze a szolgáltatást.

- Fejezze be a Tanúsítványkezelő: az összes tanúsítvány beszerzésének és kezelik a kulcsokat meg. A tanúsítványok automatikusan kiépítése és megújítani lejárta előtt. Ez teljesen eltávolítja a szolgáltatás megszakítás miatt a tanúsítvány lejárati ideje kockázatát.

>[!NOTE] 
>Mielőtt engedélyezné a HTTPS PROTOKOLLT támogatja, akkor kell már létrehozott egy [egyéni Azure CDN-tartomány](./cdn-map-content-to-custom-domain.md).

## <a name="step-1-enabling-the-feature"></a>1. lépés: A funkció engedélyezése 

1. Az a [Azure-portálon](https://portal.azure.com), tallózással keresse meg a Verizon standard vagy prémium szintű CDN-profilt.

2. A végpontok, kattintson az egyéni tartomány tartalmazó végpont.

3. Kattintson az egyéni tartománynak a HTTPS engedélyezéséhez.

    ![Végpont panel](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Kattintson a **a** HTTPS engedélyezése és a módosítás mentéséhez.

    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>2. lépés: Tartomány ellenőrzéséhez

>[!IMPORTANT] 
>Tartomány ellenőrzéséhez kell végeznie, mielőtt a saját egyéni tartománynévre HTTPS aktív lesz. 6 üzleti napja hagyja jóvá a tartományban van. Kérelem 6 munkanapon belül nem jóváhagyással végrehajtások leállnak.  

Miután engedélyezte az egyéni tartomány a HTTPS, a HTTPS-tanúsítvány-szolgáltatója DigiCert fogja ellenőrizni a tulajdonjogát, a tartomány kapcsolatba lépve a bejegyzés a tartományba, WHOIS bejegyzés információk, e-mailben (alapértelmezés) vagy telefonos alapján. DigiCert is el fogja küldeni az ellenőrző e-mailt az alábbi címek. Ha WHOIS bejegyzés információk személyes, győződjön meg arról jóváhagyhatja közvetlenül az egyik ezeknél a címeknél.

>felügyeleti @< your-tartományi-name.com > rendszergazda @< your-tartományi-name.com >  
>gazdáját @< your-tartományi-name.com >  
>hostmaster @< your-tartományi-name.com >  
>postamester @< your-tartományi-name.com >


E-mail fogadása után ellenőrzési két lehetőség közül választhat:

1. Ugyanazt a fiókot a legfelső szintű tartománynak, pl. consoto.com keresztül az összes jövőbeni megrendelések jóváhagyhatja. Ez az ajánlott módszer, abban az esetben, ha azt tervezi, hogy további egyéni tartományokat a jövőben ugyanazon gyökértartományának hozzáadása.
 
2. A megadott állomásnév, amelyet a kérés ugyanúgy jóváhagyhatja. További jóváhagyás lesz szükséges további kérelmeknél.

    Példa e-mailek:
    
    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/domain-validation-email-example.png)

A jóváhagyást követően DigiCert felveszi az egyéni tartománynevet a SAN-tanúsítvány. Az egy évet a tanúsítvány érvényes lesz, és automatikusan megújul lejárta előtt.

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a>3. lépés: A propagálás várja meg, majd indítsa el, a szolgáltatás használata

A tartománynév érvényesítését követően az egyéni tartomány HTTPS funkció aktív akár 6 – 8 óráig fog tartani. A folyamat befejezése után, az Azure portálon "egyéni HTTPS" állapotát állítja be "Engedélyezett". HTTPS-FORGALOMHOZ pedig az egyéni tartomány már készen áll a használatra.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

1. *A tanúsítványszolgáltató és használt tanúsítvány típusa?*

    Tulajdonos alternatív neve (SAN) tanúsítvány DigiCert által biztosított használjuk. SAN-tanúsítványnak biztonságossá teheti az egy tanúsítvánnyal rendelkező több teljesen minősített tartománynév.

2. *A dedikált tanúsítvány is használható?*
    
    Jelenleg nem de a programba.

3. *Mi történik, ha a tartomány megerősítési e-mailt nem jelenik meg a DigiCert?*

    Lépjen kapcsolatba a Microsofttal, ha 24 órán belül nem kap egy e-mailt.

4. *Kevésbé biztonságos, mint egy dedikált tanúsítvány SAN tanúsítványt használ?*
    
    Egy SAN-tanúsítvány, egy dedikált cert azonos titkosítási és biztonsági szabványok követi. Összes kiállított SSL-tanúsítvány SHA-256 algoritmust használ a fokozott biztonság.

5. *Használható-e az egyéni tartomány HTTPS Akamai Azure CDN?*

    Ez a funkció jelenleg csak Azure CDN Verizon érhető el. Ez a szolgáltatás Akamai Azure CDN támogatása az elkövetkező hónapokban dolgozunk.


## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan állíthat be egy [Azure CDN-végpont egyéni tartományhoz](./cdn-map-content-to-custom-domain.md)


