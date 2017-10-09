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
# <a name="azure-functions-sendgrid-bindings"></a>Az Azure Functions SendGrid kötések

Ez a cikk azt ismerteti, hogyan tooconfigure és az Azure Functions SendGrid kötések munkahelyi. A SendGrid programozott módon használhatja az Azure Functions toosend testreszabott e-mail.

Ez a cikk az Azure Functions fejlesztői útmutatót. Ha új tooAzure funkciók, indítsa el a következő erőforrások hello:

[Az első Azure-függvény létrehozása](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), vagy [csomópont](functions-reference-node.md) fejlesztői hivatkozik.

## <a name="functionjson-for-sendgrid-bindings"></a>a SendGrid kötések Function.JSON

Az Azure Functions biztosít SendGrid egy kimeneti kötése. a kimeneti hello SendGrid kötés lehetővé teszi a toocreate, majd küldje el e-mailek programozott módon. 

hello SendGrid kötés támogatja-e hello következő tulajdonságai:

- `name`: Szükséges – hello hello kérelemmel vagy a kérelem törzsében függvény a kódban használt változó neve. Ez az érték ```$return``` Ha csak egy visszatérési értéket. 
- `type`: Szükséges – túl lehet beállított "sendGrid."
- `direction`: Szükséges – kell állítható túl "lejárt."
- `apiKey`: Szükséges – toohello neve a API-kulcs hello függvény App Alkalmazásbeállítások tárolva kell lennie.
- `to`: hello címzett e-mail címét.
- `from`: hello feladó e-mail címe.
- `subject`: hello hello e-mail tárgya.
- `text`: e-mailek tartalmának hello.

Példa **function.json**:

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
> Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal. Ez a művelet megvédi a bizalmas adatokat.
>
>

## <a name="c-example-of-hello-sendgrid-output-binding"></a>C# – példa hello SendGrid kimeneti kötése

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

## <a name="node-example-of-hello-sendgrid-output-binding"></a>Hello csomópont példa SendGrid kimeneti kötése

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

## <a name="next-steps"></a>Következő lépések
Az Azure Functions más kötései és eseményindítók kapcsolatos információkért lásd: 
- [Az Azure Functions eseményindítók és kötések fejlesztői segédanyagai](functions-triggers-bindings.md)

- [Gyakorlati tanácsok az Azure Functions](functions-best-practices.md) néhány gyakorlati tanácsok toouse sorolja fel, az Azure Functions létrehozásakor.

- [Az Azure Functions fejlesztői segédanyagai](functions-reference.md) programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.
