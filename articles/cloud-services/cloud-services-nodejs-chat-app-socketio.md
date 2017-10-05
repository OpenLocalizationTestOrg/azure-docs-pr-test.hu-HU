---
title: "NODE.js-alkalmazás Socket.io segítségével |} Microsoft Docs"
description: "Ismerje meg, hogyan socket.io használható az Azure-platformon futó node.js-alkalmazás."
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
ms.openlocfilehash: a85d4348a13b79b5b7542362de9956aa3398375a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a><span data-ttu-id="aabba-103">A Node.js csevegőalkalmazás Socket.IO létrehozása az Azure-Felhőszolgáltatás-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="aabba-103">Build a Node.js Chat Application with Socket.IO on an Azure Cloud Service</span></span>
<span data-ttu-id="aabba-104">Socket.IO biztosít a valós idejű között a node.js-kiszolgáló és az ügyfelek közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="aabba-104">Socket.IO provides realtime communication between between your node.js server and clients.</span></span> <span data-ttu-id="aabba-105">Ez az oktatóanyag végigvezeti egy szoftvercsatorna üzemeltetéséhez. IO Csevegés alkalmazás Azure-alapú.</span><span class="sxs-lookup"><span data-stu-id="aabba-105">This tutorial will walk you through hosting a socket.IO based chat application on Azure.</span></span> <span data-ttu-id="aabba-106">A Socket.IO további információkért lásd: <http://socket.io/>.</span><span class="sxs-lookup"><span data-stu-id="aabba-106">For more information on Socket.IO, see <http://socket.io/>.</span></span>

<span data-ttu-id="aabba-107">A kész alkalmazás képernyőfelvételének alatt van:</span><span class="sxs-lookup"><span data-stu-id="aabba-107">A screenshot of the completed application is below:</span></span>

![Az Azure-platformon futó szolgáltatás megjelenítő böngészőablak][completed-app]  

## <a name="prerequisites"></a><span data-ttu-id="aabba-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="aabba-109">Prerequisites</span></span>
<span data-ttu-id="aabba-110">Győződjön meg arról, hogy a következő termékek és termékverziók telepítve sikeres végrehajtásához az ebben a cikkben példa:</span><span class="sxs-lookup"><span data-stu-id="aabba-110">Ensure that the following products and versions are installed to successfully complete the example in this article:</span></span>

* <span data-ttu-id="aabba-111">A [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) telepítése</span><span class="sxs-lookup"><span data-stu-id="aabba-111">Install [Visual Studio](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)</span></span>
* <span data-ttu-id="aabba-112">[Node.js](https://nodejs.org/download/) telepítése</span><span class="sxs-lookup"><span data-stu-id="aabba-112">Install [Node.js](https://nodejs.org/download/)</span></span>
* <span data-ttu-id="aabba-113">Telepítés [Python 2.7.10 verziója](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="aabba-113">Install [Python version 2.7.10](https://www.python.org/)</span></span>

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="aabba-114">Egy Felhőszolgáltatás-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="aabba-114">Create a Cloud Service Project</span></span>
<span data-ttu-id="aabba-115">Az alábbi lépéseket a felhőszolgáltatás-projekt, amely üzemelteti a Socket.IO alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aabba-115">The following steps create the cloud service project that will host the Socket.IO application.</span></span>

1. <span data-ttu-id="aabba-116">Az a **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="aabba-116">From the **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="aabba-117">Végül kattintson a jobb gombbal **Windows PowerShell** válassza **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="aabba-117">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Az Azure PowerShell ikon][powershell-menu]
2. <span data-ttu-id="aabba-119">Hozzon létre egy könyvtárat nevű **c:\\csomópont**.</span><span class="sxs-lookup"><span data-stu-id="aabba-119">Create a directory called **c:\\node**.</span></span> 
   
        PS C:\> md node
3. <span data-ttu-id="aabba-120">Lépjen a **c:\\csomópont** könyvtár</span><span class="sxs-lookup"><span data-stu-id="aabba-120">Change directories to the **c:\\node** directory</span></span>
   
        PS C:\> cd node
4. <span data-ttu-id="aabba-121">Adja meg a következő parancsok futtatásával hozzon létre egy új megoldás nevű **chatapp** és a feldolgozói szerepkör nevű **WorkerRole1**:</span><span class="sxs-lookup"><span data-stu-id="aabba-121">Enter the following commands to create a new solution named **chatapp** and a worker role named **WorkerRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    <span data-ttu-id="aabba-122">A következő válasz jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="aabba-122">You will see the following response:</span></span>
   
    ![Az új-azureservice és hozzáadása azurenodeworkerrolecmdlets kimenete](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a><span data-ttu-id="aabba-124">Töltse le a Csevegés – példa</span><span class="sxs-lookup"><span data-stu-id="aabba-124">Download the Chat Example</span></span>
<span data-ttu-id="aabba-125">Ebben a projektben a Csevegés példa fogjuk használni a [Socket.IO GitHub-tárházban].</span><span class="sxs-lookup"><span data-stu-id="aabba-125">For this project, we will use the chat example from the [Socket.IO GitHub repository].</span></span> <span data-ttu-id="aabba-126">A következő lépésekkel töltse le a példában, és adja hozzá a projekthez, korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="aabba-126">Perform the following steps to download the example and add it to the project you previously created.</span></span>

1. <span data-ttu-id="aabba-127">A tárház helyi másolat létrehozása a **Klónozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="aabba-127">Create a local copy of the repository by using the **Clone** button.</span></span> <span data-ttu-id="aabba-128">Is használhatja a **ZIP-** gombra kattintva töltse le a projektet.</span><span class="sxs-lookup"><span data-stu-id="aabba-128">You may also use the **ZIP** button to download the project.</span></span>
   
   ![Egy böngészőablakban https://github.com/LearnBoost/socket.io/tree/master/examples/chat, megtekintését a ZIP-letöltési ikonja kiemelve][chat-example-view]
2. <span data-ttu-id="aabba-130">Keresse meg a helyi tárház a könyvtárstruktúra, amíg megérkezik a a **példák\\Csevegés** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="aabba-130">Navigate the directory structure of the local repository until you arrive at the **examples\\chat** directory.</span></span> <span data-ttu-id="aabba-131">A könyvtár tartalmának másolása a **C:\\csomópont\\chatapp\\WorkerRole1** korábban létrehozott könyvtár.</span><span class="sxs-lookup"><span data-stu-id="aabba-131">Copy the contents of this directory to the **C:\\node\\chatapp\\WorkerRole1** directory created earlier.</span></span>
   
   ![Explorer, a példák tartalmának\\Csevegés directory kinyert az archívumban][chat-contents]
   
   <span data-ttu-id="aabba-133">A fenti képernyőfelvételen kiemelt elemeket a rendszer átmásolja a fájlokat a **példák\\Csevegés** könyvtár</span><span class="sxs-lookup"><span data-stu-id="aabba-133">The highlighted items in the screenshot above are the files copied from the **examples\\chat** directory</span></span>
3. <span data-ttu-id="aabba-134">Az a **C:\\csomópont\\chatapp\\WorkerRole1** könyvtár, törölje a **server.js** fájlt, és nevezze át a **app.js** fájl **server.js**.</span><span class="sxs-lookup"><span data-stu-id="aabba-134">In the **C:\\node\\chatapp\\WorkerRole1** directory, delete the **server.js** file, and then rename the **app.js** file to **server.js**.</span></span> <span data-ttu-id="aabba-135">Ezzel eltávolítja az alapértelmezett **server.js** által korábban létrehozott fájl a **Add-AzureNodeWorkerRole** parancsmag és az alkalmazásfájl a Csevegés példa felülírja.</span><span class="sxs-lookup"><span data-stu-id="aabba-135">This removes the default **server.js** file created previously by the **Add-AzureNodeWorkerRole** cmdlet and replaces it with the application file from the chat example.</span></span>

### <a name="modify-serverjs-and-install-modules"></a><span data-ttu-id="aabba-136">Módosítsa a Server.js és -modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="aabba-136">Modify Server.js and Install Modules</span></span>
<span data-ttu-id="aabba-137">Mielőtt az alkalmazás tesztelése az Azure emulátorban, néhány kisebb módosításokat kell azt.</span><span class="sxs-lookup"><span data-stu-id="aabba-137">Before testing the application in the Azure emulator, we must make some minor modifications.</span></span> <span data-ttu-id="aabba-138">Hajtsa végre a megfelelő server.js fájl az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="aabba-138">Perform the following steps to the server.js file:</span></span>

1. <span data-ttu-id="aabba-139">Nyissa meg a **server.js** fájlt a Visual Studio vagy szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="aabba-139">Open the **server.js** file in Visual Studio or any text editor.</span></span>
2. <span data-ttu-id="aabba-140">Keresés a **modul függőségek** server.js elején szakaszt, és módosítsa a sor tartalmazó **sio = require('.. //.. lib//Socket.IO ")** való **sio = require('socket.io')** alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="aabba-140">Find the **Module dependencies** section at the beginning of server.js and change the line containing **sio = require('..//..//lib//socket.io')** to **sio = require('socket.io')** as shown below:</span></span>
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. <span data-ttu-id="aabba-141">Annak érdekében, hogy az alkalmazás a helyes portot figyeli, nyissa meg a server.js a Jegyzettömb vagy a kedvenc szerkesztőt, és módosítsa a következő sor tartományvezérlőkkel történő lecserélésével **3000** rendelkező **process.env.port** alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="aabba-141">To ensure the application listens on the correct port, open server.js in Notepad or your favorite editor, and then change the following line by replacing **3000** with **process.env.port** as shown below:</span></span>
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

<span data-ttu-id="aabba-142">A módosítások mentése után **server.js**, a következő lépésekkel telepítse a szükséges modulokat, és tesztelje az alkalmazás az Azure emulátorban:</span><span class="sxs-lookup"><span data-stu-id="aabba-142">After saving the changes to **server.js**, use the following steps to install required modules, and then test the application in the Azure emulator:</span></span>

1. <span data-ttu-id="aabba-143">Használatával **Azure PowerShell**, lépjen a **C:\\csomópont\\chatapp\\WorkerRole1** könyvtárra, és használja, a következő parancsot a modulok telepítése az alkalmazás számára szükséges:</span><span class="sxs-lookup"><span data-stu-id="aabba-143">Using **Azure PowerShell**, change directories to the **C:\\node\\chatapp\\WorkerRole1** directory and use the following command to install the modules required by this application:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   <span data-ttu-id="aabba-144">Ez telepíti a package.json fájlban felsorolt modulok.</span><span class="sxs-lookup"><span data-stu-id="aabba-144">This will install the modules listed in the package.json file.</span></span> <span data-ttu-id="aabba-145">A parancs befejezése után a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="aabba-145">After the command completes, you should see output similar to the following:</span></span>
   
   ![Az npm kimenete telepítési parancs][The-output-of-the-npm-install-command]
2. <span data-ttu-id="aabba-147">Mivel ebben a példában eredetileg a Socket.IO GitHub-tárházban része volt, és közvetlenül a Socket.IO könyvtár hivatkozik relatív elérési út, Socket.IO nem mutat hivatkozás a package.json fájlban, ezért azt telepítenie kell a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="aabba-147">Since this example was originally a part of the Socket.IO GitHub repository, and directly referenced the Socket.IO library by relative path, Socket.IO was not referenced in the package.json file, so we must install it by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a><span data-ttu-id="aabba-148">Tesztelése és telepítése</span><span class="sxs-lookup"><span data-stu-id="aabba-148">Test and Deploy</span></span>
1. <span data-ttu-id="aabba-149">Indítsa el az emulátorban a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="aabba-149">Launch the emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > <span data-ttu-id="aabba-150">Ha problémák lépnek fel az emulátor, pl. indítása.: Start-AzureEmulator: nem várt hiba történt.</span><span class="sxs-lookup"><span data-stu-id="aabba-150">If you encounter issues with launching emulator, eg.: Start-AzureEmulator : An unexpected failure occurred.</span></span>  <span data-ttu-id="aabba-151">Részletek: Észlelt váratlan hiba a kommunikációs objektum System.ServiceModel.Channels.ServiceChannel, nem használható kommunikációra, mert Faulted állapotban van.</span><span class="sxs-lookup"><span data-stu-id="aabba-151">Details: Encountered an unexpected error The communication object,  System.ServiceModel.Channels.ServiceChannel, cannot be used for communication because it is in the Faulted state.</span></span>
   
      <span data-ttu-id="aabba-152">Telepítse újra a AzureAuthoringTools v 2.7.1-es és AzureComputeEmulator v 2.7 – győződjön meg arról, hogy verziója megegyezik.</span><span class="sxs-lookup"><span data-stu-id="aabba-152">reinstall AzureAuthoringTools v 2.7.1 and AzureComputeEmulator v 2.7 - make sure that version matches.</span></span>
   >
   >


2. <span data-ttu-id="aabba-153">Nyisson meg egy böngészőt, és keresse meg **http://127.0.0.1**.</span><span class="sxs-lookup"><span data-stu-id="aabba-153">Open a browser and navigate to **http://127.0.0.1**.</span></span>
3. <span data-ttu-id="aabba-154">Amikor megnyílik a böngészőablakban, becenév adja meg, és nyomja le írja be.</span><span class="sxs-lookup"><span data-stu-id="aabba-154">When the browser window opens, enter a nickname and then hit enter.</span></span>
   <span data-ttu-id="aabba-155">Ez lehetővé teszi, hogy egy adott becenév üzenetek elküldeni.</span><span class="sxs-lookup"><span data-stu-id="aabba-155">This will allow you to post messages as a specific nickname.</span></span> <span data-ttu-id="aabba-156">Többfelhasználós működésének teszteléséhez nyissa meg az azonos URL-cím segítségével további böngészőablakot, és adjon meg másik becenevet.</span><span class="sxs-lookup"><span data-stu-id="aabba-156">To test multi-user functionality, open additional browser windows using the same URL and enter different nicknames.</span></span>
   
   ![Két böngészőablakot felhasználó1, mind a Felhasználó2 üzeneteit megjelenítése](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. <span data-ttu-id="aabba-158">Után az alkalmazás tesztelése, állítsa le az emulátorban a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="aabba-158">After testing the application, stop the emulator by issuing the following command:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. <span data-ttu-id="aabba-159">Telepítse központilag az alkalmazást, az Azure-ba, használja a **Publish-AzureServiceProject** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="aabba-159">To deploy the application to Azure, use the **Publish-AzureServiceProject** cmdlet.</span></span> <span data-ttu-id="aabba-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="aabba-160">For example:</span></span>
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > <span data-ttu-id="aabba-161">Ügyeljen arra, hogy az egyedi név, máskülönben a közzétételi folyamat meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="aabba-161">Be sure to use a unique name, otherwise the publish process will fail.</span></span> <span data-ttu-id="aabba-162">A telepítés befejezése után a böngésző megnyitásához, és keresse meg a telepített szolgáltatáson.</span><span class="sxs-lookup"><span data-stu-id="aabba-162">After the deployment has completed, the browser will open and navigate to the deployed service.</span></span>
   > 
   > <span data-ttu-id="aabba-163">Ha hibaüzenet jelenik meg, amely meghatározza, hogy, hogy a megadott előfizetés nevét az importált közzétételi profil nem létezik, akkor töltse le, és az előfizetéshez, mielőtt telepítené az Azure közzétételi profil importálásához.</span><span class="sxs-lookup"><span data-stu-id="aabba-163">If you receive an error stating that the provided subscription name doesn't exist in the imported publish profile, you must download and import the publishing profile for your subscription before deploying to Azure.</span></span> <span data-ttu-id="aabba-164">Tekintse meg a **Azure az alkalmazás központi telepítését** szakasza [felépítéséhez és az Azure Cloud Service a Node.js-alkalmazás központi telepítése](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="aabba-164">See the **Deploying the Application to Azure** section of [Build and deploy a Node.js application to an Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 
   
   ![Az Azure-platformon futó szolgáltatás megjelenítő böngészőablak][completed-app]
   
   > [!NOTE]
   > <span data-ttu-id="aabba-166">Ha hibaüzenet jelenik meg, amely meghatározza, hogy, hogy a megadott előfizetés nevét az importált közzétételi profil nem létezik, akkor töltse le, és az előfizetéshez, mielőtt telepítené az Azure közzétételi profil importálásához.</span><span class="sxs-lookup"><span data-stu-id="aabba-166">If you receive an error stating that the provided subscription name doesn't exist in the imported publish profile, you must download and import the publishing profile for your subscription before deploying to Azure.</span></span> <span data-ttu-id="aabba-167">Tekintse meg a **Azure az alkalmazás központi telepítését** szakasza [felépítéséhez és az Azure Cloud Service a Node.js-alkalmazás központi telepítése](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span><span class="sxs-lookup"><span data-stu-id="aabba-167">See the **Deploying the Application to Azure** section of [Build and deploy a Node.js application to an Azure Cloud Service](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)</span></span>
   > 
   > 

<span data-ttu-id="aabba-168">Az alkalmazás mostantól az Azure-on fut, és továbbíthat csevegési üzeneteket a Socket.IO segítségével különböző ügyfelek között.</span><span class="sxs-lookup"><span data-stu-id="aabba-168">Your application is now running on Azure, and can relay chat messages between different clients using Socket.IO.</span></span>

> [!NOTE]
> <span data-ttu-id="aabba-169">Az egyszerűség Ez a minta korlátozódik Csevegés ugyanazon csatlakozó felhasználók között.</span><span class="sxs-lookup"><span data-stu-id="aabba-169">For simplicity, this sample is limited to chatting between users connected to the same instance.</span></span> <span data-ttu-id="aabba-170">Ez azt jelenti, hogy a felhőszolgáltatás két szerepkör feldolgozópéldányok hoz létre, ha felhasználók csak tudnak megoszthatja másokkal a feldolgozói szerepkör példányt csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="aabba-170">This means that if the cloud service creates two worker role instances, users will only be able to chat with others connected to the same worker role instance.</span></span> <span data-ttu-id="aabba-171">Az alkalmazás működéséhez a szerepkör több példánya méretezéséhez olyan technológia, például a Service Bus segítségével a Socket.IO-állapot tárolása megosztani példányok.</span><span class="sxs-lookup"><span data-stu-id="aabba-171">To scale the application to work with multiple role instances, you could use a technology like Service Bus to share the Socket.IO store state across instances.</span></span> <span data-ttu-id="aabba-172">Tekintse meg a Service Bus-üzenetsorok és témakörök használati minták a [Azure SDK for Node.js GitHub-tárházban](https://github.com/WindowsAzure/azure-sdk-for-node).</span><span class="sxs-lookup"><span data-stu-id="aabba-172">For examples, see the Service Bus Queues and Topics usage samples in the [Azure SDK for Node.js GitHub repository](https://github.com/WindowsAzure/azure-sdk-for-node).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="aabba-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aabba-173">Next steps</span></span>
<span data-ttu-id="aabba-174">Ez az oktatóanyag megtanulta, hogyan hozhat létre egy Azure-Felhőszolgáltatásban tárolt alapvető Csevegés alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aabba-174">In this tutorial you learned how to create a basic chat application hosted in an Azure Cloud Service.</span></span> <span data-ttu-id="aabba-175">Ez az alkalmazás az Azure-webhely üzemeltetésére, lásd: [összeállíthat egy Node.js Csevegés alkalmazást a Socket.IO segítségével az Azure webhelyén található][chatwebsite].</span><span class="sxs-lookup"><span data-stu-id="aabba-175">To learn how to host this application in an Azure Website, see [Build a Node.js Chat Application with Socket.IO on an Azure Web Site][chatwebsite].</span></span>

<span data-ttu-id="aabba-176">További információ: a [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="aabba-176">For more information, see also the [Node.js Developer Center](/develop/nodejs/).</span></span>

[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

[Azure SLA]: http://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
<span data-ttu-id="aabba-177">[Socket.IO GitHub-tárházban]: https://github.com/LearnBoost/socket.io/tree/0.9.14</span><span class="sxs-lookup"><span data-stu-id="aabba-177">[Socket.IO GitHub repository]: https://github.com/LearnBoost/socket.io/tree/0.9.14</span></span>
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png


