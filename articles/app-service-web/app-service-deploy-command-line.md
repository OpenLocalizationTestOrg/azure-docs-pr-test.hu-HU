---
title: "Automatizálhatja az Azure parancssori eszközök alkalmazás központi telepítési |} Microsoft Docs"
description: "Információk az Azure alkalmazását a parancssori telepítésének észlelése"
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
ms.openlocfilehash: e0e2e65557911bcac06d4dc355f47e9331934f8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Az Azure parancssori eszközök alkalmazás központi telepítési automatizálásához
Automatizálható az Azure parancssori eszközök alkalmazások központi telepítését. Ez a cikk felsorolja az elérhető eszközöket és a hasznos hivatkozások, amelyek bemutatják, hogyan használhatja ezeket a telepítési munkafolyamat. 

## <a name="msbuild"></a>MSBuild központi telepítés automatizálása
Ha használja a [Visual Studio IDE](#vs) fejlesztési, használhatja [MSBuild](http://msbuildbook.com/) automatizálásához semmit a IDE elvégezhető műveleteket. Konfigurálhatja az MSBuild használata [Web Deploy](#webdeploy) vagy [FTP/ftps-t](#ftp) másolja a fájlokat. A Web Deploy is automatizálhatja a sok más központi telepítési kapcsolatos feladatok, például adatbázisok telepítését.

Parancssori telepítés MSBuild használatával kapcsolatos további információkért lásd a következőket:

* [Az ASP.NET Web Deployment Visual Studio használatával: parancssori telepítési](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). A Visual Studio használatával Azure telepítésére vonatkozó oktatóanyag egy sorozat része hetek. Bemutatja, hogyan után beállítása profilok közzététele a Visual Studio telepítése a parancssor használatával.
* [A Microsoft Build motor belül: MSBuild és a Team Foundation Build](http://msbuildbook.com/). Nyomtatott címjegyzék-alkalmazásával, amely tartalmazza az MSBuild központi telepítés használatával fejezetek.

## <a name="powershell"></a>A Windows PowerShell központi telepítés automatizálása
MSBuild- vagy FTP-a központi telepítési művelet végezhető [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Ha így tesz, a többi Azure szolgáltatásfelügyeleti API hívása könnyedén Windows PowerShell-parancsmagok egy gyűjteményét is használhatja.

További információkért lásd a következőket:

* [A GitHub-tárházban kapcsolódó webalkalmazás üzembe helyezése](app-service-web-arm-from-github-provision.md)
* [Az SQL-adatbázis a webes alkalmazás telepítéséhez](app-service-web-arm-with-sql-database-provision.md)
* [Kiosztását és telepítését mikroszolgáltatások kiszámítható módon tudja az Azure-ban](app-service-deploy-complex-application-predictably.md)
* [Épület valós felhőalapú alkalmazásokat az Azure - automatizálásához mindent](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). E-könyv fejezet, amely ismerteti, hogyan látható módon az e-könyv mintaalkalmazás használja Azure tesztelési környezet létrehozása és telepítése céljából, a Windows PowerShell-parancsfájlok. Tekintse meg a [erőforrások](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) szakasz Azure PowerShell további dokumentációkra mutató hivatkozások.
* [Windows PowerShell-parancsfájlok használatával történő közzétételéhez fejlesztési és tesztkörnyezetek](../vs-azure-tools-publishing-using-powershell-scripts.md). Visual Studio állít elő, hogyan használhatja a Windows PowerShell telepítési parancsfájlok.

## <a name="api"></a>.NET felügyeleti API központi telepítés automatizálása
C#-kódban az MSBuild vagy FTP-feladatokat látnak el a központi telepítés írhat. Ha így tesz, az Azure felügyeleti REST API-t webhely-felügyeleti feladatokat végezheti el.

További információkért tekintse meg a következő erőforráshoz:

* [Automatizálás mindent az Azure felügyeleti szalagtárak és a .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Bevezetés a .NET felügyeleti API és a további dokumentációjára mutató hivatkozásokat.

## <a name="cli"></a>Központi telepítése az Azure parancssori felület (Azure CLI)
Windows, Mac vagy Linux rendszerű számítógépek a parancssor segítségével központi telepítése FTP használatával. Ha így tesz, a többi Azure szolgáltatásfelügyeleti API az Azure parancssori felület használatával is elérhető.

További információkért tekintse meg a következő erőforráshoz:

* [Az Azure parancssori eszközök](https://azure.microsoft.com/downloads/). Portál lapján parancssori eszköz információkat Azure.com webhelyre.

## <a name="webdeploy"></a>A parancssorból a Web Deploy telepítése
[Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) való telepítéshez, amely nem csak az intelligens fájlszinkronizálás biztosít az IIS Microsoft-szoftverek szolgáltatásokat, de is is hajtsa végre, vagy sok más telepítéssel kapcsolatos feladat, amelyet nem lehet automatizálni FTP használatakor koordinálja. Például a Web Deploy telepíthet egy új adatbázist, vagy az adatbázis-frissítéseket és a webes alkalmazást. A Web Deploy is lehet egy meglévő hely frissítése óta az intelligens módon csak a módosult fájlokat másolja szükséges idő minimalizálásához. Microsoft Visual Studio és a Team Foundation Server van a Web Deploy beépített támogatása, de használhatja a Web Deploy közvetlenül a parancssorból automatizálhatja a központi telepítés. Web Deploy parancsok is nagyon hatékony, de lehet, hogy a tanulási görbére elsajátíthatják a használatát.

## <a name="more-resources"></a>További erőforrások
A felhő alapú szolgáltatást használ, mint egy másik központi telepítési beállítást arra parancssori automation [Polip telepítése](http://en.wikipedia.org/wiki/Octopus_Deploy). További információkért lásd: [telepítéséhez az ASP.NET-alkalmazások az Azure-webhelyek](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Parancssori eszközök további információkért tekintse meg a következő erőforráshoz:

* [Egyszerű webalkalmazások: Telepítési](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog által David Ebbo arról az eszközről, hogy egyszerűbb legyen a Web Deploy használatához naplóba írt.
* [Web Deployment eszköz](http://technet.microsoft.com/library/dd568996). A Microsoft TechNet webhelyen hivatalos dokumentációját. Azonban továbbra is egy remek kezdőpont dátummal.
* [Webes telepítése](http://www.iis.net/learn/publish/using-web-deploy). A Microsoft IIS.NET helyen hivatalos dokumentációját. De remek kezdőpont is dátummal.
* [Az ASP.NET Web Deployment Visual Studio használatával: parancssori telepítési](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild a Visual Studio által használt build motor, és azt is használható a parancssorból telepíthessenek webalkalmazásokat a Web Apps. Ez az oktatóanyag egy sorozat, amely főként a Visual Studio telepítésére vonatkozó részét képezi.

