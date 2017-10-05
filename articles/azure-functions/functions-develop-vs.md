---
title: "Visual Studio használatával Azure Functions kidolgozása |} Microsoft Docs"
description: "Megtudhatja, hogyan fejlesztéséhez és teszteléséhez az Azure Functions által az Azure Functions Tools for Visual Studio 2017 használatával."
services: functions
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: glenga
ms.openlocfilehash: 1e0568bc58e8879cabe409cf8e9b5866f922e7c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-tools-for-visual-studio"></a><span data-ttu-id="defd7-103">Az Azure Functions Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="defd7-103">Azure Functions Tools for Visual Studio</span></span>  

<span data-ttu-id="defd7-104">Az Azure funkciók Tools for Visual Studio 2017 bővítménye, amely lehetővé teszi a fejlesztés, tesztelése és C# funkciók telepítse az Azure Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="defd7-104">Azure Functions Tools for Visual Studio 2017 is an extension for Visual Studio that lets you develop, test, and deploy C# functions to Azure.</span></span> <span data-ttu-id="defd7-105">Ha ez az első tapasztalattal az Azure Functions, többet is megtudhat a [megismerkedhet az Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="defd7-105">If this is your first experience with Azure Functions, you can learn more at [An introduction to Azure Functions](functions-overview.md).</span></span>

<span data-ttu-id="defd7-106">Az Azure Functions eszközök a következő előnyöket nyújtja:</span><span class="sxs-lookup"><span data-stu-id="defd7-106">The Azure Functions Tools provides the following benefits:</span></span> 

* <span data-ttu-id="defd7-107">Szerkesztés, elkészítéséhez és funkciók a helyi fejlesztési számítógépen futtassa.</span><span class="sxs-lookup"><span data-stu-id="defd7-107">Edit, build, and run functions on your local development computer.</span></span> 
* <span data-ttu-id="defd7-108">Az Azure Functions projekt közzététele közvetlenül az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="defd7-108">Publish your Azure Functions project directly to Azure.</span></span> 
* <span data-ttu-id="defd7-109">WebJobs-attribútumok segítségével közvetlenül a C#-kódban helyett egy külön function.json a definíciók kötési fenntartása a függvénykötés deklarálható.</span><span class="sxs-lookup"><span data-stu-id="defd7-109">Use WebJobs attributes to declare function bindings directly in the C# code instead of maintaining a separate function.json for binding definitions.</span></span>
* <span data-ttu-id="defd7-110">Fejlesztésekor, és előre lefordított függvények C# telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="defd7-110">Develop and deploy pre-compiled C# functions.</span></span> <span data-ttu-id="defd7-111">Előre lefordított függvények teljesítményt lehet biztosítani a jobban cold indítási mint C# parancsfájlalapú funkciók.</span><span class="sxs-lookup"><span data-stu-id="defd7-111">Pre-complied functions provide a better cold-start performance than C# script-based functions.</span></span> 
* <span data-ttu-id="defd7-112">A funkciók a C# kód a Visual Studio fejlesztői előnyeit mindegyikével közben.</span><span class="sxs-lookup"><span data-stu-id="defd7-112">Code your functions in C# while having all of the benefits of Visual Studio development.</span></span> 

<span data-ttu-id="defd7-113">Ez a témakör bemutatja, hogyan a Azure Functions Tools for Visual Studio 2017 segítségével a C# funkciók fejlesztése.</span><span class="sxs-lookup"><span data-stu-id="defd7-113">This topic shows you how to use the Azure Functions Tools for Visual Studio 2017 to develop your functions in C#.</span></span> <span data-ttu-id="defd7-114">Azt is megtudhatja, hogyan a projekt közzététele az Azure-ba, mint a .NET-szerelvény.</span><span class="sxs-lookup"><span data-stu-id="defd7-114">You also learn how to publish your project to Azure as a .NET assembly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="defd7-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="defd7-115">Prerequisites</span></span>

<span data-ttu-id="defd7-116">Az Azure Functions eszközök tartalmazza az Azure fejlesztési munkaterhelését [Visual Studio 2017 verzió 15.3](https://www.visualstudio.com/vs/), vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="defd7-116">Azure Functions Tools is included in the Azure development workload of [Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/), or a later version.</span></span> <span data-ttu-id="defd7-117">Győződjön meg arról, a **Azure fejlesztési** munkaterhelés a Visual Studio 2017 15.3 verzió telepítése:</span><span class="sxs-lookup"><span data-stu-id="defd7-117">Make sure you include the **Azure development** workload in your Visual Studio 2017 version 15.3 installation:</span></span>

![Az Azure-fejlesztési számítási feladatot is tartalmazó Visual Studio 2017 telepítése](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<span data-ttu-id="defd7-119">Hozzon létre, és funkciók telepítése, akkor is szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="defd7-119">To create and deploy functions, you also need:</span></span>

* <span data-ttu-id="defd7-120">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="defd7-120">An active Azure subscription.</span></span> <span data-ttu-id="defd7-121">Ha nem rendelkezik Azure-előfizetéssel, [fiókok szabad](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="defd7-121">If you don't have an Azure subscription, [free accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) are available.</span></span>

* <span data-ttu-id="defd7-122">Egy Azure Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="defd7-122">An Azure Storage account.</span></span> <span data-ttu-id="defd7-123">A storage-fiók létrehozásához lásd: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="defd7-123">To create a storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
## <a name="create-an-azure-functions-project"></a><span data-ttu-id="defd7-124">Az Azure Functions projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="defd7-124">Create an Azure Functions project</span></span> 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a><span data-ttu-id="defd7-125">A helyi fejlesztési projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="defd7-125">Configure the project for local development</span></span>

<span data-ttu-id="defd7-126">Amikor létrehoz egy új projektet az Azure Functions sablonnal, kap egy üres C# projekt, amely a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="defd7-126">When you create a new project using the Azure Functions template, you get an empty C# project that contains the following files:</span></span>

* <span data-ttu-id="defd7-127">**Host.JSON**: lehetővé teszi a funkciók gazdagép konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="defd7-127">**host.json**: Lets you configure the Functions host.</span></span> <span data-ttu-id="defd7-128">Ezeket a beállításokat is alkalmazza, ha fut a helyi és az Azure-ban is.</span><span class="sxs-lookup"><span data-stu-id="defd7-128">These settings apply both when running locally and in Azure.</span></span> <span data-ttu-id="defd7-129">További információkért lásd: [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) áttekintésével foglalkozó cikkben.</span><span class="sxs-lookup"><span data-stu-id="defd7-129">For more information, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) reference article.</span></span>
    
* <span data-ttu-id="defd7-130">**Local.Settings.JSON**: funkciók a helyi futtatás során használt beállításokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="defd7-130">**local.settings.json**: Maintains settings used when running functions locally.</span></span> <span data-ttu-id="defd7-131">Ezek a beállítások nem használhatók az Azure-ban, azokat a [Azure Functions Core eszközök](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="defd7-131">These settings are not used by Azure, they are used by the [Azure Functions Core Tools](functions-run-local.md).</span></span> <span data-ttu-id="defd7-132">Ez a fájl használatával adja meg a beállításokat, például más Azure-szolgáltatásokhoz való kapcsolódási karakterláncokat.</span><span class="sxs-lookup"><span data-stu-id="defd7-132">Use this file to specify settings, such as connection strings to other Azure services.</span></span> <span data-ttu-id="defd7-133">Adja hozzá egy új kulccsal, hogy a **értékek** tömb minden egyes funkciók a projekt által igényelt kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="defd7-133">Add a new key to the **Values** array for each connection required by functions in your project.</span></span> <span data-ttu-id="defd7-134">További információkért lásd: [helyi beállításfájl](functions-run-local.md#local-settings-file) az Azure Functions Core eszközök a témakörben.</span><span class="sxs-lookup"><span data-stu-id="defd7-134">For more information, see [Local settings file](functions-run-local.md#local-settings-file) in the Azure Functions Core Tools topic.</span></span>

<span data-ttu-id="defd7-135">A Functions futtatókörnyezete belső egy Azure Storage-fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="defd7-135">The Functions runtime uses an Azure Storage account internally.</span></span> <span data-ttu-id="defd7-136">Összes indítás típusú HTTP- és webhookokkal, be kell állítani a **Values.AzureWebJobsStorage** kulcs egy érvényes Azure Storage-fiók kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="defd7-136">For all trigger types other than HTTP and webhooks, you must set the **Values.AzureWebJobsStorage** key to a valid Azure Storage account connection string.</span></span>

[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

 <span data-ttu-id="defd7-137">A tárolási fiók kapcsolati karakterlánc beállítása:</span><span class="sxs-lookup"><span data-stu-id="defd7-137">To set the storage account connection string:</span></span>

1. <span data-ttu-id="defd7-138">A Visual Studióban nyissa meg a **Cloud Explorer**, bontsa ki a **Tárfiók** > **a Tárfiók**, majd jelölje be **tulajdonságok**, és másolja a **elsődleges kapcsolódási karakterlánc** érték.</span><span class="sxs-lookup"><span data-stu-id="defd7-138">In Visual Studio, open **Cloud Explorer**, expand **Storage Account** > **Your Storage Account**, then select **Properties** and copy the **Primary Connection String** value.</span></span>   

2. <span data-ttu-id="defd7-139">A projektben nyissa meg a local.settings.json projektfájlt, és állítsa a **AzureWebJobsStorage** kulcs a kapcsolati karakterlánc módosításait másolta.</span><span class="sxs-lookup"><span data-stu-id="defd7-139">In your project, open the local.settings.json project file and set the value of the **AzureWebJobsStorage** key to the connection string you copied.</span></span>

3. <span data-ttu-id="defd7-140">Az előző lépésben egyedi kulccsal hozzáadásához ismételje meg a **értékek** bármely más, a funkciók által igényelt kapcsolatok tömb.</span><span class="sxs-lookup"><span data-stu-id="defd7-140">Repeat the previous step to add unique keys to the **Values** array for any other connections required by your functions.</span></span>  

## <a name="create-a-function"></a><span data-ttu-id="defd7-141">Függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="defd7-141">Create a function</span></span>

<span data-ttu-id="defd7-142">Az előre lefordított függvények a függvény által használt kötéseket határozzák meg a kódban attribútumok alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="defd7-142">In pre-compiled functions, the bindings used by the function are defined by applying attributes in the code.</span></span> <span data-ttu-id="defd7-143">A függvény a megadott felügyeleticsomag-sablonok létrehozásához használhatja az Azure Functions eszközök, ezek az attribútumok meg válnak érvényessé.</span><span class="sxs-lookup"><span data-stu-id="defd7-143">When you use the Azure Functions Tools to create your functions from the provided templates, these attributes are applied for you.</span></span> 

1. <span data-ttu-id="defd7-144">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza az **Add** (Hozzáadás)  > **New Item** (Új elem) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="defd7-144">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="defd7-145">Válassza ki **Azure-függvény**, adjon meg egy **neve** az osztályt, majd kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="defd7-145">Select **Azure Function**, type a **Name** for the class, and click **Add**.</span></span>

2. <span data-ttu-id="defd7-146">Válassza ki az eseményindító, állítsa be a kötési tulajdonságok, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="defd7-146">Choose your trigger, set the binding properties, and click **Create**.</span></span> <span data-ttu-id="defd7-147">A következő példa bemutatja a beállításokat, kiváltásakor a várólista-tároló létrehozása funkciót.</span><span class="sxs-lookup"><span data-stu-id="defd7-147">The following example shows the settings when creating a Queue storage triggered function.</span></span> 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    <span data-ttu-id="defd7-148">Nevű kapcsolati karakterlánc kulcs **QueueStorage** van megadva, amely a local.settings.json fájlban van megadva.</span><span class="sxs-lookup"><span data-stu-id="defd7-148">A connection string key named **QueueStorage** is supplied, which is defined in the local.settings.json file.</span></span> 
 
3. <span data-ttu-id="defd7-149">Vizsgálja meg az újonnan hozzáadott osztály.</span><span class="sxs-lookup"><span data-stu-id="defd7-149">Examine the newly added class.</span></span> <span data-ttu-id="defd7-150">Megjelenik a statikus **futtatása** van módszerhez a **függvénynév** attribútum.</span><span class="sxs-lookup"><span data-stu-id="defd7-150">You see a static **Run** method, that is attributed with the **FunctionName** attribute.</span></span> <span data-ttu-id="defd7-151">Ez az attribútum azt jelzi, hogy a metódus a belépési pont, a függvény.</span><span class="sxs-lookup"><span data-stu-id="defd7-151">This attribute indicates that the method is the entry point for the function.</span></span> 

    <span data-ttu-id="defd7-152">A következő C# osztály például egy alapszintű várólista indított tárolási függvény jelöli:</span><span class="sxs-lookup"><span data-stu-id="defd7-152">For example, the following C# class represents a basic Queue storage triggered function:</span></span>

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    <span data-ttu-id="defd7-153">Minden kötelező paraméter, a belépési pont metódus számára megadott alkalmazva van egy kötése-specifikus attribútum.</span><span class="sxs-lookup"><span data-stu-id="defd7-153">A binding-specific attribute is applied to each binding parameter supplied to the entry point method.</span></span> <span data-ttu-id="defd7-154">Az attribútum a kötési információ paraméterekként vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="defd7-154">The attribute takes the binding information as parameters.</span></span> <span data-ttu-id="defd7-155">Az előző példában az első paraméter tartozik egy **QueueTrigger** attribútuma, várólista indított függvény jelző.</span><span class="sxs-lookup"><span data-stu-id="defd7-155">In the previous example, The first parameter has a **QueueTrigger** attribute applied, indicating queue triggered function.</span></span> <span data-ttu-id="defd7-156">A várólista nevét és a kapcsolati karakterlánc Beállításnév paraméterként.</span><span class="sxs-lookup"><span data-stu-id="defd7-156">The queue name and connection string setting name are passed as parameters.</span></span>  

## <a name="testing-functions"></a><span data-ttu-id="defd7-157">Függvények tesztelése</span><span class="sxs-lookup"><span data-stu-id="defd7-157">Testing functions</span></span>

<span data-ttu-id="defd7-158">Az Azure Functions Core Tools lehetővé teszi Azure Functions-projektek helyi fejlesztői számítógépen való futtatását.</span><span class="sxs-lookup"><span data-stu-id="defd7-158">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="defd7-159">Amikor a Visual Studióból először indít el egy függvényt, a rendszer arra kéri, hogy telepítse ezeket az eszközöket.</span><span class="sxs-lookup"><span data-stu-id="defd7-159">You are prompted to install these tools the first time you start a function from Visual Studio.</span></span>  

<span data-ttu-id="defd7-160">A függvény teszteléséhez nyomja le az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="defd7-160">To test your function, press F5.</span></span> <span data-ttu-id="defd7-161">Ha a rendszer kéri, fogadja el a Visual Studio kérését az Azure Functions Core (CLI) eszközök telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="defd7-161">If prompted, accept the request from Visual Studio to download and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="defd7-162">Lehet, hogy egy tűzfalkivételt is engedélyeznie kell, hogy az eszközök kezelhessék a HTTP-kéréseket.</span><span class="sxs-lookup"><span data-stu-id="defd7-162">You may also need to enable a firewall exception so that the tools can handle HTTP requests.</span></span>

<span data-ttu-id="defd7-163">A projekt fut tesztelheti a kódot, érdemes tesztelni telepített függvény.</span><span class="sxs-lookup"><span data-stu-id="defd7-163">With the project running, you can test your code as you would test deployed function.</span></span> <span data-ttu-id="defd7-164">További információkért lásd: [a kódot az Azure Functions tesztelése kapcsolatos olyan stratégiák](functions-test-a-function.md).</span><span class="sxs-lookup"><span data-stu-id="defd7-164">For more information, see [Strategies for testing your code in Azure Functions](functions-test-a-function.md).</span></span> <span data-ttu-id="defd7-165">Hibakeresési módban fut, töréspontok az elvárt módon vannak elérte a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="defd7-165">When running in debug mode, breakpoints are hit in Visual Studio as expected.</span></span> 

<span data-ttu-id="defd7-166">A várólista indított függvény tesztelése példáért lásd: a [indított várólista függvény gyors üzembe helyezési útmutató](functions-create-storage-queue-triggered-function.md#test-the-function).</span><span class="sxs-lookup"><span data-stu-id="defd7-166">For an example of how to test a queue triggered function, see the [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).</span></span>  

<span data-ttu-id="defd7-167">Az Azure Functions Core eszközök használatával kapcsolatos további tudnivalókért lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="defd7-167">To learn more about using the Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>

## <a name="publish-to-azure"></a><span data-ttu-id="defd7-168">Közzététel az Azure platformon</span><span class="sxs-lookup"><span data-stu-id="defd7-168">Publish to Azure</span></span>

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
><span data-ttu-id="defd7-169">Minden hozzáadott a local.settings.json beállítást is meg kell adni a függvény alkalmazásba az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="defd7-169">Any settings you added in the local.settings.json must be also added to the function app in Azure.</span></span> <span data-ttu-id="defd7-170">Ezek a beállítások nem kerülnek be automatikusan.</span><span class="sxs-lookup"><span data-stu-id="defd7-170">These settings are not added automatically.</span></span> <span data-ttu-id="defd7-171">Kötelező beállítások adhat hozzá az függvény alkalmazásban az alábbi módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="defd7-171">You can add required settings to your function app in one of these ways:</span></span>
>
>* <span data-ttu-id="defd7-172">[Az Azure portál használatával](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="defd7-172">[Using the Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>
>* <span data-ttu-id="defd7-173">[Használja a `--publish-local-settings` publish beállítást, az Azure Functions Core eszközök](functions-run-local.md#publish).</span><span class="sxs-lookup"><span data-stu-id="defd7-173">[Using the `--publish-local-settings` publish option in the Azure Functions Core Tools](functions-run-local.md#publish).</span></span>
>* <span data-ttu-id="defd7-174">[Az Azure parancssori felület használatával](/cli/azure/functionapp/config/appsettings#set).</span><span class="sxs-lookup"><span data-stu-id="defd7-174">[Using the Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="defd7-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="defd7-175">Next steps</span></span>

<span data-ttu-id="defd7-176">További információ az Azure Functions eszközök című rész a gyakori kérdéseket a [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="defd7-176">For more information about Azure Functions Tools, see the Common Questions section of the [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog post.</span></span>

<span data-ttu-id="defd7-177">Az Azure Functions alapvető eszközökkel kapcsolatos további tudnivalókért lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="defd7-177">To learn more about the Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>  
<span data-ttu-id="defd7-178">A függvények .NET-osztálytárakként való fejlesztéséről további információért lásd [a .NET-osztálytárak és az Azure Functions használatát](functions-dotnet-class-library.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="defd7-178">To learn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> <span data-ttu-id="defd7-179">Ez a témakör is példákat attribútumok használata a különféle az Azure Functions által támogatott kötések deklarálnia.</span><span class="sxs-lookup"><span data-stu-id="defd7-179">This topic also provides examples of how to use attributes to declare the various types of bindings supported by Azure Functions.</span></span>    
