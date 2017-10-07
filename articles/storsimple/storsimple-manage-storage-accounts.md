---
title: "aaaManage a StorSimple tárfiók |} Microsoft Docs"
description: "Ismerteti, hogyan használható hello StorSimple Manager konfigurálása lap tooadd, szerkesztése, törlése vagy elforgatás hello biztonsági kulcsok tárfiókok esetén."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 93207c40-e0eb-489e-8724-59fb94907081
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/29/2016
ms.author: v-sharos
ms.openlocfilehash: 78f408818ee8532dfaac445200048145547c987c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-storage-account"></a>A tárfiók hello StorSimple Manager szolgáltatás toomanage használata
## <a name="overview"></a>Áttekintés
Hello **konfigurálása** lapon megadja hello globális szolgáltatás minden paraméterét, amely a StorSimple Manager szolgáltatás hello hozhatók létre. Ezek a paraméterek lehet alkalmazott tooall hello eszközök toohello szolgáltatás csatlakoztatva, és tartalmazzák:

* Tárfiókok 
* Sávszélesség-sablonok 
* Hozzáférés-vezérlési rekordokat 

Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **konfigurálása** tooadd, szerkesztése vagy törlése storage-fiókok lapon vagy elforgatása hello biztonsági kulcsokat a tárfiók.

 ![Lapjának konfigurálása](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Storage-fiókok hello eszköz által használt tooaccess a tárfiók a felhőszolgáltatóval hello hitelesítő adatokat tartalmaz. A Microsoft Azure storage-fiókok ezek a hitelesítő adatok, például a hello fióknevet és hello elsődleges elérési kulcsot. 

A hello **konfigurálása** lapon, az előfizetés számlázási hello létrehozott fióknál hello a következő információkat tartalmazó táblázatos formában jelennek meg minden tároló:

* **Név** – hello egyedi név, hozzárendelése toohello fiók létrehozásakor.
* **Az SSL engedélyezett** – hogy hello SSL engedélyezve van, és eszköz-felhő kommunikációs hello biztonságos csatornán keresztül.
* **Által használt** – hello hello tárfiók kötetek számát.

hello leggyakoribb feladatokat a hello végrehajtható toostorage fiókoknak kapcsolódó **konfigurálása** lapon:

* Tárfiók hozzáadása 
* A storage-fiók szerkesztése 
* Tárfiók törlése 
* Kulcs elforgatási szögét storage-fiókok 

## <a name="types-of-storage-accounts"></a>A tárfiókok típusai
A StorSimple eszköz használható storage-fiókok három típusa van.

* **Automatikusan létrehozott tárfiókok** – hello nevet javasol, mivel a tárfióknak a típusa automatikusan előállított, hello szolgáltatás létrehozásakor. toolearn ezt a tárfiókot hogyan hozták létre, kapcsolatos további információkért lásd: [1. lépés: új szolgáltatás létrehozása](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) a [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-deployment-walkthrough.md). 
* **Hello szolgáltatás előfizetés tárfiókjai** – ezek a hello Azure storage-fiókok hello társított hello szolgáltatást, amely ugyanazt az előfizetést. toolearn ezekre a tárfiókokra létrehozását, kapcsolatos további információkért lásd: [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md). 
* **Storage-fiókok kívül hello szolgáltatás előfizetésének** – ezek a hello Azure storage-fiókok, amelyek nincsenek a szolgáltatáshoz tartozik, és valószínűleg már előtt hello szolgáltatás létrejött.

## <a name="add-a-storage-account"></a>Tárfiók hozzáadása
A storage-fiók is hozzáadhat egy egyedi megadásával rövid nevét és a hozzáférési hitelesítő adatok, amelyek kapcsolódó tárfiók toohello (felhőszolgáltatóval hello megadott). Akkor is hello secure sockets layer (SSL) mód toocreate engedélyezése az eszköz és hello felhő közötti hálózati kommunikációhoz biztonságos csatorna hello lehetőségét.

Egy adott felhőre szolgáltató több fiók is létrehozhat. Ne feledje azonban, a tárfiók létrehozása után nem módosítható hello felhőbeli szolgáltatás szolgáltatója.

Hello storage-fiók mentése folyamatban van, amíg a hello szolgáltatás megkísérli toocommunicate a felhőszolgáltatóval. hello hitelesítő adatokat és a megadott hello hozzáférés anyagot hitelesítése most. A storage-fiók jön létre, csak akkor, ha hello hitelesítés sikeres. Ha hello hitelesítés nem sikerül, majd a megfelelő hibaüzenet jelenik meg.

Azure-portálon létrehozott erőforrás-kezelő tárfiókok a StorSimple használata is támogatott. hello storage-fiókok nem jelennek meg a kijelölés hello legördülő listában toocreate egy kötettárolót közben az erőforrás-kezelő csak hello tárolási hello a klasszikus Azure portálon létrehozott fiókok fog megjelenni. Erőforrás-kezelő tárfiókok toobe hello eljárás tooadd az alábbiakban a storage-fiók használatával kell.

> [!NOTE]
> hello egy tárfiók hozzáadása eljárása alapján hello StorSimple szoftver verzióját használja. Lehet, hogy toofollow hello megfelelő eljárását a StorSimple-verzió.
> 
> 

[!INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[!INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>A storage-fiók szerkesztése
Szerkesztheti, melynek használatával a kötettároló tárfiók. Ha manuálisan szerkeszti, amelyek jelenleg használatban van, hello csak mező elérhető toomodify hello tárfiók hello a hozzáférési kulcsot. Adja meg a hello új tárelérési kulcs, és a frissített hello-beállítások mentése.

#### <a name="tooedit-a-storage-account"></a>a tárfiók tooedit
1. A lap a hello szolgáltatás követően, válassza ki a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, és kattintson **konfigurálása**.
2. Kattintson a **Tárfiók hozzáadása/szerkesztése**.
3. A hello **hozzáadása/szerkesztése Tárfiókok** párbeszédpanel:
   
   1. Hello legördülő listájában **Tárfiókok**, válasszon ki egy meglévő fiókot, hogy szeretné-e toomodify. Ez a hello szolgáltatás létrehozásakor automatikusan generált hello storage-fiókok is lehetnek.
   2. Ha szükséges, módosíthatja hello **SSL-mód engedélyezése** kijelölés.
   3. A fiók tárelérési kulcsok választható toorotate. Lásd: [elforgatási szögét tárfiókok kulcs](#key-rotation-of-storage-accounts) hogyan tooperform kulcs Elforgatás további információt.
   4. Kattintson a pipa ikonra hello ![pipa ikon](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) toosave hello beállításait. hello beállítások lesznek frissítve hello **konfigurálása** lap. Kattintson a **mentése** toosave hello újonnan beállításainak frissítése.
      
      ![A storage-fiók szerkesztése](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)

## <a name="delete-a-storage-account"></a>Tárfiók törlése
> [!IMPORTANT]
> A tárfiók törlése csak akkor, ha nem használják a kötettároló. A kötettároló tárfiók használja, ha először törölje az hello kötettároló, majd hello kapcsolódó tárfiók.
> 
> 

#### <a name="toodelete-a-storage-account"></a>a tárfiók toodelete
1. Hello StorSimple Manager szolgáltatás követően lapon jelölje be a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd **konfigurálása**.
2. Hello táblázatos listában tárfiókok hogy kívánja-e toodelete hello fiók mutasson.
3. A Törlés ikonra (**x**) hello szélsőséges jobb oldali oszlopban, hogy a tárfiók fog megjelenni. Kattintson a hello **x** ikon toodelete hello hitelesítő adatokat.
4. Amikor felszólítja a megerősítésre, kattintson **Igen** toocontinue a hello törlését. hello táblázatos felsorolása lesz frissített tooreflect hello módosításokat.

## <a name="key-rotation-of-storage-accounts"></a>Kulcs elforgatási szögét storage-fiókok
Biztonsági okokból kulcs Elforgatás feltétele gyakran adatközpontokban. 

> [!NOTE]
> a következő kulcs Elforgatás információkat és hello mechanizmusnak hello tooMicrosoft Azure-tárfiókok csak alkalmazni. Ha egy másik felhőszolgáltatóval használ, kezelheti ezt a szolgáltatót irányítópulton keresztül történő tárfiókkulcsok.
> 
> 

Minden Microsoft Azure-előfizetéssel társított tárfiókokat rendelkezhet. hello toothese fiókok hello előfizetés és minden tárfiók hozzáférési kulcsainak listázása határozza meg. 

Amikor létrehoz egy tárfiókot, a Microsoft Azure két 512 bites tárelérési kulcsot a hitelesítéshez használt hello tárfiókját állít elő. Két tárelérési kulcsot lehetővé teszi, hogy tooregenerate hello kulcsok megszakítás tooyour társzolgáltatás és toothat szolgáltatás nélkül. hello kulcs jelenleg használatban hello *elsődleges* kulcs és hello biztonsági mentési kulcsa hivatkozott tooas hello *másodlagos* kulcs. Ezek a két kulcsok egyikét kell megadni, ha a Microsoft Azure StorSimple eszköz éri el a felhő tárolási szolgáltató.

## <a name="what-is-key-rotation"></a>Mi az, hogy a kulcs Elforgatás?
Általában alkalmazások használják hello kulcsok tooaccess közül csak az adatokat. Egy bizonyos idő után akkor az alkalmazások vált át toousing hello második kulcs. Az alkalmazások toohello másodlagos kulcs váltottunk, miután kivonása hello első kulcsát, és ezután hozzon létre egy új kulcsot. Ezzel a módszerrel hello két kulcsok használata lehetővé teszi, hogy az alkalmazások hozzáférési toohello adatok állásidő nélkül.

hello tárfiókkulcsok mindig hello szolgáltatás titkosított formában tárolja. Azonban ezek állítható vissza hello StorSimple Manager szolgáltatás segítségével. hello szolgáltatás beszerezheti hello elsődleges kulcs, és a másodlagos kulcs az összes hello hello ugyanahhoz az előfizetéshez, többek között a hello tároló szolgáltatás, valamint az alapértelmezett tárfiók hello létrehozott fiókot létrehozott tárfiókok amikor hello StorSimple Manager szolgáltatás szolgáltatás létrehozása. hello StorSimple Manager szolgáltatás mindig beszerezni ezeket a kulcsokat hello a klasszikus Azure portálon, és a majd a titkosított módon történő tárolásához.

## <a name="rotation-workflow"></a>Elforgatás munkafolyamat
A Microsoft Azure rendszergazda újragenerálja vagy hello elsődleges vagy másodlagos kulcsot módosítása hello storage-fiók (a Microsoft Azure Storage szolgáltatás hello) keresztül közvetlenül elérésével. automatikusan hello StorSimple Manager szolgáltatás nem látja ezt a módosítást.

tooinform hello StorSimple Manager szolgáltatás hello változás, szüksége lesz tooaccess hello StorSimple Manager szolgáltatás hello tárfiók eléréséhez, és ezt követően szinkronizálja a hello elsődleges vagy másodlagos kulcsot (attól függően, melyiket megváltozott). hello szolgáltatás majd hello legutóbbi kulcs lekérdezi, hello kulcsokkal titkosítja, és elküldi a hello titkosított kulcs toohello eszköz.

#### <a name="toosynchronize-keys-for-storage-accounts-in-hello-same-subscription-as-hello-service-azure-only"></a>a storage-fiókok toosynchronize kulcsok hello ugyanahhoz az előfizetéshez hello szolgáltatást (csak Azure)
1. A hello **szolgáltatások** hello kattintson **konfigurálása** fülre.
2. Kattintson a **Tárfiók hozzáadása/szerkesztése**.
3. Az hello hello párbeszédpanelen a következő:
   
   1. Válassza ki a hello tárfiók hello kulccsal, amelyet az toosynchronize. hello tárfiókkulcsok titkosíthatók jelennek meg.
   2. A StorSimple Manager szolgáltatás hello be kell tooupdate hello kulcsot, amely korábban a Microsoft Azure Storage szolgáltatás hello megváltozott. Ha hello elsődleges elérési kulcsát (újragenerált) módosult, kattintson a **elsődleges kulcsának szinkronizálása**. Ha másodlagos kulcs hello módosult, kattintson a **másodlagos kulcsának szinkronizálása**.
      
      ![kulcsok szinkronizálása](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="toosynchronize-keys-for-storage-accounts-outside-of-hello-service-subscription"></a>storage-fiókok kívül hello szolgáltatás előfizetésének toosynchronize kulcsok
1. A hello **szolgáltatások** hello kattintson **konfigurálása** fülre.
2. Kattintson a **Tárfiók hozzáadása/szerkesztése**.
3. Az hello hello párbeszédpanelen a következő:
   
   1. Jelölje ki a hello tárolási fiókját hello elérési kulcsot, amelyet az tooupdate.
   2. Tooupdate hello tárelérési kulcs a StorSimple Manager szolgáltatás hello kell. Ebben az esetben hello tárelérési kulcs látható. Adjon meg új kulcs hello hello **Tárfiók_elérési_kulcsa**y mezőbe. 
   3. Mentse a módosításokat. A tárfiók hozzáférési kulcsának most frissíteni kell.

## <a name="next-steps"></a>Következő lépések
* További információ [StorSimple biztonsági](storsimple-security.md).
* További információ [használatával hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

