---
title: "az Azure virtuális gépekkel használt aaaAzure biztonsági funkciók |} Microsoft Docs"
description: " Hello Azure biztonsági alapszolgáltatások használható az Azure virtuális gépekkel áttekintése. Az Azure virtuális gépek biztosít anélkül, hogy toobuy biztosítják a virtualizálás rugalmasságát hello és hello fizikai hardveren futó virtuális gép hello karbantartása. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: terrylan
ms.openlocfilehash: 1a1b9f02bd82a2655f4e2e5d9f9ce7a6671f63fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-security-overview"></a>Az Azure virtuális gépek biztonsági áttekintése
Az Azure Virtual Machines rugalmas környezetet biztosít a legkülönbözőbb számítástechnikai megoldások üzembe helyezéséhez. Számítási feladatait a Windows-, Linux-, SQL Server-, Oracle-, IBM-, SAP- és Azure BizTalk Services-támogatás révén mérettől és programnyelvtől függetlenül, szinte bármely operációs rendszerben üzembe helyezheti.

Egy Azure virtuális gép által biztosított biztosítják a virtualizálás rugalmasságát hello anélkül, hogy toobuy és karbantartása hello fizikai hardver, amelyen fut a virtuális gép hello.  Hozza létre, és az, hogy az adatok-e a védett és biztonságos rendkívül biztonságos adatközpontjainkba hello megbízhatóságú alkalmazások központi telepítését.

Az Azure-hozhat létre biztonságos, megfelelő megoldás, amely:

* Virtuális gépek védelme a vírusokkal és kártevőkkel szemben
* Bizalmas adatok titkosítása
* Biztonságos hálózati adatforgalom
* Fenyegetések azonosítása és felderítése
* A megfelelőségi követelmények teljesítése

hello Ez a cikk célja tooprovide hello alapvető Azure biztonsági funkcióit, amelyek együtt a virtuális gépek áttekintése. Azt is biztosítanak, amelyek egyes szolgáltatások részleteit biztosítanak, így további hivatkozások tooarticles.  

hello core Azure virtuális gép biztonsági képességei toobe Ez a cikk tárgyalja:

* Kártevőirtó
* Hardveres biztonsági modul
* Virtuális gép lemeztitkosítás
* Virtuális gép biztonsági mentése
* Azure Site Recovery
* Virtuális hálózat
* Biztonsági házirendek kezelése és jelentéskészítés
* Megfelelőség

## <a name="antimalware"></a>Kártevőirtó
Az Azure-használhatja például a Microsoft, illetve Symantec, Trend Micro és Kaspersky tooprotect biztonsági szállítóktól származó kártevőirtó szoftver a virtuális gépek rosszindulatú fájlok, hirdetéseket és más fenyegetésekkel szemben. Lásd: hello cikkek toofind szakaszban olvashatók további információk a partneri megoldások.

Azure Cloud Services és a virtuális gépek Microsoft Antimalware a valós idejű védelem képessége, amelyik segít azonosításához és eltávolításához a vírusok, kémprogramok és más, kártevő szoftverek.  A Microsoft Antimalware konfigurálható riasztást küld, ha ismert kártevő vagy nemkívánatos szoftver kísérletek tooinstall magát, vagy az Azure rendszeren futtatásához biztosít.

A Microsoft Antimalware egy olyan single-ügynök megoldás az alkalmazások és a bérlői környezetek tervezett toorun hello háttérben emberi beavatkozás nélkül. Az alkalmazás-munkaterhelések, vagy alapvető biztonságos--alapértelmezés szerint a hello igények alapján, vagy egyéni konfiguráció – beleértve a kártevőirtó-figyelés speciális védelem telepítése.

Ha központi telepítése a Microsoft Antimalware engedélyezi, a következő alapvető funkcióit hello érhetők el:

* A valós idejű védelem - figyelők tevékenység a Felhőszolgáltatások és virtuális gépek toodetect, és letiltja a kártevő szoftverek végrehajtásakor.
* Ütemezett ellenőrzés - rendszeres időközönként a célzott beolvasási toodetect kártevő szoftverek, beleértve, aktívan futó programok.
* Kártevő szoftver eltávolításának - automatikusan hajt végre műveletet a kártevő, például törlése vagy a karanténba helyezés kártevő fájlok és a rosszindulatú beállításjegyzék-bejegyzések törlése.
* Aláírás frissítések - automatikusan telepíti hello legújabb védelmi aláírások (vírus-definíciók) tooensure védelem egy előre meghatározott gyakoriságáról naprakész.
* Frissíti a kártevőirtó motor – automatikusan a frissítéseket a Microsoft Antimalware motor hello.
* Kártevőirtó Platform frissítései – automatikusan a frissítéseket a Microsoft Antimalware platform hello.
* Aktív védelem - jelentések tooAzure telemetriai metaadatok fenyegetésről és gyanús erőforrások tooensure gyors választ, és lehetővé teszi, hogy a valós idejű szinkron aláírás kézbesítési hello Microsoft Active Protection rendszer (MAPS) keresztül.
* Minták reporting - biztosít, és minták jelentések toohello Microsoft Antimalware szolgáltatás toohelp pontosítsa hello szolgáltatást és a hibaelhárítási engedélyezése.
* Kizárások – lehetővé teszi, hogy az alkalmazás és szolgáltatás-rendszergazdák tooconfigure bizonyos fájlokat, a folyamatokat, és fokozza a tooexclude, őket a védelmi és beolvasó teljesítménye és más okok miatt.
* Eseménygyűjtés kártevőirtó - hello kártevőirtó szolgáltatás állapota, a gyanús tevékenységek és a szervizelés műveleteit hello operációs rendszer-eseménynaplójában rögzíti, és összegyűjti a hello ügyfél Azure Storage figyelembe.

További: további információk a kártevőirtó szoftver tooprotect toolearn a virtuális gépek, lásd:

* [A Microsoft Antimalware Azure Felhőszolgáltatások és virtuális gépek](azure-security-antimalware.md)
* [Kártevőirtó megoldások telepítése Azure-beli virtuális gépeken](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása a Windows virtuális gép szolgáltatásként](../virtual-machines/windows/classic/install-trend.md)
* [Hogyan tooinstall és a Symantec Endpoint Protection konfigurálása a Windows virtuális gép](../virtual-machines/windows/classic/install-symantec.md)
* [Biztonsági megoldásokat hello Azure piactéren](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardveres biztonsági modul
Titkosítás és hitelesítés védelmét azáltal, hogy a kulcs biztonsági javítja fejleszthető. Egyszerűbbé teheti az hello kezeléséért és a kritikus titkos kulcsok és a kulcsok Azure Key Vault tárolja őket. Key Vault biztosít hello beállítás toostore a kulcsokat, a hardveres biztonsági modulokkal (HSM) hitelesített tooFIPS 140-2 2. szintű szabványoknak. Az SQL Server titkosítási kulcsok biztonsági mentésre vagy [átlátható adattitkosítás](https://msdn.microsoft.com/library/bb934049.aspx) összes tárolhatja Key Vault kulcsok és titkos kulcsokat a alkalmazásokból. Engedélyekkel és hozzáférési toothese védett elemek használatával kezelhető [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

További információ:

* [Mi az Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md)
* [Az Azure Key Vault blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuális gép lemeztitkosítás
Az Azure Disk Encryption egy új képesség, amely lehetővé teszi a Windows és Linux Azure virtuális gépek lemezeit titkosításához. Az Azure Disk Encryption használ hello iparági szabvány [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) és a Windows hello szolgáltatás [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux tooprovide kötet titkosítási hello az operációs rendszer és adatlemezek hello szolgáltatást.

hello megoldás integrálva van az Azure Key Vault toohelp vezérlik, és hello lemez titkosítási kulcsok és titkos kódokat a kulcstartót az előfizetést, biztosítva, hogy a virtuális gépek lemezeit hello az összes adat titkosítva legyenek az Azure tárolás közben.

További információ:

* [Windows és Linux IaaS virtuális gépeket az Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [A Linux és a Windows virtuális gépek az Azure Disk Encryption](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [A virtuális gép titkosítása](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Virtuális gép biztonsági mentése
Az Azure Backup egy méretezhető megoldás, amellyel megvédheti, és tőkebefektetés nélkül, minimális működési költségek mellett megőrizheti alkalmazásadatait. Az alkalmazáshibák adatai sérülését okozhatják, az emberi hibák pedig az alkalmazások meghibásodásához vezethetnek. Az Azure Backup szolgáltatással a Windows és Linux rendszerű virtuális gépek védelmének.

További információ:

* [Mi az az Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Az Azure biztonsági mentési képzési terv](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Azure biztonsági mentési szolgáltatás – gyakori kérdések](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
A szervezete BCDR-stratégia egyik fontos része az tudja, hogyan tookeep vállalati munkaterheléseket és alkalmazásokat fel és futtathat tervezett nem tervezett leállások történik. Az Azure Site Recovery segítségével levezényelni a replikáció, feladatátvétel és helyreállítás munkaterhelések és alkalmazások, így azok elérhetők a másodlagos helyről, ha az elsődleges hely leáll.

Helyreállítás:

* **Egyszerűbbé teszi a BCDR-stratégia** – a Site Recovery segítségével könnyen toohandle replikáció, feladatátvétel és helyreállítás több üzleti számítási feladatok és alkalmazások egyetlen helyről. A Site Recovery koordinálja a replikációt és a feladatátvételt, de nem fér hozzá az alkalmazás adataihoz, és semmilyen arra vonatkozó információval nem rendelkezik.
* **Rugalmas replikáció biztosít** – a Site Recovery segítségével replikálhatja a Hyper-V virtuális gépek, a VMware virtuális gépek és a windowsos/Linuxos fizikai kiszolgálókon futó számítási feladatok.
* **Támogatja a feladatátvételi és helyreállítási** – a Site Recovery feladatátvételi teszteket biztosít a toosupport vészhelyreállítások részletezésének nem befolyásolja a termelési környezetben. Nulla adatvesztéssel járó tervezett feladatátvételeket is futtathat várt leállások esetére, illetve (a replikáció gyakoriságától függően) minimális adatvesztéssel járó nem tervezett feladatátvételeket a váratlan vészhelyzetek esetére. A feladatátvétel után a feladat-visszavétel tooyour elsődleges helyek is. A Site Recovery olyan helyreállítási terveket biztosít, amelyek parancsfájlokat és Azure Automation-munkafüzeteket tartalmazhatnak, így testre szabhatja a többrétegű alkalmazások feladatátvételét és helyreállítását.
* **Megszünteti a másodlagos adatközpontba** – tooa másodlagos helyszíni helyre vagy tooAzure replikálhatja. Az Azure célhelye vész-helyreállítási kiküszöböli hello költségek és a másodlagos hely karbantartásának. A replikált adatok Azure Storage tárolja.
* **Integrálható a meglévő BCDR-technológiákkal** – Site Recovery együttműködik az alkalmazás más BCDR-funkcióival. Például a Site Recovery tooprotect hello SQL Server háttéralkalmazását a vállalati számítási feladatok is használhatja. Ez magában foglalja a natív támogatást az SQL Server AlwaysOn rendelkezésre állási csoportok toomanage hello feladatátvétele.

További információ:

* [Mi az Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Hogyan működik az Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Mi az Azure Site Recovery által védett munkaterhelések?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuális hálózat
Virtuális gépek hálózati kapcsolattal kell. toosupport ezt a követelményt, Azure igényel a csatlakozó virtuális gépek toobe tooan Azure-beli virtuális hálózathoz. Egy Azure virtuális hálózatra egy logikai szerkezet hello fizikai Azure hálózati háló platformra épül. Minden logikai Azure Virtual Network el különítve a többi Azure virtuális hálózatok. Elkülönítés segítségével biztosítja, hogy a hálózati forgalmat a központi telepítés nem érhető el tooother Microsoft Azure-ügyfelek.

További információ:

* [Az Azure biztonsági áttekintése](security-network-overview.md)
* [Virtual Network áttekintése](../virtual-network/virtual-networks-overview.md)
* [Hálózati szolgáltatásait és partnerségei vállalati forgatókönyvek esetén](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Biztonsági házirendek kezelése és jelentéskészítés
Az Azure Security Center segít a megakadályozása, észlelésében és kezelésében toothreats, és biztosítja, hogy lássák, és szabályozhatják, az Azure-erőforrások biztonságának hello nőtt. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

Azure Security Center segítségével optimalizálása és figyelheti a virtuális gép biztonsági megoldást:

* Így a virtuális gép [biztonsági javaslatok](../security-center/security-center-recommendations.md) ilyen, a rendszer frissítések alkalmazása, hozzáférés-vezérlési listák végpontok konfigurálása, kártevőirtó engedélyezése, engedélyezze a hálózati biztonsági csoportok és lemeztitkosítás alkalmazni.
* A virtuális gépek hello állapotának figyelése

További információ:

* [A Security Center bemutatása tooAzure](../security-center/security-center-intro.md)
* [Az Azure Security Center gyakran ismételt kérdések](../security-center/security-center-faq.md)
* [Azure Security Center tervezéséhez és műveletek](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Megfelelőség
Az Azure virtuális gépek FISMA, a FedRAMP, a HIPAA, a PCI DSS szintjét 1 és az egyéb kulcs megfelelőségi alkalmazásokhoz minősítéssel. A hitelesítésszolgáltató megkönnyíti a saját Azure-alkalmazások toomeet megfelelőségi követelmények és az üzleti tooaddress hazai és nemzetközi szabályozási követelmények széles skáláját.

További információ:

* [A Microsoft biztonsági és adatkezelési központ: megfelelőségi](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Megbízható felhő: Microsoft Azure biztonsági, adatvédelmi és megfelelőségi](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
