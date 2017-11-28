---
title: "Node.js csevegőalkalmazás Socket.IO segítségével az Azure App Service aaaCreate"
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
ms.openlocfilehash: 3bd7867ccc297dc0a21c7a00cc9db06358877f5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a><span data-ttu-id="d59dc-103">Node.js csevegőalkalmazás létrehozása a Socket.IO segítségével az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="d59dc-103">Create a Node.js chat application with Socket.IO in Azure App Service</span></span>
<span data-ttu-id="d59dc-104">Socket.IO biztosít a valós idejű kommunikációját a node.js-kiszolgáló és az ügyfél websocket elemeket.</span><span class="sxs-lookup"><span data-stu-id="d59dc-104">Socket.IO provides real-time communication between your node.js server and clients using WebSockets.</span></span> <span data-ttu-id="d59dc-105">Tartalék tooother átvitelek (például hosszú lekérdezési), amely a régebbi böngészők is támogatja.</span><span class="sxs-lookup"><span data-stu-id="d59dc-105">It also supports fallback tooother transports (such as long polling) that work with older browsers.</span></span> <span data-ttu-id="d59dc-106">Ez az oktatóanyag végigvezetik egy alapú Socket.IO Csevegés alkalmazásra, amely egy Azure webalkalmazás üzemeltetéséhez, és bemutatják, hogyan tooscale hello használó [Azure Redis Cache].</span><span class="sxs-lookup"><span data-stu-id="d59dc-106">This tutorial will walk you through hosting a Socket.IO based chat application as an Azure web app, and show you how tooscale hello application using [Azure Redis Cache].</span></span> <span data-ttu-id="d59dc-107">A Socket.IO további információkért lásd: <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="d59dc-107">For more information on Socket.IO, see <http://socket.io/>.</span></span>

> [!NOTE]
> <span data-ttu-id="d59dc-108">Ebben a feladatban hello eljárások alkalmazhatók túl[App Service Web Apps]; a Felhőszolgáltatások, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].</span><span class="sxs-lookup"><span data-stu-id="d59dc-108">hello procedures in this task apply too[App Service Web Apps]; for Cloud Services, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>
> 
> 

## <a name="download-hello-chat-example"></a><span data-ttu-id="d59dc-109">Töltse le a hello Csevegés – példa</span><span class="sxs-lookup"><span data-stu-id="d59dc-109">Download hello chat example</span></span>
<span data-ttu-id="d59dc-110">Ebben a projektben használjuk hello Csevegés példa hello [Socket.IO GitHub-tárházban].</span><span class="sxs-lookup"><span data-stu-id="d59dc-110">For this project, we will use hello chat example from hello [Socket.IO GitHub repository].</span></span> <span data-ttu-id="d59dc-111">Hajtsa végre a következő lépéseket toodownload hello példa hello, majd vegye fel azt a korábban létrehozott toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="d59dc-111">Perform hello following steps toodownload hello example and add it toohello project you previously created.</span></span>

1. <span data-ttu-id="d59dc-112">Töltse le a [ZIP- vagy GZ archivált kiadás] hello Socket.IO projekt (a dokumentum a 1.3.5 verziója lett megadva)</span><span class="sxs-lookup"><span data-stu-id="d59dc-112">Download a [ZIP or GZ archived release] of hello Socket.IO project (version 1.3.5 was used for this document)</span></span>
2. <span data-ttu-id="d59dc-113">Bontsa ki a hello archiválási és másolási hello **példák\\Csevegés** directory tooa új helyre.</span><span class="sxs-lookup"><span data-stu-id="d59dc-113">Extract hello archive and copy hello **examples\\chat** directory tooa new location.</span></span> <span data-ttu-id="d59dc-114">Például  **\\csomópont\\Csevegés**.</span><span class="sxs-lookup"><span data-stu-id="d59dc-114">For example, **\\node\\chat**.</span></span>

## <a name="modify-appjs-and-install-modules"></a><span data-ttu-id="d59dc-115">Az App.js fájl módosítása és -modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="d59dc-115">Modify app.js and install modules</span></span>
1. <span data-ttu-id="d59dc-116">Nevezze át a hello **index.js** fájl túl**app.js**.</span><span class="sxs-lookup"><span data-stu-id="d59dc-116">Rename hello **index.js** file too**app.js**.</span></span> <span data-ttu-id="d59dc-117">Ez lehetővé teszi az Azure toodetect, hogy ez a Node.js-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d59dc-117">This allows Azure toodetect that this is a Node.js application.</span></span>
2. <span data-ttu-id="d59dc-118">Nyissa meg hello **app.js** fájlt egy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="d59dc-118">Open hello **app.js** file in a text editor.</span></span> <span data-ttu-id="d59dc-119">Változás hello sort tartalmazó `var io = require('../..')(server);` alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="d59dc-119">Change hello line containing `var io = require('../..')(server);` as shown below:</span></span>
   
       var express = require('express');
       var app = express();
       var server = require('http').createServer(app);
       // var io = require('../..')(server);
       // New:
       var io = require('socket.io')(server);
       var port = process.env.PORT || 3000;
3. <span data-ttu-id="d59dc-120">Nyissa meg hello **package.json** fájlt, és a hivatkozás toosocket.io alatt `dependencies`, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d59dc-120">Open hello **package.json** file and add a reference toosocket.io under `dependencies`, as shown below:</span></span>
   
        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }
4. <span data-ttu-id="d59dc-121">Hello parancssori, módosítsa toohello  **\\csomópont\\Csevegés** könyvtárra, és használja npm tooinstall hello modult az alkalmazás számára szükséges:</span><span class="sxs-lookup"><span data-stu-id="d59dc-121">From hello command-line, change toohello **\\node\\chat** directory and use npm tooinstall hello modules required by this application:</span></span>
   
        npm install
   
    <span data-ttu-id="d59dc-122">Ezzel telepíti hello modulok nevű almappába **node_modules**.</span><span class="sxs-lookup"><span data-stu-id="d59dc-122">This will install hello modules into a subfolder named **node_modules**.</span></span>

## <a name="create-an-azure-web-app"></a><span data-ttu-id="d59dc-123">Azure-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d59dc-123">Create an Azure Web App</span></span>
<span data-ttu-id="d59dc-124">Kövesse ezeket a lépéseket toocreate Azure-webalkalmazás Git-közzététel engedélyezése és majd a hello webalkalmazás WebSocket támogatásának engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d59dc-124">Follow these steps toocreate an Azure web app, enable Git publishing, and then enable WebSocket support for hello web app.</span></span>

> [!NOTE]
> <span data-ttu-id="d59dc-125">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d59dc-125">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="d59dc-126">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="d59dc-126">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d59dc-127">További információkért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="d59dc-127">For details, see <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

1. <span data-ttu-id="d59dc-128">Hello Azure parancssori felület (CLI) telepítése, és csatlakozzon az Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="d59dc-128">Install hello Azure Command-Line Interface (Azure CLI) and connect tooyour Azure subscription.</span></span> <span data-ttu-id="d59dc-129">Lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="d59dc-129">See [Install and Configure hello Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="d59dc-130">Ha ez az első idő beállítása a tárházat az Azure-ban, toocreate bejelentkezési hitelesítő adatokat kell.</span><span class="sxs-lookup"><span data-stu-id="d59dc-130">If this is your first time setting up a repository in Azure, you need toocreate login credentials.</span></span> <span data-ttu-id="d59dc-131">Hello Azure CLI adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="d59dc-131">From hello Azure CLI, enter hello following command:</span></span>
   
        azure site deployment user set [username] [password]
3. <span data-ttu-id="d59dc-132">Módosítsa a toohello  **\\node\chat** használata hello követően és a directory parancs toocreate egy új Azure web app és egy helyi Git-tárházat.</span><span class="sxs-lookup"><span data-stu-id="d59dc-132">Change toohello **\\node\chat** directory and use hello following command toocreate a new Azure web app and a local Git repository.</span></span> <span data-ttu-id="d59dc-133">Ez a parancs is létrehoz egy Git távoli elnevezett "azure".</span><span class="sxs-lookup"><span data-stu-id="d59dc-133">This command also creates a Git remote named 'azure'.</span></span>
   
        azure site create mysitename --git
   
    <span data-ttu-id="d59dc-134">A webalkalmazás egyedi nevű, "mysitename" le kell cserélnie.</span><span class="sxs-lookup"><span data-stu-id="d59dc-134">You must replace 'mysitename' with a unique name for your web app.</span></span>
4. <span data-ttu-id="d59dc-135">Véglegesítse hello meglévő fájlok toohello helyi tárház hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="d59dc-135">Commit hello existing files toohello local repository by using hello following commands:</span></span>
   
        git add .
        git commit -m "Initial commit"
5. <span data-ttu-id="d59dc-136">Leküldéses hello fájlok toohello Azure Web Apps tárház a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d59dc-136">Push hello files toohello Azure Web Apps repository with hello following command:</span></span>
   
        git push azure master
   
    <span data-ttu-id="d59dc-137">Amikor a rendszer kéri, adja meg a hitelesítő adatokat a 2. lépésben.</span><span class="sxs-lookup"><span data-stu-id="d59dc-137">When prompted, enter your credentials from step 2.</span></span> <span data-ttu-id="d59dc-138">Állapotüzenetek kap, a rendszer importálja a hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d59dc-138">You will receive status messages as modules are imported on hello server.</span></span> <span data-ttu-id="d59dc-139">Ez a folyamat befejezése után hello alkalmazás fogja üzemeltetni az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d59dc-139">Once this process has completed, hello application will be hosted on your Azure web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d59dc-140">A modul a telepítés során azt tapasztalhatja hibák, amelyek "hello importált projekt … nem található".</span><span class="sxs-lookup"><span data-stu-id="d59dc-140">During module installation, you may notice errors that 'hello imported project ... was not found'.</span></span> <span data-ttu-id="d59dc-141">Ezek biztonságosan figyelmen kívül hagyható.</span><span class="sxs-lookup"><span data-stu-id="d59dc-141">These can safely be ignored.</span></span>
   > 
   > 
6. <span data-ttu-id="d59dc-142">Socket.IO használ a websocket elemek, amelyek az Azure-on alapértelmezés szerint nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="d59dc-142">Socket.IO uses WebSockets, which are not enabled by default on Azure.</span></span> <span data-ttu-id="d59dc-143">webes szoftvercsatornák tooenable, hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="d59dc-143">tooenable web sockets, use hello following command:</span></span>
   
        azure site set -w
   
    <span data-ttu-id="d59dc-144">Ha a rendszer kéri, adja meg a webalkalmazás hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d59dc-144">If prompted, enter hello name of hello web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d59dc-145">"az azure site set -w" parancs lesz csak verziójával 0.7.4 munkahelyi vagy az Azure parancssori felület hello magasabb hello.</span><span class="sxs-lookup"><span data-stu-id="d59dc-145">hello 'azure site set -w' command will work only with version 0.7.4 or higher of hello Azure Command-Line Interface.</span></span> <span data-ttu-id="d59dc-146">Engedélyezheti a WebSocket-támogatás hello használatával [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d59dc-146">You can also enable WebSocket support using hello [Azure Portal](https://portal.azure.com).</span></span>
   > 
   > <span data-ttu-id="d59dc-147">tooenable websocket elemek használata hello Azure portálon kattintson a hello webalkalmazás hello webalkalmazások panelen, kattintson **összes beállítás** > **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="d59dc-147">tooenable WebSockets using hello Azure Portal, click hello web app from hello Web Apps blade, click **All settings** > **Application settings**.</span></span> <span data-ttu-id="d59dc-148">A **webes szoftvercsatornák**, kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="d59dc-148">Under **Web Sockets**, click **On**.</span></span> <span data-ttu-id="d59dc-149">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d59dc-149">Then click **Save**.</span></span>
   > 
   > 
7. <span data-ttu-id="d59dc-150">tooview hello webalkalmazást az Azure, használjon hello következő toolaunch parancsot a webböngészőt, és keresse meg a toohello futtatott webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="d59dc-150">tooview hello web app on Azure, use hello following command toolaunch your web browser and navigate toohello hosted web app:</span></span>
   
        azure site browse

<span data-ttu-id="d59dc-151">Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="d59dc-151">Your app is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

## <a name="scale-out"></a><span data-ttu-id="d59dc-152">Horizontális felskálázás</span><span class="sxs-lookup"><span data-stu-id="d59dc-152">Scale out</span></span>
<span data-ttu-id="d59dc-153">Socket.IO alkalmazások kiterjeszthető használatával egy **adapter** toodistribute üzenetek és események között több alkalmazáspéldányt.</span><span class="sxs-lookup"><span data-stu-id="d59dc-153">Socket.IO applications can be scaled out by using an **adapter** toodistribute messages and events between multiple application instances.</span></span> <span data-ttu-id="d59dc-154">Nincsenek elérhető több adapterek, amíg hello [socket.io-redis] adapter hello Azure Redis Cache szolgáltatással könnyen használható.</span><span class="sxs-lookup"><span data-stu-id="d59dc-154">While there are several adapters available, hello [socket.io-redis] adapter can be easily used with hello Azure Redis Cache feature.</span></span>

> [!NOTE]
> <span data-ttu-id="d59dc-155">Egy további követelmény a Socket.IO megoldás kiterjesztése a kapcsolódó munkamenetek támogatása.</span><span class="sxs-lookup"><span data-stu-id="d59dc-155">An additional requirement for scaling out a Socket.IO solution is support for sticky sessions.</span></span> <span data-ttu-id="d59dc-156">A kapcsolódó munkamenetek alapértelmezés szerint engedélyezve vannak a Azure Web Apps használatával Azure alkalmazáskérelmek irányítására.</span><span class="sxs-lookup"><span data-stu-id="d59dc-156">Sticky sessions are enabled by default for Azure Web Apps through Azure Request Routing.</span></span> <span data-ttu-id="d59dc-157">További információkért lásd: [példány kapcsolat az Azure-webhelyek].</span><span class="sxs-lookup"><span data-stu-id="d59dc-157">For more information, see [Instance Affinity in Azure Web Sites].</span></span>
> 
> 

### <a name="create-a-redis-cache"></a><span data-ttu-id="d59dc-158">A Redis gyorsítótár létrehozása</span><span class="sxs-lookup"><span data-stu-id="d59dc-158">Create a Redis cache</span></span>
<span data-ttu-id="d59dc-159">A hello lépésekkel [a gyorsítótár létrehozása az Azure Redis Cache] toocreate új gyorsítótárat.</span><span class="sxs-lookup"><span data-stu-id="d59dc-159">Perform hello steps in [Create a cache in Azure Redis Cache] toocreate a new cache.</span></span>

> [!NOTE]
> <span data-ttu-id="d59dc-160">Mentés hello **állomásnév** és **elsődleges kulcs** a gyorsítótárhoz, mivel ezek lesz szükség a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="d59dc-160">Save hello **Host name** and **Primary key** for your cache, as these will be needed in hello next steps.</span></span>
> 
> 

### <a name="add-hello-redis-and-socketio-redis-modules"></a><span data-ttu-id="d59dc-161">Hello redis és socket.io-redis-modulok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d59dc-161">Add hello redis and socket.io-redis modules</span></span>
1. <span data-ttu-id="d59dc-162">A parancssorból, módosítsa a toohello  **\\csomópont\\Csevegés** parancs a következő könyvtárra, és használja hello.</span><span class="sxs-lookup"><span data-stu-id="d59dc-162">From a command-line, change toohello **\\node\\chat** directory and use hello following command.</span></span>
   
        npm install socket.io-redis@0.1.4 redis@0.12.1 --save
   
   > [!NOTE]
   > <span data-ttu-id="d59dc-163">Ebben a parancsban megadott hello verziók Ez a cikk tesztelése során használt hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d59dc-163">hello versions specified in this command are hello versions used when testing this article.</span></span>
   > 
   > 
2. <span data-ttu-id="d59dc-164">Módosítsa a hello **app.js** fájl tooadd hello következő sorok után azonnal`var io = require('socket.io')(server);`</span><span class="sxs-lookup"><span data-stu-id="d59dc-164">Modify hello **app.js** file tooadd hello following lines immediately after `var io = require('socket.io')(server);`</span></span>
   
        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
   
        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));
   
    <span data-ttu-id="d59dc-165">Cserélje le **redishostname** és **rediskey** hello állomásnév és a Redis gyorsítótár kulccsal.</span><span class="sxs-lookup"><span data-stu-id="d59dc-165">Replace **redishostname** and **rediskey** with hello host name and key for your Redis cache.</span></span>
   
    <span data-ttu-id="d59dc-166">A hozzon létre egy közzétételi és előfizetési ügyfél toohello korábban létrehozott Redis gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="d59dc-166">This will create a publish and subscribe client toohello Redis cache created previously.</span></span> <span data-ttu-id="d59dc-167">hello ügyfelek majd használt hello adapter tooconfigure Socket.IO toouse hello Redis gyorsítótár az üzenetek és események továbbításához az alkalmazás példányai között</span><span class="sxs-lookup"><span data-stu-id="d59dc-167">hello clients are then used with hello adapter tooconfigure Socket.IO toouse hello Redis cache for passing messages and events between instances of your application</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d59dc-168">Hello közben **socket.io-redis** adapter közvetlenül tooRedis kommunikálhatnak, hello aktuális verziója nem támogatja az Azure Redis cache-által igényelt hello hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d59dc-168">While hello **socket.io-redis** adapter can communicate directly tooRedis, hello current version does not support hello authentication required by Azure Redis cache.</span></span> <span data-ttu-id="d59dc-169">Így hello hello kezdeti kapcsolat létre **redis** modult, majd hello ügyfél átadása toohello **socket.io-redis** adapter.</span><span class="sxs-lookup"><span data-stu-id="d59dc-169">So hello initial connection is created using hello **redis** module, then hello client is passed toohello **socket.io-redis** adapter.</span></span>
   > 
   > <span data-ttu-id="d59dc-170">Azure Redis Cache-példányokhoz biztonságos 6380 portot használ, miközben ebben a példában használt hello modulok nem támogatja a biztonságos kapcsolatokat 7/14/2014-től.</span><span class="sxs-lookup"><span data-stu-id="d59dc-170">While Azure Redis Cache supports secure connections using port 6380, hello modules used in this example do not support secure connections as of 7/14/2014.</span></span> <span data-ttu-id="d59dc-171">kód fent hello hello alapértelmezés szerint nem biztonságos portja 6379 használja.</span><span class="sxs-lookup"><span data-stu-id="d59dc-171">hello above code uses hello default, unsecure port of 6379.</span></span>
   > 
   > 
3. <span data-ttu-id="d59dc-172">Mentés hello módosított **app.js**</span><span class="sxs-lookup"><span data-stu-id="d59dc-172">Save hello modified **app.js**</span></span>

### <a name="commit-changes-and-redeploy"></a><span data-ttu-id="d59dc-173">Véglegesítse a módosításokat, és telepítse újra</span><span class="sxs-lookup"><span data-stu-id="d59dc-173">Commit changes and redeploy</span></span>
<span data-ttu-id="d59dc-174">A parancssori a hello hello  **\\csomópont\\Csevegés** directory, a következő parancsok toocommit módosítások hello használja, és telepítse újra a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d59dc-174">From hello command-line in hello **\\node\\chat** directory, use hello following commands toocommit changes and redeploy hello application.</span></span>

    git add .
    git commit -m "implementing scale out"
    git push azure master

<span data-ttu-id="d59dc-175">Miután hello módosítások toohello server értesítését, méretezhető a webhely több példánya között hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="d59dc-175">Once hello changes have been pushed toohello server, you can scale your site across multiple instances by using hello following command.</span></span>

    azure site scale instances --instances #

<span data-ttu-id="d59dc-176">Ha  **#**  példányok toocreate hello száma.</span><span class="sxs-lookup"><span data-stu-id="d59dc-176">Where **#** is hello number of instances toocreate.</span></span>

<span data-ttu-id="d59dc-177">Több böngészők vagy számítógépek tooverify küldött üzenetek megfelelően tooall ügyfelek tooyour webalkalmazás lehet csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="d59dc-177">You can connect tooyour web app from multiple browsers or computers tooverify that messages are correctly sent tooall clients.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="d59dc-178">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="d59dc-178">Troubleshooting</span></span>
### <a name="connection-limits"></a><span data-ttu-id="d59dc-179">Kapcsolat korlátai</span><span class="sxs-lookup"><span data-stu-id="d59dc-179">Connection limits</span></span>
<span data-ttu-id="d59dc-180">Az Azure Web Apps több SKU, hello erőforrások elérhető tooyour hely meghatározó érhető el.</span><span class="sxs-lookup"><span data-stu-id="d59dc-180">Azure Web Apps is available in multiple SKUs, which determine hello resources available tooyour site.</span></span> <span data-ttu-id="d59dc-181">Ez magában foglalja a hello engedélyezett WebSocket-kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="d59dc-181">This includes hello number of allowed WebSocket connections.</span></span> <span data-ttu-id="d59dc-182">További információkért lásd: hello [Web Apps árképzést ismertető oldalra].</span><span class="sxs-lookup"><span data-stu-id="d59dc-182">For more information, see hello [Web Apps Pricing page].</span></span>

### <a name="messages-arent-being-sent-using-websockets"></a><span data-ttu-id="d59dc-183">Üzenetek nem küldött websocket elemek használatával</span><span class="sxs-lookup"><span data-stu-id="d59dc-183">Messages aren't being sent using WebSockets</span></span>
<span data-ttu-id="d59dc-184">Ügyfélböngészők tartsa visszaváltás toolong lekérdezési websocket elemek használata helyett, ha miatt hello a következők egyike lehet.</span><span class="sxs-lookup"><span data-stu-id="d59dc-184">If client browsers keep falling back toolong polling instead of using WebSockets, it may be because of one of hello following.</span></span>

* <span data-ttu-id="d59dc-185">**Próbálja a hello átviteli toojust websocket elemek korlátozása**</span><span class="sxs-lookup"><span data-stu-id="d59dc-185">**Try limiting hello transport toojust WebSockets**</span></span>
  
    <span data-ttu-id="d59dc-186">Ahhoz, hogy Socket.IO toouse, átviteli üzenetküldési hello websocket elemeket hello kiszolgáló és az ügyfél támogatnia kell websocket elemeket.</span><span class="sxs-lookup"><span data-stu-id="d59dc-186">In order for Socket.IO toouse WebSockets as hello messaging transport, both hello server and client must support WebSockets.</span></span> <span data-ttu-id="d59dc-187">Ha egy vagy többi hello nem, a Socket.IO egyezteti egy másik átviteltől, pl. hosszú lekérdezési.</span><span class="sxs-lookup"><span data-stu-id="d59dc-187">If one or hello other does not, Socket.IO will negotiate another transport, such as long polling.</span></span> <span data-ttu-id="d59dc-188">átvitelek Socket.IO által használt alapértelmezett listája hello ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span><span class="sxs-lookup"><span data-stu-id="d59dc-188">hello default list of transports used by Socket.IO is ` websocket, htmlfile, xhr-polling, jsonp-polling`.</span></span> <span data-ttu-id="d59dc-189">Kényszerítheti tooonly használata szoftvercsatornák használatával a következő kód toohello hello hozzáadásával **app.js** fájl után hello sort tartalmazó `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="d59dc-189">You can force it tooonly use WebSockets by adding hello following code toohello **app.js** file, after hello line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('transports', ['websocket']);
        });
  
  > [!NOTE]
  > <span data-ttu-id="d59dc-190">Megjegyzés: régebbi websocket elemek nem támogató böngészők nem fog tudni tooconnect toohello hely pedig kód fent hello aktív, csak a kommunikációs tooWebSockets korlátozza.</span><span class="sxs-lookup"><span data-stu-id="d59dc-190">Note that older browsers that do not support WebSockets will not be able tooconnect toohello site while hello above code is active, as it restricts communication tooWebSockets only.</span></span>
  > 
  > 
* <span data-ttu-id="d59dc-191">**SSL használata**</span><span class="sxs-lookup"><span data-stu-id="d59dc-191">**Use SSL**</span></span>
  
    <span data-ttu-id="d59dc-192">Néhány kisebb használt HTTP-fejlécek, például a hello támaszkodik websocket elemek **frissítése** fejléc.</span><span class="sxs-lookup"><span data-stu-id="d59dc-192">WebSockets relies on some lesser used HTTP headers, such as hello **Upgrade** header.</span></span> <span data-ttu-id="d59dc-193">Néhány, a közbenső hálózati eszközök, például webes proxykat, ezek a fejlécek távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="d59dc-193">Some intermediate network devices, such as web proxies, may remove these headers.</span></span> <span data-ttu-id="d59dc-194">tooavoid probléma létrehozhat hello WebSocket-kapcsolat SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="d59dc-194">tooavoid this problem, you can establish hello WebSocket connection over SSL.</span></span>
  
    <span data-ttu-id="d59dc-195">Egy egyszerű módot tooaccomplish Ez az tooconfigure Socket.IO túl`match origin protocol`.</span><span class="sxs-lookup"><span data-stu-id="d59dc-195">An easy way tooaccomplish this is tooconfigure Socket.IO too`match origin protocol`.</span></span> <span data-ttu-id="d59dc-196">Ez arra utasítja a Socket.IO toosecure websocket elemek kommunikációs hello ugyanaz, mint a HTTP/HTTPS-címekről hello kérelem hello weblap.</span><span class="sxs-lookup"><span data-stu-id="d59dc-196">This instructs Socket.IO toosecure WebSockets communication hello same as hello originating HTTP/HTTPS request for hello web page.</span></span> <span data-ttu-id="d59dc-197">Ha egy böngészőt használ egy HTTPS URL-cím toovisit a webhelyet, a további WebSocket kommunikáció Socket.IO segítségével megfelelően történik SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="d59dc-197">If a browser uses an HTTPS URL toovisit your website, subsequent WebSocket communications through Socket.IO will be secured over SSL.</span></span>
  
    <span data-ttu-id="d59dc-198">toomodify a példa tooenable ebben a konfigurációban adja hozzá a következő kód toohello hello **app.js** után hello sor tartalmazó fájl `, nicknames = {};`.</span><span class="sxs-lookup"><span data-stu-id="d59dc-198">toomodify this example tooenable this configuration, add hello following code toohello **app.js** file after hello line containing `, nicknames = {};`.</span></span>
  
        io.configure(function() {
          io.set('match origin protocol', true);
        });
* <span data-ttu-id="d59dc-199">**Ellenőrizze a web.config beállításait**</span><span class="sxs-lookup"><span data-stu-id="d59dc-199">**Verify web.config settings**</span></span>
  
    <span data-ttu-id="d59dc-200">Node.js-alkalmazásokat az Azure web apps használata hello **web.config** fájl tooroute bejövő kérelmek toohello Node.js-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d59dc-200">Azure web apps that host Node.js applications use hello **web.config** file tooroute incoming requests toohello Node.js application.</span></span> <span data-ttu-id="d59dc-201">Node.js-alkalmazások és helyesen toofunction websocket elemeket, hello **web.config** bejegyzés a következő hello tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="d59dc-201">For WebSockets toofunction correctly with Node.js applications, hello **web.config** must contain hello following entry.</span></span>
  
        <webSocket enabled="false"/>
  
    <span data-ttu-id="d59dc-202">Ez letiltja az hello IIS websocket elemek modult, amely magában foglalja a saját végrehajtásának websocket elemek és a Node.js adott WebSocket modulok például Socket.IO ütközik.</span><span class="sxs-lookup"><span data-stu-id="d59dc-202">This disables hello IIS WebSockets module, which includes its own implementation of WebSockets and conflicts with Node.js specific WebSocket modules such as Socket.IO.</span></span> <span data-ttu-id="d59dc-203">Ha a sor nem észlelhető, vagy túl van beállítva`true`, ennek az lehet, hogy az alkalmazás nem működik a hello WebSocket átviteli hello OK.</span><span class="sxs-lookup"><span data-stu-id="d59dc-203">If this line is not present, or is set too`true`, this may be hello reason that hello WebSocket transport is not working for your application.</span></span>
  
    <span data-ttu-id="d59dc-204">Általában a Node.js-alkalmazások nem tartalmaznak egy **web.config** fájlt, így Azure Websitesra automatikusan létrehoz egy Node.js-alkalmazások a rendszer telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="d59dc-204">Normally, Node.js applications do not include a **web.config** file, so Azure Websites will automatically generate one for Node.js applications when they are deployed.</span></span> <span data-ttu-id="d59dc-205">Ez a fájl automatikusan létrehozott hello kiszolgálón, mert kell használnia hello FTP vagy FTPS URL-CÍMÉT a webhely tooview az ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="d59dc-205">Since this file is automatically generated on hello server, you must use hello FTP or FTPS URL for your website tooview this file.</span></span> <span data-ttu-id="d59dc-206">Hello FTP és FTPS URL-címek a hely az hello klasszikus portálon található; ehhez válassza a webes alkalmazás, és majd hello **irányítópult** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d59dc-206">You can find hello FTP and FTPS URLs for your site in hello classic portal by selecting your web app, and then hello **Dashboard** link.</span></span> <span data-ttu-id="d59dc-207">hello URL-címek jelennek meg hello **gyors áttekintő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d59dc-207">hello URLs are displayed in hello **quick glance** section.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="d59dc-208">Hello **web.config** csak létre fájlt Azure Websitesra Ha az alkalmazás nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="d59dc-208">hello **web.config** file is only generated by Azure Websites if your application does not provide one.</span></span> <span data-ttu-id="d59dc-209">Ha megad egy **web.config** fájl hello legfelső szintű a projektet, azt által használt Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d59dc-209">If you provide a **web.config** file in hello root of your application project, it will be used by Azure Web Apps.</span></span>
  > 
  > 
  
    <span data-ttu-id="d59dc-210">Ha hello bejegyzése nincs jelen, vagy tooa értékének beállítása `true`, akkor létre kell hoznia egy **web.config** a hello a Node.js-alkalmazás gyökérkönyvtárában, és adjon meg egy értéket a `false`.</span><span class="sxs-lookup"><span data-stu-id="d59dc-210">If hello entry is not present, or is set tooa value of `true`, then you should create a **web.config** in hello root of your Node.js application and specify a value of `false`.</span></span>  <span data-ttu-id="d59dc-211">Összehasonlításul, az alábbi hello egy alapértelmezett **web.config** az alkalmazás által használt **app.js** hello belépési pontként.</span><span class="sxs-lookup"><span data-stu-id="d59dc-211">For reference, hello below is a default **web.config** for an application that uses **app.js** as hello entry point.</span></span>
  
        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used toorun node processes behind
             IIS or IIS Express.  For more information, visit:
  
             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->
  
        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that hello server.js file is a node.js web app toobe handled by hello iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>
  
                <!-- First we consider whether hello incoming URL matches a physical file in hello /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>
  
                <!-- All other URLs are mapped toohello node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using hello following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes toorestart hello server
                * node_env: will be propagated toonode as NODE_ENV environment variable
                * debuggingEnabled - controls whether hello built-in debugger is enabled
  
              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>
  
    <span data-ttu-id="d59dc-212">Ha az alkalmazás nem használja a belépési pont **app.js**, le kell cserélnie összes előfordulását **app.js** hello megfelelő belépési pont.</span><span class="sxs-lookup"><span data-stu-id="d59dc-212">If your application uses an entry point other than **app.js**, you must replace all occurrences of **app.js** with hello correct entry point.</span></span> <span data-ttu-id="d59dc-213">Például cseréje **app.js** rendelkező **server.js**.</span><span class="sxs-lookup"><span data-stu-id="d59dc-213">For example, replacing **app.js** with **server.js**.</span></span>

> [!NOTE]
> <span data-ttu-id="d59dc-214">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása], ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="d59dc-214">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d59dc-215">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="d59dc-215">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d59dc-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d59dc-216">Next steps</span></span>
<span data-ttu-id="d59dc-217">Ebben az oktatóanyagban megtanulta, hogyan toocreate egy csevegőalkalmazás tárolt Azure-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d59dc-217">In this tutorial you learned how toocreate a chat application hosted in an Azure web app.</span></span> <span data-ttu-id="d59dc-218">Az alkalmazás Azure-Felhőszolgáltatásban is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="d59dc-218">You can also host this application as an Azure Cloud Service.</span></span> <span data-ttu-id="d59dc-219">Útmutatást a tooaccomplish a, lásd: [Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban].</span><span class="sxs-lookup"><span data-stu-id="d59dc-219">For steps on how tooaccomplish this, see [Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service].</span></span>

<span data-ttu-id="d59dc-220">További információkért lásd még: hello [Node.js fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="d59dc-220">For more information, see also hello [Node.js Developer Center].</span></span>

## <a name="whats-changed"></a><span data-ttu-id="d59dc-221">A változások</span><span class="sxs-lookup"><span data-stu-id="d59dc-221">What's changed</span></span>
* <span data-ttu-id="d59dc-222">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások].</span><span class="sxs-lookup"><span data-stu-id="d59dc-222">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services].</span></span>

<!-- URL List -->

[Azure Redis Cache]: /documentation/services/redis-cache/
[App Service Web Apps]: http://go.microsoft.com/fwlink/?LinkId=529714
[Web Apps árképzést ismertető oldalra]: http://go.microsoft.com/fwlink/?LinkId=511643
[Socket.IO segítségével egy Node.js Csevegés alkalmazás létrehozása az Azure-Felhőszolgáltatásban]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure hello Azure CLI]: ../cli-install-nodejs.md
[Azure App Service és a hatása a meglévő Azure-szolgáltatások]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js fejlesztői központ]: /develop/nodejs/
[App Service kipróbálása]: https://azure.microsoft.com/try/app-service/
[példány kapcsolat az Azure-webhelyek]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[a gyorsítótár létrehozása az Azure Redis Cache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[socket.io-redis]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub-tárházban]: https://github.com/socketio/socket.io
[ZIP- vagy GZ archivált kiadás]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
