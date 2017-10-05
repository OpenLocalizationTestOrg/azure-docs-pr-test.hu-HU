---
title: "A Node.js webalkalmazás hibakeresése az Azure App Service-ben"
description: "További tudnivalók az Azure App Service a Node.js webalkalmazás hibakeresése."
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
ms.openlocfilehash: 5e302a4c58a171d40e43a22c34c724e868019ec8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a><span data-ttu-id="2173a-103">A Node.js webalkalmazás hibakeresése az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="2173a-103">How to debug a Node.js web app in Azure App Service</span></span>
<span data-ttu-id="2173a-104">Azure biztosít, elősegítve ezzel a hibakeresés üzemeltetett Node.js-alkalmazások beépített diagnosztika [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2173a-104">Azure provides built-in diagnostics to assist with debugging Node.js applications hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="2173a-105">Ebből a cikkből megtudhatja, hogy a stdout és az stderr naplózását, a hiba adatait a böngészőben megjelenő, és töltse le és naplófájlban talál.</span><span class="sxs-lookup"><span data-stu-id="2173a-105">In this article, you will learn how to enable logging of stdout and stderr, display error information in the browser, and how to download and view log files.</span></span>

<span data-ttu-id="2173a-106">Az Azure-platformon futó Node.js alkalmazások diagnosztika által biztosított [IISNode].</span><span class="sxs-lookup"><span data-stu-id="2173a-106">Diagnostics for Node.js applications hosted on Azure is provided by [IISNode].</span></span> <span data-ttu-id="2173a-107">Ez a cikk ismerteti a leggyakoribb beállítások diagnosztikai adatok gyűjtésének, amíg nem biztosít teljes IISNode való munkához.</span><span class="sxs-lookup"><span data-stu-id="2173a-107">While this article discusses the most common settings for gathering diagnostics information, it does not provide a complete reference for working with IISNode.</span></span> <span data-ttu-id="2173a-108">Az IISNode munkavégzés további információkért lásd: a [IISNode információs] a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2173a-108">For more information on working with IISNode, see the [IISNode Readme] on GitHub.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="2173a-109">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2173a-109">Enable logging</span></span>
<span data-ttu-id="2173a-110">Alapértelmezés szerint egy App Service webalkalmazásba csak telepítések, például a Git használatával webes alkalmazás telepítésekor diagnosztikai adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="2173a-110">By default, an App Service web app only captures diagnostic information about deployments, such as when you deploy a web app using Git.</span></span> <span data-ttu-id="2173a-111">Ez az információ akkor hasznos, ha problémák merülnek fel a hivatkozott modul telepítésekor hiba például a telepítés során **package.json**, vagy ha egy egyéni telepítési parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="2173a-111">This information is useful if you are having problems during deployment, such as a failure when installing a module referenced in **package.json**, or if you are using a custom deployment script.</span></span>

<span data-ttu-id="2173a-112">Az stdout és az stderr adatfolyamok a naplózás engedélyezéséhez létre kell hoznia egy **iisnode.yml fájlt** fájlt a Node.js-alkalmazás gyökérkönyvtárában, és adja hozzá a következő:</span><span class="sxs-lookup"><span data-stu-id="2173a-112">To enable the logging of stdout and stderr streams, you must create an **IISNode.yml** file at the root of your Node.js application and add the following:</span></span>

    loggingEnabled: true

<span data-ttu-id="2173a-113">Ez lehetővé teszi az stderr-en és a Node.js-alkalmazás az stdout naplózását.</span><span class="sxs-lookup"><span data-stu-id="2173a-113">This enables the logging of stderr and stdout from your Node.js application.</span></span>

<span data-ttu-id="2173a-114">A **iisnode.yml fájlt** fájl is is kell annak vezérlésére szolgál, hogy egyéni hibaüzenetek és a fejlesztői hibát a rendszer visszairányítja a böngésző Ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="2173a-114">The **IISNode.yml** file can also be used to control whether friendly errors or developer errors are returned to the browser when a failure occurs.</span></span> <span data-ttu-id="2173a-115">Fejlesztői hibák engedélyezéséhez vegye fel a következő sort a **iisnode.yml fájlt** fájlt:</span><span class="sxs-lookup"><span data-stu-id="2173a-115">To enable developer errors, add the following line to the **IISNode.yml** file:</span></span>

    devErrorsEnabled: true

<span data-ttu-id="2173a-116">Ha engedélyezi ezt a beállítást, az IISNode, mint például "egy belső kiszolgálóhiba történt" helyett a felhasználóbarát hibaüzenet stderr elküldött adatok utolsó 64K ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2173a-116">Once this option is enabled, IISNode will return the last 64K of information sent to stderr instead of a friendly error such as "an internal server error occurred".</span></span>

> [!NOTE]
> <span data-ttu-id="2173a-117">Amíg devErrorsEnabled akkor hasznos, ha problémák diagnosztizálása a fejlesztés során, a termelési környezetben engedélyezné azt az küldi el a végfelhasználók fejlesztési hibákat eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="2173a-117">While devErrorsEnabled is useful when diagnosing problems during development, enabling it in a production environment may result in development errors being sent to end users.</span></span>
> 
> 

<span data-ttu-id="2173a-118">Ha a **iisnode.yml fájlt** fájl már nem létezett az alkalmazáson belül, a frissített alkalmazás közzététele után újra kell indítania a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2173a-118">If the **IISNode.yml** file did not already exist within your application, you must restart your web app after publishing the updated application.</span></span> <span data-ttu-id="2173a-119">Egyszerűen módosítani egy meglévő beállítások **iisnode.yml fájlt** korábban közzétett fájl, újraindításra nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="2173a-119">If you are simply changing settings in an existing **IISNode.yml** file that has previously been published, no restart is required.</span></span>

> [!NOTE]
> <span data-ttu-id="2173a-120">Ha a webalkalmazás létrehozása az Azure parancssori eszközök vagy az Azure PowerShell-parancsmagok, egy alapértelmezett **iisnode.yml fájlt** fájl automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="2173a-120">If your web app was created using the Azure Command-Line Tools or Azure PowerShell Cmdlets, a default **IISNode.yml** file is automatically created.</span></span>
> 
> 

<span data-ttu-id="2173a-121">A webalkalmazás újraindítása, válassza ki a webalkalmazás a [Azure Portal](https://portal.azure.com), és kattintson a **indítsa újra a** gombra:</span><span class="sxs-lookup"><span data-stu-id="2173a-121">To restart the web app, select the web app in the [Azure Portal](https://portal.azure.com), and then click **RESTART** button:</span></span>

![Indítsa újra a gomb][restart-button]

<span data-ttu-id="2173a-123">Ha a fejlesztési környezetet az Azure parancssori eszközök vannak telepítve, a következő paranccsal indítsa újra a webes alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="2173a-123">If the Azure Command-Line Tools are installed in your development environment, you can use the following command to restart the web app:</span></span>

    azure site restart [sitename]

> [!NOTE]
> <span data-ttu-id="2173a-124">Míg loggingEnabled és devErrorsEnabled a leggyakrabban használt iisnode.yml fájlt konfigurációs beállítások diagnosztikai adatok rögzítéséhez, iisnode.yml fájlt számos lehetőséget a üzemeltetési környezet konfigurálásához használható.</span><span class="sxs-lookup"><span data-stu-id="2173a-124">While loggingEnabled and devErrorsEnabled are the most commonly used IISNode.yml configuration options for capturing diagnostic information, IISNode.yml can be used to configure a variety of options for your hosting environment.</span></span> <span data-ttu-id="2173a-125">A konfigurációs beállítások teljes listáját lásd: a [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fájlt.</span><span class="sxs-lookup"><span data-stu-id="2173a-125">For a full list of the configuration options, see the [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) file.</span></span>
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a><span data-ttu-id="2173a-126">Naplók elérése</span><span class="sxs-lookup"><span data-stu-id="2173a-126">Accessing logs</span></span>
<span data-ttu-id="2173a-127">Diagnosztikai naplók elérhető háromféleképpen; A File Transfer Protocol (FTP), egy Zip-archívum letöltése használatával, vagy mint egy élő adatfolyam a napló (más néven utóhívás) frissítése.</span><span class="sxs-lookup"><span data-stu-id="2173a-127">Diagnostic logs can be accessed in three ways; Using the File Transfer Protocol (FTP), downloading a Zip archive, or as a live updated stream of the log (also known as a tail).</span></span> <span data-ttu-id="2173a-128">A Zip-archívumot, a naplófájlok letöltése, illetve az élő adatfolyam megtekintése megkövetelése az Azure parancssori eszközök.</span><span class="sxs-lookup"><span data-stu-id="2173a-128">Downloading the Zip archive of the log files or viewing the live stream require the Azure Command-Line Tools.</span></span> <span data-ttu-id="2173a-129">Ezek a következő paranccsal telepíthető:</span><span class="sxs-lookup"><span data-stu-id="2173a-129">These can be installed by using the following command:</span></span>

    npm install azure-cli -g

<span data-ttu-id="2173a-130">A telepítést követően az eszközök segítségével érhető el az "azure" paranccsal.</span><span class="sxs-lookup"><span data-stu-id="2173a-130">Once installed, the tools can be accessed using the 'azure' command.</span></span> <span data-ttu-id="2173a-131">A parancssori eszközök először konfigurálni kell az Azure-előfizetés használatára.</span><span class="sxs-lookup"><span data-stu-id="2173a-131">The command-line tools must first be configured to use your Azure subscription.</span></span> <span data-ttu-id="2173a-132">Ennek a feladatnak kapcsolatos információkért lásd: a **letöltése és importálása közzétételi beállítások** szakasza a [hogyan használható az Azure parancssori eszközt](../xplat-cli-connect.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2173a-132">For information on how to accomplish this task, see the **How to download and import publish settings** section of the [How to Use The Azure Command-Line Tools](../xplat-cli-connect.md) article.</span></span>

### <a name="ftp"></a><span data-ttu-id="2173a-133">FTP</span><span class="sxs-lookup"><span data-stu-id="2173a-133">FTP</span></span>
<span data-ttu-id="2173a-134">A diagnosztikai információk eléréséhez FTP keresztül, látogasson el a [Azure Portal](https://portal.azure.com), a webes alkalmazást, majd válassza ki és a **IRÁNYÍTÓPULT**.</span><span class="sxs-lookup"><span data-stu-id="2173a-134">To access the diagnostic information through FTP, visit the [Azure Portal](https://portal.azure.com), select your web app, and then select the **DASHBOARD**.</span></span> <span data-ttu-id="2173a-135">Az a **Gyorshivatkozások** szakaszban, a **FTP diagnosztikai naplók** és **diagnosztikai naplók FTPS** hivatkozásokat biztosít hozzáférést a naplókat az FTP protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="2173a-135">In the **quick links** section, the **FTP DIAGNOSTIC LOGS** and **FTPS DIAGNOSTIC LOGS** links provide access to the logs using the FTP protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="2173a-136">Ha nem korábban konfigurált felhasználónevet és jelszót FTP- vagy a központi telepítés, ehhez a a **gyors üzembe helyezés** kezelése lap választásával **üzembe helyezési hitelesítő adatok beállítása**.</span><span class="sxs-lookup"><span data-stu-id="2173a-136">If you have not previously configured user name and password for FTP or deployment, you can do so from the **Quickstart** management page by selecting **Set up deployment credentials**.</span></span>
> 
> 

<span data-ttu-id="2173a-137">Az FTP URL-cím, az irányítópult visszaadott a **naplófájlok** könyvtár, amely tartalmazza a következő alkönyvtárat:</span><span class="sxs-lookup"><span data-stu-id="2173a-137">The FTP URL returned in the dashboard is for the **LogFiles** directory, which will contain the following sub-directories:</span></span>

* <span data-ttu-id="2173a-138">[Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy azonos nevű könyvtárat jön létre, és a központi telepítések kapcsolatos adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2173a-138">[Deployment Method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of the same name will be created and will contain information related to deployments.</span></span>
* <span data-ttu-id="2173a-139">nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)</span><span class="sxs-lookup"><span data-stu-id="2173a-139">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="zip-archive"></a><span data-ttu-id="2173a-140">A ZIP-archívum</span><span class="sxs-lookup"><span data-stu-id="2173a-140">Zip archive</span></span>
<span data-ttu-id="2173a-141">Egy Zip-archívum a diagnosztikai naplók letöltéséhez az Azure parancssori eszközök a következő parancs használható:</span><span class="sxs-lookup"><span data-stu-id="2173a-141">To download a Zip archive of the diagnostic logs, use the following command from the Azure Command-Line Tools:</span></span>

    azure site log download [sitename]

<span data-ttu-id="2173a-142">Ez letölti egy **diagnostics.zip** az aktuális könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="2173a-142">This will download a **diagnostics.zip** in the current directory.</span></span> <span data-ttu-id="2173a-143">Ebből az archívumból tartalmazza a következő könyvtárstruktúrát:</span><span class="sxs-lookup"><span data-stu-id="2173a-143">This archive contains the following directory structure:</span></span>

* <span data-ttu-id="2173a-144">központi telepítések - információ az alkalmazás központi naplózása</span><span class="sxs-lookup"><span data-stu-id="2173a-144">deployments - A log of information about deployments of your application</span></span>
* <span data-ttu-id="2173a-145">Naplófájlok</span><span class="sxs-lookup"><span data-stu-id="2173a-145">LogFiles</span></span>
  
  * <span data-ttu-id="2173a-146">[Az üzembe helyezési módszer](web-sites-deploy.md) -használhatja egy központi telepítési módszer, például a Git, ha egy azonos nevű könyvtárat jön létre, és a központi telepítések kapcsolatos adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2173a-146">[Deployment method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of the same name will be created and will contain information related to deployments.</span></span>
  * <span data-ttu-id="2173a-147">nodejs - Stdout és az stderr információk rögzítése az alkalmazás összes példányát (teljesülésekor loggingEnabled.)</span><span class="sxs-lookup"><span data-stu-id="2173a-147">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="live-stream-tail"></a><span data-ttu-id="2173a-148">Élő stream (utóhívás)</span><span class="sxs-lookup"><span data-stu-id="2173a-148">Live stream (tail)</span></span>
<span data-ttu-id="2173a-149">Egy élő adatfolyam diagnosztikai naplófájl-információk megtekintéséhez az Azure parancssori eszközök a következő parancs használható:</span><span class="sxs-lookup"><span data-stu-id="2173a-149">To view a live stream of diagnostic log information, use the following command from the Azure Command-Line Tools:</span></span>

    azure site log tail [sitename]

<span data-ttu-id="2173a-150">Ezzel visszatér a naplóeseményeket a kiszolgálón előforduló frissített adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="2173a-150">This will return a stream of log events that are updated as they occur on the server.</span></span> <span data-ttu-id="2173a-151">Ez az adatfolyam visszaadható központi telepítési információk, valamint az stdout és az stderr adatokat (ha loggingEnabled értéke igaz.)</span><span class="sxs-lookup"><span data-stu-id="2173a-151">This stream will return deployment information as well as stdout and stderr information (when loggingEnabled is true.)</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="2173a-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2173a-152">Next Steps</span></span>
<span data-ttu-id="2173a-153">Ebben a cikkben megtanulta, engedélyezése, és hozzáférés az Azure diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="2173a-153">In this article you learned how to enable and access diagnostics information for Azure.</span></span> <span data-ttu-id="2173a-154">Míg ezek az információk ismertetése problémákat, amelyek a az alkalmazás hasznos, akkor használ, vagy Node.js App Service Web Apps által használt verziója nem egyezik a központi telepítési környezetben használt modul probléma mutat.</span><span class="sxs-lookup"><span data-stu-id="2173a-154">While this information is useful in understanding problems that occur with your application, it may point to a problem with a module you are using or that the version of Node.js used by App Service Web Apps is different than the one used in your deployment environment.</span></span>

<span data-ttu-id="2173a-155">Információ a modulok használata Azure: [Node.js modulok használata Azure alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md).</span><span class="sxs-lookup"><span data-stu-id="2173a-155">For information in working with modules on Azure, see [Using Node.js Modules with Azure Applications](../nodejs-use-node-modules-azure-apps.md).</span></span>

<span data-ttu-id="2173a-156">Információ az alkalmazás a Node.js verzió megadása: [a Node.js verzió megadása az Azure alkalmazásban].</span><span class="sxs-lookup"><span data-stu-id="2173a-156">For information on specifying a Node.js version for your application, see [Specifying a Node.js version in an Azure application].</span></span>

<span data-ttu-id="2173a-157">További információ: a [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="2173a-157">For more information, see also the [Node.js Developer Center](/develop/nodejs/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="2173a-158">A változások</span><span class="sxs-lookup"><span data-stu-id="2173a-158">What's changed</span></span>
* <span data-ttu-id="2173a-159">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="2173a-159">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="2173a-160">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="2173a-160">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2173a-161">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="2173a-161">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="2173a-162">[IISNode]: https://github.com/tjanczuk/iisnode</span><span class="sxs-lookup"><span data-stu-id="2173a-162">[IISNode]: https://github.com/tjanczuk/iisnode</span></span>
<span data-ttu-id="2173a-163">[IISNode információs]: https://github.com/tjanczuk/iisnode#readme</span><span class="sxs-lookup"><span data-stu-id="2173a-163">[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme</span></span>
[How to Use The Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
<span data-ttu-id="2173a-164">[a Node.js verzió megadása az Azure alkalmazásban]: ../nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="2173a-164">[Specifying a Node.js version in an Azure application]: ../nodejs-specify-node-version-azure-apps.md</span></span>

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

