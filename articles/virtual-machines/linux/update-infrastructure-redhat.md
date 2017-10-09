---
title: "Hat frissítés infrastruktúra (RHUI) aaaRed |} Microsoft Docs"
description: "Ismerje meg a Red Hat frissítés infrastruktúra (RHUI) igény Red Hat Enterprise Linux-példányok a Microsoft Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: BorisB2015
manager: timlt
editor: 
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/13/2017
ms.author: borisb
ms.openlocfilehash: cc244857104b25e4e61862c518db77e915e137ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="red-hat-update-infrastructure-rhui-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Red Hat frissítési infrastruktúra (RHUI) igény Red Hat Enterprise Linux virtuális gépek Azure-ban
Hello igény Red Hat Enterprise Linux (RHEL) lemezképeket Azure piactéren alapján létrehozott virtuális gépeken regisztrált tooaccess hello Red Hat frissítés infrastruktúra (RHUI) az Azure-ban telepítve.  hello igény szerinti RHEL célpéldánynál hozzáférés tooa regionális yum tárház és képes tooreceive növekményes frissítéseket.

hello yum tárház listájában RHUI kezeli, amely konfigurálva van az RHEL példányának kiépítése során. Nem kell toodo további konfiguráció - futtatása `yum update` után RHEL példány készen tooget hello legújabb frissítéseit.

> [!NOTE]
> 2016 szeptemberétől helyeztünk üzembe egy frissített Azure RHUI és 2017. január azt elindított hello fázisokra bontva történő leállításának régebbi Azure RHUI. Ha használta hello RHEL képet (vagy a pillanatképek) 2016 szeptemberétől vagy későbbi - valószínűleg intézkedés nem szükséges. Ha azonban még a korábbi pillanatképek/virtuális gépek, a konfigurációjukat kell toobe folyamatos hozzáférést toohello Azure RHUI frissítve.
> 

## <a name="rhui-azure-infrastructure-update"></a>RHUI Azure-infrastruktúra frissítése
2016 szeptemberétől Azure tartozik egy új Red Hat frissítés infrastruktúra (RHUI) kiszolgálókon. Ezekre a kiszolgálókra telepített [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) , hogy egy végpontot (rhui-1.microsoft.com) régió függetlenül minden virtuális gép által használható. hello hello Azure piactér (dátummal 2016 szeptemberétől vagy újabb verzió) pont toohello új Azure RHUI kiszolgálók új RHEL használatalapú fizetés (PAYG) lemezképeit, és minden további műveletet nem igénylő.

### <a name="determine-if-action-is-required"></a>Megállapítja, hogy szükség-e a művelet
Ha a csatlakozás az Azure RHEL PAYG virtuális gép tooAzure RHUI problémákat tapasztal, kövesse az alábbi lépéseket
1. Vizsgálja meg a Virtuálisgép-konfiguráció Azure RHUI végpont

    Ellenőrizze, hogy `/etc/yum.repos.d/rh-cloud.repo` fájl túl hivatkozása`rhui-[1-3].microsoft.com` a baseurl a `[rhui-microsoft-azure-rhel*]` hello fájl szakaszában. Ha az - használ hello az új Azure RHUI.

    Ha azt tooa helyre mutató a következő mintát hello `mirrorlist.*cds[1-4].cloudapp.net` -hello konfigurációs frissítés szükség.

    Ha hello új konfigurációt használ, és továbbra sem sikerül kapcsolódni tooAzure RHUI - fájl a Microsoft vagy a Red Hat támogatási esetet.

    > [!NOTE]
    > Hozzáférés tooAzure által szolgáltatott RHUI korlátozott toohello virtuális gépeken belül [Microsoft Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).
    > 

2. Ha hello régi Azure RHUI marad. Ehhez ellenőrizze, és azt szeretné, hogy tooautomatically frissítés hello konfigurációs, hajtható végre a következő parancs hello:

    `sudo yum update RHEL6`vagy `sudo yum update RHEL7` hello RHEL termékcsalád verziójától függően.

3. Ha már nem kapcsolható toohello régi Azure RHUI, hajtsa végre hello manuális lépések hello következő szakaszban leírt.

4. Ellenőrizze, hogy tooupdate hello konfigurációs hello forrás lemezkép vagy pillanatkép hatással a virtuális gép át lett kiépítve.

### <a name="phased-shutdown-of-hello-old-azure-rhui"></a>Szakaszos leállása hello régi Azure RHUI
A hello hello leállítás során régi Azure RHUI azt korlátozása elérhető tooit az alábbiak szerint:

1. Az IP-címek tooit már csatlakozó access (ACL) tooset további korlátozásához. Lehetséges mellékhatása: Ha továbbra is használja a régi Azure RHUI hello – az új virtuális gépeket nem lehet képes tooconnect tooit. RHEL rendelkező virtuális gépek dinamikus IP-címek, amelyek halad át leállítás/felszabadítani/start feladatütemezési jelenhet meg új VI, és ezért is sikerült meghiúsul az tooconnect toohello régi Azure RHUI

2. Állítsa le a tükör tartalomkézbesítési kiszolgálók. Lehetséges mellékhatása:, azt több CDSes leállítása jelenhetnek már `yum update` karbantartási idő, amíg hello további időtúllépések terjesztésipont-amikor már nem kapcsolható toohello régi Azure RHUI.

### <a name="hello-ips-for-hello-new-rhui-content-delivery-servers-are"></a>IP-címek hello hello új RHUI tartalomkézbesítési kiszolgálók vannak
Hálózati konfiguráció toofurther használata RHEL PAYG virtuális gépek elérésének korlátozása, győződjön meg arról, hogy a következő IP-címek hello használata engedélyezett `yum update` toowork a hello környezettől függően. 

```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213

# Azure US Government
13.72.186.193

# Azure Germany
51.5.243.77
51.4.228.145
```

### <a name="manual-update-procedure-toouse-hello-new-azure-rhui-servers"></a>Kézi frissítés eljárás toouse hello új Azure RHUI kiszolgálók
(A curl) keresztül letöltési hello nyilvános kulcs aláírása

```bash
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 
```

Ellenőrizze a letöltött hello kulcsot

```bash
gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release
```

Ellenőrizze a hello kimeneti, győződjön meg arról `keyid` és `user ID packet`:

```bash
Version: GnuPG v1.4.7 (GNU/Linux)
:public key packet:
        version 4, algo 1, created 1446074508, expires 0
        pkey[0]: [2048 bits]
        pkey[1]: [17 bits]
        keyid: EB3E94ADBE1229CF
:user ID packet: "Microsoft (Release signing) <gpgsecurity@microsoft.com>"
:signature packet: algo 1, keyid EB3E94ADBE1229CF
        version 4, created 1446074508, md5len 0, sigclass 0x13
        digest algo 2, begin of digest 1a 9b
        hashed subpkt 2 len 4 (sig created 2015-10-28)
        hashed subpkt 27 len 1 (key flags: 03)
        hashed subpkt 11 len 5 (pref-sym-algos: 9 8 7 3 2)
        hashed subpkt 21 len 3 (pref-hash-algos: 2 8 3)
        hashed subpkt 22 len 2 (pref-zip-algos: 2 1)
        hashed subpkt 30 len 1 (features: 01)
        hashed subpkt 23 len 1 (key server preferences: 80)
        subpkt 16 len 8 (issuer key ID EB3E94ADBE1229CF)
        data: [2047 bits]
```

Hello nyilvános kulcs telepítése

```bash
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release
```

Töltse le, ellenőrizze és RPM ügyfél telepítése

Letöltés: Az RHEL 6

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel6/rhui-azure-rhel6-2.0-2.noarch.rpm 
```

Az RHEL 7

```bash
curl -o azureclient.rpm https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel7/rhui-azure-rhel7-2.0-2.noarch.rpm  
```

Ellenőrizze a:

```bash
rpm -Kv azureclient.rpm
```

A kimeneti ellenőrizze, hogy az aláírás hello csomag az OK gombra

```bash
azureclient.rpm:
    Header V3 RSA/SHA256 Signature, key ID be1229cf: OK
    Header SHA1 digest: OK (927a3b548146c95a3f6c1a5d5ae52258a8859ab3)
    V3 RSA/SHA256 Signature, key ID be1229cf: OK
    MD5 digest: OK (c04ff605f82f4be8c96020bf5c23b86c)
```

Hello RPM telepítése

```bash
sudo rpm -U azureclient.rpm
```

Befejezett győződjön meg arról, hogy elérhető Azure RHUI űrlap hello VM

### <a name="all-in-one-script-for-automating-hello-preceding-task"></a>A feladat megelőző hello automatizálásához mindent egy parancsfájl
Az érintett virtuális gépek toohello új Azure RHUI kiszolgálók frissítésének szükséges tooautomate hello feladatok a parancsfájl következő hello használata.

```sh
# Download key
curl -o RPM-GPG-KEY-microsoft-azure-release https://download.microsoft.com/download/9/D/9/9d945f05-541d-494f-9977-289b3ce8e774/microsoft-sign-public.asc 

# Validate key
if ! gpg --list-packets --verbose < RPM-GPG-KEY-microsoft-azure-release | grep -q "keyid: EB3E94ADBE1229CF"; then
    echo "Keyfile azure.asc NOT valid. Exiting."
    exit 1
fi

# Install Key
sudo install -o root -g root -m 644 RPM-GPG-KEY-microsoft-azure-release /etc/pki/rpm-gpg
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-microsoft-azure-release

# Download RPM package
if grep -q "release 7" /etc/redhat-release; then
    ver=7
elif  grep -q "release 6" /etc/redhat-release; then
    ver=6
else
    echo "Version not supported, exiting"
    exit 1
fi

url=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel$ver/rhui-azure-rhel$ver-2.0-2.noarch.rpm
curl -o azureclient.rpm "$url"

# Verify package
if ! rpm -Kv azureclient.rpm | grep -q "key ID be1229cf: OK"; then
    echo "RPM failed validation ($url)"
    exit 1
fi

# Install package
sudo rpm -U azureclient.rpm
```

## <a name="rhui-overview"></a>RHUI áttekintése
[Red Hat frissítési infrastruktúra](https://access.redhat.com/products/red-hat-update-infrastructure) egy kiválóan méretezhető megoldást toomanage yum tárház tartalmának Red Hat hitelesített szolgáltatók által üzemeltetett Red Hat Enterprise Linux-felhő példányok kínál. Hello fölérendelt cellulóz projekt alapján, RHUI lehetővé teszi a szolgáltatók toolocally tükrözött tárház Red Hat által szolgáltatott tartalom hozzon létre egyéni adattárak a saját tartalom, és ezek adattárak elérhető tooa nagy egy elosztott terhelésű végfelhasználók csoport a tartalom kézbesítésével rendszer.

## <a name="regions-where-rhui-is-available"></a>Ha rendelkezésre áll-e RHUI régiók
RHUI minden régióban, ahol elérhetők az RHEL igény szerinti lemezképek érhető el. Jelenleg magában foglalja a hello felsorolt összes nyilvános régiókban [Azure állapota irányítópult](https://azure.microsoft.com/status/) Azure Amerikai Egyesült államokbeli kormányzati és a Németországi Azure-régiók lap. Az ár RHUI hozzáférés RHEL igény szerinti lemezképek kiosztott virtuális gépek tartalmazza. További területi/nemzeti felhőalapú rendelkezésre állási, azt bontsa ki az RHEL igény szerinti rendelkezésre állása, a jövőbeli hello lesznek frissítve.

> [!NOTE]
> Hozzáférés tooAzure által szolgáltatott RHUI korlátozott toohello virtuális gépeken belül [Microsoft Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

## <a name="get-updates-from-another-update-repository"></a>Egy másik tárházához frissítések beszerzése
Ha (az Azure által üzemeltetett RHUI) helyett egy másik frissítés tárházból tooget frissítésekre van szükség, először meg kell toounregister RHUI a példányok. Akkor szükséges, hogy toore-nyilvántartás hello kívánt frissítési infrastruktúrával (például a Red Hat műholdas vagy a Red Hat felhasználói portál CDN) őket. Akkor lesz kell megfelelő Red Hat előfizetések ezen szolgáltatások és a regisztrációs [Red Hat felhőalapú férhetnek hozzá az Azure-ban](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure).

toounregister RHUI és regisztrálja újra tooyour frissítési infrastruktúra kövesse az alábbi lépéseket:

1. Szerkessze a /etc/yum.repos.d/rh-cloud.repo, és módosítsa az összes `enabled=1` túl`enabled=0`. Példa:
   
   ```bash
   sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo
   ```
   
2. Módosítsuk /etc/yum/pluginconf.d/rhnplugin.conf és `enabled=0` túl`enabled=1`.
3. Hello kívánt infrastruktúrával, például a Red Hat Ügyfélportál majd regisztrálni. Hajtsa végre a Red Hat megoldás útmutatót az [hogyan tooregister és a rendszer toohello Red Hat Ügyfélportál előfizetés](https://access.redhat.com/solutions/253273).

> [!NOTE]
> Hozzáférés toohello Azure által üzemeltetett RHUI hello RHEL használatalapú fizetés (PAYG) kép ár tartalmazza. Regisztrációjának törlése az Azure által üzemeltetett RHUI hello PAYG RHEL virtuális gép nem átalakítása hello virtuális gép VM kerüljön-a-saját-licenc használata (BYOL) típus. Ha regisztrál hello azonos virtuális gép egy másik frissítések-forrással rendelkező, előfordulhat, hogy lehet díjfizetési dupla: először Azure RHEL szoftver díj és hello a Red Hat előfizetés(ek) még egyszer. 
> 
> Következetesen toouse Azure által üzemeltetett RHUI eltérő frissítési infrastruktúra javasolt létrehozása és telepítése a saját (BYOL-type) lemezképek leírtak [létrehozása és feltöltése Red Hat-alapú virtuális gépet az Azure-](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) cikk.
> 

## <a name="next-steps"></a>Következő lépések
Red Hat Enterprise Linux virtuális gép Azure piactér – használatalapú fizetés lemezképet, és használja ki az Azure által üzemeltetett RHUI toocreate lépjen túl[Azure piactér](https://azure.microsoft.com/marketplace/partners/redhat/). Fogja tudni toouse `yum update` RHEL betűtípusainak további beállítások elvégzése nélkül.

