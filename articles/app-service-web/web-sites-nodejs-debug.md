---
title: "aaaHow toodebug Node.js-webalkalmazás az Azure App Service-ben"
description: "Ismerje meg, hogyan toodebug egy Node.js webalkalmazás az Azure App Service-ben."
tags: azure-portal
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: a48f906c-1a3e-43bc-ae84-7d2dde175b15
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 888ec5c3f92cfc3aeea4ea86005b9b6a0d1306ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-a-nodejs-web-app-in-azure-app-service"></a><span data-ttu-id="66353-103">Hogyan toodebug egy Node.js webalkalmazás az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="66353-103">How toodebug a Node.js web app in Azure App Service</span></span>
<span data-ttu-id="66353-104">Azure biztosít a hibakeresés üzemeltetett Node.js-alkalmazások beépített diagnosztika tooassist [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="66353-104">Azure provides built-in diagnostics tooassist with debugging Node.js applications hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="66353-105">Ebből a cikkből megtudhatja, hogyan stdout és az stderr-en, a hello böngészőben megjelenített hibaadatok és a hogyan toodownload és nézet naplófájlok tooenable naplózása.</span><span class="sxs-lookup"><span data-stu-id="66353-105">In this article, you will learn how tooenable logging of stdout and stderr, display error information in hello browser, and how toodownload and view log files.</span></span>

<span data-ttu-id="66353-106">Az Azure-platformon futó Node.js alkalmazások diagnosztika által biztosított [IISNode].</span><span class="sxs-lookup"><span data-stu-id="66353-106">Diagnostics for Node.js applications hosted on Azure is provided by [IISNode].</span></span> <span data-ttu-id="66353-107">Ez a cikk ismerteti, amelyek a hello leggyakrabban használt beállítások diagnosztikai adatok gyűjtésének, amíg nem biztosít teljes IISNode való munkához.</span><span class="sxs-lookup"><span data-stu-id="66353-107">While this article discusses hello most common settings for gathering diagnostics information, it does not provide a complete reference for working with IISNode.</span></span> <span data-ttu-id="66353-108">Az IISNode munkavégzés további információkért lásd: hello [IISNode információs] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="66353-108">For more information on working with IISNode, see hello [IISNode Readme] on GitHub.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="66353-109">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="66353-109">Enable logging</span></span>
<span data-ttu-id="66353-110">Alapértelmezés szerint egy App Service webalkalmazásba csak telepítések, például a Git használatával webes alkalmazás telepítésekor diagnosztikai adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="66353-110">By default, an App Service web app only captures diagnostic information about deployments, such as when you deploy a web app using Git.</span></span> <span data-ttu-id="66353-111">Ez az információ akkor hasznos, ha problémák merülnek fel a hivatkozott modul telepítésekor hiba például a telepítés során **package.json**, vagy ha egy egyéni telepítési parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="66353-111">This information is useful if you are having problems during deployment, such as a failure when installing a module referenced in **package.json**, or if you are using a custom deployment script.</span></span>

<span data-ttu-id="66353-112">tooenable hello naplózás az stdout és az stderr adatfolyamok, létre kell hoznia egy **iisnode.yml fájlt** a(z) hello a Node.js-alkalmazás gyökérkönyvtárában, és adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="66353-112">tooenable hello logging of stdout and stderr streams, you must create an **IISNode.yml** file at hello root of your Node.js application and add hello following:</span></span>

    loggingEnabled: true

<span data-ttu-id="66353-113">Ez lehetővé teszi a stderr-en és a Node.js-alkalmazás az stdout hello naplózása.</span><span class="sxs-lookup"><span data-stu-id="66353-113">This enables hello logging of stderr and stdout from your Node.js application.</span></span>

<span data-ttu-id="66353-114">Hello **iisnode.yml fájlt** fájl is lehet használt toocontrol e egyéni hibaüzenetek vagy fejlesztői hibák visszakerülnek toohello böngésző, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="66353-114">hello **IISNode.yml** file can also be used toocontrol whether friendly errors or developer errors are returned toohello browser when a failure occurs.</span></span> <span data-ttu-id="66353-115">tooenable fejlesztői hibák, adja hozzá a következő sor toohello hello **iisnode.yml fájlt** fájlt:</span><span class="sxs-lookup"><span data-stu-id="66353-115">tooenable developer errors, add hello following line toohello **IISNode.yml** file:</span></span>

    devErrorsEnabled: true

<span data-ttu-id="66353-116">Ha engedélyezi ezt a beállítást, az IISNode utolsó 64K továbbított toostderr helyett egy rövid hiba, például a "egy belső kiszolgálóhiba történt" hello ad vissza.</span><span class="sxs-lookup"><span data-stu-id="66353-116">Once this option is enabled, IISNode will return hello last 64K of information sent toostderr instead of a friendly error such as "an internal server error occurred".</span></span>

> [!NOTE]
> <span data-ttu-id="66353-117">DevErrorsEnabled akkor hasznos, ha problémák diagnosztizálása a fejlesztés során, amíg a termelési környezetben engedélyezné azt az elküldött tooend felhasználók fejlesztési hibákat eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="66353-117">While devErrorsEnabled is useful when diagnosing problems during development, enabling it in a production environment may result in development errors being sent tooend users.</span></span>
> 
> 

<span data-ttu-id="66353-118">Ha hello **iisnode.yml fájlt** fájl már nem létezett az alkalmazáson belül, újra kell indítania a webalkalmazás frissítése hello alkalmazás közzététele után.</span><span class="sxs-lookup"><span data-stu-id="66353-118">If hello **IISNode.yml** file did not already exist within your application, you must restart your web app after publishing hello updated application.</span></span> <span data-ttu-id="66353-119">Egyszerűen módosítani egy meglévő beállítások **iisnode.yml fájlt** korábban közzétett fájl, újraindításra nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="66353-119">If you are simply changing settings in an existing **IISNode.yml** file that has previously been published, no restart is required.</span></span>

> [!NOTE]
> <span data-ttu-id="66353-120">Ha a webalkalmazás létrejött hello Azure parancssori eszközök, vagy az Azure PowerShell-parancsmagok, egy alapértelmezett **iisnode.yml fájlt** fájl automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="66353-120">If your web app was created using hello Azure Command-Line Tools or Azure PowerShell Cmdlets, a default **IISNode.yml** file is automatically created.</span></span>
> 
> 

<span data-ttu-id="66353-121">toorestart hello web app alkalmazásban válassza hello webalkalmazás hello [Azure Portal](https://portal.azure.com), és kattintson a **indítsa újra a** gombra:</span><span class="sxs-lookup"><span data-stu-id="66353-121">toorestart hello web app, select hello web app in hello [Azure Portal](https://portal.azure.com), and then click **RESTART** button:</span></span>

![Indítsa újra a gomb][restart-button]

<span data-ttu-id="66353-123">Ha hello Azure parancssori eszköz a fejlesztési környezetben van telepítve, a következő parancs toorestart hello webalkalmazás hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="66353-123">If hello Azure Command-Line Tools are installed in your development environment, you can use hello following command toorestart hello web app:</span></span>

    azure site restart [sitename]

> [!NOTE]
> <span data-ttu-id="66353-124">Míg loggingEnabled és devErrorsEnabled leggyakrabban használt hello iisnode.yml fájlt konfigurációs beállítások diagnosztikai adatok rögzítéséhez, iisnode.yml fájlt lehet használt tooconfigure számos lehetőséget a üzemeltetési környezet.</span><span class="sxs-lookup"><span data-stu-id="66353-124">While loggingEnabled and devErrorsEnabled are hello most commonly used IISNode.yml configuration options for capturing diagnostic information, IISNode.yml can be used tooconfigure a variety of options for your hosting environment.</span></span> <span data-ttu-id="66353-125">Hello konfigurációs beállítások teljes listáját lásd: hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fájlt.</span><span class="sxs-lookup"><span data-stu-id="66353-125">For a full list of hello configuration options, see hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) file.</span></span>
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a><span data-ttu-id="66353-126">Naplók elérése</span><span class="sxs-lookup"><span data-stu-id="66353-126">Accessing logs</span></span>
<span data-ttu-id="66353-127">Diagnosztikai naplók elérhető háromféleképpen; Hello File Transfer Protocol (FTP), egy Zip-archívum letöltése használatával, vagy mint egy élő adatfolyam hello napló (más néven utóhívás) frissítése.</span><span class="sxs-lookup"><span data-stu-id="66353-127">Diagnostic logs can be accessed in three ways; Using hello File Transfer Protocol (FTP), downloading a Zip archive, or as a live updated stream of hello log (also known as a tail).</span></span> <span data-ttu-id="66353-128">A Zip-archívum hello hello naplófájlok letöltése vagy megtekinti a hello élő adatfolyam szükséges hello Azure parancssori eszközök.</span><span class="sxs-lookup"><span data-stu-id="66353-128">Downloading hello Zip archive of hello log files or viewing hello live stream require hello Azure Command-Line Tools.</span></span> <span data-ttu-id="66353-129">Ezek a hello a következő parancs használatával telepíthető:</span><span class="sxs-lookup"><span data-stu-id="66353-129">These can be installed by using hello following command:</span></span>

    npm install azure-cli -g

<span data-ttu-id="66353-130">A telepítést követően hello eszközök segítségével férhetők el hello "azure" parancsot.</span><span class="sxs-lookup"><span data-stu-id="66353-130">Once installed, hello tools can be accessed using hello 'azure' command.</span></span> <span data-ttu-id="66353-131">hello parancssori eszközöket kell konfigurált toouse az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="66353-131">hello command-line tools must first be configured toouse your Azure subscription.</span></span> <span data-ttu-id="66353-132">Hogyan tooaccomplish ennek a feladatnak a információkért lásd: hello **hogyan toodownload-importálási közzétételi beállítások** hello szakasza [hogyan tooUse hello Azure parancssori eszközök](../xplat-cli-connect.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="66353-132">For information on how tooaccomplish this task, see hello **How toodownload and import publish settings** section of hello [How tooUse hello Azure Command-Line Tools](../xplat-cli-connect.md) article.</span></span>

### <a name="ftp"></a><span data-ttu-id="66353-133">FTP</span><span class="sxs-lookup"><span data-stu-id="66353-133">FTP</span></span>
<span data-ttu-id="66353-134">tooaccess hello diagnosztikai adatokat keresztül FTP, látogasson el a hello [Azure Portal](https://portal.azure.com), a webes alkalmazást, majd válassza ki és hello **IRÁNYÍTÓPULT**.</span><span class="sxs-lookup"><span data-stu-id="66353-134">tooaccess hello diagnostic information through FTP, visit hello [Azure Portal](https://portal.azure.com), select your web app, and then select hello **DASHBOARD**.</span></span> <span data-ttu-id="66353-135">A hello **Gyorshivatkozások** című szakaszba hello **FTP diagnosztikai naplók** és **diagnosztikai naplók FTPS** hivatkozásokra kattintva belépési toohello naplók hello FTP protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="66353-135">In hello **quick links** section, hello **FTP DIAGNOSTIC LOGS** and **FTPS DIAGNOSTIC LOGS** links provide access toohello logs using hello FTP protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="66353-136">Ha nem korábban konfigurált felhasználónevet és jelszót FTP- vagy a központi telepítés, ehhez a hello **gyors üzembe helyezés** kezelése lap választásával **üzembe helyezési hitelesítő adatok beállítása**.</span><span class="sxs-lookup"><span data-stu-id="66353-136">If you have not previously configured user name and password for FTP or deployment, you can do so from hello **Quickstart** management page by selecting **Set up deployment credentials**.</span></span>
> 
> 

<span data-ttu-id="66353-137">hello hello irányítópulton visszaadott FTP URL van hello **naplófájlok** könyvtár, amely tartalmazza a következő alkönyvtár hello:</span><span class="sxs-lookup"><span data-stu-id="66353-137">hello FTP URL returned in hello dashboard is for hello **LogFiles** directory, which will contain hello following sub-directories:</span></span>

* <span data-ttu-id="66353-138">[Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy könyvtárat hello ugyanazzal a névvel jön létre, és információt tartalmaz a kapcsolódó toodeployments.</span><span class="sxs-lookup"><span data-stu-id="66353-138">[Deployment Method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of hello same name will be created and will contain information related toodeployments.</span></span>
* <span data-ttu-id="66353-139">nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)</span><span class="sxs-lookup"><span data-stu-id="66353-139">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="zip-archive"></a><span data-ttu-id="66353-140">A ZIP-archívum</span><span class="sxs-lookup"><span data-stu-id="66353-140">Zip archive</span></span>
<span data-ttu-id="66353-141">Zip-archívumból hello diagnosztikai naplók, a következő parancsot a hello Azure parancssori eszközök használata hello toodownload:</span><span class="sxs-lookup"><span data-stu-id="66353-141">toodownload a Zip archive of hello diagnostic logs, use hello following command from hello Azure Command-Line Tools:</span></span>

    azure site log download [sitename]

<span data-ttu-id="66353-142">Ez letölti egy **diagnostics.zip** hello aktuális könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="66353-142">This will download a **diagnostics.zip** in hello current directory.</span></span> <span data-ttu-id="66353-143">Ebből az archívumból tartalmazza a következő könyvtárstruktúrát hello:</span><span class="sxs-lookup"><span data-stu-id="66353-143">This archive contains hello following directory structure:</span></span>

* <span data-ttu-id="66353-144">központi telepítések - információ az alkalmazás központi naplózása</span><span class="sxs-lookup"><span data-stu-id="66353-144">deployments - A log of information about deployments of your application</span></span>
* <span data-ttu-id="66353-145">Naplófájlok</span><span class="sxs-lookup"><span data-stu-id="66353-145">LogFiles</span></span>
  
  * <span data-ttu-id="66353-146">[Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy könyvtárat hello ugyanazzal a névvel jön létre, és információt tartalmaz a kapcsolódó toodeployments.</span><span class="sxs-lookup"><span data-stu-id="66353-146">[Deployment method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of hello same name will be created and will contain information related toodeployments.</span></span>
  * <span data-ttu-id="66353-147">nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)</span><span class="sxs-lookup"><span data-stu-id="66353-147">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="live-stream-tail"></a><span data-ttu-id="66353-148">Élő stream (utóhívás)</span><span class="sxs-lookup"><span data-stu-id="66353-148">Live stream (tail)</span></span>
<span data-ttu-id="66353-149">diagnosztikai szereplő információkat, a következő parancsot a hello Azure parancssori eszközök használata hello élő adatfolyam tooview:</span><span class="sxs-lookup"><span data-stu-id="66353-149">tooview a live stream of diagnostic log information, use hello following command from hello Azure Command-Line Tools:</span></span>

    azure site log tail [sitename]

<span data-ttu-id="66353-150">Ezzel visszatér a naplóeseményeket hello kiszolgálón előforduló frissített adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="66353-150">This will return a stream of log events that are updated as they occur on hello server.</span></span> <span data-ttu-id="66353-151">Ez az adatfolyam visszaadható központi telepítési információk, valamint az stdout és az stderr adatokat (ha loggingEnabled értéke igaz.)</span><span class="sxs-lookup"><span data-stu-id="66353-151">This stream will return deployment information as well as stdout and stderr information (when loggingEnabled is true.)</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="66353-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66353-152">Next Steps</span></span>
<span data-ttu-id="66353-153">Az e cikkben megtanulta, hogyan meg tooenable és hozzáférés az Azure diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="66353-153">In this article you learned how tooenable and access diagnostics information for Azure.</span></span> <span data-ttu-id="66353-154">Amíg ez az információ a ismertetése előforduló problémákat az alkalmazással, akkor lehetséges, hogy pont tooa probléma használ modul vagy a Node.js App Service Web Apps által használt hello verzió eltér hello egyet a központi telepítésben használja környezet.</span><span class="sxs-lookup"><span data-stu-id="66353-154">While this information is useful in understanding problems that occur with your application, it may point tooa problem with a module you are using or that hello version of Node.js used by App Service Web Apps is different than hello one used in your deployment environment.</span></span>

<span data-ttu-id="66353-155">Információ a modulok használata Azure: [Node.js modulok használata Azure alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md).</span><span class="sxs-lookup"><span data-stu-id="66353-155">For information in working with modules on Azure, see [Using Node.js Modules with Azure Applications](../nodejs-use-node-modules-azure-apps.md).</span></span>

<span data-ttu-id="66353-156">Információ az alkalmazás a Node.js verzió megadása: [a Node.js verzió megadása az Azure alkalmazásban].</span><span class="sxs-lookup"><span data-stu-id="66353-156">For information on specifying a Node.js version for your application, see [Specifying a Node.js version in an Azure application].</span></span>

<span data-ttu-id="66353-157">További információkért lásd még: hello [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="66353-157">For more information, see also hello [Node.js Developer Center](/develop/nodejs/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="66353-158">A változások</span><span class="sxs-lookup"><span data-stu-id="66353-158">What's changed</span></span>
* <span data-ttu-id="66353-159">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="66353-159">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="66353-160">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="66353-160">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="66353-161">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="66353-161">No credit cards required; no commitments.</span></span>
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode információs]: https://github.com/tjanczuk/iisnode#readme
[How tooUse hello Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[a Node.js verzió megadása az Azure alkalmazásban]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

