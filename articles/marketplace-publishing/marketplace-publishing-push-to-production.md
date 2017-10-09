---
title: "aaaDeploy az ajánlat toohello Azure piactérről |} Microsoft Docs"
description: "További tudnivalók és bízná hello utasításokat toodeploy ajánlatát – virtuálisgép-lemezkép, fejlesztői szolgáltatás, szolgáltatás stb--toohello Azure piactéren."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 8f79b891-84e2-4f41-ba0d-66420e2c6b2e
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2016
ms.author: hascipio
ms.openlocfilehash: ab0bb7c78020187505c2d5f09c4de246987ecd97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-offer-toohello-azure-marketplace"></a>Az ajánlat toohello Azure piactér telepítése
Ha elégedett az ajánlatot (Ez azt jelenti, amelyeket tesztelt forgatókönyvet, marketing tartalom stb.), és készen áll a toolaunch kérelem **tooproduction leküldéses** a hello **közzététel** fülre.  

1. hello négy lépések alapján hello forgatókönyv lap hello portal közzétételi kell befejeződött, és a zöld. Virtuális gép ajánlatokat gondoskodjon arról, hogy a következő irányelveket hello követi.
   
    ![rajz][img-pubportal-walkthru-checked]
2. Jelölje be hello **közzététel** lap bal oldalán hello hello listából.
   
    ![rajz][img-pubportal-menu-publish]
3. Hello gombra **toopush tooproduction jóváhagyási kérelmek**. Miután hello kérelem, hello jóváhagyási team végrehajtja a végső áttekintése, és majd ajánlatát hello Azure piactéren elérhető lesz.
   
    ![rajz][img-pubportal-publish-pushproduction]

> [!IMPORTANT]
> Virtuális gépek esetén hello gomb kérés jóváhagyása toopush tooproduction kattintva hello a következő lépéseket végzik hello helyszín mögött. Képes tooview hello folyamat minden egyes lépést hello közzététel lap hello fogja portal közzététele. Ellenőrizze ezen a lapon rendszeres időközönként (amíg hello állapota "Felsorolt") a átfogóan javítási igénylő hiba információkat.
> 
> * Először a termelési kéréseket, amelyekre toohello hitelesítő csoport hello vhd ellenőrzése. Azonban ha már felsorolt ajánlatát frissít, és hello kérelem rendelkezik azt csak a módosítás marketing, majd hello hitelesítő a lépés kimarad.
> * Hello a következő lépésben hello kérelem hello marketing hello ajánlat tartalmának ellenőrzése toohello tartalomérvényesítési csoport származnak.
> * Ha a fenti lépéseket hello sikeres, majd hello ajánlat jóváhagyja éles környezetben. Ilyenkor hello állapota lesz "szereplő" hello közzétételi portáljára. Azonban az "Felsorolt" állapot nem feltétlenül jelenti azt, hogy hello folyamat be nem fejeződik. a következő lépéseket kell toobe befejezése előtt hello ajánlat hello hello Azure piactér érhető el.
> * Miután hello ajánlat jóváhagyják a fenti hello lépésben éles, a replikációs hello ajánlat indításának összes hello Azure adatközpontjaiban. Általában 24-48hours hello replikációs toocomplete az szükséges, de tooa hét attól függően, hogy hello virtuális merevlemez méretére hello is tarthat. Ha már felsorolt ajánlatát frissít, és rendelkezik lett, csak a módosítás marketing, majd hello replikációs azonban gyorsabb.
> * Hello replikáció befejeződése után hello ajánlat hello Azure piactéren elérhető lesz.
> 
> Törölheti hello ajánlat mindig le egy **vázlat** állapota (azaz soha nem **toostaging leküldéses** vagy **tooproduction leküldéses**). A hello **előzmények** lapra, majd hello **vázlat elvetése** tervezet hello lap toodelete hello alján gombra.
> 
> 

## <a name="production-checklist-for-all-virtual-machine-offers"></a>Minden virtuális gép ajánlatok éles ellenőrzőlistája
* Győződjön meg arról, hogy-e a Microsoft Azure Certified partneren
* Hello termékváltozatok lapon hello beállítás "elrejtőzni a piactér hello a Termékváltozat, mert azt kell mindig vásárolható keresztül a megoldássablon" kell megjelölve lennie Igen, csak ha hello SKU része a Megoldássablon. Minden egyéb esetben hello, ez a beállítás mindig jelölésű nem.
* Ne feledje: Nem kell módosítani hello SKU láthatósági Ha hello SKU szerepel. Ez a funkció nem támogatott.
* Győződjön meg arról, hogy a hello emblémák toohello Azure piactér emblémáinak használatáról az alábbi igazodik.
* Ajánlat és SKU leírása nem lehet azonos.
* SKU a Title és kínálnak hosszú összefoglaló nem lehet azonos.
* Termékváltozat-cím és kínálnak összegzése nem lehet azonos.
* Termékváltozat-címek nem lehet azonos az ajánlatot több SKU.

**Az Azure piactér emblémáinak használatáról**

* hello Azure tervezési egy egyszerű színpaletta rendelkezik. Tarthatja hello száma elsődleges és másodlagos színek az embléma alacsony.
* hello téma hello Azure-portálon a színeket fehér és fekete. Ezért kerülje ezeket a színeket, a emblémák hello háttérszínét. Néhány színt, amely jól láthatóan elhelyezett hello Azure-portálon az lenne a emblémák használja. Azt javasoljuk, hogy egyszerű alapszínek. Átlátszó háttérrel használ, majd győződjön meg arról, hogy hello embléma/szöveg nincs fehér vagy fekete.
* Ne használjon egy színátmenetes hátterének hello embléma.
* Ne helyezzen hello embléma szöveg, még akkor is a vállalat vagy a márka neve.
* hello megjelenését és működését az embléma "egyszerű" kell, és átmenetek kerülendő.
* nem kell felhőbe hello embléma.

**Hello Hero embléma irányelveket:**

* hello Hero embléma nem kötelező megadni. hello publisher nem tooupload Hero embléma választhat. **Azonban egyszer feltöltött hello hero ikon nem lehet törölni az hello portal közzététele. Ugyanakkor a hello partner hello Azure piactér irányelveket kell követnie Hero ikonok más hello ajánlat nem lesznek jóváhagyott tooproduction.**
* hello Publisher megjelenített név, SKU-cím és hello kínálnak, hosszú összefoglalás fehér betűszíne jelennek meg. Ezért ne bármely könnyű szín tartása hello hátterében hello Hero ikonra. Fekete, fehér és átlátható háttér nem engedélyezett hőse ikonok.
* hello publisher megjelenített név SKU cím, hosszú összefoglalás hello ajánlat létrehozásával, és hello gomb vannak ágyazva programozott módon hello Hero embléma után hello ajánlat felsorolt kerül. Ezért kell meg szöveg hello Hero embléma tervezésekor. Ne változtassa meg üres területet hello jobb oldali mert hello szöveg (pl. közzétevő megjelenített név, SKU-cím, hello ajánlat hosszú összegzése) lesznek a tagjai programozott módon lépünk Önnel ott keresztül. hello üres hello szöveg kell lenniük a jobb oldali hello 415 x 100 (és hello balról 370px ellensúlyozza).

## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Kínál már listán szereplő virtuális gép további éles ellenőrzőlistája
* Ellenőrizze, ha már létezik egy ajánlat hello, azonos kínálnak a vállalat neve. Ha igen, majd adja hello SKU új verziójának hello meglévő ajánlat helyett egy új ismétlődő ajánlat létrehozásakor.
* Adatlemez nem szükséges módosítani hello két verziója közötti azonos Termékváltozat.
* hello Azure piactér nem támogatja azt hatással van a meglévő ügyfelek hello hello számlázási Termékváltozatok árazás változásával az hello szerepel. Győződjön meg arról, hogy nem változtat hello árképzési felsorolt hello SKU hello régiókban, ahol hello SKU áll rendelkezésre. Azonban új termékváltozatok hozzáadása, vagy adja hozzá az új régiók tooan meglévő Termékváltozat.

## <a name="next-steps"></a>Következő lépések
Hello ajánlat élő kerül, ha tesztelése hello ügyfél forgatókönyvek toovalidate hello szerződések és a funkció működéséhez megfelelően hello éles környezetben tesztelt és jóváhagyott a hello átmeneti környezet szerint.

## <a name="see-also"></a>Lásd még:
* [Első lépések: hogyan toopublish egy ajánlat toohello Azure piactéren](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
