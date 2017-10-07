---
title: "az Azure Logic Apps különféle műveletek aaaSwitch nyilatkozata |} Microsoft Docs"
description: "Adja meg a logic apps kifejezés értékek alapján a switch utasítás használatával különféle műveletek tooperform"
services: logic-apps
keywords: "Switch utasítás"
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: 09ed7e4a752003aba157e9156bf4dc89ef86f5ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>A logic apps kapcsoló utasítással különböző műveleteket

Amikor egy munkafolyamat szerzői, gyakran előfordul, hogy tootake különféle műveletek objektum vagy kifejezés hello érték alapján. Érdemes például a logic app toobehave másképp hello állapotkód a HTTP-lekérdezés alapján, vagy egy e-mailben kiválasztott lehetőséget.

Használhatja a switch utasítás tooimplement forgatókönyvekben. A Logic Apps alkalmazást egy token vagy kifejezés kiértékelése, és válasszon hello esetet, amelyben hello azonos érték tooexecute hello megadott műveleteket. Csak egyetlen esetet meg kell felelnie hello switch utasításban.

> [!TIP]
> Minden programozási nyelv, például a switch utasítás csak egyenlőségi operátorok támogatja. Ha más relációs operátorok kell például a "nagyobb, mint", az állapot utasítás használható.
> tooensure determinisztikus végrehajtási viselkedésének, esetekben dinamikus jogkivonatok vagy kifejezés helyett egyedi és statikus értéket kell tartalmaznia.

## <a name="prerequisites"></a>Előfeltételek

- Aktív Azure-előfizetés. Ha nincs egy aktív Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/), vagy próbálja [logikai alkalmazások szabad](https://tryappservice.azure.com/).
- [A logic apps alapszintű ismerete](logic-apps-what-are-logic-apps.md)

## <a name="add-a-switch-statement-tooyour-workflow"></a>A kapcsoló utasítás tooyour munkafolyamat hozzáadása

a switch utasítás működése tooshow, ebben a példában, hogy a figyelők fájlok feltöltése tooDropbox logikai alkalmazás hoz létre. Ha hello új fájlok feltöltése után hello logikai alkalmazás küld-e e-mailek tooan jóváhagyó úgy dönt, hogy tootransfer ezen fájlok tooSharePoint. hello alkalmazása használja-e a switch utasítás hajtja végre különböző műveleteket hello érték alapján hello jóváhagyó választja ki.

1. Logikai alkalmazás létrehozása, és válassza ki az ehhez az eseményindítóhoz: **Dropbox - fájl létrehozásakor**.

   ![Használja a Dropbox -, ha egy fájl jön létre eseményindító](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. Hello eseményindító, adja hozzá ezt a műveletet: **Outlook.com - jóváhagyási e-mail küldése**

   > [!TIP]
   > A Logic apps egy Office 365 Outlook-fiókot küldő jóváhagyási e-mail lehetőségeket is támogatja.

   - Ha nincs meglévő kapcsolat, megkéri toocreate egyet.
   - Hello kötelező mezők kitöltésével. Például az **való**, adja meg az üdvözlő e-mail címet hello jóváhagyó e-mailek küldéséhez.
   - A **felhasználói beállítások**, adja meg `Approve, Reject`.

   ![Kapcsolat beállítása](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Adjon hozzá egy kapcsoló utasítást.

   - Válassza ki **+ új lépés** > **... További** > **kapcsoló eset hozzáadása**. 
   - Most azt szeretnénk, ha tooselect hello művelet tooperform hello alapján `SelectedOptions` hello kimenetét *jóváhagyása e-mailek küldése* művelet. 
   Ez a mező található hello **dinamikus tartalom hozzáadása az** választó.
   - Használjon *1. eset* toohandle, amikor kijelöli hello jóváhagyó `Approve`.
     - Jóváhagyás esetén másolja hello eredeti fájl tooSharePoint Online rendelkező hello [ **SharePoint Online - fájl létrehozása** művelet](../connectors/connectors-create-api-sharepointonline.md).
     - Adja hozzá a hello eset toonotify felhasználókat, hogy egy új fájl áll rendelkezésre a SharePoint-webhelyen belül egy másik művelet.
   - Adjon hozzá egy másik eset toohandle, ha a felhasználó kijelöli `Reject`.
     - Ha vissza kell utasítani, hogy hello fájl elutasítva, és nincs további teendője, amely arról tájékoztatja, hogy más jóváhagyóknak értesítő e-mail küldése.
   - `SelectedOptions`csak két lehetőséget biztosít, így azt is hagyhatja hello **alapértelmezett** esetben üres.

   ![Switch utasítás](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > A switch utasítás kell legalább egy eset hozzáadása toohello alapértelmezett esetben.

4. Hello switch utasításban, követően törölje ezt a műveletet hello eredeti feltöltött fájl tooDropbox: **Dropbox - fájl törlése**

5. Mentse a Logic Apps alkalmazást. Az alkalmazás tesztelése a fájl tooDropbox feltöltésével. Hamarosan jóváhagyása e-mailt kell kapnia. Jelöljön ki egy lehetőséget, és tekintse meg az hello viselkedését.

   > [!TIP]
   > Ellenőrizze, hogyan túl[logikai alkalmazások figyelése](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-hello-code-behind-switch-statements"></a>Hello háttérkódot kapcsoló utasítások megismeréséhez

Most, hogy sikeresen létrehozta a logikai alkalmazás egy switch utasítás használatával, nézzük hello kód megadása mögött hello switch utasításban.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* `"Switch"`átnevezheti az olvashatóság hello switch utasítás hello neve van. 
* `"type": "Switch"`azt jelzi, hogy hello művelet egy switch utasításban. 
* `"expression"`Ebben a példában a hello jóváhagyó kiválasztott beállítás, és minden esetben később hello definition deklarált képest értékeli ki. 
* `"cases"`tetszőleges számú esetek is tartalmazhat. A minden esetben `"Case *"` hello alapértelmezett név az olvashatóság átnevezheti hello eset. 
`"case"`Megadja a hello eset címke, amely hello kapcsoló kifejezést használ az összehasonlításhoz, és állandó és egyedi értéknek kell lennie. Ha nincs hello esetekben hello kapcsoló kifejezésnek a feltétel teljesüléséhez, a műveletek `"default"` hajtja végre a rendszer.

## <a name="get-help"></a>Segítségkérés

tooask kérdéseket, válasz kérdések és milyen egyéb Azure Logic Apps felhasználók végzi, tekintse meg a Microsoft hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan túl[feltételek hozzáadása](logic-apps-use-logic-app-features.md)
- További tudnivalók [hiba és kivételkezelést](logic-apps-exception-handling.md)
- Fedezze fel több [munkafolyamat nyelvi képességek](logic-apps-author-definitions.md)