---
title: "Active Directory-alkalmazást a regisztráció aaaAzure |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse hello Azure portál tooregister egy alkalmazás az Azure Active Directoryval"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 7dc7b89f-653f-405a-b5f4-2c1288720c15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: priyamo
ms.reviewer: elisol
ms.openlocfilehash: 0134e299dcc53919a6f789a0878a1cf64a8e244d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="register-your-application-with-your-azure-active-directory-tenant"></a>Az alkalmazás regisztrálása az Azure Active Directory-bérlő

Használható az Azure portál tooregister hello az alkalmazás az Azure Active Directory (Azure AD) bérlő. Hello alkalmazás Azonosítóját hoz létre, és lehetővé teszi a tooreceive jogkivonatokat.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és kattintson a **Hozzáadás**.
4. Hello utasításokat követve, és hozzon létre egy új alkalmazást. Ha szeretné, hogy a webes alkalmazások és natív alkalmazások példák, tekintse meg a [quickstarts](active-directory-developers-guide.md).
  * A webes alkalmazásokhoz, adja meg a hello **bejelentkezési URL-cím**, hello alap URL-CÍMÉT az alkalmazásához. Ez az adott felhasználó tud egyszerre bejelentkezni például `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Natív alkalmazások esetén adja meg a **átirányítási URI-**, mely az Azure AD tooreturn token válaszok használja. Adjon meg egy értéket adott tooyour alkalmazást. például`http://MyFirstAADApp`
5. Miután végrehajtotta a regisztráció, az Azure AD rendeli hozzá az alkalmazás egy egyedi ügyfél-azonosítókra, hello azonosítót.

## <a name="update-application-settings-from-hello-azure-portal"></a>Hello Azure portálra az alkalmazás beállításainak frissítése

Könnyen módosíthatja egy meglévő alkalmazást a beállításokat a hello Azure-portálon. Például érdemes lehet tooconfigure egy válasz URL-CÍMEN, amely van, ahol az Azure AD kibocsát token válaszokat. Akkor is érdemes lehet tooconfigure engedélyek tooother alkalmazások, a példány tooallow az alkalmazás tooaccess hello Microsoft Graph API-val. Ehhez minden keresztül hello alkalmazás beállításait tartalmazó lap.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és válassza ki az alkalmazás hello listáról.
4. Kattintson a **beállítások** tooopen hello alkalmazás hello beállítások oldalára.
  * Hello **tulajdonságok** lap lehetővé teszi, hogy módosítsa hello általános adatokat hello alkalmazáshoz. Ide tartoznak hello alkalmazásnév, hello bejelentkezési URL-cím és hello kijelentkezési URL-cím.
  * Hello **válasz URL-címek** lap lehetővé teszi a tooadd egy válasz URL-CÍMEN, amely ahol küld az Azure AD a token válaszokat.
  * Hello **tulajdonosok** lap lehetővé teszi a tooadd alkalmazástulajdonosok.
  * Hello **engedélyek** lap lehetővé teszi a hello app tooconfigure engedélyeit. Például tooaccess hello Microsoft Graph API-t, kattintson **Hozzáadás** válassza **Microsoft Graph** hello API-választó, majd válassza hello engedély szükséges, például **címtáradatok olvasása** .
  * Hello **kulcsok** lap lehetővé teszi a tooadd alkalmazás titkos kulcsok. hello titkos kulcs csak akkor látható, egyszer a létrehozás után azonnal ezért győződjön meg arról, hogy toocopy azt további használatra.

## <a name="use-hello-inline-manifest-editor"></a>Hello beágyazott jegyzék szerkesztő használata

Hello beágyazott jegyzék szerkesztő toomodify egyes alkalmazás tulajdonságait, amelyek nem érhetők el közvetlenül hello Azure-portálon is használhatja. Például használhatja toomodify hello alkalmazás App ID URI tooenable hello OAuth2.0 implicit engedélyezési folyamat hello alapértelmezett engedélyezési helyett engedélyezése vagy folyamata.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Válassza ki az Azure AD-bérlő fiókja hello jobb felső sarkában hello lap választásával.
3. Hello bal oldali navigációs ablaktábláján válassza **több szolgáltatások**, kattintson a **App regisztrációk**, és válassza ki az alkalmazás hello listáról.
4. Kattintson a **Manifest** a hello alkalmazás tooopen hello beágyazott jegyzék szerkesztő.
5. Közvetlenül hogy módosítások toohello manifest, és mentse azt, amikor készen áll. Alternatív megoldásként letöltheti a jegyzék tooopen hello azt a kedvenc szerkesztő és a feltöltése hello a jegyzékfájl frissítése.

## <a name="next-steps"></a>Következő lépések

1. Tekintse meg a hello [Quickstarts](active-directory-developers-guide.md) részletes forgatókönyvek végrehajtása az Azure AD hitelesítési kérelmek számára.
2. Tekintse meg a teljes listáját a mintakódok [GitHub](https://github.com/azure-samples).
