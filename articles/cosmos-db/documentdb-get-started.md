---
title: "Azure Cosmos DB: DocumentDB API kezdeti lépéseket ismertető oktatóanyag| Microsoft Docs"
description: "Ez az oktatóanyag létrehoz egy online adatbázist és a C# konzolalkalmazást hello DocumentDB API használatával."
keywords: "nosql-oktatóanyag, online adatbázis, c# konzolalkalmazás"
services: cosmos-db
documentationcenter: .net
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2017
ms.author: anhoh
ms.openlocfilehash: 65a181f715a670987492ad7815ef2ec94498e84d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a><span data-ttu-id="90f70-104">Azure Cosmos DB: DocumentDB API kezdeti lépéseket ismertető oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="90f70-104">Azure Cosmos DB: DocumentDB API getting started tutorial</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="90f70-105">.NET</span><span class="sxs-lookup"><span data-stu-id="90f70-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="90f70-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="90f70-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="90f70-107">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="90f70-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="90f70-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="90f70-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="90f70-109">Java</span><span class="sxs-lookup"><span data-stu-id="90f70-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="90f70-110">C++</span><span class="sxs-lookup"><span data-stu-id="90f70-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="90f70-111">Üdvözli a toohello Azure Cosmos DB DocumentDB API használatába bevezető oktatóanyagot!</span><span class="sxs-lookup"><span data-stu-id="90f70-111">Welcome toohello Azure Cosmos DB DocumentDB API getting started tutorial!</span></span> <span data-ttu-id="90f70-112">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="90f70-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="90f70-113">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="90f70-113">We'll cover:</span></span>

* <span data-ttu-id="90f70-114">Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="90f70-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="90f70-115">A Visual Studio megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="90f70-115">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="90f70-116">Online adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-116">Creating an online database</span></span>
* <span data-ttu-id="90f70-117">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-117">Creating a collection</span></span>
* <span data-ttu-id="90f70-118">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-118">Creating JSON documents</span></span>
* <span data-ttu-id="90f70-119">Hello gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="90f70-119">Querying hello collection</span></span>
* <span data-ttu-id="90f70-120">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="90f70-120">Replacing a document</span></span>
* <span data-ttu-id="90f70-121">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="90f70-121">Deleting a document</span></span>
* <span data-ttu-id="90f70-122">Hello adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="90f70-122">Deleting hello database</span></span>

<span data-ttu-id="90f70-123">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="90f70-123">Don't have time?</span></span> <span data-ttu-id="90f70-124">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="90f70-124">Don't worry!</span></span> <span data-ttu-id="90f70-125">a teljes megoldás hello érhető [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="90f70-125">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> <span data-ttu-id="90f70-126">Ugrás a toohello [hello teljes NoSQL-oktatóanyag megoldás szakaszának beolvasása](#GetSolution) gyors utasításokért.</span><span class="sxs-lookup"><span data-stu-id="90f70-126">Jump toohello [Get hello complete NoSQL tutorial solution section](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="90f70-127">Ezt követően adjon használata hello szavazás gombok hello felső vagy a lap toogive alján nekünk visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="90f70-127">Afterwards, please use hello voting buttons at hello top or bottom of this page toogive us feedback.</span></span> <span data-ttu-id="90f70-128">Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude a megjegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="90f70-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="90f70-129">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="90f70-129">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90f70-130">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="90f70-130">Prerequisites</span></span>
<span data-ttu-id="90f70-131">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="90f70-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="90f70-132">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="90f70-132">An active Azure account.</span></span> <span data-ttu-id="90f70-133">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="90f70-133">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="90f70-134">Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="90f70-134">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="90f70-135">[A Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="90f70-135">[Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="90f70-136">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="90f70-137">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="90f70-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="90f70-138">Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="90f70-138">If you already have an account you want toouse, you can skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span> <span data-ttu-id="90f70-139">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="90f70-139">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Setup your Visual Studio Solution](#SetupVS).</span></span>

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="90f70-140"><a id="SetupVS"></a>2. lépés: A Visual Studio megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="90f70-140"><a id="SetupVS"></a>Step 2: Setup your Visual Studio solution</span></span>
1. <span data-ttu-id="90f70-141">Nyissa meg a **Visual Studio 2017-et** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="90f70-141">Open **Visual Studio 2017** on your computer.</span></span>
2. <span data-ttu-id="90f70-142">A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="90f70-142">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="90f70-143">A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás**nevű a projekt, és végül **OK**.</span><span class="sxs-lookup"><span data-stu-id="90f70-143">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console Application**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="90f70-144">![Képernyőfelvétel a hello új projekt ablakról](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="90f70-144">![Screen shot of hello New Project window](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)</span></span>
4. <span data-ttu-id="90f70-145">A hello **Megoldáskezelőben**, kattintson az új konzolalkalmazásra, amely a Visual Studio megoldás alatt, a jobb gombbal, és kattintson a **NuGet-csomagok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="90f70-145">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Képernyőfelvétel a hello jobbra a hello projekt helyi](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="90f70-147">A hello **Nuget** lapra, majd **Tallózás**, és írja be **az azure documentdb** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="90f70-147">In hello **Nuget** tab, click **Browse**, and type **azure documentdb** in hello search box.</span></span>
6. <span data-ttu-id="90f70-148">Hello eredmények belül található **Microsoft.Azure.DocumentDB** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="90f70-148">Within hello results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="90f70-149">hello Azure Cosmos DB DocumentDB API Ügyfélkódtárának Csomagazonosítója hello van [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span><span class="sxs-lookup"><span data-stu-id="90f70-149">hello package ID for hello Azure Cosmos DB DocumentDB API Client Library is [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).</span></span>
   <span data-ttu-id="90f70-150">![Képernyőfelvétel a hello Nuget menüről a Azure Cosmos DB ügyfél SDK-kereséshez](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="90f70-150">![Screen shot of hello Nuget Menu for finding Azure Cosmos DB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="90f70-151">Ha egy üzenetek kapcsolatos módosításokat toohello megoldás áttekintése, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="90f70-151">If you get a messages about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="90f70-152">Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.</span><span class="sxs-lookup"><span data-stu-id="90f70-152">If you get a message about license acceptance, click **I accept**.</span></span>

<span data-ttu-id="90f70-153">Remek!</span><span class="sxs-lookup"><span data-stu-id="90f70-153">Great!</span></span> <span data-ttu-id="90f70-154">Most, hogy befejeztük hello beállítása, először néhány kódot ír.</span><span class="sxs-lookup"><span data-stu-id="90f70-154">Now that we finished hello setup, let's start writing some code.</span></span> <span data-ttu-id="90f70-155">A [GitHubon](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs) megtalálhatja az oktatóanyagban szereplő kódprojekt befejezett változatát.</span><span class="sxs-lookup"><span data-stu-id="90f70-155">You can find a completed code project of this tutorial at [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).</span></span>

## <span data-ttu-id="90f70-156"><a id="Connect"></a>3. lépés: Csatlakozás tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="90f70-156"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="90f70-157">Először adja hozzá ezeket a C#-alkalmazás, hello Program.cs fájl elejéhez toohello hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="90f70-157">First, add these references toohello beginning of your C# application, in hello Program.cs file:</span></span>

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART tooYOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> <span data-ttu-id="90f70-158">Rendelés toocomplete hello oktatóanyag ellenőrizze, hello függőségeket.</span><span class="sxs-lookup"><span data-stu-id="90f70-158">In order toocomplete hello tutorial, make sure you add hello dependencies above.</span></span>
> 
> 

<span data-ttu-id="90f70-159">Most adja hozzá ezt a két állandót és az *ügyfél* változót a *Program* nyilvános osztály alatt.</span><span class="sxs-lookup"><span data-stu-id="90f70-159">Now, add these two constants and your *client* variable underneath your public class *Program*.</span></span>

    public class Program
    {
        // ADD THIS PART tooYOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

<span data-ttu-id="90f70-160">A következő head biztonsági toohello [Azure Portal](https://portal.azure.com) tooretrieve a végponti URL-cím és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="90f70-160">Next, head back toohello [Azure Portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="90f70-161">hello végponti URL-cím és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="90f70-161">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="90f70-162">A hello Azure portál, keresse meg a tooyour Azure Cosmos DB fiókot, és kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="90f70-162">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span>

<span data-ttu-id="90f70-163">Hello portálról hello URI másolja és illessze be azt `<your endpoint URL>` hello program.cs fájlban.</span><span class="sxs-lookup"><span data-stu-id="90f70-163">Copy hello URI from hello portal and paste it into `<your endpoint URL>` in hello program.cs file.</span></span> <span data-ttu-id="90f70-164">Ezután másolási elsődleges kulcs hello hello portálról, és illessze be azt `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="90f70-164">Then copy hello PRIMARY KEY from hello portal and paste it into `<your primary key>`.</span></span>

![Képernyőfelvétel a hello hello NoSQL-oktatóanyag toocreate a C# Konzolalkalmazás által használt Azure-portálról.][keys]

<span data-ttu-id="90f70-167">Ezt követően először foglalkozunk hello alkalmazást hozzon létre egy új példányát hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="90f70-167">Next, we'll start hello application by creating a new instance of hello **DocumentClient**.</span></span>

<span data-ttu-id="90f70-168">Alább hello **fő** módszer, vegye fel a elnevezésű új aszinkron feladatot **GetStartedDemo**, amely létrehozza nekünk az új **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="90f70-168">Below hello **Main** method, add this new asynchronous task called **GetStartedDemo**, which will instantiate our new **DocumentClient**.</span></span>

    static void Main(string[] args)
    {
    }

    // ADD THIS PART tooYOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

<span data-ttu-id="90f70-169">Hello következő kódot toorun az aszinkron feladat hozzáadása a **fő** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-169">Add hello following code toorun your asynchronous task from your **Main** method.</span></span> <span data-ttu-id="90f70-170">Hello **fő** metódus kivételeket, és azt toohello konzol írja azokat.</span><span class="sxs-lookup"><span data-stu-id="90f70-170">hello **Main** method will catch exceptions and write them toohello console.</span></span>

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

<span data-ttu-id="90f70-171">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-171">Press **F5** toorun your application.</span></span> <span data-ttu-id="90f70-172">hello konzol kimenetét hello üzenet jelenik meg `End of demo, press any key tooexit.` erősítse meg, hogy létrejött-e hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="90f70-172">hello console window output displays hello message `End of demo, press any key tooexit.` confirming that hello connection was made.</span></span>  <span data-ttu-id="90f70-173">Hello konzolablak majd zárja be.</span><span class="sxs-lookup"><span data-stu-id="90f70-173">You can then close hello console window.</span></span> 

<span data-ttu-id="90f70-174">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-174">Congratulations!</span></span> <span data-ttu-id="90f70-175">Sikeresen csatlakozott tooan Azure Cosmos DB fiók, most vessünk egy pillantást a Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="90f70-175">You have successfully connected tooan Azure Cosmos DB account, let's now take a look at working with Azure Cosmos DB resources.</span></span>  

## <a name="step-4-create-a-database"></a><span data-ttu-id="90f70-176">4. lépés: Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-176">Step 4: Create a database</span></span>
<span data-ttu-id="90f70-177">Az adatbázis létrehozásához hello kód hozzáadása előtt adjon hozzá egy segédmetódust toohello konzol írásához.</span><span class="sxs-lookup"><span data-stu-id="90f70-177">Before you add hello code for creating a database, add a helper method for writing toohello console.</span></span>

<span data-ttu-id="90f70-178">Másolja és illessze be a hello **WriteToConsoleAndPromptToContinue** metódus után hello **GetStartedDemo** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-178">Copy and paste hello **WriteToConsoleAndPromptToContinue** method after hello **GetStartedDemo** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key toocontinue ...");
            Console.ReadKey();
    }

<span data-ttu-id="90f70-179">Az Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="90f70-179">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="90f70-180">Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.</span><span class="sxs-lookup"><span data-stu-id="90f70-180">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

<span data-ttu-id="90f70-181">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello ügyfél létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="90f70-181">Copy and paste hello following code tooyour **GetStartedDemo** method after hello client creation.</span></span> <span data-ttu-id="90f70-182">Ezzel létrehoz egy *FamilyDB* elnevezésű adatbázist.</span><span class="sxs-lookup"><span data-stu-id="90f70-182">This will create a database named *FamilyDB*.</span></span>

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART tooYOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

<span data-ttu-id="90f70-183">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-183">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-184">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-184">Congratulations!</span></span> <span data-ttu-id="90f70-185">Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="90f70-185">You have successfully created an Azure Cosmos DB database.</span></span>  

## <span data-ttu-id="90f70-186"><a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-186"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="90f70-187">A **CreateDocumentCollectionIfNotExistsAsync** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="90f70-187">**CreateDocumentCollectionIfNotExistsAsync** will create a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="90f70-188">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="90f70-188">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="90f70-189">A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="90f70-189">A [collection](documentdb-resources.md#collections) can be created by using hello [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="90f70-190">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="90f70-190">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="90f70-191">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello adatbázis létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="90f70-191">Copy and paste hello following code tooyour **GetStartedDemo** method after hello database creation.</span></span> <span data-ttu-id="90f70-192">Ezzel létrehoz egy *FamilyCollection* elnevezésű dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="90f70-192">This will create a document collection named *FamilyCollection*.</span></span>

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART tooYOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

<span data-ttu-id="90f70-193">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-193">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-194">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-194">Congratulations!</span></span> <span data-ttu-id="90f70-195">Sikeresen létrehozott egy Azure Cosmos DB-dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="90f70-195">You have successfully created an Azure Cosmos DB document collection.</span></span>  

## <span data-ttu-id="90f70-196"><a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="90f70-196"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="90f70-197">A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="90f70-197">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="90f70-198">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="90f70-198">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="90f70-199">Most már beilleszthetünk egy vagy több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="90f70-199">We can now insert one or more documents.</span></span> <span data-ttu-id="90f70-200">Ha van olyan adat, milyen toostore az adatbázisban, használhatja a hello Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) tooimport hello adatokat az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="90f70-200">If you already have data you'd like toostore in your database, you can use hello Azure Cosmos DB [Data Migration tool](import-data.md) tooimport hello data into a database.</span></span>

<span data-ttu-id="90f70-201">Először létre kell toocreate egy **termékcsalád** osztály, amely a mintában Azure Cosmos DB tárolt objektumokat képviseli.</span><span class="sxs-lookup"><span data-stu-id="90f70-201">First, we need toocreate a **Family** class that will represent objects stored within Azure Cosmos DB in this sample.</span></span> <span data-ttu-id="90f70-202">Létrehozunk még egy **Szülő**, **Gyermek**, **Háziállat** és **Cím** alosztályt is a **Család** osztályban való használatra.</span><span class="sxs-lookup"><span data-stu-id="90f70-202">We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**.</span></span> <span data-ttu-id="90f70-203">Ne feledje, hogy a dokumentumoknak rendelkezniük kell egy **Azonosító** tulajdonsággal, amely a JSON-fájlban **id**-ként van szerializálva.</span><span class="sxs-lookup"><span data-stu-id="90f70-203">Note that documents must have an **Id** property serialized as **id** in JSON.</span></span> <span data-ttu-id="90f70-204">Az osztályok létrehozásához adja hozzá a következő belső alosztályok után hello hello **GetStartedDemo** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-204">Create these classes by adding hello following internal sub-classes after hello **GetStartedDemo** method.</span></span>

<span data-ttu-id="90f70-205">Másolja és illessze be a hello **termékcsalád**, **szülő**, **gyermek**, **háziállat**, és **cím** után hello osztályok **WriteToConsoleAndPromptToContinue** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-205">Copy and paste hello **Family**, **Parent**, **Child**, **Pet**, and **Address** classes after hello **WriteToConsoleAndPromptToContinue** method.</span></span>

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

<span data-ttu-id="90f70-206">Másolja és illessze be a hello **CreateFamilyDocumentIfNotExists** metódust a **cím** osztály.</span><span class="sxs-lookup"><span data-stu-id="90f70-206">Copy and paste hello **CreateFamilyDocumentIfNotExists** method underneath your **Address** class.</span></span>

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

<span data-ttu-id="90f70-207">Majd szúrja be két dokumentumot, egyet mindegyik hello Andersen családhoz, majd hello Wakefield családhoz.</span><span class="sxs-lookup"><span data-stu-id="90f70-207">And insert two documents, one each for hello Andersen Family and hello Wakefield Family.</span></span>

<span data-ttu-id="90f70-208">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello dokumentumgyűjtemény létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="90f70-208">Copy and paste hello following code tooyour **GetStartedDemo** method after hello document collection creation.</span></span>

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

<span data-ttu-id="90f70-209">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-209">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-210">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-210">Congratulations!</span></span> <span data-ttu-id="90f70-211">Sikeresen létrehozott két Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="90f70-211">You have successfully created two Azure Cosmos DB documents.</span></span>  

![Diagram szemléltető hello hierarchikus kapcsolatát hello fiók hello online adatbázis, gyűjtemény hello és hello NoSQL-oktatóanyag toocreate által használt hello dokumentumok a C# Konzolalkalmazás](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="90f70-213"><a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="90f70-213"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="90f70-214">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="90f70-214">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="90f70-215">hello következő mintakód bemutatja, több olyan lekérdezést, mert mindkét Azure Cosmos DB SQL használatával szintaxisát, valamint a LINQ -, amely azt is futtathatók hello azt hello előző lépésben beszúrt dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="90f70-215">hello following sample code shows various queries - using both Azure Cosmos DB SQL syntax as well as LINQ - that we can run against hello documents we inserted in hello previous step.</span></span>

<span data-ttu-id="90f70-216">Másolja és illessze be a hello **ExecuteSimpleQuery** metódus után a **CreateFamilyDocumentIfNotExists** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-216">Copy and paste hello **ExecuteSimpleQuery** method after your **CreateFamilyDocumentIfNotExists** method.</span></span>

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

<span data-ttu-id="90f70-217">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello második dokumentum létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="90f70-217">Copy and paste hello following code tooyour **GetStartedDemo** method after hello second document creation.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART tooYOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="90f70-218">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-218">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-219">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-219">Congratulations!</span></span> <span data-ttu-id="90f70-220">Sikeres lekérdezést végzett egy Azure Cosmos DB-gyűjteményen.</span><span class="sxs-lookup"><span data-stu-id="90f70-220">You have successfully queried against an Azure Cosmos DB collection.</span></span>

<span data-ttu-id="90f70-221">hello következő diagram azt ábrázolja, hogyan hello Azure Cosmos adatbázis SQL-lekérdezés szintaxisa hívást hello gyűjtemény létrehozása, és hello ugyanez a logika vonatkozik toohello LINQ-lekérdezésekre is.</span><span class="sxs-lookup"><span data-stu-id="90f70-221">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created, and hello same logic applies toohello LINQ query as well.</span></span>

![A C# Konzolalkalmazás hello NoSQL-oktatóanyag toocreate használja és hello lekérdezés jelentését hello hatókör ábrázoló diagram](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

<span data-ttu-id="90f70-223">Hello [FROM](documentdb-sql-query.md#FromClause) kulcsszó nem kötelező, hello lekérdezésben, mert Azure Cosmos DB lekérdezések már hatókörön belüli tooa egyetlen gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="90f70-223">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="90f70-224">Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre.</span><span class="sxs-lookup"><span data-stu-id="90f70-224">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="90f70-225">Az Azure Cosmos DB következtethető ki, hogy családok, legfelső szintű vagy hello változó neve választja, hivatkozás hello aktuális gyűjtemény alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="90f70-225">Azure Cosmos DB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

## <span data-ttu-id="90f70-226"><a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje</span><span class="sxs-lookup"><span data-stu-id="90f70-226"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="90f70-227">Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="90f70-227">Azure Cosmos DB supports replacing JSON documents.</span></span>  

<span data-ttu-id="90f70-228">Másolja és illessze be a hello **ReplaceFamilyDocument** metódus után a **ExecuteSimpleQuery** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-228">Copy and paste hello **ReplaceFamilyDocument** method after your **ExecuteSimpleQuery** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

<span data-ttu-id="90f70-229">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello lekérdezés-végrehajtás hello végén lévő hello metódus után.</span><span class="sxs-lookup"><span data-stu-id="90f70-229">Copy and paste hello following code tooyour **GetStartedDemo** method after hello query execution, at hello end of hello method.</span></span> <span data-ttu-id="90f70-230">Miután kicserélte hello dokumentum, ez fog futni hello azonos újra lekérdezése tooview hello megváltozott dokumentum.</span><span class="sxs-lookup"><span data-stu-id="90f70-230">After replacing hello document, this will run hello same query again tooview hello changed document.</span></span>

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART tooYOUR CODE
    // Update hello Grade of hello Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

<span data-ttu-id="90f70-231">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-231">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-232">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-232">Congratulations!</span></span> <span data-ttu-id="90f70-233">Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="90f70-233">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="90f70-234"><a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="90f70-234"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="90f70-235">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.</span><span class="sxs-lookup"><span data-stu-id="90f70-235">Azure Cosmos DB supports deleting JSON documents.</span></span>  

<span data-ttu-id="90f70-236">Másolja és illessze be a hello **DeleteFamilyDocument** metódus után a **ReplaceFamilyDocument** metódust.</span><span class="sxs-lookup"><span data-stu-id="90f70-236">Copy and paste hello **DeleteFamilyDocument** method after your **ReplaceFamilyDocument** method.</span></span>

    // ADD THIS PART tooYOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

<span data-ttu-id="90f70-237">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus hello második lekérdezés végrehajtása, hello végén lévő hello metódus után.</span><span class="sxs-lookup"><span data-stu-id="90f70-237">Copy and paste hello following code tooyour **GetStartedDemo** method after hello second query execution, at hello end of hello method.</span></span>

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART tooCODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

<span data-ttu-id="90f70-238">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-238">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-239">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-239">Congratulations!</span></span> <span data-ttu-id="90f70-240">Sikeresen törölt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="90f70-240">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="90f70-241"><a id="DeleteDatabase"></a>10. lépés: Hello adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="90f70-241"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="90f70-242">A létrehozott adatbázis törlésével hello eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).</span><span class="sxs-lookup"><span data-stu-id="90f70-242">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="90f70-243">Másolás és beillesztés hello következő code tooyour **GetStartedDemo** metódus után hello dokumentum törlése toodelete hello adatbázis és az összes gyermek-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="90f70-243">Copy and paste hello following code tooyour **GetStartedDemo** method after hello document delete toodelete hello entire database and all children resources.</span></span>

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART tooCODE
    // Clean up/delete hello database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

<span data-ttu-id="90f70-244">Nyomja le az **F5** toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="90f70-244">Press **F5** toorun your application.</span></span>

<span data-ttu-id="90f70-245">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-245">Congratulations!</span></span> <span data-ttu-id="90f70-246">Sikeresen törölt egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="90f70-246">You have successfully deleted an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="90f70-247"><a id="Run"></a>11. lépés: Futtassa a teljes C# konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="90f70-247"><a id="Run"></a>Step 11: Run your C# console application all together!</span></span>
<span data-ttu-id="90f70-248">Kattintson az F5 billentyűt a Visual Studio toobuild hello alkalmazás hibakeresési módban.</span><span class="sxs-lookup"><span data-stu-id="90f70-248">Hit F5 in Visual Studio toobuild hello application in debug mode.</span></span>

<span data-ttu-id="90f70-249">A konzolablakban az első lépések alkalmazás kimenetének hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="90f70-249">You should see hello output of your get started app in a console window.</span></span> <span data-ttu-id="90f70-250">hello kimenet hello hello eredményét megjeleníti azt hozzá, és meg kell felelnie az alábbi hello mintaszöveggel lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="90f70-250">hello output will show hello results of hello queries we added and should match hello example text below.</span></span>

    Created FamilyDB
    Press any key toocontinue ...
    Created FamilyCollection
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

<span data-ttu-id="90f70-251">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="90f70-251">Congratulations!</span></span> <span data-ttu-id="90f70-252">Hello az oktatóanyag elvégzése után, és van egy működő C# konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="90f70-252">You've completed hello tutorial and have a working C# console application!</span></span>

## <span data-ttu-id="90f70-253"><a id="GetSolution"></a>Hello teljes oktatóanyag megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="90f70-253"><a id="GetSolution"></a> Get hello complete tutorial solution</span></span>
<span data-ttu-id="90f70-254">Ha Ön nem volt idő toocomplete hello lépések Ez az oktatóanyag, vagy csak a kívánt toodownload hello kódpéldák tárgymutatójának, beszerezheti a [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span><span class="sxs-lookup"><span data-stu-id="90f70-254">If you didn't have time toocomplete hello steps in this tutorial, or just want toodownload hello code samples, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started).</span></span> 

<span data-ttu-id="90f70-255">toobuild hello GetStarted-megoldás, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="90f70-255">toobuild hello GetStarted solution, you will need hello following:</span></span>

* <span data-ttu-id="90f70-256">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="90f70-256">An active Azure account.</span></span> <span data-ttu-id="90f70-257">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="90f70-257">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="90f70-258">Egy [Azure Cosmos DB fiók][cosmos-db-create-account].</span><span class="sxs-lookup"><span data-stu-id="90f70-258">An [Azure Cosmos DB account][cosmos-db-create-account].</span></span>
* <span data-ttu-id="90f70-259">Hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) megoldás elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="90f70-259">hello [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="90f70-260">toorestore hello hivatkozások toohello Azure Cosmos DB .NET SDK-t a Visual Studióban, kattintson a jobb gombbal a hello **GetStarted** Megoldáskezelőben, majd megoldás **engedélyezése NuGet-csomagok visszaállításának**.</span><span class="sxs-lookup"><span data-stu-id="90f70-260">toorestore hello references toohello Azure Cosmos DB .NET SDK in Visual Studio, right-click hello **GetStarted** solution in Solution Explorer, and then click **Enable NuGet Package Restore**.</span></span> <span data-ttu-id="90f70-261">Következő lépésként hello App.config fájlban frissítse hello végponti URL-cím és az AuthorizationKey értékeket a [tooan Azure Cosmos DB fiók csatlakozás](#Connect).</span><span class="sxs-lookup"><span data-stu-id="90f70-261">Next, in hello App.config file, update hello EndpointUrl and AuthorizationKey values as described in [Connect tooan Azure Cosmos DB account](#Connect).</span></span>

<span data-ttu-id="90f70-262">Ennyi az egész! Építse ki, és máris jó úton jár!</span><span class="sxs-lookup"><span data-stu-id="90f70-262">That's it, build it and you're on your way!</span></span>


## <a name="next-steps"></a><span data-ttu-id="90f70-263">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90f70-263">Next steps</span></span>
* <span data-ttu-id="90f70-264">Összetettebb ASP.NET MVC-oktatóanyagot szeretne?</span><span class="sxs-lookup"><span data-stu-id="90f70-264">Want a more complex ASP.NET MVC tutorial?</span></span> <span data-ttu-id="90f70-265">Lásd: [ASP.NET MVC oktatóprogram: webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-dotnet-application.md).</span><span class="sxs-lookup"><span data-stu-id="90f70-265">See [ASP.NET MVC Tutorial: Web application development with Azure Cosmos DB](documentdb-dotnet-application.md).</span></span>
* <span data-ttu-id="90f70-266">Szeretné, hogy tooperform méretezés és teljesítmény Azure Cosmos DB tesztelték?</span><span class="sxs-lookup"><span data-stu-id="90f70-266">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="90f70-267">Lásd: [teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="90f70-267">See [Performance and scale testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="90f70-268">Ismerje meg, hogyan túl[figyelése Azure Cosmos DB kéréseket, a használat és a tárolási](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="90f70-268">Learn how too[monitor Azure Cosmos DB requests, usage, and storage](monitor-accounts.md).</span></span>
* <span data-ttu-id="90f70-269">A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="90f70-269">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="90f70-270">toolearn Azure Cosmos DB, kapcsolatos további információkért lásd: [tooAzure Cosmos DB üdvözli](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="90f70-270">toolearn more about Azure Cosmos DB, see [Welcome tooAzure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account
