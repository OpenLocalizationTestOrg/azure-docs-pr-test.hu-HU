---
title: "a virtuális gép rendszerképet az Azure piactér hello aaaManaging |} Microsoft Docs"
description: "Részletes útmutató, hogyan toomanage a virtuális gép rendszerképet az Azure piactér hello kezdeti közzétételét követően"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: 47a082e686e1248ceacb844d3c0f2f5c33133dab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="post-production-guide-for-virtual-machine-offers-in-hello-azure-marketplace"></a>Gyártás utáni útmutató a virtuális gép kínál a hello Azure piactér
Ez a cikk ismerteti, hogyan frissítheti az olyan élő virtuális gép ajánlat a hello Azure piactéren. Végigvezeti egy vagy több új termékváltozatok tooan meglévő ajánlat Hozzáadás hello folyamata. Azt is végigvezeti Önt élő virtuális gép ajánlat vagy SKU eltávolítása hello piactér hello folyamatán.

Miután egy ajánlat/SKU elő van készítve a hello [Azure-portálon](http://portal.azure.com), nem módosíthatja a következő szövegmezőkben hello:

* **Ajánlat azonosítója**: hello közzététele a portálon lépjen túl**virtuális gépek** válassza ki az ajánlatot. Kattintson a **Virtuálisgép-rendszerképek** > **kínálnak azonosító**.
* **Termékváltozat-azonosító**: hello közzététele a portálon lépjen túl**virtuális gépek** válassza ki az ajánlatot. Kattintson a **Termékváltozatok** > **adja hozzá a Termékváltozat**.
* **A Publisher Namespace**: hello közzététele a portálon lépjen túl**virtuális gépek** > **forgatókönyv** > **mondja el nekünk kapcsolatban a vállalati** (az található a "Lépés 2 regisztrálása a vállalati") > **Publisher Namespace** > **Namespace**.

Miután hello ajánlat/SKU hello szerepel [piactér](http://azure.microsoft.com/marketplace), nem módosíthatja a következő szövegmezőkben hello:

* **Ajánlat azonosítója**: hello közzététele a portálon lépjen túl**virtuális gépek** válassza ki az ajánlatot. Kattintson a **Virtuálisgép-rendszerképek** > **kínálnak azonosító**.
* **Termékváltozat-azonosító**: hello közzététele a portálon lépjen túl**virtuális gépek** válassza ki az ajánlatot. Kattintson a **Termékváltozatok** > **adja hozzá a Termékváltozat**.
* **A Publisher Namespace**: hello közzététele a portálon lépjen túl**virtuális gépek** > **forgatókönyv** > **mondja el nekünk kapcsolatban a vállalati** ("Lépés 2 regisztrálása" alatt található) **Publisher Namespace** > **Namespace**.
* **Portok**: hello közzététele a portálon lépjen túl**virtuális gépek** válassza ki az ajánlatot. Kattintson a **Virtuálisgép-rendszerképek** > **megnyitott portok**.
* **Módosítsa a felsorolt SKU(s) díjszabása**
* **Felsorolt SKU(s) számlázási modell megváltozott**
* **A felsorolt SKU(s) régiói számlázási törlése**
* **Hello adatok változásaival lemez felsorolt SKU(s) száma**

## <a name="update-hello-technical-details-of-a-sku"></a>A metódust a hello műszaki részleteinek frissítése
egy új verzió toohello tooadd SKU felsorolt és ismételt közzététele az ajánlatot, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **Virtuálisgép-rendszerképek** fülre.
4. A hello **termékváltozatok** területén keresse meg, hogy szeretné-e tooupdate SKU hello.
5. Egy új verziószámot hello SKU, és kattintson a hello  **+**  gombra. hello új verziószámának X.Y.Z formátumban, ahol X, Y és Z egész számnak kell lennie. Megváltozik a verzió csak növekményes kell lennie.
6. A hello **az operációs rendszer virtuális merevlemez URL-cím** mezőbe, írja be a hello közös hozzáférésű jogosultságkódot hello virtuális merevlemez operációs rendszerhez létrehozott URI és hello módosítások mentése.

   > [!IMPORTANT]
   > Ön nem növekvő/csökkentést hello adatlemez felsorolt metódust. Ebben az esetben szüksége van egy új SKU toocreate. Részletes útmutatásért tekintse meg a toohello szakasz [hozzáadása egy új SKU felsorolt ajánlat keretében](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
8. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Virtuálisgép-rendszerképek](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-hello-nontechnical-details-of-an-offer-or-a-sku"></a>Az ajánlat vagy egy SKU hello a felhasználóknak részleteinek frissítése
Frissítheti a felhasználóknak hello (marketing, jogi, támogatási, kategóriák) hello piactér az élő ajánlat vagy SKU részleteit.

### <a name="update-hello-offer-description-and-logos"></a>Hello ajánlat leírása és emblémák frissítése
tooupdate hello részleteket nyújtanak, és az ajánlatot közzétenni, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **MARKETING** fülre.
4. Kattintson a **angol (amerikai nyelv)**.
5. Kattintson a hello **részletek** fülre. A hello **leírása** szakaszban, a frissítés hello ajánlat **cím**, **összegzés**, és **hosszú összefoglalás** és hello módosítások mentéséhez.

   > [!NOTE]
   > Hello SKU részletek frissítésekor e korlátozások figyelembe venni: 
   * Ismétlődő szöveg hello ajánlat leírását és hello SKU leírása nem adja meg.
   * Nem ismétlődő szöveg megadása hello SKU cím és hello ajánlat hosszú összegzése. 
   * Nem ismétlődő szöveg megadása hello SKU cím és hello ajánlat-összefoglaló.
   >
   >

    ![Részletek](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. A hello **emblémák** hello szakasza **részletek** lapján hello emblémák frissítése. Győződjön meg arról, hogy hello emblémák kövesse hello [Azure piactér irányelvek](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > A kiemelt ikon használata nem kötelező. Nem tooupload választhat egy kiemelt ikonra. Azonban után egy kiemelt ikon töltheti fel, nincs nincs kiépítési toodelete azt hello portal közzététele. Hajtsa végre a hello [hero ikon irányelvek](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
8. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Emblémák](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-hello-sku-description"></a>Frissítés hello SKU leírása
Termékváltozat közzé az ajánlatot, és adatokat tooupdate hello kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **MARKETING** fülre.
4. Kattintson a **angol (amerikai nyelv)**.
5. Kattintson a hello **tervek** fülre. A hello **termékváltozatok** területen hello SKU frissítése **cím**, **összegzés**, és **leírás** és hello módosítások mentése.

   > [!NOTE]
   > Hello SKU részletek frissítésekor e korlátozások figyelembe venni: 
   * Ismétlődő szöveg hello ajánlat leírását és hello SKU leírása nem adja meg. 
   * Nem ismétlődő szöveg megadása hello SKU cím és hello ajánlat hosszú összegzése. 
   * Nem ismétlődő szöveg megadása hello SKU cím és hello ajánlat-összefoglaló.
   >
   >
6. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
7. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Tervek](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Módosítsa a meglévő kapcsolatokat, vagy adja hozzá az új hivatkozások
toochange meglévő kapcsolatokat, vagy új hivatkozások hozzáadása és újbóli az ajánlatot, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **MARKETING** fülre.
4. Kattintson a **angol (amerikai nyelv)**.
5. Kattintson a hello **hivatkozások** fülre.
6. egy új hivatkozás a hello tooadd **hivatkozások** kattintson **+ hivatkozás hozzáadása**. A hello **hivatkozás hozzáadása** párbeszédpanelen adja meg a hello hivatkozás **cím** és **URL-cím** és hello módosítások mentése. Megadhatja, hogy minden hivatkozás, amely segít a felhasználóknak információkat tartalmaz.
7. tooupdate vagy törölje a meglévő hivatkozás, jelölje ki a hello hivatkozásra, és kattintson a hello **szerkesztése** gombra való kattintást vagy hello **törlése** gombra.

   > [!NOTE]
   > Győződjön meg arról, hogy hello hivatkozásokat tartalmaz, amelyek a jelen szakaszban megadott működik megfelelően, mert ezek a hivatkozások beolvasása érvényesíti kérelem folyamat során.
   >
   >
8. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
9. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Hivatkozások](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Hivatkozás hozzáadása](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Módosítsa a meglévő mintát lemezképet, vagy adjon hozzá egy új minta lemezképet
egy meglévő mintát toochange kép vagy új minta lemezképek hozzáadása és újbóli az ajánlatot, kövesse az alábbi lépéseket:

> [!NOTE]
> Csak egy minta kép megjelenik hello [Azure-portálon](https://portal.azure.com).
>
>

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **MARKETING** fülre.
4. Kattintson a **angol (amerikai nyelv)**.
5. Kattintson a hello **minta képek** fülre.
6. egy új minta lemezképet hello tooadd **minta képek** területen kattintson **új LEMEZKÉP FELTÖLTÉSE** és hello módosítások mentéséhez.

   > [!NOTE]
   > Többek között a minta-lemezkép nem kötelező megadni.
   >
7. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
8. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![A minta lemezképek](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-hello-legal-content"></a>Hello jogi tartalom frissítése
tooupdate jogi tartalom hello és ismételt közzététele az ajánlatot, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **MARKETING** fülre.
4. Kattintson a **angol (amerikai nyelv)**.
5. Kattintson a hello **jogi** fülre. A hello **jogi** szakaszban, a házirendek/használati feltételek frissítése. Írja vagy illessze be a hello házirendek/feltételeket a hello **használati** mezőbe, majd hello módosítások mentéséhez.
6. hello karakter hello jogi használati feltételek határa 1 millió karaktereket.
7. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
8. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Jogi tudnivalók](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-hello-support-information"></a>Hello támogatási információk frissítése
tooupdate hello támogatási információkat és az ajánlatot közzétenni, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **támogatási** fülre.
4. A hello **forduljon a mérnöki** részben, a frissítés hello mérnöki kapcsolattartási adatait. Ezen adatok csak közötti hello partnerek és a Microsoft belső kommunikációhoz használt.
5. A hello **ügyfélszolgálatot** területen hello támogatási kapcsolattartási adatai és hello frissítése **TÁMOGATJA az URL-cím**. Ezen adatok csak közötti hello partnerek és a Microsoft belső kommunikációhoz használt.

   > [!NOTE]
   > Ha azt szeretné, hogy tooprovide csak e-mailes támogatást, adjon meg egy üres telefonszámot hello **ügyfélszolgálatot** szakasz. Ebben az esetben hello e-mailt a megadott helyett használnak.
   >
   >
6. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
7. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Támogatás](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-hello-categories"></a>Frissítés hello kategóriák
tooupdate hello kategóriák szakaszt az előfizetéshez, és közzé az ajánlatot, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **kategóriák** fülre.
4. A hello **kategóriák** szakaszt, frissítse az előfizetéshez hello kategóriák és hello módosítások mentése. Választhat, hello Azure piactér galéria toofive kategóriák létrehozása.
5. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
6. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Kategóriák](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Adja hozzá a felsorolt ajánlat keretében egy új Termékváltozat
az élő ajánlatot egy új Termékváltozata tooadd kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **Termékváltozatok** fülre. Kattintson a **adja hozzá a Termékváltozat**. 
4. A hello párbeszédpanelen adjon meg egy **SKU-azonosítója** kisbetűs. Jelölje be hello **kapcsolja a saját licenc (használata BYOL) számlázási modellt** jelölőnégyzetet, ha azt szeretné, hogy toopublish hello BYOL számlázási modell új Termékváltozat. Minden más esetben törölje hello jelölőnégyzetet. Kattintson a hello osztásjelek megjelölés toocreate egy új Termékváltozat. Ha nem hello BYOL számlázási modellt, hello számlázási modellt automatikusan toohourly van beállítva. Ha hello 30 napos ingyenes próbaverzióval hello óránkénti számlázási modell, jelölje be **egy hónap** a **érhető el egy ingyenes próbaverzióra?** Máskülönben válassza **nem próbaverzió**. (**Érhető el egy ingyenes próbaverzióra?**  csak akkor, ha nincs kiválasztva BYOL, amíg új SKU létrehozása hello jelenik meg.)

   > [!IMPORTANT]
   > **A Termékváltozat a piactér hello elrejtése, mert azt kell mindig vásárolható keresztül a megoldássablon** kell **Igen** *csak* Ha közzétételéhez a megoldássablon még jóvá. Ellenkező esetben ez a beállítás mindig kell **nem**.
   >
   >
4. Hello hello bal oldali menüben kattintson a hello **Virtuálisgép-rendszerképek** lapra, és megtudhatja, hello létrehozott új Termékváltozat.
5. tooset legfeljebb hello új SKU- című [kifutott a VM-lemezkép](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) útmutatót.
6. az anyag marketing tooadd hello új SKU, lásd: [tartalom marketing meg piactér](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. tartozó díjszabási információkat tooadd új SKU hello című [az árak](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
9. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

    ![Termékváltozat](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Adja hozzá a Termékváltozat](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-hello-data-disk-count-for-a-listed-sku"></a>Hello adatlemez felsorolt SKU módosítása
Ön nem növekvő/csökkentést hello adatlemez felsorolt metódust. Ebben az esetben szüksége van egy új SKU toocreate. Részletes útmutatásért tekintse meg a toohello szakasz [hozzáadása egy új SKU felsorolt ajánlat keretében](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-hello-marketplace"></a>A felsorolt ajánlat törlése hello piactér
Különböző szempontjairól hello esetében egy kérelem tooremove élő ajánlat végrehajtott megvagyunk toobe kell. tooget útmutatások a hello támogatási csapatának tooremove a felsorolt ajánlatot hello piactér, kövesse az alábbi lépéseket:

1. Emelje a hello támogatási jegy [incidens létrehozása](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) lap.

2. Válassza ki **problématípust** , **ajánlatok kezelése**, és válassza ki **kategória** , **módosítása egy ajánlatot és/vagy a Termékváltozat már éles környezetben**.
3. Hello kérelmet.

hello támogatási csoport végigvezeti hello ajánlat/SKU törlési folyamat.

> [!NOTE]
> Mindig törölheti hello ajánlat, amíg a Vázlat állapotba (de nem átmeneti vagy üzemi). A hello **előzmények** lapra, majd **ELVETÉSE VÁZLAT**.
>
>

## <a name="delete-a-listed-sku-from-hello-marketplace"></a>A felsorolt SKU törlése hello piactér
a felsorolt SKU hello piactér, a toodelete kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).

2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali ablaktáblában kattintson a hello **Termékváltozatok** fülre.
4. Jelölje be hello SKU, hogy szeretné, hogy toodelete, és kattintson a hello **törlése** gombra.
5. Nyissa meg toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish hello ajánlat hello piactéren.
6. Miután hello ajánlat a piactér hello ismételt közzététele, hello SKU törlődik hello piactér és hello Azure-portálon.

## <a name="delete-hello-current-version-of-a-listed-sku-from-hello-marketplace"></a>A felsorolt SKU hello jelenlegi verziója törlése hello piactér
toodelete hello aktuális verzió a listában szereplő metódust a hello piactér, kövesse az alábbi lépéseket: 

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).

2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **Virtuálisgép-rendszerképek** fülre.
4. Válassza ki hello SKU, amelynek jelenlegi verzióját, toodelete, majd kattintson hello **törlése** gombra.
5. Nyissa meg toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish hello ajánlat hello piactéren.
6. Miután hello ajánlat a lekérdezi a piactér hello ismételt közzététele, hello hello felsorolt SKU törlődik a piactér hello és hello Azure-portál jelenlegi verziója. hello SKU majd vissza lesz állítva tooits előző verziót.

## <a name="revert-hello-listing-price-tooproduction-values"></a>Állítsa vissza a hello tooproduction árértékeket listázása
toorevert tooproduction hello listaelem árértékeket, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).
2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **ÁRAZÁS** fülre.
4. Válasszon ki egy régiót, amelynek árképzési kívánt tooreset.

    ![Árképzési régiók](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Az SKU egy óránkénti számlázási modellt alaphelyzetbe összes hello magok hello árakat éles hello kijelölt terület szerint. SKU BYOL számlázási modell, ügyeljen hello régióban elérhető SKU hello hello jelölőnégyzet bejelölésével a hello SKU a hello **EXTERNALLY-LICENSED (BYOL) SKU rendelkezésre ÁLLÁSI** szakasz.

    ![Árképzési modellekkel](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Kattintson a **PIACOKRA AUTOPRICE az Egyesült Államokban árak alapján**.

   > [!NOTE]
   > hello gomb feliratát kiválasztott hello régió függően eltérőek lehetnek. Az Amerikai Egyesült Államok választott azt, mert hello gomb címkéje **AUTOPRICE egyéb piacok alapú ON árak része az Egyesült Államok**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. A lapon hello Autoprice varázsló 1 hello alap piaci válassza ki, majd kattintson hello **nyíl** gombra.

    ![Alap piaci](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. A 2 lapon, válassza ki a service-csomagokról és mérőszámok (magok), majd kattintson a hello **nyíl** gombra.

    ![Service-csomagokról és mérőszámok](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. 3 lapján kattintson a **váltása összes** tooselect minden régióban. Vagy manuálisan kiválaszthatja az adott régióban jelölőnégyzeteket. Kattintson a hello **nyíl** gombra.

    ![Válassza ki a piacon](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. A lapon 4 ellenőrizze hello árfolyam, és kattintson a **Befejezés**. hello varázsló függően tooyour beállításokat árképzési hello alaphelyzetbe állítása.

11. A hello **ÁRAZÁS** lapra, majd **összegzése és módosítások**.
    A **verzió megtekintése**, jelölje be **vázlat**, és a **összehasonlításra**, jelölje be **éles**. Ha nincs árképzési különbség, hello árképzési vissza toohello éles értékek sikeresen.

    ![Összefoglalás megtekintése és a változások](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
13. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

## <a name="revert-hello-billing-model-tooproduction-values"></a>Állítsa vissza a számlázási modell tooproduction értékek hello
toorevert hello számlázási modell tooproduction értékek, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).

2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **Termékváltozatok** fülre.
4. Kattintson a hello **szerkesztése** gomb toorevert hello számlázási modellt. A megnyíló hello ablakban válassza ki, vagy törölje a hello **számlázási és licencelési történik külsőleg (más néven a saját licenc) az Azure-ból** jelölőnégyzetet.

    ![Számlázási szerkesztése](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Ebben a cikkben kövesse a "Revert hello tooproduction árértékeket listázása" hello lépéseit.
6. Nyissa meg toohello **közzététel** fülre, majd **LEKÜLDÉSES tooSTAGING**. Az ajánlat tesztelése az átmeneti környezet hello részletes útmutatást lásd: [tesztelése a virtuális gép ajánlat hello piactér](marketplace-publishing-vm-image-test-in-staging.md).
7. Az ajánlatot tesztelését követően a tesztelési, lépjen a toohello **közzététel** hello lapján portal közzététele. Kattintson a **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

## <a name="revert-hello-visibility-setting-of-a-listed-sku-toohello-production-value"></a>A felsorolt SKU toohello éles értékét hello láthatósági visszaállítása
toorevert hello látható a listában szereplő SKU toohello éles érték beállítása, kövesse az alábbi lépéseket:

1. Jelentkezzen be toohello [portal közzétételi](https://publish.windowsazure.com).

2. Nyissa meg toohello **virtuális gépek** fülre, és válassza ki az ajánlatot.
3. Hello hello bal oldali menüben kattintson a hello **Termékváltozatok** fülre.
4. Válassza ki a Termékváltozat, és hello láthatósági beállítását hello SKU toohello termelési érték visszaállításához.

    ![Látható](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Miután hello változtatásokat végzett, kattintson **vonatkozó KÉRÉS jóváhagyása tooPUSH tooPRODUCTION** toorepublish a hello Piactéri ajánlat.

## <a name="see-also"></a>Lásd még:
* [Első lépések: Az ajánlat toohello Azure piactér közzététele](marketplace-publishing-getting-started.md)
* [Kifizetés reporting ismertetése](marketplace-publishing-report-payout.md)
* [A Cloud Solution Provider viszonteladóhoz célzó olyan ösztönzők előnyeivel módosítása](marketplace-publishing-csp-incentive.md)
* [Közös közzétételi kapcsolatos problémák elhárítása a hello piactér](marketplace-publishing-support-common-issues.md)
* [Segítségre van szüksége közzétevőként](marketplace-publishing-get-publisher-support.md)
* [Hozzon létre egy Virtuálisgép-lemezkép helyszíni](marketplace-publishing-vm-image-creation-on-premise.md)
* [Hozzon létre egy olyan virtuális géphez a Windows hello Azure betekintő portálon](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
