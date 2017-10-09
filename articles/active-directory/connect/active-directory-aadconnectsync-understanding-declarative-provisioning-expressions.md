---
title: "Az Azure AD Connect: Deklaratív kiépítés kifejezéseinek |} Microsoft Docs"
description: "Hello deklaratív kiépítés kifejezéseinek ismerteti."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 516bcf1991c608d33aefc19551254d8b2bfc024f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect szinkronizálása: deklaratív kiépítés kifejezések ismertetése
Azure AD Connect szinkronizálása épít deklaratív kiépítés először a Forefront Identity Manager 2010 verzióban jelent. Tooimplement lehetővé teszi a teljes identity integration üzleti logika nélkül hello kell toowrite lefordított kódot.

Nagyon fontos részét képezik deklaratív kiépítés hello kifejezés nyelvi attribútumfolyamok használatban. hello nyelvét része a Microsoft® Visual Basic® Applications (VBA). Ezen a nyelven a Microsoft Office szolgál, és a VBScript élménye rendelkező felhasználók is felismeri azt. Deklaratív kiépítés kifejezés nyelvi hello csak funkciókat használ, és nem strukturált nyelvet. Nincsenek módszerek vagy utasításokat. Ehelyett beágyazott függvények tooexpress program folyamata.

További részletekért lásd: [üdvözli a Visual Basic toohello az alkalmazások nyelvi referencia az Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

hello attribútumok vannak erős típusmegadású. Egy függvény csak a megfelelő típusú hello attribútumokat fogad el. Egyúttal kis-és nagybetűket. Mind a nevét, és az attribútumok nevében kell rendelkeznie a megfelelő kis-és nagybetűk, vagy hiba történt.

## <a name="language-definitions-and-identifiers"></a>Nyelvi definíciókat és -azonosítók
* Funkciók argumentumokat szögletes zárójelbe követ nevet adni: (1 argumentum, N argumentum) függvénynév.
* Attribútumok szögletes zárójelbe azonosítja: [attributeName]
* Paraméterek százalékjelek azonosítja: % ParameterName %
* A karakterlánckonstansokat ajánlatok tette: például "Contoso" (Megjegyzés: Egyenes idézőjel kell használnia. "", illetve nem idézőjelek között "")
* Numerikus értékek szerint van megadva, ajánlatok és várt toobe decimális nélkül. Hexadecimális értékek fűzve előtagként & H. Például a 98052 & HFF
* Logikai értékek szerint van megadva, az állandókat: True, False.
* Beépített állandók és literálok szerint van megadva. csak a nevükkel: NULL, CRLF, IgnoreThisFlow

### <a name="functions"></a>Functions
Deklaratív kiépítés számos funkciók tooenable hello lehetőségét tootransform attribútum értékeket használja. Ezeket a funkciókat, hello eredménye egy függvényből tooanother függvénynek átadott egymásba.

`Function1(Function2(Function3()))`

hello funkciók teljes listája megtalálható hello [hivatkozás működéséhez](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Paraméterek
A paraméter PowerShell használó rendszergazda vagy egy összekötő van definiálva. Paraméterek általában tartalmaznak, amelyek eltérnek a rendszer toosystem értékek, például hello hello tartományfelhasználói hello neve található. Ezek a paraméterek attribútumfolyamok használható.

hello Active Directory-összekötő megadott hello paraméterek bejövő szinkronizálási szabályok a következő:

| Paraméter neve | Megjegyzés |
| --- | --- |
| Domain.Netbios |Hello tartomány jelenleg importált, például FABRIKAMSALES NetBIOS formátuma |
| Domain.FQDN |Hello tartomány jelenleg importált, például sales.fabrikam.com FQDN-formátumban |
| Domain.LDAP |LDAP-formátum hello tartomány jelenleg importált, például a tartományvezérlő értékesítés, DC = fabrikam, DC = = com |
| Forest.Netbios |Hello erdő neve jelenleg importált, például FABRIKAMCORP NetBIOS formátuma |
| Forest.FQDN |Hello erdő nevének jelenleg importált, fabrikam.com például FQDN-formátumban |
| Forest.LDAP |LDAP-formátum hello erdő nevének jelenleg importált, például: DC = fabrikam, DC = com |

hello rendszer biztosít hello követően paraméter, amely használt tooget hello azonosítója hello Connector jelenleg fut:  
`Connector.ID`

Íme egy példa a hello metaverzum-attribútum tartomány hello tartomány hello felhasználói hello netbios-nevét feltöltő:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operátorok
hello alábbi operátorok használhatók:

* **Összehasonlítás**: <, < =, <>, =, >, > =
* **Matematikai**: +, -, \*, -
* **Karakterlánc**: & (ÖSSZEFŰZ)
* **Logikai**: & & (és). (vagy)
* **Kiértékelési sorrend**:)

Operátorok kiértékelt bal oldali tooright és hello prioritású kiértékelése. Ez azt jelenti, hogy hello \* (szorzója) nem kerül kiértékelésre, mielőtt - (kivonás). 2\*(5 + 3) rendszer nem hello ugyanaz, mint 2\*5 + 3. hello zárójelek közé (-) használt toochange hello kiértékelési sorrend amikor bal oldali tooright kiértékelési sorrend nem megfelelő.

## <a name="multi-valued-attributes"></a>Többértékű attribútumok
hello funkciók is egyértékű és többértékű attribútumok is működik. Többértékű attribútumok hello függvény minden egyes értékhez keresztül működik, és érvényes hello olyan funkciókat tooevery érték.

Példa:  
`Trim([proxyAddresses])`Minden hello proxyAddress attribútum értékének Trim tegye.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Minden értékekre, egy @-sign, cserélje le a hello tartomány @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Keresse meg hello SIP-cím, és távolítsa el azt a hello értékeket.

## <a name="next-steps"></a>Következő lépések
* További információk a hello konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Lásd: hogyan deklaratív használt out-of-box a [ismertetése hello alapértelmezett konfiguráció](active-directory-aadconnectsync-understanding-default-configuration.md).
* Tekintse meg, hogyan toomake egy gyakorlati módosítani, használja a deklaratív kiépítés [hogyan toomake egy módosítás toohello alapértelmezett konfiguráció](active-directory-aadconnectsync-change-the-configuration.md).

**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

**Referencia-témakörei**

* [Azure AD Connect szinkronizálása: funkciók referencia](active-directory-aadconnectsync-functions-reference.md)

