---
title: "aaaSign modulok több hiba után"
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
ms.openlocfilehash: 48d137dc3abf65287cb3b9ba8a6ff10340f6741f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-ins-after-multiple-failures"></a>Több hibát követő bejelentkezések
Ez a jelentés azt jelzi, felhasználók, akik a sikeres bejelentkezést követően több egymást követő sikertelen bejelentkezési kísérlet után. Lehetséges okok a következők:

* Felhasználói kellett elfelejti a jelszavát</li><li>Felhasználó hello áldozata sikeres jelszó-előállító találgatásos támadás

Ez a jelentés eredményeinek jelennek meg, hány egymást követő sikertelen bejelentkezési kísérletek előzetes toohello sikeres bejelentkezés hello és társított időbélyeg hello első sikeres bejelentkezés.

**Beállítások jelentés**: hello minimális számú egymást követő sikertelen bejelentkezési kísérletek kell hello jelentés megjelenítése előtt konfigurálhatja. Állítaná toothis van módosításokat fontos toonote, hogy a módosítások nem lesznek alkalmazott tooany meglévő sikertelen bejelentkezések a meglévő jelentés jelenleg látható. Azonban alkalmazott tooall későbbi bejelentkezések fogja. A licenccel rendelkező rendszergazdák csak is végezhető módosítások toothis jelentés.

![Több hiba után indított bejelentkezések](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

