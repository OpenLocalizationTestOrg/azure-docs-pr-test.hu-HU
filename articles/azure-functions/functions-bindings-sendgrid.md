---
title: "aaaAzure funkciók SendGrid kötések |} Microsoft Docs"
description: "Az Azure Functions SendGrid kötések referencia"
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/16/2017
ms.author: rachelap
ms.openlocfilehash: 10a3837875eb6ae18e6c789bcf64cc401cf5f26a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-sendgrid-bindings"></a><span data-ttu-id="53ab1-103">Az Azure Functions SendGrid kötések</span><span class="sxs-lookup"><span data-stu-id="53ab1-103">Azure Functions SendGrid bindings</span></span>

<span data-ttu-id="53ab1-104">Ez a cikk azt ismerteti, hogyan tooconfigure és az Azure Functions SendGrid kötések munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="53ab1-104">This article explains how tooconfigure and work with SendGrid bindings in Azure Functions.</span></span> <span data-ttu-id="53ab1-105">A SendGrid programozott módon használhatja az Azure Functions toosend testreszabott e-mail.</span><span class="sxs-lookup"><span data-stu-id="53ab1-105">With SendGrid, you can use Azure Functions toosend customized email programmatically.</span></span>

<span data-ttu-id="53ab1-106">Ez a cikk az Azure Functions fejlesztői útmutatót.</span><span class="sxs-lookup"><span data-stu-id="53ab1-106">This article is reference information for Azure Functions developers.</span></span> <span data-ttu-id="53ab1-107">Ha új tooAzure funkciók, indítsa el a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="53ab1-107">If you're new tooAzure Functions, start with hello following resources:</span></span>

<span data-ttu-id="53ab1-108">[Az első Azure-függvény létrehozása](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="53ab1-108">[Create your first Azure Function](functions-create-first-azure-function.md).</span></span> 
<span data-ttu-id="53ab1-109">[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), vagy [csomópont](functions-reference-node.md) fejlesztői hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="53ab1-109">[C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md), or [Node](functions-reference-node.md) developer references.</span></span>

## <a name="functionjson-for-sendgrid-bindings"></a><span data-ttu-id="53ab1-110">a SendGrid kötések Function.JSON</span><span class="sxs-lookup"><span data-stu-id="53ab1-110">function.json for SendGrid bindings</span></span>

<span data-ttu-id="53ab1-111">Az Azure Functions biztosít SendGrid egy kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="53ab1-111">Azure Functions provides an output binding for SendGrid.</span></span> <span data-ttu-id="53ab1-112">a kimeneti hello SendGrid kötés lehetővé teszi a toocreate, majd küldje el e-mailek programozott módon.</span><span class="sxs-lookup"><span data-stu-id="53ab1-112">hello SendGrid output binding enables you toocreate and send email programmatically.</span></span> 

<span data-ttu-id="53ab1-113">hello SendGrid kötés támogatja-e hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="53ab1-113">hello SendGrid binding supports hello following properties:</span></span>

- <span data-ttu-id="53ab1-114">`name`: Szükséges – hello hello kérelemmel vagy a kérelem törzsében függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="53ab1-114">`name` : Required - hello variable name used in function code for hello request or request body.</span></span> <span data-ttu-id="53ab1-115">Ez az érték ```$return``` Ha csak egy visszatérési értéket.</span><span class="sxs-lookup"><span data-stu-id="53ab1-115">This value is ```$return``` when there is only one return value.</span></span> 
- <span data-ttu-id="53ab1-116">`type`: Szükséges – túl lehet beállított "sendGrid."</span><span class="sxs-lookup"><span data-stu-id="53ab1-116">`type` : Required - must be set too"sendGrid."</span></span>
- <span data-ttu-id="53ab1-117">`direction`: Szükséges – kell állítható túl "lejárt."</span><span class="sxs-lookup"><span data-stu-id="53ab1-117">`direction` : Required - must be set too"out."</span></span>
- <span data-ttu-id="53ab1-118">`apiKey`: Szükséges – toohello neve a API-kulcs hello függvény App Alkalmazásbeállítások tárolva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="53ab1-118">`apiKey` : Required - must be set toohello name of your API key stored in hello Function App's app settings.</span></span>
- <span data-ttu-id="53ab1-119">`to`: hello címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="53ab1-119">`to` : hello recipient's email address.</span></span>
- <span data-ttu-id="53ab1-120">`from`: hello feladó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="53ab1-120">`from` : hello sender's email address.</span></span>
- <span data-ttu-id="53ab1-121">`subject`: hello hello e-mail tárgya.</span><span class="sxs-lookup"><span data-stu-id="53ab1-121">`subject` : hello subject of hello email.</span></span>
- <span data-ttu-id="53ab1-122">`text`: e-mailek tartalmának hello.</span><span class="sxs-lookup"><span data-stu-id="53ab1-122">`text` : hello email content.</span></span>

<span data-ttu-id="53ab1-123">Példa **function.json**:</span><span class="sxs-lookup"><span data-stu-id="53ab1-123">Example of **function.json**:</span></span>

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

> [!NOTE]
> <span data-ttu-id="53ab1-124">Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="53ab1-124">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="53ab1-125">Ez a művelet megvédi a bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="53ab1-125">This action safeguards your sensitive information.</span></span>
>
>

## <a name="c-example-of-hello-sendgrid-output-binding"></a><span data-ttu-id="53ab1-126">C# – példa hello SendGrid kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="53ab1-126">C# example of hello SendGrid output binding</span></span>

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

## <a name="node-example-of-hello-sendgrid-output-binding"></a><span data-ttu-id="53ab1-127">Hello csomópont példa SendGrid kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="53ab1-127">Node example of hello SendGrid output binding</span></span>

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: "sender@contoso.com",        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};

```

## <a name="next-steps"></a><span data-ttu-id="53ab1-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53ab1-128">Next steps</span></span>
<span data-ttu-id="53ab1-129">Az Azure Functions más kötései és eseményindítók kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="53ab1-129">For information about other bindings and triggers for Azure Functions, see</span></span> 
- [<span data-ttu-id="53ab1-130">Az Azure Functions eseményindítók és kötések fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="53ab1-130">Azure Functions triggers and bindings developer reference</span></span>](functions-triggers-bindings.md)

- <span data-ttu-id="53ab1-131">[Gyakorlati tanácsok az Azure Functions](functions-best-practices.md) néhány gyakorlati tanácsok toouse sorolja fel, az Azure Functions létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="53ab1-131">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices toouse when creating Azure Functions.</span></span>

- <span data-ttu-id="53ab1-132">[Az Azure Functions fejlesztői segédanyagai](functions-reference.md) programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="53ab1-132">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>
