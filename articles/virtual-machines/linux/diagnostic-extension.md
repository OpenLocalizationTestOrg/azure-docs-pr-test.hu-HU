---
title: "Számítási aaaAzure - Linux diagnosztikai bővítmény |} Microsoft Docs"
description: "Hogyan tooconfigure hello Azure Linux diagnosztikai bővítmény (LAD) toocollect metrikák eseményeit és Azure-ban futó Linux virtuális gépek."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: 2b27ec00a876ded359a75170b407e28c40d8445d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-linux-diagnostic-extension-toomonitor-metrics-and-logs"></a>Linux diagnosztikai bővítmény toomonitor metrikák és a naplókat

Ez a dokumentum ismerteti a 3.0-s vagy újabb a hello Linux diagnosztikai bővítmény verzió.

> [!IMPORTANT]
> 2.3-as és a régebbi verziójával kapcsolatos információkért lásd: [Ez a dokumentum](./classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Bevezetés

hello Linux diagnosztikai bővítmény segít a felhasználó a figyelő hello a Microsoft Azure-on futó Linux virtuális gépek állapotát. Hello a következő képességekkel rendelkezik:

* A virtuális gép hello rendszer teljesítménymutatók gyűjt, és a megadott tábla kijelölt tárfiókban tárolja azokat.
* Alkalmazásnapló-események lekéri a syslog, és tárolja őket a megadott tábla kijelölt tárfiókot hello.
* Lehetővé teszi, hogy a felhasználók toocustomize hello adatok metrikák gyűjtött és a feltöltése.
* Lehetővé teszi a felhasználók toocustomize hello syslog létesítményekben és súlyossági szintek az eseményeket, amelyek gyűjtött és a feltöltése.
* Lehetővé teszi, hogy a felhasználók tooupload megadott napló fájlok tooa kijelölt tároló tábla.
* Támogatja a küldés, metrikákat és naplózási események tooarbitrary EventHub végpontok és blobok JSON-formátumú hello kijelölt tárfiókot.

A bővítmény mindkét Azure üzembe helyezési modellel működik.

## <a name="installing-hello-extension-in-your-vm"></a>A virtuális gép hello-bővítmény telepítése

A bővítmény hello Azure PowerShell-parancsmagok, az Azure parancssori felület parancsfájlok vagy Azure központi telepítési sablonok használatával engedélyezheti. További információkért lásd: [bővítményeit biztosító funkciókat](./extensions-features.md).

hello Azure-portál nem használt tooenable vagy LAD 3.0 konfigurálása. Ehelyett azt telepíti, és konfigurálja a 2.3 verziója. Az Azure portál diagramok és értesítések működéséhez hello bővítmény verzióját is adataival.

A telepítési utasításokat és egy [letölthető mintakonfiguráció](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0-s konfigurálása:

* rögzítési és tároló hello metrikák LAD 2.3; által biztosított volt
* fájl rendszer metrika, új tooLAD 3.0; hasznos készlete rögzítése
* hello alapértelmezett syslog gyűjtemény LAD 2.3; által engedélyezett rögzítése
* Engedélyezze a hello Azure portál felhasználói diagramkészítési, és a virtuális gép metrikák riasztást küld.

hello letölthető beállítás csak egy példa; Módosítsa ezt a toosuit saját igényeinek.

### <a name="prerequisites"></a>Előfeltételek

* **Az Azure Linux ügynök 2.2.0 verzió vagy újabb**. A legtöbb Azure virtuális gép Linux gyűjtemény lemezképei 2.2.7 verzióját tartalmazzák, vagy később. Futtatás `/usr/sbin/waagent -version` tooconfirm hello verziója van telepítve, a virtuális gép hello. Virtuális gép hello hello Vendég ügynök egy régebbi verziója fut, hajtsa végre a [ezeket az utasításokat](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) tooupdate azt.
* **Azure parancssori felület (CLI)**. [Azure CLI 2.0 hello beállítása](https://docs.microsoft.com/cli/azure/install-azure-cli) környezet a számítógépen.
* hello wget parancs, ha már nincs: futtatása `sudo apt-get install wget`.
* Meglévő Azure-előfizetése és a meglévő tárolási fiók belül toostore hello adatokat.

### <a name="sample-installation"></a>A minta telepítése

Adja meg helyes paraméterek hello hello első három sort, majd hajtsa végre ezt a parancsfájlt a legfelső szintű:

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login tooAzure first before anything else
az login

# Select hello subscription containing hello storage account
az account set --subscription <your_azure_subscription_id>

# Download hello sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build hello VM resource ID. Replace storage account name and resource ID in hello public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build hello protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure tooinstall and enable hello extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

hello mintakonfiguráció hello URL-címet, és annak tartalmát, amelyek tulajdonos toochange. Töltse le a hello portálbeállítások JSON-fájl egy példányát, és az igényeinek megfelelően. A sablonok vagy automation, hozhat létre egy saját másolat ahelyett, hogy letöltése URL-címet minden alkalommal, amikor használja.

### <a name="updating-hello-extension-settings"></a>Hello bővítmény beállításainak frissítése folyamatban

Miután megváltoztatta a védett vagy nyilvános beállításai, telepíteni őket toohello VM futtatásával hello ugyanezt a parancsot. Változás a hello-beállítások, ha frissített hello beállítások toohello bővítmény küldése. LAD hello konfiguráció újratöltése, és újraindítja a saját magát.

### <a name="migration-from-previous-versions-of-hello-extension"></a>Hello bővítmény korábbi verzióiról való áttelepítés

hello hello bővítmény legújabb verziója van **3.0**. **A régi verziókat (2.x) elavultak, ezért lehet, hogy közzé nem tett, vagy azt követően 2018 július 31**.

> [!IMPORTANT]
> A bővítmény legfrissebb változtatásokat toohello konfigurációs hello bővítmény vezet be. Egy ilyen módosítás tooimprove hello biztonsági hello bővítmény; Ennek eredményeképpen visszamenőleges 2.x kompatibilitást sikerült nem kell tartani. Hello bővítmény Publisher ehhez a kiterjesztéshez is hello közzétevője hello 2.x verziója eltérő.
>
> a 2.x toothis új hello-kiterjesztés verziószámának toomigrate, el kell távolítania hello régi bővítmény (a hello régi közzétevő neve), majd a telepítés 3 hello bővítmény verziója.

Javaslatok:

* Telepítse a hello bővítmény engedélyezve van az automatikus alverzió frissítését.
  * A klasszikus üzembe helyezési modellt virtuális gépeket adja meg a "3.*" hello verzió telepítésekor hello bővítmény Azure XPLAT parancssori felületen vagy a Powershellen keresztül.
  * Az Azure Resource Manager telepítési modellhez tartozó virtuális gépek esetén tartalmazhat ""autoUpgradeMinorVersion": true" hello VM központi telepítési sablon.
* A LAD 3.0 új/eltérőek a tárolási fiók használata. Van több kis incompatibilities LAD 2.3 és LAD 3.0 között, amelyek megosztása a problémás fiók:
  * LAD 3.0 tárolja a syslog-események egy táblát, amely egy másik nevet.
  * a karakterláncok hello counterSpecifier `builtin` metrikák LAD 3.0 különböznek.

## <a name="protected-settings"></a>Védett beállításai

Ez a konfigurációs adatokat bizalmas adatokat, amelyeket védeni kell a nyilvános nézet, például a tároló hitelesítő adatait tartalmazza. Ezek a beállítások esetén az átvitt tooand hello bővítmény titkosított formában tárolja.

```json
{
    "storageAccountName" : "hello storage account tooreceive data",
    "storageAccountEndPoint": "hello hostname suffix for hello cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

Név | Érték
---- | -----
storageAccountName | hello neve, amelyben hello bővítmény adatot ír hello tárfiók.
storageAccountEndPoint | (választható) hello végpont melyik hello storage-fiókban található hello felhő azonosítása. Ha ez a beállítás hiányzik, LAD alapértelmezés szerint használt érték toohello Azure nyilvános felhőjében `https://core.windows.net`. a tárfiók a Németországi Azure, Azure Government vagy Azure Kína toouse megfelelően állítsa ezt az értéket.
storageAccountSasToken | Egy [fiók SAS-jogkivonat](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) a Blob és Table szolgáltatásait (`ss='bt'`), megfelelő toocontainers és objektumok (`srt='co'`), amely hozzáadásához létrehozása, listában frissítése, és írási engedélyekkel (`sp='acluw'`). Tegye *nem* hello bevezető kérdőjel (?) tartalmazza.
mdsdHttpProxy | (választható) HTTP proxy szükséges információkat tooenable hello bővítmény tooconnect toohello megadott tárfiók és végpontot.
sinksConfig | (választható) Alternatív célok toowhich metrikákkal és eseményekkel részleteit kézbesítése. egyes adatokat fogadó hello bővítmény által támogatott hello részleteit az alábbi hello szakaszok ismertetnek.

Könnyen összeállíthat szükséges hello SAS-jogkivonat hello Azure-portálon keresztül.

1. Válassza ki a kívánt hello bővítmény toowrite hello általános célú tárfiókok fiók toowhich
1. Válassza a "Megosztás hozzáférési aláírás" hello hello bal oldali menü részéből beállításai
1. Ellenőrizze az előzőekben leírt hello megfelelő szakaszait
1. Hello "Készítése SAS" gombra.

![Kép](./media/diagnostic-extension/make_sas.png)

Másolás hello SAS létre hello storageAccountSasToken mezőbe; Távolítsa el a hello bevezető kérdőjel ("?").

### <a name="sinksconfig"></a>sinksConfig

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

Ez a szakasz választható toowhich hello bővítmény összegyűjti az hello információkat küld a további célok meghatározása. hello "fogadó" tömb minden további adatokat fogadó egy objektumot tartalmaz. "típus" attribútum meghatározza, hogy hello hello hello objektum a többi attribútumával.

Elem | Érték
------- | -----
név | Egy karakterláncot toorefer toothis fogadó részeiben hello bővítménykonfiguráció használják.
type | a fogadó múltbeli hello típusa. Határozza meg, más értékek hello (ha vannak) az ilyen típusú példányok.

Hello Linux diagnosztikai bővítmény 3.0-s verziója két fogadó-típusokat támogatja: az EventHub és JsonBlob.

#### <a name="hello-eventhub-sink"></a>hello EventHub fogadó

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

hello "sasURL" bejegyzés hello közzé kell tenni a teljes URL-címet, beleértve a SAS-jogkivonat, hello Eseményközpont toowhich adatokat tartalmazza. LAD egy házirendet, amely lehetővé teszi, hogy a hello küldési jogcím elnevezési Aláírást igényel. Példa:

* Hozzon létre egy Event Hubs-névtér neve`contosohub`
* Létrehoz egy Eseményközpontot hello névtér neve`syslogmsgs`
* Megosztott hozzáférési házirend létrehozása a hello nevű Eseményközpont `writer` , hogy lehetővé teszi, hogy hello küldési jogcím

Ha létrehozott egy SAS jó 2018. január 1. az UTC éjfél hello sasURL érték lehet:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

További információ a SAS-jogkivonatokat előállító az Event Hubs: [Ez a weblap](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

#### <a name="hello-jsonblob-sink"></a>hello JsonBlob fogadó

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

Adatok irányítja a rendszer az Azure-tárfiókba blobok tooa JsonBlob fogadó tárolja. Minden példánya LAD blob minden fogadó név óránként hoz létre. Minden egyes blob mindig tartalmaz egy szintaktikailag érvényes JSON-tömb objektum. Új bejegyzések i kerülnek toohello tömb. Blobok egy azonos nevet, amint hello fogadó hello-tárolóban vannak tárolva. az Azure storage szabályok hello blob tároló nevének alkalmazni JsonBlob nyelő toohello nevek: 3 és 63 kisbetűs alfanumerikus ASCII-karaktereket, és kötőjelek között.

## <a name="public-settings"></a>Nyilvános beállításai

Ez a struktúra hello bővítmény által összegyűjtött hello adatokat szabályozó beállítások több blokkot tartalmaz. Minden beállítás nem kötelező. Ha megad `ladCfg`, meg kell adni `StorageAccount`.

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "hello storage account tooreceive data",
    "mdsdHttpProxy" : ""
}
```

Elem | Érték
------- | -----
StorageAccount | hello neve, amelyben hello bővítmény adatot ír hello tárfiók. Kell azonos hello megadott nevet hello [beállítások védett](#protected-settings).
mdsdHttpProxy | (választható) Ugyanaz, mint hello [beállítások védett](#protected-settings). hello nyilvános érték felülbírálja hello titkos érték, ha beállítása. Helyezze el, amely tartalmazza a titkos kulcs, például a jelszó, hello proxybeállítások [beállítások védett](#protected-settings).

hello maradék összetevőit részletei a következő részekben hello.

### <a name="ladcfg"></a>ladCfg

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

A választható struktúra vezérlők hello gyűjtése metrikák és a naplókat a kézbesítési toohello Azure metrikák szolgáltatás és tooother adatokat fogadók esetében. Meg kell adnia vagy `performanceCounters` vagy `syslogEvents` vagy mindkettőt. Meg kell adnia a hello `metrics` struktúra.

Elem | Érték
------- | -----
eventVolume | (választható) Vezérlők hello hello tárolási tábla belül létrehozott partíciók száma. Egyikének kell lennie `"Large"`, `"Medium"`, vagy `"Small"`. Ha nincs megadva, hello alapértelmezett értéke `"Medium"`.
sampleRateInSeconds | (választható) hello alapértelmezett időközétől nyers (unaggregated) mérőszámok gyűjteménye. legkisebb támogatott hello mintavételi gyakoriság: 15 másodperc. Ha nincs megadva, hello alapértelmezett értéke `15`.

#### <a name="metrics"></a>metrics

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

Elem | Érték
------- | -----
resourceId | hello Azure Resource Manager erőforrás-azonosító hello virtuális gép vagy virtuálisgép-méretezési hello beállítása toowhich hello VM tartozik. Ezzel a beállítással lehet is adni, ha bármely JsonBlob fogadó hello konfigurációban szerepel.
scheduledTransferPeriod | hello gyakoriság, amellyel összesített adatok gyűjtése le toobe számított, és át tooAzure metrikák van 8601 időt időközönkénti kifejezve. hello legkisebb átviteli időtartam 60 másodperc, ez azt jelenti, hogy PT1M. Meg kell adnia legalább egy scheduledTransferPeriod.

Minták hello performanceCounters szakaszában megadott metrikák gyűjtött 15 másodpercenként hello vagy hello mintavételi gyakoriság hello számláló explicit módon definiálva. Ha több scheduledTransferPeriod gyakoriságot (mint pl. a hello) jelenik meg, minden Összesítés kiszámítása egymástól függetlenül.

#### <a name="performancecounters"></a>performanceCounters

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

Ez a szakasz választható metrikák hello gyűjteményét határozza meg. Nyers minták összesítik az egyes [scheduledTransferPeriod](#metrics) tooproduce ezeket az értékeket:

* témakörök
* minimális
* Maximális
* utolsó összegyűjtött érték
* nyers minták száma használt toocompute hello összesítés

Elem | Érték
------- | -----
fogadók esetében | (választható) A neveket vesszővel tagolt listája LAD küld összesített metrika eredmények toowhich fogadók esetében. Az összes összesített adatok gyűjtése le felsorolva közzétett tooeach fogadó. Lásd: [sinksConfig](#sinksconfig). Példa: `"EHsink1, myjsonsink"`.
type | Hello tényleges szolgáltató hello mérőszám azonosítja.
Osztály | "Számláló", és azonosítja az adott metrika hello hello szolgáltató névtéren belül.
A számláló | "Class" együtt azonosítja az adott metrika hello hello szolgáltató névtéren belül.
counterSpecifier | Azonosítja az adott metrika hello hello Azure metrikák névtéren belül.
Az állapot | (választható) Választja ki egy adott példányához hello objektum toowhich hello metrika vonatkozik, vagy választják ki hello összesítési, hogy az objektum összes példánya között. További információkért lásd: hello [ `builtin` metrikai meghatározásainak](#metrics-supported-by-builtin).
sampleRate | AZ, hogy beállítja hello gyakoriság, amellyel ez a mérőszám a nyers minták gyűjtik 8601 időközönként. Ha nincs megadva, hello adatgyűjtési időköz értéke hello értékének [sampleRateInSeconds](#ladcfg). hello legrövidebb támogatott mintavételi gyakoriság: 15 másodperc (PT15S).
egység | Ezek a karakterláncok egyike lehet: "Count", "Memória", "S", "Százaléka", "CountPerSecond", "BytesPerSecond", "Ezredmásodperces". Hello egység hello mérőszám határozza meg. Hello gyűjtött adatok fogyasztók várt hello gyűjtött adatok értékek toomatch a egység. LAD figyelmen kívül hagyja ezt a mezőt.
displayName | hello (nyelven hello hello társított területi beállítást által megadott) címke toobe csatolt Azure metrikák toothis adatokat. LAD figyelmen kívül hagyja ezt a mezőt.

hello counterSpecifier tetszőleges azonosító, amely. A mérőszámok, például az Azure portál diagramkészítési, és a szolgáltatás, riasztás hello fogyasztók hello "kulcsot" azonosító a metrika vagy metrika példányának counterSpecifier használja. A `builtin` metrika, ajánlott counterSpecifier értékek kezdődő `/builtin/`. Gyűjti a metrika egy adott példányához, azt javasoljuk csatlakoztatása hello toohello counterSpecifier értékének hello azonosítója. Néhány példa:

* `/builtin/Processor/PercentIdleTime`-Az összes mag között átlagosan üresjárati idő
* `/builtin/Disk/FreeSpace(/mnt)`– Hello /mnt FileSystem szabad terület
* `/builtin/Disk/FreeSpace`– Az összes csatlakoztatott fájlrendszerek között átlagosan szabad terület

LAD sem hello Azure-portálon vár hello counterSpecifier érték toomatch bármely mintát. Hogyan hozható létre counterSpecifier értékek a kell.

Ha a `performanceCounters`, LAD tooa adattábla mindig ír az Azure storage. Akkor is rendelkezik hello azonos tooJSON blobokat és/vagy az Event Hubs írt adatokat, de tárolni tooa adattábla nem tiltható le. Hello konfigurált diagnosztikai bővítmény toouse összes példánya ugyanazt a tárfiókot, nevét és a végpont hozzáadása a metrikák és a naplók toohello hello ugyanabban a táblában. Ha túl sok virtuális gép írás képes szabályozni a tábla partícióra, Azure toohello ír toothat partíció. hello eventVolume okok bejegyzések toobe beállítása 1 (kicsi), 10 (közepes) vagy 100 (nagy) különböző partíciók elosztva. Általában, "Közepes" is elegendő tooensure forgalom nem folyamatban van. hello Azure metrikák szolgáltatása hello Azure-portálon a tábla tooproduce diagramjait vagy tootrigger riasztások hello adatait használja. Ezek a karakterláncok hello összefűzése kell hello nevét:

* `WADMetrics`
* hello "scheduledTransferPeriod" hello összesítve hello táblában tárolt értékek
* `P10DV2S`
* Hello űrlap "ÉÉÉÉHHNN", amely megváltoztatja minden 10 nap, dátum

Példák `WADMetricsPT1HP10DV2S20170410` és `WADMetricsPT1MP10DV2S20170609`.

#### <a name="syslogevents"></a>syslogEvents

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

Ez a szakasz választható határozza meg a syslog naplóeseményeket hello gyűjteménye. Hello szakasz elhagyása esetén syslog-események nem minden rögzíti.

hello syslogEventConfiguration gyűjteményben szerepel egy bejegyzés minden egyes csomópontjára syslog-szolgáltatást. Ha minSeverity "NONE" az egy adott létesítményt a személyes, vagy az, hogy a létesítmény nem jelenik meg hello elem minden, az, hogy a létesítmény események nem lesznek rögzítve.

Elem | Érték
------- | -----
fogadók esetében | A neveket vesszővel tagolt listája toowhich egyedi aktiválási naplót esemény közzé lesz téve fogadók esetében. Hello korlátozások syslogEventConfiguration a megfelelő összes naplózási eseményt felsorolt közzétett tooeach fogadó. Példa: "EHforsyslog"
facilityName | A syslog létesítmény nevét (például a "napló\_felhasználói" vagy "napló\_LOCAL0"). Című rész hello "létesítmény" hello [syslog man lap](http://man7.org/linux/man-pages/man3/syslog.3.html) hello teljes listáját.
minSeverity | A syslog súlyossági szint (például a "napló\_hiba" vagy "napló\_INFO"). Című rész hello "szint" hello [syslog man lap](http://man7.org/linux/man-pages/man3/syslog.3.html) hello teljes listáját. hello kiterjesztés küldött események toohello intézmény rögzíti, vagy a fenti hello megadott szint.

Ha a `syslogEvents`, LAD tooa adattábla mindig ír az Azure storage. Akkor is rendelkezik hello azonos tooJSON blobokat és/vagy az Event Hubs írt adatokat, de tárolni tooa adattábla nem tiltható le. Ez a táblázat viselkedése particionálás hello van hello megegyeznek a `performanceCounters`. Ezek a karakterláncok hello összefűzése kell hello nevét:

* `LinuxSyslog`
* Hello űrlap "ÉÉÉÉHHNN", amely megváltoztatja minden 10 nap, dátum

Példák `LinuxSyslog20170410` és `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Ez a szakasz választható szabályozza tetszőleges végrehajtásának [OMI](https://github.com/Microsoft/omi) lekérdezések.

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

Elem | Érték
------- | -----
Namespace | (választható) hello OMI névtér melyik hello belül lekérdezés kell végrehajtani. Ha nincs megadva, hello alapértelmezett értéke "legfelső szintű/scx", hello által megvalósított [a System Center platformfüggetlen szolgáltatók](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
lekérdezés | hello OMI lekérdezés toobe végre.
Tábla | (választható) hello az Azure storage tábla, a kijelölt tárfiókkal hello (lásd: [beállítások védett](#protected-settings)).
frequency | hello lekérdezés végrehajtása között eltelt másodpercek számát (nem kötelező) hello. Alapértelmezett értéke 300 (5 percig); minimális érték 15 másodpercre.
fogadók esetében | (választható) További mosdók toowhich nyers minta metrika eredmények neveket vesszővel tagolt listája közzé kell tenni. Nincs összesítési e nyers minták számított hello bővítmény vagy Azure metrikákat.

"Table" vagy "fogadók esetében", vagy mindkettőt, meg kell adni.

### <a name="filelogs"></a>fileLogs

Vezérlők hello rögzítése a naplófájlok. LAD új szöveges sort rögzíti, az oktatóprogram toohello fájl, és írja őket az tootable sorok és/vagy a megadott mosdók (JsonBlob vagy EventHub).

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

Elem | Érték
------- | -----
Fájl | hello teljes elérési útja hello napló fájl toobe figyelése, és rögzített. hello pathname nevet egyetlen fájl; nem egy könyvtár nevet és nem tartalmazhat helyettesítő karaktereket.
Tábla | (választható) hello az Azure storage tábla kijelölt hello Storage (ahogy a védett hello configuration), az új sorok hello "kisebb" hello fájl íródtak a fiókot.
fogadók esetében | (választható) Küldött neve további mosdók toowhich napló vesszővel tagolt listája.

"Table" vagy "fogadók esetében", vagy mindkettőt, meg kell adni.

## <a name="metrics-supported-by-hello-builtin-provider"></a>Hello beépített szolgáltató támogatja a mérőszámok

hello beépített metrika szolgáltató metrikákat a legérdekesebb tooa széles körét felhasználók forrásaként szolgál. A metrikák öt széleskörű osztályok sorolhatók:

* Processzor
* Memory (Memória)
* Network (Hálózat)
* Fájlrendszer
* Lemez

### <a name="builtin-metrics-for-hello-processor-class"></a>a beépített metrikáját hello processzor osztály

hello processzor osztály a mérőszámok tájékoztatást ad azokról a virtuális gép hello processzor kihasználtsága. Százalékos összesítésekor hello eredménye hello átlagos összes processzorok között. A két fő virtuális gép egy alapvető 100 %-os elfoglalt volt, és más hello 100 %-os üresjárati, hello jelez-e PercentIdleTime pedig 50. Ha minden core 50 % foglalt volt hello azonos az az időszak hello jelentett eredmény is pedig 50. Egy négy alapvető virtuális gép, egy alapvető 100 % foglalt és hello üresjárati, mások hello jelentett PercentIdleTime 75 lenne.

A számláló | Jelentése
------- | -------
PercentIdleTime | Hogy a processzorok volt végrehajtása hello kernel üresjárati hurok hello összesítési időszakban idő százalékos aránya
PercentProcessorTime | Nem üresjárati szálat idő százalékos aránya
PercentIOWaitTime | Várakozás az I/O műveletek toocomplete idő százalékos aránya
PercentInterruptTime | Hardver vagy szoftver megszakítások, DPC-k (késleltetett eljáráshívások) végrehajtása idő százalékos aránya
PercentUserTime | Hello összesítési időszak alatt nem üresjárati idő hello fordított idő százalékos aránya a felhasználó több normál prioritással
PercentNiceTime | Nem üresjárati idő hello süllyesztett (jó) prioritással töltött százalékos aránya
PercentPrivilegedTime | Nem üresjárati idő hello védett (kernel) módban töltött százalékos aránya

hello első négy számlálók kell összeg too100 %. hello utolsó három teljesítményszámlálók is sum too100 %; Ezek tovább PercentProcessorTime PercentIOWaitTime és PercentInterruptTime hello összege.

egyetlen mérőszám az összes processzor gyűjtődnek tooobtain beállítása `"condition": "IsAggregate=TRUE"`. megadott processzorsebességgel rendelkező metrika tooobtain hello második logikai processzorral egy négy alapvető VM, mint például beállítása `"condition": "Name=\\"1\\""`. Logikai processzor számok hello tartományban vannak `[0..n-1]`.

### <a name="builtin-metrics-for-hello-memory-class"></a>a beépített metrikáját hello memória osztály

hello memória osztály a mérőszámok memóriafelhasználás a lapozást, és áttelepíteni a forráskörnyezetból információkat biztosít.

A számláló | Jelentése
------- | -------
AvailableMemory | Rendelkezésre álló fizikai memória MIB
PercentAvailableMemory | A teljes memória százalékos rendelkezésre álló fizikai memória
UsedMemory | Használatban lévő fizikai memória (MiB)
PercentUsedMemory | A teljes memória százalékos a használatban lévő fizikai memória
PagesPerSec | Teljes lapozófájl (olvasás/írás)
PagesReadPerSec | Lapok olvasni háttértár (lapozófájl programfájlt, leképezett fájlt, stb.)
PagesWrittenPerSec | Toobacking írt lapok tárolja (lapozófájl, leképezett fájlt, stb.)
AvailableSwap | Nem használt lapozóterület (MiB)
PercentAvailableSwap | Nem használt lapozóterület teljes lapozófájl-kapacitás százalékában
UsedSwap | Használatban lévő lapozóterület (MiB)
PercentUsedSwap | Használatban lévő lapozóterület teljes lapozófájl-kapacitás százalékában

Ez az osztály a mérőszámok csak egyetlen példány van. hello "feltétel" attribútum nem hasznos beállításokkal rendelkezik, és megadni.

### <a name="builtin-metrics-for-hello-network-class"></a>a beépített metrikáját hello hálózati osztály

hello hálózati osztály a mérőszámok tevékenységről szolgáltat információkat hálózati az egyes hálózati adaptereken indítása óta. LAD nem biztosít sávszélesség metrikákat, amelyek a gazdagép-metrikák kérhető.

A számláló | Jelentése
------- | -------
BytesTransmitted | Rendszerindítás óta küldött bájtok száma összesen
BytesReceived | Rendszerindítás óta fogadott összes bájt
BytesTotal | Küldött vagy indítása óta fogadott bájtok teljes száma
PacketsTransmitted | Rendszerindítás óta küldött csomagok száma összesen
PacketsReceived | Rendszerindítás óta fogadott csomagok száma összesen
TotalRxErrors | A fogadási hibák száma indítása óta
TotalTxErrors | -Küldési hibák száma indítása óta
TotalCollisions | Rendszerindítás óta hello hálózati portok által jelentett ütközések száma

 Bár ez az osztály van instanced, LAD nem támogatja az összes hálózati eszköz gyűjtődnek rögzítésével hálózati metrikákat. egy adott illesztő eth0, például metrikáját tooobtain hello beállítása `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-hello-filesystem-class"></a>a beépített metrikáját hello Filesystem osztály

hello Filesystem osztály a mérőszámok filesystem használati információkat biztosít. Abszolút és százalékos értéket jelentett, a megjelenő tooan a szokványos felhasználói (nem a legfelső szintű) lesznek.

A számláló | Jelentése
------- | -------
FreeSpace | Szabad lemezterület (bájt)
UsedSpace | A felhasznált lemezterület (bájt)
PercentFreeSpace | Szabad terület százalékos aránya
PercentUsedSpace | A felhasznált terület százalékos aránya
PercentFreeInodes | Nem használt Inode-OK százaléka
PercentUsedInodes | Összesítve közötti összes fájlrendszerek lefoglalt (használatban) Inode-OK százaléka
BytesReadPerSecond | Másodpercenként olvasott bájtok száma
BytesWrittenPerSecond | Másodpercenként írt bájtok
BytesPerSecond | Bájt nem írható és olvasható / másodperc
ReadsPerSecond | Olvasási műveletek másodpercenkénti száma
WritesPerSecond | Írási műveletek másodpercenkénti száma
TransfersPerSecond | Olvasási vagy írási műveletek másodpercenkénti száma

Összesített értékeket fájlrendszerek keresztül érhető el úgy, hogy `"condition": "IsAggregate=True"`. Például egy adott csatlakoztatott fájlrendszer értékei "/ mnt", úgy, hogy szerezhetők `"condition": 'Name="/mnt"'`.

### <a name="builtin-metrics-for-hello-disk-class"></a>a beépített metrikáját hello lemez osztály

hello lemez osztály a mérőszámok eszköz lemezhasználati információkat biztosít. A statisztikai információk toohello teljes meghajtó alkalmazni. Ha egy eszközön több fájlrendszereket, hello számlálók az eszköznek vannak, gyakorlatilag az összes gyűjtődnek.

A számláló | Jelentése
------- | -------
ReadsPerSecond | Olvasási műveletek másodpercenkénti száma
WritesPerSecond | Írási műveletek másodpercenkénti száma
TransfersPerSecond | Teljes műveletek másodpercenkénti száma
AverageReadTime | Olvasási művelet átlagos másodpercben
AverageWriteTime | Írási művelet átlagos másodpercben
AverageTransferTime | Művelet átlagos másodpercben
AverageDiskQueueLength | Várólistára helyezett lemezen műveletek átlagos száma
ReadBytesPerSecond | A másodpercenként beolvasott bájtok száma
WriteBytesPerSecond | Másodpercenként írt bájtok száma
BytesPerSecond | Olvassa el és másodpercenként írt bájtok száma

Minden lemezeken összesített értékeket szerezhető be úgy, hogy `"condition": "IsAggregate=True"`. beállítás tooget adatokat egy adott eszköz számára (például/dev/sdf1), `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Telepítése és konfigurálása LAD 3.0 parancssori felület használatával

Ha a védett beállítások hello fájlban PrivateConfig.json és a nyilvános konfigurációs adatokat PublicConfig.json, futtassa ezt a parancsot:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

hello parancs feltételezi, hogy a hello Azure Resource Manager módot (arm) a hello Azure parancssori felület. tooconfigure LAD a klasszikus üzembe helyezési modell (ASM) virtuális gépeket, váltson túl "asm" mód (`azure config mode asm`), és hagyja ki a hello erőforráscsoport-név hello parancsban. További információkért lásd: hello [platformfüggetlen parancssori felület dokumentáció](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>Egy példa LAD 3.0 konfiguráció

A definíciók, itt meg egy minta LAD 3.0 bővítménykonfiguráció rövid megelőző hello alapján. tooapply ez jelen példában tooyour kell használni a saját tárfióknév, SAS-jogkivonat fiókot, és EventHubs SAS-jogkivonatok.

### <a name="privateconfigjson"></a>PrivateConfig.json

Ezek a személyes beállítások konfigurálása:

* a storage-fiók
* a megfelelő fiók SAS-jogkivonat
* több fogadók esetében (JsonBlob vagy az SAS-tokenje EventHubs)

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a>PublicConfig.json

A nyilvános beállítások okozhat a LAD:

* Töltse fel a százalékos processzoridő és használt-terület toohello `WADMetrics*` tábla
* Töltse fel a syslog létesítmény "user" és a súlyosság "Infó" toohello üzeneteit `LinuxSyslog*` tábla
* Töltse fel a nyers OMI lekérdezési eredmények (PercentProcessorTime és PercentIdleTime) toohello nevű `LinuxCPU` tábla
* Töltse fel a fájl sorainak hozzáfűzött `/var/log/myladtestlog` toohello `MyLadTestLog` tábla

Minden esetben adatokat is feltöltött:

* Az Azure Blob storage (a tároló neve: hello JsonBlob fogadó meghatározottak szerint)
* EventHubs végpont (ahogy a hello EventHubs fogadó)

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

Hello `resourceId` hello konfigurációs meg kell egyeznie, hogy hello VM vagy hello virtuálisgép-méretezési állítva.

* Az Azure platform metrikák diagramkészítési és riasztás tudja hello dolgozunk a virtuális gép hello erőforrás-azonosítója. A virtuális gép hello resourceId hello keresési kulcs használatával toofind hello adatok azt vár.
* Az Azure automatikus skálázás használatakor hello resourceId hello automatikus skálázás konfigurációban meg kell egyeznie hello resourceId LAD használják.
* hello resourceId beépített LAD által írt JsonBlobs hello nevét.

## <a name="view-your-data"></a>Az adatok megtekintése

Hello Azure portál tooview teljesítményadatok, vagy állítson be riasztásokat:

![Kép](./media/diagnostic-extension/graph_metrics.png)

Hello `performanceCounters` adatok mindig egy Azure Storage táblázatban vannak tárolva. Az Azure Storage API-k sok nyelvekhez és platformokhoz érhetők el.

TooJsonBlob mosdók küldött adatok blobot, amely a hello nevű hello tárfiók tárolja [beállítások védett](#protected-settings). Hello Blobadatok bármely Azure Blob Storage API-k használatával is felhasználhatnak.

Ezenkívül használhatja a felhasználói felület eszközök tooaccess hello adatokat az Azure Storage:

* A Visual Studio Server Explorer.
* [A Microsoft Azure Tártallózó](https://azurestorageexplorer.codeplex.com/ "Azure Tártallózó").

A Microsoft Azure Tártallózó munkamenet pillanatképe látható hello az Azure Storage-táblákat és a tárolók jön létre a teszteléshez használt virtuális gép egy megfelelően konfigurált LAD 3.0 bővítménnyel. hello kép nem egyezik pontosan hello [LAD 3.0 mintakonfiguráció](#an-example-lad-30-configuration).

![Kép](./media/diagnostic-extension/stg_explorer.png)

Tekintse meg a megfelelő hello [EventHubs dokumentáció](../../event-hubs/event-hubs-what-is-event-hubs.md) toolearn hogyan tooconsume üzenetek közzétett tooan EventHubs végpont.

## <a name="next-steps"></a>Következő lépések

* A metrika értesítések [Azure figyelő](../../monitoring-and-diagnostics/insights-alerts-portal.md) hello metrikáihoz összegyűjtése.
* Hozzon létre [diagramok figyelési](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) a metrikáihoz.
* Ismerje meg, hogyan túl[hozzon létre egy virtuálisgép-méretezési csoport](/azure/virtual-machines/linux/tutorial-create-vmss) használ a metrikák toocontrol automatikus skálázást.
