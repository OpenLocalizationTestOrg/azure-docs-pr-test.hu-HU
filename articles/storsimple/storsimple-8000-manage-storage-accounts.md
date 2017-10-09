---
title: "a StorSimple storage-fiók hitelesítő adatait, a Microsoft Azure StorSimple 8000 sorozat eszközeire aaaManage |} Microsoft Docs"
description: "Ismerteti, hogyan használható hello StorSimple Device Manager konfigurálása lap tooadd, szerkesztése, törlése vagy elforgatás hello biztonsági kulcsok tárfiókok esetén."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 132ee46509b39db4d1b97b0f1077800a253e8da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-storage-account-credentials"></a>Hello StorSimple Device Manager szolgáltatás toomanage a tárfiók hitelesítő adatainak használata

## <a name="overview"></a>Áttekintés

Hello **konfigurációs** hello StorSimple Device Manager szolgáltatás létrehozott összes hello globális szolgáltatás paraméterek szakaszban hello StorSimple Device Manager szolgáltatás a panelen. Ezek a paraméterek lehet alkalmazott tooall hello eszközök toohello szolgáltatás csatlakoztatva, és tartalmazzák:

* Tárfiók hitelesítő adatai
* Sávszélesség-sablonok 
* Hozzáférés-vezérlési rekordokat 

Ez az oktatóanyag azt ismerteti, hogyan tooadd, szerkesztése, törlése a tárfiók hitelesítő adatainak vagy elforgatása hello biztonsági kulcsokat a tárfiók.

 ![Tárfiók hitelesítő adatainak listája](./media/storsimple-8000-manage-storage-accounts/createnewstorageacct6.png)  

Storage-fiókok hello hitelesítő adatokat, amelyeket a StorSimple eszköz által használt tooaccess a tárfiók a felhőszolgáltatóval hello tartalmaz. A Microsoft Azure storage-fiókok ezek a hitelesítő adatok, például a hello fióknevet és hello elsődleges elérési kulcsot. 

A hello **Tárfiók hitelesítő adatainak** panel, az előfizetés számlázási hello létrehozott fióknál hello a következő információkat tartalmazó táblázatos formában jelennek meg minden tároló:

* **Név** – hello egyedi név, hozzárendelése toohello fiók létrehozásakor.
* **Az SSL engedélyezett** – hogy hello SSL engedélyezve van, és eszköz-felhő kommunikációs hello biztonságos csatornán keresztül.
* **Által használt** – hello hello tárfiók kötetek számát.

hello leggyakoribb feladatokat kapcsolódó toostorage fiókok végrehajtható a következők:

* Tárfiók hozzáadása 
* A storage-fiók szerkesztése 
* Tárfiók törlése 
* Kulcs elforgatási szögét storage-fiókok 

## <a name="types-of-storage-accounts"></a>A tárfiókok típusai

A StorSimple eszköz használható storage-fiókok három típusa van.

* **Automatikusan létrehozott tárfiókok** – hello nevet javasol, mivel a tárfióknak a típusa automatikusan előállított, hello szolgáltatás létrehozásakor. toolearn ezt a tárfiókot hogyan hozták létre, kapcsolatos további információkért lásd: [1. lépés: új szolgáltatás létrehozása](storsimple-8000-deployment-walkthrough-u2.md#step-1-create-a-new-service) a [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md). 
* **Hello szolgáltatás előfizetés tárfiókjai** – ezek a hello Azure storage-fiókok hello társított hello szolgáltatást, amely ugyanazt az előfizetést. toolearn ezekre a tárfiókokra létrehozását, kapcsolatos további információkért lásd: [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md). 
* **Storage-fiókok kívül hello szolgáltatás előfizetésének** – ezek a hello Azure storage-fiókok, amelyek nincsenek a szolgáltatáshoz tartozik, és valószínűleg már előtt hello szolgáltatás létrejött.

## <a name="add-a-storage-account"></a>Tárfiók hozzáadása

A storage-fiók is hozzáadhat egy egyedi megadásával rövid nevét és a hozzáférési hitelesítő adatok, amelyek kapcsolódó tárfiók toohello (felhőszolgáltatóval hello megadott). Akkor is hello secure sockets layer (SSL) mód toocreate engedélyezése az eszköz és hello felhő közötti hálózati kommunikációhoz biztonságos csatorna hello lehetőségét.

Egy adott felhőre szolgáltató több fiók is létrehozhat. Ne feledje azonban, a tárfiók létrehozása után nem módosítható hello felhőbeli szolgáltatás szolgáltatója.

Hello storage-fiók mentése folyamatban van, amíg a hello szolgáltatás megkísérli toocommunicate a felhőszolgáltatóval. hello hitelesítő adatokat és a megadott hello hozzáférés anyagot hitelesítése most. A storage-fiók jön létre, csak akkor, ha hello hitelesítés sikeres. Ha hello hitelesítés nem sikerül, majd a megfelelő hibaüzenet jelenik meg.

A következő eljárások tooadd az Azure storage-fiók hitelesítő adatainak hello használata:

* a tárolási fiók hitelesítő adatait, amelynek tooadd hello hello Eszközkezelő szolgáltatásként azonos Azure-előfizetés
* egy Azure storage fiók hitelesítő adatait, amely kívül hello Eszközkezelő szolgáltatás előfizetésének tooadd

[!INCLUDE [add-a-storage-account-update2](../../includes/storsimple-8000-configure-new-storage-account-u2.md)]

#### <a name="tooadd-an-azure-storage-account-credential-outside-of-hello-storsimple-device-manager-service-subscription"></a>egy Azure storage fiók hitelesítő adatait, kívül hello StorSimple Device Manager szolgáltatási előfizetés szükséges tooadd

1. Keresse meg a tooyour StorSimple Device Manager szolgáltatás, válassza ki, és kattintson rá duplán. Ekkor megnyílik a hello **áttekintése** panelen.
2. Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz. A parancs megjeleníti minden meglévő tárfiók hitelesítő adatainak társított hello StorSimple Device Manager szolgáltatást.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. A hello **adja hozzá a tárolási fiók hitelesítő adatait** panelen a következő hello:
   
    1. A **előfizetés**, jelölje be **más**.
   
    2. Adja meg az Azure storage-fiók hitelesítő adatait hello nevét.
   
    3. A hello **tárfiók_elérési_kulcsa** szövegmezőben adjon meg hello elsődleges elérési kulcsát az Azure storage-fiók hitelesítő adatait. a kulcs tooget nyissa toohello Azure Storage szolgáltatás, válassza ki a tárolási fiók hitelesítő adatait, és kattintson **fiók kulcsok kezelése**. Ekkor átmásolhatja hello elsődleges elérési kulcsát.
   
    4. tooenable SSL, kattintson a hello **engedélyezése** gomb toocreate biztonságos csatornát a StorSimple Device Manager szolgáltatás és hello felhő közötti hálózati kommunikációhoz. Kattintson a hello **letiltása** gomb csak akkor, ha magánfelhőben tevékenykedik.
   
    5. Kattintson az **Add** (Hozzáadás) parancsra. Hello tárolási fiók hitelesítő adatait sikeres létrehozása után értesítést kap.

5. az újonnan létrehozott hello tárolási fiók hitelesítő adatait hello konfigurálása Eszközkezelő StorSimple szolgáltatás panel alatt jelenik meg **Tárfiók hitelesítő adatainak**.
   


## <a name="edit-a-storage-account"></a>A storage-fiók szerkesztése

Szerkesztheti, melynek használatával a kötettároló tárfiók. Ha manuálisan szerkeszti, amelyek jelenleg használatban van, hello csak mező elérhető toomodify hello tárfiók hello a hozzáférési kulcsot. Adja meg a hello új tárelérési kulcs, és a frissített hello-beállítások mentése.

#### <a name="tooedit-a-storage-account"></a>a tárfiók tooedit

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást. A hello **konfigurációs** kattintson **Tárfiók hitelesítő adatainak**.

    ![Tárfiók hitelesítő adatai](./media/storsimple-8000-manage-storage-accounts/editstorageacct1.png)

2. A hello **Tárfiók hitelesítő adatainak** panelen tárfiók hitelesítő adatait, jelölje be azon hello listájáról, és kattintson egy tooedit kívánja hello. 

3. Módosíthatja a hello **SSL engedélyezése** kijelölés. Is **több...**  majd **szinkronizálási hozzáférési kulcs toorotate** a fiók tárelérési kulcsok. Nyissa meg túl[elforgatási szögét tárfiókok kulcs](#key-rotation-of-storage-accounts) hogyan tooperform kulcs Elforgatás olvashat. Hello beállítások módosítása után kattintson **mentése**. 

    ![Mentse a módosított tárfiók hitelesítő adatait](./media/storsimple-8000-manage-storage-accounts/editstorageacct3.png)

4. Ha a rendszer megerősítést kér, kattintson az **Igen** gombra. 

    ![Módosítások megerősítése](./media/storsimple-8000-manage-storage-accounts/editstorageacct4.png)

hello beállítások módosítása és a tárfiók mentve. 

## <a name="delete-a-storage-account"></a>Tárfiók törlése

> [!IMPORTANT]
> A tárfiók törlése csak akkor, ha nem használják a kötettároló. A kötettároló tárfiók használja, ha először törölje az hello kötettároló, majd hello kapcsolódó tárfiók.

#### <a name="toodelete-a-storage-account"></a>a tárfiók toodelete

1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást. A hello **konfigurációs** kattintson **Tárfiók hitelesítő adatainak**.

2. Hello táblázatos listában tárfiókok hogy kívánja-e toodelete hello fiók mutasson. Kattintson a jobb gombbal a tooinvoke hello helyi menüt, és kattintson a **törlése**.

    ![Tárfiók hitelesítő adatainak törlése](./media/storsimple-8000-manage-storage-accounts/deletestorageacct1.png)

3. Amikor felszólítja a megerősítésre, kattintson **Igen** toocontinue a hello törlését. hello táblázatos felsorolása lesz frissített tooreflect hello módosításokat.

    ![Törlés megerősítése](./media/storsimple-8000-manage-storage-accounts/deletestorageacct2.png)

## <a name="key-rotation-of-storage-accounts"></a>Kulcs elforgatási szögét storage-fiókok

Biztonsági okokból kulcs Elforgatás feltétele gyakran adatközpontokban. Minden Microsoft Azure-előfizetéssel társított tárfiókokat rendelkezhet. hello toothese fiókok hello előfizetés és minden tárfiók hozzáférési kulcsainak listázása határozza meg. 

Amikor létrehoz egy tárfiókot, a Microsoft Azure két 512 bites tárelérési kulcsot a hitelesítéshez használt hello tárfiókját állít elő. Két tárelérési kulcsot lehetővé teszi, hogy tooregenerate hello kulcsok megszakítás tooyour társzolgáltatás és toothat szolgáltatás nélkül. hello kulcs jelenleg használatban hello *elsődleges* kulcs és hello biztonsági mentési kulcsa hivatkozott tooas hello *másodlagos* kulcs. Ezek a két kulcsok egyikét kell megadni, ha a Microsoft Azure StorSimple eszköz éri el a felhő tárolási szolgáltató.

## <a name="what-is-key-rotation"></a>Mi az, hogy a kulcs Elforgatás?

Általában alkalmazások használják hello kulcsok tooaccess közül csak az adatokat. Egy bizonyos idő után akkor az alkalmazások vált át toousing hello második kulcs. Az alkalmazások toohello másodlagos kulcs váltottunk, miután kivonása hello első kulcsát, és ezután hozzon létre egy új kulcsot. Ezzel a módszerrel hello két kulcsok használata lehetővé teszi, hogy az alkalmazások hozzáférési toohello adatok állásidő nélkül.

hello tárfiókkulcsok mindig hello szolgáltatás titkosított formában tárolja. Azonban ezek állítható vissza keresztül hello StorSimple Device Manager szolgáltatást. hello szolgáltatás beszerezheti hello elsődleges kulcs, és a másodlagos kulcs az összes hello hello ugyanahhoz az előfizetéshez, többek között a hello tároló szolgáltatás, valamint az alapértelmezett tárfiók hello létrehozott fiókot létrehozott tárfiókok amikor hello StorSimple Device Manager szolgáltatás létrehozása. hello StorSimple Device Manager szolgáltatás mindig beszerezni ezeket a kulcsokat hello a klasszikus Azure portálon, és a majd a titkosított módon történő tárolásához.

## <a name="rotation-workflow"></a>Elforgatás munkafolyamat

A Microsoft Azure rendszergazda újragenerálja vagy hello elsődleges vagy másodlagos kulcsot módosítása hello storage-fiók (a Microsoft Azure Storage szolgáltatás hello) keresztül közvetlenül elérésével. automatikusan hello StorSimple Device Manager szolgáltatás nem látja ezt a módosítást.

tooinform hello StorSimple Device Manager szolgáltatás hello változás, szüksége lesz tooaccess hello StorSimple Device Manager szolgáltatás hello tárfiók eléréséhez, és ezt követően szinkronizálja a hello elsődleges vagy másodlagos kulcsot (attól függően, melyiket megváltozott). hello szolgáltatás majd hello legutóbbi kulcs lekérdezi, hello kulcsokkal titkosítja, és elküldi a hello titkosított kulcs toohello eszköz.

#### <a name="toosynchronize-keys-for-storage-accounts-in-hello-same-subscription-as-hello-service"></a>a storage-fiókok toosynchronize kulcsok hello hello szolgáltatást ugyanahhoz az előfizetéshez 
1. Nyissa meg tooyour StorSimple Device Manager szolgáltatást. A hello **konfigurációs** kattintson **Tárfiók hitelesítő adatainak**.
2. A storage-fiókok listája táblázatos hello, kattintson a megjeleníteni kívánt toomodify egy hello. 

    ![kulcsok szinkronizálása](./media/storsimple-8000-manage-storage-accounts/syncaccesskey1.png)

3. Kattintson a **... További** majd **szinkronizálási hozzáférési kulcs toorotate**.   

    ![kulcsok szinkronizálása](./media/storsimple-8000-manage-storage-accounts/syncaccesskey2.png)

4. A StorSimple eszköz Manager szolgáltatás hello be kell tooupdate hello kulcsot, amely korábban a Microsoft Azure Storage szolgáltatás hello megváltozott. Ha hello elsődleges elérési kulcsát (újragenerált) módosult, válassza ki a **elsődleges** kulcs. Ha másodlagos kulcs hello módosult, válassza ki a **másodlagos** kulcs. Kattintson a **szinkronizálási kulcs**.
      
      ![kulcsok szinkronizálása](./media/storsimple-8000-manage-storage-accounts/syncaccesskey3.png)

Értesítést fog kapni után hello kulcs sikeresen sycnhronized.

#### <a name="toosynchronize-keys-for-storage-accounts-outside-of-hello-service-subscription"></a>storage-fiókok kívül hello szolgáltatás előfizetésének toosynchronize kulcsok
1. A hello **szolgáltatások** hello kattintson **konfigurálása** fülre.
2. Kattintson a **Tárfiók hozzáadása/szerkesztése**.
3. Az hello hello párbeszédpanelen a következő:
   
   1. Jelölje ki a hello tárolási fiókját hello elérési kulcsot, amelyet az tooupdate.
   2. Szüksége lesz tooupdate hello tárelérési kulcs hello StorSimple Device Manager szolgáltatás található. Ebben az esetben hello tárelérési kulcs látható. Adjon meg új kulcs hello hello **Tárfiók_elérési_kulcsa** mezőbe. 
   3. Mentse a módosításokat. A tárfiók hozzáférési kulcsának most frissíteni kell.

## <a name="next-steps"></a>Következő lépések
* További információ [StorSimple biztonsági](storsimple-8000-security.md).
* További információ [használatával hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

