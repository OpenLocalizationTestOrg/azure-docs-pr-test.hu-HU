---
title: "Az Azure felügyeleti API-tanúsítvány feltöltése |} Microsoft Docs"
description: "Megtudhatja, hogyan athe felügyeleti API-tanúsítvány feltöltése a klasszikus Azure portálon."
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
ms.openlocfilehash: 9dc438e927acd9aef38f06807fabf3dda9b021c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Az Azure szolgáltatásfelügyeleti API Management-tanúsítvány feltöltése
Felügyeleti tanúsítványok hitelesítik magukat az Azure által biztosított klasszikus üzembe helyezési modellel teszik lehetővé. Számos programok telepítése és eszközök (például a Visual Studio vagy az Azure SDK-val) és a különböző Azure-szolgáltatások telepítési automatizálására használható ezeket a tanúsítványokat. 

> [!WARNING]
> légy óvatos! Ezek a típusok, a tanúsítványok bárki hitelesíti magát azokat kezelheti az előfizetést, amelyekhez tartoznak.
>
>

Ha szeretne további információ az Azure (beleértve a önaláírt tanúsítvány létrehozása), lásd: [tanúsítványok áttekintése Azure-szolgáltatásokhoz](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

Is [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) automation célokra Ügyfélkód hitelesítéséhez.

## <a name="upload-a-management-certificate"></a>A felügyeleti tanúsítvány feltöltése
Ha elvégezte a felügyeleti tanúsítvány létrehozása, (.cer-fájl csak a nyilvános kulcs) feltöltheti a portált. Ha a tanúsítvány áll rendelkezésre a portálon, bárki, aki egyező tanúsítványt (titkos kulcs) csatlakozzon a felügyeleti API-n keresztül, és a társított előfizetés erőforrásokhoz férnek hozzá.

1. Jelentkezzen be az [Azure portálra](http://portal.azure.com).
2. Kattintson a **további szolgáltatások** alsó Azure-szolgáltatások listája, majd válassza **előfizetések** a a _általános_ szolgáltatási csoport.

    ![Előfizetés menü](./media/azure-api-management-certs/subscriptions_menu.png)

3. Ügyeljen arra, hogy válassza ki a megfelelő előfizetést, amelyet szeretne társítani a tanúsítványt.     
4. Miután kiválasztotta a megfelelő előfizetés, nyomja le az **felügyeleti tanúsítványok** a a _beállítások_ csoport.

    ![Beállítások](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Nyomja meg a **feltöltése** gombra.

    ![A lapon a tanúsítványok feltöltése](./media/azure-api-management-certs/certificates_page.png)
6. Töltse ki a párbeszédpanelen információkat, és nyomja le az **feltöltése**.

    ![Beállítások](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Következő lépések
Most, hogy egy előfizetéshez társított felügyeleti tanúsítvány, (miután helyileg a megfelelő tanúsítvány telepítése) programokon keresztül csatlakozhat a [klasszikus üzembe helyezési modellel REST API](https://msdn.microsoft.com/library/azure/mt420159.aspx) és automatizálhatja a különböző Azure-erőforrások, amelyek is, hogy az előfizetéshez társítva.
