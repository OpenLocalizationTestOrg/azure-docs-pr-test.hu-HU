---
title: "Node.js csevegőalkalmazás létrehozása a Socket.IO segítségével az Azure App Service-ben"
description: "Ez az oktatóanyag bemutatja a socket.io segítségével az Azure-platformon futó node.js webalkalmazás."
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c4c4af36-3ecf-4619-b586-ca90d53ce35b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: e1aa539e1134884261ea7464bfda6d14815618d4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a><span data-ttu-id="af715-103">Node.js csevegőalkalmazás létrehozása a Socket.IO segítségével az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="af715-103">Create a Node.js chat application with Socket.IO in Azure App Service</span></span>
<span data-ttu-id="af715-104">Socket.IO biztosít a valós idejű kommunikációját a node.js-kiszolgáló és az ügyfél websocket elemeket.</span><span class="sxs-lookup"><span data-stu-id="af715-104">Socket.IO provides real-time communication between your node.js server and clients using WebSockets.</span></span> <span data-ttu-id="af715-105">Visszalépnek más átvitelek (például hosszú lekérdezési), amely a régebbi böngészők is támogatja.</span><span class="sxs-lookup"><span data-stu-id="af715-105">It also supports fallback to other transports (such as long polling) that work with older browsers.</span></span> <span data-ttu-id="af715-106">Ez az oktatóanyag végigvezetik egy alapú Socket.IO Csevegés alkalmazásra, amely egy Azure webalkalmazás üzemeltetéséhez, és bemutatják a használó méretezni [Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="af715-106">This tutorial will walk you through hosting a Socket.IO based chat application as an Azure web app, and show you how to scale the application using [Azure Redis Cache].</span></span> <span data-ttu-id="af715-107">A Socket.IO további információkért lásd: <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="af715-107">For more information on Socket.IO, see <http://socket.io/>.</span></span>

> [!NOTE]
> <span data-ttu-id="af715-108">Ebben a feladatban eljárásokat alkalmazni [App Service Web Apps]; Felhőszolgáltatásai számára, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].</span><span class="sxs-lookup"><span data-stu-id="af715-108">The procedures in this task apply to [App Service Web Apps]; for Cloud Services, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>
> 
> 

## <a name="download-the-chat-example"></a><span data-ttu-id="af715-109">Töltse le a Csevegés – példa</span><span class="sxs-lookup"><span data-stu-id="af715-109">Download the chat example</span></span>
<span data-ttu-id="af715-110">Ebben a projektben a Csevegés példa fogjuk használni a [Socket.IO GitHub-tárházban].</span><span class="sxs-lookup"><span data-stu-id="af715-110">For this project, we will use the chat example from the [Socket.IO GitHub repository].</span></span> <span data-ttu-id="af715-111">A következő lépésekkel töltse le a példában, és adja hozzá a projekthez, korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="af715-111">Perform the following steps to download the example and add it to the project you previously created.</span></span>

1. <span data-ttu-id="af715-112">Töltse le a [ZIP- vagy GZ archivált kiadás] a Socket.IO projekt (a dokumentum a 1.3.5 verziója lett megadva)</span><span class="sxs-lookup"><span data-stu-id="af715-112">Download a [ZIP or GZ archived release] of the Socket.IO project (version 1.3.5 was used for this document)</span></span>
2. <span data-ttu-id="af715-113">Bontsa ki a az archiválás és másolja a **példák\\Csevegés** könyvtár, egy új helyre.</span><span class="sxs-lookup"><span data-stu-id="af715-113">Extract the archive and copy the **examples\\chat** directory to a new location.</span></span> <span data-ttu-id="af715-114">Például  **\\csomópont\\Csevegés**.</span><span class="sxs-lookup"><span data-stu-id="af715-114">For example, **\\node\\chat**.</span></span>

## <a name="modify-appjs-and-install-modules"></a><span data-ttu-id="af715-115">Az App.js fájl módosítása és -modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="af715-115">Modify app.js and install modules</span></span>
1. <span data-ttu-id="af715-116">Nevezze át a **index.js** fájl **app.js**.</span><span class="sxs-lookup"><span data-stu-id="af715-116">Rename the **index.js** file to **app.js**.</span></span> <span data-ttu-id="af715-117">Ez lehetővé teszi az Azure észleli, hogy ez a Node.js-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="af715-117">This allows Azure to detect that this is a Node.js application.</span></span>
2. <span data-ttu-id="af715-118">Nyissa meg a **app.js** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="af715-118">Open the **app.js** file in a text editor.</span></span> <span data-ttu-id="af715-119">Módosítsa a sor tartalmazó `var io = require('../..')(server);` alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="af715-119">Change the line containing `var io = require('../..')(server);` as shown below:</span></span>
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. <span data-ttu-id="af715-120">Nyissa meg a **package.json** fájlt, és adjon hozzá egy hivatkozást, a socket.io `dependencies`, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="af715-120">Open the **package.json** file and add a reference to socket.io under `dependencies`, as shown below:</span></span>
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. <span data-ttu-id="af715-121">A parancssorból, módosítsa a  **\\csomópont\\Csevegés** könyvtárra, és használja a jelen alkalmazás által igényelt moduljainak telepítése npm:</span><span class="sxs-lookup"><span data-stu-id="af715-121">From the command-line, change to the **\\node\\chat** directory and use npm to install the modules required by this application:</span></span>
   
        npm install
   
    <span data-ttu-id="af715-122">Ezzel telepíti a modulok nevű almappába **node_modules**.</span><span class="sxs-lookup"><span data-stu-id="af715-122">This will install the modules into a subfolder named **node_modules**.</span></span>

## <a name="create-an-azure-web-app"></a><span data-ttu-id="af715-123">Azure-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="af715-123">Create an Azure Web App</span></span>
<span data-ttu-id="af715-124">A lépések végrehajtásával hozzon létre egy Azure webalkalmazás Git-közzététel engedélyezése és engedélyeznie kell a webalkalmazás WebSocket támogatása.</span><span class="sxs-lookup"><span data-stu-id="af715-124">Follow these steps to create an Azure web app, enable Git publishing, and then enable WebSocket support for the web app.</span></span>

> [!NOTE]
> <span data-ttu-id="af715-125">Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="af715-125">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="af715-126">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="af715-126">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="af715-127">További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="af715-127">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

1. <span data-ttu-id="af715-128">Az Azure parancssori felület (CLI) telepítése, és csatlakozzon az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="af715-128">Install the Azure Command-Line Interface (Azure CLI) and connect to your Azure subscription.</span></span> <span data-ttu-id="af715-129">Lásd: [telepítése és konfigurálása az Azure parancssori felület](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="af715-129">See [Install and Configure the Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="af715-130">Ha ez az első idő beállítása a tárházat az Azure-ban, a bejelentkezési hitelesítő adatok létrehozásához szüksége.</span><span class="sxs-lookup"><span data-stu-id="af715-130">If this is your first time setting up a repository in Azure, you need to create login credentials.</span></span> <span data-ttu-id="af715-131">Az Azure parancssori felületen adja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="af715-131">From the Azure CLI, enter the following command:</span></span>
   
        azure site deployment user set [username] [password]
3. <span data-ttu-id="af715-132">Módosítsa a  **\\node\chat** könyvtárra, és használja a következő paranccsal hozhat létre egy új Azure web app és egy helyi Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="af715-132">Change to the **\\node\chat** directory and use the following command to create a new Azure web app and a local Git repository.</span></span> <span data-ttu-id="af715-133">Ez a parancs is létrehoz egy Git távoli elnevezett "azure".</span><span class="sxs-lookup"><span data-stu-id="af715-133">This command also creates a Git remote named 'azure'.</span></span>
   
        azure site create mysitename --git
   
    <span data-ttu-id="af715-134">A webalkalmazás egyedi nevű, "mysitename" le kell cserélnie.</span><span class="sxs-lookup"><span data-stu-id="af715-134">You must replace 'mysitename' with a unique name for your web app.</span></span>
4. <span data-ttu-id="af715-135">Véglegesítse a meglévő fájlokat a helyi tárházba a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="af715-135">Commit the existing files to the local repository by using the following commands:</span></span>
   
        git add .
        git commit -m "Initial commit"
5. <span data-ttu-id="af715-136">A fájlok az Azure Web Apps tárház leküldése a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="af715-136">Push the files to the Azure Web Apps repository with the following command:</span></span>
   
        git push azure master
   
    <span data-ttu-id="af715-137">Amikor a rendszer kéri, adja meg a hitelesítő adatokat a 2. lépésben.</span><span class="sxs-lookup"><span data-stu-id="af715-137">When prompted, enter your credentials from step 2.</span></span> <span data-ttu-id="af715-138">Állapotüzenetek fog kapni, mivel a rendszer importálja a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="af715-138">You will receive status messages as modules are imported on the server.</span></span> <span data-ttu-id="af715-139">Ez a folyamat befejezése után az alkalmazás fogja üzemeltetni az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="af715-139">Once this process has completed, the application will be hosted on your Azure web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="af715-140">Modul a telepítés során azt tapasztalhatja, hibák, amelyek "... az importált projekt nem található".</span><span class="sxs-lookup"><span data-stu-id="af715-140">During module installation, you may notice errors that 'The imported project ... was not found'.</span></span> <span data-ttu-id="af715-141">Ezek biztonságosan figyelmen kívül hagyható.</span><span class="sxs-lookup"><span data-stu-id="af715-141">These can safely be ignored.</span></span>
   > 
   > 
6. <span data-ttu-id="af715-142">Socket.IO használ a websocket elemek, amelyek az Azure-on alapértelmezés szerint nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="af715-142">Socket.IO uses WebSockets, which are not enabled by default on Azure.</span></span> <span data-ttu-id="af715-143">Webes szoftvercsatornák engedélyezéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="af715-143">To enable web sockets, use the following command:</span></span>
   
        azure site set -w
   
    <span data-ttu-id="af715-144">Ha a rendszer kéri, adja meg a webalkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="af715-144">If prompted, enter the name of the web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="af715-145">A "az azure site set -w" parancs fog csak verziójával 0.7.4 munkahelyi vagy az Azure parancssori felület magasabb.</span><span class="sxs-lookup"><span data-stu-id="af715-145">The 'azure site set -w' command will work only with version 0.7.4 or higher of the Azure Command-Line Interface.</span></span> <span data-ttu-id="af715-146">WebSocket-támogatás segítségével is engedélyezheti a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af715-146">You can also enable WebSocket support using the [Azure Portal](https://portal.azure.com).</span></span>
   > 
   > <span data-ttu-id="af715-147">Ahhoz, hogy a websocket elemek az Azure portál használatával, kattintson a webalkalmazás a Web Apps paneljéről, kattintson **összes beállítás** > **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="af715-147">To enable WebSockets using the Azure Portal, click the web app from the Web Apps blade, click **All settings** > **Application settings**.</span></span> <span data-ttu-id="af715-148">A **webes szoftvercsatornák**, kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="af715-148">Under **Web Sockets**, click **On**.</span></span> <span data-ttu-id="af715-149">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="af715-149">Then click **Save**.</span></span>
   > 
   > 
7. <span data-ttu-id="af715-150">A webalkalmazás Azure megtekintéséhez a következő paranccsal indítsa el a webböngészőt, és keresse meg a futtatott webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="af715-150">To view the web app on Azure, use the following command to launch your web browser and navigate to the hosted web app:</span></span>
   
        azure site browse

<span data-ttu-id="af715-151">Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="af715-151">Your app is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

## <a name="scale-out"></a><span data-ttu-id="af715-152">Horizontális felskálázás</span><span class="sxs-lookup"><span data-stu-id="af715-152">Scale out</span></span>
<span data-ttu-id="af715-153">Socket.IO alkalmazások kiterjeszthető használatával egy **adapter** üzenetek és több alkalmazáspéldányt események terjesztését.</span><span class="sxs-lookup"><span data-stu-id="af715-153">Socket.IO applications can be scaled out by using an **adapter** to distribute messages and events between multiple application instances.</span></span> <span data-ttu-id="af715-154">Több adapterek nincsenek elérhető, amíg a [socket.io-redis] adapter könnyen használható az Azure Redis Cache szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="af715-154">While there are several adapters available, the [socket.io-redis] adapter can be easily used with the Azure Redis Cache feature.</span></span>

> [!NOTE]
> <span data-ttu-id="af715-155">Egy további követelmény a Socket.IO megoldás kiterjesztése a kapcsolódó munkamenetek támogatása.</span><span class="sxs-lookup"><span data-stu-id="af715-155">An additional requirement for scaling out a Socket.IO solution is support for sticky sessions.</span></span> <span data-ttu-id="af715-156">A kapcsolódó munkamenetek alapértelmezés szerint engedélyezve vannak a Azure Web Apps használatával Azure alkalmazáskérelmek irányítására.</span><span class="sxs-lookup"><span data-stu-id="af715-156">Sticky sessions are enabled by default for Azure Web Apps through Azure Request Routing.</span></span> <span data-ttu-id="af715-157">További információkért lásd: [példány kapcsolat az Azure-webhelyek].</span><span class="sxs-lookup"><span data-stu-id="af715-157">For more information, see [Instance Affinity in Azure Web Sites].</span></span>
> 
> 

### <a name="create-a-redis-cache"></a><span data-ttu-id="af715-158">A Redis gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="af715-158">Create a Redis cache</span></span>
<span data-ttu-id="af715-159">Hajtsa végre a lépéseket [a gyorsítótár létrehozása az Azure Redis Cache] új gyorsítótárat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="af715-159">Perform the steps in [Create a cache in Azure Redis Cache] to create a new cache.</span></span>

> [!NOTE]
> <span data-ttu-id="af715-160">Mentse a **állomásnév** és **elsődleges kulcs** a gyorsítótárhoz, ezek szükség lesz a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="af715-160">Save the **Host name** and **Primary key** for your cache, as these will be needed in the next steps.</span></span>
> 
> 

### <a name="add-the-redis-and-socketio-redis-modules"></a><span data-ttu-id="af715-161">Adja hozzá a redis és socket.io-redis-modulok</span><span class="sxs-lookup"><span data-stu-id="af715-161">Add the redis and socket.io-redis modules</span></span>
1. <span data-ttu-id="af715-162">A parancssorból, módosítsa a  **\\csomópont\\Csevegés** könyvtárra, és használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="af715-162">From a command-line, change to the **\\node\\chat** directory and use the following command.</span></span>
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > <span data-ttu-id="af715-163">Ez a parancs-ben megadott verzió a-e ez a cikk tesztelése során használt.</span><span class="sxs-lookup"><span data-stu-id="af715-163">The versions specified in this command are the versions used when testing this article.</span></span>
   > 
   > 
2. <span data-ttu-id="af715-164">Módosítsa a **app.js** fájlt adja hozzá a következő sor után azonnal`var io = require('socket.io')(server);`</span><span class="sxs-lookup"><span data-stu-id="af715-164">Modify the **app.js** file to add the following lines immediately after `var io = require('socket.io')(server);`</span></span>
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    <span data-ttu-id="af715-165">Cserélje le **redishostname** és **rediskey** az állomásnév és a Redis gyorsítótár kulccsal.</span><span class="sxs-lookup"><span data-stu-id="af715-165">Replace **redishostname** and **rediskey** with the host name and key for your Redis cache.</span></span>
   
    <span data-ttu-id="af715-166">Ez a publish készít, és ügyfél számára a korábban létrehozott Redis gyorsítótárt fizessen elő.</span><span class="sxs-lookup"><span data-stu-id="af715-166">This will create a publish and subscribe client to the Redis cache created previously.</span></span> <span data-ttu-id="af715-167">Az ügyfelek majd a adapterrel konfigurálására szolgálnak az üzenetek és események továbbításához az alkalmazás példányai között a Redis gyorsítótárt használandó Socket.IO</span><span class="sxs-lookup"><span data-stu-id="af715-167">The clients are then used with the adapter to configure Socket.IO to use the Redis cache for passing messages and events between instances of your application</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="af715-168">Amíg a **socket.io-redis** adapter kommunikálhatnak közvetlenül a Redis, hogy a jelenlegi verziója nem támogatja az Azure Redis cache-szükséges hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="af715-168">While the **socket.io-redis** adapter can communicate directly to Redis, the current version does not support the authentication required by Azure Redis cache.</span></span> <span data-ttu-id="af715-169">Ezért a kezdeti kapcsolat létrehozása a **redis** modul, akkor az ügyfél átadott a **socket.io-redis** adapter.</span><span class="sxs-lookup"><span data-stu-id="af715-169">So the initial connection is created using the **redis** module, then the client is passed to the **socket.io-redis** adapter.</span></span>
   > 
   > <span data-ttu-id="af715-170">Azure Redis Cache-példányokhoz biztonságos 6380 portot használ, miközben ebben a példában használt modulok nem támogatja a biztonságos kapcsolatokat 7/14/2014-től.</span><span class="sxs-lookup"><span data-stu-id="af715-170">While Azure Redis Cache supports secure connections using port 6380, the modules used in this example do not support secure connections as of 7/14/2014.</span></span> <span data-ttu-id="af715-171">A fenti kódot használja az alapértelmezett, 6379 nem biztonságos portja.</span><span class="sxs-lookup"><span data-stu-id="af715-171">The above code uses the default, unsecure port of 6379.</span></span>
   > 
   > 
3. <span data-ttu-id="af715-172">Mentse a módosított **app.js**</span><span class="sxs-lookup"><span data-stu-id="af715-172">Save the modified **app.js**</span></span>

### <a name="commit-changes-and-redeploy"></a><span data-ttu-id="af715-173">Véglegesítse a módosításokat, és telepítse újra</span><span class="sxs-lookup"><span data-stu-id="af715-173">Commit changes and redeploy</span></span>
<span data-ttu-id="af715-174">Parancsot a parancssorból a  **\\csomópont\\Csevegés** directory, a következő parancsok segítségével a változtatások véglegesítése a határidő, és telepítse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="af715-174">From the command-line in the **\\node\\chat** directory, use the following commands to commit changes and redeploy the application.</span></span>

    git add .
    git commit -m "implementing scale out"
    git push azure master

<span data-ttu-id="af715-175">Miután a változások értesítését a kiszolgálóra, a következő paranccsal a webhely méretezheti több példánya között.</span><span class="sxs-lookup"><span data-stu-id="af715-175">Once the changes have been pushed to the server, you can scale your site across multiple instances by using the following command.</span></span>

    azure site scale instances --instances #

<span data-ttu-id="af715-176">Ha  **#**  létrehozásához példányok száma.</span><span class="sxs-lookup"><span data-stu-id="af715-176">Where **#** is the number of instances to create.</span></span>

<span data-ttu-id="af715-177">A webalkalmazás több böngészők vagy számítógépek számára, győződjön meg arról, hogy megfelelően küldi el az összes ügyfél csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="af715-177">You can connect to your web app from multiple browsers or computers to verify that messages are correctly sent to all clients.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="af715-178">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="af715-178">Troubleshooting</span></span>
### <a name="connection-limits"></a><span data-ttu-id="af715-179">Kapcsolat korlátai</span><span class="sxs-lookup"><span data-stu-id="af715-179">Connection limits</span></span>
<span data-ttu-id="af715-180">Az Azure Web Apps több SKU, amelyek megadják a webhely számára elérhető erőforrások érhető el.</span><span class="sxs-lookup"><span data-stu-id="af715-180">Azure Web Apps is available in multiple SKUs, which determine the resources available to your site.</span></span> <span data-ttu-id="af715-181">Ez magában foglalja az engedélyezett WebSocket-kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="af715-181">This includes the number of allowed WebSocket connections.</span></span> <span data-ttu-id="af715-182">További információkért lásd: a [Web Apps árképzést ismertető oldalra].</span><span class="sxs-lookup"><span data-stu-id="af715-182">For more information, see the [Web Apps Pricing page].</span></span>

### <a name="messages-arent-being-sent-using-websockets"></a><span data-ttu-id="af715-183">Üzenetek nem küldött websocket elemek használatával</span><span class="sxs-lookup"><span data-stu-id="af715-183">Messages aren't being sent using WebSockets</span></span>
<span data-ttu-id="af715-184">Ha az ügyfelek böngészőin tartsa visszaváltás websocket elemek használata helyett hosszú jelenség, lehet a következők valamelyike miatt.</span><span class="sxs-lookup"><span data-stu-id="af715-184">If client browsers keep falling back to long polling instead of using WebSockets, it may be because of one of the following.</span></span>

* <span data-ttu-id="af715-185">**Próbálja a átvitel, csak a websocket elemek korlátozása**</span><span class="sxs-lookup"><span data-stu-id="af715-185">**Try limiting the transport to just WebSockets**</span></span>
  
    <span data-ttu-id="af715-186">Ahhoz, hogy az üzenetkezelési átvitel websocket elemek használandó Socket.IO a kiszolgáló és az ügyfél támogatnia kell websocket elemeket.</span><span class="sxs-lookup"><span data-stu-id="af715-186">In order for Socket.IO to use WebSockets as the messaging transport, both the server and client must support WebSockets.</span></span> <span data-ttu-id="af715-187">Ha az egyiket nem, a Socket.IO egyezteti egy másik átviteltől, pl. hosszú lekérdezési.</span><span class="sxs-lookup"><span data-stu-id="af715-187">If one or the other does not, Socket.IO will negotiate another transport, such as long polling.</span></span> <span data-ttu-id="af715-188">Az átviteli módokat Socket.IO által használt alapértelmezett listája ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span><span class="sxs-lookup"><span data-stu-id="af715-188">The default list of transports used by Socket.IO is ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span></span> <span data-ttu-id="af715-189">Beállíthatja úgy, hogy csak használjon szoftvercsatornák használatával adja hozzá az alábbi kódot a **app.js** fájl, a sor tartalmazó után `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="af715-189">You can force it to only use WebSockets by adding the following code to the **app.js** file, after the line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > <span data-ttu-id="af715-190">Figyelje meg, hogy a régebbi websocket elemek nem támogató böngészők nem tudnak kapcsolódni a helyhez, amíg a fenti kódot aktív, websocket elemek csak kommunikációt korlátozza.</span><span class="sxs-lookup"><span data-stu-id="af715-190">Note that older browsers that do not support WebSockets will not be able to connect to the site while the above code is active, as it restricts communication to WebSockets only.</span></span>
  > 
  > 
* <span data-ttu-id="af715-191">**SSL használata**</span><span class="sxs-lookup"><span data-stu-id="af715-191">**Use SSL**</span></span>
  
    <span data-ttu-id="af715-192">Websocket elemek, mint támaszkodik néhány kisebb használt HTTP-fejléceket, a **frissítése** fejléc.</span><span class="sxs-lookup"><span data-stu-id="af715-192">WebSockets relies on some lesser used HTTP headers, such as the **Upgrade** header.</span></span> <span data-ttu-id="af715-193">Néhány, a közbenső hálózati eszközök, például webes proxykat, ezek a fejlécek távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="af715-193">Some intermediate network devices, such as web proxies, may remove these headers.</span></span> <span data-ttu-id="af715-194">Ez a probléma elkerülése érdekében létrehozhat a WebSocket-kapcsolat SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="af715-194">To avoid this problem, you can establish the WebSocket connection over SSL.</span></span>
  
    <span data-ttu-id="af715-195">Ehhez egyszerűen, hogy konfigurálja a Socket.IO `match origin protocol`.</span><span class="sxs-lookup"><span data-stu-id="af715-195">An easy way to accomplish this is to configure Socket.IO to `match origin protocol`.</span></span> <span data-ttu-id="af715-196">Ez arra utasítja a Socket.IO ugyanaz, mint az eredeti HTTP/HTTPS-kérést a weblap websocket elemek kommunikáció biztonságossá tételére.</span><span class="sxs-lookup"><span data-stu-id="af715-196">This instructs Socket.IO to secure WebSockets communication the same as the originating HTTP/HTTPS request for the web page.</span></span> <span data-ttu-id="af715-197">Ha egy böngésző és látogasson el a webhely egy HTTPS URL-címet használ, a további WebSocket kommunikáció Socket.IO segítségével megfelelően történik SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="af715-197">If a browser uses an HTTPS URL to visit your website, subsequent WebSocket communications through Socket.IO will be secured over SSL.</span></span>
  
    <span data-ttu-id="af715-198">Ebben a példában ahhoz, hogy ez a konfiguráció módosításához adja hozzá a következő kódot a **app.js** fájlt, miután a sor tartalmazó `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="af715-198">To modify this example to enable this configuration, add the following code to the **app.js** file after the line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* <span data-ttu-id="af715-199">**Ellenőrizze a web.config beállításait**</span><span class="sxs-lookup"><span data-stu-id="af715-199">**Verify web.config settings**</span></span>
  
    <span data-ttu-id="af715-200">Azure-webalkalmazásokban üzemeltető Node.js alkalmazások használatát a **web.config** továbbítani a beérkező kéréseket a Node.js-alkalmazás a fájlt.</span><span class="sxs-lookup"><span data-stu-id="af715-200">Azure web apps that host Node.js applications use the **web.config** file to route incoming requests to the Node.js application.</span></span> <span data-ttu-id="af715-201">A Node.js-alkalmazások, és helyesen függvény szoftvercsatornák használatával a **web.config** kell a következő bejegyzés szerepelhet.</span><span class="sxs-lookup"><span data-stu-id="af715-201">For WebSockets to function correctly with Node.js applications, the **web.config** must contain the following entry.</span></span>
  
        <webSocket enabled="false"/>
  
    <span data-ttu-id="af715-202">Ez letiltja az IIS websocket elemek modult, amely magában foglalja a saját végrehajtásának websocket elemek és a Node.js adott WebSocket modulok például Socket.IO ütközik.</span><span class="sxs-lookup"><span data-stu-id="af715-202">This disables the IIS WebSockets module, which includes its own implementation of WebSockets and conflicts with Node.js specific WebSocket modules such as Socket.IO.</span></span> <span data-ttu-id="af715-203">Ha a sor nem észlelhető, vagy értékre van állítva `true`, ennek az lehet az oka, hogy a WebSocket-átvitel nem működik az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="af715-203">If this line is not present, or is set to `true`, this may be the reason that the WebSocket transport is not working for your application.</span></span>
  
    <span data-ttu-id="af715-204">Általában a Node.js-alkalmazások nem tartalmaznak egy **web.config** fájlt, így Azure Websitesra automatikusan létrehoz egy Node.js-alkalmazások a rendszer telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="af715-204">Normally, Node.js applications do not include a **web.config** file, so Azure Websites will automatically generate one for Node.js applications when they are deployed.</span></span> <span data-ttu-id="af715-205">Ez a fájl automatikusan létrejön a kiszolgálón, mert a fájl megtekintéséhez az FTP- vagy FTPS URL-CÍMÉT a webhely kell használnia.</span><span class="sxs-lookup"><span data-stu-id="af715-205">Since this file is automatically generated on the server, you must use the FTP or FTPS URL for your website to view this file.</span></span> <span data-ttu-id="af715-206">Találhat meg az FTP és FTPS URL-címek a klasszikus portál webhelyét a webalkalmazás kiválasztásával, majd a **irányítópult** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="af715-206">You can find the FTP and FTPS URLs for your site in the classic portal by selecting your web app, and then the **Dashboard** link.</span></span> <span data-ttu-id="af715-207">Az URL-címek jelennek meg a **gyors áttekintő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="af715-207">The URLs are displayed in the **quick glance** section.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="af715-208">A **web.config** csak létre fájlt Azure Websitesra Ha az alkalmazás nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="af715-208">The **web.config** file is only generated by Azure Websites if your application does not provide one.</span></span> <span data-ttu-id="af715-209">Ha megad egy **web.config** fájlt a projekt gyökérkönyvtárában azt által használt Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="af715-209">If you provide a **web.config** file in the root of your application project, it will be used by Azure Web Apps.</span></span>
  > 
  > 
  
    <span data-ttu-id="af715-210">Ha a bejegyzés nincs jelen, vagy értékre van állítva `true`, akkor létre kell hoznia egy **web.config** a Node.js-alkalmazás gyökérkönyvtárában, és adjon meg egy értéket a `false`.</span><span class="sxs-lookup"><span data-stu-id="af715-210">If the entry is not present, or is set to a value of `true`, then you should create a **web.config** in the root of your Node.js application and specify a value of `false`.</span></span>  <span data-ttu-id="af715-211">Referenciaként az alábbiakban található alapértelmezett **web.config** az alkalmazás által használt **app.js** belépési pontként.</span><span class="sxs-lookup"><span data-stu-id="af715-211">For reference, the below is a default **web.config** for an application that uses **app.js** as the entry point.</span></span>
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    <span data-ttu-id="af715-212">Ha az alkalmazás nem használja a belépési pont **app.js**, le kell cserélnie összes előfordulását **app.js** a megfelelő belépési ponthoz.</span><span class="sxs-lookup"><span data-stu-id="af715-212">If your application uses an entry point other than **app.js**, you must replace all occurrences of **app.js** with the correct entry point.</span></span> <span data-ttu-id="af715-213">Például cseréje **app.js** rendelkező **server.js**.</span><span class="sxs-lookup"><span data-stu-id="af715-213">For example, replacing **app.js** with **server.js**.</span></span>

> [!NOTE]
> <span data-ttu-id="af715-214">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása] oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="af715-214">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="af715-215">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="af715-215">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="af715-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af715-216">Next steps</span></span>
<span data-ttu-id="af715-217">Ez az oktatóanyag megtanulta, hogyan hozhat létre egy Azure-webalkalmazás található Csevegés alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="af715-217">In this tutorial you learned how to create a chat application hosted in an Azure web app.</span></span> <span data-ttu-id="af715-218">Az alkalmazás Azure-Felhőszolgáltatásban is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="af715-218">You can also host this application as an Azure Cloud Service.</span></span> <span data-ttu-id="af715-219">Ehhez útmutatást a, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].</span><span class="sxs-lookup"><span data-stu-id="af715-219">For steps on how to accomplish this, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>

<span data-ttu-id="af715-220">További információ: a [Node.js fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="af715-220">For more information, see also the [Node.js Developer Center].</span></span>

## <a name="whats-changed"></a><span data-ttu-id="af715-221">A változások</span><span class="sxs-lookup"><span data-stu-id="af715-221">What's changed</span></span>
* <span data-ttu-id="af715-222">A módosítás a websites szolgáltatásról az App Service-re váltásról: [Azure App Service és a hatása a meglévő Azure-szolgáltatások].</span><span class="sxs-lookup"><span data-stu-id="af715-222">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services].</span></span>

<!-- URL List -->

<span data-ttu-id="af715-223">[Azure Redis Cache]: /documentation/services/redis-cache/</span><span class="sxs-lookup"><span data-stu-id="af715-223">[Azure Redis Cache]: /documentation/services/redis-cache/</span></span>
<span data-ttu-id="af715-224">[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714</span><span class="sxs-lookup"><span data-stu-id="af715-224">[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714</span></span>
<span data-ttu-id="af715-225">[Web Apps árképzést ismertető oldalra]: http://go.microsoft.com/fwlink/?LinkId=511643</span><span class="sxs-lookup"><span data-stu-id="af715-225">[Web Apps Pricing page]: http://go.microsoft.com/fwlink/?LinkId=511643</span></span>
<span data-ttu-id="af715-226">[Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md</span><span class="sxs-lookup"><span data-stu-id="af715-226">[Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md</span></span>
[Install and Configure the Azure CLI]: ../cli-install-nodejs.md
<span data-ttu-id="af715-227">[Azure App Service és a hatása a meglévő Azure-szolgáltatások]: http://go.microsoft.com/fwlink/?LinkId=529714</span><span class="sxs-lookup"><span data-stu-id="af715-227">[Azure App Service and Its Impact on Existing Azure Services]: http://go.microsoft.com/fwlink/?LinkId=529714</span></span>
<span data-ttu-id="af715-228">[Node.js fejlesztői központ]: /develop/nodejs/</span><span class="sxs-lookup"><span data-stu-id="af715-228">[Node.js Developer Center]: /develop/nodejs/</span></span>
<span data-ttu-id="af715-229">[Az Azure App Service kipróbálása]: https://azure.microsoft.com/try/app-service/</span><span class="sxs-lookup"><span data-stu-id="af715-229">[Try App Service]: https://azure.microsoft.com/try/app-service/</span></span>
<span data-ttu-id="af715-230">[példány kapcsolat az Azure-webhelyek]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/</span><span class="sxs-lookup"><span data-stu-id="af715-230">[Instance Affinity in Azure Web Sites]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/</span></span>
<span data-ttu-id="af715-231">[a gyorsítótár létrehozása az Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md</span><span class="sxs-lookup"><span data-stu-id="af715-231">[Create a cache in Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md</span></span>

<span data-ttu-id="af715-232">[socket.io-redis]: https://github.com/socketio/socket.io-redis</span><span class="sxs-lookup"><span data-stu-id="af715-232">[socket.io-redis]: https://github.com/socketio/socket.io-redis</span></span>
<span data-ttu-id="af715-233">[Socket.IO GitHub-tárházban]: https://github.com/socketio/socket.io</span><span class="sxs-lookup"><span data-stu-id="af715-233">[Socket.IO GitHub repository]: https://github.com/socketio/socket.io</span></span>
<span data-ttu-id="af715-234">[ZIP- vagy GZ archivált kiadás]: https://github.com/socketio/socket.io/releases</span><span class="sxs-lookup"><span data-stu-id="af715-234">[ZIP or GZ archived release]: https://github.com/socketio/socket.io/releases</span></span>

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
