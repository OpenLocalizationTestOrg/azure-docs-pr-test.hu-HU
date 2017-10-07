---
title: "aaaTroubleshoot felhőalapú szolgáltatás telepítésével kapcsolatos problémák |} Microsoft Docs"
description: "Előfordulhat, hogy tapasztal egy felhőalapú szolgáltatás tooAzure telepítésekor néhány gyakori problémák vannak. Ez a cikk ismerteti a megoldások toosome közülük."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: 15aea4f2b2913d95f3378b2e9762b232531f3c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Felhőalapú szolgáltatás telepítésével kapcsolatos problémák elhárítása
Amikor telepít egy felhőalapú szolgáltatás alkalmazás csomag tooAzure, szerezhetők be hello fejlesztésével kapcsolatos információk hello **tulajdonságok** hello Azure-portálon ablaktábláján. Hello részleteit is használhatja a az ablak toohelp nyújtva a hibaelhárításban hello felhőalapú szolgáltatással, és új támogatási kérelem megnyitásakor az információk tooAzure támogatási biztosíthat.

Hello található **tulajdonságok** ablaktáblán az alábbiak szerint:

* Az Azure-portálon hello kattintson hello üzembe helyezett a felhőalapú szolgáltatás, kattintson **összes beállítás**, és kattintson a **tulajdonságok**.
* A klasszikus Azure portálon hello kattintson hello üzembe helyezett a felhőalapú szolgáltatás, kattintson **IRÁNYÍTÓPULT**, amelynek helye hello lap jobb alsó sarkában hello (alatt **gyors áttekintő**). Vegye figyelembe, hogy nincs nincs "Tulajdonságok" címke ezen az ablaktáblán.

> [!NOTE]
> Hello hello tartalmát másolhatja **tulajdonságok** ablaktábla toohello vágólapra hello ablak jobb felső sarkában hello hello ikonjára kattint.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probléma: nem érhető el a webhely, de a telepítés nincs elindítva, készen áll az összes szerepkörpéldányt
hello webhely URL-cím hivatkozás jelenik meg hello portálon nem tartalmazza a hello port. hello alapértelmezett port webhelyekhez: 80-as. Ha az alkalmazás egy másik portot a konfigurált toorun, hozzá kell adnia hello portjának száma toohello URL hello webhely elérésekor.

1. Hello Azure-portálon kattintson a felhőszolgáltatás hello telepítési.
2. A hello **tulajdonságok** hello Azure-portálon ablaktábláján ellenőrizze hello portok hello szerepkörpéldányokért (alatt **bemeneti végpontok**).
3. Ha hello port nincs 80-as, adjon hozzá hello portjának értéke toohello URL-címe, ha hello alkalmazás elérése. toospecify egy nem alapértelmezett portot hello URL-címét, majd egy kettőspontot (:), hello port számát, szóközök nélkül.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probléma: Az a szerepkörpéldányok nélkül me bármi felhasználását.
Javítás szolgáltatás esetén automatikusan Azure probléma csomópontok észleli, és ezért a szerepkör példányok toonew csomópontok helyezi. Ha ez történik, a szerepkörpéldányok automatikusan újrahasznosítási jelenhet meg. a szolgáltatás, hogy toofind javítás történt:

1. Hello Azure-portálon kattintson a felhőszolgáltatás hello telepítési.
2. A hello **tulajdonságok** hello Azure-portálon ablaktábláján ellenőrizze hello adatokat, és határozza meg, hogy Ön megfigyelt hello szerepkörök újrahasznosítása hello időszakban szolgáltatásjavításnak történt.

Szerepkörök is indul újra nagyjából havonta egyszer gazdagép-OS és a vendég-operációs rendszer frissítése során.  
További információkért lásd: hello blogbejegyzésben [szerepkör példány újraindítása miatt tooOS frissítések](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probléma: I nem hajtsa végre a virtuális IP-címcsere és olyan hibaüzenetet kap
A virtuális IP-címcsere nem engedélyezett, ha a központi telepítés frissítése folyamatban van. Telepítési frissítések automatikusan történik ha:

* Új vendég operációs rendszer és az automatikus frissítések konfigurált.
* Szolgáltatás javító következik be.

kimenő toofind Ha az automatikus frissítés megakadályozza ennek során a virtuális IP-címcsere:

1. Hello Azure-portálon kattintson a felhőszolgáltatás hello telepítési.
2. A hello **tulajdonságok** hello Azure-portálon ablaktábláján hello értékének meg **állapot**. Ha **készen**, majd ellenőrizze **a legutóbbi művelet** toosee, ha az egyik nemrégiben történt, amely megakadályozhatja, hogy hello VIP lapozófájl-kapacitás.
3. Az éles környezet hello ismételje meg az 1. és 2.
4. Ha egy automatikus frissítési folyamat során, várjon, amíg az toofinish közben toodo hello VIP swap előtt.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probléma: Egy szerepkörpéldányt ismétlési van elindítva, inicializálás, foglalt és leállítva között
Ez az állapot problémára mutathat vissza az alkalmazás kódjában, csomagjában, vagy konfigurációs fájljában. Ebben az esetben meg kell tudni toosee hello állapot módosítása néhány percenként és hello Azure-portálon vagy hasonlót **Recycling**, **foglalt**, vagy **inicializálás**. Az azt jelenti, hogy valami nem működik, amely hello szerepkörpéldányt megakadályozza a futtatását hello alkalmazással.

Hogyan további információt a probléma tootroubleshoot lásd hello blogbejegyzésben [Azure PaaS számítási diagnosztikai adatainak](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) és [közös állít ki, hogy OK szerepkörök toorecycle](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Probléma: Az alkalmazás nem működik
1. A hello Azure-portálon kattintson a hello szerepkörpéldányt.
2. A hello **tulajdonságok** ablaktábláján hello Azure-portálon vegye figyelembe a következő feltételek tooresolve hello a problémát:
   * Ha hello szerepkörpéldányt nemrég leállt (ellenőrizheti a hello értékének **megszakítások száma**), hello telepítési sikerült frissítése. Várjon, amíg toosee Ha hello szerepkörpéldányt folytatja működik-e a saját.
   * Ha hello szerepkörpéldányt **foglalt**, jelölje be az alkalmazás kódja toosee, ha hello [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) esemény. Előfordulhat, hogy tooadd kell, vagy javítsa ki a néhány kódot, amely kezeli ezt az eseményt.
   * Diagnosztikai adatok hello keresztül halad, és hibaelhárítás esetén ajánlott a hello blogbejegyzésben [Azure PaaS számítási diagnosztikai adatainak](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Ha újrahasznosítja a felhőalapú szolgáltatás, alaphelyzetbe hello tulajdonságok hello üzembe helyezés esetén hatékonyan törlése hello eredeti probléma hello adatait.
>
>

## <a name="next-steps"></a>Következő lépések
További [cikkek hibaelhárítási](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.

Hogyan tootroubleshoot felhő szerepkör-szolgáltatás Azure PaaS számítógép diagnosztikai adatok használatával állít toolearn lásd [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
