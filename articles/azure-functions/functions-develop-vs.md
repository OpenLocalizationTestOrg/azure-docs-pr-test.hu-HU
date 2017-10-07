---
title: "Visual Studio használatával Azure Functions aaaDevelop |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop és tesztelése az Azure Functions által az Azure Functions Tools for Visual Studio 2017 használatával."
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
ms.openlocfilehash: c9baf882bf58068cb9a8930bea337fe51b2a77ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-tools-for-visual-studio"></a>Az Azure Functions Tools for Visual Studio  

Az Azure funkciók Tools for Visual Studio 2017 bővítménye, amely lehetővé teszi a fejlesztés, tesztelése és C# funkciók tooAzure telepítése a Visual Studio. Ha ez az első tapasztalattal az Azure Functions, többet is megtudhat a [egy bevezető tooAzure működik](functions-overview.md).

hello Azure Functions eszközök hello a következő előnyöket biztosítja: 

* Szerkesztés, elkészítéséhez és funkciók a helyi fejlesztési számítógépen futtassa. 
* Közzététel az Azure Functions közvetlenül a projekt tooAzure. 
* Webjobs-feladatok attribútumok toodeclare függvénykötés közvetlenül hello C#-kódban helyett egy külön function.json a definíciók kötési karbantartása használja.
* Fejlesztésekor, és előre lefordított függvények C# telepítésekor. Előre lefordított függvények teljesítményt lehet biztosítani a jobban cold indítási mint C# parancsfájlalapú funkciók. 
* A funkciók a C# code során a teljes Visual Studio fejlesztői hello előnyeit. 

Ez a témakör bemutatja, hogyan toouse hello Azure Functions eszközök Visual Studio 2017 toodevelop C# függvényeit. Azt is megtudhatja, hogyan toopublish a projekt tooAzure, mint a .NET-szerelvény.

## <a name="prerequisites"></a>Előfeltételek

Az Azure Functions eszközök megtalálható hello Azure fejlesztési munkaterhelését [Visual Studio 2017 verzió 15.3](https://www.visualstudio.com/vs/), vagy újabb verziója. Győződjön meg arról, hello **Azure fejlesztési** munkaterhelés a Visual Studio 2017 15.3 verzió telepítése:

![A Visual Studio 2017 hello Azure fejlesztési alkalmazások és szolgáltatások telepítése](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

toocreate és funkciók, a rendszer szükség:

* Aktív Azure-előfizetés. Ha nem rendelkezik Azure-előfizetéssel, [fiókok szabad](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) érhetők el.

* Egy Azure Storage-fiókot. a tárfiók toocreate lásd: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account).  
## <a name="create-an-azure-functions-project"></a>Az Azure Functions projekt létrehozása 

[!INCLUDE [Create a project using hello Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-hello-project-for-local-development"></a>Helyi fejlesztési hello projekt konfigurálása

Amikor egy új projektet hello Azure Functions sablonnal hoz létre, egy üres C# projekt, amely tartalmazza a következő fájlok hello elérhetővé:

* **Host.JSON**: konfigurálását teszi lehetővé hello funkciók állomás. Ezeket a beállításokat is alkalmazza, ha fut a helyi és az Azure-ban is. További információkért lásd: [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) áttekintésével foglalkozó cikkben.
    
* **Local.Settings.JSON**: funkciók a helyi futtatás során használt beállításokat tárolja. Ezek a beállítások nem használhatók az Azure-ban, hello használják [Azure Functions Core eszközök](functions-run-local.md). Használja a toospecify beállításai, például kapcsolati karakterláncok tooother Azure szolgáltatások. Adja hozzá egy új kulcs toohello **értékek** tömb minden egyes funkciók a projekt által igényelt kapcsolathoz. További információkért lásd: [helyi beállításfájl](functions-run-local.md#local-settings-file) hello Azure Functions Core eszközök témakörben.

hello Functions futtatókörnyezete belső egy Azure Storage-fiókot használja. Összes indítás típusú HTTP- és webhookokkal, be kell állítani hello **Values.AzureWebJobsStorage** kulcs tooa érvényes Azure tárolási fiók kapcsolati karakterlánc.

[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

 tooset hello tárolási fiók kapcsolati karakterlánc:

1. A Visual Studióban nyissa meg a **Cloud Explorer**, bontsa ki a **Tárfiók** > **a Tárfiók**, majd jelölje be **tulajdonságok**és másolási hello **elsődleges kapcsolódási karakterlánc** érték.   

2. A projektben nyissa meg hello local.settings.json projekt fájlt, és állítsa be hello hello **AzureWebJobsStorage** kimásolt kapcsolati karakterláncot toohello kulcsát.

3. Ismételje meg a hello előző lépés tooadd egyedi kulcsok toohello **értékek** bármely más, a funkciók által igényelt kapcsolatok tömb.  

## <a name="create-a-function"></a>Függvény létrehozása

A előre lefordított függvények a hello függvény által használt hello kötéseket hello kódban attribútumok alkalmazása alapján határozzák meg. Hello Azure Functions eszközök toocreate a megadott hello sablonok funkciókat használ, ezek az attribútumok meg válnak érvényessé. 

1. A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza az **Add** (Hozzáadás)  > **New Item** (Új elem) lehetőséget. Válassza ki **Azure-függvény**, adjon meg egy **neve** hello osztályt, majd kattintson a **Hozzáadás**.

2. Válassza ki az eseményindító, hello beállítása a kötési tulajdonságok, majd kattintson **létrehozása**. hello következő példa bemutatja hello beállítások kiváltásakor a várólista-tároló létrehozása funkciót. 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    Nevű kapcsolati karakterlánc kulcs **QueueStorage** van megadva, amely hello local.settings.json fájlban van megadva. 
 
3. Vizsgálja meg hello újonnan hozzáadott osztály. Megjelenik a statikus **futtatása** metódus, amely a hello tudható **függvénynév** attribútum. Ez az attribútum azt jelzi, hogy hello metódus hello függvény hello összes belépési pontjához. 

    A következő C# osztály hello például egy alapszintű várólista indított tárolási függvény jelöli:

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
 
    A kötés-specifikus attribútummal alkalmazott tooeach kötés a megadott paraméter toohello belépési pont metódusa. hello attribútum hello kötési információ paraméterekként vesz igénybe. Hello előző példában hello első paraméternek egy **QueueTrigger** attribútuma, várólista indított függvény jelző. hello várólista nevét és a kapcsolati karakterlánc Beállításnév paraméterként.  

## <a name="testing-functions"></a>Függvények tesztelése

Az Azure Functions Core Tools lehetővé teszi Azure Functions-projektek helyi fejlesztői számítógépen való futtatását. Ezek az eszközök hello első indításakor függvény Visual Studio felszólító tooinstall áll.  

tootest a függvény, nyomja le az F5 billentyűt. Ha a rendszer kéri, fogadja el a Visual Studio toodownload hello kérelmet, és telepítse az Azure Functions mag (CLI) eszközök.  Szükség lehet a tooenable olyan érvényes tűzfalkivétel, hogy hello eszközök kezelni tud a HTTP-kérelmekre.

Hello projekt fut tesztelheti a kódot, érdemes tesztelni telepített függvény. További információkért lásd: [a kódot az Azure Functions tesztelése kapcsolatos olyan stratégiák](functions-test-a-function.md). Hibakeresési módban fut, töréspontok az elvárt módon vannak elérte a Visual Studio. 

Például egy váltódik ki, hogyan tootest várólista függvény, lásd: hello [indított várólista függvény gyors üzembe helyezési útmutató](functions-create-storage-queue-triggered-function.md#test-the-function).  

toolearn hello Azure Functions alapvető eszközökkel, bővebben lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md).

## <a name="publish-tooazure"></a>TooAzure közzététele

[!INCLUDE [Publish hello project tooAzure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
>Minden hozzáadott hello local.settings.json beállítást is szerepelnie kell toohello függvény alkalmazást az Azure-ban. Ezek a beállítások nem kerülnek be automatikusan. Kötelező beállítások tooyour függvény alkalmazást adhat hozzá az alábbi módszerek valamelyikével:
>
>* [Az Azure portál használatával hello](functions-how-to-use-azure-function-app-settings.md#settings).
>* [Hello segítségével `--publish-local-settings` publish beállítást, az Azure Functions Core eszközök hello](functions-run-local.md#publish).
>* [Az Azure parancssori felület használatával hello](/cli/azure/functionapp/config/appsettings#set). 

## <a name="next-steps"></a>Következő lépések

További információ az Azure Functions eszközök, lásd: gyakori kérdések című hello hello [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blogbejegyzést.

toolearn hello Azure Functions Core eszközök bővebben lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md).  
További információ az alkalmazás, mint funkciók fejlesztése toolearn lásd [használó alkalmazás az Azure Functions](functions-dotnet-class-library.md). Ez a témakör is példákat hogyan toouse attribútumok toodeclare hello kötések Azure Functions által támogatott különböző típusú.    
