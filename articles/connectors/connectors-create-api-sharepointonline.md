---
title: "A SharePoint Online összekötőt használata a logic apps |} Microsoft Docs"
description: "Hozzon létre a logic apps alkalmazást a SharePoint Online Manage listák a SharePoint-összekötővel."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: e0ec3149-507a-409d-8e7b-d5fbded006ce
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: bdda380360cd2a7595fa05dfe3fc6504e76255b5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-sharepoint-online-connector"></a>A SharePoint Online összekötő az első lépései
A SharePoint Online összekötő segítségével kezelheti a SharePoint-listát.  

Használandó [a csatlakozókat](apis-list.md), először hozzon létre egy logikai alkalmazást. Elkezdheti által [logikai alkalmazás létrehozása most](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-sharepoint-online"></a>Csatlakozás a SharePoint online szolgáltatáshoz
A Logic Apps alkalmazást bármely szolgáltatás hozzáférni, először hozzon létre egy *kapcsolat* a szolgáltatáshoz. A [kapcsolat](connectors-overview.md) biztosít a logikai alkalmazás és egy másik szolgáltatás közötti kapcsolat.  

### <a name="create-a-connection-to-sharepoint-online"></a>Kapcsolatot létesíthet a SharePoint online-hoz
> [!INCLUDE [Steps to create a connection to SharePoint](../../includes/connectors-create-api-sharepointonline.md)]


## <a name="use-a-sharepoint-online-trigger"></a>SharePoint Online eseményindítók
Egy eseményindító nem egy eseményt, a logikai alkalmazás definiált munkafolyamat indításához használható. [További tudnivalók az eseményindítók](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a SharePoint Online trigger](../../includes/connectors-create-api-sharepointonline-trigger.md)]


## <a name="use-a-sharepoint-online-action"></a>A SharePoint Online művelettel
Egy művelet során a logikai alkalmazás definiált munkafolyamat által végzett. [További információ a műveletek](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a SharePoint Online action](../../includes/connectors-create-api-sharepointonline-action.md)]


## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/sharepoint/).

## <a name="next-steps"></a>További lépések
[Logikai alkalmazás létrehozása](../logic-apps/quickstart-create-first-logic-app-workflow.md)

