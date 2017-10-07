---
title: "a Visual Studio egy Azure-felhőszolgáltatás-projekt aaaConfigure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure az Azure felhőalapú szolgáltatás projektre a Visual Studio, a projekt követelményeitől függően."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 609d6965-05cc-47b1-82dc-c76a92d4f295
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: 40eb5eedd6ea23bf03c8707431799be28beae701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>A Visual Studio egy Azure-felhőszolgáltatás-projekt konfigurálása
Egy Azure-felhőszolgáltatás-projekt, beállíthatja a projekt követelményeitől függően. A következő kategóriák hello hello projekt tulajdonságait állíthatja be:

- **Közzétételéhez egy cloud service tooAzure** -beállíthat egy tulajdonság toomake meg arról, hogy egy meglévő felhőalapú környezetben telepített tooAzure nem véletlenül törli.
- **Futtassa, vagy egy felhőalapú szolgáltatás a helyi számítógépen hello debug** -kiválaszthatja a szolgáltatás konfigurációs toouse, és adja meg, hogy toostart hello az Azure storage emulator.
- **Ellenőrizze a cloud service-csomag létrehozásakor** -eldöntheti, így biztosíthatja, hogy hello cloud service-csomag összes figyelmeztetés hibaként telepíti probléma nélkül tootreat. 

## <a name="steps-tooconfigure-an-azure-cloud-service-project"></a>Lépéseket tooconfigure egy Azure-felhőszolgáltatás-projekt
1. Nyissa meg vagy egy felhőszolgáltatás-projekt létrehozása a Visual Studióban

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **tulajdonságok**.
   
1. Hello projekt Tulajdonságok lapján válassza hello **fejlesztési** fülre.

    ![Projekt tulajdonságai menü](./media/vs-azure-tools-configuring-an-azure-project/solution-explorer-project-properties-menu.png)

1. Állítsa be **Rákérdezés a meglévő telepítés törlése előtt** túl**igaz**. Ez a beállítás segít tooensure véletlenül törli nem egy meglévő Azure-telepítés

1. Szükséges válassza hello **szolgáltatáskonfiguráció** mely szolgáltatáskonfiguráció kívánt toouse futtatásakor vagy a felhőalapú szolgáltatás helyileg debug tooindicate. További információt a toomodify szolgáltatáskonfiguráció szerepkör, lásd: [hogyan tooconfigure hello szerepkörök az Azure cloud service, a Visual Studio](./vs-azure-tools-configure-roles-for-cloud-service.md).

1. Állítsa be **Start Azure storage emulator** túl**igaz** toostart hello az Azure storage emulator futtatásakor vagy a felhőalapú szolgáltatás helyi hibakeresése.

1. Állítsa be **figyelmeztetések hibaként** túl**igaz** toomake meg arról, hogy Ön nem tehető közzé, ha a csomag érvényesítési hibák vannak.

1. Állítsa be **webes projekt portok használatára** túl**igaz** toomake arról, hogy, hogy a webes szerepkör hello azonos port minden egyes elindításakor azt a helyi IIS Express.

1. Hello Visual Studio eszköztárán válassza **mentése**.

## <a name="next-steps"></a>Következő lépések
- [Több szolgáltatáskonfiguráció használata Azure-projekt konfigurálása](vs-azure-tools-multiple-services-project-configurations.md)

