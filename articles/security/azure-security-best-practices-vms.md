---
title: "aaaAzure virtuális gép ajánlott biztonsági eljárások |} Microsoft Docs"
description: "Ez a cikk számos biztonsági található, Azure virtuális gépekben használt bevált gyakorlatok toobe."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 5e757abe-16f6-41d5-b1be-e647100036d8
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: b03bcc75fde6d49897f9a7f6f15aec87456edd0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-vm-security"></a>Ajánlott eljárások az Azure virtuális gép biztonsági

A legtöbb szolgáltatott infrastruktúra (IaaS) szolgáltatás forgatókönyvek esetén [Azure virtuális gépek (VM)](https://docs.microsoft.com/en-us/azure/virtual-machines/) hello fő munkaterhelés szervezeteknek, amelyek használják a felhőalapú informatika vannak. A tény, különösen akkor egyértelmű [hibrid forgatókönyvek](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) ahol szervezetek kívánt tooslowly áttelepítése munkaterhelések toohello felhő. Ilyen esetben kövesse a hello [általános biztonsági szempontok az infrastruktúra-szolgáltatási](https://social.technet.microsoft.com/wiki/contents/articles/3808.security-considerations-for-infrastructure-as-a-service-iaas.aspx), és a biztonság bevált gyakorlatok tooall alkalmazása a virtuális gépek.

Ez a cikk ismerteti a különböző virtuális gép ajánlott biztonsági eljárások, minden egyes származik a felhasználók saját virtuális gépek közvetlen felhasználói.

hello gyakorlati tanácsok a véleményét az együttműködési alapulnak, és együttműködik a jelenlegi Azure platform olyan képességeit, és a szolgáltatáskészletek. Vélemények és technológiák idővel megváltozhat, mert tooupdate tervezzük ennek a következő cikket: rendszeresen tooreflect ezeket a módosításokat.

Az egyes célszerű hello a cikk azt ismerteti:

* Milyen hello ajánlott.
* Miért egy jó ötlet tooenable azt.
* Hogyan szerezhet tooenable azt.
* Mi történne, ha Ön nem tooenable azt.
* Lehetséges alternatívák toohello ajánlott eljárás.

hello cikk megvizsgálja-e a következő virtuális gép ajánlott biztonsági eljárások hello:

* Virtuális gép hitelesítési és hozzáférés-vezérlés
* Virtuális gép rendelkezésre állási és hálózati hozzáférés
* Titkosítási kényszerítésével inaktív adatok a virtuális gépek védelme
* A virtuális gép frissítések kezelése
* A virtuális gép biztonságot kezelése
* Virtuális gép teljesítményének figyelése

## <a name="vm-authentication-and-access-control"></a>Virtuális gép hitelesítési és hozzáférés-vezérlés

hello első lépése a virtuális gép védelme, tooensure, csak a felhasználók jogosultak a következők: új virtuális gépeinek képes tooset. Használhat [Azure Resource Manager házirendek](../azure-resource-manager/resource-manager-policy.md) tooestablish szabályai a szervezetnél található erőforrásokhoz létrehozzon testreszabott házirendeket, és ezek házirendek tooresources vonatkoznak, mint [erőforráscsoportok](../azure-resource-manager/resource-group-overview.md).

Természetesen tooa erőforráscsoporthoz tartozó virtuális gépek a házirendek jelentik. Bár javasolt ezt a módszert toomanaging virtuális gépeket, is vezérlő tooindividual VM házirendjei segítségével [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md).

Ha engedélyezi az erőforrás-kezelő házirendeket és Szerepalapú toocontrol VM hozzáférési, tökéletesítése általános virtuális gép biztonsági. Azt javasoljuk, hogy az azonos élettartama ciklus be hello hello vonják össze virtuális gépek ugyanabban az erőforráscsoportban. Erőforráscsoport-sablonok használatával telepítheti, figyeléséhez és költségek az erőforrások számlázási összesítő. tooenable felhasználók tooaccess, és állítsa be a virtuális gépek, használja a [legalacsonyabb jogosultsági megközelítés](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models). És jogosultságokkal toousers rendel, tervezze meg a következő beépített Azure szerepkörök toouse hello:

- [Virtuális gép közreműködő](../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor): is a virtuális gépek kezeléséhez, de nem hello a virtuális hálózati vagy tárolási fiók toowhich vannak csatlakoztatva.
- [Klasszikus virtuális gép közreműködő](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor): hello klasszikus üzembe helyezési modellel, de nem hello virtuális hálózati vagy tárolási fiók toowhich hello virtuális gépek csatlakoznak használatával létrehozott virtuális gépek kezelhetők.
- [Biztonságkezelő](../active-directory/role-based-access-built-in-roles.md#security-manager): kezelhetik biztonsági összetevők, a biztonsági házirendek és a virtuális gépek.
- [DevTest Labs felhasználói](../active-directory/role-based-access-built-in-roles.md#devtest-labs-user): mindent megtekinthetnek, és csatlakozzon, elindítható, indítsa újra, és állítsa le a virtuális gépek.

Ne ossza meg fiókkal és jelszóval egymástól a rendszergazdákat, és ne használja ugyanazt a jelszót több felhasználói fiókhoz vagy szolgáltatáshoz, különösen a közösségi oldalakon jelszavak vagy más nem felügyeleti tevékenységek. Ideális esetben használjon [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) sablonok tooset a virtuális gépeinek biztonságos helyen. Ezt a módszert használja, a központi telepítési beállítások megerősítése, és biztonsági beállításait az egész hello telepítési kényszerítéséhez.

A szervezeteknek, amelyek kényszeríti ki az adatok-hozzáférés-vezérlés RBAC képesség kihasználásával előfordulhat, hogy engedélyező felhasználóik szükségesnél több jogosultsággal. Nem megfelelő felhasználói hozzáférés toocertain adatok közvetlenül kedvezőtlenül befolyásolhatja a adatokat.

## <a name="vm-availability-and-network-access"></a>Virtuális gép rendelkezésre állási és hálózati hozzáférés

Ha a virtuális gép toohave magas rendelkezésre állást igénylő kritikus fontosságú alkalmazások fut, javasoljuk, hogy több virtuális gép használja. A jobb rendelkezésre állás érdekében hozzon létre legalább két virtuális gépek hello [rendelkezésre állási csoport](../virtual-machines/windows/tutorial-availability-sets.md).

[Az Azure terheléselosztó](../load-balancer/load-balancer-overview.md) is szükséges, hogy a virtuális gépek elosztott terhelésű toohello tartozik ugyanahhoz a rendelkezésre állási csoporthoz. Ha az hello Internet érhető el a virtuális gépeken, konfigurálnia kell egy [internetre terheléselosztó](../load-balancer/load-balancer-internet-overview.md).

Ha virtuális gépek kitett toohello Internet, fontos, hogy [hálózati biztonsági csoportokkal (NSG-k) a hálózati adatforgalom szabályozásához](../virtual-network/virtual-networks-nsg.md). Mivel NSG-ket lehet alkalmazott toosubnets, minimalizálhatja hello NSG-k száma alhálózat szerint csoportosítja az erőforrásokat, és majd alkalmazza az NSG-k toohello alhálózatok. hello célja toocreate hálózatelkülönítés, amely megfelelő a hello beállítása teheti réteget [hálózati biztonság](../best-practices-network-security.md) képességek az Azure-ban.

Is szolgáltatással hello just-in-time (JIT) VM-hozzáférés az Azure Security Center toocontrol rendelkező távelérési tooa speciális virtuális Gépet, és mennyi ideig.

A szervezeteknek, amelyek a hálózat-hozzáférési korlátozásokat tooInternet irányuló virtuális gépeken nem kényszerítenek kitett toosecurity kockázatok, például a távoli asztal protokoll (RDP) találgatásos támadással.

## <a name="protect-data-at-rest-in-your-vms-by-enforcing-encryption"></a>Inaktív adatok a virtuális gépek a titkosítási kényszerítésével védelme

[Inaktív adatok titkosítása](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) kötelező lépés adatvédelmi, megfelelőségi és az adatok közös joghatóság alá felé. [Az Azure Disk Encryption](../security/azure-security-disk-encryption.md) lehetővé teszi az informatikai rendszergazdák tooencrypt Windows és Linux rendszerű infrastruktúra-szolgáltatási virtuális lemezek. Lemeztitkosítás hello szabványos Windows BitLocker-szolgáltatás és a hello Linux dm-crypt szolgáltatás tooprovide kötettitkosítást hello az operációs rendszer és hello adatlemezek egyesíti.

Lemeztitkosítás alkalmazhat toohelp megakadályozhatja az adatok toomeet a szervezeti biztonsági és megfelelőségi követelményeknek. A szervezet kell érdemes lehet titkosítást használni toohelp kapcsolódó toounauthorized adatelérési kockázatok csökkentése. Javasoljuk továbbá, a meghajtók titkosítani bizalmas adatokat toothem írása előtt.

Tooencrypt meg arról, hogy a virtuális gép adatok kötetek tooprotect azokat a rest-az Azure-tárfiókot kell. Hello titkosítási kulcsok és titkos kulcs védelme használatával [Azure Key Vault](https://azure.microsoft.com/en-us/documentation/articles/key-vault-whatis/).

A szervezetek, amelyeket nem az adattitkosítás több kitett toodata-integritási probléma. Például jogosulatlan vagy rosszindulatú felhasználók lehet, hogy a sérült biztonságú fiók adatok ellopására vagy bekódolni ClearFormat a jogosulatlan hozzáférés toodata kapnak. Az ilyen veszélyek véve, és iparági szabályozások toocomply, mellett vállalatok igazolja, hogy azok vannak gyakorló gondossággal, és használja a megfelelő biztonsági vezérlők tooenhance az adatok biztonsági.

További információ az adatok titkosítása toolearn lásd [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket](azure-security-disk-encryption.md).


## <a name="manage-your-vm-updates"></a>A virtuális gép frissítések kezelése

Mivel Azure virtuális gépeken, például az összes a helyszíni virtuális gépek tervezett toobe felhasználó által felügyelt, Azure leküldéses nem a Windows-frissítések toothem. Áll, azonban javasolt tooleave hello automatikus Windows Update-beállítás engedélyezve van. Másik lehetőség is toodeploy [Windows Server Update Services (WSUS)](https://technet.microsoft.com/windowsserver/bb332157.aspx) vagy egy másik virtuális gép vagy a helyszíni egy másik megfelelő frissítés-felügyeleti termék. A WSUS és a Windows Update virtuális gépek naprakészen tartása. Azt javasoljuk, hogy a beolvasási termék tooverify, az infrastruktúra-szolgáltatási virtuális gépek toodate másolatot használja.

Azure által biztosított készlet lemezkép rendszeresen frissített tooinclude hello legutóbbi kerek, a Windows-frissítések. Van azonban nem garantálja, hogy hello lemezképek központi telepítéskor aktuális lesz. Lehet, hogy némi késés (legfeljebb néhány héttel) a következő nyilvános kiadásokban lehetséges. Ellenőrzése és az összes Windows-frissítések telepítésével hello első lépése minden üzembe helyezés kell lennie. Ez a mérték különösen fontos tooapply esetén, amelyeket Ön vagy a saját könyvtár a lemezképek központi telepítése. Hello Azure piactér részeként biztosított lemezképet alapértelmezés szerint automatikusan frissülnek.

A szervezeteknek, amelyek a szoftverfrissítés házirendek kényszerítésére nem ismert, korábban rögzített réseit több kitett toothreats. Ilyen fenyegetéseket, és iparági szabályozások toocomply kockázata mellett vállalatok igazolja, hogy azok vannak gyakorló gondossággal, és használja a megfelelő biztonsági vezérlők toohelp biztonsága hello a munkaterhelés hello felhőben található.

Szoftverfrissítés-ajánlott eljárások a hagyományos adatközpontok, fontos tooemphasize és Azure infrastruktúra-szolgáltatási sok a hasonlóság rendelkezik. Ezért azt javasoljuk, hogy az aktuális szoftver frissítés házirendek tooinclude virtuális gépek értékeli.

## <a name="manage-your-vm-security-posture"></a>A virtuális gép biztonságot kezelése

Számítógépes fenyegetések vannak fejlődnek, és a gazdag ellenőrzési képesség, amely gyorsan észleli a veszélyeket, megakadályozza a jogosulatlan hozzáférés tooyour erőforrások, aktiváltak riasztásokat, és vakriasztások számának csökkentése érdekében a virtuális gépek védelmére van szükség. ilyen egy munkaterhelés számára hello biztonságot hello VM, a frissítés felügyeleti toosecure hálózati hozzáférés minden biztonsági szempontból foglalja magában.

toomonitor hello biztonsági állapotát a [Windows](../security-center/security-center-virtual-machine.md) és [Linux virtuális gépek](../security-center/security-center-linux-virtual-machine.md), használjon [az Azure Security Center](../security-center/security-center-intro.md). Az Azure Security Centerben megakadályozhatja a virtuális gépek a következő képességeket hello kihasználásával:

* Az ajánlott konfiguráció szabályok az operációs rendszer biztonsági beállításainak alkalmazása
* Azonosíthatja, és töltse le a rendszer biztonsági és kritikus frissítések hiányoznak
* Végpont kártevőirtó védelmi javaslatok telepítése
* Ellenőrizze a lemez titkosítása
* Mérheti fel, és biztonsági rések javítása
* Fenyegetésészlelés

A Security Center aktívan képes figyelni a fenyegetések, és a potenciális fenyegetések ki vannak téve a **biztonsági riasztások**. Korrelált fenyegetések összesítése nevű egyetlen nézetben **biztonsági incidens**.

toounderstand hogyan Security Center segít azonosítani a potenciális fenyegetések, a virtuális gépek Azure-ban található a bemutató a következő videó hello:

<iframe src="https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Security-Center-in-Incident-Response/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

A szervezeteknek, amelyek a virtuális gépek erős biztonságot nem kényszerítenek jogosulatlan felhasználók toocircumvent létrehozott biztonsági vezérlők érzékelte a lehetséges kísérletek maradnak.

## <a name="monitor-vm-performance"></a>Virtuális gép teljesítményének figyelése

Erőforrás visszaélések problémát okozhat, ha a virtuális gép folyamat több erőforrást, mint azok kell. Teljesítménnyel kapcsolatos problémák, a virtuális gép sérti hello rendszerbiztonsági tagot a rendelkezésre állás tooservice megszűnésének is járhat. Ezért nincs imperatív toomonitor VM hozzáférés csak reaktív, amíg egy probléma jelentkezett, de is proaktív alapteljesítményének a normál működés során mért ellen.

Elemzésével [Azure diagnosztikai naplófájlok](https://azure.microsoft.com/en-us/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/), figyelheti a Virtuálisgép-erőforrások, és, mely veszélyeztetheti a teljesítmény és rendelkezésre állás lehetséges problémák azonosítása. hello Azure Diagnostics bővítmény figyelési és diagnosztika képességeket biztosít a Windows-alapú virtuális gépeken. Ezek a képességek engedélyezéséhez hello kiterjesztéssel együtt hello részeként [Azure Resource Manager sablon](../virtual-machines/windows/extensions-diagnostics-template.md).

Is [Azure figyelő](../monitoring-and-diagnostics/monitoring-overview-metrics.md) toogain lássák az erőforrás állapotát.

A szervezeteknek, amelyek nem a virtuális gép teljesítményének figyelése nem toodetermine, hogy bizonyos teljesítmény változására normál és rendellenes. Hello virtuális gép nem használ-e további erőforrások, mint a normál, ha egy ilyen anomáliadetektálási oka lehet egy potenciális támadási egy külső erőforrás vagy sérült biztonságú hello virtuális gép futó folyamat.
