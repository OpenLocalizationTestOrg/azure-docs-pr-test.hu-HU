---
title: "SharePoint Server-összekötőt a Logic Apps a aaaUse hello |} Microsoft Docs"
description: "Hello hello SharePoint Server-összekötőt a Logic apps a használatának megkezdése"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 3b814f42611e4971ff5c94ae3b021829217911dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-sharepoint-connector"></a>Hello SharePoint összekötő az első lépései
SharePoint-összekötő hello egy módon toowork listákkal SharePoint biztosít.

Hozzon létre egy logic app; első lépései Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-toosharepoint"></a>Egy kapcsolat tooSharePoint létrehozása
toouse hello SharePoint összekötő, először létre kell hoznia egy **kapcsolat** hello részletek adja meg ezeket a tulajdonságokat: 

| Tulajdonság | Szükséges | Leírás |
| --- | --- | --- |
| Token |Igen |Adja meg a SharePoint rendszerbeli hitelesítő adatokat |

tooconnect túl**SharePoint**, adja meg az identitás (felhasználónév és jelszó, intelligens kártya hitelesítő adatainak stb.) tooSharePoint. Amennyiben Ön már hitelesítve, toouse hello SharePoint összekötő a Logic Apps alkalmazást továbbléphessen. 

A hello tervezője a Logic Apps alkalmazást, kövesse ezeket a lépéseket toosign a SharePoint toocreate hello kapcsolat **kapcsolat** használható a Logic Apps alkalmazást:

1. Hello keresőmezőbe írja be a SharePoint, és várjon, amíg hello keresési tooreturn hello neve a SharePoint összes bejegyzést:   
   ![A SharePoint konfigurálása][1]  
2. Válassza ki **SharePoint - fájl létrehozásakor**   
3. Válassza ki **tooSharePoint bejelentkezés**:   
   ![A SharePoint konfigurálása][2]    
4. Adja meg a SharePoint hitelesítő adatok toosign tooauthenticate a SharePoint   
   ![A SharePoint konfigurálása][3]     
5. Hello hitelesítés befejezése után lesz átirányított tooyour logic app toocomplete azt SharePoint konfigurálásával **fájl létrehozásának** párbeszédpanel.          
   ![A SharePoint konfigurálása][4]  
6. Más eseményindítók és műveletek toocomplete a Logic Apps alkalmazást újra kell majd is hozzáadhat.   
7. Mentse a munkáját kiválasztásával **mentése** fenti hello menüsávjában.  

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sharepoint/).

## <a name="more-connectors"></a>További összekötők
Lépjen vissza toohello [API-k lista](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
