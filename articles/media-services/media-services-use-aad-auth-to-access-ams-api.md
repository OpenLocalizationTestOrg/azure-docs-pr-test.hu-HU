---
title: "Azure Active Directory-hitelesítés az Azure Media Services API aaaAccess |} Microsoft Docs"
description: "További információk a fogalmakat és lépéseket tootake toouse Azure Active Directory (Azure AD) tooauthenticate hozzáférés toohello Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: bb8f75f39100dc37098260c24ab4a199ef689d46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-hello-azure-media-services-api-with-azure-ad-authentication"></a>Hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés
 
Azure Media Services API hello RESTful API. Használhatja tooperform media erőforrásokon végrehajtott műveletek a REST API használatával, vagy elérhető ügyfél SDK-k használatával. Az Azure Media Services egy Media Services-ügyfél SDK kínál a Microsoft .NET-hez. toobe tooaccess Media Services erőforrások és a Media Services API hello engedélyezett, akkor először hitelesíteni kell. 

Media Services szolgáltatásban [Azure Active Directory (Azure AD)-alapú hitelesítés](../active-directory/active-directory-whatis.md). Azure Media REST-szolgáltatást hello megköveteli, hogy hello felhasználó vagy alkalmazás, amely lehetővé teszi a REST API-kérés hello vagy hello **közreműködő** vagy **tulajdonos** szerepkör tooaccess hello erőforrásokat. További információkért lásd: [Ismerkedés a szerepköralapú hozzáférés-vezérlés az Azure-portálon hello](../active-directory/role-based-access-control-what-is.md).  

> [!IMPORTANT]
> Jelenleg a Media Services hello Azure Access Control service hitelesítési modellt támogatja. Azonban a 2018. június 1 elavulttá válik hozzáférés-vezérlés engedélyt. Azt javasoljuk, hogy az áttelepített toohello az Azure AD hitelesítési modell lehető legrövidebb időn belül.

Ez a dokumentum áttekintést hogyan tooaccess hello REST vagy a .NET API-k használatával Media Services API.

## <a name="access-control"></a>Hozzáférés-vezérlés

Hello Azure Media REST kérelem toosucceed hello hívó felhasználói hello tooaccess próbál Media Services-fiók közreműködői vagy a tulajdonos szerepkört kell rendelkeznie.  
Csak hello tulajdonos szerepkörrel rendelkező felhasználó media erőforrás (fiók) hozzáférési toonew felhasználók vagy alkalmazások biztosíthat. hello közreműködői szerepkör elér csak hello média-erőforrás.
Jogosulatlan kérelem sikertelen lesz, a 401-es állapotkóddal. Ha hibaüzenet jelenik meg, ellenőrizze, hogy a felhasználó van-e a hello közreműködő vagy a tulajdonos szerepkörrel hello felhasználói Media Services-fiók. Az Azure-portálon hello ellenőrizheti. Keresse meg az adathordozó-fiókját, és kattintson a hello **hozzáférés-vezérlés** fülre. 

![Hozzáférés-vezérlési lap](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Típusú hitelesítés 
 
Az Azure AD-alapú hitelesítés használatakor az Azure Media Services hitelesítési két lehetőség közül választhat:

- **Felhasználó hitelesítése**. Egy Media Services-erőforrásokkal hello app toointeract használó felhasználó hitelesítéséhez. hello interaktív alkalmazás először kell hello felhasználók hello felhasználó hitelesítő adatainak kéréséhez. Például egy engedéllyel rendelkező felhasználók toomonitor kódolási feladatok által használt felügyeleti konzol alkalmazás vagy élő adatfolyam. 
- **Szolgáltatás egyszerű hitelesítési**. A szolgáltatás hitelesítéséhez. Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok futtatásához. Többek között a webalkalmazások, függvény alkalmazások, a logic apps, API és mikroszolgáltatások létrehozására.

### <a name="user-authentication"></a>Felhasználóhitelesítés 

Hello felhasználói hitelesítési módszert kell használó alkalmazások: a felügyeleti vagy a natív alkalmazások figyeléséről: mobilalkalmazások, a Windows-alkalmazások és a konzol alkalmazások. Az ilyen típusú megoldást akkor hasznos, ha azt szeretné, hogy valamelyik a következő forgatókönyvek hello hello szolgáltatásban emberi beavatkozást igényel:

- Figyelési irányítópult a kódolási feladatokhoz.
- Az élő adatfolyamok a figyelési irányítópulton.
- Az asztali és mobil felhasználók tooadminister erőforrások a Media Services-fiók alkalmazás kezelése.

> [!NOTE]
> Ezt a hitelesítési módszert felhasználók felé néző alkalmazásokban kell nem használhatók. 

Egy natív alkalmazás kell először szerezni az Azure ad-hozzáférési tokent, és használja azt meg, hogy a HTTP kérelmeket toohello Media Services REST API-t. Adja hozzá a hello access token toohello kérelemfejlécet. 

a következő diagram hello egy interaktív alkalmazások hitelesítési folyamat jeleníti meg: 

![Natív alkalmazások diagramja](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

A fenti diagram hello hello számok jelölik hello folyamata hello kérelmek időrendi sorrendben.

> [!NOTE]
> Hello felhasználói hitelesítési módszer használata esetén minden alkalmazás megosztása hello natív alkalmazás ügyfél-azonosító ugyanazon (alapértelmezett) és a natív alkalmazás átirányítási URI-címe. 

1. A felhasználók hitelesítő adatainak kéréséhez.
2. Egy Azure AD hozzáférési jogkivonat a következő paraméterek hello. kérelem:  

    * Azure AD-bérlő végpont.

        hello bérlői adatait lekérhetők hello Azure-portálon. Vigye a kurzort keresztül hello bejelentkezett felhasználó neve hello hello jobb felső sarokban található.
    * A Media Services erőforrás URI azonosítója. 

        Ezt az URI van hello azonos hello a Media Services-fiókok Azure ugyanabban a környezetben (például https://rest.media.azure.net).

    * A Media Services (natív) alkalmazás ügyfél-azonosító.
    * Media Services (natív) alkalmazás átirányítási URI-címe.
    * Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz.
        
        hello URI hello REST API-végpont (például https://test03.restv2.westus.media.azure.net/api/) jelöli.

    Ezek a paraméterek értékeit tooget lásd [hello Azure portál tooaccess az Azure AD hitelesítési beállítások](media-services-portal-get-started-with-aad.md) hello felhasználói hitelesítés beállítás használatával.

3. hello Azure AD hozzáférési jogkivonat toohello ügyfél küldött.
4. hello ügyfél küldi hello Azure AD hozzáférési jogkivonatok kérelem toohello Azure Media REST API-t.
5. hello ügyfél vissza hello adatok lekérése a Media Services.

További információ a hogyan toouse az Azure AD hitelesítési toocommunicate többi hello ügyfél-Media Services .NET SDK használatával kér: [használja az Azure AD hitelesítési tooaccess hello Media Services API a .NET](media-services-dotnet-get-started-with-aad.md). 

Ha nem használ hello Media Services .NET ügyfél SDK, manuálisan kell létrehoznia az Azure AD hozzáférési jogkivonat kérelem a 2. lépésben leírt hello paraméterek használatával. További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Egyszerű szolgáltatásnév hitelesítése

Ezt a hitelesítési módszert gyakran használó alkalmazások olyan alkalmazások, amelyeket a középső rétegbeli szolgáltatások és az ütemezett feladatok futtatásához: web apps, függvény alkalmazások, a logic apps, API-k és mikroszolgáltatások létrehozására. Ezt a hitelesítési módszert is alkalmas lehet, hogy ki toouse a szolgáltatás fiók toomanage erőforrásainak interaktív alkalmazások.

Hello szolgáltatás egyszerű hitelesítési módszer toobuild fogyasztói forgatókönyveket használatakor hitelesítési általában kell kezelni a középső réteg hello (néhány API-n) keresztül, és nem közvetlenül a hordozható vagy asztali alkalmazás. 

toouse ezzel a módszerrel hozzon létre egy Azure AD-alkalmazást, és a saját bérlőt az egyszerű szolgáltatás. Hello alkalmazás létrehozása után adjon hello app közreműködő vagy a tulajdonos szerepkör hozzáférés toohello Media Services-fiók. Ehhez a hello Azure-portálon az Azure parancssori felület használatával, vagy egy PowerShell-parancsfájlt. Egy Azure AD-alkalmazást is használhatja. Regisztrálhat és kezeli az Azure AD alkalmazás- és egyszerű szolgáltatás [hello Azure-portálon a](media-services-portal-get-started-with-aad.md). Is ehhez a [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) vagy [PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Középső rétegbeli alkalmazások](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Miután létrehozta az Azure AD-alkalmazást, a következő beállítások hello beolvasása értékek. A hitelesítéshez kell ezeket az értékeket:

- Ügyfél-azonosító 
- Titkos ügyfélkulcs 

A fenti ábra hello hello számok hello kérelmek időrendben hello áramló mutatják be:
    
1. A középső rétegbeli alkalmazás (webes API-t vagy webalkalmazás) egy Azure AD hozzáférési jogkivonatot, amely rendelkezik a következő paraméterek hello kéri:  

    * Azure AD-bérlő végpont.

        hello bérlői adatait lekérhetők hello Azure-portálon. Vigye a kurzort keresztül hello bejelentkezett felhasználó neve hello hello jobb felső sarokban található.
    * A Media Services erőforrás URI azonosítója. 

        Ezt az URI van hello azonos hello találhatók a Media Services-fiókok Azure ugyanabban a környezetben (például https://rest.media.azure.net).

    * Erőforrás URI azonosítója a többi Media Services szolgáltatásokhoz.

        hello URI hello REST API-végpont (például https://test03.restv2.westus.media.azure.net/api/) jelöli.

    * Az Azure AD alkalmazás értékek: hello ügyfél-azonosító és a titkos ügyfélkódot.
    
    Ezek a paraméterek értékeit tooget lásd [hello Azure portál tooaccess az Azure AD hitelesítési beállítások](media-services-portal-get-started-with-aad.md) hello szolgáltatás egyszerű hitelesítés lehetőséget.

2. hello Azure AD hozzáférési jogkivonat toohello középső réteg zajlik.
4. hello középső réteg küld kérelmet toohello Azure Media REST API hello Azure AD-tokenhez.
5. hello középső réteg vissza hello adatok lekérése a Media Services.

Hogyan toouse az Azure AD hitelesítési toocommunicate többi kérelmek hello ügyfél-Media Services .NET SDK használatával kapcsolatos további információkért lásd: [használja az Azure AD hitelesítési tooaccess Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md). 

Ha nem használ hello Media Services .NET ügyfél SDK, manuálisan kell létrehoznia az Azure AD-jogkivonatkérelem az 1. lépésben leírt paraméterek használatával. További információkért lásd: [hogyan toouse hello Azure AD Authentication Library tooget hello Azure AD-jogkivonat](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Hibaelhárítás

Kivétel: "hello távoli kiszolgáló hibát adott vissza: (401) nem engedélyezett."

Megoldás: Hello Media Services REST kérelem toosucceed hello hívó felhasználónak kell hello tooaccess próbál Media Services-fiók közreműködői vagy a tulajdonos szerepet. További információkért lásd: hello [hozzáférés-vezérlés](media-services-use-aad-auth-to-access-ams-api.md#access-control) szakasz.

## <a name="resources"></a>Erőforrások

a következő cikkek hello az Azure AD hitelesítési fogalmak áttekintése: 

- [Az Azure AD által érintett hitelesítési forgatókönyvek](../active-directory/develop/active-directory-authentication-scenarios.md#basics-of-authentication-in-azure-ad)
- [Hozzáadása, módosítása vagy az Azure AD alkalmazás eltávolítása](../active-directory/develop/active-directory-integrating-applications.md)
- [Konfigurálja és a szerepköralapú hozzáférés-vezérlés kezelése a PowerShell használatával](../active-directory/role-based-access-control-manage-access-powershell.md)

## <a name="next-steps"></a>Következő lépések

* Hello Azure-portált használja túl[elérni az Azure AD hitelesítési tooconsume Azure Media Services API](media-services-portal-get-started-with-aad.md).
* Az Azure AD-hitelesítés használatára túl[elérni az Azure Media Services API a .NET](media-services-dotnet-get-started-with-aad.md).

