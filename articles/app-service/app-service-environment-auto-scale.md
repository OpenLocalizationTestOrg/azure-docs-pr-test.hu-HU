---
title: "aaaAutoscaling és az App Service Environment-környezet 1-es verzió"
description: "Automatikus skálázás és az App Service Environment-környezet"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 1a03cf494309e80596b64471d1a067b2f64a9fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a>Automatikus skálázás és az App Service Environment-környezet 1-es verzió

> [!NOTE]
> Ez a cikk hello App Service Environment-környezet v1 tárgya.  App Service-környezet, amely könnyebben toouse, és nagyobb teljesítményű infrastruktúra futó hello újabb verziója van telepítve. több hello új verzióval kapcsolatos kezdődnie hello toolearn [bemutatása toohello App Service Environment-környezet](../app-service/app-service-environment/intro.md).
> 

Támogatja az Azure App Service-környezetek *automatikus skálázás*. Használhatja az automatikus skálázás egyedi feldolgozókészletek metrikák vagy ütemezés alapján.

![A feldolgozókészleten automatikus skálázás beállításai.][intro]

Automatikus skálázás optimalizálja az erőforrás-használat automatikusan növekvő, és az App Service-környezet toofit zsugorítását a keret és vagy profil.

## <a name="configure-worker-pool-autoscale"></a>Munkavégző készlet automatikus skálázás konfigurálása
Hello automatikus skálázási funkciót elérhető hello **beállítások** hello feldolgozókészletek lapján.

![Hello feldolgozókészletek beállítások lapján.][settings-scale]

Ott hello felületet kell viszonylag jól ismert, mert hello tervezze meg, hogy látni, amikor egy App Service méretezni ugyanazt a felhasználói élményt. 

![Manuális skálázási beállításokat.][scale-manual]

Az automatikus skálázási profilok is konfigurálhatja.

![Automatikus skálázási beállításokat.][scale-profile]

Automatikus skálázási profilok olyan hasznos tooset vonatkozó korlátozások a skála. Ezzel a módszerrel lehet egy egységes élmény úgy, hogy egy felső határa (2) egy alsó határ skálaértéknél (1) és egy előre jelezhető ráfordítás cap beállításával teljesítmény.

![A profil beállítását.][scale-profile2]

Után egy profil megadásához felfelé vagy lefelé hello feldolgozókészletek hello-profil által meghatározott hello határokon belül található példányok száma hello automatikus skálázási szabályok tooscale adhat hozzá. Automatikus skálázási szabályok metrikák alapulnak.

![Skálázási szabályhoz.][scale-rule]

 A feldolgozókészleten vagy előtér-metrikák használt toodefine automatikus skálázási szabályok lehet. Ezeket a mérési metrikák hello erőforrás panel diagramokban figyelése, vagy állítsa be a riasztások hello.

## <a name="autoscale-example"></a>Automatikus skálázás – példa
Egy App Service Environment-környezet automatikus skálázás legjobb érdekében a forgatókönyv segítségével mutatható be.

Ez a cikk ismerteti az összes hello szükséges szempontok automatikus skálázás beállításához. hello a cikk bemutatja, hogyan hello figyelembe a automatikus skálázás App Service Environment-környezetben üzemeltetett App Service-környezetek, amelyek kapcsolati lejátszását.

### <a name="scenario-introduction"></a>A forgatókönyv bemutatása
Frank a sysadmin (rendszergazda), akik át lett telepítve, hogy tooan App Service Environment-környezet kezel hello munkaterhelések része a vállalati.

hello App Service-környezet skálázási toomanual a következőképpen konfigurálva:

* **Első végződik:** 3
* **A feldolgozókészleten 1**: 10
* **A feldolgozókészleten 2**: 5
* **A feldolgozókészleten 3**: 5

1 feldolgozókészletek használjuk a termelési számítási feladatokhoz, feldolgozókészletek 2 és 3 feldolgozókészletek a minőségi megbízhatósági (QA) és a fejlesztői munkaterhelések szolgálnak.

hello App Service-csomagok a QA és fejlesztői toomanual méretezési konfigurálva. hello éles App Service-csomag tooautoscale toodeal rendelkező változata állítja be a terhelési és a forgalom.

Frank hello alkalmazással tisztában. Tudja, hogy a hello csúcsidőszakon terhelést 9:00-kor és 18:00:00 közé esnek, mivel ez egy alkalmazottak által használt során hello office-üzletági (LOB) alkalmazás. Ezt követően használati elutasítja azokat a felhasználókat adott napon szabásakor. Csúcsidőszakon kívüli nincs még néhány betöltése mert felhasználók férhetnek hozzá hello app távolról a mobileszközök és az otthoni számítógépek használatával. App Service-csomag már hello éles CPU-használat alapján a következő szabályok hello tooautoscale konfigurálva:

![Az ÜZLETÁGI alkalmazás beállításait.][asp-scale]

| **Automatikus skálázási profil – létrehozását – az App Service-csomag** | **Automatikus skálázási profil – hétvégéket – az App Service-csomag** |
| --- | --- |
| **Name:** hétköznap profil |**Name:** hétvégi profil |
| **A skála:** ütemezés és a teljesítmény szabályok |**A skála:** ütemezés és a teljesítmény szabályok |
| **Profil:** hétköznapokon |**Profil:** hétvégi |
| **Típus:** ismétlődési |**Típus:** ismétlődési |
| **Tartomány cél:** 5 too20 példányok |**Tartomány cél:** 3 too10 példányok |
| **Nap:** hétfő, kedd, szerda, csütörtök, péntek |**Nap:** , szombat és vasárnap |
| **Kezdési időpont:** 9:00-kor |**Kezdési időpont:** 9:00-kor |
| **Időzóna:** UTC-08 |**Időzóna:** UTC-08 |
|  | |
| **Automatikus skálázási szabály (vertikális Felskálázás)** |**Automatikus skálázási szabály (vertikális Felskálázás)** |
| **Erőforrás:** éles (App Service Environment-környezet) |**Erőforrás:** éles (App Service Environment-környezet) |
| **A metrika:** CPU % |**A metrika:** CPU % |
| **Művelet:** nagyobb, mint 60 % |**Művelet:** 80 %-nál nagyobb |
| **Időtartam:** 5 perc |**Időtartam:** 10 perc |
| **Összesítési idő:** átlagos |**Összesítési idő:** átlagos |
| **Művelet:** számának növeléséhez a 2. régiója |**Művelet:** számának növeléséhez 1 |
| **Ritkán használt adatok le (perc):** 15 |**Ritkán használt adatok le (perc):** 20 |
|  | |
| **Automatikus skálázási szabály (méretezési le)** |**Automatikus skálázási szabály (méretezési le)** |
| **Erőforrás:** éles (App Service Environment-környezet) |**Erőforrás:** éles (App Service Environment-környezet) |
| **A metrika:** CPU % |**A metrika:** CPU % |
| **Művelet:** 30 %-nál kisebb |**Művelet:** 20 %-nál kisebb |
| **Időtartam:** 10 perc |**Időtartam:** 15 perc |
| **Összesítési idő:** átlagos |**Összesítési idő:** átlagos |
| **Művelet:** számának csökkentése 1 |**Művelet:** számának csökkentése 1 |
| **Ritkán használt adatok le (perc):** 20 |**Ritkán használt adatok le (perc):** 10 |

### <a name="app-service-plan-inflation-rate"></a>Az alkalmazásszolgáltatási csomag inflációja
App Service-csomagok, amelyek a konfigurált tooautoscale ehhez óránként sebességet. Ezt az értéket a megadott automatikus skálázási szabály hello hello értékek alapján lehet kiszámolni.

Megismerés és kiszámítása hello *App Service-csomag inflációja* fontos az App Service-környezet automatikus skálázási, mert nincsenek azonnali méretezési módosítások tooa munkavégző készletét.

az alkalmazásszolgáltatási csomag inflációja hello kiszámítása a következőképpen történik:

![Az alkalmazásszolgáltatási csomag inflációja számítási.][ASP-Inflation]

Hello automatikus skálázás – hello hétköznap profil hello termelési App Service-csomag vertikális Felskálázás szabály alapján:

![App Service csomag infláció arány alapján automatikus skálázás – vertikális Felskálázás szabály létrehozását.][Equation1]

Az automatikus skálázás – hello hétvégi profil hello termelési App Service-csomag vertikális Felskálázás szabály hello hello esetben hello képlet kellene tartoznia:

![App Service csomag infláció arány hétvégéket automatikus skálázás – vertikális Felskálázás szabály alapján.][Equation2]

Ez az érték is kerülhet sor, lefelé méretezéshez műveletekhez.

Hello automatikus skálázás – hello hétköznap profil hello termelési App Service-csomag méretezési le szabály alapján ez lenne az alábbiak szerint:

![App Service csomag infláció arány alapján automatikus skálázás – méretezési le szabály létrehozását.][Equation3]

Az automatikus skálázás – hello hétvégi profil hello termelési App Service-csomag méretezési le szabály hello hello esetben hello képlet kellene tartoznia:  

![App Service csomag infláció arány hétvégéket automatikus skálázás – méretezési le szabály alapján.][Equation4]

hello éles App Service-csomag milyen mértékben növelhető a maximális sebesség hello héten nyolc példányok/óra és négy példányok óránként hello hétvégén. Azt is megjelenhetnek példányok négy példányok/óra hello héten sebességet és hat példányok/óra során tartozik.

Ha több App Service-csomagokról a feldolgozókészleten alatt tárolt, hogy van-e toocalculate hello *teljes inflációja* , hogy az összes olyan App Service-csomagok hello éppen hello inflációja hello munkavégző készlethez tartozó üzemeltetéséhez.

![A feldolgozókészleten üzemeltetett több App Service-csomagokról teljes inflációja számítását.][ASP-Total-Inflation]

### <a name="use-hello-app-service-plan-inflation-rate-toodefine-worker-pool-autoscale-rules"></a>App Service használata hello inflációja toodefine munkavégző készlet automatikus skálázási szabályok megtervezése
Munkavégző készletbe üzemeltető App Service-csomagok, amelyek a konfigurált tooautoscale kell kapacitás puffert lefoglalni. hello puffer lehetővé teszi, hogy hello automatikus skálázási műveletek toogrow, és az App Service-csomag igazodjon. hello minimális puffer hello számított teljes App Service megtervezése inflációja lesz.

App Service-környezet skálázási műveleteinek igénybe vehet néhány alkalommal tooapply, mert további fordulhat elő, amíg folyamatban van egy skálázási műveletet igény szerinti módosítások változásait kell fiókot. tooaccommodate a késés, azt javasoljuk, hogy használja a program teljes App Service megtervezése inflációja hello hello minden automatikus skálázási művelet hozzáadott-példányok minimális száma.

Az információ Frank hello adhatja meg a következő automatikus skálázási profil és a szabályok:

![Automatikus skálázási profil szabályok LOB például.][Worker-Pool-Scale]

| **Automatikus skálázási profil – hétköznapokon** | **Automatikus skálázási profil – hétvégéket** |
| --- | --- |
| **Name:** hétköznap profil |**Name:** hétvégi profil |
| **A skála:** ütemezés és a teljesítmény szabályok |**A skála:** ütemezés és a teljesítmény szabályok |
| **Profil:** hétköznapokon |**Profil:** hétvégi |
| **Típus:** ismétlődési |**Típus:** ismétlődési |
| **Tartomány cél:** 13 too25 példányok |**Tartomány cél:** 6 too15 példányok |
| **Nap:** hétfő, kedd, szerda, csütörtök, péntek |**Nap:** , szombat és vasárnap |
| **Kezdési időpont:** 7:00-kor |**Kezdési időpont:** 9:00-kor |
| **Időzóna:** UTC-08 |**Időzóna:** UTC-08 |
|  | |
| **Automatikus skálázási szabály (vertikális Felskálázás)** |**Automatikus skálázási szabály (vertikális Felskálázás)** |
| **Erőforrás:** feldolgozókészletek 1 |**Erőforrás:** feldolgozókészletek 1 |
| **A metrika:** WorkersAvailable |**A metrika:** WorkersAvailable |
| **Művelet:** legalább 8 |**Művelet:** legalább 3 |
| **Időtartam:** 20 perc |**Időtartam:** 30 perc |
| **Összesítési idő:** átlagos |**Összesítési idő:** átlagos |
| **Művelet:** számának növeléséhez 8 |**Művelet:** növeléséhez száma 3 |
| **Ritkán használt adatok le (perc):** 180 |**Ritkán használt adatok le (perc):** 180 |
|  | |
| **Automatikus skálázási szabály (méretezési le)** |**Automatikus skálázási szabály (méretezési le)** |
| **Erőforrás:** feldolgozókészletek 1 |**Erőforrás:** feldolgozókészletek 1 |
| **A metrika:** WorkersAvailable |**A metrika:** WorkersAvailable |
| **Művelet:** 8-nál nagyobb |**Művelet:** 3-nál nagyobb |
| **Időtartam:** 20 perc |**Időtartam:** 15 perc |
| **Összesítési idő:** átlagos |**Összesítési idő:** átlagos |
| **Művelet:** számának csökkentése 2 |**Művelet:** csökkentése száma 3 |
| **Ritkán használt adatok le (perc):** 120 |**Ritkán használt adatok le (perc):** 120 |

a példányok minimális hello az App Service-csomag hello-profil + puffer definiált hello hello profilban megadott cél tartomány számítja ki.

hello maximális tartomány lenne az összes hello maximális tartomány hello munkavégző készletben tárolt összes App Service-csomagokról hello összege.

hello hello skálázási szabályok száma növelni kell set tooat legalább 1 X az App Service csomag inflációja bővített fel.

Csökkentse száma lehet módosított toosomething 1/2 X között, vagy 1 X App Service megtervezése inflációja hello méretezési le.

### <a name="autoscale-for-front-end-pool"></a>Automatikus skálázás előtér-készlet
Az előtér-automatikus skálázási szabályok egyszerűbbek, mint a feldolgozókészletek. Elsősorban akkor  
Győződjön meg arról, hogy hello mérési és hello cooldown időzítők időtartama vegye figyelembe, hogy az App Service-csomagot a skálázási műveletek nem azonnali.

Ebben a forgatókönyvben a Frank tudja, hogy hello Hibaarány előtér-webkiszolgálóinak eléri a 80 %-os processzorhasználatot megnő, és beállítja hello automatikus skálázási szabály tooincrease példányok az alábbiak szerint:

![Automatikus skálázási beállításokat előtér-készlet.][Front-End-Scale]

| **Automatikus skálázási profil – első karakterlánccal végződik-e** |
| --- |
| **Name:** automatikus skálázás – első karakterlánccal végződik-e |
| **A skála:** ütemezés és a teljesítmény szabályok |
| **Profil:** mindennapi |
| **Típus:** ismétlődési |
| **Tartomány cél:** 3 too10 példányok |
| **Nap:** mindennapi |
| **Kezdési időpont:** 9:00-kor |
| **Időzóna:** UTC-08 |
|  |
| **Automatikus skálázási szabály (vertikális Felskálázás)** |
| **Erőforrás:** előtér-készlet |
| **A metrika:** CPU % |
| **Művelet:** nagyobb, mint 60 % |
| **Időtartam:** 20 perc |
| **Összesítési idő:** átlagos |
| **Művelet:** növeléséhez száma 3 |
| **Ritkán használt adatok le (perc):** 120 |
|  |
| **Automatikus skálázási szabály (méretezési le)** |
| **Erőforrás:** feldolgozókészletek 1 |
| **A metrika:** CPU % |
| **Művelet:** 30 %-nál kisebb |
| **Időtartam:** 20 perc |
| **Összesítési idő:** átlagos |
| **Művelet:** csökkentése száma 3 |
| **Ritkán használt adatok le (perc):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
