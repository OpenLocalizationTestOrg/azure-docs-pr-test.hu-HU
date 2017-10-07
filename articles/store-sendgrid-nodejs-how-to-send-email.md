---
title: "aaaHow toouse hello SendGrid e-mail szolgáltatás (Node.js) |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket hello SendGrid e-mail szolgáltatás az Azure-on. A Kódminták hello Node.js API használatával."
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
ms.openlocfilehash: fd617b6aaa656e7b5dd51c51ebb0db1e848450f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-nodejs"></a><span data-ttu-id="e8452-104">Hogyan tooSend E-mail használatával SendGrid Node.js-ből</span><span class="sxs-lookup"><span data-stu-id="e8452-104">How tooSend Email Using SendGrid from Node.js</span></span>
<span data-ttu-id="e8452-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok a sendgrid e-mail szolgáltatás az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="e8452-105">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="e8452-106">hello minták hello Node.js API segítségével készül.</span><span class="sxs-lookup"><span data-stu-id="e8452-106">hello samples are written using hello Node.js API.</span></span> <span data-ttu-id="e8452-107">hello tárgyalt forgatókönyvekben szerepel a **hozhat létre, e-mailek**, **e-mailek küldéséhez**, **mellékletek hozzáadása**, **szűrőkkel**, és **tulajdonságainak frissítése**.</span><span class="sxs-lookup"><span data-stu-id="e8452-107">hello scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="e8452-108">A SendGrid és e-mailek küldéséhez további információkért lásd: hello [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="e8452-108">For more information on SendGrid and sending email, see hello [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="e8452-109">Mi az az üdvözlő E-mail szolgáltatás SendGrid?</span><span class="sxs-lookup"><span data-stu-id="e8452-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="e8452-110">SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt.</span><span class="sxs-lookup"><span data-stu-id="e8452-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="e8452-111">Gyakori SendGrid használati forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="e8452-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="e8452-112">Visszaigazolások toocustomers automatikus küldése</span><span class="sxs-lookup"><span data-stu-id="e8452-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="e8452-113">Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája</span><span class="sxs-lookup"><span data-stu-id="e8452-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="e8452-114">Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése</span><span class="sxs-lookup"><span data-stu-id="e8452-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="e8452-115">Toohelp azonosíthatja a trendeket jelentések létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8452-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="e8452-116">Ügyfél-lekérdezések továbbítása</span><span class="sxs-lookup"><span data-stu-id="e8452-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="e8452-117">E-mail értesítések az alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="e8452-117">Email notifications from your application</span></span>

<span data-ttu-id="e8452-118">További információkért lásd: [https://sendgrid.com](https://sendgrid.com).</span><span class="sxs-lookup"><span data-stu-id="e8452-118">For more information, see [https://sendgrid.com](https://sendgrid.com).</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="e8452-119">A SendGrid-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e8452-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-nodejs-module"></a><span data-ttu-id="e8452-120">Hivatkozás hello SendGrid Node.js modul</span><span class="sxs-lookup"><span data-stu-id="e8452-120">Reference hello SendGrid Node.js Module</span></span>
<span data-ttu-id="e8452-121">hello SendGrid modul a Node.js hello csomópont Csomagkezelő (npm) keresztül telepíthető hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="e8452-121">hello SendGrid module for Node.js can be installed through hello node package manager (npm) by using hello following command:</span></span>

    npm install sendgrid

<span data-ttu-id="e8452-122">A telepítést követően a kód a következő hello segítségével megkövetelheti hello modul az alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="e8452-122">After installation, you can require hello module in your application by using hello following code:</span></span>

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

<span data-ttu-id="e8452-123">hello SendGrid modul exportálja hello **SendGrid** és **E-mail** funkciók.</span><span class="sxs-lookup"><span data-stu-id="e8452-123">hello SendGrid module exports hello **SendGrid** and **Email** functions.</span></span>
<span data-ttu-id="e8452-124">**SendGrid** felelős a webes API-n keresztül e-mail elküldése közben **E-mail** foglalja az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="e8452-124">**SendGrid** is responsible for sending email through Web API, while **Email** encapsulates an email message.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="e8452-125">Hogyan: hozzon létre egy e-mailt</span><span class="sxs-lookup"><span data-stu-id="e8452-125">How to: Create an Email</span></span>
<span data-ttu-id="e8452-126">Hello SendGrid modullal e-mailt létrehozása magában foglalja először e-mailt hello E-mail funkcióval, és majd küldje el azt a hello SendGrid függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="e8452-126">Creating an email message using hello SendGrid module involves first creating an email message using hello Email function, and then sending it using hello SendGrid function.</span></span> <span data-ttu-id="e8452-127">hello az alábbiakban látható egy példa egy új üzenetet hello E-mail függvény használatával hoz létre:</span><span class="sxs-lookup"><span data-stu-id="e8452-127">hello following is an example of creating a new message using hello Email function:</span></span>

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

<span data-ttu-id="e8452-128">HTML üzenet hello html tulajdonság beállításával támogató ügyfelek is megadható.</span><span class="sxs-lookup"><span data-stu-id="e8452-128">You can also specify an HTML message for clients that support it by setting hello html property.</span></span> <span data-ttu-id="e8452-129">Példa:</span><span class="sxs-lookup"><span data-stu-id="e8452-129">For example:</span></span>

    html: This is a sample <b>HTML<b> email message.

<span data-ttu-id="e8452-130">Ügyfelek, amelyek nem támogatják a HTML-üzenetek mindkét hello szöveg és a html-tulajdonságok beállítása sikeres-e tartalékként történő szöveges tartalom biztosít.</span><span class="sxs-lookup"><span data-stu-id="e8452-130">Setting both hello text and html properties provides graceful fallback to text content for clients that cannot support HTML messages.</span></span>

<span data-ttu-id="e8452-131">Az összes tulajdonság hello E-mail függvény által támogatott további információkért lásd: [sendgrid-nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="e8452-131">For more information on all properties supported by hello Email function, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="e8452-132">Útmutató: az e-mailek küldése</span><span class="sxs-lookup"><span data-stu-id="e8452-132">How to: Send an Email</span></span>
<span data-ttu-id="e8452-133">Miután létrehozta az üdvözlő E-mail függvény használatával e-mailt, elküldheti azt hello SendGrid által biztosított webes API használatával.</span><span class="sxs-lookup"><span data-stu-id="e8452-133">After creating an email message using hello Email function, you can send it using hello Web API provided by SendGrid.</span></span> 

### <a name="web-api"></a><span data-ttu-id="e8452-134">Webes API</span><span class="sxs-lookup"><span data-stu-id="e8452-134">Web API</span></span>
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> <span data-ttu-id="e8452-135">Amíg a fenti példák hello sikeres megjelenítése egy e-mailek objektum és a visszahívás függvényben, e-mailes tulajdonságai közvetlenül megadásával közvetlenül is hívhat meg hello küldési függvény.</span><span class="sxs-lookup"><span data-stu-id="e8452-135">While hello above examples show passing in an email object and callback function, you can also directly invoke hello send function by directly specifying email properties.</span></span> <span data-ttu-id="e8452-136">Példa:</span><span class="sxs-lookup"><span data-stu-id="e8452-136">For example:</span></span>  
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

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="e8452-137">Hogyan: hozzá mellékletet</span><span class="sxs-lookup"><span data-stu-id="e8452-137">How to: Add an Attachment</span></span>
<span data-ttu-id="e8452-138">Mellékletek felveheti tooa üzenet hello hello fájl neve és elérési útjait megadásával **fájlok** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8452-138">Attachments can be added tooa message by specifying hello file name(s) and path(s) in hello **files** property.</span></span> <span data-ttu-id="e8452-139">hello a következő példa bemutatja a melléklet küldése:</span><span class="sxs-lookup"><span data-stu-id="e8452-139">hello following example demonstrates sending an attachment:</span></span>

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used toospecify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> <span data-ttu-id="e8452-140">Hello használatakor **fájlok** tulajdonság, hello fájl elérhetőnek kell lennie keresztül [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span><span class="sxs-lookup"><span data-stu-id="e8452-140">When using hello **files** property, hello file must be accessible through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span></span> <span data-ttu-id="e8452-141">Ha tooattach kívánja hello fájl az Azure Storage, például egy Blob tároló kell másolnia hello toolocal fájltároló vagy tooan Azure meghajtó hello segítségével mellékletként küldése előtt **fájlok** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8452-141">If hello file you wish tooattach is hosted in Azure Storage, such as in a Blob container, you must first copy hello file toolocal storage or tooan Azure drive before it can be sent as an attachment using hello **files** property.</span></span>
> 
> 

## <a name="how-to-use-filters-tooenable-footers-and-tracking"></a><span data-ttu-id="e8452-142">Hogyan: szűrési tooEnable láblécek és nyomon követése</span><span class="sxs-lookup"><span data-stu-id="e8452-142">How to: Use Filters tooEnable Footers and Tracking</span></span>
<span data-ttu-id="e8452-143">SendGrid szűrők hello használata további e-mail-funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="e8452-143">SendGrid provides additional email functionality through hello use of filters.</span></span> <span data-ttu-id="e8452-144">Ezek a beállítások tooan e-mailt ahhoz, hogy bizonyos funkciók, például kattintson nyomon követése, Google analytics, nyomon követése, előfizetés adható hozzá és így tovább.</span><span class="sxs-lookup"><span data-stu-id="e8452-144">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="e8452-145">Szűrők teljes listáját lásd: [szűrőbeállítások][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="e8452-145">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

<span data-ttu-id="e8452-146">Szűrők hello segítségével lehet alkalmazott tooa üzenet **szűrők** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8452-146">Filters can be applied tooa message by using hello **filters** property.</span></span>
<span data-ttu-id="e8452-147">Minden egyes szűrő szűrő-specifikus beállításokat tartalmazó kivonat szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="e8452-147">Each filter is specified by a hash containing filter-specific settings.</span></span>
<span data-ttu-id="e8452-148">hello alábbi példák bemutatják, hello élőláb, majd kattintson szűrők nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="e8452-148">hello following examples demonstrate hello footer and click tracking filters:</span></span>

### <a name="footer"></a><span data-ttu-id="e8452-149">Élőláb</span><span class="sxs-lookup"><span data-stu-id="e8452-149">Footer</span></span>
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

### <a name="click-tracking"></a><span data-ttu-id="e8452-150">Kattintson a nyomon követése</span><span class="sxs-lookup"><span data-stu-id="e8452-150">Click Tracking</span></span>
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

## <a name="how-to-update-email-properties"></a><span data-ttu-id="e8452-151">Útmutató: E-mail tulajdonságainak frissítése</span><span class="sxs-lookup"><span data-stu-id="e8452-151">How to: Update Email Properties</span></span>
<span data-ttu-id="e8452-152">Néhány e-mail tulajdonság használatával felülírható  **beállítása*tulajdonság***, illetve használatával  **hozzáadása*tulajdonság***.</span><span class="sxs-lookup"><span data-stu-id="e8452-152">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span> <span data-ttu-id="e8452-153">Például hozzáadhat további címzetteket használatával</span><span class="sxs-lookup"><span data-stu-id="e8452-153">For example, you can add additional recipients by using</span></span>

    email.addTo('jeff@contoso.com');

<span data-ttu-id="e8452-154">vagy állítsa be a szűrő használatával</span><span class="sxs-lookup"><span data-stu-id="e8452-154">or set a filter by using</span></span>

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

<span data-ttu-id="e8452-155">További információkért lásd: [sendgrid-nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="e8452-155">For more information, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="e8452-156">Útmutató: további SendGrid szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="e8452-156">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="e8452-157">SendGrid kínál webes API-k, hogy tooleverage további SendGrid funkciót használhatja az Azure alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="e8452-157">SendGrid offers web-based APIs that you can use tooleverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="e8452-158">Teljes további információkért lásd: hello [SendGrid API-JÁNAK dokumentációja][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="e8452-158">For full details, see hello [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8452-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8452-159">Next Steps</span></span>
<span data-ttu-id="e8452-160">Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="e8452-160">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="e8452-161">SendGrid Node.js modul tárház: [sendgrid-nodejs][sendgrid-nodejs]</span><span class="sxs-lookup"><span data-stu-id="e8452-161">SendGrid Node.js module repository: [sendgrid-nodejs][sendgrid-nodejs]</span></span>
* <span data-ttu-id="e8452-162">SendGrid API dokumentációjának: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="e8452-162">SendGrid API documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="e8452-163">SendGrid különleges ajánlat Azure ügyfeleknek: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span><span class="sxs-lookup"><span data-stu-id="e8452-163">SendGrid special offer for Azure customers: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span></span>

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[felhőalapú szolgáltatás]: https://sendgrid.com/email-solutions
[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/transactional-email
