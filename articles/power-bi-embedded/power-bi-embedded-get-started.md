---
title: "lépések a Microsoft Power BI Embedded aaaGet"
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
ms.openlocfilehash: ebb550cb4eba761dde3c23e4dd0314fc885817e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-microsoft-power-bi-embedded"></a>A Mcirosoft Power BI Embedded bemutatása

**A Power BI Embedded** van az Azure-szolgáltatások, hogy lehetővé teszi, hogy alkalmazás fejlesztők tooadd interaktív Power BI-jelentéseket vehetnek fel saját alkalmazásaikba. **A Power BI Embedded** jelentkezzen be a meglévő alkalmazások újratervezése kellene vagy hello módon módosítás nélkül működik.

Az erőforrások **Microsoft Power BI Embedded** törlődnek az hello [Azure ARM API-k](https://msdn.microsoft.com/library/mt712306.aspx). Ebben az esetben kiépítése hello erőforrás van egy **Power BI-Munkaterületcsoport**.

![](media/power-bi-embedded-get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Munkaterület-csoport létrehozása

A **munkaterület-csoportok** hello legfelső szintű Azure-erőforrás és a rendszer beágyazza az alkalmazás hello tartalom tárolója. A **munkaterület-csoportok** kétféleképpen hozhatók létre:

* Hello Azure portál segítségével manuálisan
* Programozott módon hello Azure Resource Manager (ARM) API-k

Hello lépéseket toobuild keresztül bemutatjuk a **munkaterület-csoportok** használatával hello Azure portálon.

1. Nyissa meg az **Azure Portalt** és jelentkezzen be: [http://portal.azure.com](http://portal.azure.com).
2. Kattintson a **+ új** a hello felső panelje.
   
   ![](media/power-bi-embedded-get-started/create-workspace-1.png)
3. Az **Adatok + analitika** területen kattintson a **Power BI Embedded** elemre.
4. A hello **munkaterület-csoport panel**, adja meg a hello szükséges adatokat. A **Díjszabásról** további információt [A Power BI Embedded díjszabása](http://go.microsoft.com/fwlink/?LinkID=760527) című témakörben talál.
   
   ![](media/power-bi-embedded-get-started/create-workspace-2.png)
5. Kattintson a **Create** (Létrehozás) gombra.

Hello **munkaterület-csoportok** eltarthat néhány másodpercig tooprovision. Amikor elkészült, akkor megnyílik toohello **munkaterület-csoport panel**.

   ![](media/power-bi-embedded-get-started/create-workspace-3.png)

Hello **létrehozás panelen** toocall hello API-t a munkaterületek létrehozásáért és a tartalom toothem szükséges hello információkat tartalmazza.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>A Power BI API elérési kulcsainak megtekintése

Hello legfontosabb szükséges információkat toocall kódrészletek hello Power BI REST API-k egyike hello **hívóbetűk**. Ezek a használt toogenerate hello **alkalmazási jogkivonatok** , amelyek használt tooauthenticate az API-kérelmek. tooview a **hívóbetűk**, kattintson a **hívóbetűk** a hello **beállítások panel**. További információk az **alkalmazás-jogkivonatokról**: [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md) (Hitelesítés és engedélyezés a Power BI Embedded használatával).

   ![](media/power-bi-embedded-get-started/access-keys.png)

Láthatja, hogy két kulccsal rendelkezik.

   ![](media/power-bi-embedded-get-started/access-keys-2.png)

Készítsen másolatot a kulcsokról, és tárolja őket biztonságos helyen az alkalmazáson belül. Nagyon fontos, hogy úgy kezelje, ezek a kulcsok jelszavát, ugyanúgy, mert hozzáférési tooall hello tartalma lesz biztosítanak a **munkaterület-csoportok**.

Jóllehet két kulcs van listázva, egyszerre azonban mindig csak az egyikre lesz szüksége. hello második kulcsra azért van, akkor a kulcsok rendszeres újragenerálása toohello szolgáltatás megszakítása nélkül.

Most, hogy rendelkezik **elérési kulcsokkal** és az alkalmazáshoz való Power BI-példánnyal, jelentést importálhat az alkalmazásba. Mielőtt megtudhatja, hogyan tooimport egy jelentést, hello a következő szakasz ismerteti az alkalmazásba létrehozása a Power BI-adatkészletek és jelentések tooembed.

## <a name="working-with-workspaces"></a>A munkaterületek használata

A munkaterület-csoport létrehozása után kell, hogy a rendszer mellett a jelentések és adatkészletek munkaterület toocreate. a munkaterület toocreate, szüksége lesz toouse hello [Post Worksapce REST API](https://msdn.microsoft.com/library/azure/mt711503.aspx).

## <a name="create-power-bi-datasets-and-reports-tooembed-into-an-app-using-power-bi-desktop"></a>A Power BI-adatkészletek és jelentések tooembed létrehozása az alkalmazásba a Power BI Desktop használatával

Most, hogy a Power BI példányának hozott létre az alkalmazáshoz, és rendelkezik **hívóbetűk**, toocreate hello Power BI-adatkészletek és jelentések, amelyet az tooembed lesz szüksége. Az adatkészletek és a jelentések a **Power BI Desktop** segítségével hozhatók létre. A [Power BI Desktop ingyenesen](https://go.microsoft.com/fwlink/?LinkId=521662) letölthető. Vagy, tooquickly első lépései, letöltheti a hello [kiskereskedelmi elemzési minta pbix-fájlt](http://go.microsoft.com/fwlink/?LinkID=780547).

> [!NOTE]
> További kapcsolatos toolearn toouse **Power BI Desktop**, lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

A **Power BI Desktop**, csatlakozás tooyour adatforrás hello adatok másolatát importálásával **Power BI Desktop** vagy az összekötő közvetlenül toohello adatok használatával **DirectQuery**.

Az alábbiakban közti különbségeket hello **importálási** és **DirectQuery**.

| Importálás | DirectQuery |
| --- | --- |
| Ezzel a módszerrel a táblákat, oszlopokat *és adatokat* is a **Power BI Desktopba** importálhatja vagy másolja. A megjelenítésekkel végzett munka **Power BI Desktop** lekérdezi hello adatok másolatát. toosee történt toohello alapjául szolgáló adatok módosítása, frissítse, vagy importálja, a teljes aktuális adatkészletet újra. |Ezzel a módszerrel csak *a táblákat és oszlopokat* importálhatja vagy másolhatja a **Power BI Desktopba**. A megjelenítésekkel végzett munka **Power BI Desktop** lekérdezések hello alapul szolgáló adatforrásban, ami azt jelenti, hogy mindig aktuális adatokat megtekintése. |

Csatlakozás tooa adatforrás kapcsolatban bővebben lásd: [Connect tooa adatforrás](power-bi-embedded-connect-datasource.md).

A **Power BI Desktopban** végzett munka mentésekor létrejön egy PBIX-fájl. Ez tartalmazza a jelentést. Emellett, ha importál adatokat hello pbix-fájlt tartalmaz hello teljes adatkészletet, vagy ha **DirectQuery**, hello pbix-fájlt tartalmaz, csak az adatkészlet sémája. Programozott módon telepítheti az hello pbix-fájlt a munkaterületre hello segítségével [Power BI importálási API](https://msdn.microsoft.com/library/mt711504.aspx).

> [!NOTE]
> **A Power BI Embedded** tartalmaz további API-k toochange hello kiszolgálót és adatbázist, hogy a DataSet adatkészlet mutat tooand beállítása a szolgáltatási fiók hitelesítő adatait, amely a hello dataset tooconnect tooyour adatbázist fogja használni. További információ: [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) és [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="create-power-bi-datasets-and-reports-using-apis"></a>Power BI-adatkészletek és -jelentések létrehozása API-k használatával

### <a name="datsets"></a>Adatkészletek

Adatkészletek belül a Power BI Embedded hello REST API használatával hozhat létre. Ezután elküldheti adatait saját adatkészletébe. Ez lehetővé teszi a Power BI Desktop hello igénye nélkül adatokkal toowork. További információkat az [adatkészletek közzétételét](https://msdn.microsoft.com/library/azure/mt778875.aspx) ismertető részben talál.

### <a name="reports"></a>Jelentések

Létrehozhat egy jelentést egy adatkészletből közvetlenül a az alkalmazás hello JavaScript API használatával. További információkért lásd az [Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md) részt.

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Új jelentés létrehozása adatkészletből a Power BI Embedded használatával](power-bi-embedded-create-report-from-dataset.md)
[Jelentések mentése](power-bi-embedded-save-reports.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)

