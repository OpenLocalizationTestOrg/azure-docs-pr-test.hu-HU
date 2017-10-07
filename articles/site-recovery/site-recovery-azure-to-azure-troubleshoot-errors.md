---
title: "aaaAzure Site Recovery Azure-Azure replikációval kapcsolatos problémák és hibák elhárítása |} Microsoft Docs"
description: "Vészhelyreállítás Azure virtuális gépek replikálása hibáinak és problémáinak elhárítása"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.openlocfilehash: bca957dd0f40e6b16e68913caf522f3431c55bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Azure-Azure virtuális gép replikálási problémák elhárítása

Ez a cikk ismerteti a hello az Azure Site Recovery replikálódik, és az Azure virtuális gépek helyreállítása egy régió tartozik tooanother régióban kapcsolatos gyakori hibák, és elmagyarázza, hogyan tootroubleshoot őket. Támogatott konfigurációk kapcsolatos további információkért lásd: hello [támogatási mátrixot az Azure virtuális gépek replikálása](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Azure-erőforrás kvótájának problémák (hibakód: 150097)
Az előfizetés engedélyezett toocreate Azure virtuális gépek toouse tervezi, a vész-helyreállítási régió hello cél régióban kell lennie. Emellett az előfizetéshez kell elegendő engedélyezett kvóta toocreate adott méretű virtuális gépeken. Alapértelmezés szerint a Site Recovery kivételezések hello hello megcélzott virtuális Gépre, a virtuális gép hello azonos mérete. Ha hello egyező méret nem érhető el, automatikusan nek hello legközelebbi lehetséges méretét. Ha nincsenek egyező méretét, amely támogatja a forrás Virtuálisgép-konfiguráció, ez a hibaüzenet jelenik meg:

**Hibakód:** | **Lehetséges okok** | **A javaslat**
--- | --- | ---
150097<br></br>**Üzenet**: hello VmName virtuális géphez nem lehetett engedélyezni a replikálást. | -Az előfizetés-azonosító nem lehet engedélyezve toocreate minden virtuális gépe hello régió célhelyet.</br></br>-Az előfizetés-azonosító nincs engedélyezve, vagy nem rendelkezik elegendő kvótája toocreate adott Virtuálisgép-méretek hello régió célhelyet.</br></br>-A megfelelő virtuális gép célméretet megfelelő hello forrás Virtuálisgép-hálózati szám (2) nem található hello előfizetési Azonosítóhoz tartozó hello régió célhelyet.| Ügyfél [Azure számlázási támogatás segítségét](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) tooenable virtuális gép létrehozásának hello szükséges Virtuálisgép-méretek hello célként megadott helyen az előfizetés. Az engedélyezése után újra hello művelet sikertelen volt.

### <a name="fix-hello-problem"></a>Hello probléma megoldása
Felveheti a kapcsolatot [Azure számlázási támogatás segítségét](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) tooenable az előfizetés toocreate hello célhelyet a szükséges méretű virtuális gépeken.

Ha hello célhelyet kapacitás korlátozást tartalmaz, tiltsa le a replikációt, majd engedélyezze tooa más helyre, ahol az előfizetés van-e elegendő kvótája toocreate szükséges hello méretű virtuális gépek.

## <a name="trusted-root-certificates-error-code-151066"></a>Megbízható legfelső szintű tanúsítványok (hibakód: 151066)

Ha minden hello legújabb megbízható legfelső szintű tanúsítvány nem található a virtuális gép hello, az "Engedélyezés replikációs" munkahelye sikertelen lehet. Hello tanúsítványok, hello hitelesítési és engedélyezési Site Recovery szolgáltatás nélkül hello VM hívás sikertelen. nem sikerült hello "replikáció engedélyezése" hely helyreállítási feladatra vonatkozó hello hibaüzenet jelenik meg:

**Hibakód:** | **Lehetséges ok** | **Javaslatok**
--- | --- | ---
151066<br></br>**Üzenet**: a Site Recovery konfigurálása nem sikerült. | hello szükséges megbízható legfelső szintű tanúsítványok az engedélyezési műveletekben használatos, és hitelesítési nem található ügynöktanúsítvány hello. | -Hello Windows operációs rendszert futtató virtuális gép, győződjön meg arról, hogy hello megbízható legfelső szintű tanúsítványok hello számítógépen találhatók. További információ: [konfigurálása a megbízható főtanúsítványok és nem engedélyezett tanúsítványok](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Hello Linux operációs rendszert futtató virtuális gép, kövesse a megbízható legfelső szintű tanúsítványok hello Linux operációs rendszer verziója forgalmazó által közzétett hello útmutatást.

### <a name="fix-hello-problem"></a>Hello probléma megoldása
**Windows**

Hello legújabb Windows frissítések telepítése a virtuális gép hello, hogy az összes hello megbízható legfelső szintű tanúsítványok hello számítógépen találhatók. Ha kapcsolat nélküli környezetben, kövesse a hello szabványos Windows frissítési folyamat a szervezet tooget hello tanúsítványokban. Ha hello szükséges tanúsítványok nincsenek rajta hello VM, hello hívások toohello Site Recovery szolgáltatás biztonsági okok miatt sikertelen.

Kövesse hello tipikus Windows update felügyeleti vagy a tanúsítvány frissítési folyamatát a szervezet tooget összes hello legújabb legfelső szintű tanúsítványok és a frissített hello visszavont listában a virtuális gépek hello.

tooverify, amely hello probléma megoldódott, toologin.microsoftonline.com nyissa meg a virtuális Gépet a böngészőből.

**Linux**

A Linux forgalmazó tooget hello legújabb megbízható legfelső szintű tanúsítványok és a hello legújabb tanúsítvány-visszavonási listát a virtuális gép hello hello útmutatást kövesse.

Mivel a SuSE Linux symlinks toomaintain listájának használ, kövesse az alábbi lépéseket:

1.  Jelentkezzen be egy legfelső szintű felhasználóként.

2.  Futtassa ezt a parancsot:

      ``# cd /etc/ssl/certs``

3.  toosee hello Symantec legfelső szintű Hitelesítésszolgáltatói tanúsítvány jelen-e, futtassa az ezt a parancsot:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Ha hello fájl nem található, futtassa az alábbi parancsokat:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  egy symlink rendelkező b204d74a.0 toocreate -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem, futtassa ezt a parancsot:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Ellenőrizze a toosee, ha ez a parancs a következő kimeneti hello rendelkezik. Ha nem, toocreate egy symlink rendelkezik:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Ha symlink 653b494a.0 nincs jelen, használja a parancs toocreate egy symlink:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Kimenő kapcsolatok a Site Recovery URL-címek vagy IP-címtartományok (hibakód: 151037 vagy 151072)

A Site Recovery replikációs toowork URL-címek vagy IP-címtartományok kimenő kapcsolat toospecific a virtuális gép hello szükség. Ha a virtuális gép mögött van egy tűzfal vagy a használt hálózati biztonsági csoport (NSG) szabályok toocontrol kimenő kapcsolatra, láthatja az alábbi hibaüzenetek valamelyike:

**Hibakódok** | **Lehetséges okok** | **Javaslatok**
--- | --- | ---
151037<br></br>**Üzenet**: nem sikerült tooregister virtuális gép az Azure Site Recovery szolgáltatással. | -Használata NSG toocontrol kimenő hozzáférést a virtuális gép hello és hello szükséges IP tartományok nem szerepel az engedélyezési listán a kimenő hozzáféréshez.</br></br>-Használata külső tűzfal eszközök és hello szükséges IP-címtartományokat/URL-címei nem szerepel az engedélyezési listán.</br>| -Ha tűzfal proxy toocontrol kimenő hálózati kapcsolat a virtuális gép hello használata esetén győződjön meg arról, hogy hello előfeltételként szükséges URL-címek vagy datacenter IP-címtartományok szerepel az engedélyezési listán. További információ: [proxy útmutatást tűzfal](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-Ha NSG szabályok toocontrol kimenő hálózati kapcsolat a virtuális gép hello használata esetén győződjön meg arról, hogy hello előfeltétel datacenter IP-címtartományok szerepel az engedélyezési listán. További információ: [hálózati biztonsági csoport útmutatást](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Üzenet**: a Site Recovery konfigurálása nem sikerült. | Kapcsolat a meghatározott tooSite helyreállítási Szolgáltatásvégpontok nem lehet. | -Ha tűzfal proxy toocontrol kimenő hálózati kapcsolat a virtuális gép hello használata esetén győződjön meg arról, hogy hello előfeltételként szükséges URL-címek vagy datacenter IP-címtartományok szerepel az engedélyezési listán. További információ: [proxy útmutatást tűzfal](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>-Ha NSG szabályok toocontrol kimenő hálózati kapcsolat a virtuális gép hello használata esetén győződjön meg arról, hogy hello előfeltétel datacenter IP-címtartományok szerepel az engedélyezési listán. További információ: [hálózati biztonsági csoport útmutatást](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-hello-problem"></a>Hello probléma megoldása
toowhitelist [hello szükséges URL-címek](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) vagy hello [IP-címtartományok szükséges](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges), kövesse hello hello [hálózati útmutató](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-hello-machine-error-code-150039"></a>Nem található hello számítógépen (hibakód: 150039)

Egy új lemezt csatolni toohello inicializálni kell a virtuális gép.

**Hibakód:** | **Lehetséges okok** | **Javaslatok**
--- | --- | ---
150039<br></br>**Üzenet**: az Azure data lemez (DiskName) (DiskURI) rendelkező logikai egységen (LUN) (LUNValue) nem volt csatlakoztatott tooa megfelelő lemez a virtuális Gépet, amely rendelkezik hello hello belül jelentett azonos logikai érték. | -Új adatlemezt csatolt toohello VM volt, de az nem lett inicializálva.</br></br>-hello adatlemez belül hello VM megfelelően nem jelent mely hello lemez volt csatolt toohello VM hello logikai érték.| Győződjön meg arról, hogy hello adatlemezek inicializálása, majd próbálja megismételni a műveletet hello.</br></br>Windows: [Attach és az új lemez inicializálása](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).</br></br>Linux: [lévő Linux új adatlemezt inicializálása](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

### <a name="fix-hello-problem"></a>Hello probléma megoldása
Győződjön meg arról, hogy hello adatlemezek inicializálása megtörtént, és próbálkozzon újra a művelettel hello:

- Windows: [Attach és az új lemez inicializálása](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).
- Linux: [lévő Linux új adatlemezt inicializálása](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

Ha hello a probléma továbbra is fennáll, forduljon a támogatási szolgálathoz.


## <a name="unable-toosee-hello-azure-vm-for-selection-in-enable-replication"></a>Nem lehet toosee hello Azure virtuális gép kiválasztható a "replikáció engedélyezése"

Előfordulhat, hogy nem látja az Azure virtuális gép kiválasztható a [engedélyezze a replikálást: 2. lépés](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines). A probléma oka lehet, toostale Site Recovery konfigurációs balra az hello Azure virtuális Gépen. hello elavult konfigurálása sikerült hagyható egy Azure virtuális gépen a következő esetekben hello:

- Engedélyezve van a hello Azure virtuális gép replikálását a Site Recovery segítségével, és anélkül, hogy explicit módon letiltja a replikáció a virtuális gép hello törli hello Site Recovery-tárolóban.
- Hello Azure virtuális gép replikálása engedélyezve a Site Recovery segítségével, és a Site Recovery-tároló hello tartalmazó nélkül explicit módon letiltja a replikáció a virtuális gép hello hello erőforráscsoport törölt.

### <a name="fix-hello-problem"></a>Hello probléma megoldása

Használhat [távolítsa el az elavult automatikus konfigurációs parancsfájl](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) , és távolítsa el a hello elavult Site Recovery konfigurálása a hello Azure virtuális Gépen. Megjelenik a virtuális gép hello [engedélyezze a replikálást: 2. lépés](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) hello elavult konfiguráció eltávolítása után.


## <a name="next-steps"></a>Következő lépések
[Azure-alapú virtuális gépek replikálása](site-recovery-replicate-azure-to-azure.md)
