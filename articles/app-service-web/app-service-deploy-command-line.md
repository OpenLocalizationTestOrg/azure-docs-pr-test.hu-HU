---
title: "az Azure parancssori eszközök alkalmazás aaaAutomate telepítési |} Microsoft Docs"
description: "Információk az Azure parancssori hello alkalmazását telepítésének észlelése"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 8b65980c-eb75-44a2-8e0f-f9eb9e617d16
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: 3df66cc4bf4e6819ed0eee7278ac79dca2e5daa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Az Azure parancssori eszközök alkalmazás központi telepítési automatizálásához
Automatizálható az Azure parancssori eszközök alkalmazások központi telepítését. Ez a cikk ismerteti a rendelkezésre álló és hello hasznos hivatkozásokat tartalmaz, amelyek bemutatják, hogyan toouse a központi telepítési munkafolyamat őket. 

## <a name="msbuild"></a>MSBuild központi telepítés automatizálása
Ha hello [Visual Studio IDE](#vs) fejlesztési, használhatja [MSBuild](http://msbuildbook.com/) tooautomate semmit a IDE elvégezhető műveleteket. MSBuild toouse konfigurálhatja vagy [Web Deploy](#webdeploy) vagy [FTP/ftps-t](#ftp) toocopy fájlokat. A Web Deploy is automatizálhatja a sok más központi telepítési kapcsolatos feladatok, például adatbázisok telepítését.

Parancssori telepítés MSBuild használatával kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Az ASP.NET Web Deployment Visual Studio használatával: parancssori telepítési](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). A központi telepítés tooAzure Visual Studio használatával kapcsolatos oktatóanyag egy sorozat része hetek. Azt mutatja be, hogyan toouse hello beállítása után parancssori toodeploy profilok közzététele a Visual Studio.
* [Belső hello Microsoft Build motor: MSBuild használatával és a Team Foundation Build](http://msbuildbook.com/). Hogyan fejezetek tartalmazó nyomtatott könyv toouse MSBuild központi telepítéshez.

## <a name="powershell"></a>A Windows PowerShell központi telepítés automatizálása
MSBuild- vagy FTP-a központi telepítési művelet végezhető [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Ha így tesz, a Windows PowerShell-parancsmagokat, amelyek hello Azure többi felügyeleti API könnyen toocall gyűjteményét is használhatja.

További információkért tekintse meg a következő erőforrások hello:

* [A webes alkalmazás kapcsolódó tooa GitHub-tárházban telepítése](app-service-web-arm-from-github-provision.md)
* [Az SQL-adatbázis a webes alkalmazás telepítéséhez](app-service-web-arm-with-sql-database-provision.md)
* [Kiosztását és telepítését mikroszolgáltatások kiszámítható módon tudja az Azure-ban](app-service-deploy-complex-application-predictably.md)
* [Épület valós felhőalapú alkalmazásokat az Azure - automatizálásához mindent](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-könyv fejezet, amely azt ismerteti, hogyan hello e-könyv látható hello mintaalkalmazást használ-e a Windows PowerShell-parancsfájlok toocreate egy Azure tesztelési környezetben, és tooit telepítése. Lásd: hello [erőforrások](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) hivatkozások tooadditional Azure PowerShell dokumentációs szakaszát.
* [Windows PowerShell-parancsfájlok tooPublish tooDev és a tesztkörnyezetek segítségével](../vs-azure-tools-publishing-using-powershell-scripts.md). Hogyan toouse Windows PowerShell telepítési parancsfájl a Visual Studio létrehozza.

## <a name="api"></a>.NET felügyeleti API központi telepítés automatizálása
C#-kódban írhat tooperform MSBuild vagy FTP-funkciók központi telepítéshez. Ha így tesz, hello Azure felügyeleti REST API tooperform hely felügyeleti funkciókat érheti el.

További információkért tekintse meg a következő erőforrás hello:

* [Automatizálás mindent hello Azure kezelési kódtárakat és .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Bevezetés toohello .NET felügyeleti API és a hivatkozások toomore dokumentációját.

## <a name="cli"></a>Központi telepítése az Azure parancssori felület (Azure CLI)
A Windows, Mac vagy Linux rendszerű gépek toodeploy hello parancssori FTP segítségével is használhatók. Ha így tesz, hello Azure többi felügyeleti API hello Azure parancssori felület használatával is elérhető.

További információkért tekintse meg a következő erőforrás hello:

* [Az Azure parancssori eszközök](https://azure.microsoft.com/downloads/). Portál lapján parancssori eszköz információkat Azure.com webhelyre.

## <a name="webdeploy"></a>A parancssorból a Web Deploy telepítése
[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) van Microsoft-szoftverek központi telepítési tooIIS intelligens fájlt nem csak biztosító szolgáltatások szinkronizálása, de is is hajtsa végre, vagy sok más telepítéssel kapcsolatos feladat, amelyet nem lehet automatizálni FTP használatakor koordinálja. Például a Web Deploy telepíthet egy új adatbázist, vagy az adatbázis-frissítéseket és a webes alkalmazást. A Web Deploy is minimálisra hello szükséges idő tooupdate egy meglévő webhely, mivel intelligens módon csak a módosult fájlokat másolja. Microsoft Visual Studio és a Team Foundation Server van a Web Deploy beépített támogatása, de használhatja a Web Deploy közvetlenül telepítéséből hello parancssori tooautomate. Web Deploy parancsok is nagyon hatékony, de lehet, hogy hello tanulási görbére elsajátíthatják a használatát.

## <a name="more-resources"></a>További erőforrások
Egy másik központi telepítési beállítás toocommand külön automation toouse egy felhőalapú szolgáltatás, mint [Polip telepítése](http://en.wikipedia.org/wiki/Octopus_Deploy). További információkért lásd: [telepítéséhez az ASP.NET alkalmazások tooAzure webhelyek](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Parancssori eszközök további információkért tekintse meg a következő erőforrás hello:

* [Egyszerű webalkalmazások: Telepítési](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). David Ebbo egy eszközről által blog ő megírt toomake azt a Web Deploy könnyebb toouse.
* [Web Deployment eszköz](http://technet.microsoft.com/library/dd568996). Hivatalos dokumentációját hello Microsoft TechNet webhelyen. Azonban továbbra is érdemes toostart dátummal.
* [Webes telepítése](http://www.iis.net/learn/publish/using-web-deploy). Hivatalos dokumentációját hello Microsoft IIS.NET helyen. De a megfelelő hely toostart is dátummal.
* [Az ASP.NET Web Deployment Visual Studio használatával: parancssori telepítési](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild hello motort használják a Visual Studio build, de azt is használható a hello parancssori toodeploy webes alkalmazások tooWeb alkalmazásokat. Ez az oktatóanyag egy sorozat, amely főként a Visual Studio telepítésére vonatkozó részét képezi.

