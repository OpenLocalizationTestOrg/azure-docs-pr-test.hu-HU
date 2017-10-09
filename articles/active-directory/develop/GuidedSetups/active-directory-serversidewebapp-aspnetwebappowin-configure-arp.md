---
title: "aaaAzure AD v2 ASP.NET Web Server bevezetés - Config |} Microsoft Docs"
description: "A Microsoft bejelentkezés végrehajtási egy ASP.NET-megoldás a hagyományos böngészőalapú webalkalmazás a szabványos OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: badc47e131290a56a507592f944a0fc7093260a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="configure-your-aspnet-web-app-with-hello-applications-registration-information"></a>Konfigurálja az ASP.NET webalkalmazás hello alkalmazás regisztrációs adatai

Ebben a lépésben a projekt toouse SSL konfigurálása, és az alkalmazás regisztrációs adatokat hello SSL URL-cím tooconfigure használhatja. Ezt követően hello alkalmazás hozzáadása "regisztrációs információ tooyour megoldás a *web.config*.

1.  A Megoldáskezelőben, válassza ki a hello projekt, és nézze meg hello `Properties` (Ha nem látja a Tulajdonságok ablak az F4) ablak
2.  Változás `SSL Enabled` túl`True`
3.  Másolja a hello értéket `SSL URL` fent és beillesztheti hello `Redirect URL` hello oldal tetején lévő mezőbe, majd kattintson az *frissítés*:<br/><br/>![Projekt tulajdonságai](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
4.  Adja hozzá a hello következő `web.config` szakaszban legfelső mappában található fájl `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter hello application Id here]" />
<add key="RedirectUri" value="[Enter hello Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
