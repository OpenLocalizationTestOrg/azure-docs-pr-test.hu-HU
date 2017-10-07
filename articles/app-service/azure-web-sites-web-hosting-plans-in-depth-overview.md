---
title: "aaaAzure App Service díjcsomagjainak részletes áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: b384790d9e69b234ca69ac591164c48a4b6ed210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a>Az Azure App Service díjcsomagjainak részletes áttekintése

App Service-csomagokról képviselő használt fizikai erőforrások toohost hello gyűjteményét az alkalmazásokat.

Az App Service-csomagok a következőket határozzák meg:

- A régióban (USA nyugati régiója, USA keleti régiója, stb.)
- Méretezési száma (egy, két, három példányt, stb.)
- Példányméretének (Small, Medium, Large)
- Termékváltozat (Ingyenes, Közös, Alapszintű, Standard, Prémium)

Web Apps, Mobile Apps, az API Apps, függvény alkalmazások (vagy funkciók), a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) összes futtassa az App Service-csomagot.  Alkalmazások hello ugyanazt az előfizetést, és régió megoszthatja az App Service-csomag. 

Minden alkalmazás hozzárendelt tooan **App Service-csomag** meghatározott hello erőforrások megosztása. A megosztás menti a pénz, esetén több alkalmazást az önálló App Service-csomag.

A **App Service-csomag** a méretezhető **szabad** és **megosztott** termékváltozatok túl**alapvető**, **szabványos**, és **Prémium** felkínálva termékváltozatok toomore erőforrások és szolgáltatások mentén hello eléréséhez módon.

Ha az App Service-csomag túl**alapvető** SKU vagy újabb verzióját, majd megadhatja a hello **mérete** , a méretezés hello virtuális gépek száma.

Például ha a csomag konfigurált toouse két "kicsi" példányok hello szabványos szolgáltatási rétegben, terv társított minden alkalmazást futtatnak mindkét esetben. Alkalmazások hozzáférési toohello szabványos réteg funkciókat is. Alkalmazások futnak terv példányok teljes körűen felügyelt, és magas rendelkezésre állású.

> [!IMPORTANT]
> Hello **SKU** és **méretezési** a hello App Service csomag határozza meg, hogy hello költségek és nincs hello azt az üzemeltetett alkalmazásokhoz.

Ez a cikk ismerteti, hello főbb jellemzőit, például a réteg és az App Service-csomag mérete, és hogyan az alkalmazások kezelését közben játékba lépnek.

## <a name="apps-and-app-service-plans"></a>Alkalmazások és az App Service-csomagokról

Lehet, hogy az App Service alkalmazás társítva van egy adott időpontban csak egy App Service-csomag.

Alkalmazások és csomagok is tartalmaznak egy **erőforráscsoport**. Erőforráscsoport hello életciklus határ minden erőforráshoz, amely hozzá lesz. Használható erőforrás-csoportok toomanage összes hello darab kérelem együtt.

Mivel egyetlen erőforráscsoportként működnek lehet több App Service-csomagokról, foglalhatja különböző alkalmazások toodifferent fizikai erőforrásokat.

Például elkülönítheti a erőforrások fejlesztői, tesztelési és éles környezetek között. Éles üzemi pontjának és fejlesztési és tesztelési célú külön környezeteket rendelkező lehetővé teszi az erőforrások elkülönítése. Ezzel a módszerrel szemben az alkalmazások új verziójának tesztelése betöltése nem "versenyeznek" az hello ugyanaz az erőforrásokhoz, mint az éles alkalmazásokat, amelyek valós ügyfelek szolgál.

Ha több csomagokról egyetlen erőforráscsoportként működnek, megadhatja a földrajzi régió átfogó alkalmazás is.

Például egy magas rendelkezésre állású két régiókban futó alkalmazást tartalmaz legalább két terveket, minden egyes régió egy és minden terv társított egy alkalmazást. Ilyen esetben minden hello másolatot hello alkalmazás egyetlen erőforráscsoportként működnek majd tartalmazza. Több csomagokban és több alkalmazás erőforráscsoport rendelkező teszi könnyen toomanage, a vezérlési és az alkalmazás hello hello állapotának megtekintése.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Az App Service-csomag létrehozása vagy meglévő ütemezés használata

Amikor létrehoz egy alkalmazást, érdemes egy erőforráscsoport létrehozása. Hello ugyanakkor, ha az alkalmazás egy nagyobb alkalmazás egy összetevőjét hozza létre azt az adott nagyobb alkalmazáshoz lefoglalt hello erőforráscsoporton belül.

Hogy hello app egy teljesen új alkalmazás vagy nagyobb egy részét, dönthet úgy toouse egy meglévő terv toohost, vagy hozzon létre egy újat. Ez a döntés nagyobb kapacitást és várható terhelési adott esetben.

Javasoljuk, hogy az alkalmazás egy új App Service-be elkülönítése ütemezésének:

- Alkalmazás erőforrás-igényes áll.
- Az alkalmazás rendelkezik a hello különböző méretezési tényező más egy meglévő csomagban üzemeltetett alkalmazásokhoz.
- Erőforrás más földrajzi régióban kell alkalmazást.

Ezzel a módszerrel lehet új készletét erőforrásokat lefoglalni az alkalmazás és az alkalmazások nagyobb ellenőrzést.

## <a name="create-an-app-service-plan"></a>App Service-csomag létrehozása

> [!TIP]
> Ha az App Service-környezetek, áttekintheti a hello dokumentáció adott tooApp Service-környezetek innen: [egy App Service Environment-környezetben az App Service-csomag létrehozása](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Üres App Service-csomagot is létrehozhat, az App Service-csomag tallózási élménynek hello vagy alkalmazás létrehozásának részeként.

A hello [Azure-portálon](https://portal.azure.com), kattintson a **új** > **Web + mobil**, majd válassza ki **Web App** vagy más App Service alkalmazás típusa.

![Alkalmazás létrehozása az Azure-portálon hello.][createWebApp]

Ezután válassza ki vagy hello App Service-csomag hello új alkalmazás létrehozása.

 ![Az App Service-csomag létrehozása.][createASP]

toocreate App Service-csomagot, kattintson a **[+] hozzon létre új**, típus hello **App Service-csomag** nevet, és válasszon egy megfelelő **hely**. Kattintson a **tarifacsomag**, majd válassza ki egy megfelelő tarifacsomagot hello szolgáltatáshoz. Válassza ki **összes** több, mint az beállítások árképzési tooview **szabad** és **megosztott**. Hello tarifacsomag kiválasztása után kattintson a hello **válasszon** gombra.

## <a name="move-an-app-tooa-different-app-service-plan"></a>Az alkalmazás tooa másik App Service-csomag áthelyezése

Az alkalmazás tooa másik App Service-csomag hello mozgathatja [Azure-portálon](https://portal.azure.com). Alkalmazások között áthelyezheti tervek mindaddig, amíg hello csomagok vannak hello ugyanazt az erőforráscsoportot és földrajzi régióban.

az alkalmazás tooanother csomag toomove:

- Keresse meg, hogy szeretné-e toomove toohello alkalmazás.
- A hello **menü**, keressen a hello **App Service-csomag** szakasz.
- Válassza ki **módosítás App Service-csomag** toostart hello folyamat.

**App Service-csomag módosítása** megnyílik hello **App Service-csomag** választó. Ezen a ponton választhatja ki egy meglévő terv toomove be ezt az alkalmazást.

> [!IMPORTANT]
> a rendszer hello a következő feltételek alapján szűri hello jelölje be az alkalmazásszolgáltatási csomag felhasználói felületén:
> - Létezik hello belül azonos erőforráscsoport
> - Hello létezik azonos földrajzi régióban
> - Létezik hello belül azonos webtárhely
>
> Egy webtárhely egy logikai szerkezet belül App Service, amely meghatározza a kiszolgáló erőforrások csoportosítása. Egy földrajzi régiót (például az USA nyugati régiója) sorrendben tooallocate ügyfelek használja az App Service számos Webspaces tartalmazza. Jelenleg az App Service-erőforrások nem képes toobe Webspaces között.
>

![App Service-csomag választó.][change]

Minden egyes tervnek a saját IP-címek. Egy hely áthelyezése a ingyenes szint tooa Standard szintű, például lehetővé teszi a tooit toouse hello szolgáltatásokat és erőforrásokat a hello Standard csomagra hozzárendelt összes alkalmazáshoz.

## <a name="clone-an-app-tooa-different-app-service-plan"></a>Az alkalmazás tooa másik App Service-csomag klónozása

Ha azt szeretné, hogy toomove hello app tooa más régióban, egy másik app-e a Klónozás. A Klónozás teszi az alkalmazás másolatát egy új vagy meglévő App Service-csomag bármely régióban.

Található **Klónozott alkalmazás** a hello **Fejlesztőeszközök** hello menü részét.

> [!IMPORTANT]
> A Klónozás rendelkezik néhány címen olvashat [Azure App Service-alkalmazást az Azure-portál használatával Klónozás](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Az App Service-csomag vertikális

Nincsenek három módon tooscale terv:

- **Hello terv tartozó IP-címek**. Egy alapszintű hello réteg terv konvertált tooStandard, és minden alkalmazás hozzárendelt tooit toouse hello funkcióit hello Standard csomagra.
- **Hello csomag példányméretének módosítása**. Tegyük fel a terv hello alapszintű rétegben, amely használja a kisméretű példányok módosított toouse nagy példányok is lehet. Minden alkalmazás társított terv most hello további memória és a példány mérete nagyobb ajánlatok hello Processzor-erőforrások használhatja.
- **Hello terv példányszám módosítása**. Például a Standard csomag kimenő toothree példányok méretezett lehet méretezett too10 példányok. A prémium tervének kiterjeszthető too20 példányok (tulajdonos tooavailability). Minden alkalmazás társított terv most hello további memória és a példányok száma nagyobb ajánlatok hello Processzor-erőforrások használhatja.

Módosíthatja az árképzési szint és a példány mérete hello kattintva hello **vertikális Felskálázás** elem hello alkalmazás vagy a hello App Service-csomag beállításai alatt. Módosítások toohello App Service-csomag alkalmazása, és hatással rajta futó összes alkalmazásra.

 ![Állítsa be az értékeket tooscale be az alkalmazást.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Az alkalmazásszolgáltatási csomag karbantartása

> [!IMPORTANT]
> **App Service-csomagok** , amelyek nem alkalmazásokkal kapcsolatos toothem továbbra is függő díj terheli, mivel azok tooreserve hello számítási kapacitás.

tooavoid váratlan költségek, a törölt hello utolsó alkalmazás az App Service-csomag tárolva hello eredményül kapott üres App Service-csomag is törlődik.

## <a name="summary"></a>Összefoglalás

App Service-csomagokról funkciók és kapacitás szempontjából lehet megosztani az alkalmazások csoportját képviselik. App Service-csomagok biztosít rugalmasságot tooallocate meghatározott alkalmazások tooa erőforráskészlethez hello és tovább optimalizálhatja az Azure erőforrás-használat. Így ha toosave pénzt takaríthat meg a tesztkörnyezet megoszthatja a tervet az több alkalmazást. Több régiók és tervek skálázással azt az éles környezetben is kihasználhatja átviteli sebesség.

## <a name="whats-changed"></a>A változások

- Webhelyek tooApp szolgáltatás útmutató toohello módosítást, lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
