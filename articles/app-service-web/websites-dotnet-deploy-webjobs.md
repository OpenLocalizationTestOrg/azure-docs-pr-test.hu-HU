---
title: "aaaDeploy webjobs-feladatok Visual Studio használatával"
description: "Megtudhatja, hogyan toodeploy Azure webjobs-feladatok tooAzure App Service Web Apps Visual Studio használatával."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2016
ms.author: glenga
ms.openlocfilehash: 5fc5d9562e8836348f5ab6844fb6c23ff40a321c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a>WebJobs üzembe helyezése Visual Studióval
## <a name="overview"></a>Áttekintés
Ez a témakör ismerteti, hogyan toouse Visual Studio toodeploy egy konzolalkalmazás projekt webalkalmazás tooa [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) , egy [Azure webjobs-feladat](http://go.microsoft.com/fwlink/?LinkId=390226). Hogyan toodeploy webjobs-feladatok használatával hello információt [Azure Portal](https://portal.azure.com), lásd: [háttérfeladatok futtatása a webjobs-feladatok](web-sites-create-web-jobs.md).

Ha a Visual Studio telepíti a WebJobs-kompatibilis Konzolalkalmazás projekt, két feladatokat hajtja végre:

* Másolatot futásidejű fájlmappa toohello megfelelő hello web app alkalmazásban (*App_Data/feladatok/folyamatos* a folyamatos webjobs-feladatok, *App_Data/feladatok/indított* az ütemezett és igény szerinti webjobs-feladatok).
* Beállítja az [Azure ütemezőjének](#scheduler) a webjobs-feladatok, amelyek ütemezett toorun adott időpontban. (Ez nem szükséges a folyamatos WebJobs.)

A WebJobs-kompatibilis projekt rendelkezik a következő elemek hozzáadott tooit hello:

* Hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-csomagot.
* A [webjobs-feladat közzététele settings.json](#publishsettings) központi telepítés és az ütemezési beállításokat tartalmazó fájl. 

![Mi meg van adva egy webjobs-feladat tooa Konzolalkalmazás tooenable központi telepítés ábrája](./media/websites-dotnet-deploy-webjobs/convert.png)

Adja hozzá a Konzolalkalmazás projekt meglévő elemek tooan, vagy használja a sablon toocreate egy új WebJobs-kompatibilis Konzolalkalmazás-projektet. 

Projekt telepítése webjobs-feladat, önmagában, vagy hivatkozás tooa webes projektet, hogy automatikusan telepíti amikor hello webes projekt telepítése. toolink projektek, a Visual Studio hello WebJobs-kompatibilis projekt nevét hello magában foglalja a [webjobs-list.json](#webjobslist) hello webes projekt fájlban.

![Webjobs-feladat projekt tooweb projekt linking bemutató ábra](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a>Előfeltételek
Webjobs-feladatok üzembe helyezési funkcióival hello Azure SDK for .NET telepítése esetén a Visual Studio érhetők el:

* [Az Azure SDK for .NET (a Visual Studio)](https://azure.microsoft.com/downloads/).

## <a id="convert"></a>Webjobs-feladatok meglévő Konzolalkalmazás projekt központi telepítésének engedélyezése
Erre két lehetősége van:

* [Engedélyezze az automatikus központi telepítési egy webes projekt](#convertlink).
  
    Egy meglévő Konzolalkalmazás-projekt konfigurálása, hogy automatikusan telepíti a webjobs-feladat, amikor egy webes projekt telepítése. Használja ezt a beállítást, ha azt szeretné, toorun a webjobs-feladat a hello hello futtassa azonos webalkalmazás kapcsolódó webalkalmazás.
* [Központi telepítés nélkül webes projektet engedélyezése](#convertnolink).
  
    Konfigurálja egy meglévő Konzolalkalmazás projekt toodeploy webjobs-feladat önmagában, nincs hivatkozás tooa webes projekt esetében. Használja ezt a beállítást, ha azt szeretné, a webes alkalmazás a webjobs-feladat toorun önmagában, nincs hello webalkalmazásban futó webes alkalmazásokkal együtt. Érdemes lehet toodo Ez a rendezés toobe képes tooscale a webjobs-feladat erőforrásokat, függetlenül a webes alkalmazás erőforrásaihoz.

### <a id="convertlink"></a>WebJobs-automatikus központi telepítési egy webes projekt engedélyezése
1. Kattintson a jobb gombbal hello webes projekt **Megoldáskezelőben**, és kattintson a **Hozzáadás** > **Azure webjobs-feladat létező projekt**.
   
    ![Azure webjobs-feladat létező projekt](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    Hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg.
2. A hello **projektnevet** legördülő listában, jelölje be hello Konzolalkalmazás projekt tooadd, webjobs-feladat.
   
    ![Válassza ki a projektet az Azure Webjobs hozzáadása párbeszédpanel](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. Teljes hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**. 

### <a id="convertnolink"></a>Webjobs-feladatok központi telepítés nélkül webes projektet engedélyezése
1. Kattintson a jobb gombbal hello Konzolalkalmazás projekt **Megoldáskezelőben**, és kattintson a **Azure webjobs-feladat közzététel...** . 
   
    ![Közzététel az Azure webjobs-feladat](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    Hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg, hello kiválasztott hello projekt **projektnevet** mezőbe.
2. Teljes hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.
   
   Hello **webhely közzététele** varázsló jelenik meg.  Ha nem szeretné, hogy toopublish azonnal, hello varázsló bezárásához. a megadott hello-beállítások a lesznek mentve, ha szeretné, hogy túl[hello projekt telepítése](#deploy).

## <a id="create"></a>WebJobs-kompatibilis új projekt létrehozása
toocreate WebJobs-kompatibilis új projekt, használhatja hello Konzolalkalmazás projekt sablon engedélyezése webjobs-feladatok központi és a [hello az előző szakaszban](#convert). Alternatív megoldásként hello webjobs-feladatok – új projekt sablont is használhatja:

* [Hello webjobs-feladatok – új projekt sablon használata az egy független webjobs-feladat](#createnolink)
  
    A projekt létrehozása és a toodeploy önmagában egy webjobs-feladat, nincs hivatkozás tooa webes projekt esetében. Használja ezt a beállítást, ha azt szeretné, a webes alkalmazás a webjobs-feladat toorun önmagában, nincs hello webalkalmazásban futó webes alkalmazásokkal együtt. Érdemes lehet toodo Ez a rendezés toobe képes tooscale a webjobs-feladat erőforrásokat, függetlenül a webes alkalmazás erőforrásaihoz.
* [A webjobs-feladat csatolt tooa webes projekt hello webjobs-feladatok – új projekt sablon használata](#createlink)
  
    A konfigurált toodeploy automatikusan webjobs-feladat során ugyanazon megoldást már telepítették hello egy webes projekt projekt létrehozása. Használja ezt a beállítást, ha azt szeretné, toorun a webjobs-feladat a hello hello futtassa azonos webalkalmazás kapcsolódó webalkalmazás.

> [!NOTE]
> hello webjobs-feladatok – új projekt sablon automatikusan telepíti a NuGet-csomagok, és magában foglalja a kód *Program.cs* a hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs). Ha nem szeretné toouse hello WebJobs SDK, távolítsa el, vagy módosítsa a hello `host.RunAndBlock` utasítás *Program.cs*.
> 
> 

### <a id="createnolink"></a>Hello webjobs-feladatok – új projekt sablon használata az egy független webjobs-feladat
1. Kattintson a **fájl** > **új projekt**, majd a hello **új projekt** párbeszédpanelen kattintson **felhő**  >   **Azure webjobs-feladat (.NET-keretrendszer)**.
   
    ![Webjobs-feladat sablon új projekt párbeszédpanel](./media/websites-dotnet-deploy-webjobs/np.png)
2. Kövesse a korábban bemutatott hello irányban túl[ellenőrizze a Konzolalkalmazás-projektet egy független WebJobs-projekt hello](#convertnolink).

### <a id="createlink"></a>A webjobs-feladat csatolt tooa webes projekt hello webjobs-feladatok – új projekt sablon használata
1. Kattintson a jobb gombbal hello webes projekt **Megoldáskezelőben**, és kattintson a **Hozzáadás** > **új Azure webjobs-feladat projekt**.
   
    ![Új Azure webjobs-feladat projekt menübejegyzést](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    Hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg.
2. Teljes hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.

## <a id="configure"></a>hello Azure Webjobs hozzáadása párbeszédpanel
Hello **adja hozzá a Azure webjobs-feladat** párbeszédpanelen lehetővé teszi, hogy adja meg a hello webjobs-feladat neve és mód beállítása a webjobs-feladat futtatása. 

![Azure webjobs-feladat párbeszédpanel hozzáadása](./media/websites-dotnet-deploy-webjobs/aaw2.png)

Ezen a párbeszédpanelen hello mezők felel meg a hello toofields **új feladat** hello Azure Portal párbeszédpanelén. További információkért lásd: [háttérfeladatok futtatása a webjobs-feladatok](web-sites-create-web-jobs.md).

> [!NOTE]
> * Parancssori telepítéssel kapcsolatos információkért lásd: [parancssori engedélyezése vagy folyamatos kézbesítését az Azure webjobs-feladatok](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).
> * Ha telepíti a webjobs-feladat, majd döntse el, toochange hello típusú webjobs-feladat, és helyezze üzembe újra, toodelete hello webjobs-feladatok közzététele settings.json fájl lesz szüksége. A Visual Studio megjelenítése hello közzétételi beállítások újra, így módosíthatja a webjobs-feladat hello típusú működésképtelenné teszi.
> * Ha egy webjobs-feladat telepíti, később módosíthatja használata a folyamatos toonon folyamatos hello, vagy fordítva, Visual Studio létrehoz egy új webjobs-feladat az Azure-ban újratelepítésekor. Ha további ütemezési beállítások módosítására, de hagyja futtatása mód hello azonos, vagy váltás ütemezett és igény szerinti, Visual Studio frissítések meglévő feladat hello helyett hozzon létre egy újat.
> 
> 

## <a id="publishsettings"></a>webjobs-feladat közzététele settings.json
Egy Konzolalkalmazást a WebJobs-telepítéshez való konfigurálásakor a Visual Studio telepíti hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet csomag- és ütemezési információkat tárolja egy *webjobs-feladat közzététele settings.json*  hello projektben fájl *tulajdonságok* hello WebJobs-projekt mappájából. Íme egy példa, hogy a fájl:

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

Közvetlenül szerkesztheti a fájlt, és a Visual Studio IntelliSense biztosít. hello fájl séma tárolódik [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) nem lehet megtekinteni.  

## <a id="webjobslist"></a>webjobs-list.json
WebJobs-kompatibilis projekt tooa webes projektet kapcsolódáskor a Visual Studio hello WebJobs projekt hello nevét tárolja egy *webjobs-list.json* hello webes projekt fájl *tulajdonságok* mappát. hello listát tartalmazhat több WebJobs-projektet, ahogy az alábbi példa hello:

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

Közvetlenül szerkesztheti a fájlt, és a Visual Studio IntelliSense biztosít. hello fájl séma tárolódik [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) nem lehet megtekinteni.

## <a id="deploy"></a>Webjobs-feladatok projekt telepítése
A WebJobs-projekt csatolta tooa webes projekt automatikusan hello webes projekt telepíti. A webes projekt telepítése kapcsolatos információkért lásd: [hogyan toodeploy tooWeb alkalmazások](web-sites-deploy.md).

toodeploy a webjobs-feladatok projekt önmagában, kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** kattintson **Azure webjobs-feladat közzététel...** . 

![Közzététel az Azure webjobs-feladat](./media/websites-dotnet-deploy-webjobs/paw.png)

Egy független webjobs-feladat, az azonos hello **webhely közzététele** webes projektek megjelenik, de kevesebb beállítások elérhető toochange varázslójától.

## <a id="nextsteps"></a>Következő lépések
Ez a cikk szerint hogyan toodeploy webjobs-feladatok Visual Studio használatával. További információ a hogyan toodeploy Azure WebJobs: [Azure WebJobs - erőforrások – ajánlott telepítési](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).

