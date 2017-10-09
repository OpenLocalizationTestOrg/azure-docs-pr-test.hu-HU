---
title: "az Azure Logic Apps aaaDropbox összekötő |} Microsoft Docs"
description: "Az Azure App service Logic Apps alkalmazások létrehozása Csatlakozás tooDropbox toomanage a fájlokat. Hajtsa végre különböző műveleteket végez, például a feltöltés, frissítése, lekérése és Dropbox fájl törlése."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cb0ae033-aba7-4ac9-beaa-be561a0f0cac
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1f307477836104c0bc0008341604a1400860987f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-dropbox-connector"></a>Hello Dropbox összekötő az első lépései
Csatlakozás tooDropbox toomanage a fájlokat. Hajtsa végre különböző műveleteket végez, például a feltöltés, frissítése, lekérése és Dropbox fájl törlése.

toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás. Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toodropbox"></a>Csatlakozás tooDropbox
A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás. Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít. Például a rendelés tooconnect tooDropbox, először a Dropbox *kapcsolat*. a kapcsolat toocreate, kellene tooprovide hello hitelesítő adatok általában használni kívánja a tooconnect tooaccess hello szolgáltatást. Igen hello Dropbox példában kellene hello hitelesítő adatok tooyour rendelés toocreate hello kapcsolat tooDropbox Dropbox-fiókot. [További információ a kapcsolatok száma]()

### <a name="create-a-connection-toodropbox"></a>Egy kapcsolat tooDropbox létrehozása
> [!INCLUDE [Steps toocreate a connection tooDropbox](../../includes/connectors-create-api-dropbox.md)]
> 
> 

## <a name="use-a-dropbox-trigger"></a>A Dropbox eseményindító használata
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

A jelen példában használjuk hello **fájl létrehozásának** eseményindító. Ehhez az eseményindítóhoz akkor fordul elő, ha azt telefonhívásokhoz hello **elérési út használatával fájl tartalmának lekérdezése** Dropbox művelet. 

1. Adja meg *dropbox* a hello Logic Apps designer hello a keresési mezőbe, majd válassza ki hello **Dropbox - fájl létrehozásakor** eseményindító.      
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger.PNG)  
2. Válassza ki a kívánt tootrack fájllétrehozást hello mappát. Válasszon... (piros hello mezőben azonosított) és a Tallózás toohello mappa hello eseményindító tooselect bemeneti kívánja.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-trigger-2.PNG)  

## <a name="use-a-dropbox-action"></a>A Dropbox művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Most, hogy hello eseményindító hozzá lett adva, kövesse ezeket a lépéseket tooadd egy műveletet, amely megkapja a hello új fájl tartalma.

1. Válassza ki **+ új lépés** tooadd hello művelet szeretné tootake amikor egy új fájl jön létre.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action.PNG)
2. Válassza ki **művelet hozzáadása**. A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-2.PNG)
3. Adja meg *dropbox* toosearch kapcsolódó tooDropbox műveletek számára.  
4. Válassza ki **Dropbox - fájl tartalmának lekérése az elérési út használatával** művelet tootake hello, amikor egy új fájl jön létre hello kijelölt a Dropbox mappa. Megnyílik a hello művelet blokk. Akkor lesz felszólító tooauthorize a logic app tooaccess a Dropbox fiókot, ha még nem meg korábban.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-3.PNG)  
5. Válasszon... (hello hello jobb oldalán található **fájl elérési útját** vezérlő), és keresse meg azt szeretné, hogy toouse toohello-fájl elérési útját. Másik lehetőségként hello **fájl elérési útját** token toospeed fel a logikai alkalmazás létrehozása.  
   ![](../../includes/media/connectors-create-api-dropbox/using-dropbox-action-4.PNG)  
6. Mentse a munkáját, és hozzon létre egy új fájlt a Dropbox tooactivate a munkafolyamat.  

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/dropbox/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).
