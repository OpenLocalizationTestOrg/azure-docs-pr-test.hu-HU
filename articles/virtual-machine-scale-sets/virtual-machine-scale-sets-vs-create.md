---
title: "Virtuálisgép-méretezési készlet Visual Studio használatával aaaDeploy |} Microsoft Docs"
description: "Központi telepítése a Visual Studio és a Resource Manager-sablon használatával virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c89a9f2478ccc3d22989aea604a4273bcc46df82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-visual-studio"></a>Hogyan toocreate egy virtuálisgép-méretezési készlet a Visual Studio
Ez a cikk bemutatja, hogyan toodeploy Azure virtuálisgép-méretezési beállítása a Visual Studio erőforrás csoport központi telepítéssel.

[Az Azure virtuálisgép-méretezési csoportok](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) van egy Azure számítási erőforrás toodeploy és hasonló virtuális gépek automatikus méretezése a gyűjteményeinek kezelését, és a terheléselosztás. Kioszthatja és központi telepítése használatával virtuálisgép-méretezési csoportok [Azure Resource Manager-sablonok](https://github.com/Azure/azure-quickstart-templates). Az Azure Resource Manager-sablonok az Azure parancssori felület, PowerShell, REST telepíthető és is közvetlenül a Visual Studio. A Visual Studio számos példa sablonok, amelyek az Azure erőforrás-csoport központi telepítésének projektek részeként telepíthetők.

Azure-erőforráscsoport központi telepítések egy módon toogroup, és tegye közzé a kapcsolódó Azure-erőforrások olyan készletét egyetlen központi telepítési művelettel. További róluk itt: [létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok a](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Előfeltételek
a Visual Studio virtuálisgép-méretezési csoportok üzembe helyezésének lépései tooget, következőkre lesz szüksége hello:

* A Visual Studio 2013 vagy újabb verzió
* Az Azure SDK 2.7, 2.8 vagy 2.9

>[!NOTE]
>Ezek az utasítások azt feltételezik, hogy a Visual Studio használata [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>A projekt létrehozása
1. Hozzon létre egy új projektet a Visual Studio kiválasztásával **fájl |} Új |} Projekt**.
   
    ![Új fájl][file_new]

2. A **Visual C# |} Felhő**, válassza a **Azure Resource Manager** toocreate projekt telepítése egy Azure Resource Manager-sablon.
   
    ![Projekt létrehozása][create_project]

3. Válassza ki a sablonok hello listáról hello Linux vagy a Windows virtuális gép méretezési beállítása sablon.
   
   ![Sablon kiválasztása][select_Template]

4. A projekt létrehozása után megjelenik PowerShell üzembe helyezési parancsfájlok, az Azure Resource Manager-sablon és a virtuálisgép-méretezési csoport hello paraméter fájl.
   
    ![Megoldáskezelő][solution_explorer]

## <a name="customize-your-project"></a>A projekt testreszabása
Most hello sablon toocustomize módosíthatja azt az alkalmazás igényeinek, például a Virtuálisgép-bővítmény tulajdonságai hozzáadása vagy szerkesztése terheléselosztási szabályok betöltése. Alapértelmezés szerint a hello virtuális gép méretezési beállítása sablonok a konfigurált toodeploy hello AzureDiagnostics bővítményt, így könnyen tooadd automatikus skálázási szabályok. Emellett központilag telepíti a terheléselosztó a nyilvános IP-cím, a bejövő NAT-szabályok konfigurálva. 

hello terheléselosztó lehetővé teszi a csatlakozást a Virtuálisgép-példányok toohello SSH (Linux) vagy RDP (Windows). hello előtéri porttartománya 50000 kezdődik. Linux Ez azt jelenti, hogy ha akkor SSH tooport 50000, irányított tooport 22 az első virtuális gép a virtuális gépek méretezési csoportjának hello hello. Csatlakozás tooport 50001 irányított tooport 22 hello virtuális gép második és így tovább.

 Egy jó módszer tooedit a sablonokat, a Visual Studio toouse hello JSON-vázlat tooorganize hello paraméterek, változók és erőforrásokat. Séma Visual Studio egy hello ismeretének mutathat hibát jelez a sablon telepítése előtt.

![JSON Explorer][json_explorer]

## <a name="deploy-hello-project"></a>Hello projekt telepítése
1. Hello Azure Resource Manager-sablon toocreate hello virtuálisgép-méretezési csoport erőforrás telepítése. Kattintson jobb gombbal a projektcsomópontra hello, és válassza a **telepítés |} Új központi telepítési**.
   
    ![Sablon üzembe helyezése][5deploy_Template]
    
2. Hello "Központi telepítés tooResource csoport" párbeszédpanelen jelölje ki az előfizetését.
   
    ![Sablon üzembe helyezése][6deploy_Template]

3. Itt egy Azure-erőforráscsoport toodeploy a sablont hozhat létre.
   
    ![Új erőforráscsoport][new_resource]

4. Ezután kattintson **paraméterek szerkesztése** tooenter tooyour sablon átadott paraméterek. Adja meg az operációs rendszer, amely szükséges toocreate hello telepítési hello hello felhasználónevet és jelszót. Ha a PowerShell-eszközök nem rendelkezik a telepített Visual Studio, akkor ajánlott toocheck **menthetik a jelszavakat** tooavoid egy rejtett PowerShell parancssori kérni, vagy használjon [keyvault támogatási](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).
   
    ![Paraméterek szerkesztése][edit_parameters]

5. Most kattintson **telepítés**. Hello **kimeneti** ablakban látható hello központi telepítésének állapotáról. Vegye figyelembe, hogy hello művelet végrehajtja az hello **telepítés-AzureResourceGroup.ps1** parancsfájl.
   
   ![Kimeneti ablak][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a>A virtuálisgép-méretezési csoport felfedezése
Hello telepítése után megtekintheti hello új virtuálisgép-méretezési a Visual Studio hello **Cloud Explorer** (frissítési hello lista). Cloud Explorer lehetővé teszi a Visual Studio Azure-erőforrások kezeléséhez alkalmazások fejlesztése során. Megtekintheti a virtuálisgép-méretezési csoportban lévő hello [Azure-portálon](https://portal.azure.com) és [Azure erőforrás-kezelő](https://resources.azure.com/).

![Cloud Explorer][cloud_explorer]

 hello portal hello toovisually kezelése az Azure-infrastruktúra egy webböngészővel rendelkező, míg az Azure erőforrás-kezelő kínál egy egyszerűen tooexplore legjobb megoldást kínál és hibakeresése az Azure-erőforrások, egyúttal jogosultságot ad az ablak "példány megtekintése" hello és is megjeleníti a PowerShell az éppen megtekintett hello erőforrások parancsok.

## <a name="next-steps"></a>Következő lépések
Miután sikeresen telepítése a Visual Studio alkalmazással virtuálisgép-méretezési csoportok, még jobban testre szabható a projekt toosuit alkalmazás igényeinek. Például állítsa be automatikus méretezése hozzáadásával egy **Insights** erőforrás, infrastruktúra tooyour sablon (például önálló virtuális gépek) hozzáadásakor, vagy egyéni parancsprogramok futtatására szolgáló bővítmény hello használó alkalmazások telepítésekor. Jó példa sablonok találhatók hello [Azure gyors üzembe helyezési sablonokat](https://github.com/Azure/azure-quickstart-templates) GitHub-adattár ("vmss" keresése).

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
