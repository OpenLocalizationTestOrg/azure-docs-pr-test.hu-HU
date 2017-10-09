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
# <a name="how-to-send-scheduled-notifications"></a>Útmutató: Az ütemezett értesítések küldése
## <a name="overview"></a>Áttekintés
Ha egy olyan forgatókönyvet, amelyben szeretné toosend hello bármikor értesítés jövőbeli, de nem rendelkeznek egy egyszerűen toowake a háttér-kód toosend hello értesítés. Standard szint a Notification Hubs egy szolgáltatás, amely lehetővé teszi fel a jövőbeni hello too7 napokat tooschedule értesítéseket támogatja.

Értesítést küld, ha egyszerűen használható hello [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) hello Notification Hubs SDK osztályt a hello a következő példában látható módon:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Emellett megszakíthatja a notificationId használatával korábban ütemezett értesítést:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Nincsenek hello számára ütemezett értesítéseket küldhet a korlátozások.

