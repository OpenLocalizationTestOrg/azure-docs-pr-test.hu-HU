---
title: "aaaAdd hello OneDrive-összekötőt a Logic Apps a |} Microsoft Docs"
description: "Hello OneDrive összekötő REST API-paraméterekkel rendelkező áttekintése"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 8303794bb3c2844de288f87f40639abb84c160fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-onedrive-connector"></a>Hello OneDrive összekötő az első lépései
Csatlakozás tooOneDrive toomanage a fájlokat, például az feltöltési, get, törölje a fájlokat. 

A onedrive-ról hogy: 

* A munkafolyamat létrehozása tárolja a fájlokat a onedrive-on, vagy frissítse a meglévő fájlokat a onedrive-on. 
* Felhasználhatja a eseményindítók toostart a munkafolyamat egy fájl létrehozásakor vagy frissítésekor a onedrive-on belül.
* Műveletek toocreate fájlt használja, és törölje a fájlt, és több. Például egy új Office 365 e-mailt fogadásakor mellékletet (trigger) hozzon létre egy új fájlt a onedrive-on (művelet).

Ez a témakör bemutatja, hogyan toouse hello OneDrive-összekötőt a logikai alkalmazás, és is listák hello eseményindítók és műveletek.

További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooonedrive"></a>Csatlakozás tooOneDrive
A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás. Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít. Például a tooconnect tooOneDrive, először a onedrive vállalati verzió *kapcsolat*. toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatok általában használni kívánja a tooconnect tooaccess hello szolgáltatást. Igen a onedrive-on, adja meg a hello hitelesítő adatok tooyour OneDrive fiók toocreate hello kapcsolat.

### <a name="create-hello-connection"></a>Hello kapcsolat létrehozása
> [!INCLUDE [Steps toocreate a connection tooOneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Eseményindítók
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. Eseményindítók "lekérdezésére" hello szolgáltatást egy intervallum és a kívánt gyakoriságát. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. A hello logikai alkalmazás írja be a "onedrive" tooget hello eseményindítók listáját:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Válassza ki **fájl módosításának**. Ha már létezik egy kapcsolat, majd válassza ki a hello objektumválasztó megjelenítése gomb tooselect egy mappát.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Ha a kért toosign, majd adja meg az hello bejelentkezési részletek toocreate hello kapcsolat. [Hello kapcsolat létrehozása](connectors-create-api-onedrive.md#create-the-connection) ebben a témakörben hello lépéseket tartalmazza. 
   
   > [!NOTE]
   > Ebben a példában hello logikai alkalmazás fut, amikor egy fájlt úgy dönt, hogy a rendszer frissíti hello mappában. Ehhez az eseményindítóhoz toosee hello eredményeit hozzáadása, amely egy e-mailt küld egy másik művelet. Adja hozzá például az Office 365 Outlook hello *egy e-mailek küldése* , hogy e-mailt küld, amikor egy fájl frissítése műveletet. 

3. Jelölje be hello **szerkesztése** gombra, majd hello **gyakoriság** és **időköz** értékeket. Például, ha azt szeretné, hello eseményindító toopoll 15 percenként, majd állítsa be hello **gyakoriság** túl**perc**, és a set hello **időköz** túl**15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Mentés** a módosításokat (bal felső sarkában hello eszköztár). A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.

## <a name="use-an-action"></a>Egy művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Válassza ki a hello plusz jel. Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Válasszon **művelet hozzáadása**.
3. Hello szövegmezőbe írja be a "onedrive" tooget összes hello elérhető műveletek listájának.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. Válassza ki a fenti példában **személyes OneDrive - fájl létrehozása**. Ha már létezik egy kapcsolat, majd válassza ki hello **mappa elérési útja** tooput hello hello adja meg **Fájlnév**, és válassza ki a hello **fájl tartalma** kívánt:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat. [Hello kapcsolat létrehozása](connectors-create-api-onedrive.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat. 
   
   > [!NOTE]
   > Ebben a példában azt hozzon létre egy új fájlt a onedrive vállalati verzió mappában. Kimeneti fájlból egy másik eseményindító toocreate hello onedrive vállalati verzió is használhatja. Adja hozzá például az Office 365 Outlook hello *egy új e-mailt fogadásakor* eseményindító. Majd adja hozzá a onedrive vállalati verzió hello *létrehozás fájl* művelet hello mellékletek és a Content-Type mezők használó ForEach toocreate hello új fájlt a onedrive-on belül. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Mentés** a módosításokat (bal felső sarkában hello eszköztár). A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.


## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).
