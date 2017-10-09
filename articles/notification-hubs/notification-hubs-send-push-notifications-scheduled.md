---
title: "aaaHow toosend ütemezett értesítések |} Microsoft Docs"
description: "Ez a témakör ismerteti az ütemezett értesítések az Azure Notification Hubs használatával."
services: notification-hubs
documentationcenter: .net
keywords: "leküldéses értesítések, leküldéses értesítési leküldéses értesítések ütemezése"
author: ysxu
manager: erikre
editor: 
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9b3ba715dad6f5d824a615e83f2863b0db47b533
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-send-scheduled-notifications"></a><span data-ttu-id="eea4f-104">Útmutató: Az ütemezett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="eea4f-104">How To: Send scheduled notifications</span></span>
## <a name="overview"></a><span data-ttu-id="eea4f-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="eea4f-105">Overview</span></span>
<span data-ttu-id="eea4f-106">Ha egy olyan forgatókönyvet, amelyben szeretné toosend hello bármikor értesítés jövőbeli, de nem rendelkeznek egy egyszerűen toowake a háttér-kód toosend hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="eea4f-106">If you have a scenario in which you want toosend a notification at some point in hello future, but do not have an easy way toowake up your back-end code toosend hello notification.</span></span> <span data-ttu-id="eea4f-107">Standard szint a Notification Hubs egy szolgáltatás, amely lehetővé teszi fel a jövőbeni hello too7 napokat tooschedule értesítéseket támogatja.</span><span class="sxs-lookup"><span data-stu-id="eea4f-107">Standard tier Notification Hubs supports a feature that enables you tooschedule notifications up too7 days in hello future.</span></span>

<span data-ttu-id="eea4f-108">Értesítést küld, ha egyszerűen használható hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) hello Notification Hubs SDK osztályt a hello a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="eea4f-108">When sending a notification, simply use hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) class in hello Notification Hubs SDK as shown in hello following example:</span></span>

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

<span data-ttu-id="eea4f-109">Emellett megszakíthatja a notificationId használatával korábban ütemezett értesítést:</span><span class="sxs-lookup"><span data-stu-id="eea4f-109">Also, you can cancel a previously scheduled notification using its notificationId:</span></span>

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

<span data-ttu-id="eea4f-110">Nincsenek hello számára ütemezett értesítéseket küldhet a korlátozások.</span><span class="sxs-lookup"><span data-stu-id="eea4f-110">There are no limits on hello number of scheduled notifications you can send.</span></span>

