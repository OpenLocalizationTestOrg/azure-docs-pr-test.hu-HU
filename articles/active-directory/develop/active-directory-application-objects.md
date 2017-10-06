---
title: "aaaAzure Active Directory-alkalmazás és szolgáltatás egyszerű objektumok |} Microsoft Docs"
description: "Egy alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directoryban hello kapcsolatát értékelése"
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
services: active-directory
editor: 
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ff7e308c0b326c3a32b101b7b323f2c0362763e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Alkalmazás és szolgáltatás egyszerű objektumok az Azure Active Directory (Azure AD)
Egyes esetekben hello szerinti hello kifejezés "alkalmazás" is böngésző, ha az Azure AD hello környezetben használják. hello Ez a cikk célja toomake tisztább fentieket olyan bemutatásáért, a regisztráció és a hozzájárulásukat adják az Azure AD alkalmazás-integráció, az elméleti és konkrét szempontok szerint, egy [több-bérlős alkalmazás](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Áttekintés
Olyan alkalmazás, amely integrálva van az Azure ad-val rendelkezik hatással vannak, amelyek hello szoftver aspektus túlmutató. "Alkalmazás" leggyakrabban fogalmi kifejezésként toonot csak hello hello alkalmazásokat, de is az Azure AD-regisztrációval és a hitelesítési/engedélyezési futásidőben a "beszélgetés" szerepkör hivatkozik. Definíció, egy alkalmazás működhet a [ügyfél](active-directory-dev-glossary.md#client-application) szerepkör (fel erőforrás), egy [erőforrás-kiszolgáló](active-directory-dev-glossary.md#resource-server) (API-k tooclients kitettségének) szerepkör, vagy akár mindkét. hello beszélgetés protokoll definiált egy [OAuth 2.0 Hitelesítésengedélyezési folyamat](active-directory-dev-glossary.md#authorization-grant), így hello ügyfél és az erőforrások tooaccess/védelme egy erőforrás-adatok kulcsattribútumokkal. Most pedig ugorjunk a mélyebb szintű, és tekintse meg, hogyan hello Azure AD alkalmazás-modell jelenti az alkalmazást tervezéskor és futásidejű. 

## <a name="application-registration"></a>alkalmazás regisztrálása
Amikor regisztrál egy Azure AD-alkalmazást a hello [Azure-portálon][AZURE-Portal], két objektum jön létre az Azure AD-bérlőn: alkalmazásobjektum, és egy szolgáltatás egyszerű objektum.

#### <a name="application-object"></a>alkalmazásobjektum
Egy Azure AD-alkalmazást határozza meg, és csak alkalmazásobjektum, ahol hello alkalmazás regisztrálva lett hello Azure AD-bérlőben található, amely annak csak egyet hello alkalmazás "otthoni" bérlői néven ismert. Azure AD Graph hello [alkalmazás entitás] [ AAD-Graph-App-Entity] hello séma az alkalmazás tulajdonságait határozza meg. 

#### <a name="service-principal-object"></a>szolgáltatás egyszerű objektum
hello szolgáltatás egyszerű objektum meghatározása hello házirendet és egy alkalmazás használatát a egy adott bérlőn, biztosító hello alapján egy biztonsági egyszerű toorepresent hello alkalmazás futásidőben. Azure AD Graph hello [szolgáltatásnév entitás] [ AAD-Graph-Sp-Entity] határozza meg a szolgáltatás egyszerű objektum tulajdonságainak hello sémáját. 

#### <a name="application-and-service-principal-relationship"></a>Alkalmazás és szolgáltatás egyszerű kapcsolat
Vegye figyelembe a hello alkalmazásobjektum hello, *globális* hello összes bérlők, és egyszerű hello szolgáltatást használni az alkalmazás ábrázolását *helyi* használatát egy adott ábrázolását Bérlői. hello alkalmazásobjektum azzal működik hello mely közös sablont és az alapértelmezett tulajdonságokat is *származtatott* megfelelő szolgáltatás egyszerű objektumok létrehozására használható. Egy alkalmazás ezért objektumnak hello alkalmazás 1:1 kapcsolatot, és a megfelelő szolgáltatás egyszerű objektumok 1:many kapcsolatot.

Mindegyik bérlő szolgáltatásnevet kell létrehozni, ahol hello alkalmazás fogja használni, lehetővé téve a bejelentkezési identitást tooestablish és/vagy a hozzáférés tooresources hello-bérlője által biztosított alatt. A bérlői egyetlen alkalmazás csak egy egyszerű (a saját otthoni bérlői), általában létre és átadni kívánt hozzájárult e használatra regisztrációja során fog rendelkezni. Egy több-bérlős webes alkalmazás/API is lesz egy egyszerű szolgáltatás létrehozása az egyes bérlők, ahol bérlőre a felhasználó hozzájárult tooits használja.  

> [!NOTE]
> Tooyour alkalmazásobjektum, hogy módosítások is megjelennek a szolgáltatás egyszerű objektumhoz hello alkalmazás otthoni bérlői csak (ahol regisztrálva lett hello bérlői). Több-bérlős alkalmazásokhoz, módosítások toohello alkalmazásobjektum nem tükröződnek az bármely fogyasztói bérlők szolgáltatás egyszerű objektumok hello hozzáférés keresztül hello eltávolításáig [alkalmazás hozzáférési Panel](https://myapps.microsoft.com) , és újra.
><br>  
> Ne feledje, hogy natív alkalmazások nyilvántartott több-bérlős alapértelmezés szerint.
> 
> 

## <a name="example"></a>Példa
hello következő diagram azt ábrázolja az alkalmazás alkalmazásobjektum és egyszerű, a több-bérlős mintaalkalmazás hello környezetében objektumok a megfelelő szolgáltatáshoz hello kapcsolatát **HR app**. Három Azure AD-bérlő szerepelnek ebben a forgatókönyvben: 

* **Adatum** – hello hello kifejlesztő hello vállalat által használt bérlői **HR-alkalmazás**
* **Contoso** – hello hello Contoso szervezet, amely felhasználóinak azon hello által használt bérlő **HR-alkalmazás**
* **Fabrikam** – hello hello is igényel, hello Fabrikam szervezet által használt bérlő **HR-alkalmazás**

![Az application objektum és a szolgáltatás egyszerű objektum közötti kapcsolat](./media/active-directory-application-objects/application-objects-relationship.png)

Hello előző ábrán az 1. lépésben az hello folyamat hello alkalmazás és szolgáltatás egyszerű objektumok létrehozása a hello alkalmazás otthoni bérlő.

2. lépésben a Contoso és Fabrikam rendszergazda jóváhagyását, befejeződése után a szolgáltatás egyszerű objektum jön létre a vállalat az Azure AD-bérlő és a hozzárendelt hello engedélyek, hogy biztosított hello rendszergazda. Vegye figyelembe azt is hello HR alkalmazást felhasználók egyéni használatra konfigurált/tervezett tooallow hozzájárulási lehet.

3. lépésben az hello HR alkalmazás (Contoso és Fabrikam) minden hello fogyasztói bérlői rendelkezik saját szolgáltatás egyszerű objektum. Minden egyes hello alkalmazás futásidőben, hello megfelelő rendszergazdai engedélyek átadni kívánt hozzájárult e hello által szabályozott egy példányának azok használatát mutatja be.

## <a name="next-steps"></a>Következő lépések
Egy alkalmazás alkalmazásobjektum elérhető hello Azure AD Graph API segítségével hello [Azure portal] [ AZURE-Portal] application manifest-szerkesztőben vagy [Azure AD PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), OData képviselt [alkalmazás entitás][AAD-Graph-App-Entity].

Egy alkalmazás szolgáltatás egyszerű objektum hello Azure AD Graph API keresztül érhető el vagy [Azure AD PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), OData képviselt [szolgáltatásnév entitás] [ AAD-Graph-Sp-Entity].

Hello [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) hasznos hello alkalmazás és a szolgáltatás egyszerű objektumok lekérdezése.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
