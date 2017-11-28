---
title: az Azure Functions SendGrid aaaHow toouse |} Microsoft Docs
description: Bemutatja, hogyan toouse az Azure Functions SendGrid
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
ms.openlocfilehash: a0ffdae04e5924c773d2d26427626fc1f570f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-sendgrid-in-azure-functions"></a><span data-ttu-id="51cb4-103">Hogyan toouse az Azure Functions SendGrid</span><span class="sxs-lookup"><span data-stu-id="51cb4-103">How toouse SendGrid in Azure Functions</span></span>

## <a name="sendgrid-overview"></a><span data-ttu-id="51cb4-104">SendGrid áttekintése</span><span class="sxs-lookup"><span data-stu-id="51cb4-104">SendGrid Overview</span></span>

<span data-ttu-id="51cb4-105">Az Azure Functions támogatja SendGrid kimeneti kötések tooenable a funkciók toosend e-mail üzenetek néhány sornyi kódot és egy SendGrid-fiókot.</span><span class="sxs-lookup"><span data-stu-id="51cb4-105">Azure Functions supports SendGrid output bindings tooenable your functions toosend email messages with a few lines of code and a SendGrid account.</span></span>

<span data-ttu-id="51cb4-106">toouse hello SendGrid API-nak az Azure-függvény, rendelkeznie kell egy [SendGrid fiók](http://SendGrid.com).</span><span class="sxs-lookup"><span data-stu-id="51cb4-106">toouse hello SendGrid API in an Azure Function, you must have a [SendGrid account](http://SendGrid.com).</span></span> <span data-ttu-id="51cb4-107">Emellett rendelkeznie kell egy SendGrid API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="51cb4-107">Additionally, you must have a SendGrid API Key.</span></span> <span data-ttu-id="51cb4-108">Jelentkezzen be tooyour SendGrid fiókot, és kattintson a **beállítások** majd **API-kulcs** toogenerate egy API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="51cb4-108">Log in tooyour SendGrid account and click **Settings** then **API Key** toogenerate an API key.</span></span> <span data-ttu-id="51cb4-109">Tartsa meg a rendelkezésre álló kulcs használata azt egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="51cb4-109">Keep this key available as you use it in an upcoming step.</span></span>

<span data-ttu-id="51cb4-110">Most már áll készen toocreate egy Azure-függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51cb4-110">You are now ready toocreate an Azure Function app.</span></span>

## <a name="create-an-azure-function-app"></a><span data-ttu-id="51cb4-111">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="51cb4-111">Create an Azure Function app</span></span> 

<span data-ttu-id="51cb4-112">Az Azure functions egy vagy több Azure függvény alkalmazások tárolói.</span><span class="sxs-lookup"><span data-stu-id="51cb4-112">Azure Function Apps are containers for one or more Azure functions.</span></span> <span data-ttu-id="51cb4-113">Az Azure functions rendszer éppen ez - függvényt.</span><span class="sxs-lookup"><span data-stu-id="51cb4-113">Azure functions are just that - a function.</span></span> <span data-ttu-id="51cb4-114">Minden Azure függvény kapcsolt tooone eseményindító, amely hello függvény toorun okozó esemény.</span><span class="sxs-lookup"><span data-stu-id="51cb4-114">Each Azure function is tied tooone trigger, which is an event that causes hello function toorun.</span></span>
<span data-ttu-id="51cb4-115">Minden függvény tartalmazhatnak tetszőleges számú bemeneti vagy kimeneti kötések.</span><span class="sxs-lookup"><span data-stu-id="51cb4-115">Each function can contain any number of input or output bindings.</span></span> <span data-ttu-id="51cb4-116">Kötések a szolgáltatások, hogy a függvényben használható.</span><span class="sxs-lookup"><span data-stu-id="51cb4-116">Bindings are services that you can use in a function.</span></span> <span data-ttu-id="51cb4-117">SendGrid, mert egy kötés, kimeneti toosend e-mail használhatják.</span><span class="sxs-lookup"><span data-stu-id="51cb4-117">SendGrid is an output binding you can use toosend email.</span></span> 

1. <span data-ttu-id="51cb4-118">Jelentkezzen be Azure-portálon toohello és [hozzon létre egy Azure-függvény alkalmazást](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) vagy nyisson meg egy meglévő függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51cb4-118">Log in toohello Azure portal and [create an Azure Function App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) or open an existing Function app.</span></span> 
2. <span data-ttu-id="51cb4-119">Egy Azure-függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="51cb4-119">Create an Azure function.</span></span> <span data-ttu-id="51cb4-120">tookeep az egyszerű, válasszon ki egy kézi indítási és C#.</span><span class="sxs-lookup"><span data-stu-id="51cb4-120">tookeep it simple, choose a manual trigger and C#.</span></span> 

 ![Egy Azure-függvény létrehozása](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a><span data-ttu-id="51cb4-122">Egy Azure-függvény alkalmazása SendGrid konfigurálása</span><span class="sxs-lookup"><span data-stu-id="51cb4-122">Configure SendGrid for use in an Azure Function app</span></span>

<span data-ttu-id="51cb4-123">A SendGrid API-kulcsot kell tárolnia, egy alkalmazás-beállítás toouse azt a függvényben.</span><span class="sxs-lookup"><span data-stu-id="51cb4-123">You must store your SendGrid API Key as an app setting toouse it in a function.</span></span> <span data-ttu-id="51cb4-124">hello ApiKey mező nem a tényleges SendGrid API-kulcsot, de egy alkalmazás, beállítás határozza meg, amely a tényleges API-kulcs jelöli.</span><span class="sxs-lookup"><span data-stu-id="51cb4-124">hello ApiKey field is not your actual SendGrid API key, but an app setting you define that represents your actual API key.</span></span> <span data-ttu-id="51cb4-125">A biztonság érdekében a kulcs tárolása így azért, mert a kódok vagy a fájlokat, előfordulhat, hogy ellenőrizni kell a verziókövetési elkülönül.</span><span class="sxs-lookup"><span data-stu-id="51cb4-125">Storing your key this way is for security, since it is separate from any code or files that might be checked into source code control.</span></span>

- <span data-ttu-id="51cb4-126">Hozzon létre egy **AppSettings** kulcs az függvény alkalmazás **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="51cb4-126">Create an **AppSettings** key in your function app's **Application Settings**.</span></span>

 ![Egy Azure-függvény létrehozása](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a><span data-ttu-id="51cb4-128">Konfigurálja a SendGrid kimeneti kötések</span><span class="sxs-lookup"><span data-stu-id="51cb4-128">Configure SendGrid output bindings</span></span>

<span data-ttu-id="51cb4-129">SendGrid áll rendelkezésre, mert az Azure függvény kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="51cb4-129">SendGrid is available as an Azure function output binding.</span></span> <span data-ttu-id="51cb4-130">a SendGrid toocreate kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="51cb4-130">toocreate a SendGrid output binding:</span></span>

1. <span data-ttu-id="51cb4-131">Nyissa meg toohello **integráció** lapon hello függvény hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="51cb4-131">Go toohello **Integrate** tab of hello function in hello Azure portal.</span></span>
2. <span data-ttu-id="51cb4-132">Kattintson a **új kimeneti** toocreate a SendGrid kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="51cb4-132">Click **New Output** toocreate a SendGrid output binding.</span></span>
3. <span data-ttu-id="51cb4-133">Adja meg a hello **API-kulcs** és **üzenet paraméternév** tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="51cb4-133">Fill in hello **API Key** and **Message parameter name** properties.</span></span> <span data-ttu-id="51cb4-134">Ha azt szeretné, akkor adhat meg egyéb tulajdonságok most hello, vagy inkább code őket.</span><span class="sxs-lookup"><span data-stu-id="51cb4-134">If you want, you can enter hello other properties now, or code them instead.</span></span> <span data-ttu-id="51cb4-135">Ezek a beállítások alapértelmezett értéke használható.</span><span class="sxs-lookup"><span data-stu-id="51cb4-135">These settings can be used as defaults.</span></span>

 ![Konfigurálja a SendGrid kimeneti kötések](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

<span data-ttu-id="51cb4-137">A kötés tooa függvény hozzáadása létrehoz egy nevű **function.json** a függvény mappában.</span><span class="sxs-lookup"><span data-stu-id="51cb4-137">Adding a binding tooa function creates a file called **function.json** in your function's folder.</span></span> <span data-ttu-id="51cb4-138">Ez a fájl tartalmazza az összes ugyanazokat az információkat, hogy a látni hello Azure függvény hello **integráció** lapon, de a Json formátumban.</span><span class="sxs-lookup"><span data-stu-id="51cb4-138">This file contains all hello same information that you see in hello Azure function's **Integrate** tab, but in Json format.</span></span> <span data-ttu-id="51cb4-139">A beállítás hello **ApiKey**, **üzenet**, és **a** mezők létrehozása a következő bejegyzést hello hello **function.json** fájl:</span><span class="sxs-lookup"><span data-stu-id="51cb4-139">Setting hello **ApiKey**, **message**, and **from** fields create hello following entries in hello **function.json** file:</span></span> 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="51cb4-140">Tetszés szerint módosíthatja a fájlt közvetlenül a saját maga.</span><span class="sxs-lookup"><span data-stu-id="51cb4-140">If you prefer, you may modify this file yourself directly.</span></span>

<span data-ttu-id="51cb4-141">Most, hogy létrehozta és hello függvény App és függvény konfigurálva, egy e-mailt hello kód toosend lehet írni.</span><span class="sxs-lookup"><span data-stu-id="51cb4-141">Now that you have created and configured hello Function App and function, you can write hello code toosend an email.</span></span>

## <a name="write-code-that-creates-and-sends-email"></a><span data-ttu-id="51cb4-142">Kód írása, amely létrehoz és e-mailt küld</span><span class="sxs-lookup"><span data-stu-id="51cb4-142">Write code that creates and sends email</span></span>

<span data-ttu-id="51cb4-143">hello SendGrid API tartalmazza az összes hello parancsok toocreate kell, és az e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="51cb4-143">hello SendGrid API contains all hello commands you need toocreate and send an email.</span></span>  

- <span data-ttu-id="51cb4-144">Hello kód hello függvényében cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="51cb4-144">Replace hello code in hello function with hello following code:</span></span>

```cs
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
    message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    // change tooemail of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

<span data-ttu-id="51cb4-145">Értesítés hello első sor tartalmaz hello ```#r``` direktíva, amely hello SendGrid szerelvényre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="51cb4-145">Notice hello first line contains hello ```#r``` directive that references hello SendGrid assembly.</span></span> <span data-ttu-id="51cb4-146">Ezután használhatja a ```using``` utasítás toomore egyszerűen hozzáférhessenek a névtér hello objektumok.</span><span class="sxs-lookup"><span data-stu-id="51cb4-146">After that, you can use a ```using``` statement toomore easily access hello objects in that namespace.</span></span> <span data-ttu-id="51cb4-147">Hello kódot, hozzon létre példányai ```Mail```, ```Personalization```, és ```Content``` hello SendGrid API hello e-mail alkotó objektumokat.</span><span class="sxs-lookup"><span data-stu-id="51cb4-147">In hello code, create instances of ```Mail```, ```Personalization```, and ```Content``` objects from hello SendGrid API that compose hello email.</span></span> <span data-ttu-id="51cb4-148">Amikor visszatér üdvözlőüzenetére, a SendGrid nyújt.</span><span class="sxs-lookup"><span data-stu-id="51cb4-148">When you return hello message, SendGrid delivers it.</span></span> 

<span data-ttu-id="51cb4-149">hello függvény aláírás is tartalmaz egy plusz típusú paraméter out ```Mail``` nevű ```message```.</span><span class="sxs-lookup"><span data-stu-id="51cb4-149">hello function's signature also contains an extra out parameter of type ```Mail``` named ```message```.</span></span> <span data-ttu-id="51cb4-150">Mindkét bemeneti és kimeneti kötések express maguk függvény paramétereiben kódot.</span><span class="sxs-lookup"><span data-stu-id="51cb4-150">Both input and output bindings express themselves as function parameters in code.</span></span> 

2. <span data-ttu-id="51cb4-151">Gombra kattintva tesztelheti a kódját **teszt** , majd gépelje be az üzenetet a hello **Request body** mező, majd kattintson a hello **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="51cb4-151">Test your code by clicking **Test** and entering a message into hello **Request body** field, then clicking hello **Run** button.</span></span>

 ![Tesztelheti a kódját](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. <span data-ttu-id="51cb4-153">Ellenőrizze, hogy a SendGrid hello e-mailt küldeni e-mailek tooverify.</span><span class="sxs-lookup"><span data-stu-id="51cb4-153">Check email tooverify that SendGrid sent hello email.</span></span> <span data-ttu-id="51cb4-154">Azt kell toohello cím hello kódban nyissa meg az 1. lépés, és tartalmazzák a hello üdvözlőüzenetére **Request body**.</span><span class="sxs-lookup"><span data-stu-id="51cb4-154">It should go toohello address in hello code from step 1, and contain hello message from hello **Request body**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51cb4-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51cb4-155">Next steps</span></span>
<span data-ttu-id="51cb4-156">Ez a cikk bemutatta, hogyan toouse SendGrid szolgáltatás toocreate hello és e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="51cb4-156">This article has demonstrated how toouse hello SendGrid service toocreate and send email.</span></span> <span data-ttu-id="51cb4-157">További információ az Azure Functions használatával az alkalmazásokban lévő toolearn lásd: a következő témakörök hello:</span><span class="sxs-lookup"><span data-stu-id="51cb4-157">toolearn more about using Azure Functions in your apps, see hello following topics:</span></span> 

- <span data-ttu-id="51cb4-158">[Gyakorlati tanácsok az Azure Functions](functions-best-practices.md) néhány gyakorlati tanácsok toouse sorolja fel, az Azure Functions létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="51cb4-158">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices toouse when creating Azure Functions.</span></span>

- <span data-ttu-id="51cb4-159">[Az Azure Functions fejlesztői segédanyagai](functions-reference.md) programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="51cb4-159">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>

- <span data-ttu-id="51cb4-160">[Az Azure Functions tesztelése](functions-test-a-function.md) különböző eszközöket és technikákat a funkciók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="51cb4-160">[Testing Azure Functions](functions-test-a-function.md) Describes various tools and techniques for testing your functions.</span></span>