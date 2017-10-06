---
title: "aaaDeploy Azure API Management-szolgáltatások toomultiple Azure régiók |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy az Azure API Management szolgáltatás példány toomultiple Azure régióban."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 47389ad6-f865-4706-833f-846115e22e4d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 04a3e762261237d73a769320a21363f99f1d20cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-an-azure-api-management-service-instance-toomultiple-azure-regions"></a>Hogyan toodeploy az Azure API Management szolgáltatás példány toomultiple Azure régiók
Az API Management támogatja több területi környezetben, amely lehetővé teszi, hogy az API-közzétevők toodistribute egyetlen API management szolgáltatás tetszőleges számú kívánt Azure-régiók között. Ezzel csökkenthető a kérelem által érzékelt késleltetés földrajzilag elosztott API fogyasztók és a szolgáltatás rendelkezésre állása is támogatja, ha egy régió tartozik offline állapotba kerül. 

Amikor egy API-kezelés szolgáltatás a kezdetben létrejön, csak az egyik tartalmaz [egység] [ unit] és egy önálló, elsődleges régió hello van kijelölve Azure régióban található. További régiókban hello Azure portálon keresztül egyszerűen lehet hozzáadni. Az API Management átjárókiszolgáló telepített tooeach régió, és hívás forgalom lesz irányított toohello legközelebbi átjáró. Ha egy régiót offline állapotba kerül, hello akkor kimenő forgalomról automatikusan újra irányított toohello következő legközelebbi átjáró. 

> [!IMPORTANT]
> Több területi telepítési csak érhető el hello  **[prémium] [ Premium]**  réteg.
> 
> 

## <a name="add-region"></a>Központi telepítése egy API-kezelés szolgáltatás példány tooa új terület
> [!NOTE]
> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Hello Azure portálon lépjen a toohello **és az árképzés** az API Management szolgáltatáspéldány a lap. 

![Skála lap][api-management-scale-service]

toodeploy tooa új régió, kattintson a **+ Hozzáadás régió** hello eszköztáron.

![Adja hozzá a régió][api-management-add-region]

Hello legördülő listából válassza ki a hello helyét, és állítsa be a hello hello csúszkával egységek száma.

![Adja meg a egység][api-management-select-location-units]

Kattintson a **Hozzáadás** tooplace hello helyek tábla választását. 

Ismételje meg ezt a folyamatot, amíg a konfigurált összes hellyel rendelkezik, és kattintson a **mentése** hello eszköztár toostart hello központi telepítési folyamat során.

## <a name="remove-region"></a>Egy API-kezelés szolgáltatás példány törlése
Hello Azure portálon lépjen a toohello **és az árképzés** az API Management szolgáltatáspéldány a lap. 

![Skála lap][api-management-scale-service]

Milyen hello hely tooremove nyissa meg a hello helyi menü használatával hello **...**  hello tábla jobb oldali végén hello gombra. Jelölje be hello **törlése** lehetőséget.

![Távolítsa el a régió][api-management-remove-region]

Hello törlés jóváhagyásához, és kattintson a **mentése** tooapply hello módosításokat.

[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-location-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-location-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Get started with Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance tooa new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[unit]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

