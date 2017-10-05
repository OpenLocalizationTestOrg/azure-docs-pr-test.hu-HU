---
title: "Egy Azure Automation-runbook kezdődő, és olyan webhook |} Microsoft Docs"
description: "A webhook, amely lehetővé teszi az ügyfél elindít egy forgatókönyvet az Azure Automation egy HTTP-hívás.  Ez a cikk ismerteti a webhook létrehozása, és hogyan hívhatja meg egy runbook indítása."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 6c65427fcd18e41a90dfb872aa9525f758b17b87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a><span data-ttu-id="bba17-104">Egy Azure Automation-runbook kezdődő, és olyan webhook</span><span class="sxs-lookup"><span data-stu-id="bba17-104">Starting an Azure Automation runbook with a webhook</span></span>
<span data-ttu-id="bba17-105">A *webhook* lehetővé teszi az adott forgatókönyv indítása az Azure Automationben egyetlen HTTP-kérelem keresztül.</span><span class="sxs-lookup"><span data-stu-id="bba17-105">A *webhook* allows you to start a particular runbook in Azure Automation through a single HTTP request.</span></span> <span data-ttu-id="bba17-106">Ez lehetővé teszi, hogy a külső szolgáltatások, például a Visual Studio Team Services, GitHub, a Microsoft Operations Management Suite Naplóelemzési vagy egy Azure Automation API használatával teljes megoldás megvalósításának nélküli runbookok elindítását egyéni alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="bba17-106">This allows external services such as Visual Studio Team Services, GitHub, Microsoft Operations Management Suite Log Analytics, or custom applications to start runbooks without implementing a full solution using the Azure Automation API.</span></span>  
<span data-ttu-id="bba17-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span><span class="sxs-lookup"><span data-stu-id="bba17-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span></span>

<span data-ttu-id="bba17-108">Összehasonlíthatja az egyéb módszerek runbook indítása webhook [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="bba17-108">You can compare webhooks to other methods of starting a runbook in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)</span></span>

## <a name="details-of-a-webhook"></a><span data-ttu-id="bba17-109">A webhook részleteit</span><span class="sxs-lookup"><span data-stu-id="bba17-109">Details of a webhook</span></span>
<span data-ttu-id="bba17-110">A következő táblázat ismerteti a tulajdonságokat, amelyeket konfigurálnia kell egy webhook.</span><span class="sxs-lookup"><span data-stu-id="bba17-110">The following table describes the properties that you must configure for a webhook.</span></span>

| <span data-ttu-id="bba17-111">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bba17-111">Property</span></span> | <span data-ttu-id="bba17-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="bba17-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="bba17-113">Név</span><span class="sxs-lookup"><span data-stu-id="bba17-113">Name</span></span> |<span data-ttu-id="bba17-114">Megadhat egy nevet, egy webhook óta ez nincs felfedve, az ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="bba17-114">You can provide any name you want for a webhook since this is not exposed to the client.</span></span>  <span data-ttu-id="bba17-115">Azt csak az Ön azonosítására szolgál a runbook az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="bba17-115">It is only used for you to identify the runbook in Azure Automation.</span></span> <br>  <span data-ttu-id="bba17-116">Ajánlott eljárásként adjon a webhook kapcsolódik az ügyfél által használt nevet.</span><span class="sxs-lookup"><span data-stu-id="bba17-116">As a best practice, you should give the webhook a name related to the client that will use it.</span></span> |
| <span data-ttu-id="bba17-117">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="bba17-117">URL</span></span> |<span data-ttu-id="bba17-118">A webhook URL-címe az ügyfelek egy HTTP POST a webhook csatolva a runbook elindításához hívja a egyedi cím.</span><span class="sxs-lookup"><span data-stu-id="bba17-118">The URL of the webhook is the unique address that a client calls with an HTTP POST to start the runbook linked to the webhook.</span></span>  <span data-ttu-id="bba17-119">A webhook létrehozásakor automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="bba17-119">It is automatically generated when you create the webhook.</span></span>  <span data-ttu-id="bba17-120">Egy egyéni URL-címe nem adható meg.</span><span class="sxs-lookup"><span data-stu-id="bba17-120">You cannot specify a custom URL.</span></span> <br> <br>  <span data-ttu-id="bba17-121">Az URL-cím egy biztonsági jogkivonatot, amely lehetővé teszi a forgatókönyv további hitelesítés nélküli külső rendszer által meghívandó tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bba17-121">The URL contains a security token that allows the runbook to be invoked by a third party system with no further authentication.</span></span> <span data-ttu-id="bba17-122">Ezért azt kell kezelni, például a jelszó.</span><span class="sxs-lookup"><span data-stu-id="bba17-122">For this reason, it should be treated like a password.</span></span>  <span data-ttu-id="bba17-123">Biztonsági okokból csak megtekintheti az URL-cím az Azure portálon, a rendszer a webhook létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="bba17-123">For security reasons, you can only view the URL in the Azure portal at the time the webhook is created.</span></span> <span data-ttu-id="bba17-124">Vegye figyelembe a jövőbeli használatra egy biztonságos helyre az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bba17-124">You should note the URL in a secure location for future use.</span></span> |
| <span data-ttu-id="bba17-125">Lejárat dátuma</span><span class="sxs-lookup"><span data-stu-id="bba17-125">Expiration date</span></span> |<span data-ttu-id="bba17-126">Például egy tanúsítványt egyes webhook van ekkor már nem használható lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="bba17-126">Like a certificate, each webhook has an expiration date at which time it can no longer be used.</span></span>  <span data-ttu-id="bba17-127">A lejárati dátumot a webhook létrehozása után módosítható.</span><span class="sxs-lookup"><span data-stu-id="bba17-127">This expiration date can be modified after the webhook is created.</span></span> |
| <span data-ttu-id="bba17-128">Engedélyezve</span><span class="sxs-lookup"><span data-stu-id="bba17-128">Enabled</span></span> |<span data-ttu-id="bba17-129">A webhook alapértelmezés szerint engedélyezve van, ha létrehozták.</span><span class="sxs-lookup"><span data-stu-id="bba17-129">A webhook is enabled by default when it is created.</span></span>  <span data-ttu-id="bba17-130">Ha beállította azt le van tiltva, akkor nincs ügyfél lesz használni tudja.</span><span class="sxs-lookup"><span data-stu-id="bba17-130">If you set it to Disabled, then no client will be able to use it.</span></span>  <span data-ttu-id="bba17-131">Beállíthatja a **engedélyezve** tulajdonság a webhook, vagy bármikor egyszer létrehozásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="bba17-131">You can set the **Enabled** property when you create the webhook or anytime once it is created.</span></span> |

### <a name="parameters"></a><span data-ttu-id="bba17-132">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bba17-132">Parameters</span></span>
<span data-ttu-id="bba17-133">A webhook runbook paramétereket, illetve ha a runbook indítja el, hogy a webhook értékeket határozhat meg.</span><span class="sxs-lookup"><span data-stu-id="bba17-133">A webhook can define values for runbook parameters that are used when the runbook is started by that webhook.</span></span> <span data-ttu-id="bba17-134">A webhook tartalmaznia kell a runbook minden kötelező paraméter értékét, és nem kötelező paraméter értékét is járhatott.</span><span class="sxs-lookup"><span data-stu-id="bba17-134">The webhook must include values for any mandatory parameters of the runbook and may include values for optional parameters.</span></span> <span data-ttu-id="bba17-135">A paraméter értékét, úgy konfigurálva, hogy a webhook még a webhoook létrehozása után módosítható.</span><span class="sxs-lookup"><span data-stu-id="bba17-135">A parameter value configured to a webhook can be modified even after creating the webhoook.</span></span> <span data-ttu-id="bba17-136">Kapcsolódó egyetlen runbook több webhook egyes eltérő értékek használhatók.</span><span class="sxs-lookup"><span data-stu-id="bba17-136">Multiple webhooks linked to a single runbook can each use different parameter values.</span></span>

<span data-ttu-id="bba17-137">Amikor egy ügyfél elindítja a runbookot egy webhook, nem bírálhatja felül a webhook definiált paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="bba17-137">When a client starts a runbook using a webhook, it cannot override the parameter values defined in the webhook.</span></span>  <span data-ttu-id="bba17-138">Az adatok fogadásához az ügyfél a runbook neve egyetlen paramétert fogadhat **$WebhookData** típusú [objektum], amely az ügyfél a POST kérelemben tartalmazó adatok fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="bba17-138">To receive data from the client, the runbook can accept a single parameter called **$WebhookData** of type [object] that will contain data that the client includes in the POST request.</span></span>

![Webhookdata tulajdonságai](media/automation-webhooks/webhook-data-properties.png)

<span data-ttu-id="bba17-140">A **$WebhookData** objektum lesz a következő jellemzőkkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="bba17-140">The **$WebhookData** object will have the following properties:</span></span>

| <span data-ttu-id="bba17-141">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bba17-141">Property</span></span> | <span data-ttu-id="bba17-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="bba17-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="bba17-143">WebhookName</span><span class="sxs-lookup"><span data-stu-id="bba17-143">WebhookName</span></span> |<span data-ttu-id="bba17-144">A webhook nevét.</span><span class="sxs-lookup"><span data-stu-id="bba17-144">The name of the webhook.</span></span> |
| <span data-ttu-id="bba17-145">RequestHeader</span><span class="sxs-lookup"><span data-stu-id="bba17-145">RequestHeader</span></span> |<span data-ttu-id="bba17-146">A kivonattábla a fejléceket a bejövő POST kérelem tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="bba17-146">Hash table containing the headers of the incoming POST request.</span></span> |
| <span data-ttu-id="bba17-147">requestBody</span><span class="sxs-lookup"><span data-stu-id="bba17-147">RequestBody</span></span> |<span data-ttu-id="bba17-148">A bejövő POST kérelem törzsét.</span><span class="sxs-lookup"><span data-stu-id="bba17-148">The body of the incoming POST request.</span></span>  <span data-ttu-id="bba17-149">Ez megőrzi a formázási karakterlánc, JSON, XML, például vagy űrlap kódolású adatot.</span><span class="sxs-lookup"><span data-stu-id="bba17-149">This will retain any formatting such as string, JSON, XML, or form encoded data.</span></span> <span data-ttu-id="bba17-150">A runbook kell írni a várt adatformátum dolgozni.</span><span class="sxs-lookup"><span data-stu-id="bba17-150">The runbook must be written to work with the data format that is expected.</span></span> |

<span data-ttu-id="bba17-151">A webhook támogatásához szükséges konfiguráció hiányában az **$WebhookData** paraméter, és a runbook nem szükséges elfogadását.</span><span class="sxs-lookup"><span data-stu-id="bba17-151">There is no configuration of the webhook required to support the **$WebhookData** parameter, and the runbook is not required to accept it.</span></span>  <span data-ttu-id="bba17-152">A runbook nem határoz meg a paramétert, ha az ügyfél által küldött kérelem részleteinek figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="bba17-152">If the runbook does not define the parameter, then any details of the request sent from the client is ignored.</span></span>

<span data-ttu-id="bba17-153">Ha megad egy értéket a $WebhookData a webhook létrehozásakor, hogy értéket fogja felülbírálva a webhook elindítja a runbookot az adatokat az ügyfél POST-kérelmet, ha akkor is, ha az ügyfél nem vesz be semmilyen adatot a kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="bba17-153">If you specify a value for $WebhookData when you create the webhook, that value will be overriden when the webhook starts the runbook with the data from the client POST request, even if the client does not include any data in the request body.</span></span>  <span data-ttu-id="bba17-154">Ha elindít egy forgatókönyvet, amely rendelkezik egy webhook eltérő módszerrel $WebhookData, biztosíthatja, hogy a runbook felismer $Webhookdata értéket.</span><span class="sxs-lookup"><span data-stu-id="bba17-154">If you start a runbook that has $WebhookData using a method other than a webhook, you can provide a value for $Webhookdata that will be recognized by the runbook.</span></span>  <span data-ttu-id="bba17-155">Ennek az értéknek kell lennie egy objektum ugyanezzel [tulajdonságok](#details-of-a-webhook) $Webhookdata, így a runbook megfelelően dolgozhat, mintha volt az olyan webhook által átadott tényleges WebhookData használata.</span><span class="sxs-lookup"><span data-stu-id="bba17-155">This value should be an object with the same [properties](#details-of-a-webhook) as $Webhookdata so that the runbook can properly work with it as if it was working with actual WebhookData passed by a webhook.</span></span>

<span data-ttu-id="bba17-156">Például ha vannak a következő runbook indítása az Azure portálról, és hogy bizonyos minta WebhookData tesztelési, mivel WebhookData objektum, azt kell átadni a felhasználói felületen JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="bba17-156">For example, if you are starting the following runbook from the Azure Portal and want to pass some sample WebhookData for testing, since WebhookData is an object, it should be passed as JSON in the UI.</span></span>

![A felhasználói Felületről WebhookData paraméter](media/automation-webhooks/WebhookData-parameter-from-UI.png)

<span data-ttu-id="bba17-158">A fenti runbookhoz, ha a WebhookData paraméter a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="bba17-158">For the above runbook, if you have the following properties for the WebhookData parameter:</span></span>

1. <span data-ttu-id="bba17-159">WebhookName: *MyWebhook*</span><span class="sxs-lookup"><span data-stu-id="bba17-159">WebhookName: *MyWebhook*</span></span>
2. <span data-ttu-id="bba17-160">RequestHeader: *= teszt felhasználó*</span><span class="sxs-lookup"><span data-stu-id="bba17-160">RequestHeader: *From=Test User*</span></span>
3. <span data-ttu-id="bba17-161">RequestBody: *["VM1", "Vm2 virtuális gépnek"]*</span><span class="sxs-lookup"><span data-stu-id="bba17-161">RequestBody: *[“VM1”, “VM2”]*</span></span>

<span data-ttu-id="bba17-162">A felhasználói felületen a WebhookData paraméter majd volna adja át a következő JSON-érték:</span><span class="sxs-lookup"><span data-stu-id="bba17-162">Then you would pass the following JSON value in the UI for the WebhookData parameter:</span></span>  

* <span data-ttu-id="bba17-163">{"WebhookName": "MyWebhook", "RequestHeader": {"Feladó": "Felhasználói teszt"}, "RequestBody": "[\"VM1\",\"vm2 virtuális gépnek\"]"}</span><span class="sxs-lookup"><span data-stu-id="bba17-163">{"WebhookName":"MyWebhook", "RequestHeader":{"From":"Test User"}, "RequestBody":"[\"VM1\",\"VM2\"]"}</span></span>

![Kezdő WebhookData paraméter a felhasználói Felületről](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> <span data-ttu-id="bba17-165">A runbook-feladat az összes bemeneti paraméterek naplózása.</span><span class="sxs-lookup"><span data-stu-id="bba17-165">The values of all input parameters are logged with the runbook job.</span></span>  <span data-ttu-id="bba17-166">Ez azt jelenti, hogy minden a webhook kérelmet az ügyfél által megadott naplózott és az automation-feladat hozzáféréssel rendelkező bárki számára elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="bba17-166">This means that any input provided by the client in the webhook request will be logged and available to anyone with access to the automation job.</span></span>  <span data-ttu-id="bba17-167">Emiatt a webhook hívások a bizalmas adatokat is beleértve elővigyázatosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bba17-167">For this reason, you should be cautious about including sensitive information in webhook calls.</span></span>
>

## <a name="security"></a><span data-ttu-id="bba17-168">Biztonság</span><span class="sxs-lookup"><span data-stu-id="bba17-168">Security</span></span>
<span data-ttu-id="bba17-169">A webhook biztonságát az URL-CÍMÉT, amely egy biztonsági jogkivonatot tartalmaz, amely lehetővé teszi, hogy meg kell hívni az adatvédelmi támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="bba17-169">The security of a webhook relies on the privacy of its URL which contains a security token that allows it to be invoked.</span></span> <span data-ttu-id="bba17-170">Azure Automation szolgáltatásbeli nem végzi el a hitelesítési a kérelem mindaddig, amíg a helyes URL-címet teszi.</span><span class="sxs-lookup"><span data-stu-id="bba17-170">Azure Automation does not perform any authentication on the request as long as it is made to the correct URL.</span></span> <span data-ttu-id="bba17-171">Emiatt webhookok nem használható a runbookok, amelyek a szigorúan bizalmas valamilyen alternatív, a kérelem ellenőrzése használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="bba17-171">For this reason, webhooks should not be used for runbooks that perform highly sensitive functions without using an alternate means of validating the request.</span></span>

<span data-ttu-id="bba17-172">Megadhatja, hogy meghívták volna a webhook által úgy, hogy a runbookban logika a **WebhookName** tulajdonság a $WebhookData paraméter.</span><span class="sxs-lookup"><span data-stu-id="bba17-172">You can include logic within the runbook to determine that it was called by a webhook by checking the **WebhookName** property of the $WebhookData parameter.</span></span> <span data-ttu-id="bba17-173">A runbook volt az ellenőrzéshez további az adott információkat keres a **RequestHeader** vagy **RequestBody** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="bba17-173">The runbook could perform further validation by looking for particular information in the **RequestHeader** or **RequestBody** properties.</span></span>

<span data-ttu-id="bba17-174">Egy másik olyan stratégia, hogy a runbook egy külső állapot néhány ellenőrzés végrehajtása, ha webhook kérelmet kapott.</span><span class="sxs-lookup"><span data-stu-id="bba17-174">Another strategy is to have the runbook perform some validation of an external condition when it received a webhook request.</span></span>  <span data-ttu-id="bba17-175">Tegyük fel, ha egy új véglegesítés egy GitHub-tárházban GitHub hívja runbook.</span><span class="sxs-lookup"><span data-stu-id="bba17-175">For example, consider a runbook that is called by GitHub whenever there is a new commit to a GitHub repository.</span></span>  <span data-ttu-id="bba17-176">A runbook kapcsolódását GitHub ellenőrzése, hogy egy új véglegesítési valójában történt volna a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="bba17-176">The runbook might connect to GitHub to validate that a new commit had actually just occurred before continuing.</span></span>

## <a name="creating-a-webhook"></a><span data-ttu-id="bba17-177">A webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="bba17-177">Creating a webhook</span></span>
<span data-ttu-id="bba17-178">A következő eljárással hozhat létre egy új webhook csatolva egy runbookot, az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bba17-178">Use the following procedure to create a new webhook linked to a runbook in the Azure portal.</span></span>

1. <span data-ttu-id="bba17-179">Az a **Runbookok panel** a runbookot elindító a webhook megtekintéséhez a részletek panelen kattintson az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bba17-179">From the **Runbooks blade** in the Azure portal, click the runbook that the webhook will start to view its detail blade.</span></span>
2. <span data-ttu-id="bba17-180">Kattintson a **Webhook** lehetőségre a panel tetején a **hozzáadása Webhook** panelen.</span><span class="sxs-lookup"><span data-stu-id="bba17-180">Click **Webhook** at the top of the blade to open the **Add Webhook** blade.</span></span> <br><span data-ttu-id="bba17-181">
   ![Webhook gomb](media/automation-webhooks/webhooks-button.png)</span><span class="sxs-lookup"><span data-stu-id="bba17-181">
![Webhooks button](media/automation-webhooks/webhooks-button.png)</span></span>
3. <span data-ttu-id="bba17-182">Kattintson a **hozzon létre új webhook** megnyitásához a **létrehozás webhook panel**.</span><span class="sxs-lookup"><span data-stu-id="bba17-182">Click **Create new webhook** to open the **Create webhook blade**.</span></span>
4. <span data-ttu-id="bba17-183">Adjon meg egy **neve**, **lejárati dátum** a webhook, és hogy azt engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="bba17-183">Specify a **Name**, **Expiration Date** for the webhook and whether it should be enabled.</span></span> <span data-ttu-id="bba17-184">Lásd: [olyan webhook részleteit](#details-of-a-webhook) további információt ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="bba17-184">See [Details of a webhook](#details-of-a-webhook) for more information these properties.</span></span>
5. <span data-ttu-id="bba17-185">A Másolás ikonra, majd nyomja meg a Ctrl + C billentyűkombinációval a webhook URL-címét.</span><span class="sxs-lookup"><span data-stu-id="bba17-185">Click the copy icon and press Ctrl+C to copy the URL of the webhook.</span></span>  <span data-ttu-id="bba17-186">Biztonságos helyen, majd rögzítse azt.</span><span class="sxs-lookup"><span data-stu-id="bba17-186">Then record it in a safe place.</span></span>  <span data-ttu-id="bba17-187">**A webhook létrehozása után újra az URL-cím nem lehet beolvasni.**</span><span class="sxs-lookup"><span data-stu-id="bba17-187">**Once you create the webhook, you cannot retrieve the URL again.**</span></span> <br><span data-ttu-id="bba17-188">
   ![Webhook URL-CÍMÉT](media/automation-webhooks/copy-webhook-url.png)</span><span class="sxs-lookup"><span data-stu-id="bba17-188">
![Webhook URL](media/automation-webhooks/copy-webhook-url.png)</span></span>
6. <span data-ttu-id="bba17-189">Kattintson a **paraméterek** a runbook-paraméterek értékének megadására.</span><span class="sxs-lookup"><span data-stu-id="bba17-189">Click **Parameters** to provide values for the runbook parameters.</span></span>  <span data-ttu-id="bba17-190">Ha a runbook kötelező paraméterekkel rendelkezik, majd csak akkor hozhat létre a webhook, kivéve, ha az értékek.</span><span class="sxs-lookup"><span data-stu-id="bba17-190">If the runbook has mandatory parameters, then you will not be able to create the webhook unless values are provided.</span></span>
7. <span data-ttu-id="bba17-191">Kattintson a **létrehozása** a webhook létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bba17-191">Click **Create** to create the webhook.</span></span>

## <a name="using-a-webhook"></a><span data-ttu-id="bba17-192">A webhook használatával</span><span class="sxs-lookup"><span data-stu-id="bba17-192">Using a webhook</span></span>
<span data-ttu-id="bba17-193">A webhook létrehozása után használatához az ügyfélalkalmazás egy HTTP POST a webhook URL-címmel kell kiadnia.</span><span class="sxs-lookup"><span data-stu-id="bba17-193">To use a webhook after it has been created, your client application must issue an HTTP POST with the URL for the webhook.</span></span>  <span data-ttu-id="bba17-194">A webhook szintaxisa a következő formátumban lesz.</span><span class="sxs-lookup"><span data-stu-id="bba17-194">The syntax of the webhook will be in the following format.</span></span>

    http://<Webhook Server>/token?=<Token Value>

<span data-ttu-id="bba17-195">Az ügyfél kap a következő visszatérési kódok a POST-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="bba17-195">The client will receive one of the following return codes from the POST request.</span></span>  

| <span data-ttu-id="bba17-196">Kód</span><span class="sxs-lookup"><span data-stu-id="bba17-196">Code</span></span> | <span data-ttu-id="bba17-197">Szöveg</span><span class="sxs-lookup"><span data-stu-id="bba17-197">Text</span></span> | <span data-ttu-id="bba17-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="bba17-198">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="bba17-199">202</span><span class="sxs-lookup"><span data-stu-id="bba17-199">202</span></span> |<span data-ttu-id="bba17-200">Elfogadva</span><span class="sxs-lookup"><span data-stu-id="bba17-200">Accepted</span></span> |<span data-ttu-id="bba17-201">Elfogadta a kérést, és a runbook sikeresen várólistára került.</span><span class="sxs-lookup"><span data-stu-id="bba17-201">The request was accepted, and the runbook was successfully queued.</span></span> |
| <span data-ttu-id="bba17-202">400</span><span class="sxs-lookup"><span data-stu-id="bba17-202">400</span></span> |<span data-ttu-id="bba17-203">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="bba17-203">Bad Request</span></span> |<span data-ttu-id="bba17-204">A kérelem nem fogadták a következő okok valamelyike miatt.</span><span class="sxs-lookup"><span data-stu-id="bba17-204">The request was not accepted for one of the following reasons.</span></span> <ul> <li><span data-ttu-id="bba17-205">A webhook érvényessége lejárt.</span><span class="sxs-lookup"><span data-stu-id="bba17-205">The webhook has expired.</span></span></li> <li><span data-ttu-id="bba17-206">A webhook le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="bba17-206">The webhook is disabled.</span></span></li> <li><span data-ttu-id="bba17-207">A lexikális elem szerepel az URL-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="bba17-207">The token in the URL is invalid.</span></span></li>  </ul> |
| <span data-ttu-id="bba17-208">404</span><span class="sxs-lookup"><span data-stu-id="bba17-208">404</span></span> |<span data-ttu-id="bba17-209">Nem található</span><span class="sxs-lookup"><span data-stu-id="bba17-209">Not Found</span></span> |<span data-ttu-id="bba17-210">A kérelem nem fogadták a következő okok valamelyike miatt.</span><span class="sxs-lookup"><span data-stu-id="bba17-210">The request was not accepted for one of the following reasons.</span></span> <ul> <li><span data-ttu-id="bba17-211">A webhook nem található.</span><span class="sxs-lookup"><span data-stu-id="bba17-211">The webhook was not found.</span></span></li> <li><span data-ttu-id="bba17-212">A runbook nem található.</span><span class="sxs-lookup"><span data-stu-id="bba17-212">The runbook was not found.</span></span></li> <li><span data-ttu-id="bba17-213">A fiók nem található.</span><span class="sxs-lookup"><span data-stu-id="bba17-213">The account was not found.</span></span></li>  </ul> |
| <span data-ttu-id="bba17-214">500</span><span class="sxs-lookup"><span data-stu-id="bba17-214">500</span></span> |<span data-ttu-id="bba17-215">Belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="bba17-215">Internal Server Error</span></span> |<span data-ttu-id="bba17-216">Az URL-cím érvénytelen volt, de hiba történt.</span><span class="sxs-lookup"><span data-stu-id="bba17-216">The URL was valid, but an error occurred.</span></span>  <span data-ttu-id="bba17-217">Küldje el a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="bba17-217">Please resubmit the request.</span></span> |

<span data-ttu-id="bba17-218">Feltéve, hogy a kérelem sikeres, a webhook válasz tartalmazza a feladatazonosítót JSON formátumban az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="bba17-218">Assuming the request is successful, the webhook response contains the job id in JSON format as follows.</span></span> <span data-ttu-id="bba17-219">Egyetlen feladatazonosító fogja tartalmazni, de lehetséges jövőbeli fejlesztések lehetővé teszi a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="bba17-219">It will contain a single job id, but the JSON format allows for potential future enhancements.</span></span>

    {"JobIds":["<JobId>"]}  

<span data-ttu-id="bba17-220">Az ügyfél nem tudja megállapítani a runbook-feladat befejezését vagy a webhook a befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="bba17-220">The client cannot determine when the runbook job completes or its completion status from the webhook.</span></span>  <span data-ttu-id="bba17-221">Ezt az információt a feladatazonosító egy másik módszerrel például meg tudja határozni [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) vagy a [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span><span class="sxs-lookup"><span data-stu-id="bba17-221">It can determine this information using the job id with another method such as [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) or the [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span></span>

### <a name="example"></a><span data-ttu-id="bba17-222">Példa</span><span class="sxs-lookup"><span data-stu-id="bba17-222">Example</span></span>
<span data-ttu-id="bba17-223">Az alábbi példa runbook indítása a webhook a Windows Powershellt használja.</span><span class="sxs-lookup"><span data-stu-id="bba17-223">The following example uses Windows PowerShell to start a runbook with a webhook.</span></span>  <span data-ttu-id="bba17-224">Vegye figyelembe, hogy bármilyen nyelvet használhat, amelyek HTTP-kérelem használhatja a webhook; Windows PowerShell csak használt példa.</span><span class="sxs-lookup"><span data-stu-id="bba17-224">Note that any language that can make an HTTP request can use a webhook; Windows PowerShell is just used here as an example.</span></span>

<span data-ttu-id="bba17-225">A runbook által várt paraméterekkel a kérelem törzsében JSON-formázott virtuális gépek listáját.</span><span class="sxs-lookup"><span data-stu-id="bba17-225">The runbook is expecting a list of virtual machines formatted in JSON in the body of the request.</span></span> <span data-ttu-id="bba17-226">Azt is vannak adatai, például ki elindítja a runbookot és a dátum és idő alatt a kérelem fejlécében elindul.</span><span class="sxs-lookup"><span data-stu-id="bba17-226">We also are including information about who is starting the runbook and the date and time it is being started in the header of the request.</span></span>      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


<span data-ttu-id="bba17-227">A következő kép bemutatja a fejléc-információ (használatával egy [Fiddler](http://www.telerik.com/fiddler) nyomkövetési) a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="bba17-227">The following image shows the header information (using a [Fiddler](http://www.telerik.com/fiddler) trace) from this request.</span></span> <span data-ttu-id="bba17-228">Ez magában foglalja a szabványos fejlécek mellett az egyéni dátum és a hozzáadott fejléceket a HTTP-kérések.</span><span class="sxs-lookup"><span data-stu-id="bba17-228">This includes standard headers of an HTTP request in addition to the custom Date and From headers that we added.</span></span>  <span data-ttu-id="bba17-229">Ezek az értékek egy a runbook számára elérhető a **RequestHeaders** tulajdonsága **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="bba17-229">Each of these values is available to the runbook in the **RequestHeaders** property of **WebhookData**.</span></span>

![Webhook gomb](media/automation-webhooks/webhook-request-headers.png)

<span data-ttu-id="bba17-231">A következő kép bemutatja a kérelem törzse (használatával egy [Fiddler](http://www.telerik.com/fiddler) nyomkövetési), amely a runbookot a rendelkezésére áll a **RequestBody** tulajdonsága **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="bba17-231">The following image shows the body of the request (using a [Fiddler](http://www.telerik.com/fiddler) trace)  that is available to the runbook in the **RequestBody** property of **WebhookData**.</span></span> <span data-ttu-id="bba17-232">A formátum a kérelem törzsében található, mert ez van formázva JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="bba17-232">This is formatted as JSON because that was the format that was included in the body of the request.</span></span>     

![Webhook gomb](media/automation-webhooks/webhook-request-body.png)

<span data-ttu-id="bba17-234">A következő kép bemutatja a Windows PowerShell és az eredményül kapott válasz küldését a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="bba17-234">The following image shows the request being sent from Windows PowerShell and the resulting response.</span></span>  <span data-ttu-id="bba17-235">A feladat azonosítója a válasz kivonjuk és karakterlánccá alakítja át.</span><span class="sxs-lookup"><span data-stu-id="bba17-235">The job id is extracted from the response and converted to a string.</span></span>

![Webhook gomb](media/automation-webhooks/webhook-request-response.png)

<span data-ttu-id="bba17-237">Az alábbi példában a runbook az előző példa kérést fogad, és elindítja a virtuális gépek a kérés törzsében megadott.</span><span class="sxs-lookup"><span data-stu-id="bba17-237">The following sample runbook accepts the previous example request and starts the virtual machines specified in the request body.</span></span>

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate to Azure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean to be started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-to-azure-alerts"></a><span data-ttu-id="bba17-238">Runbookok elindítása Azure riasztás</span><span class="sxs-lookup"><span data-stu-id="bba17-238">Starting runbooks in response to Azure alerts</span></span>
<span data-ttu-id="bba17-239">A runbookok Webhook-kompatibilis használható reagálni [Azure riasztások](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="bba17-239">Webhook-enabled runbooks can be used to react to [Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="bba17-240">Az Azure-erőforrások a statisztikai adatokat, például a teljesítmény, rendelkezésre állását és használati Azure riasztások segítségével-készítés révén figyelhető.</span><span class="sxs-lookup"><span data-stu-id="bba17-240">Resources in Azure can be monitored by collecting the statistics like performance, availability and usage with the help of Azure alerts.</span></span> <span data-ttu-id="bba17-241">Figyelési metrikákat alapuló riasztást kaphat, vagy eseményeket az Azure-erőforrások, jelenleg az Automation-fiókok csak metrikák támogatja.</span><span class="sxs-lookup"><span data-stu-id="bba17-241">You can receive an alert based on monitoring metrics or events for your Azure resources, currently Automation Accounts support only metrics.</span></span> <span data-ttu-id="bba17-242">Ha egy megadott metrika értéke meghaladja a küszöbértéket, hozzárendelt, vagy ha a beállított esemény akkor váltódik ki, akkor a rendszer értesítést küld a szolgáltatás-rendszergazdák vagy a társadminisztrátoroknak a riasztás feloldásához jelenítse meg, további információ a metrikák és események olvassa el [ Az Azure riasztások](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="bba17-242">When the value of a specified metric exceeds the threshold assigned or if the configured event is triggered then a notification is sent to the service admin or co-admins to resolve the alert, for more information on metrics and events please refer to [Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="bba17-243">Használata Azure riasztások értesítési rendszert, mellett is is indítsa el a runbookok riasztás.</span><span class="sxs-lookup"><span data-stu-id="bba17-243">Besides using Azure alerts as a notification system, you can also kick off runbooks in response to alerts.</span></span> <span data-ttu-id="bba17-244">Azure Automation szolgáltatásbeli lehetővé teszi runbookok webhook-kompatibilis Azure riasztások futtatásához.</span><span class="sxs-lookup"><span data-stu-id="bba17-244">Azure Automation provides the capability to run webhook-enabled runbooks with Azure alerts.</span></span> <span data-ttu-id="bba17-245">Ha egy metrika meghaladja a beállított küszöbértéknél majd a riasztási szabály válik aktívvá, és elindítja az automation-webhook, amely ezután végrehajtja a runbookot.</span><span class="sxs-lookup"><span data-stu-id="bba17-245">When a metric exceeds the configured threshold value then the alert rule becomes active and triggers the automation webhook which in turn executes the runbook.</span></span>

![webhook](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a><span data-ttu-id="bba17-247">Riasztás környezete</span><span class="sxs-lookup"><span data-stu-id="bba17-247">Alert context</span></span>
<span data-ttu-id="bba17-248">Érdemes lehet például egy virtuális gépet egy Azure-erőforrás, a CPU-felhasználás gép egyik legfontosabb teljesítményi metrikát.</span><span class="sxs-lookup"><span data-stu-id="bba17-248">Consider an Azure resource such as a virtual machine, CPU utilization of this machine is one of the key performance metric.</span></span> <span data-ttu-id="bba17-249">Ha a Processzor kihasználtsága 100 %-os vagy bizonyos egynél hosszú időn keresztül, érdemes próbálja megoldani a problémát a virtuális gép újraindításához.</span><span class="sxs-lookup"><span data-stu-id="bba17-249">If the CPU utilization is 100% or more than a certain amount for long period of time, you might want to restart the virtual machine to fix the problem.</span></span> <span data-ttu-id="bba17-250">Ez megoldható egy riasztási szabály, amely a virtuális gép konfigurálása, és ez a szabály tart, mint a metrika processzorszázaléka.</span><span class="sxs-lookup"><span data-stu-id="bba17-250">This can be solved by configuring an alert rule to the virtual machine and this rule takes CPU percentage as its metric.</span></span> <span data-ttu-id="bba17-251">Itt processzorszázaléka csak példaként lesz végrehajtva, de nincsenek sok más metrikákkal konfigurálható az Azure-erőforrások és a virtuális gép újraindítása egy műveletet, amelyekre szükség van, a probléma megoldásához, konfigurálhatja a forgatókönyvet, hogy más műveletek.</span><span class="sxs-lookup"><span data-stu-id="bba17-251">CPU percentage here is just taken as an example but there are many other metrics that you can configure to your Azure resources and restarting the virtual machine is an action that is taken to fix this issue, you can configure the runbook to take other actions.</span></span>

<span data-ttu-id="bba17-252">Ha ez a riasztási szabály válik aktívvá, és elindítja a webhook-kompatibilis runbook küld a riasztási környezetben a runbookot.</span><span class="sxs-lookup"><span data-stu-id="bba17-252">When this the alert rule becomes active and triggers the webhook-enabled runbook, it sends the alert context to the runbook.</span></span> <span data-ttu-id="bba17-253">[Riasztás környezete](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) tartalmaz információt talál többek között **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** és **időbélyeg** a forgatókönyvet, hogy az erőforrás, amely a művelet azt véve azonosításához szükséges.</span><span class="sxs-lookup"><span data-stu-id="bba17-253">[Alert context](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) contains details including **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** and **Timestamp** which are required for the runbook to identify the resource on which it will be taking action.</span></span> <span data-ttu-id="bba17-254">Riasztási környezet a szervezet részét beágyazott a **WebhookData** elküldi a runbook és az objektum az elérhető **Webhook.RequestBody** tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bba17-254">Alert context is embedded in the body part of the **WebhookData** object sent to the runbook and it can be accessed with **Webhook.RequestBody** property</span></span>

### <a name="example"></a><span data-ttu-id="bba17-255">Példa</span><span class="sxs-lookup"><span data-stu-id="bba17-255">Example</span></span>
<span data-ttu-id="bba17-256">Hozzon létre egy Azure virtuális gép előfizetését és társítható egy [CPU százalékos metrika figyelése riasztás](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="bba17-256">Create an Azure virtual machine in your subscription and associate an [alert to monitor CPU percentage metric](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="bba17-257">Riasztás létrehozása során győződjön meg arról, hogy feltölti a webhook mező a webhook létrehozása során létrehozó webhook URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bba17-257">While creating the alert make sure you populate the webhook field with the URL of the webhook which was generated while creating the webhook.</span></span>

<span data-ttu-id="bba17-258">Az alábbi példában a runbook lesz kiváltva, ha a riasztási szabály válik aktívvá, és összegyűjti a riasztás környezete paraméterek, amelyek szükségesek a forgatókönyvet, hogy az erőforrás, amely a művelet azt véve azonosítása.</span><span class="sxs-lookup"><span data-stu-id="bba17-258">The following sample runbook is triggered when the alert rule becomes active and it collects the Alert context parameters which are required for the runbook to identify the resource on which it will be taking action.</span></span>

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on the webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain the WebhookBody containing the AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain the AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on the AlertContext data, in our case restarting the VM.
            # Authenticate to your Azure subscription using Organization ID to be able to restart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check the status property of the VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart the VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant to only be started from a webhook."  
        }  
    }



## <a name="next-steps"></a><span data-ttu-id="bba17-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bba17-259">Next steps</span></span>
* <span data-ttu-id="bba17-260">A runbook-indítási különböző módokon részletekért lásd: [Runbook indítása](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="bba17-260">For details on different ways to start a runbook, see [Starting a Runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="bba17-261">A Runbook-feladatok állapotának megtekintése a tudnivalókat lásd a [a Runbook végrehajtása az Azure Automationben](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="bba17-261">For information on viewing the Status of a Runbook Job, refer to [Runbook execution in Azure Automation](automation-runbook-execution.md).</span></span>
* <span data-ttu-id="bba17-262">Azure Automation műveletet végrehajthat Azure riasztások használatával kapcsolatban lásd: [javítása Azure virtuális gép riasztások Automation-Runbook](automation-azure-vm-alert-integration.md).</span><span class="sxs-lookup"><span data-stu-id="bba17-262">To learn how to use Azure Automation to take action on Azure Alerts, see [Remediate Azure VM Alerts with Automation Runbooks](automation-azure-vm-alert-integration.md).</span></span>
