---
title: App (Node.js) expressz aaaWeb |} Microsoft Docs
description: "Az oktatóanyag, amely hello cloud service oktatóanyag épül, és bemutatja, hogyan toouse hello Express modul."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 91921bfbe137eeca9a110d4cb18eb57b46b0060e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a><span data-ttu-id="47463-103">Express használata Azure Cloud Service a Node.js-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="47463-103">Build a Node.js web application using Express on an Azure Cloud Service</span></span>
<span data-ttu-id="47463-104">NODE.js hello core runtime a legszükségesebb funkciókat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="47463-104">Node.js includes a minimal set of functionality in hello core runtime.</span></span>
<span data-ttu-id="47463-105">A fejlesztők gyakran használnak 3. fél modulok tooprovide további funkciókat, a Node.js-alkalmazások fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="47463-105">Developers often use 3rd party modules tooprovide additional functionality when developing a Node.js application.</span></span> <span data-ttu-id="47463-106">Ebben az oktatóanyagban létrehoz egy új alkalmazást, az hello [Express] [ Express] modult, amely egy MVC keretrendszert biztosít a Node.js webalkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="47463-106">In this tutorial you will create a new application using hello [Express][Express] module, which provides an MVC framework for creating Node.js web applications.</span></span>

<span data-ttu-id="47463-107">A képernyőfelvétel a hello befejeződött alkalmazás alatt van:</span><span class="sxs-lookup"><span data-stu-id="47463-107">A screenshot of hello completed application is below:</span></span>

![A webböngészőben tooExpress Üdvözli az Azure-ban](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a><span data-ttu-id="47463-109">Egy Felhőszolgáltatás-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="47463-109">Create a Cloud Service Project</span></span>
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

<span data-ttu-id="47463-110">Hajtsa végre a következő hello lépéseket toocreate egy új felhőszolgáltatás-projekt "expressapp" nevű:</span><span class="sxs-lookup"><span data-stu-id="47463-110">Perform hello following steps toocreate a new cloud service project named 'expressapp':</span></span>

1. <span data-ttu-id="47463-111">A hello **Start menü** vagy **kezdőképernyőn**, keressen **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="47463-111">From hello **Start Menu** or **Start Screen**, search for **Windows PowerShell**.</span></span> <span data-ttu-id="47463-112">Végül kattintson a jobb gombbal **Windows PowerShell** válassza **Futtatás rendszergazdaként**.</span><span class="sxs-lookup"><span data-stu-id="47463-112">Finally, right-click **Windows PowerShell** and select **Run As Administrator**.</span></span>
   
    ![Az Azure PowerShell ikon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. <span data-ttu-id="47463-114">Módosítsa a könyvtárakat toohello **c:\\csomópont** könyvtár és írja be a következő parancsok toocreate nevű új megoldás hello **expressapp** és a webes szerepkör nevű **WebRole1** :</span><span class="sxs-lookup"><span data-stu-id="47463-114">Change directories toohello **c:\\node** directory and then enter hello following commands toocreate a new solution named **expressapp** and a web role named **WebRole1**:</span></span>
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > <span data-ttu-id="47463-115">Alapértelmezés szerint **Add-AzureNodeWebRole** Node.js régebbi verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="47463-115">By default, **Add-AzureNodeWebRole** uses an older version of Node.js.</span></span> <span data-ttu-id="47463-116">Hello **Set-AzureServiceProjectRole** fenti utasítás arra utasítja az Azure toouse v0.10.21 csomópont.</span><span class="sxs-lookup"><span data-stu-id="47463-116">hello **Set-AzureServiceProjectRole** statement above instructs Azure toouse v0.10.21 of Node.</span></span>  <span data-ttu-id="47463-117">Megjegyzés: hello paraméterei kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="47463-117">Note hello parameters are case-sensitive.</span></span>  <span data-ttu-id="47463-118">Ellenőrizheti a Node.js hello verziójának kijelölt hello ellenőrzésével **motorok** tulajdonság **WebRole1\package.json**.</span><span class="sxs-lookup"><span data-stu-id="47463-118">You can verify hello correct version of Node.js has been selected by checking hello **engines** property in **WebRole1\package.json**.</span></span>
    > 
    > 

## <a name="install-express"></a><span data-ttu-id="47463-119">Express telepítése</span><span class="sxs-lookup"><span data-stu-id="47463-119">Install Express</span></span>
1. <span data-ttu-id="47463-120">Telepítse a hello Express generátor hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="47463-120">Install hello Express generator by issuing hello following command:</span></span>
   
        PS C:\node\expressapp> npm install express-generator -g
   
    <span data-ttu-id="47463-121">hello npm parancs kimenetében hello toohello hasonló, az alábbi eredmény kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="47463-121">hello output of hello npm command should look similar toohello result below.</span></span> 
   
    ![A Windows PowerShell megjelenítését hello kimenete hello npm telepítési expressz parancs.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. <span data-ttu-id="47463-123">Módosítsa a könyvtárakat toohello **WebRole1** könyvtárra, és használja hello expressz parancs toogenerate egy új alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="47463-123">Change directories toohello **WebRole1** directory and use hello express command toogenerate a new application:</span></span>
   
        PS C:\node\expressapp\WebRole1> express
   
    <span data-ttu-id="47463-124">Akkor lesz felszólító toooverwrite a korábbi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="47463-124">You will be prompted toooverwrite your earlier application.</span></span> <span data-ttu-id="47463-125">Adja meg **y** vagy **Igen** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="47463-125">Enter **y** or **yes** toocontinue.</span></span> <span data-ttu-id="47463-126">Express hello app.js fájl- és az alkalmazás felépítése mappaszerkezetét hoz létre.</span><span class="sxs-lookup"><span data-stu-id="47463-126">Express will generate hello app.js file and a folder structure for building your application.</span></span>
   
    ![hello hello expressz parancs kimenete](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. <span data-ttu-id="47463-128">tooinstall további függőségek hello package.json fájlban meghatározott adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="47463-128">tooinstall additional dependencies defined in hello package.json file, enter hello following command:</span></span>
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![hello npm hello kimenete telepítési parancs](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. <span data-ttu-id="47463-130">Használjon hello következő parancsot a toocopy hello **bin/www** fájl túl**server.js**.</span><span class="sxs-lookup"><span data-stu-id="47463-130">Use hello following command toocopy hello **bin/www** file too**server.js**.</span></span> <span data-ttu-id="47463-131">Ez a helyzet hello felhőszolgáltatás hello belépési ponton található ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="47463-131">This is so hello cloud service can find hello entry point for this application.</span></span>
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   <span data-ttu-id="47463-132">Ez a parancs után rendelkeznie kell egy **server.js** hello WebRole1 fájl.</span><span class="sxs-lookup"><span data-stu-id="47463-132">After this command completes, you should have a **server.js** file in hello WebRole1 directory.</span></span>
5. <span data-ttu-id="47463-133">Hello módosítása **server.js** hello egyik tooremove "." karaktert tartalmaz a következő sor hello.</span><span class="sxs-lookup"><span data-stu-id="47463-133">Modify hello **server.js** tooremove one of hello '.' characters from hello following line.</span></span>
   
       var app = require('../app');
   
   <span data-ttu-id="47463-134">Ez a módosítás elvégzése, után hello sor meg kell jelennie az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="47463-134">After making this modification, hello line should appear as follows.</span></span>
   
       var app = require('./app');
   
   <span data-ttu-id="47463-135">Ez a változás azért szükséges, mert hello fájl helyeztük (korábbi nevén **bin/www**,) toohello megadni hello alkalmazásfájl könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="47463-135">This change is required since we moved hello file (formerly **bin/www**,) toohello same directory as hello app file being required.</span></span> <span data-ttu-id="47463-136">A módosítás elvégzése után mentse a hello **server.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="47463-136">After making this change, save hello **server.js** file.</span></span>
6. <span data-ttu-id="47463-137">A következő parancs toorun hello alkalmazás hello Azure emulátorban hello használata:</span><span class="sxs-lookup"><span data-stu-id="47463-137">Use hello following command toorun hello application in hello Azure emulator:</span></span>
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Üdvözli a tooexpress tartalmazó webhelyet.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-hello-view"></a><span data-ttu-id="47463-139">Hello nézet módosítása</span><span class="sxs-lookup"><span data-stu-id="47463-139">Modifying hello View</span></span>
<span data-ttu-id="47463-140">Most módosítsa a nézet toodisplay hello üdvözlőüzenetére "Üdvözli tooExpress az Azure-ban".</span><span class="sxs-lookup"><span data-stu-id="47463-140">Now modify hello view toodisplay hello message "Welcome tooExpress in Azure".</span></span>

1. <span data-ttu-id="47463-141">Adja meg a következő parancs tooopen hello index.jade fájl hello:</span><span class="sxs-lookup"><span data-stu-id="47463-141">Enter hello following command tooopen hello index.jade file:</span></span>
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![hello index.jade fájl hello tartalmát.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   <span data-ttu-id="47463-143">Jade hello alapértelmezett megjelenítési motort Express alkalmazások által használt.</span><span class="sxs-lookup"><span data-stu-id="47463-143">Jade is hello default view engine used by Express applications.</span></span> <span data-ttu-id="47463-144">Hello Jade megjelenítési motort további információkért lásd: [http://jade-lang.com][http://jade-lang.com].</span><span class="sxs-lookup"><span data-stu-id="47463-144">For more information on hello Jade view engine, see [http://jade-lang.com][http://jade-lang.com].</span></span>
2. <span data-ttu-id="47463-145">Szöveg utolsó sorának hello módosítása hozzáfűzésével **az Azure-ban**.</span><span class="sxs-lookup"><span data-stu-id="47463-145">Modify hello last line of text by appending **in Azure**.</span></span>
   
   ![hello index.jade fájl utolsó sora hello beolvassa: p üdvözli túl\#{title} az Azure-ban](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. <span data-ttu-id="47463-147">Mentse hello fájlt, és lépjen ki a Jegyzettömbből.</span><span class="sxs-lookup"><span data-stu-id="47463-147">Save hello file and exit Notepad.</span></span>
4. <span data-ttu-id="47463-148">Frissítse a böngészőt, és látni fogja a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="47463-148">Refresh your browser and you will see your changes.</span></span>
   
   ![Egy böngészőablakban hello lap tartalmazza-e az Azure-ban üdvözlő tooExpress](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

<span data-ttu-id="47463-150">Tesztelési hello alkalmazását követően használja a hello **Stop-AzureEmulator** parancsmag toostop hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="47463-150">After testing hello application, use hello **Stop-AzureEmulator** cmdlet toostop hello emulator.</span></span>

## <a name="publishing-hello-application-tooazure"></a><span data-ttu-id="47463-151">Közzétételi hello alkalmazás tooAzure</span><span class="sxs-lookup"><span data-stu-id="47463-151">Publishing hello Application tooAzure</span></span>
<span data-ttu-id="47463-152">Hello Azure PowerShell-ablakban, használja a hello **Publish-AzureServiceProject** parancsmag toodeploy hello tooa felhőalapú alkalmazásszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="47463-152">In hello Azure PowerShell window, use hello **Publish-AzureServiceProject** cmdlet toodeploy hello application tooa cloud service</span></span>

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

<span data-ttu-id="47463-153">Miután hello telepítési művelet befejeződik, a böngészőben nyissa meg, és hello weblap megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="47463-153">Once hello deployment operation completes, your browser will open and display hello web page.</span></span>

![A webböngészőben hello Express lap.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a><span data-ttu-id="47463-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47463-156">Next steps</span></span>
<span data-ttu-id="47463-157">További információkért lásd: hello [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="47463-157">For more information, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


