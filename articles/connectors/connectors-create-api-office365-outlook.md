---
title: "aaaAdd hello Office 365 Outlook-összekötőt a Logic Apps a |} Microsoft Docs"
description: "Hozzon létre a logic apps és az Office 365 Office 365 összekötő tooenable beavatkozást igényel. Például: létrehozása, szerkesztése és a partnerek és a naptári elemek frissítése."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 86a573c9c54701de3d3f0500d19eaf545e0710ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-office-365-outlook-connector"></a>Hello Office 365 Outlook összekötő az első lépései
hello Office 365 Outlook összekötő lehetővé teszi, hogy az Office 365 Outlook való együttműködéshez. Az összekötő toocreate, a Szerkesztés, és a frissítés névjegyeket és a naptári elemek használata is beolvasása, küldése és tooemail válasz.

Az Office 365 Outlook meg:

* A munkafolyamat belül Office 365 e-mailek és a naptári funkciók hello használatához felépítéséhez. 
* Használja toostart váltja ki, a munkafolyamatot, ha egy új e-mailt, a naptárelemek frissítésekor és még sok más.
* Műveletek toosend egy e-mailt használja, hozzon létre új naptár esemény, és több. Például, ha van egy új objektumot a Salesforce (trigger), küldjön egy e-mailt tooyour Office 365 Outlook (művelet). 

Ez a témakör bemutatja, hogyan toouse hello Office 365 Outlook-összekötőt a logikai alkalmazás, és is listák hello eseményindítók és műveletek.

> [!NOTE]
> Hello cikk e verziója tooLogic alkalmazások általános elérhetőségével (GA) vonatkozik.
> 
> 

További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toooffice-365"></a>Csatlakozás tooOffice 365
A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás. Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít. Például tooconnect tooOffice 365 Outlook, először van szüksége az Office 365 *kapcsolat*. toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatok általában használni kívánja a tooconnect tooaccess hello szolgáltatást. Ezért az Office 365 Outlook, adja meg a hello hitelesítő adatok tooyour Office 365 fiók toocreate hello kapcsolat.

## <a name="create-hello-connection"></a>Hello kapcsolat létrehozása
> [!INCLUDE [Steps toocreate a connection tooOffice 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Eseményindítók
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. Eseményindítók "lekérdezésére" hello szolgáltatást egy intervallum és a kívánt gyakoriságát. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. A hello logikai alkalmazás írja be a "office 365" tooget hello eseményindítók listáját:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Válassza ki **Office 365 Outlook - jövőbeli esemény hamarosan indításakor**. Ha már létezik egy kapcsolat, majd válassza ki a naptárban hello legördülő listában.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Ha a kért toosign, majd adja meg az hello bejelentkezési részletek toocreate hello kapcsolat. [Hello kapcsolat létrehozása](connectors-create-api-office365-outlook.md#create-the-connection) ebben a témakörben hello lépéseket tartalmazza. 
   
   > [!NOTE]
   > Ebben a példában hello logikai alkalmazás fut, amikor a naptár esemény frissül. Ehhez az eseményindítóhoz toosee hello eredményeit hozzáadása, amely egy szöveges üzenetet küld Önnek egy másik művelet. Adja hozzá például hello Twilio *üzenet küldése* 15 perc múlva indul, amikor hello naptár esemény szövegek műveletet. 
   > 
   > 
3. Jelölje be hello **szerkesztése** gombra, majd hello **gyakoriság** és **időköz** értékeket. Például, ha azt szeretné, hello eseményindító toopoll 15 percenként, majd állítsa be hello **gyakoriság** túl**perc**, és a set hello **időköz** túl**15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Mentés** a módosításokat (bal felső sarkában hello eszköztár). A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.

## <a name="use-an-action"></a>Egy művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Válassza ki a hello plusz jel. Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Válasszon **művelet hozzáadása**.
3. Hello szövegmezőbe írja be a "office 365" tooget összes hello elérhető műveletek listáját.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. Válassza ki a fenti példában **Office 365 Outlook - ügyfél létrehozása**. Ha már létezik egy kapcsolat, majd válassza a hello **mappa azonosítója**, **Utónév**, és egyéb tulajdonságok:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat. [Hello kapcsolat létrehozása](connectors-create-api-office365-outlook.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat. 
   
   > [!NOTE]
   > Ebben a példában az Office 365 Outlook új ügyfél tudjuk létrehozni. Egy másik eseményindító toocreate hello forduljon kimenete is használhatja. Adja hozzá például a hello SalesForce *egy objektumának létrejöttekor* eseményindító. Adja meg az Office 365 Outlook hello *ügyfél létrehozása* hello SalesForce használó művelet mezők toocreate hello új új lépjen kapcsolatba Office 365-ben. 
   > 
   > 
5. **Mentés** a módosításokat (bal felső sarkában hello eszköztár). A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/office365connector/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).

