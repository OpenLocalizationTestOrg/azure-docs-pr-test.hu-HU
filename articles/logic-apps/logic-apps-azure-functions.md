---
title: "az Azure Functions Azure Logic Apps aaaCustom kódját |} Microsoft Docs"
description: "Hozzon létre és futtasson egyéni kód az Azure Logic Apps az Azure Functions"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 18b3821ed7e434feb568b9b96e9a5a2189dba3bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-and-run-custom-code-for-logic-apps-through-azure-functions"></a>Adja hozzá, és egyéni kódot a logic Apps alkalmazásokat futtasson Azure Functions használatával

toorun egyéni kódtöredékek C# vagy node.js a logic apps, az Azure Functions egyéni funkciókhoz is létrehozhat. 
[Az Azure Functions](../azure-functions/functions-overview.md) kínál a kiszolgáló-mentes számítógép-használatról a Microsoft Azure és a feladatok végrehajtásához használt hasznos:

* Speciális formázás vagy számítási a logic apps mezők
* A munkafolyamat számításokat.
* Bővíthetők hello logic app C# vagy node.js által támogatott funkciók

## <a name="create-custom-functions-for-your-logic-apps"></a>A logic Apps alkalmazásokat a felhasználói függvények létrehozása

Azt javasoljuk, hogy hozzon létre egy olyan függvényt hello Azure Functions portálon, hello **általános Webhook - csomópont** vagy **általános Webhook - C#** sablonok. hello eredmény létrehoz egy automatikus feltöltve a sablont, amely elfogadja `application/json` a logikai alkalmazás. Függvények, amelyek ezeket a sablonokat hoz létre a rendszer automatikusan észleli és hello jelennek meg a Logic App Designer **Azure Functions a régióban.**

Az Azure portál, a hello hello **integráció** ablaktábla a függvénynél, a sablon jelenítsen meg, amely **mód** túl beállítása**Webhook** és **Webhook típus** értéke túl**általános JSON**. 

Webhook funkciók fogadja el a kérelmet, és adja át az keresztül hello metódusnak egy `data` változó. Érhető el a tartalom hello tulajdonságok pontjelöléssel például `data.function-name`. Például egy egyszerű JavaScript függvény DateTime típusú értékké alakít át egy dátumkarakterlánc néz ki a következő példa hello:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-logic-apps"></a>Az Azure Functions hívható meg a logic Apps alkalmazásokból

toolist hello tárolókat az előfizetését és select hello függvény, amelyet az toocall Logic App tervezőben kattintson hello **műveletek** menüt, és válassza ki a **Azure Functions régiómban**.

Hello függvény kiválasztása után áll ismételt toospecify bemeneti forgalma objektum. Ez az objektum üdvözlőüzenetére hello logic app küld toohello függvény, és JSON-objektumnak kell lennie. Például, ha azt szeretné, hogy a hello toopass **utolsó módosítás** dátuma a Salesforce-eseményindítóval hello függvény hasznos nézhet ki ebben a példában:

![Utolsó módosítás dátuma][1]

## <a name="trigger-logic-apps-from-a-function"></a>Egy függvény eseményindító logikai alkalmazások

Indíthat el egy függvényen belül egy logikai alkalmazást. Lásd: [a Logic apps hívható végpontként](logic-apps-http-endpoint.md). Kézi indítási rendelkező logikai alkalmazás létrehozása, majd a a függvényen belül készítése egy HTTP POST toohello kézi indítási URL-CÍMÉT a használni kívánt toohello logikai alkalmazás küldött hello tartalom.

### <a name="create-a-function-from-logic-app-designer"></a>A Logic App Designer függvény létrehozása

A node.js webhook függvény hello designer is létrehozhat. Első lépésként válassza ki **régiómban, az Azure Functions** , és válassza ki a függvény tárolója. Ha még nem rendelkezik a tárolóhoz, szükség van-e toocreate a hello [Azure Functions portálon](https://functions.azure.com/signin). Válassza ki **hozzon létre új**.  

egy sablon, amelyet az toocompute, hello adatok alapján toogenerate hello környezeti objektumot toopass tervezi egy függvénynek adja meg. Ez az objektum JSON objektumnak kell lennie. Például ha egy FTP-művelet a teljesíti a hello fájl tartalma a, hello környezetben hasznos a következőképpen néz ebben a példában:

![A környezetben hasznos][2]

> [!NOTE]
> Ez az objektum nem alakítható karakterláncként, mert hello tartalom kerüljön közvetlenül toohello JSON-adattartalmat. Hiba történik, ha hello objektum nem egy JSON-jogkivonatot (Ez azt jelenti, hogy egy karakterlánc- vagy JSON objektum vagy tömb). toocast hello objektum karakterláncként, adjon hozzá idézőjelek között, ebben a cikkben hello első ábrán látható módon.
> 

hello designer akkor is létrehozhat beágyazott függvény sablont hoz létre. Változók előre toopass tervezi hello függvénynek hello környezet alapján hozzák létre.

<!--Image references-->
[1]: ./media/logic-apps-azure-functions/callfunction.png
[2]: ./media/logic-apps-azure-functions/createfunction.png
