---
title: "aaaHow toosecure hozzáférés tooAzure Data Catalog |} Microsoft Docs"
description: "Ez a cikk azt ismertetik, hogyan toosecure a data catalog és az adategységeket."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/17/2017
ms.author: maroche
ms.openlocfilehash: d7c35fd57d8add1cdc152b75f111879288e1548f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-access-toodata-catalog-and-data-assets"></a>Hogyan érik el toosecure toodata katalógust és az adatok eszközök
> [!IMPORTANT]
> Ez a funkció csak a hello az Azure Data Catalog standard kiadásában érhető el.

Az Azure Data Catalog lehetővé teszi a toospecify, ki férhet hozzá hello a data catalog, és milyen műveletek (regisztrálásához, megjegyzésekkel, vegye saját tulajdonba) a metaadatok hello katalógusban ellátására. 

## <a name="catalog-users-and-permissions"></a>Katalógus-felhasználók és engedélyek
toogive egy felhasználó vagy egy csoport hello tooa a data catalog eléréséhez, és az engedélyek beállítása:

1. A hello [a data CATALOG kezdőlap](http://www.azuredatacatalog.com), kattintson a **beállítások** hello eszköztáron.

    ![a Data catalog - beállítások](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Hello-beállítások lapján bontsa ki a hello **katalógus felhasználói** szakasz.
    ![Katalógus - felhasználók hozzáadása](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. Adja meg a teljesen minősített hello **felhasználónév** vagy hello neve **biztonsági csoport** az Azure Active Directory (AAD) társított hello katalógus hello. Használjon vesszőt (', '), egy elválasztó hozzáadásakor több mint egy felhasználó vagy csoport.
    ![Katalógus felhasználói - felhasználók és csoportok](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Nyomja le az **ENTER** vagy **lapon** kívül hello szövegmezőben. 
6.  Ellenőrizze, hogy minden engedély (**jegyzet**, **regisztrálása**, és **tulajdonba**) toothese felhasználók vagy csoportok alapértelmezés szerint tartoznak. Ez azt jelenti, hogy hello felhasználó vagy csoport is [adategységek regisztrálása]( data-catalog-how-to-register.md), [adhat megjegyzéseket az adategységekhez]( data-catalog-how-to-annotate.md), és [az adategységek saját tulajdonba]( data-catalog-how-to-manage.md). 
    ![Katalógus felhasználók – az alapértelmezett engedélyek](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  egy felhasználó vagy csoport toogive csak hello olvasási hozzáférés toohello katalógus, törölje a jelet hello **megjegyzésekkel** lehetőség az adott felhasználó vagy csoport. Ha így tesz, hello felhasználó vagy csoport nem adhat megjegyzéseket az adategységekhez hello katalógusban, de is megtekinthetik. 
8.  toodeny egy felhasználó vagy csoport regisztrálja az adategységek, törölje a jelet hello **regisztrálása** lehetőség az adott felhasználó vagy csoport.
9.  a felhasználó egy adategységet, törölje a jelet hello tulajdonba toodeny **saját tulajdonba** lehetőség az adott felhasználó vagy csoport. 
10. a katalógus felhasználók számára, egy felhasználó/csoport toodelete kattintson **x** hello felhasználó vagy csoport hello lista hello alján. 
    ![Katalógus-felhasználók - felhasználó törlése](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Azt javasoljuk, hogy közvetlenül hozzáadni a felhasználók hozzáadásával helyett a biztonsági csoportok toocatalog felhasználók és engedélyek hozzárendelése. Adja hozzá a felhasználók toohello biztonsági csoportok, amelyek megfelelnek a szerepkörök és a szükséges hozzáférési toohello katalógus.

## <a name="special-considerations"></a>Különleges szempontok

- hello jogosultságokkal toosecurity csoportok additívak. Tegyük fel a felhasználó két szerepel. Egy csoport rendelkezik megjegyzésekkel engedélyek és egyéb csoport nem rendelkezik megjegyzésekkel engedélyeket. Ezt követően felhasználó engedélyekkel rendelkezik megjegyzésekkel. 
- hello engedélyeket explicit módon tooa felhasználót felülbírálás hello jogosultságait toogroups toowhich hello felhasználó tartozik. Hello előző példában tegyük fel például, kifejezetten hello felhasználó toocatalog felhasználók, és ne rendelje hozzá a jegyzet engedélyeket. hello felhasználói nem adhat megjegyzéseket az adategységekhez, annak ellenére, hogy hello felhasználó tagja egy csoportot, amely rendelkezik engedélyekkel megjegyzésekkel.

## <a name="next-steps"></a>Következő lépések
- [Ismerkedés az Azure Data Catalog szolgáltatással](data-catalog-get-started.md)

