---
title: "Egyszeri bejelentkezési kimenő SAML protokoll aaaAzure |} Microsoft Docs"
description: Ez a cikk ismerteti a hello egyetlen Sign-Out SAML protokoll az Azure Active Directoryban
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 889c9b3397a601c16ba6971d2b15bfee305576de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Egyetlen kijelentkezési SAML protokoll
Az Azure Active Directory (Azure AD) által támogatott hello SAML 2.0 web browser egyetlen kijelentkezési-profilt. Az egyetlen kijelentkezési toowork megfelelően hello **LogoutURL** a hello alkalmazás explicit módon regisztrálni kell az Azure AD-alkalmazás regisztrációja során. Az Azure AD hello LogoutURL tooredirect felhasználók használ, miután a rendszer kijelentkezteti.

Hello munkafolyamat hello Azure AD-egyetlen kijelentkezési folyamat látható.

![Munkafolyamat egyetlen kijelentkezés](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest
cloud service küld hello egy `LogoutRequest` üzenet tooAzure AD tooindicate, hogy a munkamenet meg lett szakítva. hello következő cikkből megmutatja, `LogoutRequest` elemet.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest
Hello `LogoutRequest` küldött elem tooAzure AD a következő attribútumok hello van szükség:

* `ID`: Ez alapján azonosíthatják a hello kijelentkezési kérelmet. hello értékének `ID` kell nem kezdődhet számmal. hello általános gyakorlat az tooappend **azonosító** GUID toohello karakterláncos ábrázolása.
* `Version`: Állítsa be ennek az elemnek hello értékének túl**2.0**. Kötelezően megadandó érték.
* `IssueInstant`: Ez egy `DateTime` koordinálják világidő (UTC) értékű karakterlánc és [körbejárási formátumban ("no")](https://msdn.microsoft.com/library/az4se3k1.aspx). Az Azure AD egy ilyen típusú értéket vár, de nem érvényesíti.

### Kiállító
Hello `Issuer` eleme egy `LogoutRequest` pontosan egyeznie kell hello **ServicePrincipalNames** felhőszolgáltatásban hello Azure AD-ben. Általában ez a beállítás toohello **App ID URI** regisztrációja során meghatározott.

### NameID
hello értékének hello `NameID` elem pontosan egyeznie kell a hello `NameID` hello felhasználó kimenő aláírással.

## LogoutResponse
Az Azure AD küld egy `LogoutResponse` a válasz tooa `LogoutRequest` elemet. hello következő cikkből megmutatja, `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse
Az Azure AD beállítja hello `ID`, `Version` és `IssueInstant` hello értékek `LogoutResponse` elemet. Hello állítja `InResponseTo` elem toohello értékének hello `ID` hello attribútumának `LogoutRequest` , amely megállapított hello válasz.

### Kiállító
Az Azure AD beállítja ezt az értéket túl`https://login.microsoftonline.com/<TenantIdGUID>/` ahol <TenantIdGUID> hello bérlői azonosító hello Azure AD-bérlő.

hello tooevaluate hello értékének `Issuer` elem, hello használata hello értékének **App ID URI** regisztrációja során.

### status
Az Azure AD használ hello `StatusCode` hello elemének `Status` elem tooindicate hello sikerességét vagy sikertelenségét kijelentkezési. Ha hello kijelentkezési kísérlet sikertelen, hello `StatusCode` elem is tartalmazhat egyéni hibaüzenetek.
