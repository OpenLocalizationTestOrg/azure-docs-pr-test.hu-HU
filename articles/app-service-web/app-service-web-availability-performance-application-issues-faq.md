---
title: "aaaApplication teljesítmény – gyakori kérdések az Azure web Apps |} Microsoft Docs"
description: "Válaszok toofrequently kérdések rendelkezésre állását, teljesítményét és alkalmazással kapcsolatos problémák az Azure App Service Web Apps szolgáltatása hello beolvasása."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: 7f2383743079e4c630fd548b0efd9993029afe11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Az Azure-webalkalmazások alkalmazásteljesítmény – gyakori kérdések

Ez a cikk rendelkezik válaszok toofrequently kérdések (GYIK) kapcsolatos teljesítményproblémák alkalmazás a hello [az Azure App Service Web Apps szolgáltatásának](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Miért van lassú alkalmazásom?

Több tényezővel hozzájárulhat tooslow alkalmazás teljesítménye. Hibaelhárítási lépések részletes: [kapcsolatos problémák elhárítása lassú webes alkalmazás teljesítménye](app-service-web-troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Hogyan hibáinak elhárítása a nagy CPU-felhasználás forgatókönyv?

Néhány magas CPU-felhasználás forgatókönyvet az alkalmazás valóban szüksége elegendő számítási erőforrás. Ebben az esetben érdemes lehet skálázás tooa magasabb szolgáltatási rétegben, így hello alkalmazás szükséges összes hello erőforrás beolvasása. Más, magas CPU-felhasználás oka lehet hibás hurkot vagy egy kódolási gyakorlatot. Mi van kiváltó megnövekedett CPU-felhasználás betekintést első egy kétlépéses folyamat. Először a folyamatkép-kiírást létrehozása, és majd elemezni hello folyamatkép-kiírást. További információkért lásd: [rögzíteni és elemezni a Web Apps magas CPU-felhasználás memóriaképfájl](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Hogyan hibáinak elhárítása a nagy memória-felhasználás forgatókönyv?

Néhány nagy memória-felhasználás forgatókönyvet az alkalmazás valóban szüksége elegendő számítási erőforrás. Ebben az esetben érdemes lehet skálázás tooa magasabb szolgáltatási rétegben, így hello alkalmazás szükséges összes hello erőforrás beolvasása. A többi időszakban hello kódban programhiba okozhatja memóriavesztés. A kódolási célszerű is növelheti a rendszermemóriát. Mi az indításhoz magas memória fogyasztása egy kétlépéses folyamat betekintést beolvasásakor. Először a folyamatkép-kiírást létrehozása, és majd elemezni hello folyamatkép-kiírást. A hello Azure bővítmény tára összeomlási diagnosztikai hatékonyan végezheti mindkét ezeket a lépéseket. További információkért lásd: [rögzíteni és elemezni az időszakos felső memóriaterület memóriaképfájl Web Apps](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>Hogyan automatizálható az App Service web apps PowerShell használatával?

PowerShell-parancsmagok toomanage használja, és az App Service web Apps alkalmazások karbantartása. A blogbejegyzésben [automatizálása a PowerShell használatával az Azure App Service-ben futó webalkalmazások](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), azt ismertetik, hogy hogyan toouse Azure Resource Manager-alapú PowerShell parancsmagok tooautomate gyakori feladatokat. hello blogbejegyzés mintakód a web apps különböző kezelési feladatokat is rendelkezik. Minden App Service web apps parancsmagok szintaxisát és leírásokat, lásd: [AzureRM.Websites](https://docs.microsoft.com/powershell/module/azurerm.websites/?view=azurermps-4.0.0).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>Hogyan tekinthetem meg a webalkalmazás-eseménynaplók?

naplózza az esemény a webalkalmazás tooview:

1. Jelentkezzen be tooyour [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).
2. Hello menüben válasszon ki **Debug konzol** > **CMD**.
3. Jelölje be hello **naplófájlok** mappát.
4. eseménynaplók tooview, válassza ki hello ceruza ikonra mellett túl**eventlog.xml**.
5. toodownload hello naplók hello PowerShell-parancsmag futtatásához `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Hogyan rögzítése a saját webes alkalmazás a felhasználói módú memóriakép?

a felhasználói módú memóriakép webalkalmazás toocapture:

1. Jelentkezzen be tooyour [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).
2. Jelölje be hello **Process Explorer** menü.
3. Kattintson a jobb gombbal hello **w3wp.exe** vagy a webjobs-feladat folyamatban.
4. Válassza ki **memóriakép letöltése** > **teljes biztonsági másolat**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Hogyan tekinthetem meg folyamatszintű információ a webalkalmazás?

A webalkalmazás folyamatszintű adatok megtekintéséhez használatos időkategóriát két lehetőség közül választhat:

*   A hello Azure-portálon:
    1. Nyissa meg hello **Process Explorer** hello webalkalmazás.
    2. toosee hello részleteit, jelölje be hello **w3wp.exe** folyamat.
*   Hello Kudu konzolon:
    1. Jelentkezzen be tooyour [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Jelölje be hello **Process Explorer** menü.
    3. A hello **w3wp.exe** folyamat, jelölje be **tulajdonságok**.

## <a name="when-i-browse-toomy-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>I tallózással toomy app, jelenik meg "Hiba 403 – Ez a webalkalmazás le lett állítva." Hogyan lehet elhárítani ezt?

Három feltétel ezt a hibát okozhatják:

* hello webalkalmazás elérte a számlázási korlátozása, és a webhely le van tiltva.
* hello webalkalmazás hello portálon le lett állítva.
* hello webalkalmazás elérte az erőforrás kvótát tooa ingyenes vagy közös méretezési service-csomag is vonatkozhatnak.

toosee hello hiba és tooresolve hello probléma, hajtsa végre hello kell találniuk lépései [Web Apps: "Hiba 403 – Ez a webalkalmazás leállt"](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Hol kaphatok további információk a kvótái és korlátai különböző App Service-csomagok?

További információ a kvótái és korlátai: [App Service limits](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-hello-response-time-for-hello-first-request-after-idle-time"></a>Hogyan csökkentheti a hello válaszideje hello első kérelem üresjárati idő után?

Alapértelmezés szerint a webalkalmazások a memóriából, ha a beállított időn üresjáratban. Ezzel a módszerrel hello rendszer állapotokban erőforrásokat. hello hátránya, hogy hello válasz toohello irányuló első kérelem után hello webalkalmazás memóriából már, tooallow hello web app tooload, és indítsa el a kérelmek teljesítése. A Basic és Standard service-csomagokról bekapcsolása hello **mindig a** beállítás tookeep hello app mindig be van töltve. Ezzel a megoldással hosszabb a betöltési idők után hello app üresjáratban. toochange hello **mindig a** beállítást:

1. A hello Azure-portálon válassza a tooyour webalkalmazás.
2. Válassza ki **Alkalmazásbeállítások**.
3. A **mindig a**, jelölje be **a**.

## <a name="how-do-i-turned-on-failed-request-tracing"></a>Hogyan kapcsolni a sikertelen kérelmek nyomkövetése?

a sikertelen kérelmek nyomkövetése tooturn:

1. A hello Azure-portálon válassza a tooyour webalkalmazás.
3. Válassza ki **összes beállítás** > **diagnosztikai naplók**.
4. A **sikertelen kérelmek nyomkövetése**, jelölje be **a**.
5. Kattintson a **Mentés** gombra.
6. Hello webalkalmazás panelen, jelölje ki **eszközök**.
7. Válassza ki **Visual Studio online-hoz**.
8. Ha a beállítás hello nem **a**, jelölje be **a**.
9. Válassza ki **Ugrás**.
10. Válassza ki **Web.config**.
11. System.webServer adja hozzá ezt a konfigurációt (toocapture egy adott URL-cím):

    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*api*" />
    <add path="*api*">
    <traceAreas>
    <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
12. tootroubleshoot teljesítményével kapcsolatos problémák, adja hozzá ezt a konfigurációt, (Ha a kérelem rögzítése hello tart több mint 30 másodperc):
    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*" />
    <add path="*">
    <traceAreas> <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions timeTaken="00:00:30" statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
13. toodownload hello sikertelen kérelmek nyomkövetési a hello [portal](https://portal.azure.com)lépjen tooyour webhelyet.
15. Válassza ki **eszközök** > **Kudu** > **Ugrás**.
18. Hello menüben válasszon ki **Debug konzol** > **CMD**.
19. Jelölje be hello **naplófájlok** mappára, majd jelölje ki hello mappa kezdetű névvel **W3SVC**.
20. toosee hello XML-fájlt, jelölje be hello ceruza ikonra.

## <a name="i-see-hello-message-worker-process-requested-recycle-due-toopercent-memory-limit-how-do-i-address-this-issue"></a>Üdvözlőüzenetére látható "munkavégző folyamat újrahasznosítást kért esedékes too'Percent memória" korlátját. " Hogyan kezelje a probléma?

hello rendelkezésre álló memória maximális mérete 32 bites folyamat (akár 64 bites operációs rendszeren) pedig 2 GB. Alapértelmezés szerint a hello munkavégző folyamat too32 bites az App Service-ben (örökölt webes alkalmazásainak kompatibilitás-ellenőrzése) van beállítva.

Fontolja meg az áttérést too64 bites folyamatok, így kihasználhatja hello további rendelkezésre álló memória a webes munkavégző szerepkörben. Ez elindítja a újra kell indítani a webes alkalmazást, úgy ütemezze ennek megfelelően.

Ne feledje, hogy egy 64 bites-környezet megköveteli-e egy alapszintű vagy Standard service-csomagot. Ingyenes, és megosztott tervek mindig 32-bit-es környezetben fusson.

További információkért lásd: [webes alkalmazások konfigurálása az App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-configure).

## <a name="why-does-my-request-time-out-after-240-seconds"></a>Miért nem a kérelem időtúllépés 240 másodperc után?

Az Azure Load Balancer négy perces alapértelmezett időtúllépést beállítás van. Ez általában egy webes kérelem időkorlátja megfelelő választ. Ha a webalkalmazás a háttérben történő feldolgozás van szüksége, Azure webjobs-feladatok használatát javasoljuk. hello Azure web app meghívhatja a webjobs-feladatok, és értesíti, ha a háttérben történő feldolgozása befejeződik. A webjobs-feladatok, beleértve a várólisták és eseményindítók segítségével többféle módszer közül választhat.

Webjobs-feladatok a háttérben történő feldolgozás szolgál. Mértékű háttérben történő feldolgozási webjobs-feladat a kívánt módon teheti meg. További információ a webjobs-feladatok: [háttérfeladatok futtatása a webjobs-feladatok](https://docs.microsoft.com/azure/app-service-web/web-sites-create-web-jobs).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>Az ASP.NET Core alkalmazásokhoz, amelyek az App Service-ben egyes esetekben nem válaszol. Hogyan oldja meg a probléma?

Egy ismert hiba, mely egy korábbi [vércse verzió](https://github.com/aspnet/KestrelHttpServer/issues/1182) okozhat üzemeltetett ASP.NET Core 1.0 alkalmazás az App Service toointermittently válaszol. Akkor is megjelenhet ez az üzenet: "hello a megadott CGI-alkalmazás egy hiba és hello leállt kiszolgáló hello folyamat észlelt."

Ez a probléma fennáll a vércse 1.0.2-es verzióját. Ebben a verzióban az ASP.NET Core 1.0.3 frissítés hello tartalmazza. tooresolve probléma, győződjön meg arról, hogy az alkalmazás függőségeit toouse vércse 1.0.2-es frissíti. Azt is megteheti, hogy is két kerülő megoldások valamelyikével hello blogbejegyzés ismertetett [lassú teljesítmény az ASP.NET Core 1.0 állít ki az App Service web apps](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-hello-file-structure-of-my-web-app-how-can-i-find-them"></a>A naplófájlok hello fájlstruktúra a webalkalmazás nem található. Hogyan tudhatom őket?

Az App Service hello helyi gyorsítótár szolgáltatását használja, ha hello naplófájlok hello mappaszerkezetét és az App Service-példány adatmappáinak érintettek. Helyi gyorsítótár használata esetén almappák hello tárolási naplófájlok és adatmappáinak jönnek létre. hello almappák használata hello elnevezési mintát "egyedi azonosítója" + időbélyegző. Minden almappa mely hello a webalkalmazás fut, vagy futott tooa Virtuálisgép-példány megfelel-e.

függetlenül attól, hogy helyi gyorsítótár, ellenőrizze az App Service toodetermine **Alkalmazásbeállítások** fülre. Ha a helyi gyorsítótárban van használatban, hello Alkalmazásbeállítás `WEBSITE_LOCAL_CACHE_OPTION` értéke túl`Always`. Helyi gyorsítótárával kapcsolatos további információkért lásd: hello [App szolgáltatás helyi gyorsítótárából áttekintése](https://docs.microsoft.com/azure/app-service/app-service-local-cache).

Ha nem használja a helyi gyorsítótárban, és ezzel a problémával találkozik, a támogatási kérelem elküldéséhez.

## <a name="i-see-hello-message-an-attempt-was-made-tooaccess-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>Üdvözlőüzenetére látható "kísérlet történt egy szoftvercsatorna tooaccess végrehajtott oly módon, a hozzáférési engedélyeket tiltott." Hogyan lehet elhárítani ezt?

Ez a hiba rendszerint azért fordul elő, ha elfogytak, hello kimenő TCP-kapcsolatok hello Virtuálisgép-példányon. Az App Service-ben vannak korlátozva hello beállíthatja, hogy minden egyes Virtuálisgép-példány kimenő kapcsolatok maximális számát. További információkért lásd: [Cross-VM numerikus korlátok](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Ez a hiba is akkor fordulhat elő, ha a helyi cím tooaccess kísérli meg az alkalmazásból. További információkért lásd: [helyi cím kérelmek](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Kimenő kapcsolatok a web app alkalmazásban kapcsolatos további információkért lásd: kapcsolatos blogbejegyzést hello [kimenő kapcsolatok tooAzure webhelyek](http://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-tooremote-debug-my-app-service-web-app"></a>Hogyan használható a Visual Studio tooremote hibakeresési my App Service webalkalmazás?

Részletes útmutatást, amely bemutatja, hogyan toodebug a Visual Studio használatával webalkalmazás: [távoli hibakereséshez az App Service webalkalmazásba](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
