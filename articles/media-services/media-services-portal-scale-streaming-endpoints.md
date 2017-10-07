---
title: "adatfolyam-végpontok az Azure-portálon hello aaaScale |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello a méretezés streamvégpontok a hello Azure-portálon."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: e466edf9232558b9e270f54ee2849cd9b22ad121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-streaming-endpoints-with-hello-azure-portal"></a>Hello Azure-portálon az adatfolyam-továbbítási végpontok méretezése
## <a name="overview"></a>Áttekintés

> [!NOTE]
> toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

A **prémium** szintű streamvégpontok a speciális feladatokhoz ideálisak, mert dedikált és méretezhető sávszélesség-kapacitást nyújtanak. A **prémium** streamvégponttal rendelkező ügyfelek alapértelmezés szerint kapnak egy adategységet (SU-t). adatfolyam-továbbítási végpontra hello SUs hozzáadásával is méretezhető. Minden egyes SU további sávszélesség kapacitás toohello alkalmazásokat tartalmaz. További információ a streaming endpoint típusok és a CDN-konfiguráció: hello [Streaming Endpoint áttekintése](media-services-portal-manage-streaming-endpoints.md) témakör.
 
Ez a témakör bemutatja, hogyan tooscale egy streamvégpontra.

Az árképzésre vonatkozó további információkért lásd: [Media Services Pricing Details](http://go.microsoft.com/fwlink/?LinkId=275107) (A Media Services árképzésére vonatkozó információk).

## <a name="scale-streaming-endpoints"></a>Az adatfolyam végpontjainak méretezése

streamelési egységek számát toochange hello hello a következő:

1. A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.
2. A hello **beállítások** ablakban válassza ki **adatfolyam-végpontok**.
3. Kattintson a hello streamvégpontra, amelyet az tooscale. 

    [!NOTE] Csak méretezheti **prémium** adatfolyam-végpontok.

4. Helyezze át a hello csúszkát toospecify hello streamelési egységek számát.

    ![Adatfolyam-továbbítási végpontra](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

