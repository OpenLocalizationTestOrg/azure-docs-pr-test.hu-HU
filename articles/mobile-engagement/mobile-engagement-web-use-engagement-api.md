---
title: Mobile Engagement webes SDK API-k aaaAzure |} Microsoft Docs
description: "Azure Mobile Engagement hello legújabb frissítésekről és hello webes SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8a87d5ac-d8b7-4a0d-bdee-414dbcc561b2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: ec1261d6ad573b8c3ad6d5f616ab7bbe560d6fe2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-mobile-engagement-api-in-a-web-application"></a>A webalkalmazás az Azure Mobile Engagement API hello használata
Ez a dokumentum egy hozzáadása toohello dokumentumot, amely azt ismerteti, hogyan túl[integrálja a Mobile Engagement egy webalkalmazásban](mobile-engagement-web-integrate-engagement.md). Hogyan toouse hello Azure Mobile Engagement API tooreport kapcsolatos részletes adatokat biztosít az alkalmazás statisztikái.

hello Mobile Engagement API által biztosított hello `engagement.agent` objektum. az alapértelmezett Azure Mobile Engagement webes SDK alias hello `engagement`. Ez az alias hello SDK konfigurációból is definiálja újra.

## <a name="mobile-engagement-concepts"></a>Mobile Engagement – fogalmak
hello következő részekből pontosítsa közös [Mobile Engagement – fogalmak](mobile-engagement-concepts.md) hello webes platform jöhet létre.

### <a name="session-and-activity"></a>`Session` és `Activity`
Hello felhasználó két tevékenység között csak néhány másodpercig inaktív marad, ha hello felhasználói műveletek sorozata alapján oszlik, két, különböző munkamenetek. Ezek néhány másodpercig hello munkamenet időkorlátja nevezzük.

Ha a webes alkalmazás önmagában nem deklarálható felhasználói tevékenységek hello vége (hívó hello által `engagement.agent.endActivity` függvény), hello a Mobile Engagement-kiszolgáló automatikusan lejárnak hello felhasználói munkamenet hello alkalmazáslap bezárása után három percen belül. Ez a lehetőség hello server munkamenet időkorlátja.

### `Crash`
Nem kezelt JavaScript-kivételeknek az automatizált jelentések nem jönnek létre, alapértelmezés szerint. Összeomlások jelentést azonban hello segítségével manuálisan `sendCrash` (lásd hello összeomlik a jelentéskészítő) működik.

## <a name="reporting-activities"></a>Jelentéskészítési tevékenység
Amikor a felhasználó elindít egy új tevékenységet és hello felhasználói addig tart, amíg a jelenlegi tevékenység hello felhasználói tevékenység Reporting tartalmaz.

### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység
    engagement.agent.startActivity("MyUserActivity");

Toocall kell `startActivity()` minden felhasználói tevékenység változik. hello első hívás toothis függvény új felhasználói munkamenet indítása.

### <a name="user-ends-hello-current-activity"></a>Felhasználói hello aktuális tevékenység befejeződik
    engagement.agent.endActivity();

Toocall kell `endActivity()` legalább egyszer amikor hello felhasználó befejezi a legutolsó tevékenység. Ebben értesíti hello Mobile Engagement webes SDK-t, hogy hello felhasználó jelenleg inaktív, valamint, hogy a felhasználói munkamenet hello toobe kell bezárása után hello munkamenet időkorlátja lejár. Ha meghívja a `startActivity()` előtt hello munkamenet időkorlátja lejár, a hello munkamenet egyszerűen folytatódik.

Mivel a nem megbízható hívásakor hello navigator ablak lezárt, akkor gyakran nehéz vagy lehetetlen toocatch hello vége felhasználói tevékenységet egy webes környezeten belül. Hogy miért van hello a kiszolgáló automatikusan lejárnak a Mobile Engagement hello felhasználói munkamenet hello alkalmazáslap bezárása után három percen belül.

## <a name="reporting-events"></a>Jelentési eseményeket
Jelentési események foglalkozik, munkamenet és önálló események.

### <a name="session-events"></a>Munkamenet-események
Munkamenet-események általában használt tooreport hello műveletek hello felhasználói munkamenet során a felhasználó által végrehajtott.

**További adatok nélkül. példa:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Példa a további adatokat:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Önálló események
Munkamenet események eltérően önálló események egy munkamenet környezetében hello kívül is előfordulhatnak.

Ezzel ``engagement.agent.sendEvent`` helyett ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Hibát jelentett
Hibákkal Reporting munkamenet hibák és önálló hibákat tartalmazza.

### <a name="session-errors"></a>Munkamenet-hibák
Munkamenet általában olyan használt tooreport hello hibákat tartalmaznak, amelyek hatással vannak az hello felhasználói hello felhasználói munkamenet során.

**További adatok nélkül. példa:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Példa a további adatokat:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Önálló hibák
Munkamenet hibák eltérően a önálló hibák adódhatnak kívül hello egy munkamenet környezetében.

Ezzel `engagement.agent.sendError` helyett `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Feladatok jelentése
Jelentés a jelentéskészítési hibák és események, a feladatok során felmerülő, és az összeomlások feladatokat tartalmazza.

**Példa**

Ha azt szeretné, hogy egy AJAX-kérelem toomonitor, hello következő használja:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>A feladat során hibát jelentett
Hibák lehetnek kapcsolódó tooa helyett toohello jelenlegi felhasználói munkamenet feladat futtatásával.

**Példa**

Ha azt szeretné, hogy tooreport hibát adott vissza, ha egy AJAX-kérelem sikertelen:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>A feladat során jelentési eseményeket
Események lehet feladat futtatásával helyett toohello jelenlegi felhasználói munkamenet köszönhetően toohello kapcsolódó tooa `engagement.agent.sendJobEvent` függvény.

Ez a funkció ugyanúgy működik, mint `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Összeomlások Reporting
Használjon hello `sendCrash` függvény tooreport manuálisan összeomlik.

Hello `crashid` argumentum egy karakterlánc, amely azonosítja a összeomlási hello típusú.
Hello `crash` általában argumentum hello Veremkivonat a hello összeomlási karakterláncként.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>További paraméterek
Csatolhat tetszőleges adatok tooan esemény, a hiba, a tevékenység vagy a feladatot.

lehet, hogy az hello adatokat bármely JSON-objektum (de nem tömb vagy egyszerű típusú).

**Példa**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Korlátok
Tooextra paraméterek vonatkozó határértékeket hello területein reguláris kifejezéseket a kulcsokat, értéktípusokat és méretét.

#### <a name="keys"></a>Kulcsok
Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:

    ^[a-zA-Z][a-zA-Z_0-9]*

Ez azt jelenti, hogy a kulcsokat legalább egy betűvel kell kezdődnie követ betűk, számok és aláhúzásjelek (\_).

#### <a name="values"></a>Értékek
Értékek: korlátozott toostring, számát, és logikai típusát.

#### <a name="size"></a>Méret
Kiegészítő funkciók korlátozva too1, 024 karakteres hívás (miután a Mobile Engagement webes SDK hello kódolja a JSON-ban).

## <a name="reporting-application-information"></a>Jelentéskészítési alkalmazással kapcsolatos adatok
Manuálisan jelentheti a nyomkövetési adatokat (vagy bármely más alkalmazás-specifikus adatait) hello segítségével `sendAppInfo()` függvény.

Vegye figyelembe, hogy ezt az információt elküldi Növekményesen. Csak hello legújabb egy adott kulcs értékét egy adott eszközhöz tárolni fogja.

Esemény kiegészítő funkciók, például a bármely JSON objektum tooabstract alkalmazással kapcsolatos adatok is használhatja. Vegye figyelembe, hogy a tömb, vagy az alárendelt objektumok számít-e egyszerű karakterlánc (JSON-szerializálás).

**Példa**

Íme egy kódminta küldő hello felhasználói nemét és születési dátuma:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Korlátok
Tooapplication információk korlátokkal vannak hello területein reguláris kifejezéseket a kulcsokat és méretét.

#### <a name="keys"></a>Kulcsok
Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:

    ^[a-zA-Z][a-zA-Z_0-9]*

Ez azt jelenti, hogy a kulcsokat legalább egy betűvel kell kezdődnie követ betűk, számok és aláhúzásjelek (\_).

#### <a name="size"></a>Méret
Az alkalmazásadatok korlátozott too1, 024 karakteres hívás (miután a Mobile Engagement webes SDK hello kódolja a JSON-ban).

A fenti példa hello hello JSON küldeni toohello server 44 karakter:

    {"birthdate":"1983-12-07","gender":"female"}
