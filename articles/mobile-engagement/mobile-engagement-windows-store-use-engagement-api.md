---
title: "aaaHow tooUse hello az univerzális Windows-Engagement API"
description: "Hogyan tooUse hello az univerzális Windows-Engagement API"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0256b839c28e4ef6c530106408d744038fa711ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-windows-universal"></a>Hogyan tooUse hello az univerzális Windows-Engagement API
Ez a dokumentum egy bővítmény toohello dokumentum [hogyan tooIntegrate Engagement az univerzális Windows-](mobile-engagement-windows-store-integrate-engagement.md): hogyan toouse hello Engagement API tooreport az alkalmazás statisztikái mélysége adatait a biztosít.

Vegye figyelembe, hogy ha csak szeretné Engagement tooreport munkamenetek, a tevékenységek, a összeomlásokat és a technikai információkat az alkalmazás, majd hello legegyszerűbb módja a rendszer minden toomake a `Page` alosztályokat örökölhet hello `EngagementPage` osztály.

Ha azt szeretné, hogy toodo további, például ha tooreport adott eseményeket, hibákat és feladatok van szüksége, vagy ha tooreport az alkalmazás tevékenységei eltérő módon, mint egy hello megvalósított hello `EngagementPage` osztályokat, akkor szükséges, hogy toouse hello Bevonási API.

hello Engagement API által biztosított hello `EngagementAgent` osztály. Toothose módszerek keresztül érheti el `EngagementAgent.Instance`.

Akkor is, ha hello ügynök modul nem lett inicializálva, minden hívás toohello API késleltetve, és végrehajtja újra elérhető hello ügynök esetén.

## <a name="engagement-concepts"></a>Engagement – fogalmak
hello következő részekből pontosítsa közös hello [Mobile Engagement fogalmait](mobile-engagement-concepts.md) hello univerzális Windows platform.

### <a name="session-and-activity"></a>`Session` és `Activity`
Egy *tevékenység* általában egy oldal, amely toosay hello hello alkalmazás társított *tevékenység* kezdődik, amikor hello oldal jelenik meg, és hello lap bezárásakor leáll: hello eset, amikor hello Hello segítségével integrált engagement SDK `EngagementPage` osztály.

De *tevékenységek* is szabályozhatja manuálisan hello Engagement API használatával. Ez lehetővé teszi egy adott oldal több sub részek tooget hello használata ezen a lapon (például milyen gyakran tooknow és mennyi ideig párbeszédpanelek ezen a lapon belül használt) kapcsolatos további részletekért toosplit.

## <a name="reporting-activities"></a>Jelentéskészítési tevékenység
### <a name="user-starts-a-new-activity"></a>Felhasználó elindítja az új tevékenység
#### <a name="reference"></a>Referencia
            void StartActivity(string name, Dictionary<object, object> extras = null)

Toocall kell `StartActivity()` minden alkalommal hello felhasználói tevékenység változik. hello első hívás toothis függvény új felhasználói munkamenet indítása.

> [!IMPORTANT]
> hello SDK automatikusan hello EndActivity metódus meghívja a hello alkalmazás bezárásakor. Ebből kifolyólag javasoljuk toocall hello StartActivity metódus amikor hello tevékenység hello felhasználó módosítja, és hello EndActivity metódus a metódus hívása óta kényszeríti hello aktuális munkamenet toobe tooNEVER hívás befejeződött.
> 
> 

#### <a name="example"></a>Példa
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Felhasználói karakterlánccal végződik-e aktuális tevékenysége
#### <a name="reference"></a>Referencia
            void EndActivity()

Ez befejezi a hello tevékenység és hello munkamenet. Ez a módszer nem célszerű hívni, csak ha valóban tudja, milyen végzett.

#### <a name="example"></a>Példa
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a>Feladatok jelentése
### <a name="start-a-job"></a>Feladat indítása
#### <a name="reference"></a>Referencia
            void StartJob(string name, Dictionary<object, object> extras = null)

Hello feladat tootrack: bizonyos feladatokat is használhat egy meghatározott időtartamra vonatkozóan.

#### <a name="example"></a>Példa
            // An upload begins...

            // Set hello extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Egy feladat vége
#### <a name="reference"></a>Referencia
            void EndJob(string name)

Amint egy tevékenység követi nyomon a feladat le lett állítva, meg kell hívnia hello EndJob metódust a feladat úgy, hogy megadja a hello feladat neve.

#### <a name="example"></a>Példa
            // In hello previous section, we started an upload tracking with a job
            // Then, hello upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a>Jelentési eseményeket
A következő három típusú eseményeket:

* Önálló események
* Munkamenet-események
* Feladat-események

### <a name="standalone-events"></a>Önálló események
#### <a name="reference"></a>Referencia
            void SendEvent(string name, Dictionary<object, object> extras = null)

Önálló események is előfordulhatnak kívül hello egy munkamenet környezetében.

#### <a name="example"></a>Példa
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Munkamenet-események
#### <a name="reference"></a>Referencia
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Munkamenet-eseményeket általában használt tooreport hello műveletek során a munkamenet a felhasználó által végrehajtott is.

#### <a name="example"></a>Példa
**Adatok: nélkül**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Adatokkal:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Feladat-események
#### <a name="reference"></a>Referencia
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Feladat eseményeket általában használt tooreport hello műveletek során a feladat felhasználó által végrehajtott is.

#### <a name="example"></a>Példa
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a>Hibát jelentett
Hibák három típusa van:

* Önálló hibák
* Munkamenet-hibák
* Hibák: feladathiba

### <a name="standalone-errors"></a>Önálló hibák
#### <a name="reference"></a>Referencia
            void SendError(string name, Dictionary<object, object> extras = null)

Ellenkező toosession hibák önálló hibák adódhatnak kívül hello egy munkamenet környezetében.

#### <a name="example"></a>Példa
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Munkamenet-hibák
#### <a name="reference"></a>Referencia
            void SendSessionError(string name, Dictionary<object, object> extras = null)

Munkamenet olyan hello felhasználói érintő a munkamenet során általában használt tooreport hello hibákat tartalmaznak.

#### <a name="example"></a>Példa
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Hibák: feladathiba
#### <a name="reference"></a>Referencia
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Hibák lehetnek feldolgozás alatt álló helyett kapcsolódó tooa kapcsolódó toohello jelenlegi felhasználói munkamenetet.

#### <a name="example"></a>Példa
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a>Jelentéskészítési összeomlik
hello ügynök biztosít két módszer toodeal összeomlik.

### <a name="send-an-exception"></a>Kivétel küldése
#### <a name="reference"></a>Referencia
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Példa
Bármikor kivétel meghívásával küldheti el:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Is használhat egy nem kötelező paraméter tooterminate hello engagement munkamenet hello ugyanannyi időt vesz igénybe, mint hello összeomlási küldésekor. Ebben az esetben hívható toodo:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Ha így tesz, a hello munkamenet és feladatok csak hello összeomlási elküldése után megszűnik.

### <a name="send-an-unhandled-exception"></a>Nem kezelt kivétel küldése
#### <a name="reference"></a>Referencia
            void SendCrash(Exception e)

Engagement is biztosít a metódus nem kezelt toosend kivételek, ha **LETILTOTT** Engagement automatikus **összeomlási** reporting. Ez különösen akkor hasznos, ha az alkalmazás UnhandledException eseménykezelő hello használni.

Ezt a módszert fog **mindig** hello engagement munkamenet és feladatok meghívott után leáll.

#### <a name="example"></a>Példa
Használat tooimplement saját UnhandledExceptionEventArgs kezelő. Adja hozzá például hello `Current_UnhandledException` hello metódusában `App.xaml.cs` fájlt:

            // In your App.xaml.cs file

            // Code tooexecute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

Az App.xaml.cs "Nyilvános App() megkeresése" hozzáadása:

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a>Eszközazonosító
            String EngagementAgent.Instance.GetDeviceId()

A metódus hívása kaphat hello engagement-eszközazonosító.

## <a name="extras-parameters"></a>Kiegészítő funkciók paraméterek
Lehet, hogy az adatokat tetszőleges csatolt tooan esemény, a hiba, a tevékenység vagy a feladat. Ezen adatok segítségével dictionary szervezhetők. Kulcsok és értékek bármilyen típusú lehet.

Kiegészítő funkciók adatok szerializálva vannak, így ha azt szeretné, tooinsert a saját típusát, a kiegészítő funkciók kell tooadd egy adategyezmény ehhez a típushoz.

### <a name="example"></a>Példa
Azt a hozzon létre egy új osztályt "Személy".

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Ezt követően adunk hozzá egy `Person` példány tooan extra.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> Ha más típusú objektumokat, győződjön meg arról a ToString() módszer megvalósított tooreturn emberi olvasható karakterláncnak.
> 
> 

### <a name="limits"></a>Korlátok
#### <a name="keys"></a>Kulcsok
Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).

#### <a name="size"></a>Méret
Kiegészítő funkciók korlátozva túl**1024** hívás karakteres.

## <a name="reporting-application-information"></a>Jelentéskészítési alkalmazással kapcsolatos adatok
### <a name="reference"></a>Referencia
            void SendAppInfo(Dictionary<object, object> appInfos)

Követés hello SendAppInfo() függvény használatával információkat (vagy egyéb alkalmazás konkrét információkat) kézzel jelentést.

Vegye figyelembe, hogy az adatküldés Növekményesen: az adott eszköz folyamatosan csak hello legújabb egy adott kulcs értékét. Esemény kiegészítő funkciók, például egy szótár használata\<objektum, objektum\> tooattach adatokat.

### <a name="example"></a>Példa
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Korlátok
#### <a name="keys"></a>Kulcsok
Minden kulcs hello objektumban meg kell egyeznie a következő reguláris kifejezésnek hello:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Ez azt jelenti, hogy kulcsok betűk, számok és aláhúzásjelek követ legalább egy betűvel kell kezdődnie (\_).

#### <a name="size"></a>Méret
Az alkalmazásadatok korlátozódik túl**1024** hívás karakteres.

Hello az előző példában hello toohello server elküldött JSON 44 karakter hosszú:

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a>Naplózás
### <a name="enable-logging"></a>Naplózás engedélyezése
hello SDK konfigurált tooproduce vizsgálati naplók hello IDE konzolon lehet.
Ezek a naplók alapértelmezés szerint nincsenek aktiválva. toocustomize a, frissítés hello tulajdonság `EngagementAgent.Instance.TestLogEnabled` hello érték hello elérhető tooone `EngagementTestLogLevel` számbavételi, például:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

