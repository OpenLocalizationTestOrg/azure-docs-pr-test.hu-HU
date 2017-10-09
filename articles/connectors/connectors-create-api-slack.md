---
title: "az Azure logic Apps alkalmazásait a Slackhez összekötő aaaUse hello |} Microsoft Docs"
description: "Csatlakozás a logic apps a tooSlack"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 6599d7b69d2147425c9fab978c5d0f93e5605f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-slack-connector"></a>Hello közzététele a Slack-összekötő az első lépései
A Slack egy csoportos kommunikációs eszköz, amely a csoporton belüli összes kommunikációt egy helyre fogja össze. Ez a hely azonnal kereshető, és bárhonnan elérhető. 

Első lépések egy logikai alkalmazás létrehozása most; Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-tooslack"></a>Egy kapcsolat tooSlack létrehozása
toouse hello Slack összekötő, először létre kell hoznia egy **kapcsolat** hello részletek adja meg ezeket a tulajdonságokat: 

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| Token |Igen |Adja meg a Slack hitelesítő adatok |

Hajtsa végre ezeket a lépéseket toosign Slackhez, és a teljes hello konfigurálása hello Slackhez **kapcsolat** a logikai alkalmazásban:

1. Válassza ki **ismétlődési**
2. Válassza ki a **gyakoriság** , és írja be egy **időköz**
3. Válassza ki **művelet hozzáadása**  
   ![Konfigurálja a Slackhez][1]  
4. Hello keresőmezőbe írja be a Slackhez, majd várja meg a keresési tooreturn hello összes bejegyzést a Slackhez hello neve
5. Válassza ki **Slackhez - üzenet közzététele**
6. Válassza ki **tooSlack bejelentkezés**:  
   ![Konfigurálja a Slackhez][2]
7. Adja meg a Slack hitelesítő adatok toosign tooauthorize hello alkalmazásban    
   ![Konfigurálja a Slackhez][3]  
8. Átirányított tooyour szervezete bejelentkezési oldalán lesz. **Engedélyezi** az a logikai alkalmazás közzététele a Slack toointeract:      
   ![Konfigurálja a Slackhez][5] 
9. Hello engedélyezési befejezése után lesz átirányított tooyour logic app toocomplete azt hello konfigurálásával **Slack - összes üzenet** szakasz. Vegyen fel más eseményindítók és műveletek, amelyekre szüksége van.  
   ![Konfigurálja a Slackhez][6]
10. Mentse a munkáját kiválasztásával **mentése** fenti hello menüsávjában.

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/slack/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
