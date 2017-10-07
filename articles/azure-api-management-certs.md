---
title: "az Azure felügyeleti API tanúsítvány aaaUpload |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupload athe felügyeleti API a klasszikus Azure portál hello tanúsítvány."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 8294d7131cfb01dba664bd4fd04b6fc22c1e93ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Az Azure szolgáltatásfelügyeleti API Management-tanúsítvány feltöltése
Felügyeleti tanúsítványok engedélyezése az Azure által biztosított tooauthenticate hello klasszikus üzembe helyezési modellt. Számos programok telepítése és eszközök (például a Visual Studio vagy hello Azure SDK-t) használni ezeket a tanúsítványokat tooautomate konfigurációs és különböző Azure-szolgáltatásokhoz központi telepítését. 

> [!WARNING]
> légy óvatos! Ezek a típusok, a tanúsítványok bárki hitelesíti magát őket toomanage hello előfizetéshez vannak hozzárendelve.
>
>

Ha szeretne további információ az Azure (beleértve a önaláírt tanúsítvány létrehozása), lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Is [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) tooauthenticate-Ügyfélkód automation célokra.

## <a name="upload-a-management-certificate"></a>A felügyeleti tanúsítvány feltöltése
Ha elvégezte a felügyeleti tanúsítvány létrehozása, (.cer-fájl csak hello nyilvános kulcs) feltöltheti hello portált. Hello tanúsítvány nem érhető el a hello portál, bárki, aki egyező tanúsítványt (titkos kulcs) hello felügyeleti API és a hozzáférés hello hello tartozó előfizetés erőforrásainak keresztül kapcsolódhatnak.

1. Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com).
2. Kattintson a **további szolgáltatások** hello alsó Azure-szolgáltatások listáját, majd válassza **előfizetések** a hello _általános_ szolgáltatási csoport.

    ![Előfizetés menü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Ellenőrizze, hogy tooselect hello megfelelő előfizetés, amelyet az tooassociate hello tanúsítvánnyal.     
4. Miután kiválasztotta a megfelelő előfizetés hello, nyomja le az **felügyeleti tanúsítványok** a hello _beállítások_ csoport.

    ![Beállítások](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Nyomja le az hello **feltöltése** gombra.

    ![A lapon a tanúsítványok feltöltése](./media/azure-api-management-certs/certificates_page.png)
6. Töltse ki a hello párbeszédpanel adatai, és nyomja le az **feltöltése**.

    ![Beállítások](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Következő lépések
Most, hogy egy előfizetéshez társított felügyeleti tanúsítvány, (a frissítés telepítése után helyileg a tanúsítvány megfelelő hello) programozott módon csatlakoztathatja toohello [klasszikus üzembe helyezési modellel REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) és automatizálásához hello különböző Azure-erőforrások is az adott előfizetéshez társítva van.
