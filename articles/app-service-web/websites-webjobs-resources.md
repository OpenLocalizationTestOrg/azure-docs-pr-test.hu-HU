---
title: "Azure WebJobs-dokumentáció erőforrások"
description: "Információforrások Azure webjobs-feladatok és az Azure WebJobs SDK használata ajánlott."
services: app-service
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: ed005e56-4334-4641-a5e5-15435c2be36b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2017
ms.author: glenga
ms.openlocfilehash: 05062d7396bdbb3e589d2ab5f0443d1dca54342a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs-dokumentáció erőforrások
## <a name="overview"></a>Áttekintés
Ez a témakör a dokumentumok forrását Azure webjobs-feladatok és az Azure WebJobs SDK használatával kapcsolatos mutató hivatkozásokat tartalmaz. Azure webjobs-feladatok környezetében háttérfolyamatként programok telepítése és a parancsfájlok futtatásához könnyű megoldást biztosítson egy [App Service webalkalmazásba, az API-alkalmazás vagy a mobilalkalmazás](../app-service/app-service-value-prop-what-is.md). Töltse fel, és egy végrehajtható fájl például futtató cmd, bat, exe (.NET) ps1, sh, php, másolása, js, és jar. Ezeket a programokat (cron) ütemezés szerint vagy folyamatos webjobs-feladatok futtatása.

A célja a [WebJobs SDK](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk) egyszerűbbé teheti a webjobs-feladat hajthat végre, például képek feldolgozását, a várólista feldolgozása, a RSS összesítési, a fájlkarbantartás, kapcsolatos általános feladatok írt kódot, és e-mailek küldésekor. A WebJobs SDK az Azure Storage és a Service Bus, a feladatütemezés és a hibák kezelése és sok más szabhatják beépített szolgáltatásai rendelkezik. Emellett úgy van kialakítva, fogva bővíthető, és nincs olyan [nyílt forráskódú extensions tárháza](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Az Azure Functions](../azure-functions/functions-overview.md) (jelenleg előzetes verzió), amely kompatibilis a C# parancsfájl, Node.js és egyéb nyelvek a WebJobs SDK verzióján alapul. 

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

Létrehozás, telepítés és webjobs-feladatok kezelésére a Visual Studio integrált tooling zökkenőmentes. Webjobs-feladatok létrehozása sablonból, közzététele és kezelése (futtassa, állítsa le, figyeléséhez és hibakeresési) őket. 

A WebJobs-irányítópulttal az Azure portálon hatékony funkciókat biztosít, amely a webjobs-feladatok, például a webjobs-feladatok belül egyedi függvények meghívása végrehajtásának teljes szabályozható. Az irányítópult is függvény futtatókörnyezetek és naplózási kimeneti jeleníti meg. 

## <a name="getstarted"></a>Ismerkedés a webjobs-feladatok és a WebJobs SDK
* [Bevezetés az Azure webjobs-feladatok](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure webjobs-feladatok Soft, és el kell kezdenie a használatuk most!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blogbejegyzés által Troy Hunt.)
* [Azure WebJobs-funkciók](https://azure.microsoft.com/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Mi az a WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Háttér feladatok útmutatást által a Microsoft minták és gyakorlatok](https://docs.microsoft.com/azure/architecture/best-practices/background-jobs)
* [A 1.1.0-ás bejelentése RTM Microsoft Azure WebJobs SDK](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Ismerkedés az Azure WebJobs SDK-val](websites-dotnet-webjobs-sdk-get-started.md)
* [Az Azure Queue Storage használata a WebJobs SDK-val](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [How to use Azure blob storage with the WebJobs SDK (Az Azure Blob Storage használata a WebJobs SDK-val)](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Az Azure Table Storage használata a WebJobs SDK-val](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Az Azure Service Bus használata a WebJobs SDK-val](websites-dotnet-webjobs-sdk-service-bus.md)
* [Azure WebJobs SDK rövid összefoglaló (PDF-fájl letöltése)](https://go.microsoft.com/fwlink/p/?linkid=845558)
* [Webjobs-feladatok beállítások dokumentáció a Githubon](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videók
  * [Webjobs-feladatok és a WebJobs SDK](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
  * [Azure webjobs-feladatok videósorozat a Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
  * [A Visual Studio szükséges WebJobs bemutatása](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [Webjobs-feladatok Tooling és a távoli hibakeresés](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
  * [Azure webjobs-feladatok frissítés Pranav Rastogi - bővíthetőségi 1.1-es kiadás](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

További információ a következő szakaszok a [telepítése webjobs-feladatok](#deploy) és [tesztelés és a WebJobs-hibakeresés](#debug).

## <a name="deploy"></a>Webjobs-feladatok telepítése
* [Hogyan telepítése Azure WebJobs Visual Studio használatával](websites-dotnet-deploy-webjobs.md)
* [Központi telepítése webjobs-feladatok az Azure portál használatával](web-sites-create-web-jobs.md)
* [Az Azure webjobs-feladatok parancssori vagy folyamatos kézbesítését engedélyezése](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [A .NET-Konzolalkalmazás Azure webjobs-feladatok használatával történő telepítésének Git](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Az F # webjobs-feladat telepítése az Azure-bA](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Azure webjobs-feladatok, egyéni szolgáltatások telepítése](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videók
  * [A Visual Studio szükséges WebJobs bemutatása](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
  * [Webjobs-feladatok Tooling és a távoli hibakeresés](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="schedule"></a>Webjobs-feladatok ütemezése
* [Az Azure webjobs-feladat párbeszédpanel hozzáadása](websites-dotnet-deploy-webjobs.md#configure)
* [Ütemezett webjobs-feladat létrehozása az Azure portálon](web-sites-create-web-jobs.md#CreateScheduled)
* [A webjobs-feladat ütemezési feladat csatlakoztatása](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Cron-kifejezés az Azure webjobs-feladatok ütemezése](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [A WebJobs SDK TimerTrigger használatával funkciók webjobs-feladat ütemezése](websites-dotnet-webjobs-sdk.md#schedule)

## <a name="debug"></a>Tesztelés és hibakeresés webjobs-feladatok
* [Új fejlesztői és az Azure webjobs-feladatok a Visual Studio hibakeresési szolgáltatásai](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [A WebJobs-irányítópulttal megtekintése](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Írás a WebJobs SDK használatával a naplókat, és megtekinthetők az irányítópulton](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Távoli hibakeresési webjobs-feladatok](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Aki megírt, hogy a blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [A felhőalapú üzemeltetési interaktív kód](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Nyomkövetési felvétele az Azure webjobs-feladatok](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Figyelése, diagnosztizálása és elhárítása a Microsoft Azure tárolás](../storage/common/storage-monitoring-diagnosing-troubleshooting.md)
* Videók
  * [Webjobs-feladatok Tooling és a távoli hibakeresés](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

## <a name="scale"></a>Webjobs-feladatok méretezése
* [A webalkalmazás az Azure-webhelyek skálázás](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Az Azure App Service: Újratervezni a jelentős mértékű készen áll az üzleti webalkalmazások](https://channel9.msdn.com/Events/Build/2014/3-626). Magában foglalja a webjobs-feladatok, többek között a WebJobs SDK webalkalmazások méretezés.
* Videók
  * [Webjobs-feladatok kiterjesztése](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

## <a name="additional"></a>Webjobs-feladatok további források
* [Azure webjobs-feladatok GA blogbejegyzés Magnus Mårtensson által](http://magnusmartensson.com/azure-webjobs-ga)
* [Powershell webes feladatok futtatása az Azure App Service](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Az első értesítés a kiválasztásakor az Azure webjobs-feladatok befejezése](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [A webjobs-feladatok egyszerű webes alkalmazás biztonsági mentés megőrzési házirend](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure-webalkalmazásokban és Felhőszolgáltatások lassú első kérésre](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Az AlwaysOn funkciót, amelyet csak a Standard tarifacsomag szimulálása webjobs-feladatok használatát mutatja.
* [Webjobs-feladatok szabályos leállítást](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). A WebJobs SDK szabályos leállítást, lásd: [szabályos leállítást](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Azure webjobs-feladatok & AzCopy biztonsági másolatok automatizálása](http://markjbrown.com/azure-webjobs-azcopy/)
* Videók
  * [Azure WebJobs-videók Magnus Mårtensson által](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
  * [Azure webjobs-feladatok videósorozat a Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="additionalsdk"></a>További WebJobs SDK-erőforrások
* [WebJobs SDK kibocsátási megjegyzései](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK forráskódja](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK-bővítmények forráskód](https://github.com/Azure/azure-webjobs-sdk-extensions), a [részletes útmutató a bővíthetőségi modell](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [WebJobs SDK parancsfájl forráskód](https://github.com/Azure/azure-webjobs-sdk-script/) (használt [Azure Functions](../azure-functions/functions-overview.md))
* [Webjobs-feladat FREB fájlok feltöltése az Azure Storage a WebJobs SDK használatával](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [A naplózási előnyt egy Azure-ból az Azure webjobs-feladatok Azure kívül üzemeltet, üzemeltetett webjobs-feladat](http://bypassion.dk/?p=510)
* [Azure webjobs-feladatok adatok importálása eszköz létrehozása](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Az Azure Functions áttekintése](../azure-functions/functions-overview.md)
* Videók
  * [Azure webjobs-feladatok videósorozat a Channel 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

## <a name="samples"></a>Mintaalkalmazások webjobs-feladat
* [A WebJobs-csapat a Githubon által biztosított mintaalkalmazások](https://github.com/azure/azure-webjobs-sdk-samples)
* [Egyszerű Azure Web Apps WebJobs-fájlok a WebJobs SDK használatával](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Ütemezett és eseményvezérelt webjobs-feladatok használatát mutatja be. A következő blogbejegyzésben található [Azure WebJobs SDK használatával SiteMonitR újraépítése](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

## <a name="blogs"></a>Blogok
* [Azure blog](/blog)
* [Amit Apple blog](http://blog.amitapple.com/). Fókusz a webjobs-feladatok (az SDK nem).
* [Magnus Mårtensson blog](http://magnusmartensson.com/)

## <a name="gethelp"></a>A webjobs-feladatok súgója
* [A webjobs-feladatok StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [A WebJobs SDK StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [Az Azure Functions a StackOverflow](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure és az ASP.NET-fórum](http://forums.asp.net/1247.aspx)
* [Az Azure App Service Web Apps-fórum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Az Azure Web Apps User Voice-hely](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). A hashtaggel történő #AzureWebJobs használja.
* [A webjobs-feladatok hiba vagy probléma](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

