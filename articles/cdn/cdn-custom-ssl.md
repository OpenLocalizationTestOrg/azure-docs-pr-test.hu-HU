---
title: "AAA \"egyéni Azure CDN-tartományt a HTTPS engedélyezése |} Microsoft dokumentumok\""
description: "Megtudhatja, hogyan tooenable HTTPS az Azure CDN-végpont egy egyéni tartomány."
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
ms.openlocfilehash: 93746222616c9ed7977ec3b22c38ac1d43b118f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a>Az egyéni Azure CDN-tartomány HTTPS engedélyezése

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Egyéni Azure CDN-tartomány HTTPS támogatása lehetővé teszi toodeliver biztonságos SSL használatával saját tartomány neve tooimprove hello biztonsági adatok az átvitel alatt a tartalomhoz. az egyéni tartomány hello végpontok közötti munkafolyamat tooenable HTTPS egyszerűsített keresztül egy kattintással engedélyezését, teljes tanúsítványkezelés, és minden további költség nélkül.

Kritikus tooensure hello adatvédelmi és adatok integritását a webes alkalmazások a bizalmas adatok az átvitel alatt. Az internet használatával a HTTPS protokoll biztosítja, hogy a bizalmas adatok titkosítva van, amikor keresztül továbbítja azokat hello hello. Megbízható, hitelesítés és a webes alkalmazások megvédi a támadások biztosít. Azure CDN jelenleg a CDN-végpont támogatja a HTTPS PROTOKOLLT. Például, ha a CDN-végpont létrehozása az Azure CDN (pl. https://contoso.azureedge.net), HTTPS alapértelmezés szerint engedélyezve van. Most az egyéni tartomány HTTPS, engedélyezheti a biztonságos kézbesítés egy egyéni tartomány (pl. https://www.contoso.com) is. 

Hello legfőbb attribútumai a HTTPS-szolgáltatás a következők:

- További költség nélkül: a tanúsítvány-beszerzéssel vagy megújítása nem költségekkel és a HTTPS-forgalmat további költség nélkül. Ugyanúgy kell fizetnie a hello CDN GB kilépő.

- Egyszerű engedélyezése: egy kattintson kiépítés rendszerhez elérhető a hello [Azure-portálon](https://portal.azure.com). REST API vagy egyéb fejlesztői eszközök tooenable hello szolgáltatást is használhatja.

- Fejezze be a Tanúsítványkezelő: az összes tanúsítvány beszerzésének és kezelik a kulcsokat meg. Tanúsítványok automatikusan törlődnek, és a korábbi tooexpiration megújítani. Ez teljesen eltávolítja a szolgáltatás megszakítás miatt a tanúsítvány lejárati ideje hello kockázatát.

>[!NOTE] 
>Előzetes tooenabling HTTPS PROTOKOLLT támogatja, meg kell már létrehozott egy [egyéni Azure CDN-tartomány](./cdn-map-content-to-custom-domain.md).

## <a name="step-1-enabling-hello-feature"></a>1. lépés: Hello szolgáltatás engedélyezése 

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour Verizon standard vagy prémium szintű CDN-profilt.

2. Hello végpontok, kattintson az egyéni tartomány tartalmazó hello végpont.

3. Kattintson a kívánt tooenable HTTPS hello egyéni tartományt.

    ![Végpont panel](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Kattintson a **a** tooenable HTTPS és hello módosítás mentéséhez.

    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a>2. lépés: Tartomány ellenőrzéséhez

>[!IMPORTANT] 
>Tartomány ellenőrzéséhez kell végeznie, mielőtt a saját egyéni tartománynévre HTTPS aktív lesz. 6 üzleti nap tooapprove hello tartományban van. Kérelem 6 munkanapon belül nem jóváhagyással végrehajtások leállnak.  

Miután engedélyezte az egyéni tartomány a HTTPS, a HTTPS-tanúsítvány-szolgáltatója DigiCert fogja ellenőrizni a tulajdonjogát, a tartomány lépjen kapcsolatba az hello a bejegyzés a tartományba, WHOIS bejegyzés információk, e-mailben (alapértelmezés) vagy telefonos alapján. DigiCert hello ellenőrző e-mailek toohello címek alatt is el fogja küldeni. Ha WHOIS bejegyzés információk személyes, győződjön meg arról jóváhagyhatja közvetlenül az egyik ezeknél a címeknél.

>felügyeleti @< your-tartományi-name.com > rendszergazda @< your-tartományi-name.com >  
>gazdáját @< your-tartományi-name.com >  
>hostmaster @< your-tartományi-name.com >  
>postamester @< your-tartományi-name.com >


Hello e-mail fogadásakor ellenőrzési két lehetőség közül választhat:

1. Minden további megrendelések keresztül hello hello fiókot jóváhagyhatja azonos gyökértartomány, pl. consoto.com. Ez az ajánlott módszer, ha azt tervezi, hogy további egyéni tartományok tooadd hello jövőbeli hello a legfelső szintű tartománynak.
 
2. Jóváhagyja hello adott állomás nevét, amelyet a kérés. További jóváhagyás lesz szükséges további kérelmeknél.

    Példa e-mailek:
    
    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/domain-validation-email-example.png)

A jóváhagyást követően DigiCert adja hozzá az egyéni tartomány nevét toohello SAN-tanúsítvány. egy évig hello tanúsítvány érvényes lesz, és automatikusan megújul lejárta előtt.

## <a name="step-3-wait-for-hello-propagation-then-start-using-your-feature"></a>3. lépés: Hello propagálás várja meg, majd indítsa el, a szolgáltatás használata

Hello tartománynév érvényesítését követően hello egyéni tartomány HTTPS szolgáltatás toobe aktív too6-8 órát másolatot tart. Hello folyamat befejezése után, túl "Enabled" hello "egyéni HTTPS" hello Azure-portálon állapota lesz beállítva. HTTPS-FORGALOMHOZ pedig az egyéni tartomány már készen áll a használatra.

## <a name="frequently-asked-questions"></a>Gyakori kérdések

1. *Ki hello tanúsítványszolgáltató és a használt tanúsítvány típusa?*

    Tulajdonos alternatív neve (SAN) tanúsítvány DigiCert által biztosított használjuk. SAN-tanúsítványnak biztonságossá teheti az egy tanúsítvánnyal rendelkező több teljesen minősített tartománynév.

2. *A dedikált tanúsítvány is használható?*
    
    Az on hello terv azonban jelenleg nem.

3. *Mi történik, ha a DigiCert hello tartomány megerősítési e-mailt nem érkezik?*

    Lépjen kapcsolatba a Microsofttal, ha 24 órán belül nem kap egy e-mailt.

4. *Kevésbé biztonságos, mint egy dedikált tanúsítvány SAN tanúsítványt használ?*
    
    A következő hello azonos titkosítási biztonsági szabványok gyűjteménye, egy dedikált cert SAN cert. Összes kiállított SSL-tanúsítvány SHA-256 algoritmust használ a fokozott biztonság.

5. *Használható-e az egyéni tartomány HTTPS Akamai Azure CDN?*

    Ez a funkció jelenleg csak Azure CDN Verizon érhető el. Ez a szolgáltatás Akamai Azure CDN támogató hello elkövetkező hónapokban dolgozunk.


## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan tooset be egy [Azure CDN-végpont egyéni tartományhoz](./cdn-map-content-to-custom-domain.md)


