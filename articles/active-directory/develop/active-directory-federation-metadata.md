---
title: "aaaAzure AD összevonási metaadatok |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Active Directory tesz közzé az Azure Active Directory-tokeneket támogató szolgáltatások hello összevonási metaadatok dokumentum."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c2d5f80b-aa74-452c-955b-d8eb3ed62652
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 23535bcd5eeb3e9b2e17d89a9b0420fc98bd3895
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="federation-metadata"></a>Összevonási metaadatok
Azure Active Directory (Azure AD), amely szolgáltatások beállítva tooaccept hello biztonsági jogkivonatokat, amelyek az Azure AD kibocsát tesz közzé egy összevonási metaadat-dokumentum. hello összevonási metaadatok dokumentumformátum ismertetett hello [Web Services összevonási Language (WS-Federation) 1.2-es verziója](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), amely kiterjeszti [hello OASIS Security Assertion Markup Language (SAML) metaadatai v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Bérlői-specifikus és bérlői független metaadat-végpontok
Az Azure AD bérlő-specifikus és bérlői független végpontok közzéteszi.

Egy adott bérlő bérlői-specifikus végpontok készültek. hello bérlői-specifikus összevonási metaadatok hello bérlői, beleértve a bérlői-specifikus kibocsátó és végpont kapcsolatos adatokat tartalmaz. Alkalmazások, amelyek korlátozzák a hozzáférést tooa egyetlen bérlő bérlői-specifikus végpontok használata.

Bérlői független végpontok adja meg a közös tooall Azure AD-bérlő információkat. Ezek az információk vonatkoznak üzemeltetett tootenants *login.microsoftonline.com* és a bérlők által megosztott. Bérlői független végpontok javasolt a több-bérlős alkalmazásokhoz, mivel azok nincsenek társítva, semmilyen különleges bérlővel.

## <a name="federation-metadata-endpoints"></a>Összevonási metaadatok végpontok
Az Azure AD közzéteszi az összevonási metaadatokban: `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

A **bérlői-specifikus végpontok**, hello `TenantDomainName` hello a következő típusok egyike lehet:

* A regisztrált tartománynév, az Azure ad bérlői, például: `contoso.onmicrosoft.com`.
* nem módosítható hello bérlői hello tartomány azonosítója, mint `72f988bf-86f1-41af-91ab-2d7cd011db45`.

A **bérlői független végpontok**, hello `TenantDomainName` van `common`. Ez a dokumentum csak hello összevonási metaadatok elemek, amelyek közös tooall Azure AD-bérlőkkel, login.microsoftonline.com tárolt sorolja fel.

Például lehet, hogy a bérlő-specifikus végpont `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. hello bérlői független végpont [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Hello összevonási metaadatok dokumentum megtekintéséhez írja be az URL-cím a böngészőben.

## <a name="contents-of-federation-metadata"></a>Összevonási metaadatok tartalmát
hello következő témakör, amely az Azure AD által kiállított hello jogkivonatokat használják szolgáltatások számára szükséges adatokat.

### <a name="entity-id"></a>Entitás azonosítója
Hello `EntityDescriptor` elem tartalmazza-e egy `EntityID` attribútum. hello értékének hello `EntityID` attribútum hello kibocsátó jelöli, ez azt jelenti, hogy hello biztonsági token szolgáltatás (STS), hogy hello kiállított jogkivonat. Amikor fogadhatnak jogkivonatot is fontos toovalidate hello kibocsátó.

hello következő metaadatok megmutatja, bérlő-specifikus `EntityDescriptor` elem egy `EntityID` elemet.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Lecserélheti hello Bérlőazonosító hello bérlői független végpont a bérlői azonosító toocreate egy bérlő-specifikus `EntityID` érték. hello eredményül kapott érték megegyeznek a jogkivonat-kibocsátó hello kell hello. hello stratégia lehetővé teszi, hogy egy több-bérlős alkalmazás toovalidate hello kibocsátó egy adott bérlő.

hello következő metaadatok megmutatja, bérlő független `EntityID` elemet. Ne feledje, hogy hello `{tenant}` szövegkonstans, nem egy helyőrző.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Jogkivonat-aláíró tanúsítványok
Ha egy szolgáltatás kap egy jogkivonatot, amely egy Azure AD-bérlő által kiadott, hello hello jogkivonat aláírását is ellenőrizni kell hello összevonási metaadatok dokumentumban közzétett aláírókulccsal. hello összevonási metaadatok hello hello hello bérlők használ a jogkivonat-aláíró tanúsítványokat nyilvános részét tartalmazza. hello tanúsítvány nyers bájt jelenik meg hello `KeyDescriptor` elemet. az aláíró csak amikor hello hello értéke érvénytelen a hello jogkivonat-aláíró tanúsítvány `use` attribútum `signing`.

Az Azure AD által közzétett összevonási metaadatok dokumentum rendelkezhet több aláírási kulcs, például ha az Azure AD arra készül, tooupdate hello aláíró tanúsítványát. Ha egy összevonási metaadat-dokumentum tartalmazza az egynél több tanúsítvány, egy szolgáltatás, amely hello jogkivonatok érvényesítése támogatnia kell a minden tanúsítvány hello dokumentumban.

hello következő metaadatok megmutatja, `KeyDescriptor` elem az aláírási kulcs.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Hello `KeyDescriptor` elem hello összevonási metaadat-dokumentum; a WS-összevonás-specifikus hello és hello SAML-specifikus szakaszát két helyen jelenik meg. mindkét szakasz közzétett hello tanúsítványok azonos lesz hello.

A WS-összevonás-specifikus hello szakaszban egy WS-összevonás metaadat-olvasó olvashatja, hello a tanúsítványokat a egy `RoleDescriptor` hello elem `SecurityTokenServiceType` típusa.

hello következő metaadatok megmutatja, `RoleDescriptor` elemet.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

Hello SAML-specifikus a szakaszban egy WS-összevonás metaadat-olvasó hello tanúsítványokat olvashatja a `IDPSSODescriptor` elemet.

hello következő metaadatok megmutatja, `IDPSSODescriptor` elemet.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Nincsenek különbségek vonatkozó bérlői és bérlői független tanúsítványok hello formátumban.

### <a name="ws-federation-endpoint-url"></a>WS-Federation végponti URL-cím
hello az összevonási metaadatok tartalmazzák az Azure AD URL-Címeket használ a egyszeri bejelentkezés és kijelentkezés a WS-Federation protokoll egyetlen hello. Ehhez a végponthoz hello megjelenik `PassiveRequestorEndpoint` elemet.

hello következő metaadatok megmutatja, `PassiveRequestorEndpoint` elem egy bérlő-specifikus végpont.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Hello bérlői független végpont hello WS-összevonás URL-cím jelenik meg hello WS-Federation végpont hello minta a következő ábrán.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokoll végponti URL-cím
hello az összevonási metaadatok tartalmazzák az Azure AD használ az egyszeri bejelentkezés és kijelentkezés a SAML 2.0 protokoll egyetlen hello URL-CÍMÉT. Ezeket a végpontokat jelennek meg hello `IDPSSODescriptor` elemet.

hello be- és kijelentkezési URL-cím látható hello `SingleSignOnService` és `SingleLogoutService` elemeket.

hello következő metaadatok megmutatja, `PassiveResistorEndpoint` egy bérlő-specifikus végpont.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Hasonlóképpen hello végpontok hello közös SAML 2.0 protokoll végpontok közzétett hello bérlői független összevonási metaadatokban hello minta a következő ábrán.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
