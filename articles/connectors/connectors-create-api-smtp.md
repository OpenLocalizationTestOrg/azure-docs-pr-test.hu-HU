---
title: "az Azure Logic Apps aaaSMTP összekötő |} Microsoft Docs"
description: "Az Azure App service logic Apps alkalmazások létrehozása Csatlakozás tooSMTP toosend e-mailt."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 36bb836851014d24f2e069fda8376ad7a08c943b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-smtp-connector"></a>Hello SMTP-összekötő az első lépései
Csatlakozás tooSMTP toosend e-mailt.

toouse [a csatlakozókat](apis-list.md), először toocreate logikai alkalmazás. Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-toosmtp"></a>Csatlakozás tooSMTP
A Logic Apps alkalmazást férhetnek hozzá a szolgáltatáshoz, először jóvá kell toocreate egy *kapcsolat* toohello szolgáltatás. A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat. Például a tooconnect tooSMTP, először egy SMTP *kapcsolat*. toocreate a kapcsolat létrehozásakor, adja meg a hello hitelesítő adatokkal kell általában tooaccess hello szolgáltatás csatlakozik. Igen a hello SMTP példában adja meg az hello hitelesítő adatok tooyour kapcsolat neve, a SMTP-kiszolgáló címére és a felhasználói bejelentkezési adatokat toocreate hello kapcsolat tooSMTP.  

### <a name="create-a-connection-toosmtp"></a>Egy kapcsolat tooSMTP létrehozása
> [!INCLUDE [Steps toocreate a connection tooSMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Az SMTP-eseményindító használata
Egy eseményindító nem lehet a logikai alkalmazás definiált használt toostart hello munkafolyamat esemény. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Ebben a példában, mert SMTP nem rendelkezik saját, eseményindító fogjuk használni hello **Salesforce - amikor létrejön egy objektum** eseményindító. Ehhez az eseményindítóhoz akkor aktiválódik, amikor új objektumot hoz létre a Salesforce-ban. A példa kedvéért be azt, hogy minden alkalommal új vezető jön létre a Salesforce, egy *e-mailek küldése* egy értesítés, amely hello új vezető létrehozásra a hello SMTP-összekötőn keresztül valósul meg.

1. Adja meg *salesforce* hello keresőmezőbe hello logic apps designer válassza hello **Salesforce - amikor létrejön egy objektum** eseményindító.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. Hello **egy objektumának létrejöttekor** vezérlő jelenik meg.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. SELECT hello **objektumtípus** válassza *vezethet* hello objektumok listája. Ebben a lépésben vannak jelzi, hogy hoz létre egy eseményindítót, amely értesíti a Logic Apps alkalmazást, amikor egy új vezető jön létre a Salesforce-ban.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. hello eseményindító létrehozása befejeződött.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Az SMTP-művelethez használata
Egy művelet által definiált logikai alkalmazás hello munkafolyamat során. [További információ a műveletek](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Most, hogy hello eseményindító hozzá lett adva, kövesse ezeket, hogy történjen a Salesforce jön létre egy új vezető lépéseket tooadd egy SMTP műveletet.

1. Válassza ki **+ új lépés** tooadd hello művelet szeretné tootake egy új vezető létrehozásakor.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Válassza ki **művelet hozzáadása**. A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Adja meg *smtp* toosearch kapcsolódó tooSMTP műveletek számára.  
4. Válassza ki **SMTP - E-mail küldése** , hello művelet tootake hello új vezető létrehozásakor. Megnyílik a hello művelet blokk. Hogy tooestablish az smtp-kapcsolat hello Tervező blokkban Ha még nem meg korábban.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. Adjon meg a kívánt e-mail adatokkal hello **SMTP - E-mail küldése** blokkot.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. Mentse a munkáját a rendelés tooactivate a munkafolyamat.  

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/smtpconnector/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).
