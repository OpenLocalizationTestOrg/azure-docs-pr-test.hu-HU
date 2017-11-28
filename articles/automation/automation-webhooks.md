---
title: a webhook az Azure Automation-runbook aaaStarting |} Microsoft Docs
description: "A webhook, amely lehetővé teszi, hogy egy ügyfél toostart egy runbook az Azure Automationben egy HTTP-hívás.  Ez a cikk ismerteti, hogyan toocreate egy webhook és hogyan toocall egy toostart egy runbookot."
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
ms.openlocfilehash: ca6cde66b3784ceb5d0bc5921cee87aea74cb150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a><span data-ttu-id="4bdd0-104">Egy Azure Automation-runbook kezdődő, és olyan webhook</span><span class="sxs-lookup"><span data-stu-id="4bdd0-104">Starting an Azure Automation runbook with a webhook</span></span>
<span data-ttu-id="4bdd0-105">A *webhook* lehetővé teszi egy adott runbookhoz toostart az Azure Automationben egyetlen HTTP-kérelem keresztül.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-105">A *webhook* allows you toostart a particular runbook in Azure Automation through a single HTTP request.</span></span> <span data-ttu-id="4bdd0-106">Ez lehetővé teszi külső szolgáltatások, például a Visual Studio Team Services, GitHub, a Microsoft Operations Management Suite Naplóelemzési vagy egyéni alkalmazások toostart runbookokat hello Azure Automation API használatával teljes megoldások megvalósításának nélkül.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-106">This allows external services such as Visual Studio Team Services, GitHub, Microsoft Operations Management Suite Log Analytics, or custom applications toostart runbooks without implementing a full solution using hello Azure Automation API.</span></span>  
<span data-ttu-id="4bdd0-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span><span class="sxs-lookup"><span data-stu-id="4bdd0-107">![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)</span></span>

<span data-ttu-id="4bdd0-108">Összehasonlíthatja a runbook indítása webhookok tooother módszerek [runbook elindítása az Azure Automationben](automation-starting-a-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="4bdd0-108">You can compare webhooks tooother methods of starting a runbook in [Starting a runbook in Azure Automation](automation-starting-a-runbook.md)</span></span>

## <a name="details-of-a-webhook"></a><span data-ttu-id="4bdd0-109">A webhook részleteit</span><span class="sxs-lookup"><span data-stu-id="4bdd0-109">Details of a webhook</span></span>
<span data-ttu-id="4bdd0-110">hello következő táblázat ismerteti, hogy konfigurálnia kell egy webhook hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-110">hello following table describes hello properties that you must configure for a webhook.</span></span>

| <span data-ttu-id="4bdd0-111">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4bdd0-111">Property</span></span> | <span data-ttu-id="4bdd0-112">Leírás</span><span class="sxs-lookup"><span data-stu-id="4bdd0-112">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4bdd0-113">Név</span><span class="sxs-lookup"><span data-stu-id="4bdd0-113">Name</span></span> |<span data-ttu-id="4bdd0-114">Megadhat egy nevet, egy webhook mivel ez nincs felfedve toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-114">You can provide any name you want for a webhook since this is not exposed toohello client.</span></span>  <span data-ttu-id="4bdd0-115">Csak akkor tooidentify hello runbook az Azure Automationben szolgál.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-115">It is only used for you tooidentify hello runbook in Azure Automation.</span></span> <br>  <span data-ttu-id="4bdd0-116">Ajánlott eljárásként adjon hello webhook neve kapcsolódó toohello ügyfél, amely fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-116">As a best practice, you should give hello webhook a name related toohello client that will use it.</span></span> |
| <span data-ttu-id="4bdd0-117">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="4bdd0-117">URL</span></span> |<span data-ttu-id="4bdd0-118">hello hello webhook URL-címe, a kapcsolt toohello webhook hello egyedi címet, hogy egy ügyfél egy HTTP POST toostart hello runbook hívja.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-118">hello URL of hello webhook is hello unique address that a client calls with an HTTP POST toostart hello runbook linked toohello webhook.</span></span>  <span data-ttu-id="4bdd0-119">Hello webhook létrehozásakor automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-119">It is automatically generated when you create hello webhook.</span></span>  <span data-ttu-id="4bdd0-120">Egy egyéni URL-címe nem adható meg.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-120">You cannot specify a custom URL.</span></span> <br> <br>  <span data-ttu-id="4bdd0-121">hello URL-címe olyan biztonsági jogkivonatot, amely lehetővé teszi a hitelesítés nélküli további külső rendszer által meghívott hello runbook toobe tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-121">hello URL contains a security token that allows hello runbook toobe invoked by a third party system with no further authentication.</span></span> <span data-ttu-id="4bdd0-122">Ezért azt kell kezelni, például a jelszó.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-122">For this reason, it should be treated like a password.</span></span>  <span data-ttu-id="4bdd0-123">Biztonsági okokból is csak nézet hello hello Azure portálon, a hello idő hello webhook URL-címet jön létre.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-123">For security reasons, you can only view hello URL in hello Azure portal at hello time hello webhook is created.</span></span> <span data-ttu-id="4bdd0-124">Vegye figyelembe a jövőbeli használatra biztonságos helyen hello URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-124">You should note hello URL in a secure location for future use.</span></span> |
| <span data-ttu-id="4bdd0-125">Lejárat dátuma</span><span class="sxs-lookup"><span data-stu-id="4bdd0-125">Expiration date</span></span> |<span data-ttu-id="4bdd0-126">Például egy tanúsítványt egyes webhook van ekkor már nem használható lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-126">Like a certificate, each webhook has an expiration date at which time it can no longer be used.</span></span>  <span data-ttu-id="4bdd0-127">A lejárati dátum hello webhook létrehozása után módosítható.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-127">This expiration date can be modified after hello webhook is created.</span></span> |
| <span data-ttu-id="4bdd0-128">Engedélyezve</span><span class="sxs-lookup"><span data-stu-id="4bdd0-128">Enabled</span></span> |<span data-ttu-id="4bdd0-129">A webhook alapértelmezés szerint engedélyezve van, ha létrehozták.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-129">A webhook is enabled by default when it is created.</span></span>  <span data-ttu-id="4bdd0-130">Ha be állítani tooDisabled, akkor nem lesz képes toouse azt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-130">If you set it tooDisabled, then no client will be able toouse it.</span></span>  <span data-ttu-id="4bdd0-131">Beállíthatja a hello **engedélyezve** tulajdonság hello webhook, vagy bármikor egyszer létrehozásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-131">You can set hello **Enabled** property when you create hello webhook or anytime once it is created.</span></span> |

### <a name="parameters"></a><span data-ttu-id="4bdd0-132">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4bdd0-132">Parameters</span></span>
<span data-ttu-id="4bdd0-133">A webhook runbook paramétereket, illetve ha hello runbook indítja el, hogy a webhook értékeket határozhat meg.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-133">A webhook can define values for runbook parameters that are used when hello runbook is started by that webhook.</span></span> <span data-ttu-id="4bdd0-134">hello webhook értékeket hello runbook minden kötelező paraméterhez, és nem kötelező paraméter értékét is járhatott.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-134">hello webhook must include values for any mandatory parameters of hello runbook and may include values for optional parameters.</span></span> <span data-ttu-id="4bdd0-135">A paraméter beállított értéke tooa webhook hello webhoook létrehozása után is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-135">A parameter value configured tooa webhook can be modified even after creating hello webhoook.</span></span> <span data-ttu-id="4bdd0-136">Egyetlen runbook több kapcsolódó webhookok tooa egyes eltérő értékek használhatók.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-136">Multiple webhooks linked tooa single runbook can each use different parameter values.</span></span>

<span data-ttu-id="4bdd0-137">Amikor egy ügyfél elindítja a runbookot egy webhook, metódus nem bírálhatja felül a hello webhook definiált hello paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-137">When a client starts a runbook using a webhook, it cannot override hello parameter values defined in hello webhook.</span></span>  <span data-ttu-id="4bdd0-138">hello ügyfél tooreceive adatait, hello runbook nevű egyetlen paramétert fogadhat **$WebhookData** típusú [objektum] adatokat tartalmazó hello ügyfél hello POST kérelemben tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-138">tooreceive data from hello client, hello runbook can accept a single parameter called **$WebhookData** of type [object] that will contain data that hello client includes in hello POST request.</span></span>

![Webhookdata tulajdonságai](media/automation-webhooks/webhook-data-properties.png)

<span data-ttu-id="4bdd0-140">Hello **$WebhookData** objektum lesz hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="4bdd0-140">hello **$WebhookData** object will have hello following properties:</span></span>

| <span data-ttu-id="4bdd0-141">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4bdd0-141">Property</span></span> | <span data-ttu-id="4bdd0-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="4bdd0-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="4bdd0-143">WebhookName</span><span class="sxs-lookup"><span data-stu-id="4bdd0-143">WebhookName</span></span> |<span data-ttu-id="4bdd0-144">hello webhook hello neve.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-144">hello name of hello webhook.</span></span> |
| <span data-ttu-id="4bdd0-145">RequestHeader</span><span class="sxs-lookup"><span data-stu-id="4bdd0-145">RequestHeader</span></span> |<span data-ttu-id="4bdd0-146">A kivonattábla tartalmazó hello fejléceket a bejövő hello POST-kérelmet.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-146">Hash table containing hello headers of hello incoming POST request.</span></span> |
| <span data-ttu-id="4bdd0-147">requestBody</span><span class="sxs-lookup"><span data-stu-id="4bdd0-147">RequestBody</span></span> |<span data-ttu-id="4bdd0-148">bejövő hello POST-kérelmet hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-148">hello body of hello incoming POST request.</span></span>  <span data-ttu-id="4bdd0-149">Ez megőrzi a formázási karakterlánc, JSON, XML, például vagy űrlap kódolású adatot.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-149">This will retain any formatting such as string, JSON, XML, or form encoded data.</span></span> <span data-ttu-id="4bdd0-150">hello runbook hello adatok formátumú várt toowork úgy kell megírni.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-150">hello runbook must be written toowork with hello data format that is expected.</span></span> |

<span data-ttu-id="4bdd0-151">Nincs a hello webhook szükséges toosupport hello konfiguráció **$WebhookData** paramétert, és hello runbook nincs szükség tooaccept azt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-151">There is no configuration of hello webhook required toosupport hello **$WebhookData** parameter, and hello runbook is not required tooaccept it.</span></span>  <span data-ttu-id="4bdd0-152">Hello runbook hello paraméter nem határozza meg, ha minden adatát hello ügyfélről küldött hello kérelem figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-152">If hello runbook does not define hello parameter, then any details of hello request sent from hello client is ignored.</span></span>

<span data-ttu-id="4bdd0-153">Ha megad egy értéket a $WebhookData hello webhook létrehozásakor, ezt az értéket fogja bírálható felül indításakor hello webhook hello runbook hello adataival hello ügyfél POST-kérelmet, még akkor is, ha hello ügyfél nem vesz be semmilyen adatot hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-153">If you specify a value for $WebhookData when you create hello webhook, that value will be overriden when hello webhook starts hello runbook with hello data from hello client POST request, even if hello client does not include any data in hello request body.</span></span>  <span data-ttu-id="4bdd0-154">Ha elindít egy forgatókönyvet, amely eltérő a webhook módszerrel $WebhookData rendelkezik, a értéket adja meg, amely hello runbook felismer $Webhookdata.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-154">If you start a runbook that has $WebhookData using a method other than a webhook, you can provide a value for $Webhookdata that will be recognized by hello runbook.</span></span>  <span data-ttu-id="4bdd0-155">Ez az érték legyen hello objektum ugyanazon [tulajdonságok](#details-of-a-webhook) , így hello runbook megfelelően dolgozhat, mintha volt az olyan webhook által átadott tényleges WebhookData használata $Webhookdata.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-155">This value should be an object with hello same [properties](#details-of-a-webhook) as $Webhookdata so that hello runbook can properly work with it as if it was working with actual WebhookData passed by a webhook.</span></span>

<span data-ttu-id="4bdd0-156">Például ha meg és indítja el a runbook követően – hello Azure Portal hello WebhookData néhány példa a tesztelésre, mivel WebhookData objektum toopass, azt kell átadni JSON-ként hello felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-156">For example, if you are starting hello following runbook from hello Azure Portal and want toopass some sample WebhookData for testing, since WebhookData is an object, it should be passed as JSON in hello UI.</span></span>

![A felhasználói Felületről WebhookData paraméter](media/automation-webhooks/WebhookData-parameter-from-UI.png)

<span data-ttu-id="4bdd0-158">A fenti runbookot, ha a következő tulajdonságok hello WebhookData paraméter hello hello:</span><span class="sxs-lookup"><span data-stu-id="4bdd0-158">For hello above runbook, if you have hello following properties for hello WebhookData parameter:</span></span>

1. <span data-ttu-id="4bdd0-159">WebhookName: *MyWebhook*</span><span class="sxs-lookup"><span data-stu-id="4bdd0-159">WebhookName: *MyWebhook*</span></span>
2. <span data-ttu-id="4bdd0-160">RequestHeader: *= teszt felhasználó*</span><span class="sxs-lookup"><span data-stu-id="4bdd0-160">RequestHeader: *From=Test User*</span></span>
3. <span data-ttu-id="4bdd0-161">RequestBody: *["VM1", "Vm2 virtuális gépnek"]*</span><span class="sxs-lookup"><span data-stu-id="4bdd0-161">RequestBody: *[“VM1”, “VM2”]*</span></span>

<span data-ttu-id="4bdd0-162">Majd át hello JSON-érték a felhasználói felület hello hello WebhookData paraméter a következő lenne:</span><span class="sxs-lookup"><span data-stu-id="4bdd0-162">Then you would pass hello following JSON value in hello UI for hello WebhookData parameter:</span></span>  

* <span data-ttu-id="4bdd0-163">{"WebhookName": "MyWebhook", "RequestHeader": {"Feladó": "Felhasználói teszt"}, "RequestBody": "[\"VM1\",\"vm2 virtuális gépnek\"]"}</span><span class="sxs-lookup"><span data-stu-id="4bdd0-163">{"WebhookName":"MyWebhook", "RequestHeader":{"From":"Test User"}, "RequestBody":"[\"VM1\",\"VM2\"]"}</span></span>

![Kezdő WebhookData paraméter a felhasználói Felületről](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> <span data-ttu-id="4bdd0-165">az összes bemeneti paraméterek értékét hello hello runbook-feladatok naplózása.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-165">hello values of all input parameters are logged with hello runbook job.</span></span>  <span data-ttu-id="4bdd0-166">Ez azt jelenti, hogy bármely hello webhook kérelem hello ügyfél által megadott hozzáférési toohello automation-feladat a naplózás és a rendelkezésre álló tooanyone lesz.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-166">This means that any input provided by hello client in hello webhook request will be logged and available tooanyone with access toohello automation job.</span></span>  <span data-ttu-id="4bdd0-167">Emiatt a webhook hívások a bizalmas adatokat is beleértve elővigyázatosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-167">For this reason, you should be cautious about including sensitive information in webhook calls.</span></span>
>

## <a name="security"></a><span data-ttu-id="4bdd0-168">Biztonság</span><span class="sxs-lookup"><span data-stu-id="4bdd0-168">Security</span></span>
<span data-ttu-id="4bdd0-169">a webhook hello biztonságát hello adatvédelme érdekében az URL-CÍMÉT, amely egy biztonsági jogkivonatot tartalmaz, amely lehetővé teszi, hogy meghívott toobe támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-169">hello security of a webhook relies on hello privacy of its URL which contains a security token that allows it toobe invoked.</span></span> <span data-ttu-id="4bdd0-170">Azure Automation szolgáltatásbeli nem végzi el a hitelesítési hello kérelem mindaddig, amíg az azt alkotó toohello helyes URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-170">Azure Automation does not perform any authentication on hello request as long as it is made toohello correct URL.</span></span> <span data-ttu-id="4bdd0-171">Emiatt webhookok nem használható a runbookok, amelyek a szigorúan bizalmas valamilyen alternatív, hello kérelem ellenőrzése használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-171">For this reason, webhooks should not be used for runbooks that perform highly sensitive functions without using an alternate means of validating hello request.</span></span>

<span data-ttu-id="4bdd0-172">Megadhatja, hogy meghívták volna a webhook által hello ellenőrzésével hello runbook toodetermine belüli logika **WebhookName** hello $WebhookData paraméter tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-172">You can include logic within hello runbook toodetermine that it was called by a webhook by checking hello **WebhookName** property of hello $WebhookData parameter.</span></span> <span data-ttu-id="4bdd0-173">hello runbook volt az ellenőrzéshez további hello az adott információkat keres **RequestHeader** vagy **RequestBody** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-173">hello runbook could perform further validation by looking for particular information in hello **RequestHeader** or **RequestBody** properties.</span></span>

<span data-ttu-id="4bdd0-174">Egy másik olyan stratégia toohave hello runbook egy külső állapot néhány ellenőrzés végrehajtása, ha webhook kérelmet kapott.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-174">Another strategy is toohave hello runbook perform some validation of an external condition when it received a webhook request.</span></span>  <span data-ttu-id="4bdd0-175">Tegyük fel, ha egy új véglegesítési tooa GitHub-tárházban GitHub hívja runbook.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-175">For example, consider a runbook that is called by GitHub whenever there is a new commit tooa GitHub repository.</span></span>  <span data-ttu-id="4bdd0-176">hello runbook kapcsolódását tooGitHub toovalidate, amely egy új véglegesítési valójában történt volna a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-176">hello runbook might connect tooGitHub toovalidate that a new commit had actually just occurred before continuing.</span></span>

## <a name="creating-a-webhook"></a><span data-ttu-id="4bdd0-177">A webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="4bdd0-177">Creating a webhook</span></span>
<span data-ttu-id="4bdd0-178">A következő eljárás toocreate hello Azure portál egy új kapcsolódó webhook tooa runbook hello használata.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-178">Use hello following procedure toocreate a new webhook linked tooa runbook in hello Azure portal.</span></span>

1. <span data-ttu-id="4bdd0-179">A hello **Runbookok panel** hello Azure-portálon, kattintson hello webhook hello runbook elindul tooview a részletek panelen.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-179">From hello **Runbooks blade** in hello Azure portal, click hello runbook that hello webhook will start tooview its detail blade.</span></span>
2. <span data-ttu-id="4bdd0-180">Kattintson a **Webhook** hello panel tooopen hello hello tetején **hozzáadása Webhook** panelen.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-180">Click **Webhook** at hello top of hello blade tooopen hello **Add Webhook** blade.</span></span> <br><span data-ttu-id="4bdd0-181">
   ![Webhook gomb](media/automation-webhooks/webhooks-button.png)</span><span class="sxs-lookup"><span data-stu-id="4bdd0-181">
![Webhooks button](media/automation-webhooks/webhooks-button.png)</span></span>
3. <span data-ttu-id="4bdd0-182">Kattintson a **hozzon létre új webhook** tooopen hello **létrehozás webhook panel**.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-182">Click **Create new webhook** tooopen hello **Create webhook blade**.</span></span>
4. <span data-ttu-id="4bdd0-183">Adjon meg egy **neve**, **lejárati dátum** hello webhook, és hogy azt engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-183">Specify a **Name**, **Expiration Date** for hello webhook and whether it should be enabled.</span></span> <span data-ttu-id="4bdd0-184">Lásd: [olyan webhook részleteit](#details-of-a-webhook) további információt ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-184">See [Details of a webhook](#details-of-a-webhook) for more information these properties.</span></span>
5. <span data-ttu-id="4bdd0-185">Hello másolás ikonra, majd nyomja meg a Ctrl + C toocopy hello URL-címe hello webhook.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-185">Click hello copy icon and press Ctrl+C toocopy hello URL of hello webhook.</span></span>  <span data-ttu-id="4bdd0-186">Biztonságos helyen, majd rögzítse azt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-186">Then record it in a safe place.</span></span>  <span data-ttu-id="4bdd0-187">**Hello webhook létrehozása után nem lehet lekérdezni a hello URL-cím újra.**</span><span class="sxs-lookup"><span data-stu-id="4bdd0-187">**Once you create hello webhook, you cannot retrieve hello URL again.**</span></span> <br><span data-ttu-id="4bdd0-188">
   ![Webhook URL-CÍMÉT](media/automation-webhooks/copy-webhook-url.png)</span><span class="sxs-lookup"><span data-stu-id="4bdd0-188">
![Webhook URL](media/automation-webhooks/copy-webhook-url.png)</span></span>
6. <span data-ttu-id="4bdd0-189">Kattintson a **paraméterek** tooprovide hello gyermekrunbook paramétereinek értékeit.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-189">Click **Parameters** tooprovide values for hello runbook parameters.</span></span>  <span data-ttu-id="4bdd0-190">Ha hello runbook kötelező paraméterekkel rendelkezik, majd nem fogja tudni toocreate hello webhook kivéve, ha az értékek.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-190">If hello runbook has mandatory parameters, then you will not be able toocreate hello webhook unless values are provided.</span></span>
7. <span data-ttu-id="4bdd0-191">Kattintson a **létrehozása** toocreate hello webhook.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-191">Click **Create** toocreate hello webhook.</span></span>

## <a name="using-a-webhook"></a><span data-ttu-id="4bdd0-192">A webhook használatával</span><span class="sxs-lookup"><span data-stu-id="4bdd0-192">Using a webhook</span></span>
<span data-ttu-id="4bdd0-193">toouse a webhook létrehozása után, az ügyfélalkalmazás hello URL-címmel hello webhook HTTP POST kell kiadnia.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-193">toouse a webhook after it has been created, your client application must issue an HTTP POST with hello URL for hello webhook.</span></span>  <span data-ttu-id="4bdd0-194">hello webhook hello szintaxisa a következő formátumban hello lesz.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-194">hello syntax of hello webhook will be in hello following format.</span></span>

    http://<Webhook Server>/token?=<Token Value>

<span data-ttu-id="4bdd0-195">hello ügyfél hello POST-kérelmet a visszatérési kódok következő hello egyikét fogja megkapni.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-195">hello client will receive one of hello following return codes from hello POST request.</span></span>  

| <span data-ttu-id="4bdd0-196">Kód</span><span class="sxs-lookup"><span data-stu-id="4bdd0-196">Code</span></span> | <span data-ttu-id="4bdd0-197">Szöveg</span><span class="sxs-lookup"><span data-stu-id="4bdd0-197">Text</span></span> | <span data-ttu-id="4bdd0-198">Leírás</span><span class="sxs-lookup"><span data-stu-id="4bdd0-198">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="4bdd0-199">202</span><span class="sxs-lookup"><span data-stu-id="4bdd0-199">202</span></span> |<span data-ttu-id="4bdd0-200">Elfogadva</span><span class="sxs-lookup"><span data-stu-id="4bdd0-200">Accepted</span></span> |<span data-ttu-id="4bdd0-201">hello kérést elfogadták, és hello runbook sikeresen várólistára került.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-201">hello request was accepted, and hello runbook was successfully queued.</span></span> |
| <span data-ttu-id="4bdd0-202">400</span><span class="sxs-lookup"><span data-stu-id="4bdd0-202">400</span></span> |<span data-ttu-id="4bdd0-203">Helytelen kérelem</span><span class="sxs-lookup"><span data-stu-id="4bdd0-203">Bad Request</span></span> |<span data-ttu-id="4bdd0-204">hello kérelem hello a következő okok valamelyike miatt elutasítva.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-204">hello request was not accepted for one of hello following reasons.</span></span> <ul> <li><span data-ttu-id="4bdd0-205">hello webhook érvényessége lejárt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-205">hello webhook has expired.</span></span></li> <li><span data-ttu-id="4bdd0-206">hello webhook le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-206">hello webhook is disabled.</span></span></li> <li><span data-ttu-id="4bdd0-207">hello token hello URL-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-207">hello token in hello URL is invalid.</span></span></li>  </ul> |
| <span data-ttu-id="4bdd0-208">404</span><span class="sxs-lookup"><span data-stu-id="4bdd0-208">404</span></span> |<span data-ttu-id="4bdd0-209">Nem található</span><span class="sxs-lookup"><span data-stu-id="4bdd0-209">Not Found</span></span> |<span data-ttu-id="4bdd0-210">hello kérelem hello a következő okok valamelyike miatt elutasítva.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-210">hello request was not accepted for one of hello following reasons.</span></span> <ul> <li><span data-ttu-id="4bdd0-211">hello webhook nem található.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-211">hello webhook was not found.</span></span></li> <li><span data-ttu-id="4bdd0-212">hello runbook nem található.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-212">hello runbook was not found.</span></span></li> <li><span data-ttu-id="4bdd0-213">hello fiók nem található.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-213">hello account was not found.</span></span></li>  </ul> |
| <span data-ttu-id="4bdd0-214">500</span><span class="sxs-lookup"><span data-stu-id="4bdd0-214">500</span></span> |<span data-ttu-id="4bdd0-215">Belső kiszolgálóhiba.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-215">Internal Server Error</span></span> |<span data-ttu-id="4bdd0-216">hello URL-cím érvénytelen volt, de hiba történt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-216">hello URL was valid, but an error occurred.</span></span>  <span data-ttu-id="4bdd0-217">Küldje el hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-217">Please resubmit hello request.</span></span> |

<span data-ttu-id="4bdd0-218">Feltéve, hogy hello kérés sikeres-e, hello webhook válaszban hello feladatazonosító JSON formátumban az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-218">Assuming hello request is successful, hello webhook response contains hello job id in JSON format as follows.</span></span> <span data-ttu-id="4bdd0-219">Egyetlen feladatazonosító fogja tartalmazni, de lehetséges jövőbeli fejlesztések hello JSON formátumban teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-219">It will contain a single job id, but hello JSON format allows for potential future enhancements.</span></span>

    {"JobIds":["<JobId>"]}  

<span data-ttu-id="4bdd0-220">hello ügyfél nem tudja megállapítani, hello runbook-feladat befejezését vagy hello webhook a befejezési állapotát.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-220">hello client cannot determine when hello runbook job completes or its completion status from hello webhook.</span></span>  <span data-ttu-id="4bdd0-221">Ezt az információt hello feladatazonosító egy másik módszerrel például meg tudja határozni [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) vagy hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-221">It can determine this information using hello job id with another method such as [Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) or hello [Azure Automation API](https://msdn.microsoft.com/library/azure/mt163826.aspx).</span></span>

### <a name="example"></a><span data-ttu-id="4bdd0-222">Példa</span><span class="sxs-lookup"><span data-stu-id="4bdd0-222">Example</span></span>
<span data-ttu-id="4bdd0-223">a következő példa hello Windows PowerShell toostart egy runbook egy webhook használ.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-223">hello following example uses Windows PowerShell toostart a runbook with a webhook.</span></span>  <span data-ttu-id="4bdd0-224">Vegye figyelembe, hogy bármilyen nyelvet használhat, amelyek HTTP-kérelem használhatja a webhook; Windows PowerShell csak használt példa.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-224">Note that any language that can make an HTTP request can use a webhook; Windows PowerShell is just used here as an example.</span></span>

<span data-ttu-id="4bdd0-225">hello runbook által várt paraméterekkel hello törzsében hello kérelem JSON-formázott virtuális gépek listáját.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-225">hello runbook is expecting a list of virtual machines formatted in JSON in hello body of hello request.</span></span> <span data-ttu-id="4bdd0-226">Azt is vannak adatai, például ki indul hello runbook és hello dátum és idő hello kérelem hello fejlécében van indítását.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-226">We also are including information about who is starting hello runbook and hello date and time it is being started in hello header of hello request.</span></span>      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


<span data-ttu-id="4bdd0-227">hello következő kép bemutatja hello fejléc-információ (használatával egy [Fiddler](http://www.telerik.com/fiddler) nyomkövetési) a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-227">hello following image shows hello header information (using a [Fiddler](http://www.telerik.com/fiddler) trace) from this request.</span></span> <span data-ttu-id="4bdd0-228">Ez tartalmaz szabványos HTTP-kérelem-fejlécekkel hozzáadása toohello egyéni dátum és a hozzáadott-fejlécek.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-228">This includes standard headers of an HTTP request in addition toohello custom Date and From headers that we added.</span></span>  <span data-ttu-id="4bdd0-229">Ezek az értékek egy elérhető toohello runbookot a hello **RequestHeaders** tulajdonsága **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-229">Each of these values is available toohello runbook in hello **RequestHeaders** property of **WebhookData**.</span></span>

![Webhook gomb](media/automation-webhooks/webhook-request-headers.png)

<span data-ttu-id="4bdd0-231">hello következő kép bemutatja hello kérelem törzse hello (használatával egy [Fiddler](http://www.telerik.com/fiddler) nyomkövetési), amely elérhető toohello runbook hello **RequestBody** tulajdonsága **WebhookData**.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-231">hello following image shows hello body of hello request (using a [Fiddler](http://www.telerik.com/fiddler) trace)  that is available toohello runbook in hello **RequestBody** property of **WebhookData**.</span></span> <span data-ttu-id="4bdd0-232">Ez formázott JSON-ként mert, amely hello hello kérelem törzsében szereplő hello formátumú volt.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-232">This is formatted as JSON because that was hello format that was included in hello body of hello request.</span></span>     

![Webhook gomb](media/automation-webhooks/webhook-request-body.png)

<span data-ttu-id="4bdd0-234">hello következő kép bemutatja a Windows PowerShell és az eredményül kapott válasz hello küldi hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-234">hello following image shows hello request being sent from Windows PowerShell and hello resulting response.</span></span>  <span data-ttu-id="4bdd0-235">hello feladatazonosító kinyerésének hello válasz és konvertált tooa karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-235">hello job id is extracted from hello response and converted tooa string.</span></span>

![Webhook gomb](media/automation-webhooks/webhook-request-response.png)

<span data-ttu-id="4bdd0-237">hello következő minta runbook hello előző példa kérést fogad és hello virtuális gépek hello kérés törzsében megadott kezdődik.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-237">hello following sample runbook accepts hello previous example request and starts hello virtual machines specified in hello request body.</span></span>

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

            # Authenticate tooAzure resources
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
            Write-Error "Runbook mean toobe started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-tooazure-alerts"></a><span data-ttu-id="4bdd0-238">Runbookok elindítása a válasz tooAzure riasztások</span><span class="sxs-lookup"><span data-stu-id="4bdd0-238">Starting runbooks in response tooAzure alerts</span></span>
<span data-ttu-id="4bdd0-239">Runbookok Webhook-kompatibilis túl lehet használt tooreact[Azure riasztások](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-239">Webhook-enabled runbooks can be used tooreact too[Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="4bdd0-240">Az Azure-erőforrások hello statisztika, például a teljesítmény, rendelkezésre állását és használati Azure riasztások hello segítségével-készítés révén figyelhető.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-240">Resources in Azure can be monitored by collecting hello statistics like performance, availability and usage with hello help of Azure alerts.</span></span> <span data-ttu-id="4bdd0-241">Figyelési metrikákat alapuló riasztást kaphat, vagy eseményeket az Azure-erőforrások, jelenleg az Automation-fiókok csak metrikák támogatja.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-241">You can receive an alert based on monitoring metrics or events for your Azure resources, currently Automation Accounts support only metrics.</span></span> <span data-ttu-id="4bdd0-242">Ha adott mérőszám hello értéke meghaladja-e a hozzárendelt hello küszöbértéket vagy hello konfigurált az esemény akkor váltódik ki, majd egy értesítést küld toohello rendszergazda vagy a társadminisztrátoroknak tooresolve hello szolgáltatásriasztás, további információt a metrikákat, és események tekintse meg a túl[ Az Azure riasztások](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-242">When hello value of a specified metric exceeds hello threshold assigned or if hello configured event is triggered then a notification is sent toohello service admin or co-admins tooresolve hello alert, for more information on metrics and events please refer too[Azure alerts](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="4bdd0-243">Használata Azure riasztások értesítési rendszert, mellett is is indítsa el a válasz tooalerts runbookok.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-243">Besides using Azure alerts as a notification system, you can also kick off runbooks in response tooalerts.</span></span> <span data-ttu-id="4bdd0-244">Azure Automation szolgáltatásbeli hello funkció toorun webhook engedélyezve van a runbookok a riasztások az Azure biztosít.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-244">Azure Automation provides hello capability toorun webhook-enabled runbooks with Azure alerts.</span></span> <span data-ttu-id="4bdd0-245">Metrika meghaladja hello konfigurálva küszöbérték akkor hello riasztási szabály válik aktívvá, és eseményindítók hello pedig végrehajtja a hello runbook automation-webhook.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-245">When a metric exceeds hello configured threshold value then hello alert rule becomes active and triggers hello automation webhook which in turn executes hello runbook.</span></span>

![webhook](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a><span data-ttu-id="4bdd0-247">Riasztás környezete</span><span class="sxs-lookup"><span data-stu-id="4bdd0-247">Alert context</span></span>
<span data-ttu-id="4bdd0-248">Érdemes lehet például egy virtuális gépet egy Azure-erőforrás, a CPU-felhasználás gép hello legfontosabb teljesítményi metrika egyikét.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-248">Consider an Azure resource such as a virtual machine, CPU utilization of this machine is one of hello key performance metric.</span></span> <span data-ttu-id="4bdd0-249">Ha hello CPU-felhasználás 100 %-os vagy több mint egy hosszú időn keresztül, érdemes lehet toorestart hello virtuális gép toofix hello probléma.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-249">If hello CPU utilization is 100% or more than a certain amount for long period of time, you might want toorestart hello virtual machine toofix hello problem.</span></span> <span data-ttu-id="4bdd0-250">Ez megoldható egy riasztási szabály toohello virtuális gép konfigurálása, és ez a szabály tart, mint a metrika processzorszázaléka.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-250">This can be solved by configuring an alert rule toohello virtual machine and this rule takes CPU percentage as its metric.</span></span> <span data-ttu-id="4bdd0-251">Processzorszázaléka Itt csupán példaként lesz végrehajtva, de nincsenek konfigurálhat a tooyour Azure számos más metrikákkal erőforrások és hello virtuális gép újraindítása egy művelet végrehajtott toofix probléma konfigurálhatja hello runbook tootake más műveletek.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-251">CPU percentage here is just taken as an example but there are many other metrics that you can configure tooyour Azure resources and restarting hello virtual machine is an action that is taken toofix this issue, you can configure hello runbook tootake other actions.</span></span>

<span data-ttu-id="4bdd0-252">Ha a hello riasztási szabály válik aktívvá, és eseményindítók hello webhook engedélyezve van a runbook, hello riasztás környezete toohello runbook küld.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-252">When this hello alert rule becomes active and triggers hello webhook-enabled runbook, it sends hello alert context toohello runbook.</span></span> <span data-ttu-id="4bdd0-253">[Riasztás környezete](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) tartalmaz információt talál többek között **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** és **időbélyeg** hello runbook tooidentify hello erőforrás amely művelet azt véve szükséges.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-253">[Alert context](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) contains details including **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** and **Timestamp** which are required for hello runbook tooidentify hello resource on which it will be taking action.</span></span> <span data-ttu-id="4bdd0-254">Riasztási környezetben beágyazott hello törzs részét hello **WebhookData** elküldött toohello runbook objektumra, és elérhető legyen a **Webhook.RequestBody** tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4bdd0-254">Alert context is embedded in hello body part of hello **WebhookData** object sent toohello runbook and it can be accessed with **Webhook.RequestBody** property</span></span>

### <a name="example"></a><span data-ttu-id="4bdd0-255">Példa</span><span class="sxs-lookup"><span data-stu-id="4bdd0-255">Example</span></span>
<span data-ttu-id="4bdd0-256">Hozzon létre egy Azure virtuális gép előfizetését és társítható egy [toomonitor CPU százalékos metrika riasztási](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-256">Create an Azure virtual machine in your subscription and associate an [alert toomonitor CPU percentage metric](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span> <span data-ttu-id="4bdd0-257">Hello riasztás létrehozása során mindenképpen feltölti hello webhook mezővel hello hello webhook létrehozása során létrehozó hello webhook URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-257">While creating hello alert make sure you populate hello webhook field with hello URL of hello webhook which was generated while creating hello webhook.</span></span>

<span data-ttu-id="4bdd0-258">hello következő minta runbook lesz kiváltva, ha hello riasztási szabály válik aktívvá, és összegyűjti az hello riasztás környezete paraméterek, amelyek szükségesek a hello runbook tooidentify hello erőforrás amely művelet azt véve.</span><span class="sxs-lookup"><span data-stu-id="4bdd0-258">hello following sample runbook is triggered when hello alert rule becomes active and it collects hello Alert context parameters which are required for hello runbook tooidentify hello resource on which it will be taking action.</span></span>

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

            # Outputs information on hello webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain hello WebhookBody containing hello AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain hello AlertContext     
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

            # Act on hello AlertContext data, in our case restarting hello VM.
            # Authenticate tooyour Azure subscription using Organization ID toobe able toorestart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check hello status property of hello VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart hello VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant tooonly be started from a webhook."  
        }  
    }



## <a name="next-steps"></a><span data-ttu-id="4bdd0-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4bdd0-259">Next steps</span></span>
* <span data-ttu-id="4bdd0-260">A különböző módokon toostart egy runbookot, lásd: [Runbook indítása](automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-260">For details on different ways toostart a runbook, see [Starting a Runbook](automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="4bdd0-261">A Runbook-feladatok állapotának megtekintése hello információkért tekintse meg túl[a Runbook végrehajtása az Azure Automationben](automation-runbook-execution.md).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-261">For information on viewing hello Status of a Runbook Job, refer too[Runbook execution in Azure Automation](automation-runbook-execution.md).</span></span>
* <span data-ttu-id="4bdd0-262">Hogyan toouse Azure Automation tootake művelet Azure riasztások: toolearn [javítása Azure virtuális gép riasztások Automation-Runbook](automation-azure-vm-alert-integration.md).</span><span class="sxs-lookup"><span data-stu-id="4bdd0-262">toolearn how toouse Azure Automation tootake action on Azure Alerts, see [Remediate Azure VM Alerts with Automation Runbooks](automation-azure-vm-alert-integration.md).</span></span>
