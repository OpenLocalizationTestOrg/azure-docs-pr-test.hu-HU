---
title: "aaaHow tooUse hello Engagement API IOS rendszerű eszközökön"
description: "Legújabb iOS SDK - tooUse hogyan Engagement API hello iOS"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7fb9b95ad319cf3b1e2de81b5d6aee5b30266069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-ios"></a>Hogyan tooUse hello IOS Engagement API
Ez a dokumentum hogyan egy bővítmény toohello dokumentum tooIntegrate Engagement IOS: hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái mélysége adatait a biztosít.

Ne feledje, hogy ha munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás csak szeretné Engagement tooreport majd hello legegyszerűbb módja van toomake az egyéni `UIViewController` örökölt objektumok megfelelő hello `EngagementViewController` osztály .

Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementViewController` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.

hello Engagement API által biztosított hello `EngagementAgent` osztály. Ez az osztály példánya lekérhetők hívó hello `[EngagementAgent shared]` statikus metódus (vegye figyelembe, hogy hello `EngagementAgent` objektumot adott vissza az Egypéldányos).

Mielőtt API-k hívja, hello `EngagementAgent` hello metódus meghívásával objektumot inicializálni kell`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

## <a name="engagement-concepts"></a>Engagement – fogalmak
hello következő részekből pontosítsa közös hello [Mobile Engagement fogalmait](mobile-engagement-concepts.md) hello iOS platformra.

### <a name="session-and-activity"></a>`Session` és `Activity`
Egy *tevékenység* általában egy képernyő hello alkalmazás, amely toosay hello társított *tevékenység* kezdődik, amikor hello képernyő jelenik meg, és leállítja az üdvözlő képernyőt bezárásakor: Ez a hello eset, amikor hello Engagement SDK integrálva van a hello segítségével `EngagementViewController` osztályok.

De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával. Így toosplit egy adott képernyő több sub részek tooget kapcsolatos további részletekért hello használata ezen a képernyőn (például milyen gyakran tooknown és mennyi ideig párbeszédpanelek használt ezen a képernyőn belül).

## <a name="reporting-activities"></a>Jelentéskészítési tevékenység
### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Toocall kell `startActivity()` minden alkalommal hello felhasználói tevékenység változik. hello első hívás toothis függvény új felhasználói munkamenet indítása.

### <a name="user-ends-his-current-activity"></a>Felhasználói karakterlánccal végződik-e aktuális tevékenysége
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> Meg kell **soha** hívja meg a függvényt önállóan, kivéve, ha azt szeretné, hogy az alkalmazás több munkamenet toosplit használatát: toothis függvény volna end hívása hello azonnal, a jelenlegi munkamenet, így a későbbi hívása túl`startActivity()`volna indítson egy újat. Ez a funkció automatikusan hívja hello SDK az alkalmazás bezárása után.
> 
> 

## <a name="reporting-events"></a>Jelentési eseményeket
### <a name="session-events"></a>Munkamenet-események
Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.

**További adatok nélkül. példa:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Példa a további adatokat:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Önálló események
Ellentétes toosession események, különálló esemény egy munkamenet környezetében hello kívül is használható.

**Példa**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a>Hibát jelentett
### <a name="session-errors"></a>Munkamenet-hibák
Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.

**Példa**

    /** hello user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* hello user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Önálló hibák
Ellentétes toosession hibák, önálló hibák kívül hello egy munkamenet környezetében használható.

**Példa**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a>Feladatok jelentése
**Példa**

Tegyük fel, hogy a bejelentkezési folyamat tooreport hello időtartama:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>A feladat során hibák jelentése
Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.

**Példa**

Tegyük fel, hogy tooreport hiba történt a bejelentkezési folyamat során:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try toosign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>A feladat során események
Események lehet feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.

**Példa**

Tegyük fel, közösségi hálózata, és egy feladat tooreport hello teljes ideje mely hello felhasználói pedig csatlakoztatott toohello server használjuk. hello felhasználói fogadhat üzeneteket az ismerősök, ez az esemény feladat.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a>További paraméterek
Lehet, hogy az adatokat tetszőleges csatolt tooevents, hibákat, tevékenységeket és feladatokat.

Ezek az adatok szervezhetők, iOS tartozó NSDictionary osztályt használja.

Vegye figyelembe, hogy a kiegészítő funkciók tartalmazhatnak `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` vagy más `NSDictionary` példányok.

> [!NOTE]
> hello kiegészítő paraméterrel szerializált a JSON-ban. Ha azt szeretné, hogy toopass különböző objektumok, mint hello fent leírtaktól, meg kell valósítani hello metódus az osztály a következő:
> 
> -(NSString*) JSONRepresentation;
> 
> hello metódus az objektum JSON-ábrázolását kell visszaadnia.
> 
> 

### <a name="example"></a>Példa
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Korlátok
#### <a name="keys"></a>Kulcsok
Minden kulcs hello `NSDictionary` meg kell egyeznie a következő reguláris kifejezésnek hello:

`^[a-zA-Z][a-zA-Z_0-9]*`

Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).

#### <a name="size"></a>Méret
Kiegészítő funkciók korlátozva túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement ügynök).

Hello az előző példában hello toohello server elküldött JSON 58 karakter hosszú:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Jelentéskészítési alkalmazással kapcsolatos adatok
Manuálisan jelentheti a nyomkövetési adatokat (vagy más alkalmazás egyedi információt) hello `sendAppInfo:` függvény.

Vegye figyelembe, hogy ezek az információt elküldi Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét.

Például a esemény kiegészítő funkciók, hello `NSDictionary` osztály használt tooabstract alkalmazással kapcsolatos információk, vegye figyelembe, hogy tömbállandó vagy alárendelt szótárak minősül, egyszerű karakterlánc (JSON-szerializálás).

**Példa**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Korlátok
#### <a name="keys"></a>Kulcsok
Minden kulcs hello `NSDictionary` meg kell egyeznie a következő reguláris kifejezésnek hello:

`^[a-zA-Z][a-zA-Z_0-9]*`

Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).

#### <a name="size"></a>Méret
Alkalmazásadatok esetén egyre korlátozódik túl**1024** karakter / hívás (egyszer kódolású JSON hello Engagement ügynök).

Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:

    {"birthdate":"1983-12-07","gender":"female"}
