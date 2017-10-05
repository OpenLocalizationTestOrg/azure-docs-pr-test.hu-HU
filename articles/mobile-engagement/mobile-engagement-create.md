---
title: "Azure Mobile Engagement-alkalmazás létrehozása | Microsoft Docs"
description: "Ez a cikk egy új Mobile Engagement-alkalmazásgyűjteménynek az Azure-ban való létrehozását, valamint azt ismerteti, hogy hogyan kezdheti meg az alkalmazások kezelését a Mobile Engagement portál használatával."
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: b8aa1798-28c6-424c-a5b5-8a264d5a0ff0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: 47c1e122f6f38654cd63bb59e50e68803f76c83d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-mobile-engagement-app"></a><span data-ttu-id="1fbb2-103">Azure Mobile Engagement-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1fbb2-103">Create an Azure Mobile Engagement App</span></span>
<span data-ttu-id="1fbb2-104">Ez a cikk bemutatja, hogyan használható a **Gyors létrehozás** módszer új **Azure Mobile Engagement**-alkalmazás létrehozására.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-104">This article shows how to use the **Quick Create** method to create a new **Azure Mobile Engagement** App.</span></span> <span data-ttu-id="1fbb2-105">A cikk azt is ismerteti, hogyan nyithatja meg a **Mobile Engagement** portált az alkalmazások figyelésének és kezelésének megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-105">The article also shows how to navigate to your **Mobile Engagement** portal in order to start monitoring and managing your apps.</span></span> 

<span data-ttu-id="1fbb2-106">Fontos, hogy legalább „alapszintű integrációt” hozzá kell adnia ahhoz, hogy adatokat gyűjthessen az alkalmazásban, valamint leküldéses értesítéseket küldhessen.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-106">Note that you must add a minimum set of "basic integration" in order to be able to collect data for your app and send push notifications.</span></span> <span data-ttu-id="1fbb2-107">A teljes integrációs dokumentáció itt található: [Mobile Engagement-integráció](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="1fbb2-107">The complete integration documentation can be found in the [Mobile Engagement integration](mobile-engagement-windows-store-integrate-engagement.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1fbb2-108">Minden Azure Mobile Engagement-oktatóanyag elvégzéséhez aktív Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-108">To complete any Azure Mobile Engagement tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="1fbb2-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-109">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="1fbb2-110">További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-110">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="setup-mobile-engagement-for-your-mobile-app-in-azure"></a><span data-ttu-id="1fbb2-111">A Mobile Engagement beállítása a mobilalkalmazáshoz az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="1fbb2-111">Setup Mobile Engagement for your mobile app in Azure</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="navigate-to-your-mobile-engagement-portal"></a><span data-ttu-id="1fbb2-112">A Mobile Engagement portál megnyitása</span><span class="sxs-lookup"><span data-stu-id="1fbb2-112">Navigate to your Mobile Engagement portal</span></span>
<span data-ttu-id="1fbb2-113">Az alkalmazás megfigyelésének és kezelésének megkezdéséhez lépjen a Mobile Engagement portálra a lap tetején található **Engagement-portál** gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="1fbb2-113">To start monitoring and managing your application, navigate to your Mobile Engagement portal by clicking the **Engagement portal** button in the top bar.</span></span>

<span data-ttu-id="1fbb2-114">Miután belépett a Mobile Engagement portálra, elemezheti, létrehozhatja és kezelheti a szegmenseket, elérheti a felhasználókat és így tovább:</span><span class="sxs-lookup"><span data-stu-id="1fbb2-114">Once you are in the  Mobile Engagement portal, you can analyze, create and manage segments, reach out to the users, etc.:</span></span>    

* [<span data-ttu-id="1fbb2-115">Az alkalmazással kapcsolatos valós idejű adatok figyelése</span><span class="sxs-lookup"><span data-stu-id="1fbb2-115">Monitor real time data about your application</span></span>](mobile-engagement-user-interface-monitor.md)
* [<span data-ttu-id="1fbb2-116">Az alkalmazás előzményadatainak elemzése</span><span class="sxs-lookup"><span data-stu-id="1fbb2-116">Analyze historical data about your application</span></span>](mobile-engagement-user-interface-analytics.md)
* [<span data-ttu-id="1fbb2-117">Felhasználók szegmenseinek létrehozása és kezelése a használati minták azonosításához</span><span class="sxs-lookup"><span data-stu-id="1fbb2-117">Create and manage segments of users to identify usage patterns</span></span>](mobile-engagement-user-interface-segments.md)
* [<span data-ttu-id="1fbb2-118">Kapcsolatfelvétel a felhasználókkal leküldéses értesítések segítségével</span><span class="sxs-lookup"><span data-stu-id="1fbb2-118">Reach out to the users of your application with push notifications</span></span>](mobile-engagement-user-interface-reach.md)

## <a name="see-also"></a><span data-ttu-id="1fbb2-119">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1fbb2-119">See Also</span></span>
[<span data-ttu-id="1fbb2-120">A Mobile Engagement-stratégia kidolgozása</span><span class="sxs-lookup"><span data-stu-id="1fbb2-120">Define your Mobile Engagement strategy</span></span>](mobile-engagement-define-your-mobile-engagement-strategy.md)

<span data-ttu-id="1fbb2-121">[Ismerkedés az Azure Mobile Engagement szolgáltatással](mobile-engagement-windows-store-dotnet-get-started.md) (a lap tetején választhat másik mobilplatformot).</span><span class="sxs-lookup"><span data-stu-id="1fbb2-121">[Get started with Azure Mobile Engagement](mobile-engagement-windows-store-dotnet-get-started.md) (you can select other mobile platforms at the top of the page).</span></span>

