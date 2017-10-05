---
title: "Az Azure AD összevonási metaadatok |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Active Directory-tokeneket támogató szolgáltatások közzéteszi Azure Active Directory összevonási metaadat-dokumentum."
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
ms.openlocfilehash: ecafb02a6ac13d1c3cd1fe77ef710cd8525e32b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="federation-metadata"></a><span data-ttu-id="7346e-103">Összevonási metaadatok</span><span class="sxs-lookup"><span data-stu-id="7346e-103">Federation metadata</span></span>
<span data-ttu-id="7346e-104">Azure Active Directory (Azure AD) tesz közzé egy összevonási metaadat-dokumentum számára, amely konfigurálva van a biztonsági jogkivonatokat, amelyek az Azure AD kibocsát fogadására.</span><span class="sxs-lookup"><span data-stu-id="7346e-104">Azure Active Directory (Azure AD) publishes a federation metadata document for services that is configured to accept the security tokens that Azure AD issues.</span></span> <span data-ttu-id="7346e-105">Az összevonási metaadatok dokumentum formátuma leírtak a [Web Services összevonási Language (WS-Federation) 1.2-es verziója](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), amely kiterjeszti [a OASIS Security Assertion Markup Language (SAML) 2.0 metaadatok](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span><span class="sxs-lookup"><span data-stu-id="7346e-105">The federation metadata document format is described in the [Web Services Federation Language (WS-Federation) Version 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), which extends [Metadata for the OASIS Security Assertion Markup Language (SAML) v2.0](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).</span></span>

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a><span data-ttu-id="7346e-106">Bérlői-specifikus és bérlői független metaadat-végpontok</span><span class="sxs-lookup"><span data-stu-id="7346e-106">Tenant-specific and Tenant-independent metadata endpoints</span></span>
<span data-ttu-id="7346e-107">Az Azure AD bérlő-specifikus és bérlői független végpontok közzéteszi.</span><span class="sxs-lookup"><span data-stu-id="7346e-107">Azure AD publishes tenant-specific and tenant-independent endpoints.</span></span>

<span data-ttu-id="7346e-108">Egy adott bérlő bérlői-specifikus végpontok készültek.</span><span class="sxs-lookup"><span data-stu-id="7346e-108">Tenant-specific endpoints are designed for a particular tenant.</span></span> <span data-ttu-id="7346e-109">A bérlő-specifikus az összevonási metaadatok tartalmazzák a bérlő bérlői-specifikus kibocsátó és a végpont párbeszédeket kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="7346e-109">The tenant-specific federation metadata includes information about the tenant, including tenant-specific issuer and endpoint information.</span></span> <span data-ttu-id="7346e-110">Alkalmazások, amely korlátozza a hozzáférést egy egyetlen bérlő bérlői-specifikus végpontok használata.</span><span class="sxs-lookup"><span data-stu-id="7346e-110">Applications that restrict access to a single tenant use tenant-specific endpoints.</span></span>

<span data-ttu-id="7346e-111">Bérlői független végpontok minden Azure AD-bérlő közös információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="7346e-111">Tenant-independent endpoints provide information that is common to all Azure AD tenants.</span></span> <span data-ttu-id="7346e-112">Ezek az információk vonatkoznak a bérlők számára a(z) *login.microsoftonline.com* és a bérlők által megosztott.</span><span class="sxs-lookup"><span data-stu-id="7346e-112">This information applies to tenants hosted at *login.microsoftonline.com* and is shared across tenants.</span></span> <span data-ttu-id="7346e-113">Bérlői független végpontok javasolt a több-bérlős alkalmazásokhoz, mivel azok nincsenek társítva, semmilyen különleges bérlővel.</span><span class="sxs-lookup"><span data-stu-id="7346e-113">Tenant-independent endpoints are recommended for multi-tenant applications, since they are not associated with any particular tenant.</span></span>

## <a name="federation-metadata-endpoints"></a><span data-ttu-id="7346e-114">Összevonási metaadatok végpontok</span><span class="sxs-lookup"><span data-stu-id="7346e-114">Federation metadata endpoints</span></span>
<span data-ttu-id="7346e-115">Az Azure AD közzéteszi az összevonási metaadatokban: `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="7346e-115">Azure AD publishes federation metadata at `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span>

<span data-ttu-id="7346e-116">A **bérlői-specifikus végpontok**, a `TenantDomainName` a következő típusok egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="7346e-116">For **tenant-specific endpoints**, the `TenantDomainName` can be one of the following types:</span></span>

* <span data-ttu-id="7346e-117">A regisztrált tartománynév, az Azure ad bérlői, például: `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="7346e-117">A registered domain name of an Azure AD tenant, such as: `contoso.onmicrosoft.com`.</span></span>
* <span data-ttu-id="7346e-118">A nem módosítható a tartomány azonosítója, mint bérlői `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span><span class="sxs-lookup"><span data-stu-id="7346e-118">The immutable tenant ID of the domain, such as `72f988bf-86f1-41af-91ab-2d7cd011db45`.</span></span>

<span data-ttu-id="7346e-119">A **bérlői független végpontok**, a `TenantDomainName` van `common`.</span><span class="sxs-lookup"><span data-stu-id="7346e-119">For **tenant-independent endpoints**, the `TenantDomainName` is `common`.</span></span> <span data-ttu-id="7346e-120">Ez a dokumentum csak login.microsoftonline.com által futtatott összes Azure AD-bérlőre vonatkozó összevonási metaadatok elemeket sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="7346e-120">This document lists only the Federation Metadata elements that are common to all Azure AD tenants that are hosted at login.microsoftonline.com.</span></span>

<span data-ttu-id="7346e-121">Például lehet, hogy a bérlő-specifikus végpont `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span><span class="sxs-lookup"><span data-stu-id="7346e-121">For example, a tenant-specific endpoint might be `https://login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`.</span></span> <span data-ttu-id="7346e-122">A bérlői független végpont [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span><span class="sxs-lookup"><span data-stu-id="7346e-122">The tenant-independent endpoint is [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml).</span></span> <span data-ttu-id="7346e-123">Írja be az URL-címet egy böngészőben megtekintheti a összevonási metaadat-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="7346e-123">You can view the federation metadata document by typing this URL in a browser.</span></span>

## <a name="contents-of-federation-metadata"></a><span data-ttu-id="7346e-124">Összevonási metaadatok tartalmát</span><span class="sxs-lookup"><span data-stu-id="7346e-124">Contents of federation Metadata</span></span>
<span data-ttu-id="7346e-125">A következő témakör szolgáltatásokról, amelyek az Azure AD által kiállított jogkivonatokat használják számára szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="7346e-125">The following section provides information needed by services that consume the tokens issued by Azure AD.</span></span>

### <a name="entity-id"></a><span data-ttu-id="7346e-126">Entitás azonosítója</span><span class="sxs-lookup"><span data-stu-id="7346e-126">Entity ID</span></span>
<span data-ttu-id="7346e-127">A `EntityDescriptor` elem tartalmazza-e egy `EntityID` attribútum.</span><span class="sxs-lookup"><span data-stu-id="7346e-127">The `EntityDescriptor` element contains an `EntityID` attribute.</span></span> <span data-ttu-id="7346e-128">Értékét a `EntityID` attribútum a kibocsátó, ez azt jelenti, hogy a biztonsági jogkivonat-szolgáltatás (STS) a jogkivonatot kibocsátó jelöli.</span><span class="sxs-lookup"><span data-stu-id="7346e-128">The value of the `EntityID` attribute represents the issuer, that is, the security token service (STS) that issued the token.</span></span> <span data-ttu-id="7346e-129">Fontos, amikor fogadhatnak jogkivonatot a kibocsátó érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7346e-129">It is important to validate the issuer when you receive a token.</span></span>

<span data-ttu-id="7346e-130">A következő metaadatokat megmutatja, bérlő-specifikus `EntityDescriptor` elem egy `EntityID` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-130">The following metadata shows a sample tenant-specific `EntityDescriptor` element with an `EntityID` element.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
<span data-ttu-id="7346e-131">A bérlői független végpont Bérlőazonosító lecserélheti egy bérlő-specifikus létrehozása a bérlő azonosítója `EntityID` érték.</span><span class="sxs-lookup"><span data-stu-id="7346e-131">You can replace the tenant ID in the tenant-independent endpoint with your tenant ID to create a tenant-specific `EntityID` value.</span></span> <span data-ttu-id="7346e-132">Az eredményül kapott érték a jogkivonat-kiállítóként ugyanaz lesz.</span><span class="sxs-lookup"><span data-stu-id="7346e-132">The resulting value will be the same as the token issuer.</span></span> <span data-ttu-id="7346e-133">A stratégia lehetővé teszi, hogy egy több-bérlős alkalmazást egy adott bérlő a kibocsátó érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="7346e-133">The strategy allows a multi-tenant application to validate the issuer for a given tenant.</span></span>

<span data-ttu-id="7346e-134">A következő metaadatokat megmutatja, bérlő független `EntityID` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-134">The following metadata shows a sample tenant-independent `EntityID` element.</span></span> <span data-ttu-id="7346e-135">Vegye figyelembe, hogy a `{tenant}` szövegkonstans, nem egy helyőrző.</span><span class="sxs-lookup"><span data-stu-id="7346e-135">Please note, that the `{tenant}` is a literal, not a placeholder.</span></span>

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a><span data-ttu-id="7346e-136">Jogkivonat-aláíró tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="7346e-136">Token signing certificates</span></span>
<span data-ttu-id="7346e-137">Ha a szolgáltatás egy jogkivonatot, amely egy Azure AD-bérlő által kiadott kap, az aláírás a jogkivonat az összevonási metaadatok dokumentum közzétett aláírókulccsal ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="7346e-137">When a service receives a token that is issued by a Azure AD tenant, the signature of the token must be validated with a signing key that is published in the federation metadata document.</span></span> <span data-ttu-id="7346e-138">Az összevonási metaadatok a bérlők számára használja a jogkivonat-aláíró tanúsítványokat nyilvános részét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7346e-138">The federation metadata includes the public portion of the certificates that the tenants use for token signing.</span></span> <span data-ttu-id="7346e-139">A tanúsítvány nyers bájt jelenik meg a `KeyDescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-139">The certificate raw bytes appear in the `KeyDescriptor` element.</span></span> <span data-ttu-id="7346e-140">A jogkivonat-aláíró tanúsítvány érvénytelen, csak ha aláírásra értékének a `use` attribútum `signing`.</span><span class="sxs-lookup"><span data-stu-id="7346e-140">The token signing certificate is valid for signing only when the value of the `use` attribute is `signing`.</span></span>

<span data-ttu-id="7346e-141">Az Azure AD által közzétett összevonási metaadatok dokumentum rendelkezhet több aláírási kulcs, például ha az Azure AD arra készül, hogy frissítse az aláíró tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="7346e-141">A federation metadata document published by Azure AD can have multiple signing keys, such as when Azure AD is preparing to update the signing certificate.</span></span> <span data-ttu-id="7346e-142">Ha egy összevonási metaadat-dokumentum tartalmazza az egynél több tanúsítvány, egy szolgáltatás, amely a jogkivonatok érvényesítése támogatnia kell a minden tanúsítvány a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="7346e-142">When a federation metadata document includes more than one certificate, a service that is validating the tokens should support all certificates in the document.</span></span>

<span data-ttu-id="7346e-143">A következő metaadatokat megmutatja, `KeyDescriptor` elem az aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="7346e-143">The following metadata shows a sample `KeyDescriptor` element with a signing key.</span></span>

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

<span data-ttu-id="7346e-144">A `KeyDescriptor` elem jelenik meg, két helyen az összevonási metaadatok dokumentum; a WS-összevonás-specifikus és SAML-specifikus szakaszát.</span><span class="sxs-lookup"><span data-stu-id="7346e-144">The `KeyDescriptor` element appears in two places in the federation metadata document; in the WS-Federation-specific section and the SAML-specific section.</span></span> <span data-ttu-id="7346e-145">Mindkét szakasz közzétett tanúsítványok ugyanaz lesz.</span><span class="sxs-lookup"><span data-stu-id="7346e-145">The certificates published in both sections will be the same.</span></span>

<span data-ttu-id="7346e-146">A WS-összevonás-specifikus a szakaszban egy WS-összevonás metaadat-olvasó olvashatja, a tanúsítványokat egy `RoleDescriptor` rendelkező elemet a `SecurityTokenServiceType` típusa.</span><span class="sxs-lookup"><span data-stu-id="7346e-146">In the WS-Federation-specific section, a WS-Federation metadata reader would read the certificates from a `RoleDescriptor` element with the `SecurityTokenServiceType` type.</span></span>

<span data-ttu-id="7346e-147">A következő metaadatokat megmutatja, `RoleDescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-147">The following metadata shows a sample `RoleDescriptor` element.</span></span>

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

<span data-ttu-id="7346e-148">A SAML-specifikus a szakaszban egy WS-összevonás metaadat-olvasó olvashatja, a tanúsítványokat egy `IDPSSODescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-148">In the SAML-specific section, a WS-Federation metadata reader would read the certificates from a `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="7346e-149">A következő metaadatokat megmutatja, `IDPSSODescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-149">The following metadata shows a sample `IDPSSODescriptor` element.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
<span data-ttu-id="7346e-150">Nincsenek különbségek vonatkozó bérlői és bérlői független tanúsítványok formátumban.</span><span class="sxs-lookup"><span data-stu-id="7346e-150">There are no differences in the format of tenant-specific and tenant-independent certificates.</span></span>

### <a name="ws-federation-endpoint-url"></a><span data-ttu-id="7346e-151">WS-Federation végponti URL-cím</span><span class="sxs-lookup"><span data-stu-id="7346e-151">WS-Federation endpoint URL</span></span>
<span data-ttu-id="7346e-152">Az összevonási metaadatok egyszeri bejelentkezés és kijelentkezés a WS-Federation protokoll egyetlen használja az Azure AD az URL-Címeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7346e-152">The federation metadata includes the URL that is Azure AD uses for single sign-in and single sign-out in WS-Federation protocol.</span></span> <span data-ttu-id="7346e-153">Ehhez a végponthoz megjelenik a `PassiveRequestorEndpoint` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-153">This endpoint appears in the `PassiveRequestorEndpoint` element.</span></span>

<span data-ttu-id="7346e-154">A következő metaadatokat megmutatja, `PassiveRequestorEndpoint` elem egy bérlő-specifikus végpont.</span><span class="sxs-lookup"><span data-stu-id="7346e-154">The following metadata shows a sample `PassiveRequestorEndpoint` element for a tenant-specific endpoint.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
<span data-ttu-id="7346e-155">A bérlői független végpont a WS-összevonás URL-cím jelenik meg a WS-Federation végpont a következő mintában látható módon.</span><span class="sxs-lookup"><span data-stu-id="7346e-155">For the tenant-independent endpoint, the WS-Federation URL appears in the WS-Federation endpoint, as shown in the following sample.</span></span>

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a><span data-ttu-id="7346e-156">SAML protokoll végponti URL-cím</span><span class="sxs-lookup"><span data-stu-id="7346e-156">SAML protocol endpoint URL</span></span>
<span data-ttu-id="7346e-157">Az összevonási metaadatok tartalmazzák az URL-címet, amely az egyszeri bejelentkezés és kijelentkezés a SAML 2.0 protokoll egyetlen használja az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7346e-157">The federation metadata includes the URL that Azure AD uses for single sign-in and single sign-out in SAML 2.0 protocol.</span></span> <span data-ttu-id="7346e-158">Ezeket a végpontokat megjelennek a `IDPSSODescriptor` elemet.</span><span class="sxs-lookup"><span data-stu-id="7346e-158">These endpoints appear in the `IDPSSODescriptor` element.</span></span>

<span data-ttu-id="7346e-159">A be- és kijelentkezési URL-cím látható a `SingleSignOnService` és `SingleLogoutService` elemeket.</span><span class="sxs-lookup"><span data-stu-id="7346e-159">The sign-in and sign-out URLs appear in the `SingleSignOnService` and `SingleLogoutService` elements.</span></span>

<span data-ttu-id="7346e-160">A következő metaadatokat megmutatja, `PassiveResistorEndpoint` egy bérlő-specifikus végpont.</span><span class="sxs-lookup"><span data-stu-id="7346e-160">The following metadata shows a sample `PassiveResistorEndpoint` for a tenant-specific endpoint.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

<span data-ttu-id="7346e-161">Hasonlóképpen a végpontokat a közös SAML 2.0 protokoll végpontok a közzétett a bérlő független összevonási metaadatok a következő mintában látható módon.</span><span class="sxs-lookup"><span data-stu-id="7346e-161">Similarly the endpoints for the common SAML 2.0 protocol endpoints are published in the tenant-independent federation metadata, as shown in the following sample.</span></span>

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
