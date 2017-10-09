---
title: "Socket.io segítségével aaaNode.js alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse socket.io node.js-alkalmazásokban az Azure-platformon futó."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 7f9435e0-7732-4aa1-a4df-ea0e894b847f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 47c6c4a748938959315b880340f41f31faab4ea9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a><span data-ttu-id="13978-103">A Node.js csevegőalkalmazás Socket.IO létrehozása az Azure-Felhőszolgáltatás-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="13978-103">Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service</span></span>
<span data-ttu-id="13978-104">Socket.IO biztosít a valós idejű között a node.js-kiszolgáló és az ügyfelek közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="13978-104">Socket.IO provides realtime communication between between your node.js server and clients.</span></span> <span data-ttu-id="13978-105">Ez az oktatóanyag végigvezeti egy szoftvercsatorna üzemeltetéséhez. IO Csevegés alkalmazás Azure-alapú.</span><span class="sxs-lookup"><span data-stu-id="13978-105">This tutorial will walk you through hosting a socket.IO based chat application on Azure.</span></span> <span data-ttu-id="13978-106">A Socket.IO további információkért lásd: <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="13978-106">For more information on Socket.IO, see <http://socket.io/>.</span></span>

<span data-ttu-id="13978-107">A képernyőfelvétel a hello befejeződött alkalmazás alatt van:</span><span class="sxs-lookup"><span data-stu-id="13978-107">A screenshot of hello completed application is below:</span></span>

![Az Azure-on tárolt hello szolgáltatás megjelenítő böngészőablak][completed-app]  

## <a name="prerequisites"></a><span data-ttu-id="13978-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13978-109">Prerequisites</span></span>
<span data-ttu-id="13978-110">Győződjön meg arról, hogy a következő termékek hello és verziók telepített toosuccessfully teljes hello példa ebben a cikkben az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="13978-110">Ensure that hello following products and versions are installed toosuccessfully complete hello example in this article:</span></span>

* <span data-ttu-id="13978-111">A [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) telepítése</span><span class="sxs-lookup"><span data-stu-id="13978-111">Install [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span></span>
* <span data-ttu-id="13978-112">[Node.js](https://nodejs.org/download/) telepítése</span><span class="sxs-lookup"><span data-stu-id="13978-112">Install [Node.js](https://nodejs.org/download/)</span></span>
* <span data-ttu-id="13978-113">Telepítés [Python 2.7.10 verziója](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="13978-113">Install [Python version 2.7.10](https://www.python.org/)</span></span>

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="13978-114">Egy Felhőszolgáltatás-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="13978-114">Create a Cloud Service Project</span></span>
<span data-ttu-id="13978-115">hello lépések létrehozása hello felhőszolgáltatás-projekt, amely hello Socket.IO alkalmazás fogja futtatni.</span><span class="sxs-lookup"><span data-stu-id="13978-115">hello following steps create hello cloud service project that will host hello Socket.IO application.</span></span>

1. <span data-ttu-id="13978-116">A hello **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="13978-116">From hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="13978-117">Végül kattintson a jobb gombbal **Windows PowerShell** válassza **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="13978-117">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Az Azure PowerShell ikon][powershell-menu]
2. <span data-ttu-id="13978-119">Hozzon létre egy könyvtárat nevű **c:\\csomópont**.</span><span class="sxs-lookup"><span data-stu-id="13978-119">Create a directory called **c:\\node**.</span></span> 
   
        PS C:\> md node
3. <span data-ttu-id="13978-120">Módosítsa a könyvtárakat toohello **c:\\csomópont** könyvtár</span><span class="sxs-lookup"><span data-stu-id="13978-120">Change directories toohello **c:\\node** directory</span></span>
   
        PS C:\> cd node
4. <span data-ttu-id="13978-121">Adja meg a következő parancsok toocreate nevű új megoldás hello **chatapp** és a feldolgozói szerepkör nevű **WorkerRole1**:</span><span class="sxs-lookup"><span data-stu-id="13978-121">Enter hello following commands toocreate a new solution named **chatapp** and a worker role named **WorkerRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    <span data-ttu-id="13978-122">Válasz a következő hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="13978-122">You will see hello following response:</span></span>
   
    ![hello kimeneti hello új-azureservice és azurenodeworkerrolecmdlets hozzáadása](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-hello-chat-example"></a><span data-ttu-id="13978-124">Töltse le a hello Csevegés – példa</span><span class="sxs-lookup"><span data-stu-id="13978-124">Download hello Chat Example</span></span>
<span data-ttu-id="13978-125">Ebben a projektben használjuk hello Csevegés példa hello [Socket.IO GitHub-tárházban].</span><span class="sxs-lookup"><span data-stu-id="13978-125">For this project, we will use hello chat example from hello [Socket.IO GitHub repository].</span></span> <span data-ttu-id="13978-126">Hajtsa végre a következő lépéseket toodownload hello példa hello, majd vegye fel azt a korábban létrehozott toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="13978-126">Perform hello following steps toodownload hello example and add it toohello project you previously created.</span></span>

1. <span data-ttu-id="13978-127">Hozzon létre egy helyi példány hello tárház hello segítségével **Klónozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="13978-127">Create a local copy of hello repository by using hello **Clone** button.</span></span> <span data-ttu-id="13978-128">Is használhatja hello **ZIP-** gomb toodownload hello projekt.</span><span class="sxs-lookup"><span data-stu-id="13978-128">You may also use hello **ZIP** button toodownload hello project.</span></span>
   
   ![Egy böngészőablakban https://github.com/LearnBoost/socket.io/tree/master/examples/chat, megtekintését hello ZIP letöltési ikonja kiemelve][chat-example-view]
2. <span data-ttu-id="13978-130">Keresse meg a hello könyvtárstruktúrát hello helyi tárház, amíg megérkezik a hello **példák\\Csevegés** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="13978-130">Navigate hello directory structure of hello local repository until you arrive at hello **examples\\chat** directory.</span></span> <span data-ttu-id="13978-131">A könyvtár toothe hello tartalom másolása **C:\\csomópont\\chatapp\\WorkerRole1** korábban létrehozott könyvtár.</span><span class="sxs-lookup"><span data-stu-id="13978-131">Copy hello contents of this directory toothe **C:\\node\\chatapp\\WorkerRole1** directory created earlier.</span></span>
   
   ![Explorer hello példák hello tartalmát megjelenítő\\hello archív kinyert Csevegés könyvtár][chat-contents]
   
   <span data-ttu-id="13978-133">hello hello fenti képernyőfelvételen kiemelt elemeinek olyan hello átmásolva hello fájlok **példák\\Csevegés** könyvtár</span><span class="sxs-lookup"><span data-stu-id="13978-133">hello highlighted items in hello screenshot above are hello files copied from hello **examples\\chat** directory</span></span>
3. <span data-ttu-id="13978-134">A hello **C:\\csomópont\\chatapp\\WorkerRole1** directory, a delete hello **server.js** fájlt, és nevezze át a hello **app.js**fájl túl**server.js**.</span><span class="sxs-lookup"><span data-stu-id="13978-134">In hello **C:\\node\\chatapp\\WorkerRole1** directory, delete hello **server.js** file, and then rename hello **app.js** file too**server.js**.</span></span> <span data-ttu-id="13978-135">Ez eltávolítja az hello alapértelmezett **server.js** hello által korábban létrehozott fájl **Add-AzureNodeWorkerRole** parancsmag és a hello alkalmazás fájlként cserél hello Csevegés példa.</span><span class="sxs-lookup"><span data-stu-id="13978-135">This removes hello default **server.js** file created previously by hello **Add-AzureNodeWorkerRole** cmdlet and replaces it with hello application file from hello chat example.</span></span>

### <a name="modify-serverjs-and-install-modules"></a><span data-ttu-id="13978-136">Módosítsa a Server.js és -modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="13978-136">Modify Server.js and Install Modules</span></span>
<span data-ttu-id="13978-137">Tesztelési hello alkalmazás hello Azure emulátorban, mielőtt néhány kisebb módosításokat kell azt.</span><span class="sxs-lookup"><span data-stu-id="13978-137">Before testing hello application in hello Azure emulator, we must make some minor modifications.</span></span> <span data-ttu-id="13978-138">Hajtsa végre a következő lépéseket toothe server.js fájl hello:</span><span class="sxs-lookup"><span data-stu-id="13978-138">Perform hello following steps toothe server.js file:</span></span>

1. <span data-ttu-id="13978-139">Nyissa meg hello **server.js** fájlt a Visual Studio vagy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="13978-139">Open hello **server.js** file in Visual Studio or any text editor.</span></span>
2. <span data-ttu-id="13978-140">Hello található **modul függőségek** server.js hello elején szakaszt, és módosítsa a hello sort tartalmazó **sio = require('.. //.. lib//Socket.IO ")** túl**sio = require('socket.io')** alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="13978-140">Find hello **Module dependencies** section at hello beginning of server.js and change hello line containing **sio = require('..//..//lib//socket.io')** too**sio = require('socket.io')** as shown below:</span></span>
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. <span data-ttu-id="13978-141">tooensure hello alkalmazás hello megfelelő porton figyel, nyissa meg a server.js a Jegyzettömb vagy a kedvenc szerkesztő, és módosítsa a következő sor tartományvezérlőkkel történő lecserélésével **3000** rendelkező **process.env.port** látható alább:</span><span class="sxs-lookup"><span data-stu-id="13978-141">tooensure hello application listens on hello correct port, open server.js in Notepad or your favorite editor, and then change the following line by replacing **3000** with **process.env.port** as shown below:</span></span>
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

<span data-ttu-id="13978-142">Túl hello módosítások mentése után**server.js**, hello lépések segítségével telepítse a szükséges modulokat, és tesztelje az Azure emulátorban hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="13978-142">After saving hello changes too**server.js**, use hello following steps to install required modules, and then test hello application in the Azure emulator:</span></span>

1. <span data-ttu-id="13978-143">Használatával **Azure PowerShell**, módosítsa a könyvtárakat toohello **C:\\csomópont\\chatapp\\WorkerRole1** könyvtárra, és használja, a következő parancs tooinstall hello hello Ez az alkalmazás által igényelt modulok:</span><span class="sxs-lookup"><span data-stu-id="13978-143">Using **Azure PowerShell**, change directories toohello **C:\\node\\chatapp\\WorkerRole1** directory and use hello following command tooinstall hello modules required by this application:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   <span data-ttu-id="13978-144">Ez a hello package.json fájlban felsorolt hello modulok telepíti.</span><span class="sxs-lookup"><span data-stu-id="13978-144">This will install hello modules listed in hello package.json file.</span></span> <span data-ttu-id="13978-145">Hello parancs után kimeneti hasonló toothe következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="13978-145">After hello command completes, you should see output similar toothe following:</span></span>
   
   ![hello npm hello kimenete telepítési parancs][The-output-of-the-npm-install-command]
2. <span data-ttu-id="13978-147">Mivel ez a példa eredetileg hello Socket.IO GitHub-tárházban része volt, és közvetlenül a relatív elérési útja által hivatkozott hello Socket.IO könyvtár, Socket.IO nem mutat hivatkozás hello package.json fájlban, ezért azt telepítenie kell a következő parancs hello kiállításával:</span><span class="sxs-lookup"><span data-stu-id="13978-147">Since this example was originally a part of hello Socket.IO GitHub repository, and directly referenced hello Socket.IO library by relative path, Socket.IO was not referenced in hello package.json file, so we must install it by issuing hello following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a><span data-ttu-id="13978-148">Tesztelése és telepítése</span><span class="sxs-lookup"><span data-stu-id="13978-148">Test and Deploy</span></span>
1. <span data-ttu-id="13978-149">Indítsa el a hello emulátor hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="13978-149">Launch hello emulator by issuing hello following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > <span data-ttu-id="13978-150">Ha problémák lépnek fel az emulátor, pl. indítása.: Start-AzureEmulator: nem várt hiba történt.</span><span class="sxs-lookup"><span data-stu-id="13978-150">If you encounter issues with launching emulator, eg.: Start-AzureEmulator : An unexpected failure occurred.</span></span>  <span data-ttu-id="13978-151">Részletek: Észlelt egy nem várt hiba hello kommunikációs objektum System.ServiceModel.Channels.ServiceChannel, nem használható kommunikációhoz mivel hello Faulted állapotban van.</span><span class="sxs-lookup"><span data-stu-id="13978-151">Details: Encountered an unexpected error hello communication object,  System.ServiceModel.Channels.ServiceChannel, cannot be used for communication because it is in hello Faulted state.</span></span>
   
      <span data-ttu-id="13978-152">Telepítse újra a AzureAuthoringTools v 2.7.1-es és AzureComputeEmulator v 2.7 – győződjön meg arról, hogy verziója megegyezik.</span><span class="sxs-lookup"><span data-stu-id="13978-152">reinstall AzureAuthoringTools v 2.7.1 and AzureComputeEmulator v 2.7 - make sure that version matches.</span></span>
   >
   >


2. <span data-ttu-id="13978-153">Nyisson meg egy böngészőt, és keresse meg a túl**http://127.0.0.1**.</span><span class="sxs-lookup"><span data-stu-id="13978-153">Open a browser and navigate too**http://127.0.0.1**.</span></span>
3. <span data-ttu-id="13978-154">Hello böngészőablakban nyílik meg, amikor becenév adja meg, és nyomja le írja be.</span><span class="sxs-lookup"><span data-stu-id="13978-154">When hello browser window opens, enter a nickname and then hit enter.</span></span>
   <span data-ttu-id="13978-155">Ez lehetővé teszi toopost üzenetek, egy adott becenév.</span><span class="sxs-lookup"><span data-stu-id="13978-155">This will allow you toopost messages as a specific nickname.</span></span> <span data-ttu-id="13978-156">tootest többfelhasználós funkcióit, nyissa meg az azonos URL-cím segítségével további böngészőablakot, és adja meg a különböző becenevet.</span><span class="sxs-lookup"><span data-stu-id="13978-156">tootest multi-user functionality, open additional browser windows using the same URL and enter different nicknames.</span></span>
   
   ![Két böngészőablakot felhasználó1, mind a Felhasználó2 üzeneteit megjelenítése](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. <span data-ttu-id="13978-158">Tesztelési hello alkalmazása után állítsa le a hello emulátor a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="13978-158">After testing hello application, stop hello emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. <span data-ttu-id="13978-159">toodeploy hello alkalmazás tooAzure, használja a **Publish-AzureServiceProject** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="13978-159">toodeploy hello application tooAzure, use the **Publish-AzureServiceProject** cmdlet.</span></span> <span data-ttu-id="13978-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="13978-160">For example:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > <span data-ttu-id="13978-161">Lehet, hogy toouse egy egyedi nevet, ellenkező esetben hello közzétételi folyamat meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="13978-161">Be sure toouse a unique name, otherwise hello publish process will fail.</span></span> <span data-ttu-id="13978-162">Hello központi telepítés befejezése után hello böngésző megnyitásához, és keresse meg a központilag telepített toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="13978-162">After hello deployment has completed, hello browser will open and navigate toohello deployed service.</span></span>
   > 
   > <span data-ttu-id="13978-163">Ha egy adott hello megadott előfizetés neve nem létezik az importált hello hiba szerint közzétételi profil, töltse le és hello a közzétételi profil importálásához tooAzure telepítése előtt az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="13978-163">If you receive an error stating that hello provided subscription name doesn't exist in hello imported publish profile, you must download and import hello publishing profile for your subscription before deploying tooAzure.</span></span> <span data-ttu-id="13978-164">Lásd: hello **hello alkalmazás tooAzure telepítése** szakasza [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="13978-164">See hello **Deploying hello Application tooAzure** section of [Build and deploy a Node.js application tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 
   
   ![Az Azure-on tárolt hello szolgáltatás megjelenítő böngészőablak][completed-app]
   
   > [!NOTE]
   > <span data-ttu-id="13978-166">Ha egy adott hello megadott előfizetés neve nem létezik az importált hello hiba szerint közzétételi profil, töltse le és hello a közzétételi profil importálásához tooAzure telepítése előtt az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="13978-166">If you receive an error stating that hello provided subscription name doesn't exist in hello imported publish profile, you must download and import hello publishing profile for your subscription before deploying tooAzure.</span></span> <span data-ttu-id="13978-167">Lásd: hello **hello alkalmazás tooAzure telepítése** szakasza [és üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="13978-167">See hello **Deploying hello Application tooAzure** section of [Build and deploy a Node.js application tooan Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 

<span data-ttu-id="13978-168">Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="13978-168">Your application is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

> [!NOTE]
> <span data-ttu-id="13978-169">Az egyszerűség, ez a minta felhasználók csatlakoztatott toohello közötti korlátozott toochatting ugyanezen példányában.</span><span class="sxs-lookup"><span data-stu-id="13978-169">For simplicity, this sample is limited toochatting between users connected toohello same instance.</span></span> <span data-ttu-id="13978-170">Ez azt jelenti, hogy hello felhőszolgáltatás két szerepkör feldolgozópéldányok hoz létre, ha a felhasználók csak tudják másokkal toochat csatlakoztatott toohello azonos feldolgozói szerepkör példánya.</span><span class="sxs-lookup"><span data-stu-id="13978-170">This means that if hello cloud service creates two worker role instances, users will only be able toochat with others connected toohello same worker role instance.</span></span> <span data-ttu-id="13978-171">tooscale hello alkalmazás toowork több szerepkör osztályt, használhat olyan technológia például a Service Bus tooshare hello Socket.IO tároló állapota példányok között.</span><span class="sxs-lookup"><span data-stu-id="13978-171">tooscale hello application toowork with multiple role instances, you could use a technology like Service Bus tooshare hello Socket.IO store state across instances.</span></span> <span data-ttu-id="13978-172">Tekintse meg a hello Service Bus-üzenetsorok és témakörök használati minták hello [Azure SDK for Node.js GitHub-tárházban](https://github.com/WindowsAzure/azure-sdk-for-node).</span><span class="sxs-lookup"><span data-stu-id="13978-172">For examples, see hello Service Bus Queues and Topics usage samples in hello [Azure SDK for Node.js GitHub repository](https://github.com/WindowsAzure/azure-sdk-for-node).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="13978-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13978-173">Next steps</span></span>
<span data-ttu-id="13978-174">Ebben az oktatóanyagban megtanulta, hogyan toocreate alapvető csevegőalkalmazás tárolt Azure-Felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="13978-174">In this tutorial you learned how toocreate a basic chat application hosted in an Azure Cloud Service.</span></span> <span data-ttu-id="13978-175">toolearn hogyan toohost az Azure-webhely, alkalmazás: [összeállíthat egy Node.js Csevegés alkalmazást a Socket.IO segítségével az Azure webhelyén található][chatwebsite].</span><span class="sxs-lookup"><span data-stu-id="13978-175">toolearn how toohost this application in an Azure Website, see [Build a Node.js Chat Application with Socket.IO on an Azure Web Site][chatwebsite].</span></span>

<span data-ttu-id="13978-176">További információkért lásd még: hello [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="13978-176">For more information, see also hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub-tárházban]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting hello Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[hello output of hello Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


