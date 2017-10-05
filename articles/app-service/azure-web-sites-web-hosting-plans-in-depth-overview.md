---
title: "Az Azure App Service-csomagok részletes áttekintése |} Microsoft Docs"
description: "Ismerje meg, hogyan App Service-csomagok Azure App Service munka, és hogyan azok előnyeit, a kezelhetőséget."
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: f97be571d104e3cc1c6ee732886fa7133ba0dc83
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a>Az Azure App Service díjcsomagjainak részletes áttekintése

App Service-csomagokról az alkalmazások futtatásához használt fizikai erőforrások gyűjteményét képviseli.

Az App Service-csomagok a következőket határozzák meg:

- A régióban (USA nyugati régiója, USA keleti régiója, stb.)
- Méretezési száma (egy, két, három példányt, stb.)
- Példányméretének (Small, Medium, Large)
- Termékváltozat (Ingyenes, Közös, Alapszintű, Standard, Prémium)

Web Apps, Mobile Apps, az API Apps, függvény alkalmazások (vagy funkciók), a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) összes futtassa az App Service-csomagot.  Az ugyanahhoz az előfizetéshez, és a régió alkalmazások oszthatnak meg az App Service-csomag. 

Hozzárendelt összes alkalmazást egy **App Service-csomag** a meghatározott erőforrások megosztása. A megosztás menti a pénz, esetén több alkalmazást az önálló App Service-csomag.

Az **App Service-csomagok** szintje az **Ingyenes** és **Közös** termékváltozatoktól az **Alapszintű**, **Standard** és **Prémium** változatokig terjed. A magasabb szintű változatok több erőforrást és szolgáltatást biztosítanak.

Ha az App Service-csomag **alapvető** SKU vagy újabb verzióját, majd megadhatja a **mérete** , a méretezés, a virtuális gépek száma.

Például ha a terv normál rétegben két "kicsi" példányt használatára van konfigurálva, terv társított összes alkalmazás futtassa mindkét-példányokon. Alkalmazások is van a standard szint szolgáltatásainak eléréséhez. Alkalmazások futnak terv példányok teljes körűen felügyelt, és magas rendelkezésre állású.

> [!IMPORTANT]
> Az App Service-csomag **Termékváltozat** és **Méretezés** tulajdonsága a költségeket befolyásolja, nem a csomagban üzemeltetett alkalmazások számát.

Ez a cikk ismerteti, a kulcs jellemzőit, például a réteg és a méretezhetőség, az App Service-csomagot, és hogyan lépnek játékba, miközben az alkalmazások kezelését.

## <a name="apps-and-app-service-plans"></a>Alkalmazások és az App Service-csomagokról

Lehet, hogy az App Service alkalmazás társítva van egy adott időpontban csak egy App Service-csomag.

Alkalmazások és csomagok is tartalmaznak egy **erőforráscsoport**. Erőforráscsoport minden szereplő erőforrásra életciklus határa funkcionál. Erőforráscsoportok segítségével együtt kezelheti az alkalmazás minden a helyére.

Egyetlen erőforráscsoportként működnek rendelkezhet több App Service-csomagokról, mert rendel a különböző alkalmazások különböző fizikai erőforrásokhoz.

Például elkülönítheti a erőforrások fejlesztői, tesztelési és éles környezetek között. Éles üzemi pontjának és fejlesztési és tesztelési célú külön környezeteket rendelkező lehetővé teszi az erőforrások elkülönítése. Ezzel a módszerrel szemben az alkalmazások új verziójának tesztelése betöltése nem "versenyeznek" az az erőforrásokhoz, az éles alkalmazásokat, amelyek valós ügyfelek szolgál.

Ha több csomagokról egyetlen erőforráscsoportként működnek, megadhatja a földrajzi régió átfogó alkalmazás is.

Például egy magas rendelkezésre állású két régiókban futó alkalmazást tartalmaz legalább két terveket, minden egyes régió egy és minden terv társított egy alkalmazást. Ilyen esetben az alkalmazás összes példányát egyetlen erőforráscsoportként működnek majd tartalmazza. Több csomagokban és több alkalmazás erőforráscsoport rendelkező egyszerűen kezelése, szabályozása és az alkalmazás állapotának megtekintéséhez.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Az App Service-csomag létrehozása vagy meglévő ütemezés használata

Amikor létrehoz egy alkalmazást, érdemes egy erőforráscsoport létrehozása. Másrészről Ha az alkalmazás egy nagyobb alkalmazás egy összetevőjét, hozza létre, amely nagyobb alkalmazás számára kíván lefoglalni az erőforráscsoporton belül.

-E az alkalmazás egy teljesen új alkalmazás vagy nagyobb egy részét, dönthet úgy, hogy egy meglévő csomag használata, vagy hozzon létre egy újat. Ez a döntés nagyobb kapacitást és várható terhelési adott esetben.

Javasoljuk, hogy az alkalmazás egy új App Service-be elkülönítése ütemezésének:

- Alkalmazás erőforrás-igényes áll.
- Az alkalmazás rendelkezik egy meglévő csomagban tárolt más alkalmazásokból különböző méretezési tényező.
- Erőforrás más földrajzi régióban kell alkalmazást.

Ezzel a módszerrel lehet új készletét erőforrásokat lefoglalni az alkalmazás és az alkalmazások nagyobb ellenőrzést.

## <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

> [!TIP]
> Ha az App Service-környezetek, megtekintheti az adott dokumentációjában App Service Environment-környezetek innen: [egy App Service Environment-környezetben az App Service-csomag létrehozása](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Üres App Service-csomagot is létrehozhat, az App Service-csomag tallózási élménynek vagy alkalmazás létrehozásának részeként.

A a [Azure-portálon](https://portal.azure.com), kattintson a **új** > **Web + mobil**, majd válassza ki **webalkalmazás** vagy más App Service alkalmazás típusa.

![Alkalmazás létrehozása az Azure portálon.][createWebApp]

Ezután válassza ki vagy hozzon létre az új alkalmazás az App Service-csomag.

 ![Az App Service-csomag létrehozása.][createASP]

Az App Service-csomag létrehozásához kattintson a **[+] hozzon létre új**, típusa a **App Service-csomag** nevet, és válasszon egy megfelelő **hely**. Kattintson a **tarifacsomag**, majd válassza ki a megfelelő tarifacsomagot a szolgáltatáshoz. Válassza ki **összes** több, mint az beállítások árképzési nézetre **szabad** és **megosztott**. Miután kiválasztotta a tarifacsomagot, kattintson a **válasszon** gombra.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Az alkalmazások áthelyezése egy másik App Service-csomag

Az alkalmazások áthelyezése egy másik App Service-csomag a a [Azure-portálon](https://portal.azure.com). Mindaddig, amíg a ugyanazt az erőforráscsoportot és a földrajzi régióban találhatók a csomagok, alkalmazások áthelyezhet tervek.

Az alkalmazások áthelyezése másik terv:

- Nyissa meg az alkalmazást, amelynek át szeretné helyezni.
- Az a **menü**, keresse meg a **App Service-csomag** szakasz.
- Válassza ki **módosítás App Service-csomag** a folyamat elindításához.

**App Service-csomag módosítása** megnyitja a **App Service-csomag** választó. Ezen a ponton választhatja ki a meglévő séma ezzel az alkalmazással történő áthelyezése.

> [!IMPORTANT]
> A rendszer a következő feltételek alapján szűri a válassza ki az App Service-csomag felhasználói felület:
> - Az ugyanazon erőforráscsoporton belül van
> - Ugyanabban a földrajzi régióban található
> - Az azonos webtárhely belül van
>
> Egy webtárhely egy logikai szerkezet belül App Service, amely meghatározza a kiszolgáló erőforrások csoportosítása. Egy földrajzi régiót (például az USA nyugati régiója) használja az App Service ügyfelek lefoglalásához sok Webspaces tartalmazza. Jelenleg az App Service-erőforrások nem helyezhetők át Webspaces tudni.
>

![App Service-csomag választó.][change]

Minden egyes tervnek a saját IP-címek. Például egy webhely áthelyezése egy ingyenes szint a Standard szintű, hogy lehetővé teszi, hogy a szolgáltatások és erőforrások Standard csomagra hozzárendelt összes alkalmazáshoz.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Egy alkalmazás egy másik App Service-csomagra klónozása

Ha azt szeretné, az alkalmazás áthelyezése egy másik régióban, egy alternatív-e az alkalmazás a Klónozás. A Klónozás teszi az alkalmazás másolatát egy új vagy meglévő App Service-csomag bármely régióban.

Található **Klónozott alkalmazás** a a **Fejlesztőeszközök** a menü részét.

> [!IMPORTANT]
> A Klónozás rendelkezik néhány címen olvashat [Azure App Service-alkalmazást az Azure-portál használatával Klónozás](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Az App Service-csomag vertikális

A csomag skálázása három módja van:

- **Módosítsa a csomag csomagazonosítóját tarifacsomag**. Standard, és minden alkalmazás rendelt Standard csomagra szolgáltatásait is használni a csomag az alapszintű rétegben alakíthatók ki.
- **A csomag példányméretének módosítása**. Tegyük fel az alapszintű rétegben, amely használja a kisméretű példányok terv nagy példányok használandó módosítható. Minden alkalmazás társított terv most használhatja a további memória és a CPU-erőforrást, amely nagyobb példányméretének nyújt.
- **Módosítsa a terv példányszám**. Például a Standard tervet készíteni, amely három alkalmazáspéldányra horizontálisan is méretezhető 10 példányok. A prémium tervének kiterjeszthető 20 példányokhoz (rendelkezésre állási) feltételei érvényesek. Minden alkalmazás társított terv most használhatja a további memória és a Processzor-erőforrások kínál a nagyobb a példányok száma.

Az árképzési szint és a példány mérete kattintva módosíthatja a **vertikális Felskálázás** elem alatt az alkalmazás vagy az App Service-csomag beállításai. Változások az App Service-csomag vonatkoznak, és hatással rajta futó összes alkalmazásra.

 ![Méretezést kívánó alkalmazás értékeinek beállításához.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Az alkalmazásszolgáltatási csomag karbantartása

> [!IMPORTANT]
> **App Service-csomagok** , amelyek nem találhatók alkalmazások hozzájuk társított továbbra is függő díj terheli, mivel azok továbbra is számítási kapacitás lefoglalni.

Az utolsó az App Service-csomag tárolva alkalmazást törlik váratlan díjak elkerülése az eredményül kapott üres App Service-csomag is törlődik.

## <a name="summary"></a>Összefoglalás

App Service-csomagokról funkciók és kapacitás szempontjából lehet megosztani az alkalmazások csoportját képviselik. App Service-csomagokról rugalmasan, a konkrét alkalmazásokat rendelhet erőforrások olyan készletét, és tovább optimalizálhatja az Azure erőforrás-használat. Ezzel a módszerrel pénzt takaríthatnak meg a tesztelési környezetben, a kívánt is megoszthatja a csomag több alkalmazás között. Több régiók és tervek skálázással azt az éles környezetben is kihasználhatja átviteli sebesség.

## <a name="whats-changed"></a>A változások

- A módosítás a websites szolgáltatásról az App Service-re, váltásról: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
