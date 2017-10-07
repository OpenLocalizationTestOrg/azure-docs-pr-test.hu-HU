---
title: "aaaConnect a Linux rendszerű számítógépek tooOperations felügyeleti csomaggal (OMS) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconnect Linux rendszerű számítógépek üzemeltetett Azure, más felhőalapú vagy helyszíni tooOMS használata Linux hello OMS-ügynököt."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte
ms.openlocfilehash: cb4fc671d0678f9fadc689c6ba7d719213aa61b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-linux-computers-toooperations-management-suite-oms"></a>A Linux rendszerű számítógépek tooOperations felügyeleti csomaggal (OMS) csatlakozás 

A Microsoft Operations Management Suite (OMS), összegyűjtheti, és Linux rendszerű számítógépek és a tároló-megoldásokkal, például a Docker, fizikai kiszolgálóként vagy virtuális gépek, virtuális gépek a helyszíni adatközpontban található adatok intézkedjen egy például az Amazon Web Services (AWS) vagy a Microsoft Azure-felhőben üzemeltetett szolgáltatás. OMS elérhető megoldásokat is használhatja például a változások követése, tooidentify konfigurációs módosításokat, és a frissítéskezelés toomanage szoftver frissítések tooproactively kezelése a Linux virtuális gépek hello életciklusát. 

hello OMS-ügynököt Linux kommunikál kimenő hello OMS szolgáltatáshoz a 443-as TCP-porton keresztül, és ha hello a számítógép tooa tűzfal vagy a proxy server toocommunicate hello interneten keresztül kapcsolódik. tekintse át [hello ügynök konfigurálása HTTP proxyk való használatra kiszolgáló vagy átjáró OMS](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) milyen konfiguráció módosításait toounderstand toobe alkalmazni kell.  Ha hello számítógépen a System Center 2016 - Operations Manager, illetve az Operations Manager 2012 R2, többhelyű hello OMS szolgáltatás toocollect adatok és előre toohello szolgáltatás is lehet, és továbbra is az Operations Manager által figyelt.  Az Operations Manager felügyeleti csoport, amely integrálva van a OMS által figyelt Linux rendszerű számítógépek nem kapják meg a konfigurációs adatforrások, és előre gyűjtött adatok hello felügyeleti csoporton keresztül.  hello OMS-ügynököt konfigurált tooreport toomore, mint egy munkaterület nem lehet.  

Az IT-biztonsági házirendeknek nem engedélyezi a hálózati tooconnect toohello Internet a számítógépek, ha hello ügynök konfigurált tooconnect toohello OMS átjáró tooreceive konfigurációs adatokat kell-e, és attól függően, hogy hello megoldás összegyűjtött adatok küldése engedélyezve van. További információk és bemutatjuk, hogyan tooconfigure egy OMS átjáró toohello OMS szolgáltatással, a Linux-ügynök OMS toocommunicate: [számítógépek tooOMS hello OMS átjáró használatával csatlakozzon](log-analytics-oms-gateway.md).  

hello következő ábra szemlélteti hello kapcsolat hello ügynök által felügyelt Linux rendszerű számítógépek és az OMS-ben, beleértve a hello irányát és portok között.

![közvetlen ügynökkommunikációhoz OMS diagramja](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Rendszerkövetelmények
Megkezdése előtt tekintse át a következő részleteket tooverify megfelel a hello Előfeltételek hello.

### <a name="supported-linux-operating-systems"></a>Támogatott Linux operációs rendszerek
a következő Linux terjesztésekről hello hivatalosan támogatottak.  Azonban hello Linux OMS-ügynököt előfordulhat, hogy is futtathatja más azokat a terjesztéseket nem szerepel a listában.

* Amazon Linux 2012.09 too2015.09 (x86/x64)
* CentOS Linux 5, 6 és 7 (x86/x64)
* Oracle Linux 5, 6 és 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6, 7 (x86/x64)
* Debian GNU/Linux 8 (x86/x64), 6, 7
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 és 12 (x86/x64)

### <a name="network"></a>Network (Hálózat)
hello alatti lista hello proxy és tűzfal konfigurációs adatokat szükséges hello Linux ügynök toocommunicate az OMS Szolgáltatáshoz. Akkor kimenő forgalomról beszélünk, a hálózati toohello OMS szolgáltatásból. 

|Ügynök erőforrása| Portok |  
|------|---------|  
|*.ods.opinsights.azure.com | 443-as port|   
|*.oms.opinsights.azure.com | 443-as port|   
|*.BLOB.Core.Windows.NET/ | 443-as port|   
|*.azure-automation.net | 443-as port|  

### <a name="package-requirements"></a>Csomag követelmények

 **Szükséges csomag**   | **Leírás**   | **Minimális verzió**
--------------------- | --------------------- | -------------------
Glibc | GNU C-függvénytár   | 2.5-12 
Openssl | OpenSSL-függvénytárak | 0.9.8e vagy 1.0
Curl | cURL-webügyfél | 7.15.5
Python-ctypes | | 
A PAM | Cserélhető hitelesítési modulok | 

> [!NOTE]
>  Rsyslog vagy a syslog-ng van szükség toocollect syslog-üzeneteket. syslog-események gyűjtése nem támogatott hello alapértelmezett syslog démon a Red Hat Enterprise Linux-, CentOS, és Oracle Linux verziója (sysklog) 5-ös verzióját. ezek azokat a terjesztéseket, a jelen verziójában a syslog-adatot toocollect hello rsyslog démon kell telepíteni, és tooreplace sysklog konfigurálva 

hello ügynök több csomagot tartalmaz. hello kiadás fájl tartalmazza a következő csomagok, a futó hello rendszerhéj köteg által elérhető hello `--extract`:

**Csomag** | **Verzió** | **Leírás**
----------- | ----------- | --------------
omsagent | 1.4.0 | Operations Management Suite-ügynök Linux hello
omsconfig | 1.1.1 | Az OMS-ügynököt hello konfiguráció ügynök
OMI | 1.2.0 | Nyissa meg a Management Infrastructure (OMI) – egy egyszerűsített CIM-kiszolgáló
az scx | 1.6.3 | Operációs rendszer teljesítménymutatók OMI a CIM-szolgáltatók
Apache-cimprov | 1.0.1 | Apache HTTP Server teljesítményfigyelés-szolgáltató az OMI. Apache HTTP Server észlelésekor telepítve.
MySQL-cimprov | 1.0.1 | MySQL-kiszolgáló teljesítményfigyelés-szolgáltató az OMI. Ha a MySQL/MariaDB kiszolgáló észleli telepítve.
docker-cimprov | 1.0.0 | Docker-szolgáltató az OMI. Ha a Docker észleli telepítve.

### <a name="compatibility-with-system-center-operations-manager"></a>A System Center Operations Manager kompatibilitása
hello Linux OMS-ügynököt hello System Center Operations Manager ügynök ügynök bináris fájlokat osztanak meg. Ha hello OMS-ügynököt a Linux, felügyelete jelenleg az Operations Manager rendszerre telepíti, akkor hello OMI és az SCX-csomagokat a hello tooa újabb verziójú. Ezen kiadási, a hello OMS és a System Center 2016 - Operations Manager, illetve az Operations Manager 2012 R2 ügynökök Linux kompatibilisek. 

> [!NOTE]
> A System Center 2012 SP1 és a korábbi verziók jelenleg nem kompatibilis vagy nem támogatott a hello Linux OMS-ügynököt.<br>
> Ha hello Linux OMS-ügynököt, amely jelenleg nem figyeli az Operations Manager által telepített tooa számítógép, amely majd kívánja toomonitor hello számítógépen az Operations Manager, módosítania kell a hello [OMI konfigurációs](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) előzetes toodiscovering hello számítógép. **Ez a lépés *nem* szükséges, ha hello Operations Manager-ügynök van telepítve előtt hello OMS-ügynököt Linux.**

### <a name="system-configuration-changes"></a>Rendszer-konfigurációs módosítások
Miután telepítette a hello OMS-ügynököt a Linux-csomagokat, hello következő további rendszerszintű konfigurációs a módosításai érvényesek lesznek. Ezen összetevők hello omsagent csomag eltávolításakor a rendszer törli.

* Egy jogosulatlan felhasználó nevű: `omsagent` jön létre. Ez a hello fiók hello omsagent démon fut.
* /Etc/sudoers.d/omsagent sudoers "tartalmaz" fájl jön létre. Ez engedélyezi a omsagent toorestart hello syslog és omsagent démonok. Sudo "tartalmaz" irányelvek sudo hello telepített verziója nem támogatott, akkor ezek a bejegyzések túl/etc/sudoers készültek.
* hello Rendszernaplózás konfigurálásánál módosított tooforward események toohello ügynök egy részét. További információkért lásd: hello **adatok gyűjtésének konfigurálása** az alábbi szakasz

### <a name="upgrade-from-a-previous-release"></a>Frissítse korábbi kiadásáról
Ebben a kiadásban 1.0.0-47 támogatottnál korábbi verzióit frissíteni. Hello telepítéssel rendelkező hello `--upgrade` parancs frissíti az összes tagjára vonatkozó hello ügynök toohello legújabb verziója.

## <a name="installing-hello-agent"></a>Hello ügynök telepítése

Ez a szakasz ismerteti, hogyan tooinstall hello bunndle, amely tartalmazza a Debian és RPM használata Linux OMS-ügynököt az egyes agent-összetevők hello csomagok.  Közvetlenül telepíthetők, vagy kibontott tooretrieve hello egyes csomagok.  

Az OMS-munkaterület azonosítója és a kulcs, amely toohello váltásával kell [OMS klasszikus portál](https://mms.microsoft.com).  A hello **áttekintése** hello felső menüben válassza a lap **beállítások**, majd keresse meg a túl**Sources\Linux kiszolgálók csatlakoztatva**.  Hello érték toohello jobb oldalán látható **munkaterület azonosítója** és **elsődleges kulcs**.  Másolással illessze be a kedvenc szerkesztő mindkettő.    

1. Letöltési hello legújabb [Linux (x64) OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) vagy [Linux x86 OMS-ügynököt](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) a Githubról.  
2. Vigye át hello megfelelő csomagot (x86 vagy x64) tooyour Linux-számítógép scp/sftp használatával.
3. Telepítés hello köteg hello segítségével `--install` vagy `--upgrade` argumentum. 

    > [!NOTE]
    > Ha a meglévő csomagokat például ha már telepítve van a Linux hello System Center Operations Manager-ügynök van telepítve, használja a hello `--upgrade` argumentum. a telepítés során a felügyeleti csomag tooconnect tooOperations biztosítanak hello `-w <WorkspaceID>` és `-s <Shared Key>` paraméterek.


#### <a name="tooinstall-and-onboard-directly"></a>tooinstall és közvetlen előkészítéséről
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="tooupgrade-hello-agent-package"></a>tooupgrade hello ügynök csomag
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="tooinstall-and-onboard-tooa-workspace-in-us-government-cloud"></a>tooinstall és a felhőben US Government előkészítésére tooa munkaterület
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-hello-agent-for-use-with-an-http-proxy-server-or-oms-gateway"></a>Hello ügynök használható egy HTTP-proxy kiszolgáló vagy az OMS-átjáró konfigurálása
hello Linux OMS-ügynököt támogatja egy HTTP vagy HTTPS-proxy kiszolgáló vagy az OMS-átjárószolgáltatás, toohello OMS keresztül kommunikál.  A névtelen és alapszintű hitelesítést (felhasználónév/jelszó) is támogatja.  

### <a name="proxy-configuration"></a>Proxykiszolgáló-konfiguráció
hello proxy konfigurációs értéke hello a következő szintaxist:

`[protocol://][user:password@]proxyhost[:port]`

Tulajdonság|Leírás
-|-
Protokoll|http vagy https
Felhasználó|Nem kötelező felhasználónév a proxyhitelesítéshez
jelszó|Opcionális jelszót a proxyhitelesítéshez
proxyhost|Cím vagy FQDN-jét hello proxy server/OMS átjáró
port|Hello proxy server/OMS átjáró választható portszáma

Például:`http://user01:password@proxy01.contoso.com:8080`

hello proxy kiszolgáló telepítése során vagy a telepítés után hello proxy.conf konfigurációs fájl módosításával adható meg.   

### <a name="specify-proxy-configuration-during-installation"></a>Adja meg a proxy konfigurációjának telepítése során
Hello `-p` vagy `--proxy` hello omsagent telepítési csomag argumentuma hello proxy konfigurációs toouse határozza meg. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p http://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-hello-proxy-configuration-in-a-file"></a>Egy fájlban hello proxy konfiguráció definiálása
hello proxykonfigurációt állítható be hello fájlok `/etc/opt/microsoft/omsagent/proxy.conf` és `/etc/opt/microsoft/omsagent/conf/proxy.conf `. hello fájlok is közvetlenül létrehozása és módosítása, de a rájuk vonatkozó engedélyek frissített toogrant hello omiuser felhasználó olvasási engedéllyel hello fájlokat kell lennie. Példa:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:8080"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-hello-proxy-configuration"></a>Hello proxy konfigurációjának eltávolítása
egy korábban definiált proxykonfigurációt tooremove és toodirect kapcsolat visszaállítani, akkor távolítsa el a hello proxy.conf fájlt:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-operations-management-suite"></a>Az Operations Management Suite előkészítési
Ha a munkaterület azonosítója és kulcsa nem adtak hello köteg telepítése során, hello ügynök ezt követően regisztrálni kell az Operations Management Suite.

### <a name="onboarding-using-hello-command-line"></a>A bevezetési hello parancssor használatával
Hello omsadmin.sh parancs ellátására hello munkaterület azonosítója és a munkaterület kulcsok. Ez a parancs (rendelkező sudo jogosultsági szint emeléséhez) rendszergazdaként kell futtatnia:
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>A bevezetési fájl használatával
1.  Hozzon létre hello fájlt `/etc/omsagent-onboard.conf`. hello olvasható és írható az alapvető kell lennie.
`sudo vi /etc/omsagent-onboard.conf`
2.  Szúrja be a következő sorokat a munkaterület azonosítója és a megosztott kulcs hello fájlban hello:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Futtassa a következő parancs tooOnboard tooOMS hello:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  a sikeres bevezetése hello fájl törlődik.

## <a name="enable-hello-oms-agent-for-linux-tooreport-toosystem-center-operations-manager"></a>A Linux tooreport tooSystem Center Operations Manager OMS-ügynököt hello engedélyezése
Hajtsa végre a következő lépéseket tooconfigure hello OMS-ügynököt Linux tooreport tooa System Center Operations Manager felügyeleti csoport hello.  

1. Hello fájl szerkesztése`/etc/opt/omi/conf/omiserver.conf`
2. Győződjön meg arról, hogy hello kezdődő **httpsport =** hello 1270-es port határozza meg. Például:`httpsport=1270`
3. Indítsa újra a hello OMI-kiszolgálón:`sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Ügynök naplók
naplók hello hello Linux OMS-ügynököt is található az: `/var/opt/microsoft/omsagent/<workspace id>/log/` hello naplók a következő hello omsconfig (ügynök konfigurációjának) program találhatók: `/var/opt/microsoft/omsconfig/log/` hello OMI és az SCX-összetevők naplóinak (amely teljesítményadatokat metrikák) helyen találhatók:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Napló Elforgatás konfigurációs ##
hello naplójának Elforgatás konfigurációját omsagent helyen találhatók:`/etc/logrotate.d/omsagent-<workspace id>`

hello alapértelmezett beállítások a következők: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-hello-oms-agent-for-linux"></a>Linux hello OMS-ügynök eltávolítása
hello ügynökcsomagokat is el kell távolítania futó hello .sh kötegfájl a hello `--purge` argumentum, amely teljes mértékben eltávolítja a hello ügynök és konfigurációja hello számítógépről.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="issue-unable-tooconnect-through-proxy-toooms"></a>Probléma: Nem tooconnect proxy tooOMS keresztül

#### <a name="probable-causes"></a>Lehetséges okok
* hello proxy bevezetése során megadott helytelen volt.
* hello OMS végpontok nincsenek az adatközpontban található whitelistested 

#### <a name="resolutions"></a>Megoldások
1. Reonboard toohello OMS szolgáltatás hello Linux OMS-ügynököt a következő parancsot hello kapcsolóval hello segítségével a `-v` engedélyezve van. Ez lehetővé teszi a részletes kimenet hello ügynök hello proxy toohello OMS szolgáltatás keresztül kapcsolódik. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Tekintse át a hello szakasz [hello ügynök konfigurálása HTTP proxyk való használatra server(#configuring the-agent-for-use-with-a-http-proxy-server) megfelelően konfigurált tooverify hello ügynök toocommunicate proxykiszolgálón keresztül.    
* Ellenőrizze, hogy a következő OMS Szolgáltatásvégpontok hello szerepel az engedélyezési listán:

    |Ügynök erőforrása| Portok |  
    |------|---------|  
    |*.ods.opinsights.azure.com | 443-as port|   
    |*.oms.opinsights.azure.com | 443-as port|   
    |ods.systemcenteradvisor.com | 443-as port|   
    |*.BLOB.Core.Windows.NET/ | 443-as port|   

### <a name="issue-you-receive-a-403-error-when-trying-tooonboard"></a>Probléma: 403-as hibaüzenetet kap tooonboard közben

#### <a name="probable-causes"></a>Lehetséges okok
* Dátum és idő nem megfelelő Linux-kiszolgálón 
* Munkaterületének Azonosítóját és kulcsát használt nem megfelelőek

#### <a name="resolution"></a>Megoldás:

1. Ellenőrizze a Linux-kiszolgálóra hello parancs dátummal hello ideje. Ha hello idő 15 perc, az aktuális időponthoz képest +/-, akkor bevezetési sikertelen lesz. toocorrect a frissítés hello dátumot és/vagy a Linux-kiszolgáló időzónáját. 
2. Ellenőrizze, hogy Linux hello legújabb verziójának hello OMS-ügynököt telepítette.  hello legújabb verziója most értesítést küld, ha eltérésére idő hello bevezetési hibája okozza.
3. Használja a megfelelő munkaterület Azonosítóját és kulcsát hello telepítési utasításai a témakörben fentebb található Reonboard.

### <a name="issue-you-see-a-500-and-404-error-in-hello-log-file-right-after-onboarding"></a>Probléma: Egy 500 és 404-es hiba látható hello naplófájlban bevezetése után
Ez az egy ismert probléma, amely az OMS-munkaterület Linux adatok első feltöltéskor. Ez nem befolyásolja az elküldött és a szolgáltatás élmény adatok.

### <a name="issue--you-are-not-seeing-any-data-in-hello-oms-portal"></a>Probléma: Nem Ön egyetlen megadott adattal sem hello OMS-portálon

#### <a name="probable-causes"></a>Lehetséges okok

- Bevezetési toohello OMS-szolgáltatás nem tudta
- Kapcsolat toohello OMS szolgáltatás le van tiltva.
- OMS-ügynököt a Linux-adatok biztonsági mentése

#### <a name="resolutions"></a>Megoldások
1. Ellenőrizze, hogy ha bevezetési hello OMS szolgáltatás sikerességéről létezésének hello következő fájlt:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Hello segítségével Reonboard `omsadmin.sh` parancssori utasításokat
3. Ha proxyt használ, tekintse meg a korábban megadott toohello proxy megoldási lépések.
4. Néhány esetben ha hello Linux OMS-ügynök nem tud kommunikálni a hello OMS szolgáltatás hello ügynökön adatai aszinkron toohello teljes puffer mérete 50 MB. hello Linux OMS-ügynököt újra kell indítani a hello a következő parancs futtatásával: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Ez a probléma fennáll ügynök verziója 1.1.0-28 és újabb verziók.
> 