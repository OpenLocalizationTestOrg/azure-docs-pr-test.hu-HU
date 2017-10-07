---
title: "a felhőszolgáltatás (klasszikus portál) aaaHow tooconfigure |} Microsoft Docs"
description: "Ismerje meg, hogy miként tooconfigure felhőalapú szolgáltatásokat az Azure-ban. Ismerje meg, tooupdate hello felhőalapú szolgáltatás konfigurációja, és konfigurálja a távelérés toorole példányok."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 1ea2320f97f667153f7984e4d61d373a6344cf6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a>TooConfigure Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-configure-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-configure.md)
> 
> 

A klasszikus Azure portálon hello konfigurálhatja egy felhőalapú szolgáltatás hello leggyakrabban használt beállításait. Vagy, tetszés szerint tooupdate a konfigurációs fájlok közvetlen, töltse le a szolgáltatás konfigurációs fájl tooupdate, majd töltse fel a frissített hello fájl- és frissítési hello felhőszolgáltatás hello konfigurációs módosítások. Mindkét módszer esetén tooall szerepkörpéldányokat hello konfigurációfrissítések vannak leküldött.

hello a klasszikus Azure portálon is lehetővé teszi túl[engedélyezése a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag](cloud-services-role-enable-remote-desktop.md)

Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosításához során hello konfigurációfrissítések ha legalább két szerepkör minden egyes szerepkörhöz. Amely lehetővé teszi, hogy egy virtuális gép tooprocess ügyfélkérelmek más hello frissítése közben. További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Egy felhőalapú szolgáltatás módosítása
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, kattintson hello felhőszolgáltatás hello nevét, majd **konfigurálása**.
   
    ![Konfiguráció lap](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    A hello **konfigurálása** lapon konfigurálja a figyelés, a frissítési beállítások, és válassza ki a hello vendég operációs rendszer és a szerepkörpéldányok készült. 
2. A **figyelési**, szintű tooVerbose figyelés beállítása hello vagy minimális, és konfigurálja a szükséges részletes figyelési hello diagnosztika kapcsolati karakterláncokat.
3. (A szerepkör szerint csoportosítva) szerepköreit frissítheti hello a következő beállításokat:
   
    * **Beállítások** hello hello megadott egyéb konfigurációs beállítások értékének módosítása *ConfigurationSettings* hello szolgáltatás konfigurációs (.cscfg) fájljában elemei.

    * **Tanúsítványok**  
        Módosítsa a hello tanúsítvány-ujjlenyomat, amely használatban van az SSL-titkosítást egy szerepkörhöz. toochange egy tanúsítványt, akkor először fel kell töltenie hello új tanúsítványt (a hello **tanúsítványok** lap). Frissítse a hello ujjlenyomat hello tanúsítvány karakterláncban hello beállítások jelennek meg.
4. A **operációs rendszer**, módosíthatja a hello operációsrendszer-családot vagy a szerepkörpéldányok verzióját, vagy válasszon **automatikus** tooenable automatikus frissítések az operációs rendszer aktuális verzióját hello. hello operációsrendszer-beállításokat alkalmazni tooweb és feldolgozói szerepkörök, de nem befolyásolják a virtuális gépek.
   
    A telepítés során összes szerepkör példánya telepítve van a hello legutóbbi operációs rendszer verziója, és hello operációs rendszerek alapértelmezés szerint automatikusan frissülnek. 
   
    Ha módosítania kell a cloud service toorun egy másik operációs rendszeren a kompatibilitási követelményei miatt a kódban, választhat egy operációsrendszer-családot és verziót. Ha úgy dönt, hogy egy adott operációsrendszer-verziót, automatikus operációsrendszer-frissítések hello felhőszolgáltatás fel vannak függesztve. Szüksége lesz a tooensure hello operációs rendszerek kapják a frissítéseket.
   
    Ha az összes, az alkalmazások hello legutóbbi operációs rendszer verziójával rendelkező kompatibilitási problémák megoldása engedélyezheti automatikus operációsrendszer-frissítések hello operációsrendszer-verziók beállítás túl**automatikus**. 
   
    ![Az operációs rendszer beállításai](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. toosave a konfigurációs beállításokat, és küldje le őket toohello szerepkörpéldányt beállítani, kattintson a **mentése**. (Kattintson **elvetése** toocancel hello módosításokat.) **Mentés** és **elvetése** toohello parancssáv kerülnek, a beállítás módosítása után.

## <a name="update-a-cloud-service-configuration-file"></a>Egy felhőalapú szolgáltatás konfigurációs fájljának frissítése
1. Töltse le a felhőalapú szolgáltatás konfigurációs fájlját (.cscfg) hello aktuális konfigurációjával. A hello **konfigurálása** oldalon felhőszolgáltatás hello **letöltése**. Kattintson a **mentése**, vagy kattintson a **Mentés másként** toosave hello fájlt.
2. Hello szolgáltatás konfigurációs fájl frissítése után feltöltése és hello konfigurációs frissítések alkalmazásához:
   
   1. A hello **konfigurálása** kattintson **feltöltése**.
      
       ![Konfigurációs feltöltése](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. A **konfigurációs fájl**, használjon **Tallózás** tooselect hello frissítése a .cscfg fájlban.
   3. Ha a felhőalapú szolgáltatás, amely csak egy példányban szerepköröket tartalmaz, válassza ki a hello **konfiguráció alkalmazásához, még akkor is, ha egy vagy több szerepkör egyetlen példányt tartalmaz** jelölőnégyzetet tooenable hello konfigurációs frissítések hello szerepkörök tooproceed.
      
       Kivéve, ha legalább két példánya minden definiálásához Azure nem garantálható, hogy legalább 99,95 % során frissíteni a konfigurációt a felhőalapú szolgáltatás rendelkezésre állását. További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).
   4. Kattintson a **OK** (jelölő). 

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).
* [Engedélyezze a távoli asztali kapcsolat egy szerepkör esetén az Azure Cloud Services csomag](cloud-services-role-enable-remote-desktop.md)
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md).

