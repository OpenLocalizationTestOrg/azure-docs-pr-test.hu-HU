---
title: "aaaAzure AD SAML protokoll referenciája |} Microsoft Docs"
description: "Ez a cikk áttekintést hello egyszeri bejelentkezéshez, és egyetlen Sign-Out SAML profilok az Azure Active Directoryban."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: priyamo
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: d712289b16dc40a6b43a96fadef729c55cdaac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Hogyan használja az Azure Active Directory a hello SAML protokoll
Az Azure Active Directory (Azure AD) használja a SAML 2.0 protokoll tooenable alkalmazások egyszeri bejelentkezés tooprovide élmény tootheir felhasználók hello. Hello [egyszeri bejelentkezés](active-directory-single-sign-on-protocol-reference.md) és [kijelentkezési egyetlen](active-directory-single-sign-out-protocol-reference.md) SAML-profilok az Azure AD azt ismertetik, hogyan helyességi feltételek, a protokollok és a kötések SAML használt hello identity provider szolgáltatás.

SAML protokoll hello identitásszolgáltató (az Azure AD) és hello szolgáltató (hello alkalmazás) tooexchange adatainak magukról igényel.

Amikor egy alkalmazás regisztrálása az Azure AD hello alkalmazás fejlesztőjének az Azure AD összevonási kapcsolatos információk regisztrálja. Ez magában foglalja a hello **átirányítási URI-** és **metaadatok URI** hello alkalmazás.

Az Azure AD használ hello **metaadatok URI** a hello cloud service tooretrieve hello aláírási kulcs és hello kijelentkezési hello felhőalapú szolgáltatás URI. Ha hello az alkalmazás nem támogatja a metaadatok URI, hello fejlesztői kell forduljon a Microsoft támogatási tooprovide hello kijelentkezési URI és az aláírási kulcs.

Az Azure Active Directory bérlői-specifikus és közös (bérlői független) egy bejelentkezés és az egyetlen kijelentkezési végpontok mutatja. URL-címmel rendelkező helyek képviselő – nincsenek csak egy azonosítók – így elvégezheti a toohello végpont tooread hello metaadatok.

* Itt található: hello bérlői-specifikus végpont `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Hello <TenantDomainName> helyőrző egy regisztrált tartománynév vagy a TenantID GUID Azonosítóját az Azure AD-bérlő. Például hello összevonási metaadatok hello contoso.com bérlő jelenleg: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Itt található: hello bérlői független végpont `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. A végpont címét **közös** megjelenik, helyett egy bérlői tartomány neve vagy azonosítója.

Az Azure AD-közzétevő hello összevonási metaadatok dokumentumok kapcsolatos információkért lásd: [összevonási metaadatok](active-directory-federation-metadata.md).
