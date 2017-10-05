---
title: "A Mcirosoft Power BI Embedded bemutatása"
description: "Power BI Embedded, interaktív Power BI-jelentések felvétele üzletiintelligencia-alkalmazásokba"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 4afa8d2c7f8ec1942521ba5fa131967dfd581c91
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>A Mcirosoft Power BI Embedded bemutatása

Az Azure-szolgáltatások körébe tartozó **Power BI Embedded** segítségével az alkalmazásfejlesztők interaktív Power BI-jelentéseket vehetnek fel saját alkalmazásaikba. A **Power BI Embedded** úgy működik együtt a meglévő alkalmazásokkal, hogy nem kell újratervezni vagy módosítani a felhasználók bejelentkezésének módját.

A **Microsoft Power BI Embedded** erőforrásai az [Azure ARM API-k](https://msdn.microsoft.com/library/mt712306.aspx) használatával hozhatók létre. Ebben az esetben a létrehozott erőforrás egy **Power BI munkaterület-csoport**.

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Munkaterület-csoport létrehozása

A **munkaterület-csoportok** a legfelsőbb szintű Azure-erőforrások, melyek az alkalmazásba beágyazandó tartalmat tárolják. A **munkaterület-csoportok** kétféleképpen hozhatók létre:

* Manuálisan, az Azure Portal segítségével
* Programozott módon, Azure Resource Manager (ARM) API-k segítségével

Az alábbiakban lépésről lépésre bemutatjuk, hogyan hozhat létre egy **munkaterület-csoportot** az Azure Portal segítségével.

1. Nyissa meg az **Azure Portalt** és jelentkezzen be: [http://portal.azure.com](http://portal.azure.com).
2. Kattintson a **+Új** lehetőségre az ablak tetején.
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. Az **Adatok + analitika** területen kattintson a **Power BI Embedded** elemre.
4. A **Munkaterület-csoport panelen** adja meg a szükséges információkat. A **Díjszabásról** további információt [A Power BI Embedded díjszabása](http://go.microsoft.com/fwlink/?LinkID=760527) című témakörben talál.
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. Kattintson a **Létrehozás** gombra.

A **munkaterület-csoport** pár percen belül létrejön. Ha ez elkészült, megnyílik a **Munkaterület-csoport panel**.

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

A **Létrehozás panelen** találhatók a munkaterületek létrehozásáért és a tartalom telepítéséért felelős API-k meghívásához szükséges adatok.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>A Power BI API elérési kulcsainak megtekintése

A Power BI REST API-k meghívásához szükséges legfontosabb információt az **elérési kulcsok** jelentik. Segítségükkel lehetséges ugyanis az API-kérelmek hitelesítéséhez szükséges **alkalmazási jogkivonatok** létrehozása. Az **elérési kulcsok** megtekintéséhez a **Beállítások panelen** kattintson az **Elérési kulcsok** lehetőségre. További információk az **alkalmazás-jogkivonatokról**: [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md) (Hitelesítés és engedélyezés a Power BI Embedded használatával).

   ![](media/power-bi-embedded-get-started/access-keys.png)

Láthatja, hogy két kulccsal rendelkezik.

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

Készítsen másolatot a kulcsokról, és tárolja őket biztonságos helyen az alkalmazáson belül. Nagyon fontos, hogy ezeket a kulcsokat úgy kezelje, mintha jelszavak volnának, mivel a segítségükkel a **munkaterület-csoport** minden tartalmához hozzá lehet férni.

Jóllehet két kulcs van listázva, egyszerre azonban mindig csak az egyikre lesz szüksége. A második kulcsra azért van szükség, hogy a kulcsok rendszeres újragenerálása ne zavarja meg a szolgáltatáshoz való hozzáférést.

Most, hogy rendelkezik **elérési kulcsokkal** és az alkalmazáshoz való Power BI-példánnyal, jelentést importálhat az alkalmazásba. Mielőtt megismertetnénk a jelentések importálásának módjával, a következő szakasz azt mutatja be, hogyan hozhatók létre az alkalmazásba beágyazandó Power BI-adatkészletek és jelentések.

## <a name="working-with-workspaces"></a>A munkaterületek használata

Munkaterület-csoport létrehozása után létre kell hoznia egy olyan munkaterületet, amely helyet biztosít a jelentéseknek és az adatkészleteknek. Munkaterület létrehozásához szüksége lesz a [Munkaterület küldése REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx)-ra.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app-using-power-bi-desktop"></a>Power BI-adatkészletek és -jelentések létrehozása és beágyazása egy Power BI Desktopot használó alkalmazásba

Most, hogy rendelkezik **elérési kulcsokkal**, és létrehozta az alkalmazáshoz való Power BI-példányt, el kell készítenie a beágyazni kívánt Power BI-adatkészleteket és jelentéseket. Az adatkészletek és a jelentések a **Power BI Desktop** segítségével hozhatók létre. A [Power BI Desktop ingyenesen](https://go.microsoft.com/fwlink/?LinkId=521662) letölthető. Vagy, a gyorsabb kezdéshez, letöltheti a [Kiskereskedelmi elemzési minta PBIX-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).

> [!NOTE]
> A **Power BI Desktop** működéséről további információt a [Bevezetés a Power BI Desktop használatába](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop) című témakörben talál.

A **Power BI Desktop** segítségével úgy csatlakozhat az adatforráshoz, hogy a **Power BI Desktopba** importálja az adatok másolatát, de a **DirectQueryt** is használhatja ugyanerre a célra.

Az alábbiakban a két módszer (**importálás** és **DirectQuery**) közti különbségeket láthatja.

| Importálás | DirectQuery |
| --- | --- |
| Ezzel a módszerrel a táblákat, oszlopokat *és adatokat* is a **Power BI Desktopba** importálhatja vagy másolja. A megjelenítésekkel végzett munka során a **Power BI Desktop** lekéri az adatok másolatát. A mögöttes adatok módosulásainak megtekintéséhez újból frissítenie vagy importálnia kell a teljes aktuális adatkészletet. |Ezzel a módszerrel csak *a táblákat és oszlopokat* importálhatja vagy másolhatja a **Power BI Desktopba**. A megjelenítésekkel végzett munka során a **Power BI Desktop** lekéri a mögöttes adatokat, azaz Önnek mindig az aktuális adatok állnak a rendelkezésére. |

Az adatforráshoz való csatlakozásról további információt a [Csatlakozás az adatforráshoz](power-bi-embedded-connect-datasource.md) című témakörben talál.

A **Power BI Desktopban** végzett munka mentésekor létrejön egy PBIX-fájl. Ez tartalmazza a jelentést. Ha importálja az adatokat, a PBIX-fájl a teljes adatkészletet tartalmazza, ha azonban a **DirectQueryt** használja, a PBIX-fájlban csak az adatkészlet sémája található. A [Power BI importálási API-jával](https://msdn.microsoft.com/library/mt711504.aspx) programozott módon telepítheti a PBIX-fájlt a munkaterületre.

> [!NOTE]
> A **Power BI Embedded** által kínált további API-k segítségével módosíthatja, hogy az adatkészlet mely kiszolgálóra vagy adatbázisra mutasson, illetve beállíthatja a szolgáltatásfiók hitelesítő adatait, melyek segítségével az adatkészlet majd csatlakozhat az adatbázishoz. További információ: [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) és [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="create-power-bi-datasets-and-reports-using-apis"></a>Power BI-adatkészletek és -jelentések létrehozása API-k használatával

### <a name="datsets"></a>Adatkészletek

A REST API segítségével létrehozhat adatkészleteket a Power BI Embeddedben. Ezután elküldheti adatait saját adatkészletébe. Ez lehetővé teszi, hogy dolgozhasson az adattal a Power BI Desktop használata nélkül is. További információkat az [adatkészletek közzétételét](https://msdn.microsoft.com/library/azure/mt778875.aspx) ismertető részben talál.

### <a name="reports"></a>Jelentések

A JavaScript API használatával saját alkalmazásában készíthet jelentést egy adatkészletből. További információkért lásd az [Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md) részt.

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md)
[Jelentések mentése](power-bi-embedded-save-reports.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
További kérdései vannak? [Tegye próbára a Power BI közösségét](http://community.powerbi.com/)

