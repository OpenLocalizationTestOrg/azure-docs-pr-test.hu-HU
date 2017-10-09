---
title: "aaaManage StorSimple virtuális tömb tárfiók hitelesítő adatainak |} Microsoft Docs"
description: "Ismerteti, hogyan használható hello StorSimple Device Manager konfigurálása lap tooadd, szerkesztése, törlése vagy elforgatás hello biztonsági kulcsok a tárfiók hitelesítő adatainak hello StorSimple virtuális tömb társítva."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 234bf8bb-d5fe-40be-9d25-721d7482bc3b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: 22a0341eae0b89020065be4dbfaae77999f8be0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-toomanage-storage-account-credentials-for-storsimple-virtual-array"></a>StorSimple az Eszközkezelő toomanage tárfiók hitelesítő adatainak a StorSimple virtuális tömb

## <a name="overview"></a>Áttekintés
Hello **konfigurációs** hello StorSimple Device Manager szolgáltatás panelre a StorSimple virtuális tömb szakaszban hello globális paramétereket, amelyek a StorSimple Manager szolgáltatás hello hozhatók létre. Ezek a paraméterek lehet alkalmazott tooall hello eszközök toohello szolgáltatás csatlakoztatva, és tartalmazzák:

* Tárfiók hitelesítő adatai
* Hozzáférés-vezérlési rekordokat
  
  ![Eszközkezelő szolgáltatás irányítópult](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccts-dashboard.png)  

Ez az oktatóanyag azt ismerteti, hogyan hozzáadásához, szerkesztéséhez vagy törléséhez a tárfiók hitelesítő adatainak a StorSimple virtuális tömbhöz. Ebben az oktatóanyagban hello információk csak érvényes toohello StorSimple virtuális tömb. Hogyan toomanage tárfiókok 8000 sorozat vonatkozó információkért lásd: [használata hello StorSimple Manager szolgáltatás toomanage a tárfiók](storsimple-manage-storage-accounts.md).

A tárfiók hitelesítő adatok eszköz hello hello hitelesítő adatokat tartalmaz a tárfiók használ tooaccess a felhőszolgáltatóval. A Microsoft Azure storage-fiókok ezek a hitelesítő adatok, például a hello fióknevet és hello elsődleges elérési kulcsot.

A hello **Tárfiók hitelesítő adatainak** panel, az összes tárfiók hitelesítő adatokat, amelyek létrejönnek az előfizetés számlázási hello hello a következő információkat tartalmazó táblázatos formában jelennek meg:

* **Név** – hello egyedi név, hozzárendelése toohello fiók létrehozásakor.
* **Az SSL engedélyezett** – hogy hello SSL engedélyezve van, és eszköz-felhő kommunikációs hello biztonságos csatornán keresztül.
  
  ![A konfigurációs szakasz](./media/storsimple-virtual-array-manage-storage-accounts/ova-storageaccountcredentials-blade.png)

hello leggyakoribb feladatokat kapcsolódó toostorage fiók hitelesítő adatait, amely a hello végrehajtható **Tárfiók hitelesítő adatainak** panel vannak:

* Tárfiók hitelesítő adatainak hozzáadása
* A tárolási fiók hitelesítő adatok szerkesztése
* Törölje a tárolási fiók hitelesítő adatait

## <a name="types-of-storage-account-credentials"></a>Tárfiók hitelesítő adatainak típusa
Tárfiók hitelesítő adatait, amely a StorSimple eszköz használható három típusa van.

* **Automatikusan létrehozott tárfiók hitelesítő adatainak** – hello nevet javasol, mivel a tárolási fiók hitelesítő adatait az ilyen típusú automatikusan előállított, hello szolgáltatás létrehozásakor. További információ a tárolási fiók hitelesítő adatait hogyan hozták létre, toolearn lásd: [hozzon létre egy új](storsimple-virtual-array-manage-service.md#create-a-service).
* **tárfiók hitelesítő adatainak hello szolgáltatás előfizetésben** – hello Azure storage-fiók olyan hitelesítő adatokat, amelyek társított hello hello szolgáltatást, amely ugyanazt az előfizetést. toolearn a tárfiók hitelesítő adatainak létrehozását, kapcsolatos további információkért lásd: [kapcsolatos Azure Storage-fiókok](../storage/common/storage-create-storage-account.md).
* **tárfiók hitelesítő adatainak kívül hello szolgáltatás előfizetésének** – ezek a hello Azure tárolási fiók hitelesítő adatait, amelyek nincsenek a szolgáltatáshoz tartozik, és valószínűleg már előtt hello szolgáltatás létrejött.

## <a name="add-a-storage-account-credential"></a>Tárfiók hitelesítő adatainak hozzáadása
Egy egyedi megadásával is hozzáadhat a tárolási fiók hitelesítő adat tooyour StorSimple Device Manager szolgáltatáskonfiguráció rövid nevét és a hozzáférési hitelesítő adatok, amelyek kapcsolódó toohello tárfiók. Akkor is hello secure sockets layer (SSL) mód toocreate engedélyezése az eszköz és hello felhő közötti hálózati kommunikációhoz biztonságos csatorna hello lehetőségét.

Egy adott felhőre szolgáltató több fiók is létrehozhat. Hello tárolási fiók hitelesítő adatait mentése folyamatban van, amíg a hello szolgáltatás megkísérli toocommunicate a felhőszolgáltatóval. hello hitelesítő adatokat és a megadott hello hozzáférés anyagot jelenleg hitelesítése. A tárolási fiók hitelesítő adatait csak akkor, ha hello hitelesítés sikeres jön létre. Ha hello hitelesítés nem sikerül, majd megfelelő hibaüzenet jelenik meg.

A következő eljárások tooadd az Azure storage-fiók hitelesítő adatainak hello használata:

* a tárolási fiók hitelesítő adatait, amelynek tooadd hello hello Eszközkezelő szolgáltatásként azonos Azure-előfizetés
* egy Azure storage fiók hitelesítő adatait, amely kívül hello Eszközkezelő szolgáltatás előfizetésének tooadd

#### <a name="tooadd-a-storage-account-credential-that-has-hello-same-azure-subscription-as-hello-device-manager-service"></a>a tárolási fiók hitelesítő adatait, amelynek tooadd hello hello Eszközkezelő szolgáltatásként azonos Azure-előfizetés

1. Keresse meg a tooyour Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán. Ekkor megnyílik a hello **áttekintése** panelen.
2. Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. A hello **tárfiók hozzáadása** panelen a következő hello:
   
    1. A **előfizetés**, jelölje be **aktuális**.
    2. Adja meg az Azure storage-fiók hello nevét.
    3. Válassza ki **engedélyezése** toocreate biztonságos csatornát a StorSimple eszköz és hello felhő közötti hálózati kommunikációhoz. Válassza ki **letiltása** csak akkor, ha magánfelhőben tevékenykedik.
    4. Kattintson az **Add** (Hozzáadás) parancsra. Értesítés jelenik meg a tárfiók hello sikeres létrehozása után.<br></br>
   
        ![Adja hozzá egy meglévő storage-fiók hitelesítő adatait](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

#### <a name="tooadd-an-azure-storage-account-credential-that-is-outside-of-hello-device-manager-service-subscription"></a>egy Azure storage fiók hitelesítő adatait, amely kívül hello Eszközkezelő szolgáltatás előfizetésének tooadd

1. Keresse meg a tooyour Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán. Ekkor megnyílik a hello **áttekintése** panelen.
2. Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz. A parancs megjeleníti minden meglévő tárfiók hitelesítő adatainak társított hello StorSimple Device Manager szolgáltatást.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. A hello **tárfiók hozzáadása** panelen a következő hello:
   
    1. A **előfizetés**, jelölje be **más**.
   
    2. Adja meg az Azure storage-fiók hitelesítő adatait hello nevét.
   
    3. A hello **tárfiók_elérési_kulcsa** szövegmezőben adjon meg hello elsődleges elérési kulcsát az Azure storage-fiók hitelesítő adatait. a kulcs tooget nyissa toohello Azure Storage szolgáltatás, válassza ki a tárolási fiók hitelesítő adatait, és kattintson **fiók kulcsok kezelése**. Ekkor átmásolhatja hello elsődleges elérési kulcsát.
   
    4. tooenable SSL, kattintson a hello **engedélyezése** gomb toocreate biztonságos csatornát a StorSimple Device Manager szolgáltatás és hello felhő közötti hálózati kommunikációhoz. Kattintson a hello **letiltása** gomb csak akkor, ha magánfelhőben tevékenykedik.
   
    5. Kattintson az **Add** (Hozzáadás) parancsra. Hello tárolási fiók hitelesítő adatait sikeres létrehozása után értesítést kap.

5. az újonnan létrehozott hello tárolási fiók hitelesítő adatait hello konfigurálása Eszközkezelő StorSimple szolgáltatás panel alatt jelenik meg **Tárfiók hitelesítő adatainak**.
   
    ![Adja hozzá a tárolási fiók hitelesítő adatait, kívül hello Eszközkezelő szolgáltatási előfizetés szükséges](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-outside-storageacct.png)

## <a name="edit-a-storage-account-credential"></a>A tárolási fiók hitelesítő adatok szerkesztése
A tárolási fiók hitelesítő adatait az eszköz által használt szerkesztheti. Ha manuálisan szerkeszti a tárolási fiók hitelesítő adatait, amely jelenleg használatban van, a hello mezők elérhető toomodify hello elérési kulcsot és hello SSL-mód hello tárolási fiók hitelesítő adatait. Adja meg a hello új tárelérési kulcs, vagy módosítsa hello **engedélyezése SSL-mód** kijelölés és frissített hello beállítások mentéséhez.

#### <a name="tooedit-a-storage-account-credential"></a>a tárolási fiók hitelesítő adatait tooedit
1. Keresse meg a tooyour Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán. Ekkor megnyílik a hello **áttekintése** panelen.
2. Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz. A parancs megjeleníti minden meglévő tárfiók hitelesítő adatainak társított hello StorSimple Device Manager szolgáltatást.
3. A tárfiók hitelesítő adatainak listája táblázatos hello válassza ki, és kattintson duplán a toomodify kívánt hello fiók.
4. Hello tárolási fiók hitelesítő adatait a **tulajdonságok** panelen a következő hello:
   
   1. Ha szükséges, módosíthatja hello **SSL engedélyezése** üzemmód kiválasztása.
   2. A fiók hitelesítő adat tárelérési kulcsok választható tooregenerate. További információkért lásd: [hello tárfiókkulcsok újragenerálása](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys). Hello új tárfiók hitelesítő kulcsot fogja tartalmazni. Egy Azure storage-fiókot ez pedig hello elsődleges elérési kulcsát.
   3. Kattintson a **mentése** hello hello tetején **tulajdonságok** panel toosave hello beállításait. hello-beállítások frissítése a hello **Tárfiók hitelesítő adatainak** panelen.
      
      ![A tárolási fiók hitelesítő adatok szerkesztése](./media/storsimple-virtual-array-manage-storage-accounts/ova-edit-storageacct.png)

## <a name="delete-a-storage-account-credential"></a>Törölje a tárolási fiók hitelesítő adatait
> [!IMPORTANT]
> Törölheti a tárolási fiók hitelesítő adatait csak akkor, ha nincs használatban. Ha a tárolási fiók hitelesítő adatait használja, értesítés jelenik meg.
> 
> 

#### <a name="toodelete-a-storage-account-credential"></a>a tárolási fiók hitelesítő adatait toodelete
1. Keresse meg a tooyour Eszközkezelő szolgáltatás, válassza ki, és kattintson rá duplán. Ekkor megnyílik a hello **áttekintése** panelen.
2. Válassza ki **Tárfiók hitelesítő adatainak** belül hello **konfigurációs** szakasz. A parancs megjeleníti minden meglévő tárfiók hitelesítő adatainak társított hello StorSimple Device Manager szolgáltatást.
3. A tárfiók hitelesítő adatainak listája táblázatos hello válassza ki, és kattintson duplán a toodelete kívánt hello fiók.
4. Hello tárolási fiók hitelesítő adatait a **tulajdonságok** panelen a következő hello:
   
   1. Kattintson a **törlése** toodelete hello hitelesítő adatokat.
   2. Amikor felszólítja a megerősítésre, kattintson **Igen** toocontinue a hello törlését. hello táblázatos felsorolása frissített tooreflect hello módosítások.
      
      ![Törölje a tárolási fiók hitelesítő adatait](./media/storsimple-virtual-array-manage-storage-accounts/ova-del-storageacct.png)

## <a name="synchronizing-storage-account-credential-keys"></a>Hitelesítő adatok tárfiókkulcsainak szinkronizálása
Biztonsági okokból kulcs Elforgatás feltétele gyakran adatközpontokban. A Microsoft Azure rendszergazda újragenerálja vagy hello elsődleges vagy másodlagos kulcsot módosítása hello tárolási fiók hitelesítő adatait (a Microsoft Azure Storage szolgáltatás hello) keresztül közvetlenül elérésével. automatikusan hello StorSimple Device Manager szolgáltatás nem látja ezt a módosítást.

tooinform hello StorSimple Device Manager szolgáltatás hello változás kell tooaccess hello StorSimple Device Manager szolgáltatás, hello tároló elérése fiók a hitelesítő adatok, majd szinkronizálja (attól függően, melyiket megváltozott) hello elsődleges vagy másodlagos kulcsot. hello szolgáltatás majd hello legutóbbi kulcs lekérdezi, hello kulcsokkal titkosítja, és elküldi a hello titkosított kulcs toohello eszköz.

#### <a name="toosynchronize-keys-for-storage-account-credentials-in-hello-same-subscription-as-hello-service-azure-only"></a>a tárfiók hitelesítő adatainak a toosynchronize kulcsok hello ugyanahhoz az előfizetéshez hello szolgáltatást (csak Azure)
1. A hello szolgáltatás követően panelen válassza ki a szolgáltatást, kattintson duplán a hello szolgáltatás nevét, majd a hello **konfigurációs** kattintson **Tárfiók hitelesítő adatainak**.
2. A hello **Tárfiók hitelesítő adatainak** panelen hello Tárfiók hitelesítő adatait, jelölje ki hello tárolási fiók hitelesítő adatait, amelyet az toosynchronize amelynek kulcsait.
3. A hello **tulajdonságok** hello panelen kiválasztott tárolási fiók hitelesítő adatait, a következő hello:
   
    1. Kattintson a **további**, és kattintson a **szinkronizálási hozzáférési kulcs**.
   
    2. Amikor felszólítja a megerősítésre, kattintson **szinkronizálási kulcs** toocomplete hello szinkronizálás.
    
4. A StorSimple eszköz Manager szolgáltatás hello be kell tooupdate hello kulcsot, amely korábban a Microsoft Azure Storage szolgáltatás hello megváltozott. A hello **szinkronizálás tárfiók kulcsa** panelt, ha módosult, hogy hello elsődleges elérési kulcsát (újragenerált), kattintson az elsődleges, és kattintson **szinkronizálási kulcs**. Ha másodlagos kulcs hello módosult, kattintson a **másodlagos**, és kattintson a **szinkronizálási kulcs**.
   
    ![A szinkronizálási hozzáférési kulcsot](./media/storsimple-virtual-array-manage-storage-accounts/ova-sync-acess-key.png)

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[felügyelete a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md).

