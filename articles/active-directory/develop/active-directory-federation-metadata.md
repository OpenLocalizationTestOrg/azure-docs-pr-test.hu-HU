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
# <a name="federation-metadata"></a><span data-ttu-id="b082c-103">Összevonási metaadatok</span><span class="sxs-lookup"><span data-stu-id="b082c-103">Federation metadata</span></span>
<span data-ttu-id="b082c-104">Azure Active Directory (Azure AD), amely szolgáltatások beállítva tooaccept hello biztonsági jogkivonatokat, amelyek az Azure AD kibocsát tesz közzé egy összevonási metaadat-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b082c-104">Azure Active Directory (Azure AD) publishes a federation metadata document for services that is configured tooaccept hello security tokens that Azure AD issues.</span></span> <span data-ttu-id="b082c-105">hello összevonási metaadatok dokumentumformátum ismertetett hello [Web Services összevonási Language (WS-Federation) 1.2-es verziója](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), amely kiterjeszti [hello OASIS Security Assertion Markup Language (SAML) metaadatai v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span><span class="sxs-lookup"><span data-stu-id="b082c-105">hello federation metadata document format is described in hello [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), which extends [Metadata for hello OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span></span>

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a><span data-ttu-id="b082c-106">Bérlői-specifikus és bérlői független metaadat-végpontok</span><span class="sxs-lookup"><span data-stu-id="b082c-106">Tenant-specific and Tenant-independent metadata endpoints</span></span>
<span data-ttu-id="b082c-107">Az Azure AD bérlő-specifikus és bérlői független végpontok közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="b082c-107">Azure AD publishes tenant-specific and tenant-independent endpoints.</span></span>

<span data-ttu-id="b082c-108">Egy adott bérlő bérlői-specifikus végpontok készültek.</span><span class="sxs-lookup"><span data-stu-id="b082c-108">Tenant-specific endpoints are designed for a particular tenant.</span></span> <span data-ttu-id="b082c-109">hello bérlői-specifikus összevonási metaadatok hello bérlői, beleértve a bérlői-specifikus kibocsátó és végpont kapcsolatos adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b082c-109">hello tenant-specific federation metadata includes information about hello tenant, including tenant-specific issuer and endpoint information.</span></span> <span data-ttu-id="b082c-110">Alkalmazások, amelyek korlátozzák a hozzáférést tooa egyetlen bérlő bérlői-specifikus végpontok használata.</span><span class="sxs-lookup"><span data-stu-id="b082c-110">Applications that restrict access tooa single tenant use tenant-specific endpoints.</span></span>

<span data-ttu-id="b082c-111">Bérlői független végpontok adja meg a közös tooall Azure AD-bérlő információkat.</span><span class="sxs-lookup"><span data-stu-id="b082c-111">Tenant-independent endpoints provide information that is common tooall Azure AD tenants.</span></span> <span data-ttu-id="b082c-112">Ezek az információk vonatkoznak üzemeltetett tootenants *login.microsoftonline.com* és a bérlők által megosztott.</span><span class="sxs-lookup"><span data-stu-id="b082c-112">This information applies tootenants hosted at *login.microsoftonline.com* and is shared across tenants.</span></span> <span data-ttu-id="b082c-113">Bérlői független végpontok javasolt a több-bérlős alkalmazásokhoz, mivel azok nincsenek társítva, semmilyen különleges bérlővel.</span><span class="sxs-lookup"><span data-stu-id="b082c-113">Tenant-independent endpoints are recommended for multi-tenant applications, since they are not associated with any particular tenant.</span></span>

## <a name="federation-metadata-endpoints"></a><span data-ttu-id="b082c-114">Összevonási metaadatok végpontok</span><span class="sxs-lookup"><span data-stu-id="b082c-114">Federation metadata endpoints</span></span>
<span data-ttu-id="b082c-115">Az Azure AD közzéteszi az összevonási metaadatokban: `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="b082c-115">Azure AD publishes federation metadata at `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span>

<span data-ttu-id="b082c-116">A **bérlői-specifikus végpontok**, hello `TenantDomainName` hello a következő típusok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="b082c-116">For **tenant-specific endpoints**, hello `TenantDomainName` can be one of hello following types:</span></span>

* <span data-ttu-id="b082c-117">A regisztrált tartománynév, az Azure ad bérlői, például: `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="b082c-117">A registered domain name of an Azure AD tenant, such as: `contoso.onmicrosoft.com`.</span></span>
* <span data-ttu-id="b082c-118">nem módosítható hello bérlői hello tartomány azonosítója, mint `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span><span class="sxs-lookup"><span data-stu-id="b082c-118">hello immutable tenant ID of hello domain, such as `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span></span>

<span data-ttu-id="b082c-119">A **bérlői független végpontok**, hello `TenantDomainName` van `common`.</span><span class="sxs-lookup"><span data-stu-id="b082c-119">For **tenant-independent endpoints**, hello `TenantDomainName` is `common`.</span></span> <span data-ttu-id="b082c-120">Ez a dokumentum csak hello összevonási metaadatok elemek, amelyek közös tooall Azure AD-bérlőkkel, login.microsoftonline.com tárolt sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b082c-120">This document lists only hello Federation Metadata elements that are common tooall Azure AD tenants that are hosted at login.microsoftonline.com.</span></span>

<span data-ttu-id="b082c-121">Például lehet, hogy a bérlő-specifikus végpont `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="b082c-121">For example, a tenant-specific endpoint might be `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span> <span data-ttu-id="b082c-122">hello bérlői független végpont [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span><span class="sxs-lookup"><span data-stu-id="b082c-122">hello tenant-independent endpoint is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span></span> <span data-ttu-id="b082c-123">Hello összevonási metaadatok dokumentum megtekintéséhez írja be az URL-cím a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="b082c-123">You can view hello federation metadata document by typing this URL in a browser.</span></span>

## <a name="contents-of-federation-metadata"></a><span data-ttu-id="b082c-124">Összevonási metaadatok tartalmát</span><span class="sxs-lookup"><span data-stu-id="b082c-124">Contents of federation Metadata</span></span>
<span data-ttu-id="b082c-125">hello következő témakör, amely az Azure AD által kiállított hello jogkivonatokat használják szolgáltatások számára szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="b082c-125">hello following section provides information needed by services that consume hello tokens issued by Azure AD.</span></span>

### <a name="entity-id"></a><span data-ttu-id="b082c-126">Entitás azonosítója</span><span class="sxs-lookup"><span data-stu-id="b082c-126">Entity ID</span></span>
<span data-ttu-id="b082c-127">Hello `EntityDescriptor` elem tartalmazza-e egy `EntityID` attribútum.</span><span class="sxs-lookup"><span data-stu-id="b082c-127">hello `EntityDescriptor` element contains an `EntityID` attribute.</span></span> <span data-ttu-id="b082c-128">hello értékének hello `EntityID` attribútum hello kibocsátó jelöli, ez azt jelenti, hogy hello biztonsági token szolgáltatás (STS), hogy hello kiállított jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="b082c-128">hello value of hello `EntityID` attribute represents hello issuer, that is, hello security token service (STS) that issued hello token.</span></span> <span data-ttu-id="b082c-129">Amikor fogadhatnak jogkivonatot is fontos toovalidate hello kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="b082c-129">It is important toovalidate hello issuer when you receive a token.</span></span>

<span data-ttu-id="b082c-130">hello következő metaadatok megmutatja, bérlő-specifikus `EntityDescriptor` elem egy `EntityID` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-130">hello following metadata shows a sample tenant-specific `EntityDescriptor` element with an `EntityID` element.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
<span data-ttu-id="b082c-131">Lecserélheti hello Bérlőazonosító hello bérlői független végpont a bérlői azonosító toocreate egy bérlő-specifikus `EntityID` érték.</span><span class="sxs-lookup"><span data-stu-id="b082c-131">You can replace hello tenant ID in hello tenant-independent endpoint with your tenant ID toocreate a tenant-specific `EntityID` value.</span></span> <span data-ttu-id="b082c-132">hello eredményül kapott érték megegyeznek a jogkivonat-kibocsátó hello kell hello.</span><span class="sxs-lookup"><span data-stu-id="b082c-132">hello resulting value will be hello same as hello token issuer.</span></span> <span data-ttu-id="b082c-133">hello stratégia lehetővé teszi, hogy egy több-bérlős alkalmazás toovalidate hello kibocsátó egy adott bérlő.</span><span class="sxs-lookup"><span data-stu-id="b082c-133">hello strategy allows a multi-tenant application toovalidate hello issuer for a given tenant.</span></span>

<span data-ttu-id="b082c-134">hello következő metaadatok megmutatja, bérlő független `EntityID` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-134">hello following metadata shows a sample tenant-independent `EntityID` element.</span></span> <span data-ttu-id="b082c-135">Ne feledje, hogy hello `{tenant}` szövegkonstans, nem egy helyőrző.</span><span class="sxs-lookup"><span data-stu-id="b082c-135">Please note, that hello `{tenant}` is a literal, not a placeholder.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a><span data-ttu-id="b082c-136">Jogkivonat-aláíró tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="b082c-136">Token signing certificates</span></span>
<span data-ttu-id="b082c-137">Ha egy szolgáltatás kap egy jogkivonatot, amely egy Azure AD-bérlő által kiadott, hello hello jogkivonat aláírását is ellenőrizni kell hello összevonási metaadatok dokumentumban közzétett aláírókulccsal.</span><span class="sxs-lookup"><span data-stu-id="b082c-137">When a service receives a token that is issued by a Azure AD tenant, hello signature of hello token must be validated with a signing key that is published in hello federation metadata document.</span></span> <span data-ttu-id="b082c-138">hello összevonási metaadatok hello hello hello bérlők használ a jogkivonat-aláíró tanúsítványokat nyilvános részét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b082c-138">hello federation metadata includes hello public portion of hello certificates that hello tenants use for token signing.</span></span> <span data-ttu-id="b082c-139">hello tanúsítvány nyers bájt jelenik meg hello `KeyDescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-139">hello certificate raw bytes appear in hello `KeyDescriptor` element.</span></span> <span data-ttu-id="b082c-140">az aláíró csak amikor hello hello értéke érvénytelen a hello jogkivonat-aláíró tanúsítvány `use` attribútum `signing`.</span><span class="sxs-lookup"><span data-stu-id="b082c-140">hello token signing certificate is valid for signing only when hello value of hello `use` attribute is `signing`.</span></span>

<span data-ttu-id="b082c-141">Az Azure AD által közzétett összevonási metaadatok dokumentum rendelkezhet több aláírási kulcs, például ha az Azure AD arra készül, tooupdate hello aláíró tanúsítványát.</span><span class="sxs-lookup"><span data-stu-id="b082c-141">A federation metadata document published by Azure AD can have multiple signing keys, such as when Azure AD is preparing tooupdate hello signing certificate.</span></span> <span data-ttu-id="b082c-142">Ha egy összevonási metaadat-dokumentum tartalmazza az egynél több tanúsítvány, egy szolgáltatás, amely hello jogkivonatok érvényesítése támogatnia kell a minden tanúsítvány hello dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="b082c-142">When a federation metadata document includes more than one certificate, a service that is validating hello tokens should support all certificates in hello document.</span></span>

<span data-ttu-id="b082c-143">hello következő metaadatok megmutatja, `KeyDescriptor` elem az aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="b082c-143">hello following metadata shows a sample `KeyDescriptor` element with a signing key.</span></span>

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

<span data-ttu-id="b082c-144">Hello `KeyDescriptor` elem hello összevonási metaadat-dokumentum; a WS-összevonás-specifikus hello és hello SAML-specifikus szakaszát két helyen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b082c-144">hello `KeyDescriptor` element appears in two places in hello federation metadata document; in hello WS-Federation-specific section and hello SAML-specific section.</span></span> <span data-ttu-id="b082c-145">mindkét szakasz közzétett hello tanúsítványok azonos lesz hello.</span><span class="sxs-lookup"><span data-stu-id="b082c-145">hello certificates published in both sections will be hello same.</span></span>

<span data-ttu-id="b082c-146">A WS-összevonás-specifikus hello szakaszban egy WS-összevonás metaadat-olvasó olvashatja, hello a tanúsítványokat a egy `RoleDescriptor` hello elem `SecurityTokenServiceType` típusa.</span><span class="sxs-lookup"><span data-stu-id="b082c-146">In hello WS-Federation-specific section, a WS-Federation metadata reader would read hello certificates from a `RoleDescriptor` element with hello `SecurityTokenServiceType` type.</span></span>

<span data-ttu-id="b082c-147">hello következő metaadatok megmutatja, `RoleDescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-147">hello following metadata shows a sample `RoleDescriptor` element.</span></span>

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

<span data-ttu-id="b082c-148">Hello SAML-specifikus a szakaszban egy WS-összevonás metaadat-olvasó hello tanúsítványokat olvashatja a `IDPSSODescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-148">In hello SAML-specific section, a WS-Federation metadata reader would read hello certificates from a `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="b082c-149">hello következő metaadatok megmutatja, `IDPSSODescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-149">hello following metadata shows a sample `IDPSSODescriptor` element.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
<span data-ttu-id="b082c-150">Nincsenek különbségek vonatkozó bérlői és bérlői független tanúsítványok hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="b082c-150">There are no differences in hello format of tenant-specific and tenant-independent certificates.</span></span>

### <a name="ws-federation-endpoint-url"></a><span data-ttu-id="b082c-151">WS-Federation végponti URL-cím</span><span class="sxs-lookup"><span data-stu-id="b082c-151">WS-Federation endpoint URL</span></span>
<span data-ttu-id="b082c-152">hello az összevonási metaadatok tartalmazzák az Azure AD URL-Címeket használ a egyszeri bejelentkezés és kijelentkezés a WS-Federation protokoll egyetlen hello.</span><span class="sxs-lookup"><span data-stu-id="b082c-152">hello federation metadata includes hello URL that is Azure AD uses for single sign-in and single sign-out in WS-Federation protocol.</span></span> <span data-ttu-id="b082c-153">Ehhez a végponthoz hello megjelenik `PassiveRequestorEndpoint` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-153">This endpoint appears in hello `PassiveRequestorEndpoint` element.</span></span>

<span data-ttu-id="b082c-154">hello következő metaadatok megmutatja, `PassiveRequestorEndpoint` elem egy bérlő-specifikus végpont.</span><span class="sxs-lookup"><span data-stu-id="b082c-154">hello following metadata shows a sample `PassiveRequestorEndpoint` element for a tenant-specific endpoint.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
<span data-ttu-id="b082c-155">Hello bérlői független végpont hello WS-összevonás URL-cím jelenik meg hello WS-Federation végpont hello minta a következő ábrán.</span><span class="sxs-lookup"><span data-stu-id="b082c-155">For hello tenant-independent endpoint, hello WS-Federation URL appears in hello WS-Federation endpoint, as shown in hello following sample.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a><span data-ttu-id="b082c-156">SAML protokoll végponti URL-cím</span><span class="sxs-lookup"><span data-stu-id="b082c-156">SAML protocol endpoint URL</span></span>
<span data-ttu-id="b082c-157">hello az összevonási metaadatok tartalmazzák az Azure AD használ az egyszeri bejelentkezés és kijelentkezés a SAML 2.0 protokoll egyetlen hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b082c-157">hello federation metadata includes hello URL that Azure AD uses for single sign-in and single sign-out in SAML 2.0 protocol.</span></span> <span data-ttu-id="b082c-158">Ezeket a végpontokat jelennek meg hello `IDPSSODescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="b082c-158">These endpoints appear in hello `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="b082c-159">hello be- és kijelentkezési URL-cím látható hello `SingleSignOnService` és `SingleLogoutService` elemeket.</span><span class="sxs-lookup"><span data-stu-id="b082c-159">hello sign-in and sign-out URLs appear in hello `SingleSignOnService` and `SingleLogoutService` elements.</span></span>

<span data-ttu-id="b082c-160">hello következő metaadatok megmutatja, `PassiveResistorEndpoint` egy bérlő-specifikus végpont.</span><span class="sxs-lookup"><span data-stu-id="b082c-160">hello following metadata shows a sample `PassiveResistorEndpoint` for a tenant-specific endpoint.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

<span data-ttu-id="b082c-161">Hasonlóképpen hello végpontok hello közös SAML 2.0 protokoll végpontok közzétett hello bérlői független összevonási metaadatokban hello minta a következő ábrán.</span><span class="sxs-lookup"><span data-stu-id="b082c-161">Similarly hello endpoints for hello common SAML 2.0 protocol endpoints are published in hello tenant-independent federation metadata, as shown in hello following sample.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
