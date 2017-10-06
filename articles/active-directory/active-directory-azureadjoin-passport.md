---
title: "aaaAuthenticating identitások keresztül üzleti és az Azure AD a Windows Hello-jelszó nélkül |} Microsoft Docs"
description: "Áttekintést nyújt a Windows Hello üzleti és a vállalati Windows Hello telepítésével kapcsolatos további információkat."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: f907bb90-8776-46ca-9e12-279949af66ff
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 7c1c52e10b7ab7a89ec3226ffa7cf01896267871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-identities-without-passwords-through-windows-hello-for-business"></a>A vállalati Windows Hello jelszó nélkül identitások hitelesítése
hello aktuális önmagában jelszavakkal hitelesítési módszereket nem elegendő tookeep felhasználók biztonságos. A felhasználók használja fel, és elfelejti a jelszavát. Jelszavak breachable, phishable, nagyon eséllyel fordulnak elő toocracks és kitalálható. Is nehéz tooremember lekérni, és hibalehetőségeket rejt magában tooattacks, például "[hello kivonatoló átadni](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-windows-hello-for-business"></a>A vállalati Windows Hello kapcsolatos
Vállalati Windows Hello a saját vagy nyilvános kulcs- vagy tanúsítványalapú hitelesítési módszert alkalmaz a szervezetek és a fogyasztók, amely jelszavak túllép. Az űrlap-hitelesítés kulcspár hitelesítő adatokat, amelyek lecserélheti a jelszavak és ellenállnak toobreaches thefts és adathalászat támaszkodik.

 Vállalati Windows Hello lehetővé teszi, hogy a felhasználó hitelesítésére tooa Microsoft-fiókkal, egy Windows Server Active Directory-fiókot, a Microsoft Azure Active Directory (Azure AD-) fiók vagy egy nem Microsoft-szolgáltatás, amely támogatja a gyors identitás Online (FIDO) hitelesítést. Után egy kezdeti kétlépéses ellenőrzés során a Windows Hello üzleti regisztrálásra a vállalati Windows Hello hello felhasználói eszközön be van állítva, és hello felhasználó állítja be a hitelesítési mód, amely a Windows Hello vagy PIN-kód. hello felhasználói biztosít hello kézmozdulat tooverify az identitásukat. A Windows majd Windows Hello üzleti tooauthenticate hello felhasználó használ, és segítséget nyújthatnak a támogatási tooaccess védett erőforrások és szolgáltatások.

hello titkos kulcs keresztül teszik elérhetővé kizárólag a "felhasználói hitelesítési mód" például a PIN-kód, biometrikus vagy egy távoli eszköz például az intelligens kártya, amely a felhasználó hello toohello eszköz toosign használja. Ez az információ a csatolt tooa tanúsítvány vagy egy aszimmetrikus kulcspár. hello titkos kulcsot, ha hello eszköznek egy platformmegbízhatósági modul (TPM) csipje igazolt hardver. titkos kulcs hello sosem hagyja el hello eszközt.

nyilvános kulcs hello regisztrálva van az Azure Active Directory és a Windows Server Active Directory (helyszíni). Identitás-szolgáltatóktól (IDPs) hello felhasználó érvényesítése a leképezési hello nyilvános kulcsával hello felhasználói toohello titkos kulcsot, és adjon meg egy alkalommal jelszót (OTP), a PhoneFactor vagy egy másik értesítési mechanizmus bejelentkezési adatait.

## <a name="why-enterprises-should-adopt-windows-hello-for-business"></a>Miért vállalatok el kell fogadnia vállalati Windows Hello
Engedélyezésével vállalati Windows Hello, vállalatok is erőforrásaik még biztonságosabbá teszi szerint:

* Beállítása vállalati Windows Hello hardver előnyben részesített módszerrel. Ez azt jelenti, hogy kulcsokat generál a TPM 1.2-es vagy a TPM 2.0, ha elérhető. Ha nem érhető el TPM, a szoftver hello kulcsot hoz létre.
* Definiáló hello összetettségét és hello hosszát a PIN-kód, és Hello használata engedélyezve van-e a szervezetében.
* Konfigurálása a Windows Hello üzleti toosupport intelligenskártya-szerű forgatókönyvek tanúsítványalapú bizalmi kapcsolat használatával.

## <a name="how-windows-hello-for-business-works"></a>Vállalati Windows Hello működése
1. Kulcsok TPM vagy szoftver által előállított hello hardveren. Sok eszköz rendelkezik beépített TPM lapka biztonságossá tételére hello hardver titkosítási kulcsok integrálása eszközök. A TPM 1.2-es vagy TPM 2.0 hoz létre kulcsokat vagy tanúsítványokat a generált hello kulcsok létrehozott.
2. hello TPM igazolja a hardver adathoz kötött kulcsokat.
3. Egyetlen unlock kézmozdulatok feloldja hello eszköz. A hitelesítési mód lehetővé teszi, hogy toomultiple erőforrások eléréséhez Ha hello eszköz tartományhoz vagy az Azure AD-tartományhoz.

## <a name="how-hello-windows-hello-for-business-lifecycle-works"></a>Hogyan működik a Windows Hello hello az üzleti életciklusa
![A Windows Hello-üzleti életciklusa](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

hello előző ábrán látható, hello személyes/nyilvános kulcsból álló kulcspárt és hello érvényesítési hello identitásszolgáltatótól. A lépések van részletes magyarázatához lásd itt:

1. hello felhasználói igazolja az identitásukat több beépített ellenőrző módszerek (kézmozdulatok, fizikai intelligens kártyával, a multi-factor authentication) keresztül, és elküldi az adatokat tooan Identity Provider (IDP) például az Azure Active Directory vagy a helyszíni Active Directory.
2. hello eszköz majd hello kulcsot hoz létre, hello kulcs igazolja, ez a kulcs nyilvános részének hello vesz igénybe, csatolja az állomás utasítások, jelentkezik be, és elküldi toohello IDP tooregister hello kulcs.
3. Amint hello IDP hello kulcs nyilvános részének hello regisztrálja, hello IDP kihívást hello eszköz toosign a hello hello kulcs titkos része.
4. hello IDP szerint hitelesíti, és problémák hello hitelesítési jogkivonat, amely lehetővé teszi, hogy a felhasználó hello és hello eszköz hello védett erőforrások eléréséhez. IDPs platformfüggetlen alkalmazások írása vagy használhatja a böngésző támogatás (keresztül JavaScript/Webcrypto API-k) toocreate és használja a Windows Hello a vállalati hitelesítő adatok a felhasználók számára.

## <a name="hello-deployment-requirements-for-windows-hello-for-business"></a>a vállalati Windows Hello hello központi telepítésére vonatkozó követelmények
### <a name="at-hello-enterprise-level"></a>Hello vállalati szinten
* hello vállalati Azure-előfizetéssel rendelkezik.

### <a name="at-hello-user-level"></a>Hello felhasználói szinten
* hello felhasználó futtatja a Windows 10 Professional és Enterprise.

Részletes telepítési utasításokért lásd: [hello szervezet vállalati Windows Hello engedélyezése](active-directory-azureadjoin-passport-deployment.md).

## <a name="additional-information"></a>További információ
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

