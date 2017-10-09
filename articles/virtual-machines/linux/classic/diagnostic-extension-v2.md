---
title: "Linux virtuális Gépet egy Virtuálisgép-bővítménnyel aaaMonitoring |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Linux diagnosztikai bővítmény toomonitor hello teljesítmény- és a Linux virtuális gépek az Azure diagnosztikai adatokat."
services: virtual-machines-linux
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: f54a11c5-5a0e-40ff-af6c-e60bd464058b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: Ning
ms.openlocfilehash: cf7bfebca8c0367941f7a975417f60fe2e2dab25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-linux-diagnostic-extension-toomonitor-hello-performance-and-diagnostic-data-of-a-linux-vm"></a>Hello Linux diagnosztikai bővítmény toomonitor hello teljesítmény- és a Linux virtuális gépek diagnosztikai adatok használata

Ez a dokumentum ismerteti a hello Linux diagnosztikai bővítmény 2.3 verzióját.

> [!IMPORTANT]
> Jelen verziója elavult, és lehet, hogy közzé nem tett 2018. június 30 követően. Helyette 3.0-s verziója. További információkért lásd: hello [hello Linux diagnosztikai bővítmény 3.0-s verziója című dokumentációban](../diagnostic-extension.md).

## <a name="introduction"></a>Bevezetés

(**Megjegyzés**: hello Linux diagnosztikai bővítmény nem nyílt forrása [GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) , ahol az első közzététel a hello hello bővítmény legújabb információkat. Érdemes lehet toocheck hello [GitHub-oldalon](https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic) első.)

Linux diagnosztikai bővítmény hello segítségével egy felhasználó figyelő hello a Microsoft Azure futó Linux virtuális gépek. Hello a következő képességekkel rendelkezik:

* Gyűjti, és feltölti a rendszer teljesítményadatok hello hello Linux virtuális gép toohello felhasználói tárolási táblából, ideértve a diagnosztikai és syslog adatokat.
* Lehetővé teszi, hogy a felhasználók toocustomize hello adatok metrikák összegyűjtésének és feltöltve.
* Lehetővé teszi, hogy a felhasználók tooupload megadott napló fájlok tooa kijelölt tároló tábla.

Hello aktuális verziójában 2.3 hello adatokat tartalmazza:

* Az összes Linux Rsyslog naplókat, beleértve a rendszer, a biztonsági és az alkalmazás naplóiban.
* Összes rendszer adat, amely meg van adva [hello System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).
* Felhasználó által meghatározott naplófájlokat.

A bővítmény hello klasszikus és Resource Manager üzembe helyezési modellel működik.

### <a name="current-version-of-hello-extension-and-deprecation-of-old-versions"></a>Hello bővítményt, és a régi változatainak elavulása jelenlegi verziója

hello hello bővítmény legújabb verziója van **2.3**, és **a régi verziókat (2.0, 2.1-es és 2.2) a rendszer elavult, majd közzé nem tett az év (2017) végéig**. Ha telepítette a hello Linux diagnosztikai bővítmény automatikus alverzió frissítés le van tiltva, erősen ajánlott hello-kiterjesztés eltávolítása, és engedélyezve van az automatikus alverzió frissítést újra kell telepítenie. A klasszikus (ASM) virtuális gépeken érhet el ez Azure XPLAT parancssori felületen vagy a Powershellen keresztül hello bővítmény telepítésekor "2.*" megadásával hello verzióként. ARM gépeken lehet ezt elérni-ot ""autoUpgradeMinorVersion": true" hello VM központi telepítési sablon. Emellett bármely új hello-bővítmény telepítése hello automatikus alverzió beállítás engedélyezve van a frissítés kell rendelkeznie.

## <a name="enable-hello-extension"></a>Hello bővítmény engedélyezése

A bővítmény hello használatával engedélyezheti [Azure-portálon](https://portal.azure.com/#), az Azure PowerShell vagy Azure CLI parancsfájlok.

tooview és hello rendszer konfigurálása, valamint teljesítményadatokat közvetlenül a hello Azure-portálon, hajtsa végre a [ezeket a lépéseket a hello Azure blog](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/).

Ez a cikk foglalkozik, hogyan tooenable és hello-kiterjesztés konfigurálása az Azure parancssori felület parancsait használva. Ez lehetővé teszi tooread és nézet hello közvetlenül hello tárolási tábla adatait.

Vegye figyelembe, hogy hello konfigurációs itt ismertetett módszerek hello Azure-portálon az nem fog működni. tooview és hello rendszer és a teljesítmény adatok közvetlenül az Azure-portálon hello konfigurálása, hello bővítmény hello portálon keresztül engedélyezve kell lennie.

## <a name="prerequisites"></a>Előfeltételek

* **Az Azure Linux ügynök 2.0.6 verzió vagy újabb**.

  Vegye figyelembe, hogy a legtöbb Azure virtuális gép Linux gyűjtemény lemezképei tartalmazzák 2.0.6 verzió vagy újabb. Futtathat **WAAgent-verzió** tooconfirm hello VM melyik verziója van telepítve. Ha a virtuális gép hello 2.0.6 korábbi verziót futtat, kövesse [ezeket az utasításokat a Githubon](https://github.com/Azure/WALinuxAgent "utasításokat") tooupdate azt.
* **Azure parancssori felület (CLI)**. Hajtsa végre a [Ez az útmutató a parancssori felület telepítése](../../../cli-install-nodejs.md) tooset hello Azure CLI környezetet a számítógépen. Az Azure parancssori felület telepítése után is használhatja hello **azure** parancsot a parancssori felület (a Bash, a Terminálszolgáltatások vagy a parancssorból) tooaccess hello Azure parancssori felület parancsait. Példa:

  * Futtatás **azure virtuálisgép-bővítmény készlet – súgó** vonatkozó részletes információk.
  * Futtatás **azure bejelentkezési** a tooAzure toosign.
  * Futtatás **azure virtuális gép lista** toolist összes hello Azure rendelkező virtuális gépek.
* A tárolási fiók toostore hello adatait. Szüksége lesz egy korábban létrehozott tárfiók neve és egy hozzáférési kulcs tooupload hello tooyour-tároló.

## <a name="use-hello-azure-cli-command-tooenable-hello-linux-diagnostic-extension"></a>Hello Azure CLI parancs tooenable hello Linux diagnosztikai bővítmény használatára

### <a name="scenario-1-enable-hello-extension-with-hello-default-data-set"></a>1. forgatókönyv. Hello alapértelmezett adatkészlet hello bővítmény engedélyezése

2.3-as vagy újabb verziója, a gyűjtendő hello alapértelmezett adatokat tartalmazza:

* Minden Rsyslog információk (beleértve a rendszer, a biztonsági és az alkalmazásnaplók).  
* Egy sor alapján a rendszer adatai. Vegye figyelembe, hogy a teljes adatkészlet hello leírása a hello [System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders).
  Ha azt szeretné, hogy tooenable további adatokat, folytassa a hello lépésekkel forgatókönyvek 2 és 3.

1. lépés A következő hello PrivateConfig.json nevű fájl létrehozása tartalom:

    {
        "storageAccountName" : "hello storage account tooreceive data",
        "storageAccountKey" : "hello key of hello account"
    }

2. lépés Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions 2.* --magánfelhő-config-path PrivateConfig.json**.

### <a name="scenario-2-customize-hello-performance-monitor-metrics"></a>2. forgatókönyv. Hello figyelő teljesítménymutatók testreszabása

Ez a szakasz ismerteti, hogyan toocustomize hello teljesítményt, és diagnosztikai adatokat.

1. lépés 1. forgatókönyv ismertetett hello tartalmú PrivateConfig.json nevű fájl létrehozása. PublicConfig.json nevű fájlt is létrehozhat. Adja meg a kívánt toocollect hello adott adatok.

Az összes támogatott szolgáltatók és változók, hivatkozhat hello [System Center Cross Platform megoldások hely](https://scx.codeplex.com/wikipage?title=xplatproviders). Több lekérdezés van, és további lekérdezések toohello parancsfájl hozzáfűzésével több táblát is tárolhatja őket.

Alapértelmezés szerint mindig gyűjtött hello Rsyslog adatokat.

    {
          "perfCfg":
          [
              {
                  "query" : "SELECT PercentAvailableMemory, AvailableMemory, UsedMemory ,PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
                  "table" : "LinuxMemory"
              }
          ]
    }


2. lépés Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions "2.*"--magánfelhő-config-path PrivateConfig.json – nyilvános-config-path PublicConfig.json**.

### <a name="scenario-3-upload-your-own-log-files"></a>3. forgatókönyv. Töltse fel a saját naplófájlok

Ez a szakasz ismerteti, hogyan toocollect és feltöltése adott naplófájljai tooyour tárfiók. Toospecify kell mindkét hello tooyour napló fájl- és hello elérési útjának hello táblázatban, ahol toostore a naplót. Több naplófájl is létrehozhat több fájl vagy a tábla bejegyzések toohello parancsfájlt.

1. lépés 1. forgatókönyv ismertetett hello tartalmú PrivateConfig.json nevű fájl létrehozása. Ezután hozzon létre egy másik fájlt PublicConfig.json hello követően a tartalom:

```json
{
    "fileCfg" :
    [
        {
            "file" : "/var/log/mysql.err",
            "table" : "mysqlerr"
            }
    ]
}
```

2. lépés Futtassa az `azure vm extension set vm_name LinuxDiagnostic Microsoft.OSTCExtensions '2.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json` parancsot.

Vegye figyelembe, hogy ezt a beállítást az hello bővítmény verziók előzetes too2.3, az összes napló írása túl`/var/log/mysql.err` előfordulhat, hogy túl duplikált`/var/log/syslog` (vagy `/var/log/messages` hello Linux distro függően) is. Ha szeretné tooavoid a naplózás duplikált, kizárhatja naplózása `local6` létesítmény rsyslog konfigurációs naplózza. Hello Linux distro függ, de az Ubuntu 14.04 rendszeren hello fájl toomodify `/etc/rsyslog.d/50-default.conf` és lecserélheti hello sor `*.*;auth,authpriv.none -/var/log/syslog` túl`*.*;auth,authpriv,local6.none -/var/log/syslog`. Ez a probléma fennáll a legújabb gyorsjavítások kiadásának hello 2.3 (2.3.9007), így ha hello bővítmény verzió 2.3, a probléma nem következik. Ha továbbra is támogatja a virtuális gép újraindítása után is, lépjen velünk kapcsolatba, és segítsen hibaelhárítása, ezért hello gyorsjavítás letöltéséhez nem települ automatikusan.

### <a name="scenario-4-stop-hello-extension-from-collecting-any-logs"></a>4. forgatókönyv. Állítsa le a naplók gyűjtésére hello bővítmény

Ez a szakasz ismerteti, hogyan naplózza az toostop hello bővítmény gyűjtése. Vegye figyelembe, hogy a figyelési ügynök folyamat hello lesz továbbra is működik, és az újrakonfigurálás mellett is. Ha szeretné, hogy teljesen figyelési ügynök folyamat toostop hello, hello bővítmény letiltásával is megteheti. hello parancs toodisable hello kiterjesztés `azure vm extension set --disable <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions '2.*'`.

1. lépés 1. forgatókönyv ismertetett hello tartalmú PrivateConfig.json nevű fájl létrehozása. Egy másik PublicConfig.json nevű hello a következő fájl tartalmának létrehozása:

    {
        "perfCfg" : [],
        "enableSyslog" : "false"
    }


2. lépés Futtatás  **azure virtuálisgép-bővítmény beállítása vm_name LinuxDiagnostic Microsoft.OSTCExtensions "2.*"--magánfelhő-config-path PrivateConfig.json – nyilvános-config-path PublicConfig.json**.

## <a name="review-your-data"></a>Tekintse át az adatokat

hello teljesítmény- és diagnosztikai adatokat egy Azure Storage tábla vannak tárolva. Felülvizsgálati [hogyan toouse Azure Table Storage-ának Ruby](../../../cosmos-db/table-storage-how-to-use-ruby.md) toolearn tooaccess hello adatok hello tábla Azure CLI-parancsfájlok használatával.

Ezenkívül használhatja következő felhasználói felületi eszközöket tooaccess hello adatokat:

1. A Visual Studio Server Explorer. Nyissa meg a tárfiók tooyour. Miután hello VM körülbelül 5 percig, látni fogja a hello négy alapértelmezett táblák: "LinuxCpu", "LinuxDisk", "LinuxMemory" és "Linuxsyslog". Kattintson duplán a hello nevek tooview hello adatait.
1. [Az Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/ "Azure Tártallózó").

![Kép](./media/diagnostic-extension/no1.png)

Ha engedélyezte a fileCfg vagy perfCfg (leírt forgatókönyvek 2 és 3), a Visual Studio Server Explorer és az Azure Tártallózó tooview nem alapértelmezett adatokat is használhatja.

## <a name="known-issues"></a>Ismert problémák

* hello Rsyslog információkat, és naplófájlt ügyfél által megadott csak parancsprogramok használatával érhető el.
