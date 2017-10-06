---
title: "Az Azure Active Directory B2C: Adja hozzá az AD FS-t egyéni házirendekkel SAML-Identitásszolgáltatóként"
description: "Hogyan-tooarticle a SAML protokoll és az egyéni házirendek használata az AD FS 2016 beállítása"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 30fb7700e7834e3d91fab1fc1b169b761584b204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Az Azure Active Directory B2C: Adja hozzá az AD FS-t egyéni házirendekkel SAML-Identitásszolgáltatóként

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez a cikk bemutatja, hogyan tooenable bejelentkezhet a felhasználók hello használata az AD FS-fiókból [egyéni házirendek](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Előfeltételek

Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.

Ezek a lépések az alábbiak:

1.  Az AD FS megbízható függő entitás megbízhatóságának létrehozása.
2.  Hello az AD FS megbízható függő entitás megbízhatóságának tanúsítvány tooAzure AD B2C hozzáadása.
3.  Jogcím-szolgáltató tooa házirend hozzáadása.
4.  Regisztrálásakor hello az AD FS fiók jogcím-szolgáltató tooa felhasználói út.
5.  Hello házirend tooan az Azure AD B2C feltöltése a bérlői és tesztelik azt.

## <a name="toocreate-a-claims-aware-relying-party-trust"></a>a jogcímeket figyelembe vevő Függőentitás-megbízhatóság toocreate

az AD FS toouse identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, az AD FS megbízható függő entitás megbízhatóságának toocreate kell, és adja meg azt a hello megfelelő paraméterekkel.

egy új függő entitás hello AD FS felügyelet beépülő moduljában a megbízható, és hello-beállítások manuális konfigurálásával tooadd összevonási kiszolgálók és az eljárást követő hello hajtható végre.

Tagság a **rendszergazdák**, vagy ezzel egyenértékű hello helyi számítógépen hello minimálisan szükséges toocomplete ezt az eljárást. Olvashat hello megfelelő fiókok és csoporttagságok [alapértelmezett helyi és tartományi csoportok](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  A Kiszolgálókezelőben kattintson **eszközök**, majd válassza ki **az AD FS felügyeleti**.

2.  Kattintson a **függő entitás megbízhatóságának hozzáadása**.
    ![Függő entitás megbízhatóságának hozzáadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  A hello **üdvözlő** lapon, válassza ki **kompatibilis jogcím** kattintson **Start**.
    ![A hello kezdőlapján válassza a jogcímek használatát](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  A hello **adatforrás kiválasztása** kattintson **hello függő entitással kapcsolatos adatok manuális megadása**, és kattintson a **következő**.
    ![Hello függő entitással kapcsolatos adatok megadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  A hello **megjelenített név meghatározása** írja be egy nevet a **megjelenített név**a **megjegyzések** írja be a függő entitás megbízhatóságának leírását, és kattintson **tovább** .
    ![Adja meg a megjelenítendő nevek és megjegyzések](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Választható. Ha van egy nem kötelező jogkivonat titkosítási tanúsítványt, majd hello **tanúsítvány konfigurálása** kattintson **Tallózás** toolocate a tanúsítványfájlt, és kattintson **következő** .
    ![Tanúsítvány konfigurálása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  A hello **URL konfigurálása** lapra, jelölje be hello **hello SAML 2.0 WebSSO-protokoll támogatásának engedélyezése a** jelölőnégyzetet. A **függő entitás SAML 2.0 SSO szolgáltatás URL-címe**, írja be a hello Security Assertion Markup Language (SAML) szolgáltatás végponti URL-Címének függő entitás megbízhatóságához, és kattintson **következő**.  A hello **függő entitás SAML 2.0 SSO szolgáltatás URL-címe**, illessze be a hello `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. Cserélje le a bérlő neve (például contosob2c.onmicrosoft.com) {tenant} és {házirend} hello cserélje le a bővítményeket házirend nevét (például B2C_1A_TrustFrameworkExtensions).
    > [!IMPORTANT]
    >hello házirendnév lehet hello egy öröklő signup_or_signin házirend, ebben az esetben: `B2C_1A_TrustFrameworkExtensions`.
    >Például hello URL-cím lehet: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Függő entitás SAML 2.0 SSO URL-címe](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. A hello **azonosítók konfigurálása** adja meg azokat a hello kattintson az előző lépésben hello URL-CÍMÉRE **hozzáadása** tooadd őket toohello listára, majd **következő**.
    ![Függő entitások megbízhatósági azonosítóinak](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  A hello **hozzáférés-vezérlési szabályzat kiválasztása** jelöljön ki egy házirendet, majd **következő**.
    ![Válassza ki a hozzáférés-vezérlési házirend](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  A hello **készen áll a megbízhatóság tooAdd** lapon ellenőrizze a hello beállításokat, és kattintson a **következő** toosave a függő entitás megbízhatóságának adatait.
    ![Mentse a függő entitás megbízhatósági adatok](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  A hello **Befejezés** kattintson **Bezárás**, ez a művelet automatikusan megjeleníti a hello **Jogcímszabályok szerkesztése** párbeszédpanel megnyitásához.
    ![Jogcímszabályok szerkesztése](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Kattintson a **szabály hozzáadása**.  
      ![Új szabály hozzáadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  A **Jogcímszabály-sablon**, jelölje be **LDAP attribútumok küldése jogcímekként**.
    ![Válassza ki az LDAP attribútumok küldése jogcímszabályként sablon](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Adjon meg **Jogcímszabály nevének**. A hello **attribútumtár** válasszon **válassza ki az Active Directory** adja hozzá a következő jogcímeket hello, majd kattintson az **Befejezés** és **OK**.
    ![A szabály tulajdonságainak beállítása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  A Kiszolgálókezelő ablakában válassza **függő entitás Megbízhatóságai** majd jelöljön ki hello létrehozott függő entitás megbízhatóságához, majd **tulajdonságok**.
    ![Függő entitás tulajdonságok szerkesztése](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Egy függő entitás megbízhatósági (B2C bemutató) tulajdonságai ablakban kattintson hello **aláírás** fülre, és kattintson **Hozzáadás**.  
    ![Set-aláírás](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Adja hozzá az aláírási tanúsítvány (.cert fájlt, titkos kulcs nélkül).  
    ![Az aláírási tanúsítvány hozzáadása](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  A hello függő entitás megbízhatósági (B2C bemutató) tulajdonságai ablakban kattintson a **speciális** lapon, és módosítsa a hello **biztonságos kivonatoló algoritmus** túl**SHA-1**, kattintson a **Ok**.  
    ![Állítsa be a biztonságos kivonatoló algoritmus tooSHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-hello-adfs-account-application-key-tooazure-ad-b2c"></a>Hello az AD FS fiók alkalmazás fő tooAzure AD B2C hozzáadása
Összevonás az AD FS-fiókok egy ügyfélkulcsot az AD FS fiók tootrust az Azure AD B2C hello alkalmazás nevében szükséges. Tanúsítványra van szükség toostore az AD FS az Azure AD B2C-bérlőben. 

1.  Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer**
2.  Válassza ki **házirend kulcsok** tooview hello kulcsok, amely az Ön bérelt szolgáltatásának.
3.  Kattintson a **+ Hozzáadás**.
4.  A **beállítások**, használjon **feltöltése**.
5.  A **neve**, használjon `ADFSSamlCert`.  
    hello előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.
6.  Hello fájl feltöltése, a ** válassza ki a .pfx-fájl titkos kulccsal. Megjegyzés: ezt a tanúsítványt (hello titkos kulcsával) hello azonos azt, amelyik hello az AD FS függő entitásnak történő kell lennie.
![Töltse fel a házirend-kulcs](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Kattintson a **Create** (Létrehozás) gombra
8.  Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>A jogcím-szolgáltató hozzáadása a bővítmény házirend
Ha azt szeretné, felhasználók toosign AD FS-fiókkal, toodefine az AD FS-fiók szükséges, a Jogcímszolgáltatók. Más szóval kell toospecify olyan végponttal, amely az Azure AD B2C-vel kommunikál. hello végpontja biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.

Az AD FS meghatározni, a Jogcímszolgáltatók hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:

1. A munkakönyvtár hello bővítmény házirendfájl (TrustFrameworkExtensions.xml) megnyitása. Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.
2. Hello található `<ClaimsProviders>` szakasz
3. Adja hozzá a következő XML-részletet a hello hello `ClaimsProviders` elem és a név felülírandó `identityProvider` a DNS-beli (tetszőleges érték, amely jelzi a tartomány), és mentse hello fájlt. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-adfs-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Hello az AD FS fiók jogcímeket szolgáltató tooSign regisztrálásához fel, vagy jelentkezzen be a felhasználó út
Ezen a ponton hello identitásszolgáltató be van állítva.  Azonban nincs sem hello sign-Close-Up/sign-in képernyők áll rendelkezésre. Most tooadd hello az AD FS fiók identitását szolgáltató tooyour felhasználói `SignUpOrSignIn` felhasználói út. toomake akkor érhető el, egy meglévő sablon felhasználói út duplikált létrehozhatunk.  Ezt követően azt módosítja azt, hello az AD FS identitásszolgáltató tartalmazza:
    >[!NOTE]
    >If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml) you can skip this section.
1.  Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).
2.  Hello található `<UserJourneys>` elemet, és másolja hello teljes tartalma `<UserJourneys>` csomópont.
3.  Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet. Ha hello elem nem létezik, vegyen fel egyet.
4.  Illessze be a teljes tartalma hello `<UserJournesy>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.

### <a name="display-hello-button"></a>Megjelenítési hello gomb
Hello `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások hello listáját és sorrendjét.  `<ClaimsProviderSelection>`az elem hasonló tooan identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési oldalára. Ha ad hozzá egy `<ClaimsProviderSelection>` elem az AD FS-fiók, egy új gomb megjelenik, amikor egy felhasználó fájljai hello oldalon. tooadd ezt az elemet:

1.  Hello található `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a másolt hello felhasználói út.
2.  Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`
3.  Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor

Most, hogy a gomb helyen, toolink kell azt tooan művelet. hello művelet, ebben az esetben, akkor az AD FS fiók tooreceive az Azure AD B2C toocommunicate jogkivonatot. Hivatkozás hello tooan megnyomásakor létrehozhatja, ha az AD FS fiók jogcímeket szolgáltató műszaki hello-profil:

1.  Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
2.  Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Győződjön meg arról hello `Id` hello azonos érték, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban hello.
> * Győződjön meg arról `TechnicalProfileReferenceId` toohello műszaki profil korábbi (a Contoso-egy SAML2) létrehozott van beállítva.

## <a name="upload-hello-policy-tooyour-tenant"></a>Hello házirend tooyour bérlői feltöltése
1.  A hello [Azure-portálon](https://portal.azure.com), hello átváltani [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg hello **az Azure AD B2C** panelen.
2.  Válassza ki **identitás élmény keretrendszer**.
3.  Nyissa meg hello **házirend** panelen.
4.  Válassza ki **házirend feltöltése**.
5.  Ellenőrizze **hello házirend felülírja, ha létezik** mezőbe.
6.  **Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello

## <a name="test-hello-custom-policy-by-using-run-now"></a>Tesztelje hello egyéni házirend segítségével futtatása most
1.  Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.
2.  Nyissa meg **B2C_1A_signup_signin**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet. Válassza ki **futtatása most**.
3.  Az AD FS fiókkal tud toosign kell lennie.

## <a name="optional-register-hello-adfs-account-claims-provider-tooprofile-edit-user-journey"></a>[Választható] Hello az AD FS fiók jogcímeket szolgáltató tooProfile szerkesztheti a felhasználói út regisztrálása
Érdemes lehet tooadd hello az AD FS-fiók identitásszolgáltató is tooyour felhasználói `ProfileEdit` felhasználói út. toomake teheti, hogy ismételje meg a hello utolsó két lépést:

### <a name="display-hello-button"></a>Megjelenítési hello gomb
1.  Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml) házirend.
2.  Hello található `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a másolt hello felhasználói út.
3.  Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`
4.  Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor
1.  Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
2.  Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Egyéni házirend hello a profil-módosítása tesztelése Futtatás most használatával
1.  Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.
2.  Nyissa meg **B2C_1A_ProfileEdit**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet. Válassza ki **futtatása most**.
3.  Az AD FS fiókkal tud toosign kell lennie.

## <a name="download-hello-complete-policy-files"></a>Hello teljes házirend fájlok letöltése
Nem kötelező: Javasoljuk az egyéni házirendek első lépések útmutató hello befejezése után a saját egyéni házirend fájlok használata forgatókönyv létrehozása. [Házirend mintafájlok csak referenciaként](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
