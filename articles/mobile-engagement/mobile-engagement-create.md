---
title: "az Azure Mobile Engagement-alkalmazáshoz aaaCreate |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy új Mobile Engagement-Alkalmazásgyűjteménynek az Azure és az alkalmazások kezelését start hello a Mobile Engagement portálon."
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
ms.openlocfilehash: 4da27e626dacfb6fcfbcb87458c37ea75689a33b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-mobile-engagement-app"></a><span data-ttu-id="17155-103">Azure Mobile Engagement-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="17155-103">Create an Azure Mobile Engagement App</span></span>
<span data-ttu-id="17155-104">Ez a cikk bemutatja, hogyan toouse hello **Gyorslétrehozás** metódus toocreate egy új **Azure Mobile Engagement** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="17155-104">This article shows how toouse hello **Quick Create** method toocreate a new **Azure Mobile Engagement** App.</span></span> <span data-ttu-id="17155-105">hello a következő cikket is bemutatja hogyan toonavigate tooyour **a Mobile Engagement** rendelés toostart figyelése és az alkalmazások kezelését a portálon.</span><span class="sxs-lookup"><span data-stu-id="17155-105">hello article also shows how toonavigate tooyour **Mobile Engagement** portal in order toostart monitoring and managing your apps.</span></span> 

<span data-ttu-id="17155-106">Vegye figyelembe a minimális készletét "alapszintű integrációt" hozzá kell adnia a rendelés toobe képes toocollect adatok az alkalmazás, valamint leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="17155-106">Note that you must add a minimum set of "basic integration" in order toobe able toocollect data for your app and send push notifications.</span></span> <span data-ttu-id="17155-107">hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement-integráció](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="17155-107">hello complete integration documentation can be found in hello [Mobile Engagement integration](mobile-engagement-windows-store-integrate-engagement.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17155-108">toocomplete bármely Azure Mobile Engagement-oktatóanyag, rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="17155-108">toocomplete any Azure Mobile Engagement tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="17155-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="17155-109">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="17155-110">További információkért lásd: <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Ingyenes Azure-fiók létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="17155-110">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>
> 
> 

## <a name="setup-mobile-engagement-for-your-mobile-app-in-azure"></a><span data-ttu-id="17155-111">A Mobile Engagement beállítása a mobilalkalmazáshoz az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="17155-111">Setup Mobile Engagement for your mobile app in Azure</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="navigate-tooyour-mobile-engagement-portal"></a><span data-ttu-id="17155-112">Lépjen a Mobile Engagement portál tooyour</span><span class="sxs-lookup"><span data-stu-id="17155-112">Navigate tooyour Mobile Engagement portal</span></span>
<span data-ttu-id="17155-113">toostart figyelése és kezelése az alkalmazásról, keresse meg tooyour Mobile Engagement portál hello kattintva **Engagement portál** gombra a felső eszköztáron hello.</span><span class="sxs-lookup"><span data-stu-id="17155-113">toostart monitoring and managing your application, navigate tooyour Mobile Engagement portal by clicking hello **Engagement portal** button in hello top bar.</span></span>

<span data-ttu-id="17155-114">Miután hello Mobile Engagement portál, elemzése, hozzon létre és kezelheti a szegmenseket is elérhetők a toohello felhasználókat stb.:</span><span class="sxs-lookup"><span data-stu-id="17155-114">Once you are in hello  Mobile Engagement portal, you can analyze, create and manage segments, reach out toohello users, etc.:</span></span>    

* [<span data-ttu-id="17155-115">Az alkalmazással kapcsolatos valós idejű adatok figyelése</span><span class="sxs-lookup"><span data-stu-id="17155-115">Monitor real time data about your application</span></span>](mobile-engagement-user-interface-monitor.md)
* [<span data-ttu-id="17155-116">Az alkalmazás előzményadatainak elemzése</span><span class="sxs-lookup"><span data-stu-id="17155-116">Analyze historical data about your application</span></span>](mobile-engagement-user-interface-analytics.md)
* [<span data-ttu-id="17155-117">A felhasználók tooidentify használati minták szegmenseinek létrehozása és kezelése</span><span class="sxs-lookup"><span data-stu-id="17155-117">Create and manage segments of users tooidentify usage patterns</span></span>](mobile-engagement-user-interface-segments.md)
* [<span data-ttu-id="17155-118">Kapcsolatfelvétel a leküldéses értesítések toohello felhasználókkal</span><span class="sxs-lookup"><span data-stu-id="17155-118">Reach out toohello users of your application with push notifications</span></span>](mobile-engagement-user-interface-reach.md)

## <a name="see-also"></a><span data-ttu-id="17155-119">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="17155-119">See Also</span></span>
[<span data-ttu-id="17155-120">A Mobile Engagement-stratégia kidolgozása</span><span class="sxs-lookup"><span data-stu-id="17155-120">Define your Mobile Engagement strategy</span></span>](mobile-engagement-define-your-mobile-engagement-strategy.md)

<span data-ttu-id="17155-121">[Ismerkedés az Azure Mobile Engagement](mobile-engagement-windows-store-dotnet-get-started.md) (választhat másik mobilplatformot hello oldal hello tetején).</span><span class="sxs-lookup"><span data-stu-id="17155-121">[Get started with Azure Mobile Engagement](mobile-engagement-windows-store-dotnet-get-started.md) (you can select other mobile platforms at hello top of hello page).</span></span>

