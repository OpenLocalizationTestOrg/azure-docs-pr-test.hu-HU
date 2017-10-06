---
title: "aaaHow tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások"
description: "Ismerteti, hogyan toouse az Azure AD-alkalmazásproxy tooprovide biztonságos távoli hozzáférés tooyour a helyszíni alkalmazások."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 289e970ed0596fcd06ccf6b2ad92203366fbb494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprovide-secure-remote-access-tooon-premises-applications"></a>Hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások

Az alkalmazottak Ma szeretné toobe hatékony bárhol, bármikor, bármilyen eszközről. Legyenek azok táblagépek, telefonok vagy hordozható kívánják a saját eszközeik toowork. És várható toobe képes tooaccess az alkalmazásaikat, SaaS-alkalmazások hello felhőben és a vállalati alkalmazásokhoz a helyszínen. Hozzáférés biztosítása tooon helyszíni alkalmazások rendelkezik hagyományosan részt vevő virtuális magánhálózatok (VPN) vagy demilitarizált zóna (DMZ-k). Nem csak a megoldások összetett és rögzített toomake biztonságos, de ezek költséges tooset fel és kezelheti.

Nincs fejlettebb módszert!

A modern munkaerő igényeire mobile-első hello, world cloud-első modern távelérési megoldásra van szükségük. Az Azure AD-alkalmazásproxy egy olyan szolgáltatás, az Azure Active Directory szolgáltatás távoli hozzáférést biztosít. Ez azt jelenti, hogy könnyen toodeploy, használatát és kezelését.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Mi az Azure Active Directory Alkalmazásproxyjával?
Az Azure AD-alkalmazásproxy biztosítja az egyszeri bejelentkezés (SSO) és a webes alkalmazások biztonságos távoli hozzáférést a helyben tárolt. Néhány érdemes toopublish alkalmazások közé tartoznak az SharePoint helyek, az Outlook Web Access vagy bármely más LOB webalkalmazások rendelkezik. Ezeket a helyszíni webes alkalmazások integrálva vannak az Azure ad-vel, ugyanazzal az identitással hello, és Office 365 által használt platform szabályozza. A végfelhasználók férhetnek hozzá a helyszíni alkalmazások hello ugyanúgy hozzáférnek az Office 365 és az egyéb SaaS-alkalmazásokhoz az Azure ad-vel integrált. Nem kell toochange hello hálózati infrastruktúra, vagy a felhasználók VPN tooprovide ehhez a megoldáshoz szükséges.

## <a name="why-is-application-proxy-a-better-solution"></a>Miért érdemes az alkalmazásproxy jobb megoldás?
Az Azure AD-alkalmazásproxy lehetővé teszi a távoli elérés egyszerű, biztonságos és költséghatékony megoldást tooall a helyszíni.

Az Azure AD-alkalmazásproxy van:

* **Egyszerű**
   * Nem kell toochange, vagy frissítse az alkalmazások toowork az alkalmazásproxy. 
   * A felhasználók egy egységes hitelesítési élmény beolvasása. Használhatnak MyApps portál tooget egyszeri bejelentkezés hello tooboth SaaS-alkalmazások hello felhőben és az alkalmazásokhoz a helyszínen. 
* **Biztonságos**
   * Az Azure AD alkalmazásproxy használó alkalmazások közzétételekor kihasználhatja a hello gazdag engedélyezési vezérlők és biztonsági analytics az Azure-ban. A felhőméretű biztonsági és az Azure biztonsági szolgáltatások, mint a feltételes hozzáférés és a kétlépéses ellenőrzés kapják meg.
   * Nincs tooopen a bejövő kapcsolatokat a tűzfal toogive keresztül a felhasználók távoli hozzáférést. 
* **Költséghatékony**
   * Alkalmazásproxy működik hello felhőben, így időt és pénzt menthető. A helyszíni megoldások általában mentése tooset megkövetelik, és DMZ-k, a peremhálózati kiszolgálóinak vagy más összetett infrastruktúra karbantartása.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Milyen típusú alkalmazások munkahelyi az alkalmazásproxy?
Az Azure AD alkalmazásproxy különböző típusú belső alkalmazás érhető el:

* Webes használó alkalmazások [integrált Windows-hitelesítés](active-directory-application-proxy-sso-using-kcd.md) hitelesítéshez  
* Webes űrlap alapú használó alkalmazások vagy [fejléc-alapú](application-proxy-ping-access.md) hozzáférés  
* Webes API-k, amelyet az tooexpose toorich alkalmazások különböző eszközökön  
* Alkalmazások mögött található egy [távoli asztali átjáró](application-proxy-publish-remote-desktop.md)  
* Gazdag ügyfél alkalmazásokat, amelyek integrálhatók a hello Active Directory Authentication Library (ADAL)

## <a name="how-does-application-proxy-work"></a>Hogyan működik az alkalmazásproxy?
Két összetevőit telepíteni kell, amely tooconfigure toomake proxyval munkahelyi: összekötő és a külső végpont. 

hello összekötő egy egyszerűsített ügynök, amely a Windows Server a hálózaton belül helyezkedik el. hello összekötő hello forgalom áramlását az alkalmazásproxy-szolgáltatás hello felhő tooyour alkalmazás helyszíni hello könnyíti meg. Kimenő kapcsolatok csak így nem rendelkezik tooopen bejövő portra vagy bármi be hello DMZ használ. hello összekötők állapot nélküli, és szükség szerint hello felhőből információkat le. Összekötők, például hogy miként kapcsolatos további információk azok terheléselosztásához és elvégezni a hitelesítést, lásd: [megértéséhez Azure AD-alkalmazásproxy összekötők](application-proxy-understand-connectors.md). 

hello külső végpont esetében, hogyan a felhasználók elérését a hálózaton kívül az alkalmazások. Ezek vagy folytathatja közvetlenül tooan külső URL-címet, akkor határozza meg, vagy hello MyApps portálon keresztül hello alkalmazás eléréséhez. Amikor a felhasználók nyissa meg a fenti végpontok tooone, hitelesítéséhez az Azure ad-ben, és majd hello összekötő toohello helyszíni alkalmazáson keresztül továbbítódnak.

 ![Alkalmazásproxy AzureAD diagramja](./media/active-directory-application-proxy-get-started/azureappproxxy.png)

1. hello felhasználó hello alkalmazásproxy-szolgáltatás keresztül fér hozzá a hello alkalmazást, és irányított toohello az Azure AD bejelentkezési oldalára tooauthenticate.
2. Egy sikeres bejelentkezés, miután egy token előállítja, és elküldi a toohello ügyféleszköz.
3. hello ügyfél küldi hello token toohello alkalmazásproxy-szolgáltatás, amely lekéri a hello egyszerű felhasználónév (UPN) és biztonsági egyszerű szolgáltatásnév (SPN) hello jogkivonatból, majd arra utasítja a hello kérelem toohello alkalmazásproxy-összekötő.
4. Ha egyszeri bejelentkezésre van beállítva, a hello összekötő hello felhasználó nevében szükséges további hitelesítést hajt végre.
5. hello összekötő hello kérelem toohello a helyszíni alkalmazások küld.  
6. hello válasz alkalmazásproxy-szolgáltatás és -összekötő toohello felhasználói keresztül küld el.

### <a name="single-sign-on"></a>Egyszeri bejelentkezés
Az Azure AD-alkalmazásproxy egyszeri bejelentkezés (SSO) tooapplications, integrált Windows-hitelesítéssel (IWA), vagy a jogcímbarát alkalmazásokhoz biztosít. Ha az alkalmazás integrált Windows-Hitelesítést használ, akkor alkalmazásproxy használata a Kerberos által korlátozott delegálás tooprovide SSO hello felhasználót megszemélyesítő. Ha egy megjelölt jogcímbarát alkalmazáshoz, amely megbízik az Azure Active Directory, egyszeri Bejelentkezéses működik, mivel a hello felhasználó már Azure ad hitelesítési.

Kerberos kapcsolatos további információkért lásd: [minden kívánt tooknow kapcsolatos Kerberos által korlátozott delegálás (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Alkalmazások felügyelete
Az alkalmazás egyik közzéteszi az alkalmazásproxy, mint bármely más vállalati alkalmazás hello Azure-portálon kezelheti. Használja az Azure Active Directory biztonsági funkciók, például a feltételes hozzáférés és a kétlépéses ellenőrzést, szabályozhatja a felhasználói engedélyek, és testre szabhatja az alkalmazás branding hello. 

## <a name="get-started"></a>Bevezetés

Alkalmazásproxy konfigurálása előtt ellenőrizze, hogy a támogatott [Azure Active Directory edition](https://azure.microsoft.com/pricing/details/active-directory/) és az Azure AD-címtár, amelynek globális rendszergazdája.

Ismerkedés az alkalmazásproxy két lépésben:

1. [Hello összekötő alkalmazásproxy engedélyezése és konfigurálása](active-directory-application-proxy-enable.md).    
2. [Alkalmazások közzététele](active-directory-application-proxy-publish.md) -használata hello gyorsan és egyszerűen varázsló tooget a helyszíni alkalmazások közzétéve és nem érhető el távolról.

## <a name="whats-next"></a>A következő lépések
Miután az első alkalmazás közzétételéhez nincs sokkal több alkalmazásproxy teheti:

* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
* [Alkalmazások közzététele saját tartománynév használatával](active-directory-application-proxy-custom-domains.md)
* [További tudnivalók az Azure AD-alkalmazásproxy-összekötő](application-proxy-understand-connectors.md)
* [Meglévő helyszíni Proxy kiszolgálók használata](application-proxy-working-with-proxy-servers.md) 
* [Állítsa be egy egyéni kezdőlap](application-proxy-office365-app-launcher.md)

Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)

