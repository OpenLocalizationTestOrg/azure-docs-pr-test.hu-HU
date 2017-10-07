---
title: "aaaConfigure SSHD Azure Linux virtuális gépeken |} Microsoft Docs"
description: "Ajánlott biztonsági eljárások és toolockdown SSH tooAzure Linux virtuális gépek SSHD konfigurálja."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: v-livech
ms.openlocfilehash: c2361be7199a24b129c06acfc899dd32f6e1d6fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sshd-on-azure-linux-vms"></a>Az Azure Linux virtuális gépeken futó SSHD konfigurálása

Ez a cikk bemutatja, hogyan toolockdown hello jelszavak helyett a SSH-kulcsok használatával SSH-kiszolgáló Linux, tooprovide bevált gyakorlatok biztonsági, valamint a toospeed hello SSH bejelentkezési folyamatot.  toofurther zárolási fogjuk toodisable hello gyökér szintű felhasználó nem tud toologin SSHD hello felhasználók, amelyek számára engedélyezett egy jóváhagyott listájából keresztül toologin letiltása az SSH protokoll verzió: 1, a minimális kulcs bit beállítása és automatikus-kijelentkezik a tétlen felhasználók konfigurálása.  Ez a cikk hello követelményei: az Azure-fiók ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és [SSH nyilvános és titkos kulcs fájlok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Gyors parancsok

Konfigurálása `/etc/ssh/sshd_config` a hello a következő beállításokat:

### <a name="disable-password-logins"></a>Letiltsa a jelszavas bejelentkezéseket

```bash
PasswordAuthentication no
```

### <a name="disable-login-by-hello-root-user"></a>Tiltsa le a bejelentkezést hello gyökér szintű felhasználó által

```bash
PermitRootLogin no
```

### <a name="allowed-groups-list"></a>Engedélyezett csoportok listáját

```bash
AllowGroups wheel
```

### <a name="allowed-users-list"></a>Engedélyezett felhasználók listája

```bash
AllowUsers ahmet ralph
```

### <a name="disable-ssh-protocol-version-1"></a>Tiltsa le az SSH protokoll 1-es verziójával

```bash
Protocol 2
```

### <a name="minimum-key-bits"></a>Minimális kulcs bits

```bash
ServerKeyBits 2048
```

### <a name="disconnect-idle-users"></a>A tétlen felhasználók leválasztása

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

SSHD hello SSH kiszolgálót, amelyen a hello Linux virtuális gép.  Az SSH egy ügyfél, amely egy a MacBook, Linux munkaállomás a rendszerhéj vagy a Bash futó Windows.  SSH egyben hello protokoll toosecure használja, és a munkaállomás és a Linux virtuális gép is így SSH (virtuális magánhálózat) VPN hello között hello kommunikáció titkosítására.

Ez a cikk a nagyon fontos tookeep egy bejelentkezési tooyour Linux virtuális gép nyissa meg a teljes útmutató hello esetén.  Miután az SSH-kapcsolat létrejött, marad munkamenetekkel mindaddig, amíg hello ablak nincs lezárva.  Egy terminál jelentkezett be, hogy lehetővé teszi a módosításokat végzett toobe toohello SSHD szolgáltatás nélkül használhatatlanná tévő változást, ha éppen zárolva.  Ön ártatlan tévedéssel a Linux virtuális gép hibás SSHD konfigurálása, ha Azure kínál hello képességét tooreset egy hibás SSHD-konfiguráció hello [Azure VM hozzáférési bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ezért azt megnyitása két terminálok és a Linux virtuális gép SSH toohello mindkettő.  A Microsoft hello első terminál toomake hello módosítások tooSSHDs konfigurációs fájlt használja, és indítsa újra hello SSHD szolgáltatást.  Második terminál tootest hello azokat hello szolgáltatás újraindítása után módosítja használjuk.  Mivel azt választotta, hogy letiltja SSH jelszavak és hagyatkoznia szigorúan SSH-kulcsok, ha az SSH-kulcsok nem helyesek, és bezárja hello kapcsolat toohello VM, hello VM lesz véglegesen zárolva van, és nem lesz képes toologin tooit toobe törölni, majd újból-beli.

## <a name="disable-password-logins"></a>Letiltsa a jelszavas bejelentkezéseket

hello leggyorsabb módon toosecure Linux virtuális gép meg-e toodisable jelszavas bejelentkezéseket.  Ha engedélyezve vannak a jelszavas bejelentkezéseket, bejárási hello webes azonnal megkezdődik toobrute kísérlet botok becslés hello jelszó kényszerítése a Linux virtuális gép SSH.  Teljes letiltása a jelszavas bejelentkezéseket, lehetővé teszi, hogy hello SSH server tooignore összes jelszó bejelentkezési kísérletek.

```bash
PasswordAuthentication no
```

## <a name="disable-login-by-hello-root-user"></a>Tiltsa le a bejelentkezést hello gyökér szintű felhasználó által

Következő Linux ajánlott eljárások hello `root` felhasználónak kell soha nem kell bejelentkeznie SSH vagy használatával `sudo su`.  Gyökér szintű engedélyek igénylő összes parancs mindig fusson a hello keresztül `sudo` parancsot, amely a jövőbeli naplózás összes műveletet naplózza.  Letiltása hello `root` SSH-kapcsolaton keresztül van bejelentkezve a felhasználó egy biztonsági ajánlott eljárások lépés, amely csak a jogosult felhasználók végezhetnek tooSSH biztosítja.

```bash
PermitRootLogin no
```

## <a name="allowed-groups-list"></a>Engedélyezett csoportok listáját

SSH megakadályozhatják a felhasználók és csoportok számára engedélyezett, vagy le van tiltva az ssh-naplózás egy módszert biztosít.  Ez a funkció listák tooapprove használ, vagy a bejelentkezés megtagadása – meghatározott felhasználókhoz és csoportokhoz.  Hello kerék csoport toohello beállítás `AllowGroups` lista korlátozza a jóváhagyott bejelentkezések keresztül SSH toojust felhasználói fiókok hello kerék csoportba.

```bash
AllowGroups wheel
```

## <a name="allowed-users-list"></a>Engedélyezett felhasználók listája

Tooaccomplish hello azonos pontosabb úgy feladat, amely korlátozza, hogy SSH bejelentkezéshez toojust felhasználók `AllowGroups` van.  

```bash
AllowUsers ahmet ralph
```

## <a name="disable-ssh-protocol-version-1"></a>Tiltsa le az SSH protokoll 1-es verziójával

SSH protokoll verziója 1 nem biztonságos, és le kell tiltani.  SSH protokoll 2-es verziójú hello aktuális verzióra, amely egy biztonságos módon tooSSH tooyour server kínál.  1-es VERZIÓJÁVAL letiltása megtagadja a bármely SSH-ügyfél, amely tooestablish próbált kapcsolatot az 1-es verziójával hello SSH-kiszolgálót.  Csak SSH-2-es kapcsolatok toonegotiate hello SSH-kiszolgálót a kapcsolatot.

```bash
Protocol 2
```

## <a name="minimum-key-bits"></a>Minimális kulcs bits

Ajánlott biztonsági eljárásokat követve jelszó SSH bejelentkezés le vannak tiltva, és csak az SSH-kulcsok használata engedélyezett toobe használt tooauthenticate hello SSH-kiszolgálót.  E SSH-kulcsok különböző hosszúságú kulcsok mérése bitben használatával hozhatók létre.  Ajánlott eljárásokat, hogy a 2048 bit hosszúságú kulcsokban hello minimális elfogadható kulcs erősségét állapotok.  Kisebb, mint 2048 bites kulccsal elméletileg kell megszakadt.  A beállítás hello `ServerKeyBits` túl`2048` lehetővé teszi, hogy a kapcsolatokat, 2048 bites vagy annál nagyobb kulcsokkal, és lehetővé a kapcsolódást kisebb, mint 2048 bit.

```bash
ServerKeyBits 2048
```

## <a name="disconnect-idle-users"></a>A tétlen felhasználók leválasztása

SSH rendelkezik hello képességét toodisconnect rendelkező felhasználók, amelyek több mint a másodpercben megadott ideje maradtak tétlen kapcsolatok megnyitása.  A megnyitott munkamenetek tooonly hello Linux virtuális gép aktív korlátok hello elérhetővé tegyék ezen felhasználók tartása.

```bash
ClientAliveInterval 300
ClientAliveCountMax 0
```

## <a name="restart-sshd"></a>Indítsa újra a SSHD

tooenable hello beállításai a `/etc/ssh/sshd_config` indítsa újra a hello SSHD folyamattal, amely újraindítja a hello SSH-kiszolgálót.  hello terminálablakot toorestart hello SSH-kiszolgálót használ hello SSH munkamenetekkel elvesztése nélkül nyitva marad.  tootest hello új SSH kiszolgálóbeállítások használjon egy második terminálablakot vagy -lapon.  Egy külön terminál tootest hello SSH-kapcsolat használata lehetővé teszi az toogo vissza és ellenőrizze további módosításokat toohello `/etc/ssh/sshd_config` hello első terminálban zárolta SSHD használhatatlanná tévő változást nélkül.  

### <a name="on-redhat-centos-and-fedora"></a>Redhat, Centos és Fedora

```bash
service sshd restart
```

### <a name="on-debian--ubuntu"></a>A Debian és Ubuntu

```bash
service ssh restart
```

## <a name="reset-sshd-using-azure-reset-access"></a>Azure alaphelyzetbe állítása-hozzáférés SSHD alaphelyzetbe állítása

Ha a legfrissebb toohello SSHD konfigurációjának módosítása a zárolt hello Azure Virtuálisgép-access-bővítmény tooreset hello SSHD hátsó toohello eredeti konfiguráció is használhatja.

Cserélje le a neveit saját.

```azurecli
azure vm reset-access \
--resource-group myResourceGroup \
--name myVM \
--reset-ssh
```

## <a name="install-fail2ban"></a>Fail2ban telepítése

Erősen ajánlott tooinstall és a telepítési hello az nyílt forráskódú alkalmazás Fail2ban, mely blokkok ismétlődő kísérletek toologin tooyour Linux virtuális gép használata a találgatásos SSH-n keresztül.  Ismétlődő Fail2ban naplók kísérletek toologin sikertelen SSH-n keresztül, és ezután hoz létre a tűzfalszabályok tooblock hello IP-címet, amely hello kísérletek származó vannak.

* [Fail2ban kezdőlap](http://www.fail2ban.org/wiki/index.php/Main_Page)

## <a name="next-steps"></a>Következő lépések

Most, hogy beállította, és a Linux virtuális gépén hello SSH-kiszolgálót zárolva vannak további biztonsági ajánlott eljárások követésével.  

* [Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [A Linux virtuális gépet az Azure parancssori felület hello lemezzel titkosítása](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

* [Hozzáférés és biztonság az Azure Resource Manager sablonokban](dotnet-core-3-access-security.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
