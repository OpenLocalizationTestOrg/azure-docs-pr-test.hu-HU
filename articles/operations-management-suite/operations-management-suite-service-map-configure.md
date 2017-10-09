---
title: "az Operations Management Suite a Szolgáltatástérkép aaaConfigure |} Microsoft Docs"
description: "Szolgáltatástérkép Operations Management Suite megoldás, amely automatikusan felderíti az alkalmazás-összetevők a Windows és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció. Ez a cikk a Service Map telepítése a környezetben, és használja azt a különféle forgatókönyvekhez, amik részletesen."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: 3127f4440f2886370f8ff617c405c6d70a926eb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-service-map-in-operations-management-suite"></a>Az Operations Management Suite a Szolgáltatástérkép konfigurálása
Szolgáltatástérkép automatikusan észleli a Windows és Linux rendszerek alkalmazás-összetevők és a maps hello szolgáltatások közötti kommunikáció. Használhatja a kiszolgálók, ha Ön gondolunk, hogy--összekapcsolt rendszerekhez, hogy a kritikus szolgáltatások tooview. Szolgáltatástérkép kiszolgálók, folyamatok és portok közötti kapcsolatok között nincs konfigurációjával kapcsolatban egy ügynök telepítése nem szükséges bármely TCP-csatlakoztatott architektúra jeleníti meg.

Ez a cikk ismerteti a Service Map és bevezetése az ügynökök konfigurálása hello részleteit. Szolgáltatástérkép használatával kapcsolatos információkért lásd: [hello Szolgáltatástérkép megoldás használata az Operations Management Suite](operations-management-suite-service-map.md).

## <a name="dependency-agent-downloads"></a>A függőségi ügynök letöltése
| Fájl | Operációs rendszer | Verzió | AZ SHA-256-RA |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |


## <a name="connected-sources"></a>Összekapcsolt források
Szolgáltatástérkép az adatok lekérése hello Microsoft függőségi ügynök. hello függőségi ügynök hello OMS-ügynököt a a kapcsolatok tooOperations felügyeleti csomag függ. Ez azt jelenti, hogy a kiszolgáló hello OMS-ügynököt telepítette, és konfigurálta az első, és a függőségi ügynök telepíthető hello kell rendelkeznie. hello következő táblázatban hello Szolgáltatástérkép megoldást támogató hello csatlakoztatott adatforrások.

| Csatlakoztatott adatforrás | Támogatott | Leírás |
|:--|:--|:--|
| Windows-ügynökök | Igen | Szolgáltatástérkép elemzi és Windows-ügynök számítógépekről gyűjt adatokat. <br><br>A hozzáadása toohello [OMS-ügynököt](../log-analytics/log-analytics-windows-agents.md), Windows-ügynökök hello Microsoft függőségi ügynök szükséges. Lásd: hello [támogatott operációs rendszerek](#supported-operating-systems) operációs rendszerek teljes listáját. |
| Linux-ügynökök | Igen | Szolgáltatástérkép elemzi, és a Linux-ügynök számítógépekről gyűjt adatokat. <br><br>A hozzáadása toohello [OMS-ügynököt](../log-analytics/log-analytics-linux-agents.md), Linux-ügynökök hello Microsoft függőségi ügynök szükséges. Lásd: hello [támogatott operációs rendszerek](#supported-operating-systems) operációs rendszerek teljes listáját. |
| System Center Operations Manage felügyeleti csoport | Igen | Szolgáltatástérkép elemzi, és összegyűjti az adatokat a Windows és Linux-ügynökök a csatlakoztatott [System Center Operations Manager felügyeleti csoport](../log-analytics/log-analytics-om-agents.md). <br><br>A hello System Center Operations Manager ügynök számítógép tooOperations közvetlen kapcsolat a felügyeleti csomag szükség. Adattárból hello felügyeleti csoport toohello Operations Management Suite adat továbbítódik.|
| Azure Storage-fiók | Nem | Szolgáltatástérkép adatokat gyűjt ügynök számítógépekről, nincsenek adatok, toocollect Azure Storage-ból. |

Szolgáltatástérkép csak 64 bites platformokat támogatja.

Windows, a Microsoft Monitoring Agent (MMA) hello használja mind a System Center Operations Manager és az Operations Management Suite toogather, és a figyelési adatok küldése. (Ez az ügynök hello System Center Operations Manager ügynök, OMS-ügynököt, Log Analytics Agent, MMA vagy közvetlen ügynök hello környezettől függően nevezzük.) A System Center Operations Manager és az Operations Management Suite adja meg a hello MMA különböző out-az-hello mezőben verzióit. Ezen verziói egyes jelenthetik tooSystem Center Operations Manager, a felügyeleti csomag tooOperations vagy tooboth.  

Linux OMS-ügynököt hello Linux összegyűjti és figyelési felügyeleti csomag adatok tooOperations küld. Szolgáltatástérkép használhatja a közvetlen OMS-ügynököt a kiszolgálón vagy kiszolgálókon, amelyek csatlakoztatott tooOperations felügyeleti csomag a System Center Operations Manager felügyeleti csoportot keresztül.  

Ebben a cikkben kifejezés tooall ügynökök – e Linux vagy a Windows-e csatlakoztatva a tooa System Center Operations Manager felügyeleti csoport vagy közvetlenül a felügyeleti csomag--tooOperations "OMS-ügynököt." hello, Hello ügynök hello adott központi telepítés neve csak akkor, ha szükség van a környezetben fogjuk használni.

hello Szolgáltatástérkép ügynök nem továbbít adatokat saját magát, és nem igényel módosításokat toofirewalls vagy portok. Szolgáltatástérkép hello adatai mindig átkerülnek hello OMS-ügynököt tooOperations felügyeleti csomag által közvetlenül vagy hello OMS-átjárón keresztül.

![Szolgáltatástérkép ügynökök](media/oms-service-map/agents.png)

Ha egy felügyeleti csoporthoz csatlakoztatott tooOperations felügyeleti csomag a System Center Operations Manager-ügyfél:

- A System Center Operations Manager-ügynökök hello Internet tooconnect tooOperations Management Suite férhet hozzá, ha nincs szükség semmilyen további konfigurációra.  
- Ha a System Center Operations Manager-ügynökök nem tud hozzáférni az Operations Management Suite hello interneten keresztül, akkor tooconfigure hello OMS átjáró toowork a System Center Operations Manager.
  
Ha hello közvetlen OMS-ügynököt használ, kell tooconfigure hello maga OMS-ügynököt tooconnect tooOperations Management Suite vagy tooyour OMS-átjáró. hello OMS átjáró tölthető le: hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

### <a name="management-packs"></a>Felügyeleti csomagok
Az Operations Management Suite-munkaterülethez a Szolgáltatástérkép aktiválásakor 300 KB-os felügyeleti csomag munkaterületről tooall hello Windows kiszolgálók küldése. Ha a System Center Operations Manager ügynököt használ egy [csatlakoztatott felügyeleti csoport](../log-analytics/log-analytics-om-agents.md), hello Szolgáltatástérkép felügyeleti csomag a System Center Operations Manager telepítésének módja. Hello ügynökök közvetlenül csatlakoztatott, ha az Operations Management Suite hello felügyeleti csomag nyújt.

Microsoft.IntelligencePacks.ApplicationDependencyMonitor hello felügyeleti csomag neve. Az írásbeli too%Programfiles%\Microsoft figyelés Agent\Agent\Health szolgáltatás State\Management Packs\. hello hello felügyeleti csomag, amelynek % Program files%\Microsoft figyelés Agent\Agent\Health szolgáltatás State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Telepítés
### <a name="install-hello-dependency-agent-on-microsoft-windows"></a>Microsoft Windows hello függőségi ügynök telepíthető
Rendszergazdai jogosultság szükséges tooinstall vagy hello ügynök eltávolítása.

hello függőségi ügynök telepítve van a Windows rendszerű számítógépeken InstallDependencyAgent-Windows.exe keresztül. Ha a végrehajtható fájl kapcsolók nélkül futtatja, az elindít egy varázslót, hogy követésével tooinstall interaktív módon.  

Hello lépéseket tooinstall hello függőségi ügynök minden Windows-számítógépen a következő használja:

1.  Telepítés hello OMS-ügynököt a hello található utasítások segítségével: [csatlakozás Windows számítógépek toohello az Azure Naplóelemzés szolgáltatás](../log-analytics/log-analytics-windows-agents.md).
2.  Windows hello-ügynök letöltése, és futtassa a következő parancs hello segítségével: <br>`InstallDependencyAgent-Windows.exe`
3.  Hajtsa végre a hello varázsló tooinstall hello ügynök.
4.  Ha hello függőségi ügynök toostart sikertelen, tekintse meg a hibával kapcsolatos részletes információk hello naplókat. A Windows-ügynökök hello naplókönyvtár %Programfiles%\Microsoft függőségi Agent\logs. 

#### <a name="windows-command-line"></a>A Windows parancssor
A hello tábla tooinstall a parancssorból a következő beállításokkal. hello telepítési jelző, hello telepítő futtatásához hello segítségével listája toosee /? Ez a jelző az alábbiak szerint.

    InstallDependencyAgent-Windows.exe /?

| Jelzője | Leírás |
|:--|:--|
| /? | Hello parancssori kapcsolók listájának lekérése. |
| / S | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |

Hello Windows függőségi ügynök fájljait alapértelmezésben a C:\Program Files\Microsoft függőségi ügynök kerül.

### <a name="install-hello-dependency-agent-on-linux"></a>Hello függőségi ügynök telepítése Linux rendszerre
Legfelső szintű hozzáférés szükséges tooinstall vagy hello ügynök konfigurálása.

Linux rendszerű számítógépek InstallDependencyAgent Linux64.bin, egy önkicsomagoló bináris héjparancsfájlt keresztül hello függőségi ügynök van telepítve. Futtassa hello fájlt megosztása, vagy adja hozzá engedélyek toohello fájl saját magát.
 
Hello lépéseket tooinstall hello függőségi ügynök a Linux rendszerű számítógépekre a következő használja:

1.  Telepítés hello OMS-ügynököt a hello található utasítások segítségével: [adatok gyűjtésére és kezelésére a Linux rendszerű számítógépek](https://technet.microsoft.com/library/mt622052.aspx).
2.  Legfelső szintű hello Linux függőségi ügynök telepítéséhez használja a következő parancs hello:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  Ha hello függőségi ügynök toostart sikertelen, tekintse meg a hibával kapcsolatos részletes információk hello naplókat. A Linux-ügynökök hello naplókönyvtár /var/opt/microsoft/dependency-agent/log.

hello telepítési jelző, a hello programjának futtatása listája toosee hello - súgó jelzőt az alábbiak szerint.

    InstallDependencyAgent-Linux64.bin -help

| Jelzője | Leírás |
|:--|:--|
| -Súgó | Hello parancssori kapcsolók listájának lekérése. |
| -s | Felhasználói beavatkozás nélküli telepítés végrehajtásához. |
| --ellenőrzése | Ellenőrizze az engedélyeit és hello operációs rendszer, de ne telepítse hello ügynök. |

Hello függőségi ügynök fájlok kerülnek, a következő könyvtárak hello:

| Fájlok | Hely |
|:--|:--|
| Alapvető fájljait | /OPT/Microsoft/Dependency-Agent |
| Naplófájlok | /var/OPT/Microsoft/Dependency-Agent/log |
| Olyan konfigurációs fájlt | /ETC/OPT/Microsoft/Dependency-Agent/config |
| Végrehajtható fájlok | /OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent<br>/OPT/Microsoft/Dependency-Agent/bin/Microsoft-Dependency-Agent-Manager |
| A tároló bináris fájljai | /var/OPT/Microsoft/Dependency-Agent/Storage |

## <a name="installation-script-examples"></a>Telepítési parancsfájl példák
tooeasily telepítése több kiszolgálón függőségi ügynök hello egyszerre, hanem toouse parancsfájlt. A következő parancsfájl példák toodownload hello használja, és hello függőségi ügynök telepíthető Windows vagy Linux.

### <a name="powershell-script-for-windows"></a>A Windows PowerShell-parancsfájl
```PowerShell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>A Linux rendszerhez parancsfájl
```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Célállapot-konfiguráló
toodeploy hello függőségi ügynök keresztül célállapot-konfiguráció, hello xPSDesiredStateConfiguration modul és a kód hasonló hello használhatja:
```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install hello Dependency Agent
    xRemoteFile DAPackage 
    {
        Uri = "https://aka.ms/dependencyagentwindows"
        DestinationPath = $DAPackageLocalPath
    }

    xPackage DA
    {
        Ensure="Present"
        Name = "Dependency Agent"
        Path = $DAPackageLocalPath
        Arguments = '/S'
        ProductId = ""
        InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
        InstalledCheckRegValueName = "DisplayName"
        InstalledCheckRegValueData = "Dependency Agent"
        DependsOn = "[xRemoteFile]DAPackage"
    }
  }
}
```

## <a name="uninstallation"></a>Az Eltávolítás
### <a name="uninstall-hello-dependency-agent-on-windows"></a>Távolítsa el a függőségi ügynök a Windows hello
A rendszergazda eltávolíthat hello függőségi ügynök a Windows Vezérlőpult segítségével.

A rendszergazda %Programfiles%\Microsoft függőségi Agent\Uninstall.exe toouninstall hello függőségi ügynök is futtathatja.

### <a name="uninstall-hello-dependency-agent-on-linux"></a>Hello Linux függőségi ügynök eltávolítása
toocompletely eltávolítás hello függőségi Linux-ügynököt, el kell távolítania a hello ügynök magát, és hello összekötőt, amely automatikusan telepítve van a hello ügynökkel. Eltávolíthatja, mindkettő hello egyetlen parancs a következő használatával:

    rpm -e dependency-agent dependency-agent-connector

## <a name="troubleshooting"></a>Hibaelhárítás
Ha bármely telepítését és futtatását a Service Map, akkor ez a szakasz segítséget. Ha még mindig nem tudja megoldani a problémát, forduljon a Microsoft Support.

### <a name="dependency-agent-installation-problems"></a>A függőségi ügynök telepítési problémák
#### <a name="installer-asks-for-a-reboot"></a>Telepítő kéri az újraindítást
hello függőségi ügynök *általában* nem kell indítani a telepítés vagy eltávolítás. Ritka esetekben azonban a Windows Server szükséges egy újraindítás toocontinue nélküli telepítés. Ez akkor fordul elő, amikor a függőség, a Microsoft Visual C++ terjeszthető változata, általában hello zárolt fájlok miatt újraindítás szükséges.

#### <a name="message-unable-tooinstall-dependency-agent-visual-studio-runtime-libraries-failed-tooinstall-code--codenumber-appears"></a>Üzenet "nem tooinstall függőségi ügynök: a Visual Studio futtató könyvtárak nem sikerült tooinstall (kód = [code_number])" jelenik meg

a Microsoft függőségi ügynök hello hello Microsoft Visual Studio futtató könyvtárak épül. Üzenetet fog kapni, ha probléma merül fel hello kódtárak telepítése során. 

hello futásidejű könyvtár telepítők naplók hello %LOCALAPPDATA%\temp mappában hozzon létre. hello fájl dd_vcredist_arch_yyyymmddhhmmss.log, ahol *architektúrája* "x86" vagy "amd64" és *ééééhhnnóóppmm* hello dátum és idő (24 órás formátumban) hello naplófájl létrehozásának. hello napló hello probléma, amely blokkolja a telepítési ismerteti.

Előfordulhat, hogy hasznos tooinstall hello [legújabb futtató könyvtárak](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) saját kezűleg első.

hello következő táblázatban kód számok és javasolt megoldások.

| Kód | Leírás | Megoldás: |
|:--|:--|:--|
| 0x17 | hello könyvtár telepítő szükséges a Windows update, amely nem lett telepítve. | Hely hello legutóbbi könyvtár installer naplójának.<br><br>Ha egy hivatkozás túl "Windows8.1-KB2999226-x64.msu" követi egy sor "0x80240017 hiba: sikertelen tooexecute MSU csomag," hello Előfeltételek tooinstall KB2999226 nem rendelkezik. Hello Előfeltételek című hello utasításokat követve [Windows Universal C futásidejű](https://support.microsoft.com/kb/2999226). Előfordulhat, hogy a Windows Update toorun kell, és indítsa újra a többször szereplő sorrendben tooinstall hello előfeltételek.<br><br>Futtassa újra a hello Microsoft függőségi ügynök telepítőjét. |

### <a name="post-installation-issues"></a>Telepítés utáni problémák
#### <a name="server-doesnt-appear-in-service-map"></a>Kiszolgáló nem jelenik meg a Service Map
Ha a függőségi ügynök telepítése sikeres volt, de nem jelenik meg a kiszolgáló a Service Map megoldás hello:
* Hello függőségi ügynök sikeres telepítését? Ezt ellenőrizheti a toosee ellenőrzése, ha hello szolgáltatás telepítve van és fut.<br><br>
**Windows**: keressen a "Microsoft Dependency ügynök." nevű hello szolgáltatás<br>
**Linux**: "microsoft-függőség-ügynök." folyamatának futtatása hello keresi

* Biztosan a hello [ingyenes tarifacsomag Operations Management Suite/Log Analyticshez](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? hello csomagra lehetővé teszi, hogy toofive egyedi Szolgáltatástérkép kiszolgáló. További kiszolgálók nem jelenik meg a Service Map, még akkor is, ha a korábbi öt hello már nem küld adatokat.

* A küldő kiszolgálónapló és a teljesítmény adatok tooOperations felügyeleti csomag van? Nyissa meg a keresési tooLog, és futtassa a következő lekérdezés a számítógép hello: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Kapott események számos hello eredmények között? Az újabb hello adatai? Ha igen, az OMS-ügynököt megfelelően működik-e és hello Operations Management Suite szolgáltatással folytatott kommunikáció. Ha nem, nézze meg hello OMS-ügynököt a kiszolgálón: [OMS ügynök a Windows hibaelhárítási](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) vagy [OMS-ügynököt a Linux hibaelhárítási](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Kiszolgáló megjelenik a Szolgáltatástérkép, de nincs folyamat
Ha a kiszolgáló a Service Map látja, de egyetlen folyamat vagy a kapcsolat adatmező, amely azt jelzi, hogy hello függőségi ügynök telepítve és fut, de hello kernel-illesztőprogram nem töltődött be. 

Ellenőrizze a hello C:\Program Files\Microsoft függőségi Agent\logs\wrapper.log fájlt (Windows) vagy /var/opt/microsoft/dependency-agent/log/service.log fájlt (Linux). hello hello fájl utolsó sora kell jeleznie, miért hello kernel nem töltődött be. Például hello kernel lehetséges, hogy nem támogatja a Linux Ha frissítette a kernel.

## <a name="data-collection"></a>Adatgyűjtés
Minden ügynök tootransmit számíthat körülbelül 25 MB naponkénti, attól függően, hogy hogyan összetett a rendszer függőségek használhatók. Minden ügynök 15 másodpercenként Szolgáltatástérkép függőségi adatokat küld.  

hello függőségi ügynök általában akkor 0,1 rendszermemória és 0,1 rendszer Processzor.

## <a name="supported-azure-regions"></a>Támogatott Azure-régiók
Szolgáltatástérkép érhető el jelenleg a következő Azure-régiók hello:
- USA keleti régiója
- Nyugat-Európa
- USA nyugati középső régiója


## <a name="supported-operating-systems"></a>Támogatott operációs rendszerek
hello alábbi szakaszok tartalmazzák hello támogatott operációs rendszerek hello függőségi ügynök. Szolgáltatástérkép nem támogatja 32 bites operációs rendszert.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Asztali Windows
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, a CentOS Linux és az Oracle Linux (az RHEL Kernel)
- Csak az alapértelmezett és az SMP Linux kernel verziókban támogatott.
- Nem szabványos kernel kiadását, például a Xen, és a fizikai nem támogatottak a Linux-disztribúció. A rendszer a "2.6.16.21-0.8-xen" hello kiadás karakterlánccal például nem támogatott.
- Egyéni mag, beleértve a szabványos kernelek újrafordításainak maximális nem támogatottak.
- CentOSPlus kernel nem támogatott.
- Ez a cikk későbbi részében Oracle szoros vállalati Kernel (UEK) vonatkozik.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Operációs rendszer verziója | Kernel-verzió |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Operációs rendszer verziója | Kernel-verzió |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Operációs rendszer verziója | Kernel-verzió |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417<br>2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Szoros vállalati Kernel az Oracle Enterprise Linux

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Operációs rendszer verziója | Kernel-verzió
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Operációs rendszer verziója | Kernel-verzió
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Operációs rendszer verziója | Kernel-verzió
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Operációs rendszer verziója | Kernel-verzió
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>diagnosztikai és használati adatok
A Microsoft automatikusan használati és teljesítményadatokat gyűjt a Szolgáltatástérkép szolgáltatás hello használata. A Microsoft ezen adatok tooprovide használ, és hello minőségének, biztonsági és hello Szolgáltatástérkép szolgáltatás integritásának javítására. Adatok tartalmazzák a szoftverek, például az operációs rendszer és verzió hello konfigurációjával kapcsolatos információkat. Is IP-cím, a DNS-nevét és a munkaállomás nevét a rendelés tooprovide pontos és hatékony hibaelhárítási képességei. Nem gyűjtünk neveket, címeket és egyéb kapcsolattartási adatait.

Az adatok gyűjtésével és felhasználásával további információkért lásd: hello [Microsoft Online Services adatvédelmi nyilatkozatát](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Következő lépések
- Ismerje meg, hogyan túl[használja a Service Map](operations-management-suite-service-map.md) telepítése és konfigurálása után.
