---
title: "aaaApplication és a szolgáltatás elérhetőségével kapcsolatos problémákat a Microsoft Azure Cloud Services – gyakori kérdések |} Microsoft Docs"
description: "Ez a cikk kapcsolatos gyakori kérdések alkalmazás és szolgáltatás rendelkezésre állás a Microsoft Azure Felhőszolgáltatások hello sorolja fel."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: cd0d9ba0beb1dc72d4761f8b89c2ecadb51c7e48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Alkalmazások és szolgáltatások elérhetőségi problémáinak Azure-szolgáltatásokhoz: gyakran ismételt kérdések (GYIK)

Ez a cikk tartalmazza az alkalmazás és szolgáltatás elérhetőségével kapcsolatos problémákat a kapcsolatos gyakran ismételt kérdések [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Is további részleteket hello [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>A szerepkör lett hasznosítva. A felhőszolgáltatás megkezdődött módosításairól történt?
Nagyjából havonta, Microsoft kiadását egy új vendég operációs rendszer verziója a Windows Azure PaaS virtuális gépeken. A vendég operációs rendszer a hello folyamat csak egy ilyen frissítés. Egy kiadási sok más tényezőktől is érinti. Emellett Azure több ezer gép több száz futtatja. Ezért nem lehetséges toopredict hello pontos dátumot és időpontot, amikor a szerepkörök újraindul. Akkor a vendég operációs rendszer frissítése RSS-hírcsatorna hello hello legújabb adatokat kell frissíteni, de vegye figyelembe, hogy jelenteni idő toobe becsült érték. Azt észleli, hogy ez a problematikus, az ügyfelek és a terv toolimit vagy a pontos idő újraindítások dolgozik.

Teljes legutóbbi vendég operációs rendszer frissítése, lásd: [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](cloud-services-guestos-update-matrix.md).

A Vendég és a gazda operációs Rendszerével frissítések újraindítást és mutatók tootechnical részletek hasznos információkat lásd: hello MSDN blogbejegyzésben [szerepkör példány újraindítása miatt tooOS frissítések](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-hello-first-request-toomy-cloud-service-after-hello-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Miért nem hello első kérelem toomy felhőszolgáltatás után hello szolgáltatás inaktív egy ideig tarthat a szokásosnál hosszabb ideig?
Hello webkiszolgáló hello első kérelmet kap, amikor először újrafordítja hello kódot, és majd a hello kérést dolgoz fel. Amely akkor miért hello első kérelem tovább tart, mint hello másoknak. Alapértelmezés szerint az hello alkalmazáskészlet tétlenség esetekben lekérdezi leállítása. hello alkalmazáskészlet is indul újra alapértelmezés szerint minden 1,740 perc (29 óra).

Internet Information Services (IIS) alkalmazáskészletek lehet rendszeres időközönként újrahasznosít tooavoid instabil állapotok tooapplication összeomlik, lefagy vagy memóriavesztés vezethet.

a következő dokumentumok hello segít megérteni, és a probléma elhárítása érdekében:
* [Az IIS lassú kezdeti betöltés kijavítása](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [Az IIS 7.5 webes alkalmazás első kérelem után-alkalmazáskészlet újrahasznosítási nagyon lelassul](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Ha azt szeretné, hogy IIS toochange hello alapértelmezett viselkedését, szüksége lesz toouse indítási feladatok, mert ha módosítások toohello webes szerepkörpéldányokat manuálisan telepítenie hello módosítások végül elvesznek.

További információkért lásd: [hogyan tooconfigure, és futtassa indítási feladatokat egy felhőalapú szolgáltatás](cloud-services-startup-tasks.md).
