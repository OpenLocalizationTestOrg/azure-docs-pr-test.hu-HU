---
title: "Az Azure Active Directory B2C: Megjegyzések egyéni házirendek segítségével a fejlesztők számára |} Microsoft Docs"
description: "Megjegyzések fejlesztők konfigurálása és karbantartása az Azure AD B2C egyéni házirendekkel"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/05/2017
ms.author: joroja
ms.openlocfilehash: 979b8a264eb819ee4a208b9171a53a5ffbf062c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-active-directory-b2c-custom-policy-public-preview"></a>Az Azure Active Directory B2C egyéni házirend nyilvános előzetes kibocsátási megjegyzései
hello egyéni házirend szolgáltatáskészlet már elérhető az összes Azure Active Directory B2C előzetes verzióját nyilvános értékelésre (az Azure AD B2C) ügyfelek. E szolgáltatáskészlet hello legösszetettebb identitáskezelési megoldások kialakításának speciális identitás fejlesztők irányul.  

E szolgáltatáskészlet jelenleg fejlesztők tooconfigure hello identitás élmény keretrendszer keresztül közvetlenül XML-fájl szerkesztésével van szükség. A konfigurációs módszer hatékony és összetett. Identitás fejlesztők hello használata speciális identitás élmény keretrendszer kell terveznie a tooinvest útmutatók befejezése és a hivatkozás dokumentumok olvasási időt. 

## <a name="features-included-in-this-public-preview"></a>A nyilvános előzetes szereplő szolgáltatások
Hello új szolgáltatásainak hello nyilvános előzetes verziójában bevezetett a fejlesztők hello a következő feladatokat hajthatja végre:<br>

* Szerző és feltöltése az egyéni hitelesítési felhasználói utak egyéni házirendekkel. 
   * Felhasználói utak cseréjét, részletes Jogcímszolgáltatók közötti ismertetik. 
   * Határozza meg azt a felhasználói utak feltételes ugorhat. 
* Az egyéni hitelesítési felhasználói utak szolgáltatások REST API-t integrálja.  
* Adja hozzá az Identitásszolgáltatók, amelyek megfelelnek a szabványos OpenIDConnect hello összevonásának. <br>
* Adja hozzá az összevonás az identitás-szolgáltatóktól toohello SAML 2.0 protokoll igazodik. 

## <a name="terms-of-hello-public-preview"></a>Hello nyilvános előzetes feltételeit.

* Javasoljuk, toouse hello új funkciók csak tesztelési célokra.<br>
* Az új szolgáltatások nem feltétlenül egy éles környezetben való használathoz.<br>
* Szolgáltatásszint-szerződések (SLA) csak akkor érvényesíthetők toohello új funkciókat. <br>
* Támogatási kérelmek nyilvántartásához rendszeres támogatási csatornákon keresztül. <br>
* Nincs megadva általánosan rendelkezésre álló Ígért dátum.<br>
* Saját belátása szerint, és bármilyen okból Microsoft is jelzőt és elutasítása vagy forgatókönyvek és a felhasználó útvonal be, amelyek mérete meghaladja a hello Azure AD B2C termék okleveles tooserve platformként felhasználói identitások és hozzáférések kezelése (CIAM) hello hatókörének korlátozása.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Egyéni házirend szolgáltatáskészlet fejlesztők feladatai
Manuális konfigurálása az alapul szolgáló platform az Azure AD B2C alacsonyabb szintű hozzáférés toohello biztosít, és egyedi, teljes mértékben testreszabható megbízhatósági keretrendszer hello létrehozását eredményezi. Ezek lehetséges kombinációinak egyéni Identitásszolgáltatók, megbízhatósági kapcsolatokat, külső szolgáltatásokkal, és lépésről lépésre munkafolyamatok Integrációk fel őket a fejlesztők speciális hello nagyobb iránti igények kielégítése érdekében helyezze el.

toofully juttatás hello nyilvános Preview, javasoljuk, hogy a fejlesztők hello egyéni házirend szolgáltatáskészlet fel igazodik a következő irányelveket toohello:
* Ismerkedjen meg hello konfigurációs nyelvét hello identitás élmény motor és a kulcs vagy titkos kulcsok kezelése.
* Saját tulajdonba forgatókönyveket és a használatot.
* A forgatókönyv módszeres tesztek végrehajtása.
* Hajtsa végre a szoftverek fejlesztési és átmeneti legalább egy fejlesztési és tesztelési környezetben az ajánlott eljárásokról és egy éles környezetben.
* Maradjon naprakész az identitás-szolgáltatóktól hello és integrálja szolgáltatások új fejlesztéseiről. Például nyomon követheti, változások a titkos kulcsok és ütemezett és nem tervezett módosítások toohello szolgáltatás.
* Aktív figyelés beállítása, és figyelje a hello válaszképességének éles környezetekben.
* Naprakészen tartható kapcsolattartási e-mail címet, és rugalmas toohello Microsoft live hely team e-mailek maradnak.
* Intézkedéseket időben elkeverni toodo módon hello Microsoft live hely csoportja. 


>[!NOTE]
>Ezek a funkciók végül az Azure AD beépített házirendek minősítené akadálymentesített tooall fejlesztők tartalmazhatja.

## <a name="next-steps"></a>Következő lépések
[Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).
