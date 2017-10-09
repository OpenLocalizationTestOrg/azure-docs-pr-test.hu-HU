---
title: "aaaRegister alkalmazást a hello Azure AD v2.0-végponttól hello portál használatával |} Microsoft Docs"
description: "Hogyan tooregister egy alkalmazást a Microsoft bejelentkezés engedélyezése és használata a Microsoft-szolgáltatások hello v2.0-végponttól"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: c56c98906656062435516e820cb318a04c03149c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooregister-an-app-with-hello-v20-endpoint"></a>Hogyan tooregister hello v2.0-végponttól alkalmazás
egy alkalmazás, amely egyaránt msa-t, és az Azure AD fogadja toobuild bejelentkezés, először egy alkalmazást a Microsoft tooregister.  Jelenleg nem képes toouse kell a meglévő alkalmazások, előfordulhat, hogy az Azure ad-vel vagy MSA - szüksége lesz egy újat márka toocreate.

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-hello-microsoft-app-registration-portal"></a>A Microsoft hello Microsoft app regisztrációs portál
Első lépések először - lépjen a túl[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Ez az egy új app regisztrációs portált, ahol kezelheti a Microsoft-alkalmazások.

Jelentkezzen be vagy egy személyes vagy a munkahelyi vagy iskolai Microsoft-fiókkal.  Ha még nem rendelkezik, vagy egy új személyes fiók. Lépjen tovább, nem tart sokáig - itt fogja várunk.

Tenni? Meg kell most megtekintik a Microsoft-alkalmazások listája olyan valószínűleg üres.  Módosítsuk, amely.

Kattintson a **hozzáadhat egy alkalmazást**, és adjon neki egy nevet.  hello portal rendeli az alkalmazás egy globálisan egyedi azonosítója, amelyet később a kódjában fog használni.  Ha az alkalmazás olyan kiszolgálóoldali összetevőt tartalmaz, hogy kell-e hozzáférési jogkivonatok hívó API-k (gondolja, hogy: Office, az Azure vagy a saját webes API-t), érdemes toocreate egy **Alkalmazáskulcsot** itt is.

Ezután adja hozzá a hello platformokat, amelyek az alkalmazás fogja használni.

* Webalapú alkalmazások esetén adja meg a **átirányítási URI-** ahol bejelentkezési üzenetek elküldhetők.
* A mobile apps szolgáltatásban a másolja le hello alapértelmezett átirányítási URI-címe, automatikusan létrejön.

Szükség esetén testre szabhatja, a bejelentkezési oldal a hello profilhoz című hello megjelenését és működését.  Győződjön meg arról, hogy tooclick **mentése** lépés előtt.

> [!NOTE]
> Amikor hoz létre egy alkalmazást, amely [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello otthoni bérlői hello, hogy használni kívánt fiók toosign hello portált hello alkalmazás regisztrálva lesz.  Ez azt jelenti, hogy nem regisztrálhatja az alkalmazás az Azure AD-bérlő személyes Microsoft-fiókkal.  Ha explicit módon kívánja tooregister egy alkalmazás egy adott bérlőn, olyan fiókkal jelentkezzen be, hogy a bérlő eredetileg létrehozott.
> 
> 

## <a name="build-a-quick-start-app"></a>A gyors üzembe helyezés alkalmazás létrehozása
Most, hogy a Microsoft app, befejezheti a v2.0 gyors üzembe helyezési oktatóprogramok valamelyikét.  Az alábbiakban néhány javaslatot olvashat:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

