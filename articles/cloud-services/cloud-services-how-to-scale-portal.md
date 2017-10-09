---
title: "aaaAuto a felhőalapú szolgáltatások méretezéséhez hello portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello portál tooconfigure automatikus skálázási szabályok felhő szerepkör-szolgáltatás webes vagy feldolgozói szerepkör az Azure-ban."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 265f4c8ec5e1ec2f85585df25f18cd0d0c9946a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-auto-scaling-for-a-cloud-service-in-hello-portal"></a>Hogyan tooconfigure automatikus skálázás egy felhőalapú szolgáltatás hello portálon
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-scale-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-scale.md)

Egy felhőalapú szolgáltatás feldolgozói szerepkör terjedő skálán bejövő vagy kimenő műveletet kiváltó feltételek állíthat be. hello szerepkör hello feltételei hello Processzor, lemez vagy hálózati terheléselosztási hello szerepkör alapulhatnak. Egy feltétel alapján néhány más Azure-erőforrás az Ön előfizetéséhez rendelve egy üzenet várólista vagy hello metrikája állíthat be.

> [!NOTE]
> Ez a cikk a felhőalapú szolgáltatás webes és feldolgozói szerepkörök összpontosít. A virtuális gép (klasszikus) közvetlenül létrehozásakor tárolja egy felhőalapú szolgáltatás. A normál virtuális gépek méretezheti társítja azt egy [rendelkezésre állási csoport](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) és manuálisan kapcsolja be és ki.

## <a name="considerations"></a>Megfontolandó szempontok
A következő információkat az alkalmazás skálázás konfigurálása előtt hello kell figyelembe vennie:

* Skálázás alapvető használati van hatással.

    Nagyobb szerepkörpéldányokat több magot használja. Az előfizetéshez tartozó méretezhető hello vonatkozó felső korlátját: belül csak egy alkalmazást. Tegyük fel, hogy előfizetéséhez maximális hossza 20 magok. Ha egy alkalmazás két közepes méretű felhőszolgáltatások (4 mag összesen) futtatja, csak legfeljebb más felhőalapú szolgáltatás telepítések az előfizetésében hello fennmaradó 16 maggal által. Méretek kapcsolatos további információkért lásd: [Felhőszolgáltatások méretével](cloud-services-sizes-specs.md).

* Méretezheti a várólista állapotüzenet küszöbértéke alapján. További információ toouse sorok, lásd: [hogyan toouse hello Queue Storage szolgáltatás](../storage/queues/storage-dotnet-how-to-use-queues.md).

* További erőforrások, az Ön előfizetéséhez rendelve is méretezheti.

* tooenable magas rendelkezésre állás az alkalmazás, akkor győződjön meg arról, hogy két vagy több szerepkör osztályt telepítették. További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).


## <a name="where-scale-is-located"></a>Ahol a méretezési
Miután kiválasztotta a felhőalapú szolgáltatás, rendelkeznie kell hello cloud service panelen látható.

1. Hello cloud service panelen, a hello **szerepkörök és példányok** csempe, jelölje be hello neve hello felhőalapú szolgáltatás.   
   **FONTOS**: Győződjön meg arról, hogy tooclick hello felhő szerepkör-szolgáltatás, nem hello szerepkörpéldányt, amely hello szerepkör alatt.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Jelölje be hello **méretezési** csempére.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatikus méretezése
A szerepkörhöz tartozó skálázási beállítások vagy két mód konfigurálható **manuális** vagy **automatikus**. Manuális módon teheti meg, beállíthatja a hello abszolút példányainak számát. Automatikus azonban lehetővé teszi a tooset meghatározó szabályok és hogyan milyen sokkal meg kell méretezni.

Set hello **szerint** beállítás túl**ütemezés és a teljesítmény szabályok**.

![Cloud services skálázási beállításokat a profil és a szabály](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Egy meglévő profilt.
2. Vegyen fel egy szülő hello-profil.
3. Egy másik profil hozzáadásához.

Válassza ki **profil hozzáadása**. hello-profil határozza meg, melyik módban toouse kívánt hello bővített: **mindig**, **ismétlődési**, **rögzített dátum**.

Miután konfigurálta a hello-profil és a szabályokat, válassza ki a hello **mentése** hello felső ikonra.

#### <a name="profile"></a>Profil
hello-profil beállítja a minimális és maximális példányainak hello méretezhető, illetve is a tartomány használata aktív.

* **Mindig**

    Ez a tartomány elérhető példányok mindig készüljön.  

    ![A felhőalapú szolgáltatás, amely mindig](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Ismétlődés**

    Válasszon olyan hello hét tooscale napon.

    ![Cloud service méretezéssel ismétlődési ütemezés](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Rögzített dátum**

    Rögzített dátum tartomány tooscale hello szerepkör.

    ![CLoud service méretezési fix dátummal](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Miután konfigurálta a hello-profil, válassza ki a hello **OK** gomb hello aljához hello-profil panelje.

#### <a name="rule"></a>Szabály
Szabályok tooa profilt ad hozzá, és hello méretezési kiváltó feltétel képviseli.

hello szabály eseményindító metrika hello felhőszolgáltatás (CPU-használat, lemezre tevékenységi vagy hálózati tevékenységek) alapuló toowhich feltételes értéket adhat hozzá. Továbbá akkor is hello eseményindító néhány más Azure-erőforrás az Ön előfizetéséhez rendelve egy üzenet várólista vagy hello metrikája alapján.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Miután konfigurálta a hello szabály, válassza ki a hello **OK** hello hello szabály panel alsó részén gombra.

## <a name="back-toomanual-scale"></a>Biztonsági toomanual méretezési
Keresse meg a toohello [beállítások méretezése](#where-scale-is-located) és set hello **bővítse a** beállítás túl**manuálisan megadott példányszám**.

![Cloud services skálázási beállításokat a profil és a szabály](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Ez a beállítás eltávolítja az automatikus skálázás hello szerepkörből, és állíthat be hello példányszám közvetlenül.

1. hello méretezési (Manuális vagy automatikus) lehetőséget.
2. Egy szerepkör példány csúszkát tooset hello példányok tooscale számára.
3. Hello szerepkör tooscale a példányai.

Miután konfigurálta a hello skálázási beállításokat, válassza ki a hello **mentése** hello felső ikon.
