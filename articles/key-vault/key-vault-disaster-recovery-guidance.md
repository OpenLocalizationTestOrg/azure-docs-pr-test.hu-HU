---
title: "egy Azure-szolgáltatások ideiglenesek, amely befolyásolja az Azure Key Vault hello eseményben aaaWhat toodo |} Microsoft Docs"
description: "Ismerje meg, milyen toodo egy Azure-szolgáltatások ideiglenesek, amely befolyásolja az Azure Key Vault hello eseményben."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 88eec82ada401a28323b3eea126168185ba4cdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Az Azure Key Vault rendelkezésre állás és redundancia
Az Azure Key Vault funkciókat rétege redundancia toomake meg arról, hogy a kulcsok és titkos továbbra is elérhető tooyour alkalmazás akkor is, ha egyes összetevőinek hello szolgáltatás sikertelen lesz.

hello a kulcstartót tartalmát replikálódnak hello régió és tooa másodlagos régióba belül legalább 150 miles számítógépnél, de hello belül azonos földrajzi hely. Ezzel a megoldással magas tartósság a kulcsok és titkos kulcsok. Lásd: hello [Azure párosítva régiók](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentumban talál részletes információt adott régióban párokat.

Hello kulcstároló szolgáltatáson belül az egyes összetevők sikertelen lesz, ha más összetevők hello régión belül lépést tooserve a kérelem toomake, hogy van-e a funkcióinak csökkenése nélkül működhet. Nem kell tootake bármely művelet tootrigger ez. Automatikusan történik, és transzparens tooyou lesz.

Hello ritka esemény, hogy a teljes Azure-régió nem érhető el, az Azure Key Vault az adott régióban végrehajtott hello kérelmek automatikusan útválasztása (*átadja a feladatokat*) tooa másodlagos régióba. Hello elsődleges régió újból elérhető lesz, ha rendszer kérést átirányítja vissza (*vissza nem sikerült*) toohello elsődleges régióban. Ebben az esetben nem kell tootake bármely művelet, mert ez automatikusan megtörténik.

Van néhány figyelmeztetések toobe tudomást:

* A régió feladatátvétel hello esetben hello szolgáltatás toofail néhány percig is eltarthat,. Ebben az időszakban irányuló kérelmek hello feladatátvételi befejezéséig sikertelen lehet.
* A feladatátvétel befejezése után a kulcstartót csak olvasható módban van. Ebben a módban támogatott kéréseket a következők:
  * Kulcstároló naplófájljainak listája
  * Kulcstároló tulajdonságainak beolvasása
  * Titkos kulcsainak listázása
  * Titkos kulcsok beszerzése
  * Listázása
  * (Tulajdonságainak) kulcs beszerzése
  * Titkosítás
  * Visszafejtés
  * Naplócsonkulási
  * Kicsomagolásához
  * Ellenőrzés
  * Bejelentkezés
  * Biztonsági mentés
* Miután a feladatátvétel nem sikerült vissza, az összes kérelemtípusok (beleértve az olvasás *és* írási kérelmeket szolgál) érhetők el.

