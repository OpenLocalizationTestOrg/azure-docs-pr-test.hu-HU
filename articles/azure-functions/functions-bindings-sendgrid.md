---
title: "Az Azure Functions SendGrid kötések |} Microsoft Docs"
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
ms.openlocfilehash: 445a40a884e648cdb2a57f8ef43bed4f8a3efcf2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-sendgrid-bindings"></a><span data-ttu-id="a1e81-103">Az Azure Functions SendGrid kötések</span><span class="sxs-lookup"><span data-stu-id="a1e81-103">Azure Functions SendGrid bindings</span></span>

<span data-ttu-id="a1e81-104">Ez a cikk azt ismerteti, hogyan konfigurálását és az Azure Functions SendGrid kötések használatát.</span><span class="sxs-lookup"><span data-stu-id="a1e81-104">This article explains how to configure and work with SendGrid bindings in Azure Functions.</span></span> <span data-ttu-id="a1e81-105">Sendgrid használhatja az Azure Functions programozott módon testreszabott e-mail üzenetek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="a1e81-105">With SendGrid, you can use Azure Functions to send customized email programmatically.</span></span>

<span data-ttu-id="a1e81-106">Ez a cikk az Azure Functions fejlesztői útmutatót.</span><span class="sxs-lookup"><span data-stu-id="a1e81-106">This article is reference information for Azure Functions developers.</span></span> <span data-ttu-id="a1e81-107">Ha most ismerkedik az Azure Functions, kezdje a a következőket:</span><span class="sxs-lookup"><span data-stu-id="a1e81-107">If you're new to Azure Functions, start with the following resources:</span></span>

<span data-ttu-id="a1e81-108">[Az első Azure-függvény létrehozása](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="a1e81-108">[Create your first Azure Function](functions-create-first-azure-function.md).</span></span> 
<span data-ttu-id="a1e81-109">[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), vagy [csomópont](functions-reference-node.md) fejlesztői hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a1e81-109">[C#](functions-reference-csharp.md), [F#](functions-reference-fsharp.md), or [Node](functions-reference-node.md) developer references.</span></span>

## <a name="functionjson-for-sendgrid-bindings"></a><span data-ttu-id="a1e81-110">a SendGrid kötések Function.JSON</span><span class="sxs-lookup"><span data-stu-id="a1e81-110">function.json for SendGrid bindings</span></span>

<span data-ttu-id="a1e81-111">Az Azure Functions biztosít SendGrid egy kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="a1e81-111">Azure Functions provides an output binding for SendGrid.</span></span> <span data-ttu-id="a1e81-112">A SendGrid kimeneti kötés lehetővé teszi, hogy a létrehozása és elküldése e-mailben elküldi programozott módon.</span><span class="sxs-lookup"><span data-stu-id="a1e81-112">The SendGrid output binding enables you to create and send email programmatically.</span></span> 

<span data-ttu-id="a1e81-113">A SendGrid kötés támogatja-e a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="a1e81-113">The SendGrid binding supports the following properties:</span></span>

- <span data-ttu-id="a1e81-114">`name`: Szükséges – a kérelem vagy kérelemtörzset függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="a1e81-114">`name` : Required - the variable name used in function code for the request or request body.</span></span> <span data-ttu-id="a1e81-115">Ez az érték ```$return``` Ha csak egy visszatérési értéket.</span><span class="sxs-lookup"><span data-stu-id="a1e81-115">This value is ```$return``` when there is only one return value.</span></span> 
- <span data-ttu-id="a1e81-116">`type`: Szükséges – kell állítani "sendGrid."</span><span class="sxs-lookup"><span data-stu-id="a1e81-116">`type` : Required - must be set to "sendGrid."</span></span>
- <span data-ttu-id="a1e81-117">`direction`: Szükséges – kell állítani "out".</span><span class="sxs-lookup"><span data-stu-id="a1e81-117">`direction` : Required - must be set to "out."</span></span>
- <span data-ttu-id="a1e81-118">`apiKey`: A függvény App Alkalmazásbeállítások tárolja az API-kulcs nevét szükséges – kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a1e81-118">`apiKey` : Required - must be set to the name of your API key stored in the Function App's app settings.</span></span>
- <span data-ttu-id="a1e81-119">`to`: a címzett e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="a1e81-119">`to` : the recipient's email address.</span></span>
- <span data-ttu-id="a1e81-120">`from`: a feladó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="a1e81-120">`from` : the sender's email address.</span></span>
- <span data-ttu-id="a1e81-121">`subject`: az e-mail tárgyát.</span><span class="sxs-lookup"><span data-stu-id="a1e81-121">`subject` : the subject of the email.</span></span>
- <span data-ttu-id="a1e81-122">`text`: az e-mailek tartalmának.</span><span class="sxs-lookup"><span data-stu-id="a1e81-122">`text` : the email content.</span></span>

<span data-ttu-id="a1e81-123">Példa **function.json**:</span><span class="sxs-lookup"><span data-stu-id="a1e81-123">Example of **function.json**:</span></span>

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
> <span data-ttu-id="a1e81-124">Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="a1e81-124">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="a1e81-125">Ez a művelet megvédi a bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="a1e81-125">This action safeguards your sensitive information.</span></span>
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a><span data-ttu-id="a1e81-126">A SendGrid például C# kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="a1e81-126">C# example of the SendGrid output binding</span></span>

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

## <a name="node-example-of-the-sendgrid-output-binding"></a><span data-ttu-id="a1e81-127">A SendGrid csomópont példa kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="a1e81-127">Node example of the SendGrid output binding</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a1e81-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1e81-128">Next steps</span></span>
<span data-ttu-id="a1e81-129">Az Azure Functions más kötései és eseményindítók kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="a1e81-129">For information about other bindings and triggers for Azure Functions, see</span></span> 
- [<span data-ttu-id="a1e81-130">Az Azure Functions eseményindítók és kötések fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="a1e81-130">Azure Functions triggers and bindings developer reference</span></span>](functions-triggers-bindings.md)

- <span data-ttu-id="a1e81-131">[Gyakorlati tanácsok az Azure Functions](functions-best-practices.md) néhány ajánlott eljárás az Azure Functions létrehozásakor használandó sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="a1e81-131">[Best practices for Azure Functions](functions-best-practices.md) Lists some best practices to use when creating Azure Functions.</span></span>

- <span data-ttu-id="a1e81-132">[Az Azure Functions fejlesztői segédanyagai](functions-reference.md) programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="a1e81-132">[Azure Functions developer reference](functions-reference.md) Programmer reference for coding functions and defining triggers and bindings.</span></span>
