---
title: "aaaHow tooScale egy alkalmazást az App Service-környezetben"
description: "Egy alkalmazás az App Service-környezetek skálázás"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.openlocfilehash: 08916eac056c46bf8cb6edffbf96285317b32062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-apps-in-an-app-service-environment"></a>Alkalmazások méretezése App Service-környezetben
Az Azure App Service hello dolgot általában három méretezheti:

* terv díjszabása
* munkavégző mérete 
* példányok száma.

-Környezetben nincs szükség tooselect, vagy módosítsa hello terv árképzési van.  Képességek szempontjából ez ugyanis már azonos a prémium tarifacsomag a képesség szintjét.  

A tekintetben tooworker méretét, a hello ASE admin rendelhet hozzá hello számítási erőforrás toobe minden feldolgozókészletek használt hello méretét.  Amely: munkavégző készlet 1 P4 számítási erőforrással rendelkezhet, és munkavégző Pool 2 P1 a számítási erőforrásokat, ha szükséges.  Nincs toobe mérete sorrendben.  Itt hello dokumentum részleteket hello méretek és tarifacsomagját [Azure App Service szolgáltatás díjszabása][AppServicePricing].  Ennek következtében a beállítások a webalkalmazások és az App Service-csomag egy App Service Environment-környezet toobe skálázás hello:

* munkavégző készlet kiválasztása
* példányok száma

Hello segítségével módosítása vagy elem történik a megfelelő felhasználói felület esetében a ASE üzemeltetett App Service-csomag látható.  

![][1]

Az ASP túl hello elérhető számítási erőforrások száma a hello munkavégző készletét, amely az ASP nem növelheti.  Ha munkavégző készlethez tartozó erőforrások szükséges számítási kell tooget a ASE rendszergazda tooadd őket.  Az információk körül újrakonfigurálása a ASE adatolvasás hello itt: [hogyan tooConfigure App Service-környezet][HowtoConfigureASE].  Tootake előnyeit hello ASE automatikus skálázás szolgáltatások tooadd kapacitás ütemezés vagy mérőszámok alapján is érdemes lehet.  maga hello ASE környezet automatikus skálázás konfigurálásáról további információ: tooget [hogyan tooconfigure automatikus skálázás egy App Service Environment-környezet][ASEAutoscale].

Létrehozhat több app service-csomagokról különböző feldolgozókészletek a számítási erőforrásokat használ, vagy használhat hello azonos munkavégző készletét.  Például ha a munkavégző (10) elérhető számítási erőforrások Pool 1, használhatja a toocreate egy app service-csomag (6) számítási erőforrásokat használ, és egy második app service-csomag által használt (4) számítási erőforrásokat.

### <a name="scaling-hello-number-of-instances"></a>-Példányok méretezési hello száma
Ha a webalkalmazás létrehozása az App Service-környezetek 1 példány kezdődik.  Ezután a kibővíthető tooadditional példányok tooprovide további számítási erőforrásokat az alkalmazásra vonatkozóan.   

Ha a ASE rendelkezik elegendő kapacitással a akkor közérthető egyszerű.  App Service-csomag, amely tárolja a hello helyek tooscale szeretné, hogy fel, és válassza ki a skála tooyour nyissa meg.  Ekkor megnyílik a felhasználói felület, ahol manuálisan hello méretezési beállítása az ASP és automatikus skálázási szabályok konfigurálása a ASP hello.  egyszerűen állítsa be az alkalmazás toomanually méretezési ***szerint*** túl***manuálisan megadott példányszám***.  Itt húzza hello csúszkát szükséges toohello mennyiség, vagy adjon meg az hello mezőben következő toohello csúszkát.  

![][2] 

az ASP a ASE munkahelyi hello automatikus skálázási szabályok hello ugyanaz, mint általában.  Kiválaszthatja ***processzor*** alatt ***szerint*** és automatikus skálázás szabályt hoz létre, a processzor, vagy alapján az ASP használatával összetettebb szabályokat hozhat létre ***ütemezés és a teljesítmény szabályok ***.  toosee több töltse ki a adatait konfigurálása az automatikus skálázás használata hello útmutató Itt [alkalmazás skálázása az Azure App Service][AppScale]. 

### <a name="worker-pool-selection"></a>Munkavégző készlet kiválasztása
Ahogy azt korábban említettük, az ASP felhasználói felületi hello hello munkavégző készlet kiválasztása érhető el.  Az ASP tooscale szeretne, és válassza ki a feldolgozókészleten hello hello panel megnyitásához.  Látni fogja az összes hello feldolgozókészletek, amely az App Service-környezet be vannak állítva.  Ha csak egy feldolgozókészletek majd csak akkor jelenik meg hello felsorolt egy készletet.  toochange milyen munkavégző tárolókészlet az ASP, egyszerűen jelölje ki az App Service-csomag toomove a kívánt hello feldolgozókészletek.  

![][3]

Az ASP áthelyezése egy munkavégző készlet tooanother előtt fontos toomake meg arról, hogy az ASP megfelelő kapacitás követően.  A feldolgozókészletek hello listájában nem csak hello munkavégző alkalmazáskészlet neve szerepel, de azt is láthatja, hogy hány munkavállalók érhetők el, hogy feldolgozókészletek.  Győződjön meg arról, hogy van elég példányok elérhető toocontain az App Service-csomag.  Ha meg kell több számítási erőforrások hello feldolgozókészletek eléréséhez, majd a ASE rendszergazda tooadd toomove kívánja őket.  

> [!NOTE]
> Az hello alkalmazásoknak, hogy az ASP a áthelyezése egy munkavégző készletből egy ASP hatására cold kezdődik.  Ennek hatására kérelmek toorun lassan, az alkalmazás cold kezdete: hello új számítási erőforrásokat.  hello hidegindítás elkerülhetők hello segítségével [meleg funkció mentése alkalmazás] [ AppWarmup] az Azure App Service-ben.  hello alkalmazásinicializálás modul hello cikkben ismertetett is működik hidegindítások, mert hello inicializálási folyamatot is nyílik meg, ha azok cold kezdete: új számítási erőforrásokat. 
> 
> 

## <a name="getting-started"></a>Bevezetés
Lásd az App Service-környezetek lépései tooget [hogyan tooCreate egy App Service Environment-környezet][HowtoCreateASE]

Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
