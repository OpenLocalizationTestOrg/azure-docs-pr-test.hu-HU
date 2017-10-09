---
title: "a webalkalmazás az App Service Environment-környezet v1 aaaCreate"
description: "Ismerje meg, hogyan toocreate webes alkalmazások és az app service-csomagokról a egy App Service Environment-környezet v1"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 322ef344517c54247b102fb4920e35645986ef98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Hozzon létre egy webalkalmazást az App Service Environment-környezet v1

> [!NOTE]
> Ez a cikk hello App Service Environment-környezet v1 tárgya.  App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Áttekintés
Ez az oktatóanyag bemutatja, hogyan toocreate webalkalmazások és az App Service-csomagok egy [App Service Environment-környezet v1](app-service-app-service-environment-intro.md) (ASE). 

> [!NOTE]
> Ha azt szeretné, toolearn hogyan toocreate egy webalkalmazást, de nem kell toodo legyen az App Service-környezetek, lásd: [.NET-webalkalmazás létrehozása](app-service-web-get-started-dotnet.md) vagy a más nyelv és keretrendszer oktatóprogramok kapcsolatos hello egyikét.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag feltételezi, hogy létrehozott egy App Service Environment-környezet. Ha, amely még nem végzett, lásd: [egy App Service Environment-környezet létrehozása](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Webalkalmazás létrehozása
1. A hello [Azure Portal](https://portal.azure.com/), kattintson a **új > Web + mobil > webalkalmazás**. 
   
    ![][1]
2. Válassza ki előfizetését.  
   
    Ha több előfizetéssel rendelkezik vegye figyelembe, hogy az alkalmazás az App Service-környezet toocreate, szüksége toouse hello ugyanazt az előfizetést, amelyet használt hello környezet létrehozásakor. 
3. Válasszon ki vagy hozzon létre egy erőforráscsoportot.
   
    *Erőforráscsoportok* engedélyezése meg toomanage Azure-erőforrások egységként és a kapcsolódó hasznosak, ha létrehozó *szerepköralapú hozzáférés-vezérlés* (RBAC) szabályok az alkalmazásokhoz. További információkért lásd: [Azure Resource Manager áttekintése][ResourceGroups]. 
4. Válassza ki, vagy hozzon létre egy App Service-csomag.
   
    *App Service-csomagok* kezelt készlet azokból a webalkalmazásokból.  Ha árképzés lehetőséget választja, hello ár általában alkalmazott toohello App Service-csomag toohello egyes alkalmazások helyett. Hello számítási kell fizetnie Service-környezetben példányok lefoglalt toohello ASE ahelyett, hogy az ASP van felsorolva.  webalkalmazás példánya hello számú tooscale vertikális felskálázás App Service-csomag hello példányait, és ez hatással van az összes hello webalkalmazások, hogy a tervben.  Néhány funkció, például hely tárhelyek vagy virtuális integráció belül hello csomag mennyiség korlátozások is.  További információkért lásd: [Azure App Service-csomagok áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)
   
    App Service-csomagok hello alapján hello terv neve alatt kell jegyezni hello helyen azonosíthatja a ASE a.  
   
    ![][5]
   
    Ha egy már meglévő App Service-csomag toouse az App Service Environment-környezetben, jelölje ki azt. Ha azt szeretné, hogy egy új App Service-csomag toocreate, lásd: a következő szakasz az ebben az oktatóanyagban hello [az App Service-csomag létrehozása az App Service-környezetek](#createplan).
5. Adja meg a webalkalmazás hello nevét, és kattintson **létrehozása**. 
   
    Ha a ASE használ egy külső VIP hello URL-CÍMÉT egy app Service-környezetben nem: [*sitename*]. [ *az App Service-környezet nevét*]. ahelyett, hogy p.azurewebsites.net [*sitename*]. azurewebsites.net
   
    Ha a ASE egy belső VIP majd hello URL-cím, egy alkalmazás használ, abban, hogy ASE elem: [*sitename*]. [ *ASE létrehozásakor megadott altartomány*]   
    Miután kiválasztotta az ASP ASE létrehozása során látni fogja a frissítés alatt hello altartomány **neve**

## <a name="createplan"></a>Az App Service-csomag létrehozása
Az App Service-környezetek App Service-csomagot hoz létre, amikor munkavégző a választott eltérőek, ahány megosztott dolgozókat-környezetben.  hello munkavállalók toouse rendelkezik hello néhányat a meglévők közül, lefoglalt toohello ASE hello rendszergazda  Ez azt jelenti, hogy toocreate egy új tervet kell toohave további lefoglalt munkavállalók tooyour ASE feldolgozókészletek mint hello példányok száma összesen összes már a tervek adott munkavégző készletét.  Ha nincs elég munkavállalók a ASE munkavégző készlet toocreate a tervet, toowork van szüksége a ASE admin tooget azokat hozzá.

Az App Service-csomagokról egy App Service Environment-környezet által üzemeltetett másik különbség kijelölés árképzési hello hiánya.  Ha az App Service-környezetek hello rendszer által felhasznált számítási erőforrások fizet, és nem rendelkeznek hozzáadott díjak hello csomagok számára az adott környezetben.  Általában az App Service-csomag létrehozásakor, válassza ki a tarifacsomagot, amely megadja, hogy a számlázási.  Az App Service Environment-környezet alapvetően helyen tartalom létrehozásához használható.  Kell fizetnie hello környezet és a nem toohost a tartalmat.

hello következő utasításokat bemutatják, hogyan toocreate egy App Service megtervezése hello oktatóanyag hello előző szakaszban leírtak szerint egy webalkalmazás létrehozása közben.

1. Kattintson a **hozzon létre új** a hello terv kiválasztási Felületet, és adja meg a csomag nevét, ahogy normális esetben tenné egy ASE kívül.
2. Válassza ki, hogy a kívánt toouse a hely objektumválasztó hello ASE.
   
    Mivel az App Service Environment-környezet tulajdonképpen egy saját telepítési helyét, a Location alatt látható. 
   
    ![][2]
   
    Egy hajlamosnak hello hely kiválasztása a kijelölés után az alkalmazásszolgáltatási csomag létrehozása felhasználói felületi hello frissíti.  hello hely most látható hello ASE rendszer hello nevét és hello régióban van, és a terv objektumválasztó árképzési hello egy alkalmazáskészlet munkavégző objektumválasztó cseréli.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>A feldolgozókészleten kiválasztása
Általában az Azure App Service és az App Service-környezetek kívül vannak 3 számítási méretét, hogy egy dedikált ár terv hello kiválasztását.  Hasonló módon az egy ASE is fel too3 készletek munkavállalók határozza meg és adja meg, hogy feldolgozókészletek használt hello számítási méretét.  Mi, amely azt jelenti, hogy az ASE, helyett egy tarifacsomagot számítási méretű App Service-csomag számára, hogy kiválassza úgynevezett hello bérlői egy *feldolgozókészletek*.  

hello munkavégző készlet felhasználói felülete látható hello számítási mérete az adott munkavégző készletét hello neve alatt használt.  hello elérhető mennyiség hivatkozik toohow nagy számítási példányok érhetők el a készlethez tartozó használja.  hello teljes licenckészletben lehet ténylegesen a megadottnál több példány, de ez az érték hivatkozik toosimply hány nincsenek használatban.  Ha tooadjust van szüksége az App Service Environment-környezet tooadd több számítási erőforrások lásd [az App Service-környezet konfigurálása](app-service-web-configure-an-app-service-environment.md).

![][4]

Ebben a példában csak két elérhető feldolgozókészletek láthatja. Ennek oka az, hello ASE rendszergazda csak lefoglalt állomások e két munkavégző készletekbe.  hello harmadik akkor jelenik meg azt a lefoglalt virtuális gépek esetén.  

## <a name="after-web-app-creation"></a>Webalkalmazás létrehozása után
Nincsenek néhány szempontjai futó webalkalmazások és -környezetben, figyelembe toobe igénylő App Service-csomagok kezelése.  

Ahogy azt korábban említettük, hello ASE hello tulajdonosa felelős hello rendszer hello méretétől és emiatt amelyekért felelősek is az App Service-csomagokról győződjön meg arról, hogy van-e elegendő kapacitása toohost hello szükséges. Ha nincs elérhető munkavállalók, akkor nem fogja tudni toocreate kell az App Service-csomag.  Ez történik akkor is igaz tooscaling kész a webalkalmazás.  Ha több példány van szüksége, akkor kellene lennie tooget az App Service Environment-környezet admin tooadd további munkavállalók.

A web app és az App Service-csomag létrehozása után egy jó ötlet tooscale azt be.  -Környezetben mindig szüksége az App Service-csomag tooprovide hibatűrési toohave legalább 2 példányok az alkalmazásokhoz.  Skálázás egy App Service-környezetben terv, hello ugyanaz, mint a normál hello App Service-csomag felhasználói felületén keresztül.  További információ a méretezés [hogyan tooscale egy webalkalmazást az App Service-környezetben](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
