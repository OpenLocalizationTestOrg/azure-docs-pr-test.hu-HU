---
title: "aaaAzure App szolgáltatás helyi gyorsítótárából áttekintése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooenable, méretezze át és lekérdezés hello hello Azure App Service helyi gyorsítótár szolgáltatás állapota"
services: app-service
documentationcenter: app-service
author: SyntaxC4
manager: yochayk
editor: 
tags: optional
keywords: 
ms.assetid: e34d405e-c5d4-46ad-9b26-2a1eda86ce80
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/04/2016
ms.author: cfowler
ms.openlocfilehash: 220331ac7e15352a434d63266701071024d868c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-local-cache-overview"></a>Az Azure App Service helyi gyorsítótár áttekintése
Az Azure web app tartalom Azure Storage tárolja, és tartósan tartalom megosztásnak illesztett be van. Ez a kialakítás tervezett toowork alkalmazások számos különböző, és a következő attribútumok hello:  

* a virtuális gép (VM) több példánya hello webalkalmazás megosztott hello tartalom.
* hello tartalom és a futó webes alkalmazások által módosítható.
* Naplófájlok és diagnosztikai adatok fájlok alatt érhetők el hello azonos megosztott tartalmat tartalmazó mappa.
* Új tartalom közzététele közvetlen frissítések hello tartalmat tartalmazó mappa. Is azonnal nézet hello azonos hello SCM webhelyen keresztül tartalmat, és hello (általában egyes technológiák, például az ASP.NET újraindításhoz egy webes alkalmazás egyes fájl módosításait tooget hello legújabb tartalom) futó webalkalmazás.

Miközben sok webes alkalmazás használja legalább mindezeket a funkciókat, néhány webalkalmazások kell nagy teljesítményű, csak olvasható tartalomtárhoz konvertál, amely futtathatják a magas rendelkezésre állású. Ezeket az alkalmazásokat is kihasználhatja a le egy Virtuálisgép-példány az adott helyi gyorsítótár.

a tartalom webes szerepkör áttekintést nyújt a hello Azure App Service helyi gyorsítótár-szolgáltatás. Ezt a tartalmat egy írási-de-elvetési gyorsítótár, a tároló tartalmának a hely indítása aszinkron módon létrehozott. Amikor készen áll a hello gyorsítótár, hello hely a kapcsolt toorun hello gyorsítótárazott tartalom. A helyi gyorsítótár futó webalkalmazások rendelkezik a következő előnyöket hello:

* Azure tárhelyén található tartalom elérésekor előforduló immúnis toolatencies.
* Immúnis toohello a tervezett frissítések vagy nem tervezett állásidőt és bármely más üzemzavarokhoz vezethet az Azure Storage hello tartalommegosztás átadott kiszolgálókon előforduló.
* Kevesebb alkalmazás újraindul toostorage megosztás módosítások miatt rendelkeznek.

## <a name="how-local-cache-changes-hello-behavior-of-app-service"></a>Hogyan a helyi gyorsítótár módosítja a hello viselkedését az App Service
* hello a helyi gyorsítótár hello /site és /siteextensions mappák hello webalkalmazás másolatát. Hello helyi Virtuálisgép-példány webes alkalmazás következő indításakor jön létre. hello hello helyi gyorsítótár web app / mérete korlátozott too300 MB. alapértelmezett, de emelheti be too2 GB.
* hello a helyi gyorsítótár írhatónak és olvashatónak. Azonban bármely módosítások el lesznek vetve hello web app virtuális gépeket helyezi át, illetve lekérdezi újraindításakor. Ne használja a helyi gyorsítótár hello tartalomtároló a kritikus fontosságú adatok tárolására szolgáló alkalmazások.
* Webalkalmazások továbbra is toowrite naplófájlokat és diagnosztikai adatok, mint jelenleg. Naplófájlok és adatokat, azonban tárolja helyileg hello virtuális gép. Majd forrásmappa rendszeresen toohello megosztott tartalomtároló keresztül. hello másolási toohello megosztott tartalomtároló hányad elérhető--írási készít biztonsági elveszhet tooa hirtelen összeomlási Virtuálisgép-példány miatt.
* Hello naplófájlok és a helyi gyorsítótár használó webalkalmazások adatmappáinak hello gyökérmappa-szerkezetében lévő változás áll. Nincsenek most almappák hello tárolási naplófájlok és a mappában adatokat az alábbi elnevezési hello "egyedi azonosítója" + időbélyegző. Minden hello felel meg a tooa Virtuálisgép-példány ahol hello webalkalmazás fut, vagy futott.  
* Közzétételi módosítások toohello webalkalmazás hello közzétételi mechanizmusok segítségével toohello megosztott tartalomtároló tesznek közzé. Ez az elvárt működés, mert azt szeretnénk, ha hello közzétett tartalom toobe tartós. toorefresh hello helyi gyorsítótárba hello webalkalmazás toobe újraindítása szükséges. Nem ez úgy tűnik, például egy túl sok lépést? toomake hello életciklus zökkenőmentes, információ hello a cikk későbbi részében.
* D:\Home toohello helyi gyorsítótár mutat. D:\Local továbbra is toohello ideiglenes virtuális gép bizonyos tárolási mutat.
* hello alapértelmezett tartalom nézet hello SCM hely továbbra is, amely a hello megosztott tartalomtároló toobe.

## <a name="enable-local-cache-in-app-service"></a>Az App Service szolgáltatásban a helyi gyorsítótár engedélyezése
Helyi gyorsítótár konfigurálása az alkalmazás foglalt beállítások használatával. Ezek a beállítások alkalmazás hello a következő módszerek használatával konfigurálhatja:

* [Azure Portal](#Configure-Local-Cache-Portal)
* [Azure Resource Manager](#Configure-Local-Cache-ARM)

### <a name="configure-local-cache-by-using-hello-azure-portal"></a>Helyi gyorsítótár konfigurálása hello Azure-portál használatával
<a name="Configure-Local-Cache-Portal"></a>

Helyi gyorsítótár /--webalkalmazás alapon, az Alkalmazásbeállítás használatával engedélyezheti:`WEBSITE_LOCAL_CACHE_OPTION` = `Always`  

![Az Azure portál Alkalmazásbeállítások: helyi gyorsítótárban](media/app-service-local-cache/app-service-local-cache-configure-portal.png)

### <a name="configure-local-cache-by-using-azure-resource-manager"></a>Helyi gyorsítótár konfigurálása az Azure Resource Manager használatával
<a name="Configure-Local-Cache-ARM"></a>

```

...

{
    "apiVersion": "2015-08-01",
    "type": "config",
    "name": "appsettings",
    "dependsOn": [
        "[resourceId('Microsoft.Web/sites/', variables('siteName'))]"
    ],

"properties": {
        "WEBSITE_LOCAL_CACHE_OPTION": "Always",
        "WEBSITE_LOCAL_CACHE_SIZEINMB": "300"
    }
}

...
```

## <a name="change-hello-size-setting-in-local-cache"></a>A helyi gyorsítótárban hello mérete beállításának módosítása
Alapértelmezés szerint hello helyi gyorsítótár mérete **300 MB**. Ez magában foglalja a hello /site, és másolja át /siteextensions mappák hello tartalmat tároló, valamint a helyileg létrehozott naplók és az adatok mappák. tooincrease ez korlátozásához hello app beállítással `WEBSITE_LOCAL_CACHE_SIZEINMB`. Növelheti a hello mérete legfeljebb túl**2 GB** (2000 MB) egy webalkalmazás.

## <a name="best-practices-for-using-app-service-local-cache"></a>Alkalmazás szolgáltatás helyi gyorsítótárából használatának ajánlott eljárásai
Javasoljuk, hogy a helyi gyorsítótár hello együtt használjon [előkészítési környezetek](../app-service-web/web-sites-staged-publishing.md) szolgáltatás.

* Adja hozzá a hello *állandóságát* Alkalmazásbeállítás `WEBSITE_LOCAL_CACHE_OPTION` hello értékű `Always` tooyour **éles** tárolóhely. Ha használ `WEBSITE_LOCAL_CACHE_SIZEINMB`, is hozzáadhatja a kapcsolódó beállítás tooyour éles tárolóhelyre.
* Hozzon létre egy **átmeneti** tárolóhely, és tegye közzé a tooyour átmeneti tárolóhely. Általában nem állít hello átmeneti tárolóhely toouse helyi gyorsítótár tooenable egy zökkenőmentes build telepítése teszt életciklusa átmeneti, ha a helyi gyorsítótár hello előnyei lekérése hello éles tárolóhelyre.
* Tesztelje a átmeneti aljzat a helyen.  
* Ha készen áll, ki egy [a felcserélési művelet](../app-service-web/web-sites-staged-publishing.md#Swap) között az átmeneti és üzemi tárolóhely.  
* A kapcsolódó beállítások közé tartoznak a neve és a kapcsolódó tooa tárolóhely. Ezért amikor hello átmeneti tárolóhely lekérdezi felcserélve éles környezetben, azt öröklik hello helyi gyorsítótár beállításainak. hello újonnan felcserélni az éles tárhely fog néhány perc múlva hello helyi gyorsítótár futtatni, majd fog kell tárolóhelyspecifikus tárolóhely melegítési részeként swap után. Ezért hello tárolóhelycsere befejeződése után az éles tárolóhelyre fog futni hello helyi gyorsítótár ellen.

## <a name="frequently-asked-questions-faq"></a>Gyakori kérdések (GYIK)
### <a name="how-can-i-tell-if-local-cache-applies-toomy-web-app"></a>Hogyan állapítható meg, ha a helyi gyorsítótár toomy webalkalmazás vonatkozik?
Ha a webalkalmazás kell egy nagy teljesítményű, megbízható tartalomtárhoz konvertál, nem hello tartalomtároló toowrite kritikus fontosságú adatok futtatás közben használ, és teljes mérete 2 GB-nál kevesebb, majd hello válasz "yes"! tooget hello teljes mérete a /site és /siteextensions mappa, hello hely bővítmény "Azure Web Apps lemezhasználati" is használhatja.  

### <a name="how-can-i-tell-if-my-site-has-switched-toousing-local-cache"></a>Hogyan állapítható meg, ha a hely váltott toousing helyi gyorsítótár?
Az átmeneti környezetének használata hello helyi gyorsítótár funkció, hello csereművelet végrehajtásához helyi gyorsítótár tárolóhelyspecifikus van. toocheck, ha a hely helyi gyorsítótár elleni fut, akkor ellenőrizheti a hello munkavégző folyamat környezeti változót `WEBSITE_LOCALCACHE_READY`. Hello utasítások használatát hello [munkavégző folyamat környezeti változó](https://github.com/projectkudu/kudu/wiki/Process-Threads-list-and-minidump-gcdump-diagsession#process-environment-variable) lap tooaccess hello munkavégző folyamat több példányt környezeti változóba.  

### <a name="i-just-published-new-changes-but-my-web-app-does-not-seem-toohave-them-why"></a>Új módosítások csak közzétett, de a webalkalmazás nem tűnik toohave őket. Hogy miért?
Ha a webes alkalmazás a helyi gyorsítótár használ, akkor szüksége toorestart a hely tooget hello legutóbbi változtatásokat. Nem kívánok toopublish módosítások tooa munkakörnyezeti helyet? Lásd: hello tárolóhely-beállítások hello előző szakaszban ajánlott eljárásokat.

### <a name="where-are-my-logs"></a>Hol találhatók a naplókat?
Helyi gyorsítótár a naplókat és az adatok mappák keressen eltérő. Azonban hello felépítése az almappák marad hello azonos, azzal a különbséggel, hogy hello almappák hello egy almappát a rendszer nestled formázása "egyedi virtuális gép azonosítója" + időbélyegző.

### <a name="i-have-local-cache-enabled-but-my-web-app-still-gets-restarted-why-is-that-i-thought-local-cache-helped-with-frequent-app-restarts"></a>Helyi gyorsítótár engedélyezve van, de a webes alkalmazás továbbra is lekérdezi újraindul. Az oka? I-re helyi gyorsítótár segített a gyakori alkalmazás újraindul.
Helyi gyorsítótár tárolással kapcsolatos webes alkalmazás újraindul megelőzése érdekében. Azonban a webalkalmazás sikerült továbbra is változni újraindítja a virtuális gép hello tervezett infrastruktúra frissítéskor. hello általános alkalmazás újraindul, hogy a hogy a helyi gyorsítótár engedélyezve legyen kevesebb.

### <a name="does-local-cache-exclude-any-directories-from-being-copied-toohello-faster-local-drive"></a>Nem helyi gyorsítótár kizárja a könyvtárak nem másolta toohello gyorsabban helyi meghajtót?
Hello lépés, és másolja át a hello tárolási tartalom részeként, a nem kerülnek bele a tárház nevű mappát. Ezzel a megoldással esetén, ahol a webhely tartalmát a verziókövetési tárházat, amely nem lehet szükség a nap tooday művelet hello webalkalmazás is tartalmazhat. 
