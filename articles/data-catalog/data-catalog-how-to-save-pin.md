---
title: "aaaSave keres, és a PIN-kód adategységeket az Azure Data Catalog |} Microsoft Docs"
description: "Hogyan-tooarticle kijelölő képességek az Azure Data Catalog adatforrások és a későbbi használatra adategységeket mentéséhez."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0ad0a31d4b84782fed9d80acc2734912eecd6d74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Az Azure Data Catalog keres, és a PIN-kód adategységeket mentése
## <a name="introduction"></a>Bevezetés
Az Azure Data Catalog az adatforrás-felderítés képességeket biztosít. Gyorsan megkeresheti és hello katalógus toolocate adatforrások szűrése és megértése azok használatának célját, így könnyebben toofind hello megfelelő adatok az elvégzendő hello feladat.

De mi történik, ha szüksége tooregularly hello használata azonos adatokat? Mi történik, ha más felhasználók rendszeresen hozzájárulhatnak a Tudásbázis toohello és azonos adatforrások hello katalógusban? Ezekben a helyzetekben, amelyek toorepeatedly probléma hello azonos lehet, hogy a keresés nem hatékony. Ez azért, ahol mentett keresés és a rögzített adatok eszközök segítségével.

## <a name="saved-searches"></a>Mentett keresések
A mentett kereséseket a Data Catalog az újrafelhasználható, felhasználókon keresési definícióját. Adja meg a keresés – többek között a keresési feltételek, a címkék és a többi szűrőt, és mentse. Újbóli futtatásával hello mentett keresés definition újabb tooreturn bármely az adategységekhez a keresési feltételeknek.

### <a name="create-a-saved-search"></a>A mentett keresés létrehozása
a mentett kereséseket toocreate hello a következő:
1. Hello Azure Data Catalog portálon, a hello **aktuális keresés** ablak, kattintson a **mentése**. 

    ![Aktuális keresési beállítások mentése hivatkozás](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Adja meg a hello keresési feltételeket, hogy szeretné, hogy tooreuse, és kattintson a **mentése**.

    ![Aktuális keresési beállítások mentett keresési neve](./media/data-catalog-how-to-save-pin/02-name.png)

3. Amikor a rendszer kéri, adja meg a mentett keresés hello nevét. Adjon meg egy megfelelő, kifejező nevet és, amely leírja, hogy hello keresés által visszaadott hello adategységeket.

### <a name="manage-saved-searches"></a>Mentett keresések kezelése
Egy vagy több keresés mentése után a **mentett keresések** hello alatt jelennek meg, a beállítás **aktuális keresés** mezőbe. Amikor hello listában ki van bontva, minden mentett keresések jelennek meg.

 ![Mentett keresések listája](./media/data-catalog-how-to-save-pin/03-list.png)

Hajtsa végre a hello műveletet:

* tooexecute keresés – hello listában válassza ki a mentett kereséseket.

* tooview a mentett keresések beállítások listáját kattintson a lefelé mutató nyílra a következő toohello keresési neve hello.

    ![Mentett keresések kezelésére vonatkozó beállítások](./media/data-catalog-how-to-save-pin/04-managing.png)

* tooenter hello mentett keresés új nevet válasszon **átnevezése**. hello keresési definíció nem változott.

* tooremove hello mentett keresés a listáról válassza ki a **törlése**, majd erősítse meg a hello törlését.

* az alapértelmezett keresési, jelölje be toomark hello mentett keresést **mentése alapértelmezett**. Az Azure Data Catalog-kezdőlap hello "empty" keresést hajt végre, ha az alapértelmezett keresés végrehajtása. Ezenkívül hello hello alapértelmezett keresési hello hello tetején jelenik meg van jelölve, amely **mentett keresések** listája.

### <a name="organizational-saved-searches"></a>Szervezeti mentett keresések
A szervezet minden felhasználója keresés saját használatra takaríthat meg. Katalógus-rendszergazdák is mentheti hello szervezeten belüli összes felhasználók keres. Amikor a rendszergazdák a Keresés mentése, hogy találkozik egy **megosztás hello vállalaton belül** lehetőséget. Ez a beállítás megosztások hello mentett keresés az összes felhasználó hello szervezet kiválasztása.

 ![Szervezeti mentett keresések](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Rögzített adategységeket
Mentett keresések mentheti, és ismét felhasználni a keresési definíciókat. hello adategységeket hello keresés által visszaadott hello katalógus módosítás hello tartalmát idővel változhatnak. Amikor adategységek, meghatározott eszközök toomake explicit módon azonosíthatja azokat könnyebben tooaccess anélkül, hogy a keresés toouse.

Egy adategységet rögzítési egyszerű. tooadd hello adatok rögzített tooyour lista, akkor egyszerűen kattintson hello **PIN-kód** ikonra. hello ikon hello sarkában hello eszköz csempe hello mozaik elrendezés nézetben, és hello listanézetben hello Azure Data Catalog portálon hello bal szélső oszlopában.

![hello adategységet rögzítés ikonja](./media/data-catalog-how-to-save-pin/05-pinning.png)

Egy adategységet feloldásával egyaránt egyszerű. Egyszerűen kattintson a hello **rögzítésének** ikon tootoggle hello beállítás hello kiválasztott eszközhöz.

![hello adategységet rögzítésének ikon](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="hello-my-assets-section"></a>Saját eszközök szakasz hello
hello tartalmazza a portál kezdőlapján a Data Catalog egy **saját eszközök** szakaszt, amely megjeleníti az eszközök érdeklődési toohello aktuális felhasználó. Ez a szakasz mindkét rögzített eszközöket tartalmazza, és a mentett kereséseket.

![hello saját eszközök szakasz hello kezdőlapján](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Összefoglalás
Az Azure Data Catalog biztosít képességeket, amelyek könnyebb toodiscover hello adatforrások van szüksége, ezért más szervezet tagja is kevesebb időt töltenek az adatok és dolgozni vele több időt keres. Mentett keresések, és a rögzített adatok eszközök ezek alapképességek létrehozásához, így a felhasználók könnyen azonosíthassák, hogy az adatforrások, amelyek működnek a ismételten.
