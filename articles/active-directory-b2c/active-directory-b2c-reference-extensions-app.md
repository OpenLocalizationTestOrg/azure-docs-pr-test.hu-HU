---
title: "Bővítmények alkalmazás – az Azure AD B2C |} Microsoft Docs"
description: "B2c-bővítmények-alkalmazás hello visszaállítása"
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
ms.openlocfilehash: b36410c18314bd893dc669b49814fdcd77fae054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-extensions-app"></a>Az Azure AD B2C: Bővítmények alkalmazás

Az Azure AD B2C-címtárban jön létre, amikor egy alkalmazás nevű `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` hello új könyvtárán belül automatikusan létrejön. Az alkalmazás, a hivatkozott tooas hello **b2c-bővítmények-alkalmazás**, látható *App regisztrációk*. Az Azure AD B2C szolgáltatás toostore szereplő felhasználók és az egyéni attribútumok hello használják. Ha hello alkalmazást törlik, az Azure AD B2C nem működik megfelelően, és az éles környezetben milyen hatással lesz.

> [!IMPORTANT]
> Ne törölje hello b2c-bővítmények-alkalmazás, kivéve, ha azt tervezi, tooimmediately törlése a bérlő. Ha több mint 30 napig hello app marad törölt, felhasználói adatok végleg elvesznek.

## <a name="verifying-that-hello-extensions-app-is-present"></a>Jelen hello bővítmények alkalmazást ellenőrzése

jelen, amely a b2c-bővítmények-alkalmazás hello tooverify:

1. Az Azure AD B2C-bérlő belül kattintson a **további szolgáltatások** hello bal oldali navigációs menü.
1. Keresse meg, és nyissa meg a **App regisztrációk**.
1. Keressen olyan alkalmazás, amelynek kezdődik **b2c-bővítmények-alkalmazás**

## <a name="recover-hello-extensions-app"></a>Hello bővítmények app helyreállítása

Ha véletlenül törli a b2c-bővítmények-alkalmazás hello, hogy van-e 30 nap toorecover azt. Visszaállíthatja a hello alkalmazásokhoz hello Graph API használatával:

1. Keresse meg a túl[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Jelentkezzen be toohello hely hello Azure AD B2C-címtár globális rendszergazdája, amelyet toorestore törölt hello alkalmazás.
1. Egy HTTP GET hello URL ki `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` az api-version 1.6-os =. Győződjön meg arról, hogy tooreplace `{tenantName}` a bérlő névvel. Ez a művelet felsorolja az összes törölt hello belül az elmúlt 30 napban hello alkalmazások.
1. Hello alkalmazás található hello lista, amelyen hello karaktersorozattal kezdődő "b2c-bővítmény-alkalmazás", és másolja a `objectid` tulajdonság értéke.
1. Hello URL HTTP POST ki `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Cserélje le a hello `{OBJECTID}` hello hello URL-cím része `objectid` hello előző lépésben. 

Most tudnia kell túl[lásd: a visszaállított hello app](#verifying-that-the-extensions-app-is-present) a hello Azure-portálon.

> [!NOTE]
> Az alkalmazás csak akkor állítható vissza, ha törölték belül hello utolsó 30 nap. Ha több mint 30 napig lett, adat végleg elvesznek. Ha további segítségre van szüksége a fájl egy támogatási jegy.
