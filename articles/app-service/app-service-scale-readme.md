---
title: "Az Azure App Service: Az App Service alkalmazások méretezése"
description: "Ismerje meg, hogy hello modulok és elemek a méretezés alkalmazás az App Service-ben."
keywords: "app service, azure app service, méret, méretezhető, app service-csomag, app service ára"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: f403c971-4450-432b-8cea-3eeb426c0147
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2016
ms.author: byvinyal
ms.openlocfilehash: d8cd12f73915a916a75d46da2f751a40d567b189
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-scaling-app-service-applications"></a>Az Azure App Service: Az App Service alkalmazások méretezése
Az Azure App Service-ben üzemeltetett alkalmazások is [jelentős mértékű elérése](https://azure.microsoft.com/blog/canadian-broadcasting-corporation-radio-canada-leverage-azure-for-smooth-election-coverage/).
Azonban az alkalmazás skálázás "egy mérete megfelel az összes" megoldás nem rendelkező összetett probléma. toocorrectly az alkalmazás vertikális 3 főbb területet, ez is hozzájárul a tooyour alkalmazások sikeres:

1. Az alkalmazás-architektúra és a gyenge ismertetése.
   * Az az alkalmazás állapotalapú alkalmazások és szolgáltatások? Állapot nélküli?
   * Mik az alkalmazás összes hello összetevőket?
     * Hol vannak hello keresztmetszetei hello alkalmazást?
   * Alkalmazott tooyour alkalmazás terhelés esetén milyen törés első?
2. Understanding hello várható terhelést és teljesítménybeli követelményeit.
   * Szükséges tooserve egy ezer felhasználók hello alkalmazás? vagy egymillió?
   * Egy földrajzi helyről vagy globálisan határozza meg forgalom?
   * Vannak-e határozza változata? forgalom csúcsait?
   * Milyen gyorsan kell válaszolnia a hello alkalmazást? 1 másodperc? 1 ezredmásodpercre?
3. Ismertetése és megfelelően emelés hello platform Alkalmazáshasználat üzemeltetési.
   * Szolgáltatások kell szeretnék használni tooachieve saját méretezési célok?

Ez a szakasz segítséget nyújt a hello tényezők és megtervezi a stratégiát, amely szükséges az App Service szolgáltatások tooachieve hello kihasználja a méretezhetőségi célok súgó.

[!INCLUDE [app-service-blueprint-scaling-app-service-applications](../../includes/app-service-blueprint-scaling-app-service-applications.md)]

