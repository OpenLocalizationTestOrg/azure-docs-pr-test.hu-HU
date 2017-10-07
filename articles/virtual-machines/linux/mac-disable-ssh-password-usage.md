---
title: "a Linux virtuális gép SSHD konfigurálásával aaaDisable SSH jelszavuk |} Microsoft Docs"
description: "A Linux virtuális Gépet az Azure-on biztonságos jelszavas bejelentkezéseket az SSH letiltásával."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 46137640-a7d2-40e5-a1e9-9effef7eb190
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/26/2016
ms.author: v-livech
ms.openlocfilehash: fb67b2f5b8b3bf2ba214858940b04f2ea9013fb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Tiltsa le a Linux virtuális gép SSH jelszavuk SSHD konfigurálásával
Ez a cikk foglalkozik, hogyan toolock hello bejelentkezési biztonsági a Linux virtuális gép le.  Amint hello 22-es SSH-port már meg van nyitva toohello globális botok indítsa el a toologin próbált jelszavak találgatás által.  Milyen műveleteket végezzük el ebben a cikkben az SSH-n keresztül letiltsa a jelszavas bejelentkezéseket.  Hello képességét teljesen eltávolításával toouse jelszavak azt hello Linux virtuális gép védelme az ilyen típusú találgatásos támadás kényszerítése.  hello bővült rendkívüli azt konfigurálja Linux SSHD tooonly lehetővé SSH nyilvános és titkos kulcsok, keresztül bejelentkezések messze hello legbiztonságosabb módja toologin tooLinux.  hello lehetséges kombinációinak azt tooguess hello titkos kulcs jelentős, és ezért megnehezíti a még bothering tootry toobrute kényszerített SSH-kulcsok botok igényelnének.

## <a name="goals"></a>Célok
* SSHD toodisallow konfigurálása:
  * Jelszavas bejelentkezéseket
  * Legfelső szintű felhasználói bejelentkezés
  * Kérdés-válasz hitelesítés
* SSHD tooallow konfigurálása:
  * csak SSH kulcs bejelentkezések
* Indítsa újra a SSHD, miközben továbbra is jelentkezik
* Hello új SSHD tesztkonfiguráció

## <a name="introduction"></a>Bevezetés
[SSH definiálva](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD hello SSH kiszolgálót, amelyen a hello Linux virtuális gép.  Az SSH egy egy rendszerhéjból a munkaállomáson MacBook vagy Linux rendszerű futtató ügyfelet.  SSH egyben hello protokoll toosecure használja, és a munkaállomás és a Linux virtuális gép hello között hello kommunikáció titkosítására.

Ez a cikk a nagyon fontos tookeep Linux virtuális gép teljes hello nyitva bízná egy bejelentkezési tooyour esetén.  Ezen okból program megnyitja két terminálok és a Linux virtuális gép SSH toohello két őket.  A Microsoft hello első terminál toomake hello módosítások tooSSHDs konfigurációs fájlt használja, és hello SSHD szolgáltatás újraindításához.  Második terminál tootest hello azokat hello szolgáltatás újraindítása után módosítja használjuk.  Mivel azt választotta, hogy letiltja SSH jelszavak és hagyatkoznia szigorúan SSH-kulcsok, ha az SSH-kulcsok nem helyesek, és bezárja hello kapcsolat toohello VM, hello VM lesz véglegesen zárolva van, és nem lesz képes toologin tooit toobe törölni, majd újból-beli.

## <a name="prerequisites"></a>Előfeltételek
* [SSH-kulcsok létrehozása Linux és Mac rendszerben Linux virtuális gépek Azure-ban](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* Azure-fiók
  * [ingyenes próba-előfizetés](https://azure.microsoft.com/pricing/free-trial/)
  * [Azure Portal](http://portal.azure.com)
* Azure-on futó Linux virtuális gép
* SSH nyilvános és titkos kulcsból álló kulcspárt a`~/.ssh/`
* A nyilvános SSH-kulcs `~/.ssh/authorized_keys` a hello Linux virtuális gép
* A virtuális gép hello Sudo jogok
* Nyissa meg a 22-es portot

## <a name="quick-commands"></a>Gyors parancsok
*Fűszerezett Linux rendszergazdák számára, akik csak hello TLDR verzió Kezdje itt.  Összes többi felhasználója számára, hogy hello próbál részletes magyarázata és lépésein végighaladva átugorja ezt a szakaszt.*

```bash
sudo vim /etc/ssh/sshd_config
```

Hello konfigurációs fájl szerkesztése az alábbiak szerint:

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no

# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes

# Change PermitRootLogin toothis:
PermitRootLogin no

# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

Indítsa újra a hello SSHD szolgáltatást. A Debian-alapú disztribúciókkal:

```bash
sudo service ssh restart
```

A Red Hat-alapú disztribúciókkal:

```bash
sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Részletes lépésein végighaladva
Bejelentkezési toohello Linux virtuális gép terminál 1 (T1).  Bejelentkezési toohello Linux virtuális gép terminál 2 (T2).

A T2 fogjuk tooedit hello SSHD konfigurációs fájlt.  

```bash
sudo vim /etc/ssh/sshd_config
```

Itt rendszer csupán hello beállítások toodisable jelszavak szerkesztése és SSH-kulcs bejelentkezések engedélyezéséhez.  Nincsenek a kutatás és toomake Linux & SSH biztonságban kell módosítani kell a fájl számos olyan beállítást.

#### <a name="disable-password-logins"></a>Letiltsa a jelszavas bejelentkezéseket

```sh
# Change PasswordAuthentication toothis:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Nyilvános kulcsos hitelesítés engedélyezése

```sh
# Change PubkeyAuthentication toothis:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Tiltsa le a legfelső szintű bejelentkezést

```sh
# Change PermitRootLogin toothis:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Kérdés-válasz hitelesítés letiltása
```sh
# Change ChallengeResponseAuthentication toothis:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Indítsa újra a SSHD
Hello T1 rendszerhéjból győződjön meg arról, hogy továbbra is jelentkezett be.  Ez elengedhetetlen, így azt nem ártatlan tévedéssel a virtuális Gépet, ha az SSH-kulcsok helytelenek, mivel a jelszavak most le vannak tiltva.  Ha minden olyan beállítás helytelen a Linux virtuális gépén, használhatja a T1 toofix sshd_config, továbbra is vannak, naplózva lesznek, és SSH fog életben hello kapcsolat során hello SSHD szolgáltatás újraindítása.

A T2 futtatása:

##### <a name="on-hello-debian-family"></a>A hello Debian termékcsalád
```bash
sudo service ssh restart
```

##### <a name="on-hello-redhat-family"></a>A hello RedHat termékcsalád
```bash
sudo service sshd restart
```

A virtuális gép védelme akkor találgatásos jelszó-bejelentkezési kísérletek jelszavak most le vannak tiltva.  Csak SSH kulccsal rendelkező engedélyezett fogja tudni toologin gyorsabb és sokkal biztonságosabb.

