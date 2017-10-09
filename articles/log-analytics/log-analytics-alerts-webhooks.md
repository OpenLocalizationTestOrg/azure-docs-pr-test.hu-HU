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
# <a name="create-an-alert-webhook-action-in-oms-log-analytics-toosend-message-tooslack"></a>Az OMS szolgáltatáshoz toosend üzenet tooSlack riasztási webhook művelet létrehozása
Egy válasz tooa futtathat hello művelet [Naplóelemzési riasztás](log-analytics-alerts.md) van egy *webhook*, így tooinvoke keresztül egyetlen HTTP-kérelem külső folyamat.  Riasztások és webhookokkal, a részleteket olvashat [Naplóelemzési riasztások](log-analytics-alerts.md)

Ebben a cikkben végigvezetjük a Slackhez, amely egy üzenetkezelési szolgáltatás használatával Naplóelemzési figyelmeztető egy webhook művelet létrehozására láthat példát.

> [!NOTE]
> Ez a minta egy Slack fiók toocomplete kell rendelkeznie.  Regisztrálhat egy ingyenes fiókot, [slack.com](http://slack.com).
> 
> 

## <a name="step-1---enable-webhooks-in-slack"></a>1. lépés – a Slackhez engedélyezése webhookok
1. Jelentkezzen be a következő tooSlack [slack.com](http://slack.com).
2. Jelölje ki a csatornát a hello **csatornák** szakasz hello bal oldali ablaktáblán.  Ez a csatorna hello hello üzenetet kapnak.  Például válasszon hello alapértelmezett csatornák közül **általános** vagy **véletlenszerű**.  Éles forgatókönyv esetében, akkor nagy valószínűséggel kell létrehoznia egy különös csatorna például **criticalservicealerts**. <br>
   
   ![Slack-csatornákon](media/log-analytics-alerts-webhooks/oms-webhooks01.png)
3. Kattintson a **hozzáadása egy alkalmazáshoz vagy egyéni integrációs** tooopen hello alkalmazás könyvtárát.
4. Típus *webhookok* hello keresési mezőbe, majd válassza ki a **bejövő Webhookok**. <br>
   
   ![Slack-csatornákon](media/log-analytics-alerts-webhooks/oms-webhooks02.png)
5. Kattintson a **telepítése** következő tooyour csoport neve.
6. Kattintson a **konfiguráció hozzáadása**.
7. Jelölje be hello hello csatorna ehhez a példához toouse fog, és kattintson **bejövő Webhookok hozzáadása integrációs**.  
8. Másolás hello **Webhook URL-CÍMÉT**.  Akkor lesz kell beillesztése ez hello riasztás konfigurálásában. <br>
   
    ![Slack-csatornákon](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>2. lépés - a Naplóelemzési riasztási szabály létrehozása
1. [Riasztási szabályt létrehozni](log-analytics-alerts.md) a beállítások a következő hello.
   * Lekérdezés:```    Type=Event EventLevelName=error ```
   * Riasztás keresésének minden: 5 perc
   * találatok száma hello: nagyobb, mint 10
   * Ezen időszak alatt: 60 perc
   * Válassza ki **Igen** a **Webhook** és **nem** a hello egyéb műveleteket.
2. Beillesztés hello Slackhez URL-címet a hello **Webhook URL-CÍMÉT** mező.
3. A beállításnak a hello túl**közé tartoznak az egyéni JSON-adattartalmat**.
4. A tartalékidő vár a hasznos adatok között JSON formátumú, nevű paraméterrel *szöveg*.  Ez a hello szöveg, amely létrehozza üdvözlőüzenetére jelennek majd.  Használhat egy vagy több riasztás paramétereknek hello hello  *#*  szimbólumának, mint például a következő hello.
   
    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds hello over threshold of #thresholdvalue ."
    }
    ```
   
    ![Példa JSON-adattartalmat](media/log-analytics-alerts-webhooks/oms-webhooks07.png)
5. Kattintson a **mentése** toosave hello riasztási szabályt.
6. Várjon, amíg egy riasztási toobe létrehozott elegendő időt, és ellenőrizze a Slackhez egy üzenet, amely hasonló toohello következő lesz.
   
   ![Példa webhook a Slackhez](media/log-analytics-alerts-webhooks/oms-webhooks08.png)

### <a name="advanced-webhook-payload-for-slack"></a>Speciális webhook hasznos a Slackhez
A Slackhez bejövő üzenetek nagymértékben testre szabható. További információkért lásd: [bejövő Webhookok](https://api.slack.com/incoming-webhooks) hello közzététele a Slack-webhelyen. Az alábbiakban olvashat egy összetettebb hasznos toocreate formázás részletes üzenet:

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


A Slack hasonló toohello következő ez hoz létre egy üzenetet.

![a Slackhez példa üzenet](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Összefoglalás
A riasztási szabálynak helyen egy üzenet tooSlack kellene lennie, minden alkalommal, amikor hello feltétel teljesülése esetén.  

Ez az egy műveletet, amely a válasz tooan riasztást hozhat létre csak egy példája.  A webhook művelet egy másik külső szolgáltatás, a runbook művelet toostart egy Azure Automation, vagy egy e-mailek művelet toosend runbookot meghívó létrehozhat egy levelezési tooyourself vagy egyéb.   

## <a name="next-steps"></a>Következő lépések
* Tudnivalók más [Naplóelemzési műveletek riasztási](log-analytics-alerts-actions.md) beleértve más műveletek.


