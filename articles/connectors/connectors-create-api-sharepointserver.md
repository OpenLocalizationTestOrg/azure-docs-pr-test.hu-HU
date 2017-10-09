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
# <a name="get-started-with-hello-sharepoint-connector"></a><span data-ttu-id="19c1a-103">Hello SharePoint összekötő az első lépései</span><span class="sxs-lookup"><span data-stu-id="19c1a-103">Get started with hello SharePoint connector</span></span>
<span data-ttu-id="19c1a-104">SharePoint-összekötő hello egy módon toowork listákkal SharePoint biztosít.</span><span class="sxs-lookup"><span data-stu-id="19c1a-104">hello SharePoint Connector provides an way toowork with Lists on SharePoint.</span></span>

<span data-ttu-id="19c1a-105">Hozzon létre egy logic app; első lépései Lásd: [logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="19c1a-105">Get started by creating a logic app; see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-toosharepoint"></a><span data-ttu-id="19c1a-106">Egy kapcsolat tooSharePoint létrehozása</span><span class="sxs-lookup"><span data-stu-id="19c1a-106">Create a connection tooSharePoint</span></span>
<span data-ttu-id="19c1a-107">toouse hello SharePoint összekötő, először létre kell hoznia egy **kapcsolat** hello részletek adja meg ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="19c1a-107">toouse hello SharePoint Connector , you first create a **connection** then provide hello details for these properties:</span></span> 

| <span data-ttu-id="19c1a-108">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="19c1a-108">Property</span></span> | <span data-ttu-id="19c1a-109">Szükséges</span><span class="sxs-lookup"><span data-stu-id="19c1a-109">Required</span></span> | <span data-ttu-id="19c1a-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="19c1a-110">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="19c1a-111">Token</span><span class="sxs-lookup"><span data-stu-id="19c1a-111">Token</span></span> |<span data-ttu-id="19c1a-112">Igen</span><span class="sxs-lookup"><span data-stu-id="19c1a-112">Yes</span></span> |<span data-ttu-id="19c1a-113">Adja meg a SharePoint rendszerbeli hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="19c1a-113">Provide SharePoint Credentials</span></span> |

<span data-ttu-id="19c1a-114">tooconnect túl**SharePoint**, adja meg az identitás (felhasználónév és jelszó, intelligens kártya hitelesítő adatainak stb.) tooSharePoint.</span><span class="sxs-lookup"><span data-stu-id="19c1a-114">tooconnect too**SharePoint**, enter your identity (username and password, smart card credentials, etc.) tooSharePoint.</span></span> <span data-ttu-id="19c1a-115">Amennyiben Ön már hitelesítve, toouse hello SharePoint összekötő a Logic Apps alkalmazást továbbléphessen.</span><span class="sxs-lookup"><span data-stu-id="19c1a-115">Once you've been authenticated, you can proceed toouse hello SharePoint connector  in your logic app.</span></span> 

<span data-ttu-id="19c1a-116">A hello tervezője a Logic Apps alkalmazást, kövesse ezeket a lépéseket toosign a SharePoint toocreate hello kapcsolat **kapcsolat** használható a Logic Apps alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="19c1a-116">While on hello designer of your logic app, follow these steps toosign into SharePoint toocreate hello connection **connection** for use in your logic app:</span></span>

1. <span data-ttu-id="19c1a-117">Hello keresőmezőbe írja be a SharePoint, és várjon, amíg hello keresési tooreturn hello neve a SharePoint összes bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="19c1a-117">Enter SharePoint in hello search box and wait for hello search tooreturn all entries with SharePoint in hello name:</span></span>   
   ![A SharePoint konfigurálása][1]  
2. <span data-ttu-id="19c1a-119">Válassza ki **SharePoint - fájl létrehozásakor**</span><span class="sxs-lookup"><span data-stu-id="19c1a-119">Select **SharePoint - When a file is created**</span></span>   
3. <span data-ttu-id="19c1a-120">Válassza ki **tooSharePoint bejelentkezés**:</span><span class="sxs-lookup"><span data-stu-id="19c1a-120">Select **Sign in tooSharePoint**:</span></span>   
   <span data-ttu-id="19c1a-121">![A SharePoint konfigurálása][2]</span><span class="sxs-lookup"><span data-stu-id="19c1a-121">![Configure SharePoint][2]</span></span>    
4. <span data-ttu-id="19c1a-122">Adja meg a SharePoint hitelesítő adatok toosign tooauthenticate a SharePoint</span><span class="sxs-lookup"><span data-stu-id="19c1a-122">Provide your SharePoint credentials toosign in tooauthenticate with SharePoint</span></span>   
   ![A SharePoint konfigurálása][3]     
5. <span data-ttu-id="19c1a-124">Hello hitelesítés befejezése után lesz átirányított tooyour logic app toocomplete azt SharePoint konfigurálásával **fájl létrehozásának** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="19c1a-124">After hello authentication completes you'll be redirected tooyour logic app toocomplete it by configuring SharePoint's **When a file is created** dialog.</span></span>          
   <span data-ttu-id="19c1a-125">![A SharePoint konfigurálása][4]</span><span class="sxs-lookup"><span data-stu-id="19c1a-125">![Configure SharePoint][4]</span></span>  
6. <span data-ttu-id="19c1a-126">Más eseményindítók és műveletek toocomplete a Logic Apps alkalmazást újra kell majd is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="19c1a-126">You can then add other triggers and actions that you need toocomplete your logic app.</span></span>   
7. <span data-ttu-id="19c1a-127">Mentse a munkáját kiválasztásával **mentése** fenti hello menüsávjában.</span><span class="sxs-lookup"><span data-stu-id="19c1a-127">Save your work by selecting **Save** on hello menu bar above.</span></span>  

## <a name="connector-specific-details"></a><span data-ttu-id="19c1a-128">Összekötő-specifikus részletei</span><span class="sxs-lookup"><span data-stu-id="19c1a-128">Connector-specific details</span></span>

<span data-ttu-id="19c1a-129">Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/sharepoint/).</span><span class="sxs-lookup"><span data-stu-id="19c1a-129">View any triggers and actions defined in hello swagger, and also see any limits in hello [connector details](/connectors/sharepoint/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="19c1a-130">További összekötők</span><span class="sxs-lookup"><span data-stu-id="19c1a-130">More connectors</span></span>
<span data-ttu-id="19c1a-131">Lépjen vissza toohello [API-k lista](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="19c1a-131">Go back toohello [APIs list](apis-list.md).</span></span>

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
