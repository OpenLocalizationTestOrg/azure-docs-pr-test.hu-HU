---
title: "az Azure blob storage-összekötőt a Logic Apps a aaaAdd hello |} Microsoft Docs"
description: "Első lépések és a logikai alkalmazás hello Azure blob storage-összekötő konfigurálása"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.openlocfilehash: add61287ef1b2228ef9d3f54ce082807bad6858b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-blob-storage-connector-in-a-logic-app"></a>A logikai alkalmazás hello Azure blob storage-összekötő használatára
Használjon hello Azure Blob storage összekötő tooupload, frissítése, beolvasása, és törölje a blobot, amely a tárfiókon belül logikai alkalmazás.  

Az Azure blob storage szolgáltatással meg:

* A munkafolyamat új projektek feltölteni, vagy nemrég frissített fájlok első hozhat létre.
* Műveletek tooget fájl metaadatok, és törölje a fájlt, a fájlok másolása és további. Például frissítésekor egy eszközt az Azure webhelyén (trigger), majd frissítse a fájlt a blob Storage tárolóban (a műveletet). 

Ez a témakör bemutatja, hogyan toouse hello blob storage-összekötőt a logikai alkalmazás.

További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

További információk a Logic Apps toolearn lásd [Mik azok a logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) és [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-tooazure-blob-storage"></a>Csatlakozás tooAzure blob-tároló
A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először létre kell hoznia egy *kapcsolat* toohello szolgáltatás. Egy kapcsolat egy logikai alkalmazást és egy másik szolgáltatás közötti kapcsolatot biztosít. Például tooconnect tooa tárfiókot, először létre kell hoznia egy blob-tároló *kapcsolat*. toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatokkal kell általában tooaccess hello szolgáltatáshoz való kapcsolódás esetén. Ezért az Azure storage hello hitelesítő adatok tooyour fiók toocreate hello tárolókapcsolat meg. 

#### <a name="create-hello-connection"></a>Hello kapcsolat létrehozása
> [!INCLUDE [Create a connection tooAzure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Eseményindítók
Ez az összekötő nem rendelkezik a eseményindítókat. Használjon más eseményindítók toostart hello logikai alkalmazás, például az ismétlődési eseményindítót, egy HTTP Webhook eseményindító, eseményindítók érhető el a többi összekötőt, és több. [Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md) példaként szolgál.

## <a name="use-an-action"></a>Egy művelettel
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során.

1. Válassza ki a hello plusz jel. Több beállítások megtekintéséhez: **művelet hozzáadása**, **feltétel hozzáadása**, vagy egy hello **további** beállítások.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Válasszon **művelet hozzáadása**.
3. Hello szövegmezőbe írja be a "blob" tooget összes hello elérhető műveletek listáját.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. Válassza ki a fenti példában **AzureBlob - elérési út használatával Get fájlmetaadata**. Ha már létezik egy kapcsolat, majd válassza ki hello **...** (Objektumválasztó megjelenítése) gomb tooselect egy fájlt.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Ha hello kapcsolati adatokat kéri, adja meg hello részletek toocreate hello kapcsolat. [Hello kapcsolat létrehozása](connectors-create-api-azureblobstorage.md#create-the-connection) a témakörben ismertetett ezeket a tulajdonságokat. 
   
   > [!NOTE]
   > Ebben a példában azt lekérése hello metaadat-fájl. toosee hello metaadatok, egy másik művelet, amely létrehoz egy új fájlt egy másik összekötővel hozzáadása. Adja hozzá például a onedrive vállalati verzió művelet, amely létrehoz egy új "teszt" hello metaadatok alapján. 


5. **Mentés** a módosításokat (bal felső sarkában hello eszköztár). A Logic Apps alkalmazást menti, és lehet, hogy automatikusan engedélyezve.

> [!TIP]
> [A Tártallózó](http://storageexplorer.com/) egy remek eszköz túl kezelése több tárfiókot.

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/azureblobconnector/). 

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).

