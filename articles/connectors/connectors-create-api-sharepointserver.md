---
title: "A SharePoint Server-összekötő használata a Logic Apps |} Microsoft Docs"
description: "Az első lépéseiben a Logic Apps alkalmazásait a SharePoint Server-összekötő"
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
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-connector"></a><span data-ttu-id="c19ee-103">A SharePoint-összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="c19ee-103">Get started with the SharePoint connector</span></span>
<span data-ttu-id="c19ee-104">A SharePoint-összekötő segítségével egy SharePoint-webhelyre listák használata.</span><span class="sxs-lookup"><span data-stu-id="c19ee-104">The SharePoint Connector provides an way to work with Lists on SharePoint.</span></span>

<span data-ttu-id="c19ee-105">Hozzon létre egy logic app; első lépései Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c19ee-105">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-sharepoint"></a><span data-ttu-id="c19ee-106">Kapcsolatot létesíthet SharePoint</span><span class="sxs-lookup"><span data-stu-id="c19ee-106">Create a connection to SharePoint</span></span>
<span data-ttu-id="c19ee-107">A SharePoint-összekötő használatához először létre kell hoznia egy **kapcsolat** adja meg a részleteket a tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="c19ee-107">To use the SharePoint Connector , you first create a **connection** then provide the details for these properties:</span></span> 

| <span data-ttu-id="c19ee-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c19ee-108">Property</span></span> | <span data-ttu-id="c19ee-109">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c19ee-109">Required</span></span> | <span data-ttu-id="c19ee-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="c19ee-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c19ee-111">Token</span><span class="sxs-lookup"><span data-stu-id="c19ee-111">Token</span></span> |<span data-ttu-id="c19ee-112">Igen</span><span class="sxs-lookup"><span data-stu-id="c19ee-112">Yes</span></span> |<span data-ttu-id="c19ee-113">Adja meg a SharePoint rendszerbeli hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="c19ee-113">Provide SharePoint Credentials</span></span> |

<span data-ttu-id="c19ee-114">Csatlakozni **SharePoint**, adja meg az identitás (felhasználónév és jelszó, intelligens kártyához tartozó hitelesítő adatok, stb.) a SharePoint.</span><span class="sxs-lookup"><span data-stu-id="c19ee-114">To connect to **SharePoint**, enter your identity (username and password, smart card credentials, etc.) to SharePoint.</span></span> <span data-ttu-id="c19ee-115">Amennyiben Ön már hitelesítve, folytassa a SharePoint-összekötő használatához a Logic Apps alkalmazást a.</span><span class="sxs-lookup"><span data-stu-id="c19ee-115">Once you've been authenticated, you can proceed to use the SharePoint connector  in your logic app.</span></span> 

<span data-ttu-id="c19ee-116">A Logic Apps alkalmazást designer, a következő lépések segítségével jelentkezzen be a SharePoint, a VPN-kapcsolat létrehozásához **kapcsolat** használható a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="c19ee-116">While on the designer of your logic app, follow these steps to sign into SharePoint to create the connection **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="c19ee-117">A keresőmezőbe írja be a SharePoint, és várja meg, a Keresés a SharePoint összes bejegyzés a nevét adja vissza:</span><span class="sxs-lookup"><span data-stu-id="c19ee-117">Enter SharePoint in the search box and wait for the search to return all entries with SharePoint in the name:</span></span>   
   ![A SharePoint konfigurálása][1]  
2. <span data-ttu-id="c19ee-119">Válassza ki **SharePoint - fájl létrehozásakor**</span><span class="sxs-lookup"><span data-stu-id="c19ee-119">Select **SharePoint - When a file is created**</span></span>   
3. <span data-ttu-id="c19ee-120">Válassza ki **jelentkezzen be a SharePoint**:</span><span class="sxs-lookup"><span data-stu-id="c19ee-120">Select **Sign in to SharePoint**:</span></span>   
   <span data-ttu-id="c19ee-121">![A SharePoint konfigurálása][2]</span><span class="sxs-lookup"><span data-stu-id="c19ee-121">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="c19ee-122">Jelentkezzen be a SharePoint való hitelesítéshez szükséges a SharePoint hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="c19ee-122">Provide your SharePoint credentials to sign in to authenticate with SharePoint</span></span>   
   ![A SharePoint konfigurálása][3]     
5. <span data-ttu-id="c19ee-124">A hitelesítés befejezése után meg fogja átirányítani a Logic Apps alkalmazást úgy konfigurálja a SharePoint befejezéséhez **fájl létrehozásának** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c19ee-124">After the authentication completes you'll be redirected to your logic app to complete it by configuring SharePoint's **When a file is created** dialog.</span></span>          
   <span data-ttu-id="c19ee-125">![A SharePoint konfigurálása][4]</span><span class="sxs-lookup"><span data-stu-id="c19ee-125">![Configure SharePoint][4]</span></span>  
6. <span data-ttu-id="c19ee-126">Más eseményindítók és műveletek végre kell hajtania a Logic Apps alkalmazást, majd adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="c19ee-126">You can then add other triggers and actions that you need to complete your logic app.</span></span>   
7. <span data-ttu-id="c19ee-127">Mentse a munkáját kiválasztásával **mentése** fenti menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="c19ee-127">Save your work by selecting **Save** on the menu bar above.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="c19ee-128">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="c19ee-128">Connector-specific details</span></span>

<span data-ttu-id="c19ee-129">Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="c19ee-129">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/sharepoint/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="c19ee-130">További összekötők</span><span class="sxs-lookup"><span data-stu-id="c19ee-130">More connectors</span></span>
<span data-ttu-id="c19ee-131">Lépjen vissza a [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c19ee-131">Go back to the [APIs list](apis-list.md).</span></span>

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
