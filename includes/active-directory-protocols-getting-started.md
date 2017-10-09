---
title: "aaaAzure AD .NET protokoll – áttekintés |} Microsoft Docs"
description: "Toouse HTTP-üzenetek tooauthorize miként férhetnek hozzá az tooweb alkalmazások és a webes API-k használata az Azure AD-bérlőben."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
ms.openlocfilehash: 5bd54af028c445afd3f35d67d47d7c84b476c757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## Alkalmazás regisztrálása az AD-bérlőben
Először is szüksége lesz tooregister az alkalmazás és az Azure Active Directory (Azure AD) bérlő. Ez lesz az alkalmazás Azonosítóját biztosítanak, valamint tooreceive jogkivonatok engedélyezheti.

* Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).
* Válassza ki az Azure AD-bérlő parancsával hello jobb felső sarkában hello lap fiókjába.
* Hello bal oldali navigációs ablaktáblájában kattintson a **Azure Active Directory**.
* Kattintson az **Alkalmazásregisztrációk**, majd a **Hozzáadás** lehetőségre.
* Hello utasításokat követve, és hozzon létre egy új alkalmazást. Ebben az oktatóanyagban nincs jelentősége, hogy ez egy webalkalmazás vagy egy natív alkalmazás-e, de ha konkrét példákat szeretne látni a webalkalmazásokra vagy a natív alkalmazásokra, tekintse meg a [gyorsindítókat](../articles/active-directory/develop/active-directory-developers-guide.md).
  * A webes alkalmazásokhoz, adja meg a hello **bejelentkezési URL-cím** hello alap URL-CÍMÉT az alkalmazásához. Ez az adott felhasználó tud egyszerre bejelentkezni például `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Natív alkalmazások esetén adja meg a **átirányítási URI-**, mely az Azure AD tooreturn token válaszokat fogja használni. Adjon meg egy értéket adott tooyour alkalmazást. például`http://MyFirstAADApp`
* Miután végrehajtotta a regisztráció, az Azure AD rendelje hozzá a az alkalmazás egy egyedi ügyfél-azonosítót, hello azonosítót. Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás oldalról.
