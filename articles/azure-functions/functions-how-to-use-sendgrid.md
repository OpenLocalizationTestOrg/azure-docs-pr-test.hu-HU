---
title: az Azure Functions SendGrid aaaHow toouse |} Microsoft Docs
description: Bemutatja, hogyan toouse az Azure Functions SendGrid
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
ms.openlocfilehash: a0ffdae04e5924c773d2d26427626fc1f570f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-sendgrid-in-azure-functions"></a>Hogyan toouse az Azure Functions SendGrid

## <a name="sendgrid-overview"></a>SendGrid áttekintése

Az Azure Functions támogatja SendGrid kimeneti kötések tooenable a funkciók toosend e-mail üzenetek néhány sornyi kódot és egy SendGrid-fiókot.

toouse hello SendGrid API-nak az Azure-függvény, rendelkeznie kell egy [SendGrid fiók](http://SendGrid.com). Emellett rendelkeznie kell egy SendGrid API-kulcsot. Jelentkezzen be tooyour SendGrid fiókot, és kattintson a **beállítások** majd **API-kulcs** toogenerate egy API-kulcs. Tartsa meg a rendelkezésre álló kulcs használata azt egy későbbi lépésben.

Most már áll készen toocreate egy Azure-függvény alkalmazást.

## <a name="create-an-azure-function-app"></a>Azure-függvényalkalmazás létrehozása 

Az Azure functions egy vagy több Azure függvény alkalmazások tárolói. Az Azure functions rendszer éppen ez - függvényt. Minden Azure függvény kapcsolt tooone eseményindító, amely hello függvény toorun okozó esemény.
Minden függvény tartalmazhatnak tetszőleges számú bemeneti vagy kimeneti kötések. Kötések a szolgáltatások, hogy a függvényben használható. SendGrid, mert egy kötés, kimeneti toosend e-mail használhatják. 

1. Jelentkezzen be Azure-portálon toohello és [hozzon létre egy Azure-függvény alkalmazást](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) vagy nyisson meg egy meglévő függvény alkalmazást. 
2. Egy Azure-függvény létrehozása. tookeep az egyszerű, válasszon ki egy kézi indítási és C#. 

 ![Egy Azure-függvény létrehozása](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>Egy Azure-függvény alkalmazása SendGrid konfigurálása

A SendGrid API-kulcsot kell tárolnia, egy alkalmazás-beállítás toouse azt a függvényben. hello ApiKey mező nem a tényleges SendGrid API-kulcsot, de egy alkalmazás, beállítás határozza meg, amely a tényleges API-kulcs jelöli. A biztonság érdekében a kulcs tárolása így azért, mert a kódok vagy a fájlokat, előfordulhat, hogy ellenőrizni kell a verziókövetési elkülönül.

- Hozzon létre egy **AppSettings** kulcs az függvény alkalmazás **Alkalmazásbeállítások**.

 ![Egy Azure-függvény létrehozása](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>Konfigurálja a SendGrid kimeneti kötések

SendGrid áll rendelkezésre, mert az Azure függvény kimeneti kötése. a SendGrid toocreate kimeneti kötése:

1. Nyissa meg toohello **integráció** lapon hello függvény hello Azure-portálon.
2. Kattintson a **új kimeneti** toocreate a SendGrid kimeneti kötése.
3. Adja meg a hello **API-kulcs** és **üzenet paraméternév** tulajdonságok. Ha azt szeretné, akkor adhat meg egyéb tulajdonságok most hello, vagy inkább code őket. Ezek a beállítások alapértelmezett értéke használható.

 ![Konfigurálja a SendGrid kimeneti kötések](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

A kötés tooa függvény hozzáadása létrehoz egy nevű **function.json** a függvény mappában. Ez a fájl tartalmazza az összes ugyanazokat az információkat, hogy a látni hello Azure függvény hello **integráció** lapon, de a Json formátumban. A beállítás hello **ApiKey**, **üzenet**, és **a** mezők létrehozása a következő bejegyzést hello hello **function.json** fájl: 

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

Tetszés szerint módosíthatja a fájlt közvetlenül a saját maga.

Most, hogy létrehozta és hello függvény App és függvény konfigurálva, egy e-mailt hello kód toosend lehet írni.

## <a name="write-code-that-creates-and-sends-email"></a>Kód írása, amely létrehoz és e-mailt küld

hello SendGrid API tartalmazza az összes hello parancsok toocreate kell, és az e-mailek küldése.  

- Hello kód hello függvényében cserélje le a következő kód hello:

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
    // change tooemail of recipient
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

Értesítés hello első sor tartalmaz hello ```#r``` direktíva, amely hello SendGrid szerelvényre hivatkozik. Ezután használhatja a ```using``` utasítás toomore egyszerűen hozzáférhessenek a névtér hello objektumok. Hello kódot, hozzon létre példányai ```Mail```, ```Personalization```, és ```Content``` hello SendGrid API hello e-mail alkotó objektumokat. Amikor visszatér üdvözlőüzenetére, a SendGrid nyújt. 

hello függvény aláírás is tartalmaz egy plusz típusú paraméter out ```Mail``` nevű ```message```. Mindkét bemeneti és kimeneti kötések express maguk függvény paramétereiben kódot. 

2. Gombra kattintva tesztelheti a kódját **teszt** , majd gépelje be az üzenetet a hello **Request body** mező, majd kattintson a hello **futtatása** gombra.

 ![Tesztelheti a kódját](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Ellenőrizze, hogy a SendGrid hello e-mailt küldeni e-mailek tooverify. Azt kell toohello cím hello kódban nyissa meg az 1. lépés, és tartalmazzák a hello üdvözlőüzenetére **Request body**.

## <a name="next-steps"></a>Következő lépések
Ez a cikk bemutatta, hogyan toouse SendGrid szolgáltatás toocreate hello és e-mailek küldése. További információ az Azure Functions használatával az alkalmazásokban lévő toolearn lásd: a következő témakörök hello: 

- [Gyakorlati tanácsok az Azure Functions](functions-best-practices.md) néhány gyakorlati tanácsok toouse sorolja fel, az Azure Functions létrehozásakor.

- [Az Azure Functions fejlesztői segédanyagai](functions-reference.md) programozói segédanyagok függvények kódolásához, valamint eseményindítók és kötések meghatározásához.

- [Az Azure Functions tesztelése](functions-test-a-function.md) különböző eszközöket és technikákat a funkciók ismerteti.