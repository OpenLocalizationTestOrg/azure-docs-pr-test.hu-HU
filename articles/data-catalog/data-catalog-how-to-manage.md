---
title: "az Azure Data Catalog aaaManage adategységeket |} Microsoft Docs"
description: "hello cikk mutatja be, hogyan toocontrol láthatóságának és adategységeket tulajdonjogát regisztrálva az Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 48a634b92d7da19c32c9e551f295eec257f54f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Az Azure Data Catalog adategységek felügyelete
## <a name="introduction"></a>Bevezetés
Az Azure Data Catalog adatforrás felderítés, célja, hogy akkor is könnyen megtalálhatóvá és értelmezhetővé hello adatforrások kell tooperform elemzés és döntéseket. E felderítés képességeivel hello legnagyobb hatást és más felhasználók is található, és elérhető adatforrások legszélesebb skáláját hello ismertetése. Ezeknek az elemeknek szem előtt, a hello alapértelmezett Data Catalog működése minden regisztrált adatforrások toobe látható tooand felderíthető katalógus minden felhasználó számára.

A Data Catalog nem ad hozzáférési toohello adatokat saját magát. Adat-hozzáférési hello adatforrás hello tulajdonosának vezérli. A Data Catalog adatforrások felfedezése és a hello metaadatok hello katalógusban regisztrált kapcsolódó toohello adatforrások megtekintéséhez.

Előfordulhat, azonban ha adatforrások használatuk csak akkor látható toospecific felhasználók vagy adott csoportok toomembers. Ilyen esetekben a felhasználók saját tulajdonba vétele belül hello katalógusban regisztrált adategységeket és majd a hello láthatóságának saját hello eszközök kezelése.

> [!NOTE]
> Ebben a cikkben leírt hello funkció csak a Standard Edition Azure Data Catalog hello érhető el. hello ingyenes kiadás nem biztosít tulajdonjoga és adategységet láthatósági korlátozása képességét.
>
>

## <a name="manage-ownership-of-data-assets"></a>Adategységek tulajdonjogát kezelése
Alapértelmezés szerint a Data Catalog regisztrált adategységeket található, a tulajdonos. Minden engedélyt tooaccess hello katalógus rendelkező felhasználó felderíteni, és ezeknek az eszközöknek a jegyzet. A felhasználók tulajdonos nélküli adategységek saját tulajdonba és hello látható-e saját hello eszközök majd korlátozza.

A Data Catalog egy adategységet tulajdonosa, ha csak olyan felhasználók, akik jogosultak a hello tulajdonosai által hello eszköz felderítését, és megtekintheti a metaadatait, és csak hello tulajdonosok hello eszköz törlése hello katalógusból.

> [!NOTE]
> A Data Catalog tulajdonjoga csak hello-katalógusban tárolt hello metaadatok hatással van. Tulajdonjog nem rendelkezik engedéllyel a hello alapul szolgáló adatforrásban.
>
>

### <a name="take-ownership"></a>Saját tulajdonba
Felhasználók tulajdonba az adategységek hello kiválasztásával **tulajdonba** hello Data Catalog-portál beállítást. Nincsenek speciális engedélyek egy tulajdonos nélküli adategységet szükséges tootake tulajdonjogát. Bármely felhasználó tulajdonba tulajdonos nélküli adatok eszköz.

### <a name="add-owners-and-co-owners"></a>Adja hozzá a tulajdonosok és a társtulajdonosok
Ha már van tulajdonosa egy adategységet, más felhasználók nem egyszerűen saját tulajdonba. Kell őket hozzáadni, társtulajdonosok meglévő tulajdonos. A tulajdonos szerint társtulajdonosok is felvehet további felhasználók és biztonsági csoportokat.

> [!NOTE]
> Az érték a tulajdonos legalább két egyéni felhasználók számára a bevált gyakorlat toohave az bármelyik birtokolt adategységet.
>
>

### <a name="remove-owners"></a>Távolítsa el a tulajdonosok
Ugyanúgy, mint bármely az eszköz tulajdonosa társtulajdonosok hozzáadására, bármely eszköz tulajdonosa távolíthatja el a társtulajdonos.

Egy eszköz tulajdonost, aki számára, vagy saját magát eltávolítja a tulajdonos már nem felügyelheti hello eszköz. Ha a hello az eszköz tulajdonosa számára, vagy saját magát eltávolítja a tulajdonos, és nincsenek egyéb közös tulajdonosok, a hello eszköz át nem rendelkezik tulajdonossal állapot tooan.

## <a name="control-visibility"></a>Láthatóságának
Adategységet tulajdonosok hello hello adategységek láthatóságának saját szabályozhatja. hello alapértelmezett toorestrict látható, a Data Catalog képes felderíteni a felhasználók és a nézet hello adategységhez, ahol hello az eszköz tulajdonosa visszaváltható hello láthatósági a **mindenki** túl**tulajdonosok és ezek felhasználók** hello eszköz hello tulajdonságaiban. Tulajdonosok majd adhat hozzá a megadott felhasználók és biztonsági csoportok.

> [!NOTE]
> Amikor csak lehetséges, eszköz tulajdonjoga és a láthatóság érdekében engedélyek hozzá kell rendelni a toosecurity csoportok és nem tooindividual felhasználók.
>
>

## <a name="catalog-administrators"></a>Katalógus-rendszergazdák
Katalógus-rendszergazdák az összes eszköz hello katalógusban társtulajdonosok implicit módon. Eszköz tulajdonosait nem távolítható el látható a rendszergazdák és rendszergazdák kezelhetik tulajdonjoga és hello katalógusban az összes adategységek láthatóságát.

## <a name="summary"></a>Összefoglalás
lehetővé teszi, hogy minden katalógus felhasználók toocontribute hello Data Catalog közösségi modell toometadata és eszköz felderítése, és felderítik. a Data Catalog Standard Edition hello tulajdonjoga és felügyeleti toolimit hello látható és meghatározott eszközök használatát készült.
