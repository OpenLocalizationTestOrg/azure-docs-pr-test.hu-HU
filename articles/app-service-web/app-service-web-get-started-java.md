---
title: "aaaCreate az első Java-webalkalmazás az Azure-ban"
description: "Ismerje meg, hogyan toorun webalkalmazások az App Service egy alapszintű Java-alkalmazás központi telepítésével."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 6/7/2017
ms.author: cephalin;robmcm
ms.custom: mvc
ms.openlocfilehash: 81315c07b5aa84cbec50a17b2cb3914927b19c00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-web-app-in-azure"></a>Az első Java-webalkalmazás létrehozása az Azure-ban

Hello [webalkalmazások](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) szolgáltatása [Azure App Service](../app-service/app-service-value-prop-what-is.md) jól skálázható, önálló javítási a webhelyszolgáltató biztosít. A gyors üzembe helyezés bemutatja, hogyan toodeploy Java webes hello segítségével app tooApp szolgáltatás [Eclipse IDE Java EE-fejlesztőknek](http://www.eclipse.org/).

![„Hello Azure!” példa webalkalmazás](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="prerequisites"></a>Előfeltételek

toocomplete a gyors üzembe helyezés telepítése:

* szabad hello [Eclipse IDE Java EE-fejlesztőknek](http://www.eclipse.org/downloads/). Ez a gyorsútmutató az Eclipse Neont használj.
* Hello [Eclipse Azure eszköztára](/azure/azure-toolkit-for-eclipse-installation).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Dinamikus webes projekt létrehozása az Eclipse-ben

Az Eclipse-ben válassza a **File (Fájl)** > **New (Új)** > **Dynamic Web Project** (Dinamikus webprojekt) lehetőséget.

A hello **új dinamikus webes projekt** párbeszédpanelen neve hello projekt **MyFirstJavaOnAzureWebApp**, és válassza ki **Befejezés**.
   
![New Dynamic Web Project (Új dinamikus webprojekt) párbeszédpanel](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

### <a name="add-a-jsp-page"></a>JSP-oldal hozzáadása

Ha a Project Explorer (Projektböngésző) nem jelenik meg, állítsa azt vissza.

![Az Eclipse-hez készült Java EE munkaterület](./media/app-service-web-get-started-java/pe.png)

A Project Explorer bontsa ki a hello **MyFirstJavaOnAzureWebApp** projekt.
Kattintson a jobb gombbal az **WebContent** elemre, majd válassza a **(New) Új** > **JSP File (JSP-fájl)** (JSP-fájl) lehetőséget.

![Az új JSP-fájl menüje a Project Explorer (Projektböngésző) nézetben](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

A hello **új JSP-fájl** párbeszédpanel:

* Nevű hello fájl **index.jsp**.
* Válassza a **Finish** (Befejezés) elemet.

  ![New JSP File (Új JSP-fájl) párbeszédpanel](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Hello index.jsp fájlban cserélje le hello `<body></body>` hello jelölés során a következő elem:

```jsp
<body>
<h1><% out.println("Hello Azure!"); %></h1>
</body>
```

Hello módosítások mentéséhez.

## <a name="publish-hello-web-app-tooazure"></a>Hello web app tooAzure közzététele

A Project Explorer, kattintson a jobb gombbal a projekt hello, és válassza **Azure** > **Azure Web Apps közzététel**.

![A Publish as Azure Web App (Közzététel Azure-webalkalmazásként) helyi menü](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

A hello **Azure bejelentkezés** párbeszédpanelen, a tárolás során is garantálják hello **interaktív** lehetőséget, majd válassza ki **bejelentkezés**.

Kövesse az utasításokat bejelentkezési hello.

### <a name="deploy-web-app-dialog-box"></a>Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel

Miután bejelentkezett az Azure-fiók tooyour, hello **webalkalmazás telepítése** párbeszédpanel jelenik meg.

Kattintson a **Létrehozás** gombra.

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

### <a name="create-app-service-dialog-box"></a>A Create App Service (App Service létrehozása) párbeszédpanel

Hello **létrehozása az App Service** párbeszédpanel jelenik meg az alapértelmezett értékekkel. szám hello **170602185241** hello a következő kép nem azonos a párbeszédpanelen látható.

![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/cas1.png)

A hello **létrehozása az App Service** párbeszédpanel:

* Tartsa hello webalkalmazás hello generált név. Ennek a névnek az Azure-on belül egyedinek kell lennie. hello neve hello URL-címet hello webalkalmazás részét képezi. Példa: Ha hello webes alkalmazás neve **MyJavaWebApp**, URL-cím hello *myjavawebapp.azurewebsites.net*.
* Tartsa hello alapértelmezett webes tárolót.
* Válasszon ki egy Azure-előfizetést.
* A hello **App service-csomag** lapon:

  * **Új**: tartsa hello alapértelmezett értéket, amely hello hello App Service-csomag nevét.
  * **Location** (Hely): válassza a **West Europe** (Nyugat-Európa) lehetőséget vagy egy Önhöz közeli helyet.
  * **A tarifacsomag**: Válasszon hello ingyenes lehetőséget. A szolgáltatások díját az [App Service díjszabás](https://azure.microsoft.com/pricing/details/app-service/) részben találja.

   ![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

### <a name="resource-group-tab"></a>Resource group (Erőforráscsoport) lap

Jelölje be hello **erőforráscsoport** fülre. Tartsa hello alapértelmezés szerint generált érték hello erőforráscsoport.

![Resource group (Erőforráscsoport) lap](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Kattintson a **Létrehozás** gombra.

<!--
### hello JDK tab

Select hello **JDK** tab. Keep hello default, and then select **Create**.

![Create App Service plan](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)
-->

hello Azure eszközkészlet hello webalkalmazás hoz létre, és megjelenik a folyamatjelző párbeszédpanel.

![App Service létrehozásának állapotjelző párbeszédpanelje](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

### <a name="deploy-web-app-dialog-box"></a>Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel

A hello **webalkalmazás telepítése** párbeszédpanelen jelölje ki **tooroot telepítése**. Ha egy alkalmazás szolgáltatás: *wingtiptoys.azurewebsites.net* toohello gyökér, hello webes alkalmazás neve nem telepít és **MyFirstJavaOnAzureWebApp** a rendszer túl *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp*.

![Deploy Web App (Webalkalmazás üzembe helyezése) párbeszédpanel](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

hello párbeszédpanel mezőben látható hello Azure JDK és webes tároló beállításokat.

Válassza ki **telepítés** toopublish hello web app tooAzure.

Hello közzététel befejezése után válassza ki a hello **közzétett** hello hivatkozásra **Azure tevékenységnapló** párbeszédpanel megnyitásához.

![Azure Activity Log (Azure tevékenységnapló) párbeszédpanel](./media/app-service-web-get-started-java/aal.png)

Gratulálunk! Sikeresen telepítette a webes alkalmazás tooAzure. 

![„Hello Azure!” példa webalkalmazás](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="update-hello-web-app"></a>Hello webes alkalmazás frissítése

Módosítsa a minta JSP kód tooa különböző üdvözlőüzenetére.

```jsp
<body>
<h1><% out.println("Hello again Azure!"); %></h1>
</body>
```

Hello módosítások mentéséhez.

A Project Explorer, kattintson a jobb gombbal a projekt hello, és válassza **Azure** > **Azure Web Apps közzététel**.

Hello **webalkalmazás telepítése** párbeszédpanel jelenik meg, és azt mutatja be, amelyet korábban hozott létre az app service hello. 

> [!NOTE]
> Válassza ki **tooroot telepítése** minden alkalommal, amikor a közzététele.
>

Hello webalkalmazás válassza ki, majd **telepítés**, amely közzéteszi a hello módosításokat.

Ha hello **közzétételi** hivatkozás jelenik meg, válassza ki azt a toobrowse toohello web app és hello módosítások megtekintéséhez.

## <a name="manage-hello-web-app"></a>Hello webes alkalmazás kezelése

Nyissa meg toohello <a href="https://portal.azure.com" target="_blank">Azure-portálon</a> toosee hello webalkalmazást, amelyet létrehozott.

Hello bal oldali menüben válassza ki a **erőforráscsoportok**.

![Portálnavigációjával tooresource csoportok](media/app-service-web-get-started-java/rg.png)

Válasszon hello erőforráscsoportot. hello lapon látható a gyors üzembe helyezés létrehozott hello erőforrásokat.

![myResourceGroup erőforráscsoport](media/app-service-web-get-started-java/rg2.png)

Jelölje be hello web app (**webapp-170602193915** a kép megelőző hello).

Hello **áttekintése** lap jelenik meg. Ezen a lapon hogyan hello app tesz a nézetét biztosítja. Itt elvégezhet olyan alapszintű felügyeleti feladatokat, mint a tallózás, leállítás, elindítás, újraindítás és törlés. hello lapok hello bal oldalán található hello megjelenítése hello megnyithatja különböző konfigurációkat. 

![Az App Service lap az Azure Portalon](media/app-service-web-get-started-java/web-app-blade.png)

[!INCLUDE [clean-up-section-portal-web-app](../../includes/clean-up-section-portal-web-app.md)]

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Egyéni tartomány leképezése](app-service-web-tutorial-custom-domain.md)
