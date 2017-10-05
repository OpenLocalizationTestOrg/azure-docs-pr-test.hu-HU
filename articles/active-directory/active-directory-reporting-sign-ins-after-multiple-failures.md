---
title: "Több hiba után indított bejelentkezések"
description: "Ez a jelentés azt jelzi, felhasználók, akik a sikeres bejelentkezést követően több egymást követő sikertelen bejelentkezési kísérlet után."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: femila
editor: 
ms.assetid: e4ec1a39-9c20-418f-8a75-6497d0117176
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: e55e0145adbdb1f41a8b8753d5555f20e96bf161
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-after-multiple-failures"></a>Több hibát követő bejelentkezések
Ez a jelentés azt jelzi, felhasználók, akik a sikeres bejelentkezést követően több egymást követő sikertelen bejelentkezési kísérlet után. Lehetséges okok a következők:

* Felhasználói kellett elfelejti a jelszavát</li><li>Felhasználó az áldozat sikeres jelszó-előállító találgatásos támadás kényszerítése

Ez a jelentés eredményeinek megtudhatja, hány egymást követő sikertelen bejelentkezési kísérlete sikeres bejelentkezés előtt készített és az első sikeres bejelentkezés társított időbélyeg.

**Beállítások jelentés**: a lehető legkevesebb egymást követő sikertelen bejelentkezési kísérletek kell a jelentés megjelenítése előtt konfigurálható. Amikor módosítja ezt a beállítást fontos megjegyezni, hogy ezeket a módosításokat nem alkalmazandó összes meglévő sikertelen bejelentkezési modulok jelenleg jelenik meg a meglévő jelentés. Azonban akkor fog alkalmazni, az összes későbbi bejelentkezések. Ez a jelentés módosításai csak licenccel rendelkező rendszergazdák is végezhető.

![Több hiba után indított bejelentkezések](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

