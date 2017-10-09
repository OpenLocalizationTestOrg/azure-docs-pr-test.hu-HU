---
title: "az Azure-ban automatikus skálázás használatába aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale az erőforrás az Azure-ban."
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 6b3c3f4529018dcaf9691c538fec63dfbb3cea06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-autoscale-in-azure"></a>Ismerkedés az Azure-ban automatikus skálázás
Ez a cikk ismerteti, hogyan tooset az automatikus skálázási beállításokat az erőforrás hello Microsoft Azure-portálon.

Az Azure a figyelő automatikus skálázási csak toovirtual gép méretezési csoportok, felhőalapú szolgáltatások, Azure App Service-csomagokról és App Service-környezetek vonatkozik. 

## <a name="discover-hello-autoscale-settings-in-your-subscription"></a>Hello automatikus skálázási beállításokat az előfizetésében felderítése
Felderíthetők az összes hello erőforrásokat, amelynek automatikus skálázás alkalmazható Azure-figyelőben. Lépésenkénti útmutató lépéseit követve hello használata:

1. Nyissa meg hello [Azure-portálon.][1]
2. Kattintson a hello Azure figyelő ikonra hello bal oldali ablaktáblán.
  ![Nyissa meg az Azure-figyelő][2]
3. Kattintson a **automatikus skálázás** tooview mely automatikus skálázás összes hello erőforrásokat kell alkalmazni, a jelenlegi állapotuk automatikus skálázás együtt.
  ![Az Azure a figyelő automatikus skálázás felderítése][3]

Használhat hello keresőablak hello felső tooscope hello lista tooselect erőforrásokhoz egy adott erőforráscsoport, adott erőforrástípusokra, vagy egy adott erőforrás le.

Az egyes erőforrások hello aktuális példányszám és hello automatikus skálázás állapot talál. hello automatikus skálázás állapota lehet:

- **Nincs konfigurálva**: nincs engedélyezve automatikus skálázás még ennél az erőforrásnál.
- **Engedélyezett**: automatikus skálázás engedélyezett ehhez az erőforráshoz.
- **Letiltott**: automatikus skálázás le van tiltva ennél az erőforrásnál.

## <a name="create-your-first-autoscale-setting"></a>Az első automatikus skálázási beállítás létrehozása

Most már halad át egy egyszerű lépésenkénti útmutató toocreate az első automatikus skálázási beállítás.

1. Nyissa meg hello **automatikus skálázás** panel az Azure-figyelő, és válassza ki a megjeleníteni kívánt tooscale erőforrás. (hello lépések használni a webalkalmazáshoz tartozó App Service-csomagot. Is [az első ASP.NET-webalkalmazás létrehozása az Azure-ban 5 perc múlva.] [4])
2. Vegye figyelembe, hogy hello aktuális példányszám 1. Kattintson a **engedélyezze az automatikus skálázás**.
  ![Új webalkalmazás skálázási beállítást][5]
3. Adjon meg egy nevet hello skálázási beállítást, és kattintson **szabály hozzáadása**. Figyelje meg hello skálázási szabály beállítások hello jobb oldalán környezetben ablaktábla megnyitása. Alapértelmezés szerint ez a beállítás hello beállítás tooscale a példányok száma 1, ha hello processzorszázaléka hello erőforrás nagyobb, mint 70 százalék. Az alapértelmezett értéken hagyja, és kattintson a **Hozzáadás**.
  ![A webes alkalmazás skálázási beállítás létrehozása][6]
4. Most létrehozott az első skálázási szabályhoz. Vegye figyelembe, hogy UX azt javasolja, hogy ajánlott eljárásokat, és hogy hello "toohave ajánlott legalább egy skálázási szabály." toodo így:
  
    a. Kattintson a **szabály hozzáadása**. 

    b. Állítsa be **operátor** túl**kisebb, mint**.

    c. Állítsa be **küszöbérték** túl**20**.

    d. Állítsa be **művelet** túl**számolva csökkentése**.

   Most már rendelkeznie kell a skálázási beállítást, hogy méretezik out/méretezik a CPU-használat alapján.
   ![A skála CPU alapján][8]
5. Kattintson a **Save** (Mentés) gombra.

Gratulálunk! Most már sikeresen létrehozta a CPU-használat alapján a webalkalmazás első skálázási beállítás tooautoscale.

> [!NOTE] 
> hello azonos lépésekre vonatkozó tooget lépések a virtuálisgép-méretezési készlet vagy a felhőbeli szerepkör-szolgáltatás.

## <a name="other-considerations"></a>Egyéb szempontok
### <a name="scale-based-on-a-schedule"></a>A skála a ütemezés szerint
Ezenkívül tooscale alapján a CPU, beállíthatja a skála másképp hello hét adott napjaira.

1. Kattintson a **méretezési feltétel hozzáadása**.
2. Hello skála mód és hello szabályok beállítása az hello azonos hello alapértelmezett feltételként.
3. Válassza ki **ismételje meg az adott napokra** hello ütemezéshez.
4. Válassza ki a hello nap és hello kezdő/záró idő, amikor hello méretezési feltétel alkalmazni kívánja a.

![Skála feltétel ütemezésen alapuló][9]
### <a name="scale-differently-on-specific-dates"></a>Az adott dátumok másképp méretezése
Továbbá a CPU-alapú tooscale, adhatja meg a skála másképp konkrét dátumokat.

1. Kattintson a **méretezési feltétel hozzáadása**.
2. Hello skála mód és hello szabályok beállítása az hello azonos hello alapértelmezett feltételként.
3. Válassza ki **adja meg a kezdő és záró dátum** hello ütemezéshez.
4. Jelölje ki a hello kezdő és záró dátumát és hello kezdő/záró időpontját amikor hello méretezési feltételt kell alkalmazni.

![Skála feltétel dátumok alapján][10]

### <a name="view-hello-scale-history-of-your-resource"></a>Az erőforrás hello méretezési előzményeinek megtekintése
Amikor az erőforrás méretezése felfelé vagy lefelé, eseményt naplózott, hello műveletnaplóban. Megtekintheti hello méretezési előzmények az erőforrás hello az elmúlt 24 óra toohello váltásával **futtatási előzményei** fülre.

![futtatási előzményei][11]

Ha azt szeretné, hogy tooview hello teljes méretezési előzmények (az akár too90 nap), jelölje be **ide toosee további részleteket**. hello tevékenységnapló nyílik meg, előre kiválasztott erőforrás és kategória automatikus skálázási.

### <a name="view-hello-scale-definition-of-your-resource"></a>Hello méretezési meghatározása az erőforrás megtekintéséhez
Automatikus skálázás egy Azure Resource Manager szerinti erőforrás. Megtekintheti hello méretezési definition JSON toohello váltásával **JSON** fülre.

![Skála meghatározása][12]

Módosíthatja a JSON-ban közvetlenül, ha szükséges. Ezek a változások azok mentése után jelenik meg.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Tiltsa le az automatikus skálázás, és manuálisan méretezhető, a példányok
Előfordulhat, ha szeretné, hogy az aktuális skálázási beállítást toodisable, és manuálisan méretezhető, az erőforrás időpontokat.

Kattintson a hello **letiltásával** hello felső gombra.
![Tiltsa le az automatikus skálázás][13]

> [!NOTE] 
> Ez a beállítás letiltja a konfigurációt. Azonban is vissza tooit automatikus skálázás megismételni engedélyezése után. 

Beállíthatja, hogy szeretné-e tooscale toomanually példányok hello száma.

![Manuális méretezési készlet][14]

Gombra kattintva térhet vissza tooAutoscale mindig **engedélyezze az automatikus skálázás** , majd **mentése**.

## <a name="next-steps"></a>Következő lépések
- [Hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés az összes sikertelen automatikus skálázás méretezési-a/kibővített műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

