---
title: aaaUsing hello REST API tooaccess Azure Mobile Engagement Service API-k
description: Ismerteti, hogyan toouse hello Azure Mobile Engagement Service API-k a Mobile Engagement REST API-k tooaccess
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: 
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 315761299a42df6f65e81df20e632f9713844b0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-rest-tooaccess-azure-mobile-engagement-service-apis"></a>REST tooaccess Azure Mobile Engagement Service API-k használatával
Az Azure Mobile Engagement biztosít hello [Azure Mobile Engagement REST API](https://msdn.microsoft.com/library/azure/mt683754.aspx) meg toomanage eszközök Reach/leküldéses kampányokra stb.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Ha nem szeretné, hogy toouse hello REST API-k közvetlenül, azt is adja meg a [Swagger-fájl](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) használható az eszközök toogenerate SDK-k számára a kívánt nyelvet. Azt javasoljuk, hello [AutoRest](https://github.com/Azure/AutoRest) eszköz toogenerate az SDK a Swagger-fájl. A .NET SDK létrehoztunk egy hasonló módon, amely lehetővé teszi a toointeract ezen API-khoz a C# burkoló használatával, és nem kell toodo hello hitelesítési jogkivonat egyeztetése révén, és frissítse magát. Lásd: [API a .NET SDK minta](mobile-engagement-dotnet-sdk-service-api.md) toolearn hogyan toouse hello .net SDK API
