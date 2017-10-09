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
ms.openlocfilehash: e666be4622ad30aaa1e12e49ae56bbe1e129b2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Hozzon létre egy alkalmazást (Express)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Adjon meg egy nevet az alkalmazás és az e-maileket
3.  Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.
4.  Hajtsa végre a hello utasításokat tooadd egy átirányítási URL-cím tooyour alkalmazás

## <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás
2. Adjon meg egy nevet az alkalmazás és az e-maileket 
3.  Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve
4.  Kattintson a `Add Platform`, majd jelölje be`Web`
5.  Lépjen vissza tooVisual Studio, és a Megoldáskezelőben, válassza ki a hello projektet, és (Ha nem látja a Tulajdonságok ablak az F4) hello tulajdonságai ablakban tekintse meg
6.  SSL engedélyezése túl módosítása`True`
7.  Hello SSL URL-cím másolása, és adja hozzá az URL-toohello listája az átirányítási URL-átirányítási URL-t hello regisztrációs portál közül:<br/><br/>![Projekt tulajdonságai](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Adja hozzá a hello következő `web.config` hello gyökérmappájában hello szakaszban található `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Cserélje le `ClientId` a hello csak regisztrált alkalmazás azonosítója
</li>
<li>
Cserélje le `redirectUri` a hello SSL URL-címet, a projekt
</li>
</ol>
<!-- End Docs -->
