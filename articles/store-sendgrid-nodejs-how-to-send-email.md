---
title: "A SendGrid e-mail szolgáltatás (Node.js) használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket a SendGrid e-mail szolgáltatás az Azure-on. A Kódminták Node.js API használatával."
services: 
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: 
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: 327cea3a24cc47a9cc463b37cc2346ebc475ef7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a><span data-ttu-id="36b29-104">Hogyan küldhet e-mailek SendGrid Node.js-ből</span><span class="sxs-lookup"><span data-stu-id="36b29-104">How to Send Email Using SendGrid from Node.js</span></span>
<span data-ttu-id="36b29-105">Ez az útmutató bemutatja, hogyan Azure a SendGrid e-mail szolgáltatás közös programozási feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="36b29-105">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="36b29-106">A minták íródtak, a Node.js API használatával.</span><span class="sxs-lookup"><span data-stu-id="36b29-106">The samples are written using the Node.js API.</span></span> <span data-ttu-id="36b29-107">Az ismertetett forgatókönyvek **hozhat létre, e-mailek**, **e-mailek küldéséhez**, **mellékletek hozzáadása**, **szűrőkkel**, és **tulajdonságainak frissítése**.</span><span class="sxs-lookup"><span data-stu-id="36b29-107">The scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="36b29-108">A SendGrid és e-mailek küldéséhez további információkért lásd: a [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="36b29-108">For more information on SendGrid and sending email, see the [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="36b29-109">Mi az a SendGrid E-mail szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="36b29-109">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="36b29-110">SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt.</span><span class="sxs-lookup"><span data-stu-id="36b29-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="36b29-111">Gyakori SendGrid használati forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="36b29-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="36b29-112">Automatikusan az ügyfél számára a visszaigazolások küldésére</span><span class="sxs-lookup"><span data-stu-id="36b29-112">Automatically sending receipts to customers</span></span>
* <span data-ttu-id="36b29-113">Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája</span><span class="sxs-lookup"><span data-stu-id="36b29-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="36b29-114">Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése</span><span class="sxs-lookup"><span data-stu-id="36b29-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="36b29-115">Jelentések segítségével azonosíthatja a trendeket létrehozása</span><span class="sxs-lookup"><span data-stu-id="36b29-115">Generating reports to help identify trends</span></span>
* <span data-ttu-id="36b29-116">Ügyfél-lekérdezések továbbítása</span><span class="sxs-lookup"><span data-stu-id="36b29-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="36b29-117">E-mail értesítések az alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="36b29-117">Email notifications from your application</span></span>

<span data-ttu-id="36b29-118">További információkért lásd: [https://sendgrid.com](https://sendgrid.com).</span><span class="sxs-lookup"><span data-stu-id="36b29-118">For more information, see [https://sendgrid.com](https://sendgrid.com).</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="36b29-119">A SendGrid-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="36b29-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a><span data-ttu-id="36b29-120">A SendGrid Node.js modul hivatkozik</span><span class="sxs-lookup"><span data-stu-id="36b29-120">Reference the SendGrid Node.js Module</span></span>
<span data-ttu-id="36b29-121">A SendGrid modul, a Node.js a csomópont package manager (npm) keresztül telepíthető a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="36b29-121">The SendGrid module for Node.js can be installed through the node package manager (npm) by using the following command:</span></span>

    npm install sendgrid

<span data-ttu-id="36b29-122">A telepítés után is megkövetelhető a modul az alkalmazásban az alábbi kód használatával:</span><span class="sxs-lookup"><span data-stu-id="36b29-122">After installation, you can require the module in your application by using the following code:</span></span>

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

<span data-ttu-id="36b29-123">A SendGrid modul exportálja a **SendGrid** és **E-mail** funkciók.</span><span class="sxs-lookup"><span data-stu-id="36b29-123">The SendGrid module exports the **SendGrid** and **Email** functions.</span></span>
<span data-ttu-id="36b29-124">**SendGrid** felelős a webes API-n keresztül e-mail elküldése közben **E-mail** foglalja az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="36b29-124">**SendGrid** is responsible for sending email through Web API, while **Email** encapsulates an email message.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="36b29-125">Hogyan: hozzon létre egy e-mailt</span><span class="sxs-lookup"><span data-stu-id="36b29-125">How to: Create an Email</span></span>
<span data-ttu-id="36b29-126">A SendGrid-modullal e-mailt létrehozása magában foglalja először e-mailt az e-mailek funkcióval, és majd küldje el azt a SendGrid függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="36b29-126">Creating an email message using the SendGrid module involves first creating an email message using the Email function, and then sending it using the SendGrid function.</span></span> <span data-ttu-id="36b29-127">A következő egy példa az e-mailek funkcióval egy új üzenet létrehozása:</span><span class="sxs-lookup"><span data-stu-id="36b29-127">The following is an example of creating a new message using the Email function:</span></span>

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

<span data-ttu-id="36b29-128">Egy HTML-üzenetet is megadhat a html-tulajdonság beállításával támogató ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="36b29-128">You can also specify an HTML message for clients that support it by setting the html property.</span></span> <span data-ttu-id="36b29-129">Példa:</span><span class="sxs-lookup"><span data-stu-id="36b29-129">For example:</span></span>

    html: This is a sample <b>HTML<b> email message.

<span data-ttu-id="36b29-130">A szöveg és a html-tulajdonságok beállítása sikeres-e tartalékként történő szöveges tartalom biztosít az ügyfelek, amelyek nem támogatják a HTML-üzenetek.</span><span class="sxs-lookup"><span data-stu-id="36b29-130">Setting both the text and html properties provides graceful fallback to text content for clients that cannot support HTML messages.</span></span>

<span data-ttu-id="36b29-131">Az e-mailek függvény által támogatott összes tulajdonság további információkért lásd: [sendgrid-nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="36b29-131">For more information on all properties supported by the Email function, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="36b29-132">Útmutató: az e-mailek küldése</span><span class="sxs-lookup"><span data-stu-id="36b29-132">How to: Send an Email</span></span>
<span data-ttu-id="36b29-133">Miután létrehozta az e-mailek funkcióval e-mailt, elküldheti azt a SendGrid által biztosított webes API használatával.</span><span class="sxs-lookup"><span data-stu-id="36b29-133">After creating an email message using the Email function, you can send it using the Web API provided by SendGrid.</span></span> 

### <a name="web-api"></a><span data-ttu-id="36b29-134">Webes API</span><span class="sxs-lookup"><span data-stu-id="36b29-134">Web API</span></span>
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> <span data-ttu-id="36b29-135">A fenti példák azt szemléltetik sikeres egy e-mailek objektum és a visszahívás függvényben, amíg e-mailes tulajdonságai közvetlenül megadásával közvetlenül is hívhat meg a Küldés függvény.</span><span class="sxs-lookup"><span data-stu-id="36b29-135">While the above examples show passing in an email object and callback function, you can also directly invoke the send function by directly specifying email properties.</span></span> <span data-ttu-id="36b29-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="36b29-136">For example:</span></span>  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="36b29-137">Hogyan: hozzá mellékletet</span><span class="sxs-lookup"><span data-stu-id="36b29-137">How to: Add an Attachment</span></span>
<span data-ttu-id="36b29-138">Mellékletek lehet hozzáadni egy üzenetet a fájl neve és elérési útjait a megadásával a **fájlok** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="36b29-138">Attachments can be added to a message by specifying the file name(s) and path(s) in the **files** property.</span></span> <span data-ttu-id="36b29-139">A következő példa bemutatja a melléklet küldése:</span><span class="sxs-lookup"><span data-stu-id="36b29-139">The following example demonstrates sending an attachment:</span></span>

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> <span data-ttu-id="36b29-140">Használatakor a **fájlok** tulajdonság, a fájl elérhetőnek kell lennie keresztül [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span><span class="sxs-lookup"><span data-stu-id="36b29-140">When using the **files** property, the file must be accessible through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span></span> <span data-ttu-id="36b29-141">Ha a fájlt csatolni szeretné az Azure Storage, például egy Blob tároló kell másolnia a fájlt helyi tárolóhoz vagy egy Azure meghajtó egy mellékletet használatával küldése előtt a **fájlok** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="36b29-141">If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.</span></span>
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a><span data-ttu-id="36b29-142">How to: Enable láblécek és követési szűrők használata</span><span class="sxs-lookup"><span data-stu-id="36b29-142">How to: Use Filters to Enable Footers and Tracking</span></span>
<span data-ttu-id="36b29-143">SendGrid szűrők segítségével további e-mail-funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="36b29-143">SendGrid provides additional email functionality through the use of filters.</span></span> <span data-ttu-id="36b29-144">Ezek a beállítások, amelyek e-mailt ahhoz, hogy bizonyos funkciók, például követési kattintson, Google analytics, követési előfizetés, és így tovább is hozzáadhatók.</span><span class="sxs-lookup"><span data-stu-id="36b29-144">These are settings that can be added to an email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="36b29-145">Szűrők teljes listáját lásd: [szűrőbeállítások][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="36b29-145">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

<span data-ttu-id="36b29-146">Szűrők használatával is alkalmazható egy üzenetet a **szűrők** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="36b29-146">Filters can be applied to a message by using the **filters** property.</span></span>
<span data-ttu-id="36b29-147">Minden egyes szűrő szűrő-specifikus beállításokat tartalmazó kivonat szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="36b29-147">Each filter is specified by a hash containing filter-specific settings.</span></span>
<span data-ttu-id="36b29-148">Az alábbi példák bemutatják, a láblécben, majd kattintson a szűrő nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="36b29-148">The following examples demonstrate the footer and click tracking filters:</span></span>

### <a name="footer"></a><span data-ttu-id="36b29-149">Élőláb</span><span class="sxs-lookup"><span data-stu-id="36b29-149">Footer</span></span>
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a><span data-ttu-id="36b29-150">Kattintson a nyomon követése</span><span class="sxs-lookup"><span data-stu-id="36b29-150">Click Tracking</span></span>
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a><span data-ttu-id="36b29-151">Útmutató: E-mail tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="36b29-151">How to: Update Email Properties</span></span>
<span data-ttu-id="36b29-152">Néhány e-mail tulajdonság használatával felülírható  **beállítása*tulajdonság***, illetve használatával  **hozzáadása*tulajdonság***.</span><span class="sxs-lookup"><span data-stu-id="36b29-152">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span> <span data-ttu-id="36b29-153">Például hozzáadhat további címzetteket használatával</span><span class="sxs-lookup"><span data-stu-id="36b29-153">For example, you can add additional recipients by using</span></span>

    email.addTo('jeff@contoso.com');

<span data-ttu-id="36b29-154">vagy állítsa be a szűrő használatával</span><span class="sxs-lookup"><span data-stu-id="36b29-154">or set a filter by using</span></span>

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

<span data-ttu-id="36b29-155">További információkért lásd: [sendgrid-nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="36b29-155">For more information, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="36b29-156">Útmutató: további SendGrid szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="36b29-156">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="36b29-157">SendGrid webes API-k segítségével kihasználhatja az Azure alkalmazásról további SendGrid funkciókat kínál.</span><span class="sxs-lookup"><span data-stu-id="36b29-157">SendGrid offers web-based APIs that you can use to leverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="36b29-158">Teljes részletekért lásd: a [SendGrid API-JÁNAK dokumentációja][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="36b29-158">For full details, see the [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="36b29-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36b29-159">Next Steps</span></span>
<span data-ttu-id="36b29-160">Most, hogy megismerte a SendGrid E-mail szolgáltatás alapjait, az alábbi hivatkozásokból további.</span><span class="sxs-lookup"><span data-stu-id="36b29-160">Now that you've learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="36b29-161">SendGrid Node.js modul tárház: [sendgrid-nodejs][sendgrid-nodejs]</span><span class="sxs-lookup"><span data-stu-id="36b29-161">SendGrid Node.js module repository: [sendgrid-nodejs][sendgrid-nodejs]</span></span>
* <span data-ttu-id="36b29-162">SendGrid API dokumentációjának: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="36b29-162">SendGrid API documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="36b29-163">SendGrid különleges ajánlat Azure ügyfeleknek: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span><span class="sxs-lookup"><span data-stu-id="36b29-163">SendGrid special offer for Azure customers: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span></span>

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
<span data-ttu-id="36b29-164">[felhőalapú szolgáltatás]: https://sendgrid.com/email-solutions</span><span class="sxs-lookup"><span data-stu-id="36b29-164">[cloud-based email service]: https://sendgrid.com/email-solutions</span></span>
<span data-ttu-id="36b29-165">[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/transactional-email</span><span class="sxs-lookup"><span data-stu-id="36b29-165">[transactional email delivery]: https://sendgrid.com/transactional-email</span></span>
