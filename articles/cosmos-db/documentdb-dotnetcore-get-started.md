---
title: "Azure Cosmos DB: DocumentDB API bevezetés .NET Core oktatóanyaggal | Microsoft Docs"
description: "Ez az oktatóanyag létrehoz egy online adatbázist és a C# konzolalkalmazást hello Azure Cosmos DB DocumentDB API .NET Core SDK használatával."
services: cosmos-db
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 5e7efb327252e9e73e9b2a340820eeecc6937fa5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-getting-started-with-hello-documentdb-api-and-net-core"></a><span data-ttu-id="361c7-103">Az Azure Cosmos DB: Hello DocumentDB API és a .NET Core első lépések</span><span class="sxs-lookup"><span data-stu-id="361c7-103">Azure Cosmos DB: Getting started with hello DocumentDB API and .NET Core</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="361c7-104">.NET</span><span class="sxs-lookup"><span data-stu-id="361c7-104">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="361c7-105">.NET Core</span><span class="sxs-lookup"><span data-stu-id="361c7-105">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="361c7-106">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="361c7-106">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="361c7-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="361c7-107">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="361c7-108">Java</span><span class="sxs-lookup"><span data-stu-id="361c7-108">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="361c7-109">C++</span><span class="sxs-lookup"><span data-stu-id="361c7-109">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="361c7-110">A DocumentDB API toohello Azure Cosmos DB Ismerkedés a .NET Core-oktatóanyag – Üdvözöljük!</span><span class="sxs-lookup"><span data-stu-id="361c7-110">Welcome toohello DocumentDB API for Azure Cosmos DB getting started with .NET Core tutorial!</span></span> <span data-ttu-id="361c7-111">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="361c7-111">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="361c7-112">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="361c7-112">We'll cover:</span></span>

* <span data-ttu-id="361c7-113">Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="361c7-113">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="361c7-114">A Visual Studio megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="361c7-114">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="361c7-115">Online adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-115">Creating an online database</span></span>
* <span data-ttu-id="361c7-116">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-116">Creating a collection</span></span>
* <span data-ttu-id="361c7-117">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-117">Creating JSON documents</span></span>
* <span data-ttu-id="361c7-118">Hello gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="361c7-118">Querying hello collection</span></span>
* <span data-ttu-id="361c7-119">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="361c7-119">Replacing a document</span></span>
* <span data-ttu-id="361c7-120">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="361c7-120">Deleting a document</span></span>
* <span data-ttu-id="361c7-121">Hello adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="361c7-121">Deleting hello database</span></span>

<span data-ttu-id="361c7-122">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="361c7-122">Don't have time?</span></span> <span data-ttu-id="361c7-123">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="361c7-123">Don't worry!</span></span> <span data-ttu-id="361c7-124">a teljes megoldás hello érhető [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span><span class="sxs-lookup"><span data-stu-id="361c7-124">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span> <span data-ttu-id="361c7-125">Ugrás a toohello [hello teljes megoldás szakaszának beolvasása](#GetSolution) gyors utasításokért.</span><span class="sxs-lookup"><span data-stu-id="361c7-125">Jump toohello [Get hello complete solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="361c7-126">Toobuild szeretné, hogy a Xamarin iOS, Android vagy űrlapok DocumentDB API és a .NET Core SDK használata alkalmazás hello?</span><span class="sxs-lookup"><span data-stu-id="361c7-126">Want toobuild a Xamarin iOS, Android, or Forms application using hello DocumentDB API and .NET Core SDK?</span></span> <span data-ttu-id="361c7-127">Lásd: [használó alkalmazások mobil a Xamarinnal és Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-127">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>

> [!NOTE]
> <span data-ttu-id="361c7-128">hello Azure Cosmos DB .NET Core SDK ebben az oktatóanyagban használt még nem kompatibilis az univerzális Windows Platform (UWP-) alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="361c7-128">hello Azure Cosmos DB .NET Core SDK used in this tutorial is not yet compatible with Universal Windows Platform (UWP) apps.</span></span> <span data-ttu-id="361c7-129">A .NET Core SDK-t támogató UWP-alkalmazások hello előzetes verzióját, e-mailek küldése túl[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="361c7-129">For a preview version of hello .NET Core SDK that does support UWP apps, send email too[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).</span></span>

<span data-ttu-id="361c7-130">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="361c7-130">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="361c7-131">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="361c7-131">Prerequisites</span></span>
<span data-ttu-id="361c7-132">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="361c7-132">Please make sure you have hello following:</span></span>

* <span data-ttu-id="361c7-133">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="361c7-133">An active Azure account.</span></span> <span data-ttu-id="361c7-134">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="361c7-134">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="361c7-135">Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="361c7-135">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="361c7-136">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="361c7-136">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/) 
    * <span data-ttu-id="361c7-137">Ha MacOS vagy Linux dolgozik, parancssori hello .NET Core alkalmazásokat fejleszthet hello telepítésével [.NET Core SDK](https://www.microsoft.com/net/core#macos) hello platform az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="361c7-137">If you're working on MacOS or Linux, you can develop .NET Core apps from hello command-line by installing hello [.NET Core SDK](https://www.microsoft.com/net/core#macos) for hello platform of your choice.</span></span> 
    * <span data-ttu-id="361c7-138">Ha a Windows dolgozik, parancssori hello .NET Core alkalmazásokat fejleszthet hello telepítésével [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span><span class="sxs-lookup"><span data-stu-id="361c7-138">If you're working on Windows, you can develop .NET Core apps from hello command-line by installing hello [.NET Core SDK](https://www.microsoft.com/net/core#windows).</span></span> 
    * <span data-ttu-id="361c7-139">Használhat saját szerkesztőt is, vagy letöltheti az ingyenes [Visual Studio Code](https://code.visualstudio.com/) alkalmazást, amely Windows, Linux és MacOS rendszeren egyaránt működik.</span><span class="sxs-lookup"><span data-stu-id="361c7-139">You can use your own editor, or download [Visual Studio Code](https://code.visualstudio.com/) which is free and works on Windows, Linux, and MacOS.</span></span> 

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="361c7-140">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-140">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="361c7-141">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="361c7-141">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="361c7-142">Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="361c7-142">If you already have an account you want toouse, you can skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="361c7-143">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="361c7-143">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="361c7-144"><a id="SetupVS"></a>2. lépés: A Visual Studio megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="361c7-144"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="361c7-145">Nyissa meg a **Visual Studio 2017-et** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="361c7-145">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="361c7-146">A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="361c7-146">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="361c7-147">A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **.NET Core** / **Console Application (a .NET Core)**, a projekt elnevezése **DocumentDBGettingStarted**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="361c7-147">In hello **New Project** dialog, select **Templates** / **Visual C#** / **.NET Core**/**Console Application (.NET Core)**, name your project **DocumentDBGettingStarted**, and then click **OK**.</span></span>

   ![Képernyőfelvétel a hello új projekt ablakról](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. <span data-ttu-id="361c7-149">A hello **Megoldáskezelőben**, kattintson a jobb gombbal **DocumentDBGettingStarted**.</span><span class="sxs-lookup"><span data-stu-id="361c7-149">In hello **Solution Explorer**, right click on **DocumentDBGettingStarted**.</span></span>
5. <span data-ttu-id="361c7-150">Ne maradjanak hello menüben, kattintson a **NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="361c7-150">Then without leaving hello menu, click on **Manage NuGet Packages...**.</span></span>

   ![Képernyőfelvétel a hello jobbra a hello projekt helyi](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)
6. <span data-ttu-id="361c7-152">A hello **NuGet** lapra, majd **Tallózás** hello és írja be a hello tetején **az azure documentdb** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="361c7-152">In hello **NuGet** tab, click **Browse** at hello top of hello window, and type **azure documentdb** in hello search box.</span></span>
7. <span data-ttu-id="361c7-153">Hello eredmények belül található **Microsoft.Azure.DocumentDB.Core** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="361c7-153">Within hello results, find **Microsoft.Azure.DocumentDB.Core** and click **Install**.</span></span>
   <span data-ttu-id="361c7-154">a DocumentDB ügyféloldali kódtára a .NET Core hello hello csomag azonosítója [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span><span class="sxs-lookup"><span data-stu-id="361c7-154">hello package ID for hello DocumentDB Client Library for .NET Core is [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core).</span></span> <span data-ttu-id="361c7-155">Ha a .NET-keretrendszer olyan verzióját célozza meg (például net461), amelyet ez a .NET Core NuGet csomag nem támogat, használja a [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)-t, amely a .NET-keretrendszer 4.5-ös verziójától kezdődően minden verziót támogat.</span><span class="sxs-lookup"><span data-stu-id="361c7-155">If you are targeting a .NET Framework version (like net461) that is not supported by this .NET Core NuGet package, then use [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) that supports all .NET Framework versions starting .NET Framework 4.5.</span></span>
8. <span data-ttu-id="361c7-156">Hello kér, fogadja el a hello NuGet csomag telepítése és hello licencszerződést.</span><span class="sxs-lookup"><span data-stu-id="361c7-156">At hello prompts, accept hello NuGet package installations and hello license agreement.</span></span>

<span data-ttu-id="361c7-157">Remek!</span><span class="sxs-lookup"><span data-stu-id="361c7-157">Great!</span></span> <span data-ttu-id="361c7-158">Most, hogy befejeztük hello beállítása, először néhány kódot ír.</span><span class="sxs-lookup"><span data-stu-id="361c7-158">Now that we finished hello setup, let's start writing some code.</span></span> <span data-ttu-id="361c7-159">A [GitHubon](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) megtalálhatja az oktatóanyagban szereplő kódprojekt befejezett változatát.</span><span class="sxs-lookup"><span data-stu-id="361c7-159">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).</span></span>

## <span data-ttu-id="361c7-160"><a id="Connect"></a>3. lépés: Csatlakozás tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="361c7-160"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="361c7-161">Először adja hozzá ezeket a C#-alkalmazás, hello Program.cs fájl elejéhez toohello hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="361c7-161">First, add these references toohello beginning of your C# application, in hello Program.cs file:</span></span>

```csharp
using System;

// ADD THIS PART tooYOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

> [!IMPORTANT]
> <span data-ttu-id="361c7-162">A rendezés toocomplete ebben az oktatóanyagban, mindenképpen adja hozzá a hello függőségeket.</span><span class="sxs-lookup"><span data-stu-id="361c7-162">In order toocomplete this tutorial, make sure you add hello dependencies above.</span></span>

<span data-ttu-id="361c7-163">Most adja hozzá ezt a két állandót és az *ügyfél* változót a *Program* nyilvános osztály alatt.</span><span class="sxs-lookup"><span data-stu-id="361c7-163">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

```csharp
class Program
{
    // ADD THIS PART tooYOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

<span data-ttu-id="361c7-164">A következő head toohello [Azure Portal](https://portal.azure.com) tooretrieve URI és elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="361c7-164">Next, head toohello [Azure Portal](https://portal.azure.com) tooretrieve your URI and primary key.</span></span> <span data-ttu-id="361c7-165">hello Azure Cosmos DB URI és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="361c7-165">hello Azure Cosmos DB URI and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="361c7-166">A hello Azure portál, keresse meg a tooyour Azure Cosmos DB fiókot, és kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="361c7-166">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="361c7-167">Hello portálról hello URI másolja és illessze be azt `<your endpoint URI>` hello program.cs fájlban.</span><span class="sxs-lookup"><span data-stu-id="361c7-167">Copy hello URI from hello portal and paste it into `<your endpoint URI>` in hello program.cs file.</span></span> <span data-ttu-id="361c7-168">Ezután másolási elsődleges kulcs hello hello portálról, és illessze be azt `<your key>`.</span><span class="sxs-lookup"><span data-stu-id="361c7-168">Then copy hello PRIMARY KEY from hello portal and paste it into `<your key>`.</span></span> <span data-ttu-id="361c7-169">Hello Azure Cosmos DB emulátor használata `https://localhost:8081` hello végpont és hello jól meghatározott hitelesítési kulcsát, [hogyan toodevelop használatával hello Azure Cosmos DB emulátor](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-169">If you are using hello Azure Cosmos DB Emulator, use `https://localhost:8081` as hello endpoint, and hello well-defined authorization key from [How toodevelop using hello Azure Cosmos DB Emulator](local-emulator.md).</span></span> <span data-ttu-id="361c7-170">Győződjön meg arról, hogy tooremove hello < és >, de hagyjon hello a végpont és a kulcs dupla idézőjelbe.</span><span class="sxs-lookup"><span data-stu-id="361c7-170">Make sure tooremove hello < and > but leave hello double quotes around your endpoint and key.</span></span>

![Képernyőfelvétel a hello hello NoSQL-oktatóanyag toocreate a C# Konzolalkalmazás által használt Azure-portálról.][keys]

<span data-ttu-id="361c7-173">Hello első lépések alkalmazáshoz hozzon létre egy új példányát hello először foglalkozunk **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="361c7-173">We'll start hello getting started application by creating a new instance of hello **DocumentClient**.</span></span>

<span data-ttu-id="361c7-174">Alább hello **fő** módszer, vegye fel a elnevezésű új aszinkron feladatot **GetStartedDemo**, amely létrehozza nekünk az új **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="361c7-174">Below hello **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART tooYOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

<span data-ttu-id="361c7-175">Hello következő kódot toorun az aszinkron feladat hozzáadása a **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-175">Add hello following code toorun your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="361c7-176">Hello **fő** metódus kivételeket, és azt toohello konzol írja azokat.</span><span class="sxs-lookup"><span data-stu-id="361c7-176">hello **Main** method will catch exceptions and write them toohello console.</span></span>

```csharp
static void Main(string[] args)
{
        // ADD THIS PART tooYOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key tooexit.");
                Console.ReadKey();
        }
```

<span data-ttu-id="361c7-177">Nyomja le az hello **DocumentDBGettingStarted** toobuild gombra, és hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="361c7-177">Press hello **DocumentDBGettingStarted** button toobuild and run hello application.</span></span>

<span data-ttu-id="361c7-178">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-178">Congratulations!</span></span> <span data-ttu-id="361c7-179">Sikeresen csatlakozott tooan Azure Cosmos DB fiók, most vessünk egy pillantást a Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="361c7-179">You have successfully connected tooan Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="361c7-180">4. lépés: Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-180">Step 4: Create a database</span></span>
<span data-ttu-id="361c7-181">Az adatbázis létrehozásához hello kód hozzáadása előtt adjon hozzá egy segédmetódust toohello konzol írásához.</span><span class="sxs-lookup"><span data-stu-id="361c7-181">Before you add hello code for creating a database, add a helper method for writing toohello console.</span></span>

<span data-ttu-id="361c7-182">Másolja és illessze be a hello **WriteToConsoleAndPromptToContinue** hello metódust **GetStartedDemo** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-182">Copy and paste hello **WriteToConsoleAndPromptToContinue** method underneath hello **GetStartedDemo** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="361c7-183">Az Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="361c7-183">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="361c7-184">Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.</span><span class="sxs-lookup"><span data-stu-id="361c7-184">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="361c7-185">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello ügyfél létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="361c7-185">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello client creation.</span></span> <span data-ttu-id="361c7-186">Ezzel létrehoz egy *FamilyDB* elnevezésű adatbázist.</span><span class="sxs-lookup"><span data-stu-id="361c7-186">This will create a database named *FamilyDB*.</span></span>

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

<span data-ttu-id="361c7-187">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-187">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-188">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-188">Congratulations!</span></span> <span data-ttu-id="361c7-189">Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="361c7-189">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="361c7-190"><a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-190"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="361c7-191">A **CreateDocumentCollectionAsync** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="361c7-191">**CreateDocumentCollectionAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="361c7-192">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="361c7-192">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>

<span data-ttu-id="361c7-193">A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="361c7-193">A [collection](documentdb-resources.md#collections) can be created by using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="361c7-194">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="361c7-194">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="361c7-195">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello adatbázis létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="361c7-195">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello database creation.</span></span> <span data-ttu-id="361c7-196">Ezzel létrehoz egy *FamilyCollection_oa* elnevezésű dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="361c7-196">This will create a document collection named *FamilyCollection_oa*.</span></span>

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExists("FamilyDB_oa");

    // ADD THIS PART tooYOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

<span data-ttu-id="361c7-197">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-197">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-198">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-198">Congratulations!</span></span> <span data-ttu-id="361c7-199">Sikeresen létrehozott egy Azure Cosmos DB-dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="361c7-199">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="361c7-200"><a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="361c7-200"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="361c7-201">A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="361c7-201">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="361c7-202">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="361c7-202">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="361c7-203">Most már beilleszthetünk egy vagy több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="361c7-203">We can now insert one or more documents.</span></span> <span data-ttu-id="361c7-204">Ha van olyan adat, milyen toostore az adatbázisban, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-204">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md).</span></span>

<span data-ttu-id="361c7-205">Először létre kell toocreate egy **termékcsalád** osztály, amely a mintában Azure Cosmos DB tárolt objektumokat képviseli.</span><span class="sxs-lookup"><span data-stu-id="361c7-205">First, we need toocreate a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="361c7-206">Létrehozunk még egy **Szülő**, **Gyermek**, **Háziállat** és **Cím** alosztályt is a **Család** osztályban való használatra.</span><span class="sxs-lookup"><span data-stu-id="361c7-206">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="361c7-207">Ne feledje, hogy a dokumentumoknak rendelkezniük kell egy **Azonosító** tulajdonsággal, amely a JSON-fájlban **id**-ként van szerializálva.</span><span class="sxs-lookup"><span data-stu-id="361c7-207">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="361c7-208">Az osztályok létrehozásához adja hozzá a következő belső alosztályok után hello hello **GetStartedDemo** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-208">Create these classes by adding hello following internal sub-classes after hello **GetStartedDemo** method.</span></span>

<span data-ttu-id="361c7-209">Másolja és illessze be a hello **termékcsalád**, **szülő**, **gyermek**, **háziállat**, és **cím** osztályokat Hello **WriteToConsoleAndPromptToContinue** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-209">Copy and paste hello **Family**, **Parent**, **Child**, **Pet**, and **Address** classes underneath hello **WriteToConsoleAndPromptToContinue** method.</span></span>

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key toocontinue ...");
    Console.ReadKey();
}

// ADD THIS PART tooYOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="361c7-210">Másolja és illessze be a hello **CreateFamilyDocumentIfNotExists** metódust a **CreateDocumentCollectionIfNotExists** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-210">Copy and paste hello **CreateFamilyDocumentIfNotExists** method underneath your **CreateDocumentCollectionIfNotExists** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

<span data-ttu-id="361c7-211">Majd szúrja be két dokumentumot, egyet mindegyik hello Andersen családhoz, majd hello Wakefield családhoz.</span><span class="sxs-lookup"><span data-stu-id="361c7-211">And insert two documents, one each for hello Andersen Family and hello Wakefield Family.</span></span>

<span data-ttu-id="361c7-212">Másolja és illessze be a következő hello kód `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** metódusba hello dokumentumgyűjtemény létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="361c7-212">Copy and paste hello code that follows `// ADD THIS PART tooYOUR CODE` tooyour **GetStartedDemo** method underneath hello document collection creation.</span></span>

```csharp
await this.CreateDatabaseIfNotExists("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

<span data-ttu-id="361c7-213">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-213">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-214">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-214">Congratulations!</span></span> <span data-ttu-id="361c7-215">Sikeresen létrehozott két Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="361c7-215">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram szemléltető hello hierarchikus kapcsolatát hello fiók hello online adatbázis, gyűjtemény hello és hello NoSQL-oktatóanyag toocreate által használt hello dokumentumok a C# Konzolalkalmazás](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="361c7-217"><a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="361c7-217"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="361c7-218">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-218">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="361c7-219">hello következő mintakód bemutatja, több olyan lekérdezést, mert mindkét Azure Cosmos DB SQL használatával szintaxisát, valamint a LINQ -, amely azt is futtathatók hello azt hello előző lépésben beszúrt dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="361c7-219">hello following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against hello documents we inserted in hello previous step.</span></span>

<span data-ttu-id="361c7-220">Másolja és illessze be a hello **ExecuteSimpleQuery** metódust a **CreateFamilyDocumentIfNotExists** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-220">Copy and paste hello **ExecuteSimpleQuery** method underneath your **CreateFamilyDocumentIfNotExists** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find hello Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // hello query is executed synchronously here, but can also be executed asynchronously via hello IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute hello same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key toocontinue ...");
        Console.ReadKey();
}
```

<span data-ttu-id="361c7-221">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello második dokumentum létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="361c7-221">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello second document creation.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART tooYOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="361c7-222">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-222">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-223">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-223">Congratulations!</span></span> <span data-ttu-id="361c7-224">Sikeres lekérdezést végzett egy Azure Cosmos DB-gyűjteményen.</span><span class="sxs-lookup"><span data-stu-id="361c7-224">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="361c7-225">hello következő diagram azt ábrázolja, hogyan hello Azure Cosmos adatbázis SQL-lekérdezés szintaxisa hívást hello gyűjtemény létrehozása, és hello ugyanez a logika vonatkozik toohello LINQ-lekérdezésekre is.</span><span class="sxs-lookup"><span data-stu-id="361c7-225">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created, and hello same logic applies toohello LINQ query as well.</span></span>

![A C# Konzolalkalmazás hello NoSQL-oktatóanyag toocreate használja és hello lekérdezés jelentését hello hatókör ábrázoló diagram](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="361c7-227">Hello [FROM](documentdb-sql-query.md#FromClause) kulcsszó nem kötelező, hello lekérdezésben, mert Azure Cosmos DB lekérdezések már hatókörön belüli tooa egyetlen gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="361c7-227">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="361c7-228">Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre.</span><span class="sxs-lookup"><span data-stu-id="361c7-228">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="361c7-229">A DocumentDB következtethető ki, hogy családok, legfelső szintű vagy hello változó neve választja, hivatkozás hello aktuális gyűjtemény alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="361c7-229">DocumentDB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

## <span data-ttu-id="361c7-230"><a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje</span><span class="sxs-lookup"><span data-stu-id="361c7-230"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="361c7-231">Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="361c7-231">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="361c7-232">Másolja és illessze be a hello **ReplaceFamilyDocument** metódust a **ExecuteSimpleQuery** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-232">Copy and paste hello **ReplaceFamilyDocument** method underneath your **ExecuteSimpleQuery** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    try
    {
        await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
        this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

<span data-ttu-id="361c7-233">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello lekérdezés végrehajtása alá.</span><span class="sxs-lookup"><span data-stu-id="361c7-233">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello query execution.</span></span> <span data-ttu-id="361c7-234">Miután kicserélte hello dokumentum, ez fog futni hello azonos újra lekérdezése tooview hello megváltozott dokumentum.</span><span class="sxs-lookup"><span data-stu-id="361c7-234">After replacing hello document, this will run hello same query again tooview hello changed document.</span></span>

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooYOUR CODE
// Update hello Grade of hello Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

<span data-ttu-id="361c7-235">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-235">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-236">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-236">Congratulations!</span></span> <span data-ttu-id="361c7-237">Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="361c7-237">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="361c7-238"><a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="361c7-238"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="361c7-239">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.</span><span class="sxs-lookup"><span data-stu-id="361c7-239">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="361c7-240">Másolja és illessze be a hello **DeleteFamilyDocument** metódust a **ReplaceFamilyDocument** metódust.</span><span class="sxs-lookup"><span data-stu-id="361c7-240">Copy and paste hello **DeleteFamilyDocument** method underneath your **ReplaceFamilyDocument** method.</span></span>

```csharp
// ADD THIS PART tooYOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    try
    {
        await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine("Deleted Family {0}", documentName);
    }
    catch (DocumentClientException de)
    {
        throw;
    }
}
```

<span data-ttu-id="361c7-241">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello második lekérdezés végrehajtása alá.</span><span class="sxs-lookup"><span data-stu-id="361c7-241">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello second query execution.</span></span>

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART tooCODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

<span data-ttu-id="361c7-242">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-242">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-243">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-243">Congratulations!</span></span> <span data-ttu-id="361c7-244">Sikeresen törölt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="361c7-244">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="361c7-245"><a id="DeleteDatabase"></a>10. lépés: Hello adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="361c7-245"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="361c7-246">A létrehozott adatbázis törlésével hello eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).</span><span class="sxs-lookup"><span data-stu-id="361c7-246">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="361c7-247">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** hello dokumentum metódust toodelete hello adatbázis és az összes gyermek-erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="361c7-247">Copy and paste hello following code tooyour **GetStartedDemo** method underneath hello document delete toodelete hello entire database and all children resources.</span></span>

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART tooCODE
// Clean up/delete hello database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

<span data-ttu-id="361c7-248">Nyomja le az hello **DocumentDBGettingStarted** toorun gombra az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="361c7-248">Press hello **DocumentDBGettingStarted** button toorun your application.</span></span>

<span data-ttu-id="361c7-249">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-249">Congratulations!</span></span> <span data-ttu-id="361c7-250">Sikeresen törölt egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="361c7-250">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="361c7-251"><a id="Run"></a>11. lépés: Futtassa a teljes C# konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="361c7-251"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="361c7-252">Nyomja le az hello **DocumentDBGettingStarted** gombra a Visual Studio toobuild hello alkalmazás hibakeresési módban.</span><span class="sxs-lookup"><span data-stu-id="361c7-252">Press hello **DocumentDBGettingStarted** button in Visual Studio toobuild hello application in debug mode.</span></span>

<span data-ttu-id="361c7-253">Meg kell jelennie az első lépések alkalmazás hello konzolablakban hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="361c7-253">You should see hello output of your get started app in hello console window.</span></span> <span data-ttu-id="361c7-254">hello kimenet hello hello eredményét megjeleníti azt hozzá, és meg kell felelnie az alábbi hello mintaszöveggel lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="361c7-254">hello output will show hello results of hello queries we added and should match hello example text below.</span></span>

```
Created FamilyDB_oa
Press any key toocontinue ...
Created FamilyCollection_oa
Press any key toocontinue ...
Created Family Andersen.1
Press any key toocontinue ...
Created Family Wakefield.7
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key toocontinue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key tooexit.
```

<span data-ttu-id="361c7-255">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="361c7-255">Congratulations!</span></span> <span data-ttu-id="361c7-256">Hello az oktatóanyag elvégzése után, és van egy működő C# konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="361c7-256">You've completed hello tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="361c7-257"><a id="GetSolution"></a>Hello teljes oktatóanyag megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="361c7-257"><a id="GetSolution"></a> Get hello complete tutorial solution</span></span>
<span data-ttu-id="361c7-258">toobuild hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="361c7-258">toobuild hello GetStarted solution that contains all hello samples in this article, you will need hello following:</span></span>

* <span data-ttu-id="361c7-259">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="361c7-259">An active Azure account.</span></span> <span data-ttu-id="361c7-260">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="361c7-260">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="361c7-261">Egy [Azure Cosmos DB fiók][create-documentdb-dotnet.md#create-account].</span><span class="sxs-lookup"><span data-stu-id="361c7-261">An [Azure Cosmos DB account][create-documentdb-dotnet.md#create-account].</span></span>
* <span data-ttu-id="361c7-262">Hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) megoldás elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="361c7-262">hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="361c7-263">toorestore hello hivatkozások toohello DocumentDB API for Azure Cosmos DB .NET Core SDK-t a Visual Studióban, kattintson a jobb gombbal a hello **GetStarted** Megoldáskezelőben, majd megoldás **engedélyezése NuGet-csomagok visszaállításának**.</span><span class="sxs-lookup"><span data-stu-id="361c7-263">toorestore hello references toohello DocumentDB API for Azure Cosmos DB .NET Core SDK in Visual Studio, right-click hello **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="361c7-264">Következő lépésként hello Program.cs fájlban frissítse hello végponti URL-cím és az AuthorizationKey értékeket a [tooan Azure Cosmos DB fiók csatlakozás](#Connect).</span><span class="sxs-lookup"><span data-stu-id="361c7-264">Next, in hello Program.cs file, update hello EndpointUrl and AuthorizationKey values as described in [Connect tooan Azure Cosmos DB account](#Connect).</span></span>

## <a name="next-steps"></a><span data-ttu-id="361c7-265">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="361c7-265">Next steps</span></span>
* <span data-ttu-id="361c7-266">Összetettebb ASP.NET MVC-oktatóanyagot szeretne?</span><span class="sxs-lookup"><span data-stu-id="361c7-266">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="361c7-267">Lásd: [ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-267">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="361c7-268">Toodevelop szeretné, hogy a Xamarin iOS, Android vagy űrlapok DocumentDB API-t használó hello Azure Cosmos DB .NET Core SDK-t?</span><span class="sxs-lookup"><span data-stu-id="361c7-268">Want toodevelop a Xamarin iOS, Android, or Forms application using hello DocumentDB API for Azure Cosmos DB .NET Core SDK?</span></span> <span data-ttu-id="361c7-269">Lásd: [használó alkalmazások mobil a Xamarinnal és Azure Cosmos DB](mobile-apps-with-xamarin.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-269">See [Build mobile applications with Xamarin and Azure Cosmos DB](mobile-apps-with-xamarin.md).</span></span>
* <span data-ttu-id="361c7-270">Szeretné, hogy tooperform méretezés és teljesítmény Azure Cosmos DB tesztelték?</span><span class="sxs-lookup"><span data-stu-id="361c7-270">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="361c7-271">Lásd: [teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="361c7-271">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="361c7-272">Ismerje meg, hogyan túl[figyelő Azure Cosmos DB kéréseket, a használati és a tárolási](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="361c7-272">Learn how too[Monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="361c7-273">A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="361c7-273">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="361c7-274">toolearn hello programozási modell kapcsolatos további információkért lásd: [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="361c7-274">toolearn more about hello programming model, see [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span>

[create-documentdb-dotnet.md#create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png
