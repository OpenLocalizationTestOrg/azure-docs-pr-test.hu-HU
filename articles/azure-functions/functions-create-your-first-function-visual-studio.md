---
title: "Az első függvény létrehozása az Azure-ban a Visual Studio használatával | Microsoft Docs"
description: "Hozzon létre és tegyen közzé egy egyszerű, HTTP-triggert használó függvényt az Azure-ban az Azure Functions Tools for Visual Studio használatával."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure functions, függvények, eseményfeldolgozás, számítás, kiszolgáló nélküli architektúra"
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 04370558725d76ffe83d8aaf5d16c88fd2803ba9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-using-visual-studio"></a><span data-ttu-id="13b1e-104">Az első függvény létrehozása a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="13b1e-104">Create your first function using Visual Studio</span></span>

<span data-ttu-id="13b1e-105">Az Azure Functions lehetővé teszi a kód végrehajtását kiszolgáló nélküli környezetben anélkül, hogy először létre kellene hoznia egy virtuális gépet vagy közzé kellene tennie egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="13b1e-105">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span>

<span data-ttu-id="13b1e-106">Ebben a témakörben elsajátíthatja, hogyan a Visual Studio 2017 eszközök, az Azure Functions segítségével hozza létre, és a "hello world" függvény helyi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="13b1e-106">In this topic, you learn how to use the Visual Studio 2017 tools for Azure Functions to create and test a "hello world" function locally.</span></span> <span data-ttu-id="13b1e-107">Ezután közzéteheti a függvénykódot az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="13b1e-107">You will then publish the function code to Azure.</span></span> <span data-ttu-id="13b1e-108">Ezek az eszközök a Visual Studio 2017 15.3-as és újabb verziójában található Azure-fejlesztési számítási feladat részeként érhetők el.</span><span class="sxs-lookup"><span data-stu-id="13b1e-108">These tools are available as part of the Azure development workload in Visual Studio 2017 version 15.3, or a later version.</span></span>

![Azure-függvénykód Visual Studio-projektben](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a><span data-ttu-id="13b1e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13b1e-110">Prerequisites</span></span>

<span data-ttu-id="13b1e-111">Az oktatóanyag elvégzéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="13b1e-111">To complete this tutorial, install:</span></span>

* <span data-ttu-id="13b1e-112">A [Visual Studio 2017 15.3-as verziója](https://www.visualstudio.com/vs/preview/), amely tartalmazza az **Azure-fejlesztési** számítási feladatot is.</span><span class="sxs-lookup"><span data-stu-id="13b1e-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), including the **Azure development** workload.</span></span>

    ![Az Azure-fejlesztési számítási feladatot is tartalmazó Visual Studio 2017 telepítése](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    <span data-ttu-id="13b1e-114">Visual Studio 2017 15.3 verzió frissítése vagy telepítése után is szükség lehet manuálisan frissíteni az Azure Functions a Visual Studio 2017 eszközök.</span><span class="sxs-lookup"><span data-stu-id="13b1e-114">After you install or upgrade to Visual Studio 2017 version 15.3, you might also need to manually update the Visual Studio 2017 tools for Azure Functions.</span></span> <span data-ttu-id="13b1e-115">Az eszközök frissítheti a **eszközök** menüt **bővítmények és frissítések...**   >  **Frissítések** > **Visual Studio piactér** > **az Azure Functions és webes feladatok eszközök**  >  **Frissítés**.</span><span class="sxs-lookup"><span data-stu-id="13b1e-115">You can update the tools from the **Tools** menu under **Extensions and Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a><span data-ttu-id="13b1e-116">Hozzon létre egy Azure Functions-projektet a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="13b1e-116">Create an Azure Functions project in Visual Studio</span></span>

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

<span data-ttu-id="13b1e-117">Most, hogy már létrehozta a projektet, hozza létre első függvényét.</span><span class="sxs-lookup"><span data-stu-id="13b1e-117">Now that you have created the project, you can create your first function.</span></span>

## <a name="create-the-function"></a><span data-ttu-id="13b1e-118">A függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="13b1e-118">Create the function</span></span>

1. <span data-ttu-id="13b1e-119">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza az **Add** (Hozzáadás)  > **New Item** (Új elem) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="13b1e-119">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="13b1e-120">Válassza ki az **Azure Function** (Azure-függvény) elemet, majd kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="13b1e-120">Select **Azure Function** and click **Add**.</span></span>

2. <span data-ttu-id="13b1e-121">Válassza ki a **HttpTrigger** elemet, adja meg a **Function Name** (Függvénynév) értékét, az **Access Rights** (Hozzáférési jog) lehetőségnél válassza az **Anonymous** (Névtelen) elemet, majd kattintson a **Create** (Létrehozás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="13b1e-121">Select **HttpTrigger**, type a **Function Name**, select **Anonymous** for **Access Rights**, and click **Create**.</span></span> <span data-ttu-id="13b1e-122">A létrehozott függvény HTTP-kéréssel bármely ügyfél részéről hozzáférhető.</span><span class="sxs-lookup"><span data-stu-id="13b1e-122">The function created is accessed by an HTTP request from any client.</span></span> 

    ![Új Azure-függvény létrehozása](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    <span data-ttu-id="13b1e-124">A forráskód fájlja kerül a projekt, amely tartalmaz egy osztály, amely megvalósítja a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="13b1e-124">A code file is added to your project that contains a class that implements your function code.</span></span> <span data-ttu-id="13b1e-125">Ez a kód egy sablon, amely fogad egy név-érték és vissza Echok alapul.</span><span class="sxs-lookup"><span data-stu-id="13b1e-125">This code is based on a template, which receives a name value and echos it back.</span></span> <span data-ttu-id="13b1e-126">A **függvénynév** attribútum állítja be a függvény nevét.</span><span class="sxs-lookup"><span data-stu-id="13b1e-126">The **FunctionName** attribute sets the name of your function.</span></span> <span data-ttu-id="13b1e-127">A **HttpTrigger** attribútum azt jelöli, az üzenet, amely elindítja a függvényt.</span><span class="sxs-lookup"><span data-stu-id="13b1e-127">The **HttpTrigger** attribute indicates the message that triggers the function.</span></span> 

    ![Függvény kódfájl](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

<span data-ttu-id="13b1e-129">Most, hogy már létrehozta a HTTP-triggerrel aktivált függvényt, tesztelheti a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="13b1e-129">Now that you have created an HTTP-triggered function, you can test it on your local computer.</span></span>

## <a name="test-the-function-locally"></a><span data-ttu-id="13b1e-130">A függvény helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="13b1e-130">Test the function locally</span></span>

<span data-ttu-id="13b1e-131">Az Azure Functions Core Tools lehetővé teszi Azure Functions-projektek helyi fejlesztői számítógépen való futtatását.</span><span class="sxs-lookup"><span data-stu-id="13b1e-131">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="13b1e-132">Amikor a Visual Studióból először indít el egy függvényt, a rendszer arra kéri, hogy telepítse ezeket az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="13b1e-132">You are prompted to install these tools the first time you start a function from Visual Studio.</span></span>  

1. <span data-ttu-id="13b1e-133">A függvény teszteléséhez nyomja le az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="13b1e-133">To test your function, press F5.</span></span> <span data-ttu-id="13b1e-134">Ha a rendszer kéri, fogadja el a Visual Studio kérését az Azure Functions Core (CLI) eszközök telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="13b1e-134">If prompted, accept the request from Visual Studio to download and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="13b1e-135">Lehet, hogy egy tűzfalkivételt is engedélyeznie kell, hogy az eszközök kezelhessék a HTTP-kéréseket.</span><span class="sxs-lookup"><span data-stu-id="13b1e-135">You may also need to enable a firewall exception so that the tools can handle HTTP requests.</span></span>

2. <span data-ttu-id="13b1e-136">Másolja a függvény URL-címét az Azure-függvény futtatókörnyezetéből.</span><span class="sxs-lookup"><span data-stu-id="13b1e-136">Copy the URL of your function from the Azure Functions runtime output.</span></span>  

    ![Az Azure helyi futtatókörnyezete](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. <span data-ttu-id="13b1e-138">Illessze be a HTTP-kérelem URL-címét a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="13b1e-138">Paste the URL for the HTTP request into your browser's address bar.</span></span> <span data-ttu-id="13b1e-139">Az URL-címhez fűzze hozzá a `&name=<yourname>` lekérdezési karakterláncot, és hajtsa végre a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="13b1e-139">Append the query string `&name=<yourname>` to this URL and execute the request.</span></span> <span data-ttu-id="13b1e-140">Az alábbiakban látható a böngészőben a helyi GET kérelemre a függvény által visszaadott válasz:</span><span class="sxs-lookup"><span data-stu-id="13b1e-140">The following shows the response in the browser to the local GET request returned by the function:</span></span> 

    ![A függvény által visszaadott localhost válasz a böngészőben](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. <span data-ttu-id="13b1e-142">A hibakeresés leállításához kattintson a **Stop** (Leállítás) gombra a Visual Studio eszköztárában.</span><span class="sxs-lookup"><span data-stu-id="13b1e-142">To stop debugging, click the **Stop** button on the Visual Studio toolbar.</span></span>

<span data-ttu-id="13b1e-143">Miután ellenőrizte, hogy a függvény megfelelően fut a helyi számítógépen, tegye közzé a projektet az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="13b1e-143">After you have verified that the function runs correctly on your local computer, it's time to publish the project to Azure.</span></span>

## <a name="publish-the-project-to-azure"></a><span data-ttu-id="13b1e-144">A projekt közzététele az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="13b1e-144">Publish the project to Azure</span></span>

<span data-ttu-id="13b1e-145">A projekt közzétételéhez rendelkeznie kell egy függvényalkalmazással.az Azure-előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="13b1e-145">You must have a function app in your Azure subscription before you can publish your project.</span></span> <span data-ttu-id="13b1e-146">Közvetlenül a Visual Studióból is létrehozhat függvényalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="13b1e-146">You can create a function app right from Visual Studio.</span></span>

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a><span data-ttu-id="13b1e-147">A függvény tesztelése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="13b1e-147">Test your function in Azure</span></span>

1. <span data-ttu-id="13b1e-148">Másolja a függvényalkalmazás alap URL-címét a Publish (Közzététel) profiloldalról.</span><span class="sxs-lookup"><span data-stu-id="13b1e-148">Copy the base URL of the function app from the Publish profile page.</span></span> <span data-ttu-id="13b1e-149">Cserélje ki a függvény helyi tesztelésekor használt `localhost:port` URL-címrészt az új alap URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="13b1e-149">Replace the `localhost:port` portion of the URL you used when testing the function locally with the new base URL.</span></span> <span data-ttu-id="13b1e-150">Ahogyan korábban, most is az URL-címhez fűzze hozzá a `&name=<yourname>` lekérdezési sztringet, és hajtsa végre a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="13b1e-150">As before, make sure to append the query string `&name=<yourname>` to this URL and execute the request.</span></span>

    <span data-ttu-id="13b1e-151">A HTTP-triggert használó függvényt meghívó URL-cím így néz ki:</span><span class="sxs-lookup"><span data-stu-id="13b1e-151">The URL that calls your HTTP triggered function looks like this:</span></span>

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. <span data-ttu-id="13b1e-152">Illessze be a HTTP-kérelem új URL-címét a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="13b1e-152">Paste this new URL for the HTTP request into your browser's address bar.</span></span> <span data-ttu-id="13b1e-153">Az alábbiakban látható a böngészőben a távoli GET kérelemre a függvény által visszaadott válasz:</span><span class="sxs-lookup"><span data-stu-id="13b1e-153">The following shows the response in the browser to the remote GET request returned by the function:</span></span> 

    ![A függvény által visszaadott válasz a böngészőben](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a><span data-ttu-id="13b1e-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13b1e-155">Next steps</span></span>

<span data-ttu-id="13b1e-156">A Visual Studio segítéségével létrehozott egy egyszerű, HTTP-triggerrel aktivált függvényt tartalmazó C#-függvényalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="13b1e-156">You have used Visual Studio to create a C# function app with a simple HTTP triggered function.</span></span> 

+ <span data-ttu-id="13b1e-157">A projekt más típusú eseményindítók és kötések támogatására történő konfigurálásával az [Azure Functions Tools for Visual Studio](functions-develop-vs.md) [projekt helyi fejlesztésekhez való konfigurálásával](functions-develop-vs.md#configure-the-project-for-local-development) kapcsolatos szakaszában ismerkedhet meg.</span><span class="sxs-lookup"><span data-stu-id="13b1e-157">To learn how to configure your project to support other types of triggers and bindings, see the [Configure the project for local development](functions-develop-vs.md#configure-the-project-for-local-development) section in [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span></span>
+ <span data-ttu-id="13b1e-158">További információ az Azure Functions Core Tools használatával végzett helyi tesztelésről és hibakeresésről: [Code and test Azure Functions locally](functions-run-local.md) (Az Azure-függvények kódolása és helyi tesztelése).</span><span class="sxs-lookup"><span data-stu-id="13b1e-158">To learn more about local testing and debugging using the Azure Functions Core Tools, see [Code and test Azure Functions locally](functions-run-local.md).</span></span> 
+ <span data-ttu-id="13b1e-159">A függvények .NET-osztálytárakként való fejlesztéséről további információért lásd [a .NET-osztálytárak és az Azure Functions használatát](functions-dotnet-class-library.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="13b1e-159">To learn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> 

