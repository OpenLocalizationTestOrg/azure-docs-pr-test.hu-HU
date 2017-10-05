---
title: "SendGrid használata az Azure Functions |} Microsoft Docs"
description: "Bemutatja, hogyan használható az Azure Functions SendGrid"
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
ms.openlocfilehash: 05c9f4e4a4351219da68af8b702c25f21d7d4d02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a><span data-ttu-id="693fe-103">Az Azure Functions SendGrid használata</span><span class="sxs-lookup"><span data-stu-id="693fe-103">How to use SendGrid in Azure Functions</span></span>

## <a name="sendgrid-overview"></a><span data-ttu-id="693fe-104">SendGrid áttekintése</span><span class="sxs-lookup"><span data-stu-id="693fe-104">SendGrid Overview</span></span>

<span data-ttu-id="693fe-105">Az Azure Functions támogatja a SendGrid kimeneti kötések néhány sornyi kódot és egy SendGrid-fiókot az e-mailek küldése a funkciók engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="693fe-105">Azure Functions supports SendGrid output bindings to enable your functions to send email messages with a few lines of code and a SendGrid account.</span></span>

<span data-ttu-id="693fe-106">Egy Azure-függvényt a SendGrid API használatához rendelkeznie kell egy [SendGrid fiók](http://SendGrid.com).</span><span class="sxs-lookup"><span data-stu-id="693fe-106">To use the SendGrid API in an Azure Function, you must have a [SendGrid account](http://SendGrid.com).</span></span> <span data-ttu-id="693fe-107">Emellett rendelkeznie kell egy SendGrid API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="693fe-107">Additionally, you must have a SendGrid API Key.</span></span> <span data-ttu-id="693fe-108">Jelentkezzen be a SendGrid-fiókját, majd kattintson a **beállítások** majd **API-kulcs** API-kulcs létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="693fe-108">Log in to your SendGrid account and click **Settings** then **API Key** to generate an API key.</span></span> <span data-ttu-id="693fe-109">Tartsa meg a rendelkezésre álló kulcs használata azt egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="693fe-109">Keep this key available as you use it in an upcoming step.</span></span>

<span data-ttu-id="693fe-110">Most már készen áll az Azure-függvény alkalmazás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="693fe-110">You are now ready to create an Azure Function app.</span></span>

## <a name="create-an-azure-function-app"></a><span data-ttu-id="693fe-111">Azure-függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="693fe-111">Create an Azure Function app</span></span> 

<span data-ttu-id="693fe-112">Az Azure functions egy vagy több Azure függvény alkalmazások tárolói.</span><span class="sxs-lookup"><span data-stu-id="693fe-112">Azure Function Apps are containers for one or more Azure functions.</span></span> <span data-ttu-id="693fe-113">Az Azure functions rendszer éppen ez - függvényt.</span><span class="sxs-lookup"><span data-stu-id="693fe-113">Azure functions are just that - a function.</span></span> <span data-ttu-id="693fe-114">Minden Azure függvény egy eseményindítót, a függvény futtatásához okozó esemény van kötve.</span><span class="sxs-lookup"><span data-stu-id="693fe-114">Each Azure function is tied to one trigger, which is an event that causes the function to run.</span></span>
<span data-ttu-id="693fe-115">Minden függvény tartalmazhatnak tetszőleges számú bemeneti vagy kimeneti kötések.</span><span class="sxs-lookup"><span data-stu-id="693fe-115">Each function can contain any number of input or output bindings.</span></span> <span data-ttu-id="693fe-116">Kötések a szolgáltatások, hogy a függvényben használható.</span><span class="sxs-lookup"><span data-stu-id="693fe-116">Bindings are services that you can use in a function.</span></span> <span data-ttu-id="693fe-117">SendGrid egy e-mailek küldése segítségével kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="693fe-117">SendGrid is an output binding you can use to send email.</span></span> 

1. <span data-ttu-id="693fe-118">Jelentkezzen be az Azure-portálon és [hozzon létre egy Azure-függvény alkalmazást](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) vagy nyisson meg egy meglévő függvény alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="693fe-118">Log in to the Azure portal and [create an Azure Function App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) or open an existing Function app.</span></span> 
2. <span data-ttu-id="693fe-119">Egy Azure-függvény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="693fe-119">Create an Azure function.</span></span> <span data-ttu-id="693fe-120">Legyen egyszerű, hogy válasszon ki egy kézi indítási és a C#.</span><span class="sxs-lookup"><span data-stu-id="693fe-120">To keep it simple, choose a manual trigger and C#.</span></span> 

 ![Egy Azure-függvény létrehozása](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a><span data-ttu-id="693fe-122">Egy Azure-függvény alkalmazása SendGrid konfigurálása</span><span class="sxs-lookup"><span data-stu-id="693fe-122">Configure SendGrid for use in an Azure Function app</span></span>

<span data-ttu-id="693fe-123">A SendGrid API-kulcs történő használatát a következő függvényt egy alkalmazás beállításként kell tárolnia.</span><span class="sxs-lookup"><span data-stu-id="693fe-123">You must store your SendGrid API Key as an app setting to use it in a function.</span></span> <span data-ttu-id="693fe-124">A ApiKey mező nem a tényleges SendGrid API-kulcsot, de egy alkalmazás, beállítás határozza meg, amely a tényleges API-kulcs jelöli.</span><span class="sxs-lookup"><span data-stu-id="693fe-124">The ApiKey field is not your actual SendGrid API key, but an app setting you define that represents your actual API key.</span></span> <span data-ttu-id="693fe-125">A biztonság érdekében a kulcs tárolása így azért, mert a kódok vagy a fájlokat, előfordulhat, hogy ellenőrizni kell a verziókövetési elkülönül.</span><span class="sxs-lookup"><span data-stu-id="693fe-125">Storing your key this way is for security, since it is separate from any code or files that might be checked into source code control.</span></span>

- <span data-ttu-id="693fe-126">Hozzon létre egy **AppSettings** kulcs az függvény alkalmazás **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="693fe-126">Create an **AppSettings** key in your function app's **Application Settings**.</span></span>

 ![Egy Azure-függvény létrehozása](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a><span data-ttu-id="693fe-128">Konfigurálja a SendGrid kimeneti kötések</span><span class="sxs-lookup"><span data-stu-id="693fe-128">Configure SendGrid output bindings</span></span>

<span data-ttu-id="693fe-129">SendGrid áll rendelkezésre, mert az Azure függvény kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="693fe-129">SendGrid is available as an Azure function output binding.</span></span> <span data-ttu-id="693fe-130">A SendGrid létrehozásához kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="693fe-130">To create a SendGrid output binding:</span></span>

1. <span data-ttu-id="693fe-131">Lépjen a **integráció** lapon, a függvény az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="693fe-131">Go to the **Integrate** tab of the function in the Azure portal.</span></span>
2. <span data-ttu-id="693fe-132">Kattintson a **új kimeneti** létrehozása a SendGrid kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="693fe-132">Click **New Output** to create a SendGrid output binding.</span></span>
3. <span data-ttu-id="693fe-133">Töltse ki a **API-kulcs** és **üzenet paraméternév** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="693fe-133">Fill in the **API Key** and **Message parameter name** properties.</span></span> <span data-ttu-id="693fe-134">Ha azt szeretné, adja meg a többi tulajdonság most, vagy inkább code őket.</span><span class="sxs-lookup"><span data-stu-id="693fe-134">If you want, you can enter the other properties now, or code them instead.</span></span> <span data-ttu-id="693fe-135">Ezek a beállítások alapértelmezett értéke használható.</span><span class="sxs-lookup"><span data-stu-id="693fe-135">These settings can be used as defaults.</span></span>

 ![Konfigurálja a SendGrid kimeneti kötések](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

<span data-ttu-id="693fe-137">Egy olyan függvényt ad hozzá egy kötés létrehoz egy nevű **function.json** a függvény mappában.</span><span class="sxs-lookup"><span data-stu-id="693fe-137">Adding a binding to a function creates a file called **function.json** in your function's folder.</span></span> <span data-ttu-id="693fe-138">Ez a fájl tartalmazza ugyanazokat az Azure függvényben információt **integráció** lapon, de a Json formátumban.</span><span class="sxs-lookup"><span data-stu-id="693fe-138">This file contains all the same information that you see in the Azure function's **Integrate** tab, but in Json format.</span></span> <span data-ttu-id="693fe-139">Beállítás a **ApiKey**, **üzenet**, és **a** mezők létrehozása a következő bejegyzések kerülhetnek a **function.json** fájlt:</span><span class="sxs-lookup"><span data-stu-id="693fe-139">Setting the **ApiKey**, **message**, and **from** fields create the following entries in the **function.json** file:</span></span> 

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

<span data-ttu-id="693fe-140">Tetszés szerint módosíthatja a fájlt közvetlenül a saját maga.</span><span class="sxs-lookup"><span data-stu-id="693fe-140">If you prefer, you may modify this file yourself directly.</span></span>

<span data-ttu-id="693fe-141">Most, hogy létrehozta és konfigurálva, a függvény alkalmazás és a függvény, egy e-mailt küldhet a kódot is írhat.</span><span class="sxs-lookup"><span data-stu-id="693fe-141">Now that you have created and configured the Function App and function, you can write the code to send an email.</span></span>

## <a name="write-code-that-creates-and-sends-email"></a><span data-ttu-id="693fe-142">Kód írása, amely létrehoz és e-mailt küld</span><span class="sxs-lookup"><span data-stu-id="693fe-142">Write code that creates and sends email</span></span>

<span data-ttu-id="693fe-143">A SendGrid API létrehozása és elküldése e-mailt kell minden parancsot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="693fe-143">The SendGrid API contains all the commands you need to create and send an email.</span></span>  

- <span data-ttu-id="693fe-144">Cserélje le a kódot, a függvény a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="693fe-144">Replace the code in the function with the following code:</span></span>

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
    // change to email of recipient
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

<span data-ttu-id="693fe-145">Figyelje meg, az első sor tartalmazza a ```#r``` direktíva, amely a SendGrid szerelvényre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="693fe-145">Notice the first line contains the ```#r``` directive that references the SendGrid assembly.</span></span> <span data-ttu-id="693fe-146">Ezután használhatja a ```using``` utasítás használatával egyszerűen hozzáférhessenek az objektumok adott névtérben.</span><span class="sxs-lookup"><span data-stu-id="693fe-146">After that, you can use a ```using``` statement to more easily access the objects in that namespace.</span></span> <span data-ttu-id="693fe-147">A kód létrehozása a példányai ```Mail```, ```Personalization```, és ```Content``` a SendGrid API az e-mailt alkotó objektumokat.</span><span class="sxs-lookup"><span data-stu-id="693fe-147">In the code, create instances of ```Mail```, ```Personalization```, and ```Content``` objects from the SendGrid API that compose the email.</span></span> <span data-ttu-id="693fe-148">Amikor visszatér az üzenetet, a SendGrid nyújt.</span><span class="sxs-lookup"><span data-stu-id="693fe-148">When you return the message, SendGrid delivers it.</span></span> 

<span data-ttu-id="693fe-149">A függvényaláíráshoz is tartalmaz egy plusz típusú paraméter out ```Mail``` nevű ```message```.</span><span class="sxs-lookup"><span data-stu-id="693fe-149">The function's signature also contains an extra out parameter of type ```Mail``` named ```message```.</span></span> <span data-ttu-id="693fe-150">Mindkét bemeneti és kimeneti kötések express maguk függvény paramétereiben kódot.</span><span class="sxs-lookup"><span data-stu-id="693fe-150">Both input and output bindings express themselves as function parameters in code.</span></span> 

2. <span data-ttu-id="693fe-151">Gombra kattintva tesztelheti a kódját **teszt** , majd gépelje be az üzenetet a **Request body** mezőben, majd kattintson a **futtatása** gombra.</span><span class="sxs-lookup"><span data-stu-id="693fe-151">Test your code by clicking **Test** and entering a message into the **Request body** field, then clicking the **Run** button.</span></span>

 ![Tesztelheti a kódját](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. <span data-ttu-id="693fe-153">Győződjön meg arról, hogy a SendGrid az e-mailben küldött e-mailek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="693fe-153">Check email to verify that SendGrid sent the email.</span></span> <span data-ttu-id="693fe-154">Azt kell nyissa meg a címet a kódban 1. lépésben, és a üzenete tartalmazza a **Request body**.</span><span class="sxs-lookup"><span data-stu-id="693fe-154">It should go to the address in the code from step 1, and contain the message from the **Request body**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="693fe-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="693fe-155">Next steps</span></span>
<span data-ttu-id="693fe-156">Ez a cikk bemutatta, hogyan használható a SendGrid szolgáltatás létrehozásához és e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="693fe-156">This article has demonstrated how to use the SendGrid service to create and send email.</span></span> <span data-ttu-id="693fe-157">Az Azure Functions alkalmazásokban való használatáról a következő témakörökben talál további információt:</span><span class="sxs-lookup"><span data-stu-id="693fe-157">To learn more about using Azure Functions in your apps, see the following topics:</span></span> 

- <span data-ttu-id="693fe-158">[Gyakorlati tanácsok az Azure Functions](functions-best-practices.md) néhány ajánlott eljárás az Azure Functions létrehozásakor használandó sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="693fe-158">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices to use when creating Azure Functions.</span></span>

- <span data-ttu-id="693fe-159">[Az Azure Functions fejlesztői segédanyagai](functions-reference.md) programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="693fe-159">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>

- <span data-ttu-id="693fe-160">[Az Azure Functions tesztelése](functions-test-a-function.md) különböző eszközöket és technikákat a funkciók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="693fe-160">[Testing Azure Functions](functions-test-a-function.md) Describes various tools and techniques for testing your functions.</span></span>