---
title: "aaaAuto felhőalapú szolgáltatásokat méretezni hello klasszikus portál |} Microsoft Docs"
description: "(klasszikus) Ismerje meg, hogyan toouse hello klasszikus portál tooconfigure automatikus skálázási szabályok felhő szerepkör-szolgáltatás webes vagy feldolgozói szerepkör az Azure-ban."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: eb167d70-4eba-42a4-b157-d8d0688abf4b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: ddb5816d4d22192c6d2f51d7508e390779742078
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-classic-portal"></a>Hogyan tooconfigure automatikus skálázás egy felhőalapú szolgáltatás hello klasszikus portál
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-scale.md)

A klasszikus Azure portálon hello hello méretezési lapján konfigurálhatja a webes szerepkör vagy a feldolgozói szerepkör automatikus méretezési beállításainak. Beállíthatja azt is megteheti, manuális skálázás automatikus skálázás szabályalapú helyett.

> [!NOTE]
> Ez a cikk a felhőalapú szolgáltatás webes és feldolgozói szerepkörök összpontosít. A virtuális gép (klasszikus) közvetlenül létrehozásakor tárolja egy felhőalapú szolgáltatás. Az információk érvényes toothese típusú virtuális gépet. Skálázás egy rendelkezésre állási csoportot a virtuális gépek csak leáll őket be- és kikapcsolását konfigurálnia hello skálázási szabályok alapján. További információ a virtuális gépek és a rendelkezésre állási csoportok: [kezelése hello rendelkezésre állású virtuális gépek](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

A következő információkat az alkalmazás skálázás konfigurálása előtt hello kell figyelembe vennie:

* Skálázás alapvető használati van hatással.

    Nagyobb szerepkörpéldányokat több magot használja. Az előfizetéshez tartozó méretezhető hello vonatkozó felső korlátját: belül csak egy alkalmazást. Tegyük fel, hogy előfizetéséhez maximális hossza 20 magok. Ha egy alkalmazás két közepes méretű felhőszolgáltatások (4 mag összesen) futtatja, csak legfeljebb más felhőalapú szolgáltatás telepítések az előfizetésében hello fennmaradó 16 maggal által. Méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások méretével](cloud-services-sizes-specs.md).

* Létrehoz egy sort kell, és társíthatja egy szerepkör előtt az alkalmazást egy állapotüzenet küszöbértéke a alapján méretezheti. További információkért lásd: [hogyan toouse hello Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md).

* Csatolt tooyour erőforrásokhoz is méretezhető felhőalapú szolgáltatás. Erőforrások csatolása kapcsolatos további információkért lásd: [hogyan: hivatkozás erőforrás tooa felhőszolgáltatás](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

* tooenable magas rendelkezésre állás az alkalmazás, akkor győződjön meg arról, hogy két vagy több szerepkör osztályt telepítették. További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).

## <a name="schedule-scaling"></a>Ütemezés skálázás
Alapértelmezés szerint az összes szerepkör ne hajtsa végre egy adott ütemezés. Ezért a beállításokat módosítani alkalmazni tooall ideje és minden nap hello év során. Ha azt szeretné, beállíthatja manuális vagy automatikus skálázás hello módok a következő egyike:

* Hétköznapokon
* Hétvégéket
* Hét éjszakák
* Hét reggelente
* Konkrét dátumokat
* Adott dátumtartományok

hello ütemezési beállítás konfigurálva van a hello [a klasszikus Azure portálon](https://manage.windowsazure.com/) a hello  
**A felhőalapú szolgáltatások** > **\[a felhőalapú szolgáltatás\]** > **méretezési** > **\[üzemi és átmeneti\]**  lap.

Kattintson a hello **ütemezés beállítása** gomb szerepkörönként toochange keresi.

![A felhőalapú szolgáltatás ütemezés alapján automatikus skálázás][scale_schedules]

## <a name="manual-scale"></a>Manuális méretezési
A hello **méretezési** lapon, akkor manuálisan növelhető és csökkenthető futó példány egy felhőalapú szolgáltatás hello száma. Ez a beállítás minden létrehozott ütemezést vagy tooall idő van konfigurálva, ha nem hozott létre ütemezés.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.
   
   > [!TIP]
   > Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.

2. Kattintson a **méretezési**.
3. Válassza ki a kívánt skálázás beállításai toochange hello ütemezés. *Nem ütemezett időpontokban* hello alapértelmezett van, ha még nem definiált ütemezések.
4. Hello található **a metrika a skála** válassza ki azt **NONE**. Ez a beállítás akkor hello alapértelmezett összes szerepköre tekintetében.
5. Hello felhőalapú szolgáltatás minden szerepkörhöz a csúszkán példányok toouse hello számának módosítása.
   
    ![Manuálisan méretezhető, a felhő szerepkör-szolgáltatás][manual_scale]
   
    Ha több példány van szüksége, szükség lehet a toochange hello [a felhő virtuálisgép-méret](cloud-services-sizes-specs.md).
6. Kattintson a **Save** (Mentés) gombra.  
   Szerepkörpéldányokat hozzáadásakor vagy eltávolításakor a kiválasztott beállítások alapján.

> [!TIP]
> Amikor látja ![][tip_icon] áthelyezése az egér tooit és kaphat arról, hogy milyen egy adott beállítás.

## <a name="automatic-scale---cpu"></a>Automatikus méretezési - Processzor
Ebben a módban arányosan, ha hello átlagos CPU-használat aránya a megadott küszöbérték alatti vagy feletti kerül. Szerepkörpéldányokat létrehozott vagy törölt, ha ez történik.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.
   
   > [!TIP]
   > Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.

2. Kattintson a **méretezési**.
3. Válassza ki a kívánt skálázás beállításai toochange hello ütemezés. *Nem ütemezett időpontokban* hello alapértelmezett van, ha még nem definiált ütemezések.
4. Hello található **a metrika a skála** válassza ki azt **CPU**.
5. Most minimális és maximális számos szerepkörök példányai, hello cél CPU-használat (a skálától beállítható tootrigger), és hogy hány példánya tooscale felfelé és lefelé által konfigurálható.

![Bővítse a cpu-terhelés a felhőalapú szolgáltatás szerepkör][cpu_scale]

> [!TIP]
> Amikor látja ![][tip_icon] áthelyezése az egér tooit és kaphat arról, hogy milyen egy adott beállítás.

## <a name="automatic-scale---queue"></a>Automatikus méretezési - várólista
Ebben a módban automatikusan méretezi, ha a várólistán lévő üzenetek hello száma a megadott küszöbérték alatti vagy feletti kerül. Szerepkörpéldányokat létrehozott vagy törölt, ha ez történik.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.
   
   > [!TIP]
   > Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.

2. Kattintson a **méretezési**.
3. Hello található **a metrika a skála** válassza ki azt **VÁRÓLISTA**.
4. Most szerepkörök példányai, hello várólista és az egyes példányok és által növekszik és csökken hány példányok tooscale várólista-üzenetek tooprocess száma minimális és maximális számos is konfigurálhat.

![Bővítse a felhőalapú szolgáltatás szerepkör üzenet-várólista][queue_scale]

> [!TIP]
> Amikor látja ![][tip_icon] áthelyezése az egér tooit és kaphat arról, hogy milyen egy adott beállítás.

## <a name="scale-linked-resources"></a>Méretezhető kapcsolt erőforrásokban
Gyakran szerepkör méretezéséhez esetén hasznos tooscale hello adatbázis hello alkalmazás által is használt. Ha hello adatbázis toohello felhőalapú szolgáltatás, hello-beállítások az adott erőforrás skálázás hello megfelelő hivatkozásra kattintva érheti el.

1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Felhőszolgáltatások**, majd hello cloud service tooopen hello irányítópult hello nevére.
   
   > [!TIP]
   > Ha nem látja a felhőalapú szolgáltatás, szükség lehet a toochange **éles** túl**átmeneti** vagy fordítva.

2. Kattintson a **méretezési**.
3. Hello található **kapcsolódó erőforrások** szakaszt, és kattintson **az adatbázis kezelésére**.
   
   > [!NOTE]
   > Ha nem látja a **kapcsolódó erőforrások** szakaszban, valószínűleg nem rendelkezik társított erőforrásokat.

![][linked_resource]

[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
