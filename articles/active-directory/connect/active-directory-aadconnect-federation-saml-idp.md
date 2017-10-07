---
title: "Az Azure AD Connect: Használja egyszeri bejelentkezéshez a SAML 2.0-s identitásszolgáltató az |} Microsoft Docs"
description: "Ez a témakör ismerteti az egyszeri bejelentkezéshez a SAML 2.0-s megfelelő Idp használ."
services: active-directory
author: billmath
manager: femila
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f9653dc44fb284a9b3c1988f623c33f27ae148cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Egyszeri bejelentkezéshez a SAML 2.0-Identitásszolgáltatóként (IdP) használata

Ez a témakör ismerteti a SAML 2.0 használatával megfelelő SP-Lite profil alapjául identitásszolgáltató hello elsődleges biztonsági jogkivonat szolgáltatás (STS), vagy identitásszolgáltató. Ez akkor hasznos, ha már rendelkezik a felhasználó és a jelszót a helyszíni SAML 2.0 használatával elérhető tárolja. A meglévő felhasználói könyvtár használható bejelentkezésre tooOffice 365 és más Azure AD által védett erőforrásokhoz. hello SAML 2.0 SP-Lite profil alapuló hello széles körben használt a Security Assertion Markup Language (SAML) összevont identitás szabványos tooprovide a bejelentkezéshez és az exchange keretrendszerével.

>[!NOTE]
>3. fél listája, amely az Azure ad-val való használatra lettek tesztelve Idps: hello [az Azure AD összevonás kompatibilitási listája](active-directory-aadconnect-federation-compatibility.md)

A Microsoft támogatja a bejelentkezést, hello integrációja egy Microsoft felhőszolgáltatásra, például az Office 365, a megfelelően konfigurált SAML 2.0-s profil IdP alapján. SAML 2.0 identitás-szolgáltatóktól harmadik féltől származó termékekre, és ezért a Microsoft nem támogatást nyújt a hello telepítési, konfigurációs, ajánlott eljárások ezekkel kapcsolatos hibaelhárítás. Egyszer megfelelően konfigurálva, hello hello identitásszolgáltató tesztelhető a megfelelő konfigurációt hello Microsoft kapcsolódási Analyzer eszköz, amely az alábbi további részletes leírását lásd a SAML 2.0 integrációja. További információ a SAML 2.0 SP-Lite profil alapján identitásszolgáltató kérje meg a hello olyan szervezet, amely megadja azt.

>[!IMPORTANT]
>Csak korlátozott számú ügyféllel érhető el, a bejelentkezés forgatókönyvet, és a SAML 2.0 Identitásszolgáltatók, ehhez a következőket:

>- Web-alapú ügyfelek, például az Outlook Web Access és a SharePoint online-hoz
- E-mailek gazdag használó ügyfeleket az egyszerű hitelesítés és a támogatott Exchange hozzáférési módszer például az IMAP-, POP-, Active Sync, MAPI, stb. (hello bővített ügyfél protokoll végpontját szükséges toobe telepítve), többek között a következőket:
    - A Microsoft Outlook 2010 vagy az Outlook 2013/Outlook 2016, Apple iPhone (különböző iOS verzió)
    - Különféle Google Android-eszközök
    - Windows Phone 7, Windows Phone 7,8 és Windows Phone 8.0
    - Levelezőprogram Windows 8 és Windows 8.1 levelezőprogram
    - Windows 10 levelezőprogram

Más ügyfelek a bejelentkezés forgatókönyvet, és a SAML 2.0 identitásszolgáltató nem érhetők el. Például hello Lync 2010 asztali ügyfél nincs képes toologin hello szolgáltatásban a SAML 2.0 identitásszolgáltatóval egyszeri bejelentkezés beállítása.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Azure AD SAML 2.0 protokoll követelmények
Ez a témakör részletes követelményeket hello protokoll és formázás, hogy a SAML 2.0 identitás szolgáltatónak tartalmaznia kell az Azure AD tooenable bejelentkezés tooone toofederate vagy több Microsoft felhőszolgáltatás (például az Office 365) üzenet. hello SAML 2.0 függő entitás (SP-STS) egy Microsoft felhőszolgáltatásra, ebben a forgatókönyvben használt az Azure AD.

Javasoljuk, hogy ellenőrizze-e az identity provider kimeneti üzenetek kell minta nyomkövetések megadott lehető hasonló toohello, SAML 2.0. A megadott hello Azure AD-metaadatok adott attribútumértéket is, használ, ahol csak lehetséges. Ha elégedett a kimeneti üzenetek, tesztelheti a Microsoft kapcsolat Analyzer hello az alább ismertetett.

az Azure AD hello metaadatok tölthető le: az URL-cím: [https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml](http://https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
Az ügyfelek Kína használatával hello Office 365 Kína-specifikus példányát, a következő összevonási végpont hello kell használni: [https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>SAML protokoll követelmények
Ez a szakasz részletek hogyan hello kérelem-válasz üzenet párok vannak hozzáfoghat a rendelés toohelp meg tooformat az üzenetek megfelelően.

Az Azure AD a SAML 2.0 SP Lite hello-profil használata egyes konkrét követelmények, az alább felsorolt identitás-szolgáltatóktól konfigurált toowork lehet. Példa SAML kérelem-válasz köszönőüzenetei automatikus és manuális tesztelési együtt használva tooachieve együttműködés az Azure ad szolgáltatással működik.

## <a name="signature-block-requirements"></a>Aláírás blokk követelmények
Belül hello SAML-válasz az üzenet hello aláírás csomópont köszönőüzenetei magára hello digitális aláírás információkat tartalmaz. hello aláírásblokkot rendelkezik hello követelményeknek:

1. hello helyességi feltétel a csomópont önmaga rendelkeznie kell aláírással.
2.  hello DigestMethod értékét hello RSA-sha1 algoritmust kell használhat. Más digitális aláírási algoritmus használata nem engedélyezett.
   `<ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  Előfordulhat, hogy is regisztrál hello XML-dokumentumot. 
4.  hello átalakító algoritmus meg kell egyeznie a következő minta hello hello értékek:`<ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  hello SignatureMethod algoritmus meg kell egyeznie a következő minta hello:`<ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Támogatott kötések
Kötések hello átviteli kapcsolatos kommunikációs paramétereket, amelyek szükségesek. a következő követelmények hello alkalmazása toohello kötések

1. HTTPS szükséges hello átviteli.
2.  Az Azure AD HTTP POST szüksége van a bejelentkezéskor token küldésének
3.  Az Azure AD hello hitelesítési kérelem toohello identitásszolgáltató és ÁTIRÁNYÍTÁSI HTTP POST hello kijelentkezési üzenet toohello identitásszolgáltató használja.

## <a name="required-attributes"></a>Szükséges attribútumok
Az alábbi táblázatban a SAML 2.0 üdvözlőüzenetére követelmények azokat az attribútumokat.
 
|Attribútum|Leírás|
| ----- | ----- |
|NameID|a helyességi feltétel hello értéke ugyanaz, mint a hello Azure AD-felhasználó ImmutableID kell hello. Ez lehet a too64 alfanumerikus karakterek fel. A HTML nem biztonságos karaktereket kell kódolni, például a "+" karaktert jelenik meg, mint ".2B".|
|IDPEmail|hello egyszerű felhasználónév (UPN) szerepel a hello SAML választ, egy a hello elemnév IDPEmail ez: hello felhasználói UserPrincipalName (UPN) az Azure AD vagy Office 365-ben. hello egyszerű felhasználónév az e-mail cím formátumú. Egyszerű felhasználónév értéke Windows Office 365 (az Azure Active Directory).|
|Kiállító|Ez azért szükség toobe hello identitásszolgáltató URI. A minta köszönőüzenetei kibocsátó hello nem szabad újra. Ha több felső szintű tartomány szerepel az Azure AD bérlők hello kibocsátó meg kell egyeznie a hello megadott URI-beállítás konfigurálva tartományonként.|

>[!IMPORTANT]
>Jelenleg az Azure AD támogatja az SAML 2.0:urn:oasis:names:tc:SAML:2.0:nameid NameID formátum URI következő hello-formátum: állandó.

## <a name="sample-saml-request-and-response-messages"></a>Példa SAML kérés- és üzenetek
Egy kérelem-válasz üzenet pár hello bejelentkezés üzenet exchange jelennek meg.
Ez az egy minta-üzenet, az Azure AD tooa minta SAML 2.0 identitásszolgáltató által küldött. hello minta SAML 2.0 identitásszolgáltató az Active Directory összevonási szolgáltatások (AD FS) toouse SAML-P protokollt konfigurált. Együttműködés ellenőrzésére is befejeződött egyéb SAML 2.0 identitás-szolgáltatóktól.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Ez az hello minta SAML 2.0-s megfelelő identity provider tooAzure AD által küldött minta válaszüzenetet vagy Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyhBREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDTE1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9ybWVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/+3ZWxd9T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEMb2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvyJOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBySx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">ABCDEG1234567890</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" NotOnOrAfter="2014-01-31T15:41:31.357Z" Recipient="https://login.microsoftonline.com/login.srf" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2014-01-31T15:36:31.263Z" NotOnOrAfter="2014-01-31T16:36:31.263Z">
      <AudienceRestriction>
        <Audience>urn:federation:MicrosoftOnline</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="IDPEmail">
        <AttributeValue>administrator@contoso.com</AttributeValue>
      </Attribute>
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2014-01-31T15:36:30.200Z" SessionIndex="_7e3c1bcd-f180-4f78-83e1-7680920793aa">
      <AuthnContext>
        <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
    </Assertion>
    </samlp:Response>`

## <a name="configure-your-saml-20-compliant-identity-provider"></a>A SAML 2.0-s megfelelő identitásszolgáltató konfigurálása
Ez a témakör útmutatást a hogyan tooconfigure a SAML 2.0 identity provider toofederate az Azure AD tooenable egyszeri bejelentkezéses hozzáférést tooone vagy további Microsoft-felhő szolgáltatásokhoz (például Office 365) hello SAML 2.0 protokoll használatával. hello függő entitás egy Microsoft felhőszolgáltatásra, ebben a forgatókönyvben használt SAML 2.0 az Azure AD.

## <a name="add-azure-ad-metadata"></a>Az Azure AD-metaadatok hozzáadása
A SAML 2.0 identitásszolgáltató az Azure AD hello függő entitással kapcsolatos tooadhere tooinformation kell. Az Azure AD https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml a metaadatok közzététele.

Javasoljuk, hogy Ön mindig hello legújabb az Azure AD metaadatok importálása a SAML 2.0 identitásszolgáltató konfigurálásakor. Vegye figyelembe, hogy az Azure AD nem beolvasni metaadatok hello identitásszolgáltató.

## <a name="add-azure-ad-as-a-relying-party"></a>Az Azure AD hozzáadni egy függő entitás
Lehetősége van a SAML 2.0 identitásszolgáltató és az Azure AD tooenable kommunikációját. Ebben a konfigurációban a megadott identitásszolgáltató függ, és az toodocumentation olvassa el. Általában állíthat hello függő entitás azonosítója toohello ugyanaz, mint az Azure AD hello metaadatokból entityid hello beállítást.

>[!NOTE]
>Győződjön meg arról a SAML 2.0-s szolgáltató identitáskiszolgálók hello órája szinkronizált tooan pontos időforrást. A pontos idő összevont bejelentkezések toofail okozhat.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>Telepítse a Windows PowerShell bejelentkezéshez a SAML 2.0 identitásszolgáltató
Miután konfigurálta a SAML 2.0 identitásszolgáltató használható az Azure AD sign-on, hello következő lépésre toodownload és a telepítés hello Active Directory modul Windows Powershellhez készült Azure. A telepítést követően használni kívánt ezen parancsmagok tooconfigure az Azure AD-tartományok összevont tartományt.

Active Directory modul Windows Powershellhez készült Azure hello letölthető az a szervezeti adatokat kezelése az Azure ad-ben. Ez a modul parancsmagjai tooWindows PowerShell; készletét telepíti. Ezen parancsmagok tooset be egyszeri bejelentkezéses hozzáférést tooAzure AD futtatja, és viszont tooall hello a felhőszolgáltatásokhoz előfizetett. Hogyan toodownload és a telepítés hello parancsmagokkal kapcsolatos útmutatásért lásd: [http://technet.microsoft.com/library/jj151815.aspx](http://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>A SAML-Identitásszolgáltatóként és az Azure AD közötti megbízhatósági kapcsolat beállítása
Összevonás konfigurálása az Azure AD-tartomány, mielőtt konfigurált egyéni tartományt kell rendelkeznie. A Microsoft által biztosított hello alapértelmezett tartomány nem vonható össze. a Microsoft hello alapértelmezett tartomány "onmicrosoft.com" végződik.
A hello Windows PowerShell parancssori felület tooadd parancsmagok futtathatók, vagy alakítsa át a tartományok egyszeri bejelentkezéshez lesz.

Minden Azure Active Directory-tartomány, amelyet a SAML 2.0 identitásszolgáltató használatával toofederate vagy kell vehető fel egy egyszeri bejelentkezéses tartománnyá vagy átalakított toobe egy egyszeri bejelentkezéses tartománnyá általános tartományból. Hozzáadása vagy konvertálása tartomány állít be a SAML 2.0 identitásszolgáltató és az Azure AD közötti megbízhatósági kapcsolat.

hello alábbi eljárás bemutatja, hogyan egy meglévő szabványos tartomány tooa összevont tartományt használja a SAML 2.0 SP-Lite átalakításának lépésein. Vegye figyelembe, hogy a tartomány, amely hatással van a felhasználók Ez a lépés elvégzése után too2 órában kimaradás problémákat tapasztalhat.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Az Azure Active Directory összevonási a tartománynév beállítása


1. Csatlakozzon az Azure AD-címtár bérlői rendszergazdaként tooyour: csatlakozás MsolService.
2.  A kívánt Office 365-tartomány toouse összevonás konfigurálása a SAML 2.0:`$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  Aláíró tanúsítvány base64 kódolású karakterláncnak a következőről a kiállító terjesztési hely metaadatfájl hello szerezhet be. Ezen a helyen például adtak meg, de előfordulhat, hogy a megvalósítás alapján némileg eltérőek lehetnek.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

További információ a "Set-MsolDomainAuthentication":: [http://technet.microsoft.com/library/dn194112.aspx](http://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Futtatnia kell a használata "$ecpUrl"https://WS2012R2-0.contoso.com/PAOS"=" csak akkor, ha az identitásszolgáltató beállított ECP kiterjesztéssel. Exchange Online ügyfelek, az Outlook Web Application (OWA), kivéve a FELADÁS egy vagy több alapú aktív végpontot. Ha a SAML 2.0 STS egy aktív végpontot egy aktív végpontot hasonló tooShibboleth ECP végrehajtásának megvalósítja a funkciógazdag ügyfeleket toointeract az Exchange Online szolgáltatáshoz hello lehetővé lehet.

Összevonási konfigurálása után átválthatja vissza túl "nem összevont" (vagy "kezelt"), azonban ez a változás tootwo óra toocomplete foglal, és szükség van hozzárendelése új véletlenszerű jelszót a felhő alapú bejelentkezési tooeach felhasználói. Mód visszakapcsolását túl "kezelt" lehet, hogy az egyes forgatókönyvek tooreset szükséges hibát jelez a beállításokat. További információ a tartomány átalakítás:: [http://msdn.microsoft.com/library/windowsazure/dn194122.aspx](http://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-tooazure-ad--office-365"></a>Felhasználó rendszerbiztonsági tagok tooAzure AD kiépítése vagy Office 365
Mielőtt a felhasználók tooOffice 365 hitelesítheti el kell juttatnia az Azure AD rendszerbiztonsági tagok, amelyek megfelelnek a SAML 2.0 hello toohello állítás jogcím felhasználóval. Ha a felhasználó rendszerbiztonsági tagok nem ismert előre tooAzure AD majd azok nem használható az összevont bejelentkezés. Az Azure AD Connect vagy a Windows PowerShell használt tooprovision felhasználó rendszerbiztonsági tagoknak lehet.

Az Azure AD Connect lehet használt tooprovision rendszerbiztonsági tagok tooyour az Azure AD-címtár hello helyszíni Active Directory tartományában. További részletekért lásd: [integrálása a helyszíni címtárakat az Azure Active Directoryval](active-directory-aadconnect.md).

Windows PowerShell is lehet hozzáadni az új felhasználók tooAzure AD használt tooautomate és toosynchronize hello helyszíni címtár-ről változik. toouse hello hello le kell töltenie a Windows PowerShell-parancsmagok [Azure Active Directory modulok](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Ez az eljárás bemutatja, hogyan tooadd egy egyfelhasználós tooAzure AD.


1. Csatlakozzon az Azure AD-címtár bérlői rendszergazdaként tooyour: csatlakozás MsolService.
2.  Hozzon létre egy új egyszerű:` New-MsolUser
        -UserPrincipalName elwoodf1@contoso.com
        -ImmutableId ABCDEFG1234567890
        -DisplayName "Elwood Folk"
        -FirstName Elwood 
        -LastName Folk 
        -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
        -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
        -UsageLocation "US" ` 

További információ a "New-MsolUser" kivételt [http://technet.microsoft.com/library/dn194096.aspx](http://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>hello értéket meg kell egyeznie a hello érték, amely elküldi az "IDPEmail" a SAML 2.0 jogcímek és az értéket meg kell egyeznie a "NameID" helyességi feltétel küldi hello érték "ImmutableID" hello "UserPrinciplName".

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Egyszeri bejelentkezéshez a SAML 2.0 IDP rendelkező ellenőrzése
Hello rendszergazdaként előtt győződjön meg arról, és az egyszeri bejelentkezés (is néven identitás-összevonási) kezelése, hello tekintse át, és hajtsa végre a következő cikkek tooset be egyszeri bejelentkezéshez a SAML 2.0 SP-Lite alapján identitásszolgáltatóval hello hello lépéseket:


1.  Hello Azure AD SAML 2.0 protokoll követelményeinek áttekintése
2.  A SAML 2.0 identitásszolgáltató konfigurált
3.  Telepítse a Windows PowerShell egyszeri bejelentkezéshez a SAML 2.0 identitásszolgáltatóval
4.  SAML 2.0 identitásszolgáltató és az Azure AD közötti megbízhatósági kapcsolat beállítása
5.  Kiépített egy ismert teszt felhasználó egyszerű tooAzure (Office 365) Active Directory Windows PowerShell vagy az Azure AD Connect keresztül.
6.  Konfigurálja a címtár-szinkronizálás használatával [az Azure AD Connect](active-directory-aadconnect.md).

Beállítása után egyszeri bejelentkezéshez a SAML 2.0 SP-Lite alapú identitású szolgáltató, ellenőrizze, hogy megfelelően működik-e.

>[!NOTE]
>Ha egy tartomány alakította át ahelyett, hogy hozzáadott, igénybe vehet too24 óra tooset be egyszeri bejelentkezést.
Egyszeri bejelentkezés ellenőriznie, mielőtt kell Active Directory-szinkronizálás beállításának befejezéséhez, a címtárak szinkronizálása, és a szinkronizált felhasználók aktiválása.

### <a name="use-hello-tool-tooverify-that-single-sign-on-has-been-set-up-correctly"></a>Egyszeri bejelentkezés használata hello eszköz tooverify megfelelően be van állítva
tooverify, hogy az egyszeri bejelentkezés beállítása helyes, a következő eljárás tooconfirm, hogy-e toohello felhőszolgáltatásban képes toosign a vállalati hitelesítő adataikkal hello végezheti el.

A Microsoft közzétett egy eszköz használható tootest a SAML 2.0-alapú identitás-szolgáltatót. Hello futtatása előtt konfigurálnia kell egy Azure AD-bérlő toofederate a identitásszolgáltatóval eszköz tesztelése.

>[!NOTE]
>hello kapcsolat Analyzer Internet Explorer 10 vagy újabb verzió szükséges.



1. Letölthető, a kapcsolat Analyzer hello [https://testconnectivity.microsoft.com/?tabid=Client](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Kattintson a telepítés toobegin tölt le, és hello eszköz telepítése.
3.  Válassza ki a "I nem állítható be az Office 365, Azure vagy az Azure Active Directory használó más szolgáltatásokba összevonásának".
4.  Ha hello eszköz letöltése és futtatása, hello kapcsolat Diagnostics ablakban jelenik meg. hello eszköz rendszer végigvezeti a összevonási kapcsolat tesztelése.
5.  hello kapcsolat Analyzer megnyílik az Ön toologon a SAML 2.0 IDP, adja meg a tesztelni kívánt hello egyszerű hello hitelesítő adatait: ![SAML](media/active-directory-aadconnect-federation-saml-idp/saml1.png)
6.  Hello összevonási, tesztelje a bejelentkezési ablak meg kell adnia fióknevet és jelszót, amely a SAML 2.0 identitásszolgáltatóval összevont konfigurált toobe hello Azure AD-bérlő. hello eszköz megpróbál toosign-a használja ezeket a hitelesítő adatokat, és kimenetként nyújtanak részletes tesztek eredményét az hello bejelentkezési kísérlet során.
![SAML](media/active-directory-aadconnect-federation-saml-idp/saml2.png)
7. Ebben az ablakban látható a vizsgálat sikertelen eredményt. Kattintson a tekintse át a részletes adatok jelennek meg, hogy végre lett hajtva a tesztelések hello eredmény. Hello eredmények toodisk is mentheti rendelés tooshare őket.
 
>[!NOTE]
>hello kapcsolat analyzer is teszteli használatával hello WS * aktív összevonási-alapú és a ECP/PAOS protokollokat. Ha nem használja ezeket figyelmen kívül hagyhatja a következő hiba hello: tesztelési hello aktív bejelentkezési folyamata az identitásszolgáltató aktív összevonási endpoint használatával.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Ellenőrizze, hogy az egyszeri bejelentkezés beállítása megfelelően manuálisan
Manuális ellenőrzést biztosít a további lépéseket, amelyek is, amely az SAML 2.0 identitás szolgáltató megfelelően működik-e a számos forgatókönyv tooensure.
amely az egyszeri bejelentkezés tooverify megfelelően be van állítva, végezze el a hello alábbi lépéseket:


1. A tartományhoz csatlakoztatott számítógépen bejelentkezés tooyour felhőalapú szolgáltatás azonos bejelentkezési név a vállalati hitelesítő adatokat használó hello segítségével.
2.  Kattintson a hello jelszó mezőbe. Ha egyszeri bejelentkezésre van beállítva, hello jelszó mező árnyékolt lesz, és látni fogja a következő üzenet hello: "áll, a szükséges toosign <your company>."
3.  Kattintson a hello jelentkezzen be a(z) <your company> hivatkozásra. Ha a képes toosign, majd egyszeri bejelentkezést be van állítva.

## <a name="next-steps"></a>Következő lépések


- [Active Directory összevonási szolgáltatások kezelése és testreszabása az Azure AD Connecttel](active-directory-aadconnect-federation-management.md)
- [Az Azure AD összevonás kompatibilitási listája](active-directory-aadconnect-federation-compatibility.md)
- [Az Azure AD Connect egyéni telepítése](active-directory-aadconnect-get-started-custom.md)
