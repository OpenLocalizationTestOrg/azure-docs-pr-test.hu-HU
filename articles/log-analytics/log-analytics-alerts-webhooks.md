---
title: "aaaWebhook riasztási műveleti minta az OMS szolgáltatáshoz |} Microsoft Docs"
description: "Egy hello művelet Naplóelemzési riasztás válasz tooa futtathat egy * webhook *, amely lehetővé teszi egyetlen HTTP-kérelem keresztül külső folyamat tooinvoke. Ez a cikk végigvezeti a Slackhez használatával Naplóelemzési figyelmeztető egy webhook művelet létrehozására láthat példát."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 13c39f0f-fd3c-472d-8324-ddf7538be45e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: bwren
ms.openlocfilehash: e60bdc4922347073d572c2e4719461b13e8e7d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-toosend-message-tooslack"></a><span data-ttu-id="c01a8-104">Az OMS szolgáltatáshoz toosend üzenet tooSlack riasztási webhook művelet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c01a8-104">Create an alert webhook action in OMS Log Analytics toosend message tooSlack</span></span>
<span data-ttu-id="c01a8-105">Egy válasz tooa futtathat hello művelet [Naplóelemzési riasztás](log-analytics-alerts.md) van egy *webhook*, így tooinvoke keresztül egyetlen HTTP-kérelem külső folyamat.</span><span class="sxs-lookup"><span data-stu-id="c01a8-105">One of hello actions you can run in response tooa [Log Analytics alert](log-analytics-alerts.md) is a *webhook*, which allows you tooinvoke an external process through a single HTTP request.</span></span>  <span data-ttu-id="c01a8-106">Riasztások és webhookokkal, a részleteket olvashat [Naplóelemzési riasztások](log-analytics-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="c01a8-106">You can read about details of alerts and webhooks in [Alerts in Log Analytics](log-analytics-alerts.md)</span></span>

<span data-ttu-id="c01a8-107">Ebben a cikkben végigvezetjük a Slackhez, amely egy üzenetkezelési szolgáltatás használatával Naplóelemzési figyelmeztető egy webhook művelet létrehozására láthat példát.</span><span class="sxs-lookup"><span data-stu-id="c01a8-107">In this article, we’ll walk through an example of creating a webhook action in a Log Analytics alert using Slack which is a messaging service.</span></span>

> [!NOTE]
> <span data-ttu-id="c01a8-108">Ez a minta egy Slack fiók toocomplete kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="c01a8-108">You must have a Slack account toocomplete this sample.</span></span>  <span data-ttu-id="c01a8-109">Regisztrálhat egy ingyenes fiókot, [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="c01a8-109">You can sign up for a free account at [slack.com](http://slack.com).</span></span>
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a><span data-ttu-id="c01a8-110">1. lépés – a Slackhez engedélyezése webhookok</span><span class="sxs-lookup"><span data-stu-id="c01a8-110">Step 1 - Enable webhooks in Slack</span></span>
1. <span data-ttu-id="c01a8-111">Jelentkezzen be a következő tooSlack [slack.com](http://slack.com).</span><span class="sxs-lookup"><span data-stu-id="c01a8-111">Sign in tooSlack at [slack.com](http://slack.com).</span></span>
2. <span data-ttu-id="c01a8-112">Jelölje ki a csatornát a hello **csatornák** szakasz hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="c01a8-112">Select a channel in hello **Channels** section in hello left pane.</span></span>  <span data-ttu-id="c01a8-113">Ez a csatorna hello hello üzenetet kapnak.</span><span class="sxs-lookup"><span data-stu-id="c01a8-113">This is hello channel that hello message will be sent to.</span></span>  <span data-ttu-id="c01a8-114">Például válasszon hello alapértelmezett csatornák közül **általános** vagy **véletlenszerű**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-114">You can select one of hello default channels such as **general** or **random**.</span></span>  <span data-ttu-id="c01a8-115">Éles forgatókönyv esetében, akkor nagy valószínűséggel kell létrehoznia egy különös csatorna például **criticalservicealerts**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-115">In a production scenario, you would most likely create a special channel such as **criticalservicealerts**.</span></span> <br>
   
   ![Slack-csatornákon](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. <span data-ttu-id="c01a8-117">Kattintson a **hozzáadása egy alkalmazáshoz vagy egyéni integrációs** tooopen hello alkalmazás könyvtárát.</span><span class="sxs-lookup"><span data-stu-id="c01a8-117">Click **Add an app or custom integration** tooopen hello App Directory.</span></span>
4. <span data-ttu-id="c01a8-118">Típus *webhookok* hello keresési mezőbe, majd válassza ki a **bejövő Webhookok**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-118">Type *webhooks* into hello search box and then select **Incoming WebHooks**.</span></span> <br>
   
   ![Slack-csatornákon](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. <span data-ttu-id="c01a8-120">Kattintson a **telepítése** következő tooyour csoport neve.</span><span class="sxs-lookup"><span data-stu-id="c01a8-120">Click **Install** next tooyour team name.</span></span>
6. <span data-ttu-id="c01a8-121">Kattintson a **konfiguráció hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-121">Click **Add Configuration**.</span></span>
7. <span data-ttu-id="c01a8-122">Jelölje be hello hello csatorna ehhez a példához toouse fog, és kattintson **bejövő Webhookok hozzáadása integrációs**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-122">Select hello hello channel that you're going toouse for this example, and then click **Add Incoming WebHooks integration**.</span></span>  
8. <span data-ttu-id="c01a8-123">Másolás hello **Webhook URL-CÍMÉT**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-123">Copy hello **Webhook URL**.</span></span>  <span data-ttu-id="c01a8-124">Akkor lesz kell beillesztése ez hello riasztás konfigurálásában.</span><span class="sxs-lookup"><span data-stu-id="c01a8-124">You'll be pasting this into hello Alert configuration.</span></span> <br>
   
    ![Slack-csatornákon](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a><span data-ttu-id="c01a8-126">2. lépés - a Naplóelemzési riasztási szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="c01a8-126">Step 2 - Create alert rule in Log Analytics</span></span>
1. <span data-ttu-id="c01a8-127">[Riasztási szabályt létrehozni](log-analytics-alerts.md) a beállítások a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c01a8-127">[Create an alert rule](log-analytics-alerts.md) with hello following settings.</span></span>
   * <span data-ttu-id="c01a8-128">Lekérdezés:```    Type=Event EventLevelName=error ```</span><span class="sxs-lookup"><span data-stu-id="c01a8-128">Query: ```    Type=Event EventLevelName=error ```</span></span>
   * <span data-ttu-id="c01a8-129">Riasztás keresésének minden: 5 perc</span><span class="sxs-lookup"><span data-stu-id="c01a8-129">Check for this alert every: 5 minutes</span></span>
   * <span data-ttu-id="c01a8-130">találatok száma hello: nagyobb, mint 10</span><span class="sxs-lookup"><span data-stu-id="c01a8-130">hello number of results is: greater than 10</span></span>
   * <span data-ttu-id="c01a8-131">Ezen időszak alatt: 60 perc</span><span class="sxs-lookup"><span data-stu-id="c01a8-131">Over this time window: 60 minutes</span></span>
   * <span data-ttu-id="c01a8-132">Válassza ki **Igen** a **Webhook** és **nem** a hello egyéb műveleteket.</span><span class="sxs-lookup"><span data-stu-id="c01a8-132">Select **Yes** for **Webhook** and **No** for hello other actions.</span></span>
2. <span data-ttu-id="c01a8-133">Beillesztés hello Slackhez URL-címet a hello **Webhook URL-CÍMÉT** mező.</span><span class="sxs-lookup"><span data-stu-id="c01a8-133">Paste hello Slack URL into hello **Webhook URL** field.</span></span>
3. <span data-ttu-id="c01a8-134">A beállításnak a hello túl**közé tartoznak az egyéni JSON-adattartalmat**.</span><span class="sxs-lookup"><span data-stu-id="c01a8-134">Select hello option too**include a custom JSON payload**.</span></span>
4. <span data-ttu-id="c01a8-135">A tartalékidő vár a hasznos adatok között JSON formátumú, nevű paraméterrel *szöveg*.</span><span class="sxs-lookup"><span data-stu-id="c01a8-135">Slack expects a payload formatted in JSON with a parameter named *text*.</span></span>  <span data-ttu-id="c01a8-136">Ez a hello szöveg, amely létrehozza üdvözlőüzenetére jelennek majd.</span><span class="sxs-lookup"><span data-stu-id="c01a8-136">This is hello text that it will display in hello message it creates.</span></span>  <span data-ttu-id="c01a8-137">Használhat egy vagy több riasztás paramétereknek hello hello  *#*  szimbólumának, mint például a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c01a8-137">You can use one or more of hello alert parameters using hello *#* symbol such as in hello following example.</span></span>
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds hello over threshold of #thresholdvalue ."
    }
    ```
   
    ![Példa JSON-adattartalmat](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. <span data-ttu-id="c01a8-139">Kattintson a **mentése** toosave hello riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="c01a8-139">Click **Save** toosave hello alert rule.</span></span>
6. <span data-ttu-id="c01a8-140">Várjon, amíg egy riasztási toobe létrehozott elegendő időt, és ellenőrizze a Slackhez egy üzenet, amely hasonló toohello következő lesz.</span><span class="sxs-lookup"><span data-stu-id="c01a8-140">Wait sufficient time for an alert toobe created and then check Slack for a message which will be similar toohello following.</span></span>
   
   ![Példa webhook a Slackhez](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a><span data-ttu-id="c01a8-142">Speciális webhook hasznos a Slackhez</span><span class="sxs-lookup"><span data-stu-id="c01a8-142">Advanced webhook payload for Slack</span></span>
<span data-ttu-id="c01a8-143">A Slackhez bejövő üzenetek nagymértékben testre szabható.</span><span class="sxs-lookup"><span data-stu-id="c01a8-143">You can extensively customize inbound messages with Slack.</span></span> <span data-ttu-id="c01a8-144">További információkért lásd: [bejövő Webhookok](https://api.slack.com/incoming-webhooks) hello közzététele a Slack-webhelyen.</span><span class="sxs-lookup"><span data-stu-id="c01a8-144">For more information, see [Incoming Webhooks](https://api.slack.com/incoming-webhooks) on hello Slack website.</span></span> <span data-ttu-id="c01a8-145">Az alábbiakban olvashat egy összetettebb hasznos toocreate formázás részletes üzenet:</span><span class="sxs-lookup"><span data-stu-id="c01a8-145">Following is a more complex payload toocreate a rich message with formatting:</span></span>

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link tooSearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


<span data-ttu-id="c01a8-146">A Slack hasonló toohello következő ez hoz létre egy üzenetet.</span><span class="sxs-lookup"><span data-stu-id="c01a8-146">This would generate a message in Slack similar toohello following.</span></span>

![a Slackhez példa üzenet](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a><span data-ttu-id="c01a8-148">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c01a8-148">Summary</span></span>
<span data-ttu-id="c01a8-149">A riasztási szabálynak helyen egy üzenet tooSlack kellene lennie, minden alkalommal, amikor hello feltétel teljesülése esetén.</span><span class="sxs-lookup"><span data-stu-id="c01a8-149">With this alert rule in place, you would have a message sent tooSlack every time hello criteria is met.</span></span>  

<span data-ttu-id="c01a8-150">Ez az egy műveletet, amely a válasz tooan riasztást hozhat létre csak egy példája.</span><span class="sxs-lookup"><span data-stu-id="c01a8-150">This is only one example of an action that you can create in response tooan alert.</span></span>  <span data-ttu-id="c01a8-151">A webhook művelet egy másik külső szolgáltatás, a runbook művelet toostart egy Azure Automation, vagy egy e-mailek művelet toosend runbookot meghívó létrehozhat egy levelezési tooyourself vagy egyéb.</span><span class="sxs-lookup"><span data-stu-id="c01a8-151">You could create a webhook action that calls another external service, a runbook action toostart a runbook in Azure Automation, or an email action toosend a mail tooyourself or other recipients.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="c01a8-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c01a8-152">Next Steps</span></span>
* <span data-ttu-id="c01a8-153">Tudnivalók más [Naplóelemzési műveletek riasztási](log-analytics-alerts-actions.md) beleértve más műveletek.</span><span class="sxs-lookup"><span data-stu-id="c01a8-153">Learn about other [alert actions in Log Analytics](log-analytics-alerts-actions.md) including other actions.</span></span>


