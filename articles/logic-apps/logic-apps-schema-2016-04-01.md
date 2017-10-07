---
title: "frissítések 2016. június-1-- Azure Logic Apps aaaSchema |} Microsoft Docs"
description: "Az Azure Logic Apps JSON-meghatározások létrehozása séma 2016-06-01-es verziójával"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/25/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: b0347fbbd692a93b63a2f8b741402a225450b35a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Az Azure Logic Apps – 2016. június 1 sémafrissítések

Az új séma- és API-t az Azure Logic Apps verzió magában foglalja a fontos fejlesztést tartalmaz, amelyek a logic apps több megbízható és egyszerűbb toouse:

* [Hatókörök](#scopes) lehetővé teszik, hogy a csoport- vagy beágyazni műveletek műveletek gyűjteményeként.
* [Feltételek és a hurkok](#conditions-loops) első osztályú műveletek is.
* Pontosabb rendezést műveletek futtatása a hello `runAfter` tulajdonság, cseréje`dependsOn`

az a logic apps 2015. augusztus 1 hello tooupgrade előzetes séma toohello 2016. június 1 séma [tekintse meg a verziófrissítés szakasz hello](##upgrade-your-schema).

<a name="scopes"></a>
## <a name="scopes"></a>Hatókörök

A séma tartalmaz a hatókörök, amelyek lehetővé teszik, hogy együtt műveleteit, vagy a nest műveletek belül egymással. Egy feltétel tartalmazhat például egy másik feltétel. További információ [szintaxis hatókör](../logic-apps/logic-apps-loops-and-scopes.md), vagy tekintse át a alapvető hatókör példa:

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>
## <a name="conditions-and-loops-changes"></a>Feltételek és a hurkok módosítása

Előző séma verziója, a feltételek és a hurkok voltak egyetlen művelettel társított paraméter. Ebben a sémában liftek ezt a korlátozást, így a feltételek és a hurkok megjelenni művelettípusok. További információ [hurkok és hatókörök](../logic-apps/logic-apps-loops-and-scopes.md), vagy tekintse át az egyszerű példában feltétel művelethez:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>
## <a name="runafter-property"></a>"runAfter" tulajdonság

Hello `runAfter` tulajdonság cserél `dependsOn`, nyújtó pontosabb futtatása hello ahhoz, hogy a műveletek megadásakor korábbi műveletek hello állapota alapján.

Hello `dependsOn` tulajdonságának azonos a "hello művelet futtatott és sikeres volt-e" volt, függetlenül attól, hogy hányszor tooexecute művelet, hogy hello előző művelet sikeres volt, alapján nem sikerült, vagy kihagytuk kívánta. Hello `runAfter` tulajdonság olyan objektum, amely megadja az összes hello művelet nevek elteltével hello objektum fut, hogy rugalmasságot biztosít. Ez a tulajdonság is megadja elfogadható eseményindítóként használható állapotait tömbjét. Például ha toorun után lépést A sikeres és is után B lépésben sikeres vagy sikertelen lesz, hozhat létre a `runAfter` tulajdonság:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>A séma frissítése

Toohello frissítése új séma mindössze néhány lépést. hello frissítési folyamat tartalmaz hello frissítési parancsprogram, futtatása mentése új logikai alkalmazás, és ha azt szeretné, valószínűleg felülírja a hello előző logikai alkalmazást.

1. Hello Azure-portálon nyissa meg a Logic Apps alkalmazást.

2. Nyissa meg túl**áttekintése**. Hello logic app eszköztáron válassza **frissítés séma**.
   
    ![Válassza ki a séma frissítése][1]
   
    hello frissített definíciós ad vissza, amelyet másolja és illessze be az erőforrás-definíció szükség esetén. 
    Azonban azt **erősen ajánlott** választja **Mentés másként** toomake meg arról, hogy az összes kapcsolat hivatkozások érvényes hello a logikai alkalmazás frissítése.

3. Hello frissítési panel eszköztárán válassza **Mentés másként**.

4. Adja meg a hello logika nevét és állapotát. toodeploy a frissített Logic Apps alkalmazást, válassza a **létrehozása**.

5. Győződjön meg arról, hogy a frissített logikai alkalmazás megfelelően működik-e.
   
   > [!NOTE]
   > Ha manuális vagy kérelem eseményindítót használ, hello visszahívási URL-címet az új logikai alkalmazás változik. Teszt hello új URL-cím toomake meg arról, hogy hello-végpont működik tapasztalhat. toopreserve előző URL-címek, klónozhat keresztül a meglévő Logic Apps alkalmazást.

6. *Nem kötelező* toooverwrite a korábbi Logic Apps alkalmazást hello új séma verzióra, a hello eszköztárán válassza **Klónozás**, tovább túl**frissítés séma**. Ez a lépés nem szükséges csak akkor, ha azt szeretné, tookeep hello azonos erőforrás azonosítója, vagy kérjen indítási URL-CÍMÉT a Logic Apps alkalmazást.

### <a name="upgrade-tool-notes"></a>Frissítési eszköz megjegyzések

#### <a name="mapping-conditions"></a>Leképezési feltételek

Hello frissített definícióban hello eszköz lehetővé teszi egy, a true és false fiókirodai műveletek csoportosítása hatóköreként a lehető legkedvezőbb módon. Pontosabban hello Tervező mintáját `@equals(actions('a').status, 'Skipped')` meg kell jelennie egy `else` művelet. Azonban hello eszköz felismerhetetlen minták észleli, ha hello eszköz előfordulhat, hogy hozzon létre külön feltételek hello IGAZ és hamis ág hello. Műveletek leképezheti a frissítés után is, ha szükséges.

#### <a name="foreach-loop-with-condition"></a>feltételt tartalmazó "foreach" hurok

Hello új sémában, használhatja a hello szűrő művelet tooreplicate hello mintáját a `foreach` elemenként, eltérő feltételt tartalmazó hurok, de ezt a változtatást automatikusan történjen, amikor frissít. hello feltétel előtt hello foreach hurok hello feltételének elemek csak egy tömb visszaküldésére használatos szűrőművelet válik, és, hogy tömb átad a hello foreach művelet. Egy vonatkozó példáért lásd: [hurkok és hatókörök](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Az erőforráscímkék

A frissítés befejezése után az erőforráscímkék eltávolítja, így kell visszaállítani őket frissített hello munkafolyamat.

## <a name="other-changes"></a>Az egyéb módosítások

### <a name="renamed-manual-trigger-toorequest-trigger"></a>Átnevezett "manual" eseményindító too'request "eseményindító

Hello `manual` indítási típus elavult, és a neve túl`request` típusú `http`. Ez a módosítás több konzisztencia hoz létre, a mintát, amely eseményindító hello hello típusú használt toobuild.

### <a name="new-filter-action"></a>Új "filter" művelet

egy nagy tömböt le meg tooa kevesebb elemek, új hello toofilter `filter` típusú tömb és egy feltétel fogad, hello feltétel minden elemhez kiértékeli, és hello feltétel értekezlet elemekhez tömböt ad vissza.

### <a name="restrictions-for-foreach-and-until-actions"></a>A "foreach" és "csak" műveletek korlátozása

Hello `foreach` és `until` hurok korlátozott tooa egyetlen művelettel vannak.

### <a name="new-trackedproperties-for-actions"></a>Új "trackedProperties" műveletek

Műveletek is most már rendelkezik egy további tulajdonság nevű `trackedProperties`, vagyis testvér toohello `runAfter` és `type` tulajdonságok. Ezen az objektumon bizonyos művelet bemeneti határozza meg, és kiírja, hogy szeretné-e a hello Azure diagnosztikai telemetriai, egy munkafolyamat részeként kibocsátott tooinclude. Példa:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Következő lépések
* [A logic apps munkafolyamat-meghatározások létrehozása](../logic-apps/logic-apps-author-definitions.md)
* [A logic Apps alkalmazások központi telepítési sablonok létrehozása](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
