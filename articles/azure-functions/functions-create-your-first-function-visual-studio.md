---
title: "a Visual Studio használatával Azure-ban működik az első aaaCreate |} Microsoft Docs"
description: "Hozzon létre, és egy egyszerű indított HTTP függvény tooAzure közzétételéhez Azure Functions Tools for Visual Studio használatával."
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
ms.openlocfilehash: 851e5b98dcc2da00334620896a0ea31f566589f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-using-visual-studio"></a><span data-ttu-id="c2da4-104">Az első függvény létrehozása a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="c2da4-104">Create your first function using Visual Studio</span></span>

<span data-ttu-id="c2da4-105">Az Azure Functions lehetővé teszi, hogy a kód egy kiszolgáló nélküli környezetben anélkül, hogy hozzon létre egy virtuális Gépet, vagy tegye közzé a webalkalmazást toofirst hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="c2da4-105">Azure Functions lets you execute your code in a serverless environment without having toofirst create a VM or publish a web application.</span></span>

<span data-ttu-id="c2da4-106">Ebben a témakörben elsajátíthatja, hogyan toouse hello Azure Functions toocreate Visual Studio 2017 eszközei és a "hello world" függvény helyi tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c2da4-106">In this topic, you learn how toouse hello Visual Studio 2017 tools for Azure Functions toocreate and test a "hello world" function locally.</span></span> <span data-ttu-id="c2da4-107">Ezután a hello függvény kód tooAzure tesznek közzé.</span><span class="sxs-lookup"><span data-stu-id="c2da4-107">You will then publish hello function code tooAzure.</span></span> <span data-ttu-id="c2da4-108">Ezek az eszközök érhetők el, vagy a Visual Studio 2017 verzió 15.3 hello Azure fejlesztési munkaterhelés részeként egy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="c2da4-108">These tools are available as part of hello Azure development workload in Visual Studio 2017 version 15.3, or a later version.</span></span>

![Azure-függvénykód Visual Studio-projektben](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a><span data-ttu-id="c2da4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2da4-110">Prerequisites</span></span>

<span data-ttu-id="c2da4-111">toocomplete oktatóanyag, telepítés:</span><span class="sxs-lookup"><span data-stu-id="c2da4-111">toocomplete this tutorial, install:</span></span>

* <span data-ttu-id="c2da4-112">[A Visual Studio 2017 verzió 15.3](https://www.visualstudio.com/vs/preview/), beleértve a hello **Azure fejlesztési** munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="c2da4-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), including hello **Azure development** workload.</span></span>

    ![A Visual Studio 2017 hello Azure fejlesztési alkalmazások és szolgáltatások telepítése](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    <span data-ttu-id="c2da4-114">TooVisual Studio 2017 15.3 verzió frissítése vagy telepítése után szükség lehet toomanually frissítés hello Visual Studio 2017 eszközök az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c2da4-114">After you install or upgrade tooVisual Studio 2017 version 15.3, you might also need toomanually update hello Visual Studio 2017 tools for Azure Functions.</span></span> <span data-ttu-id="c2da4-115">Frissítheti a hello hello eszközök **eszközök** menüt **bővítmények és frissítések...**   >  **Frissítések** > **Visual Studio piactér** > **az Azure Functions és webes feladatok eszközök**  >  **Frissítés**.</span><span class="sxs-lookup"><span data-stu-id="c2da4-115">You can update hello tools from hello **Tools** menu under **Extensions and Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a><span data-ttu-id="c2da4-116">Hozzon létre egy Azure Functions-projektet a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="c2da4-116">Create an Azure Functions project in Visual Studio</span></span>

[!INCLUDE [Create a project using hello Azure Functions template](../../includes/functions-vstools-create.md)]

<span data-ttu-id="c2da4-117">Most, hogy a létrehozott hello projekt, az első függvényét is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="c2da4-117">Now that you have created hello project, you can create your first function.</span></span>

## <a name="create-hello-function"></a><span data-ttu-id="c2da4-118">Hello függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2da4-118">Create hello function</span></span>

1. <span data-ttu-id="c2da4-119">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza az **Add** (Hozzáadás)  > **New Item** (Új elem) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c2da4-119">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="c2da4-120">Válassza ki az **Azure Function** (Azure-függvény) elemet, majd kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="c2da4-120">Select **Azure Function** and click **Add**.</span></span>

2. <span data-ttu-id="c2da4-121">Válassza ki a **HttpTrigger** elemet, adja meg a **Function Name** (Függvénynév) értékét, az **Access Rights** (Hozzáférési jog) lehetőségnél válassza az **Anonymous** (Névtelen) elemet, majd kattintson a **Create** (Létrehozás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="c2da4-121">Select **HttpTrigger**, type a **Function Name**, select **Anonymous** for **Access Rights**, and click **Create**.</span></span> <span data-ttu-id="c2da4-122">bármely ügyfél HTTP-kérelem által létrehozott hello függvény érhető el.</span><span class="sxs-lookup"><span data-stu-id="c2da4-122">hello function created is accessed by an HTTP request from any client.</span></span> 

    ![Új Azure-függvény létrehozása](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    <span data-ttu-id="c2da4-124">A forráskód fájlja kerül tooyour projekt, amely tartalmaz egy osztály, amely megvalósítja a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="c2da4-124">A code file is added tooyour project that contains a class that implements your function code.</span></span> <span data-ttu-id="c2da4-125">Ez a kód egy sablon, amely fogad egy név-érték és vissza Echok alapul.</span><span class="sxs-lookup"><span data-stu-id="c2da4-125">This code is based on a template, which receives a name value and echos it back.</span></span> <span data-ttu-id="c2da4-126">Hello **függvénynév** attribútum állítja be a függvény hello neve.</span><span class="sxs-lookup"><span data-stu-id="c2da4-126">hello **FunctionName** attribute sets hello name of your function.</span></span> <span data-ttu-id="c2da4-127">Hello **HttpTrigger** attribútum, amely elindítja a hello függvény üdvözlőüzenetére jelzi.</span><span class="sxs-lookup"><span data-stu-id="c2da4-127">hello **HttpTrigger** attribute indicates hello message that triggers hello function.</span></span> 

    ![Függvény kódfájl](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

<span data-ttu-id="c2da4-129">Most, hogy már létrehozta a HTTP-triggerrel aktivált függvényt, tesztelheti a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c2da4-129">Now that you have created an HTTP-triggered function, you can test it on your local computer.</span></span>

## <a name="test-hello-function-locally"></a><span data-ttu-id="c2da4-130">Helyileg hello függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="c2da4-130">Test hello function locally</span></span>

<span data-ttu-id="c2da4-131">Az Azure Functions Core Tools lehetővé teszi Azure Functions-projektek helyi fejlesztői számítógépen való futtatását.</span><span class="sxs-lookup"><span data-stu-id="c2da4-131">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="c2da4-132">Ezek az eszközök hello első indításakor függvény Visual Studio felszólító tooinstall áll.</span><span class="sxs-lookup"><span data-stu-id="c2da4-132">You are prompted tooinstall these tools hello first time you start a function from Visual Studio.</span></span>  

1. <span data-ttu-id="c2da4-133">tootest a függvény, nyomja le az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="c2da4-133">tootest your function, press F5.</span></span> <span data-ttu-id="c2da4-134">Ha a rendszer kéri, fogadja el a Visual Studio toodownload hello kérelmet, és telepítse az Azure Functions mag (CLI) eszközök.</span><span class="sxs-lookup"><span data-stu-id="c2da4-134">If prompted, accept hello request from Visual Studio toodownload and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="c2da4-135">Szükség lehet a tooenable olyan érvényes tűzfalkivétel, hogy hello eszközök kezelni tud a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="c2da4-135">You may also need tooenable a firewall exception so that hello tools can handle HTTP requests.</span></span>

2. <span data-ttu-id="c2da4-136">Másolás hello URL-CÍMÉT a függvényt a hello Azure Functions futtatókörnyezettel kimeneti.</span><span class="sxs-lookup"><span data-stu-id="c2da4-136">Copy hello URL of your function from hello Azure Functions runtime output.</span></span>  

    ![Az Azure helyi futtatókörnyezete](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. <span data-ttu-id="c2da4-138">Hello URL-cím a HTTP-kérelmek hello illessze be a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="c2da4-138">Paste hello URL for hello HTTP request into your browser's address bar.</span></span> <span data-ttu-id="c2da4-139">Hello lekérdezési karakterlánc hozzáfűzése `&name=<yourname>` toothis URL-cím és hello kérelem végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c2da4-139">Append hello query string `&name=<yourname>` toothis URL and execute hello request.</span></span> <span data-ttu-id="c2da4-140">hello következő hello válasz hello böngésző toohello helyi GET kérelem hello függvény által visszaadott jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c2da4-140">hello following shows hello response in hello browser toohello local GET request returned by hello function:</span></span> 

    ![Függvény localhost válasz hello böngészőben](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. <span data-ttu-id="c2da4-142">hibakeresés, toostop kattintson hello **leállítása** hello Visual Studio gombjára.</span><span class="sxs-lookup"><span data-stu-id="c2da4-142">toostop debugging, click hello **Stop** button on hello Visual Studio toolbar.</span></span>

<span data-ttu-id="c2da4-143">Miután ellenőrizte, hogy a hello függvény megfelelően fut a helyi számítógépen, akkor idő toopublish hello projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c2da4-143">After you have verified that hello function runs correctly on your local computer, it's time toopublish hello project tooAzure.</span></span>

## <a name="publish-hello-project-tooazure"></a><span data-ttu-id="c2da4-144">Hello projekt tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="c2da4-144">Publish hello project tooAzure</span></span>

<span data-ttu-id="c2da4-145">A projekt közzétételéhez rendelkeznie kell egy függvényalkalmazással.az Azure-előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="c2da4-145">You must have a function app in your Azure subscription before you can publish your project.</span></span> <span data-ttu-id="c2da4-146">Közvetlenül a Visual Studióból is létrehozhat függvényalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2da4-146">You can create a function app right from Visual Studio.</span></span>

[!INCLUDE [Publish hello project tooAzure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a><span data-ttu-id="c2da4-147">A függvény tesztelése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c2da4-147">Test your function in Azure</span></span>

1. <span data-ttu-id="c2da4-148">Hello alap URL-Címének másolása hello függvény alkalmazás hello közzétételi profil oldalról.</span><span class="sxs-lookup"><span data-stu-id="c2da4-148">Copy hello base URL of hello function app from hello Publish profile page.</span></span> <span data-ttu-id="c2da4-149">Cserélje le a hello `localhost:port` hello hello függvény hello új alap URL-címet a helyi tesztelése során használt URL-cím része.</span><span class="sxs-lookup"><span data-stu-id="c2da4-149">Replace hello `localhost:port` portion of hello URL you used when testing hello function locally with hello new base URL.</span></span> <span data-ttu-id="c2da4-150">Mint korábban, győződjön meg arról, hogy tooappend hello lekérdezési karakterlánc `&name=<yourname>` toothis URL-cím és hello kérelem végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c2da4-150">As before, make sure tooappend hello query string `&name=<yourname>` toothis URL and execute hello request.</span></span>

    <span data-ttu-id="c2da4-151">hello URL-címet, amely behívja a HTTP függvény néz indított:</span><span class="sxs-lookup"><span data-stu-id="c2da4-151">hello URL that calls your HTTP triggered function looks like this:</span></span>

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. <span data-ttu-id="c2da4-152">Az új URL-cím hello HTTP-kérelem illessze be a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="c2da4-152">Paste this new URL for hello HTTP request into your browser's address bar.</span></span> <span data-ttu-id="c2da4-153">hello következő hello válasz hello böngésző toohello távoli GET kérelem hello függvény által visszaadott jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="c2da4-153">hello following shows hello response in hello browser toohello remote GET request returned by hello function:</span></span> 

    ![Függvény válasz hello böngészőben](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a><span data-ttu-id="c2da4-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2da4-155">Next steps</span></span>

<span data-ttu-id="c2da4-156">Visual Studio toocreate egy C# függvény alkalmazás használta egy egyszerű indított HTTP függvénnyel.</span><span class="sxs-lookup"><span data-stu-id="c2da4-156">You have used Visual Studio toocreate a C# function app with a simple HTTP triggered function.</span></span> 

+ <span data-ttu-id="c2da4-157">toolearn hogyan tooconfigure a projekt toosupport eseményindítók és kötések, más típusú, lásd: hello [konfigurálása hello projekt helyi fejlesztési](functions-develop-vs.md#configure-the-project-for-local-development) szakasz [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="c2da4-157">toolearn how tooconfigure your project toosupport other types of triggers and bindings, see hello [Configure hello project for local development](functions-develop-vs.md#configure-the-project-for-local-development) section in [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span></span>
+ <span data-ttu-id="c2da4-158">További információ a helyi tesztelés és hibakeresés hello Azure Functions alapvető eszközökkel toolearn lásd: [kódot és az Azure Functions tesztelése helyileg](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="c2da4-158">toolearn more about local testing and debugging using hello Azure Functions Core Tools, see [Code and test Azure Functions locally](functions-run-local.md).</span></span> 
+ <span data-ttu-id="c2da4-159">További információ az alkalmazás, mint funkciók fejlesztése toolearn lásd [használó alkalmazás az Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="c2da4-159">toolearn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> 

