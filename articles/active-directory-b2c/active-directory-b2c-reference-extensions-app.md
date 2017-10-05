---
title: "Bővítmények alkalmazás – az Azure AD B2C |} Microsoft Docs"
description: "A b2c-bővítmények-alkalmazás visszaállítása"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: 17500b572a0e92c1c233c6967840a5b6d96e21cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-extensions-app"></a>Az Azure AD B2C: Bővítmények alkalmazás

Az Azure AD B2C-címtárban jön létre, amikor egy alkalmazás nevű `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` automatikusan létrejön az új könyvtárán belül. Az alkalmazáshoz, a továbbiakban a **b2c-bővítmények-alkalmazás**, látható *App regisztrációk*. Segítségével az Azure AD B2C szolgáltatás felhasználók és az egyéni attribútumok adatainak tárolására. Ha az alkalmazást törlik, az Azure AD B2C nem működik megfelelően, és az éles környezetben milyen hatással lesz.

> [!IMPORTANT]
> Ne törölje a b2c-bővítmények-alkalmazás, kivéve, ha azt tervezi, hogy a bérlő azonnal törli. Ha az alkalmazás több mint 30 napig törölt marad, felhasználói adatok végleg elvesznek.

## <a name="verifying-that-the-extensions-app-is-present"></a>Ellenőrzi, hogy jelen-e a bővítmények alkalmazás

Annak ellenőrzéséhez, hogy a b2c-bővítmények-alkalmazás jelen:

1. Az Azure AD B2C-bérlő belül kattintson a **további szolgáltatások** a bal oldali navigációs menü.
1. Keresse meg, és nyissa meg a **App regisztrációk**.
1. Keressen olyan alkalmazás, amelynek kezdődik **b2c-bővítmények-alkalmazás**

## <a name="recover-the-extensions-app"></a>A bővítmények app helyreállítása

Ha véletlenül törli a b2c-bővítmények-alkalmazás, végezze el a helyreállítást 30 napja van. Visszaállíthatja az alkalmazást, a Graph API-val:

1. Keresse meg a [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Jelentkezzen be a webhely, amelyet szeretne visszaállítani a törölt alkalmazást az Azure AD B2C könyvtár globális rendszergazdaként.
1. Az URL egy HTTP GET ki `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` az api-version 1.6-os =. Győződjön meg arról, hogy `{tenantName}` a bérlő névvel. Ez a művelet felsorolja összes alkalmazást, amely az elmúlt 30 napban törölve lett.
1. Alkalmazás található a listában, ahol a karaktersorozattal kezdődő "b2c-bővítmény-alkalmazás", és másolja a `objectid` tulajdonság értéke.
1. Az URL HTTP POST ki `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Cserélje le a `{OBJECTID}` URL-CÍMÉT a részét a `objectid` az előző lépésben. 

Meg kell tudni [lásd: a visszaállított app](#verifying-that-the-extensions-app-is-present) az Azure portálon.

> [!NOTE]
> Az alkalmazás csak akkor állítható vissza, ha törölve lett az utolsó 30 napban. Ha több mint 30 napig lett, adat végleg elvesznek. Ha további segítségre van szüksége a fájl egy támogatási jegy.