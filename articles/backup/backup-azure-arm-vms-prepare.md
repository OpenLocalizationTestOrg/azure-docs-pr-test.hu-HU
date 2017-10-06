---
title: "Az Azure Backup: Készítse elő a virtuális gépek tooback |} Microsoft Docs"
description: "Győződjön meg arról, hogy a környezet elő kell készíteni az Azure virtuális gépek biztonsági mentését."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "biztonsági mentések; biztonsági mentése;"
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 5c3a41b5d3bd56e62ca5f207442867913aa99816
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-environment-tooback-up-resource-manager-deployed-virtual-machines"></a>A virtuális gépek erőforrás-kezelő telepített környezet tooback előkészítése
> [!div class="op_single_selector"]
> * [Erőforrás-kezelő modell](backup-azure-arm-vms-prepare.md)
> * [Klasszikus modell](backup-azure-vms-prepare.md)
>
>

Ez a cikk a Resource Manager telepített virtuális gépek (VM) a környezet tooback előkészítése hello lépéseit ismerteti. hello eljárások használata hello Azure-portálon megjelenő hello lépéseket.  

hello Azure Backup szolgáltatás kétféle tárolók (tárolók és a biztonsági mentését recovery services-tárolók) a virtuális gépek védelmére. A mentési tároló hello klasszikus telepítési modell használatával telepített virtuális gépek védelmére. A recovery services-tároló védi **mindkét klasszikus telepített és erőforrás-kezelő telepített virtuális gépek**. A Recovery Services tároló tooprotect egy erőforrás-kezelő telepített virtuális Gépet kell használnia.

> [!NOTE]
> Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Lásd: [készítse elő a környezetet tooback Azure virtuális gépek](backup-azure-vms-prepare.md) talál részletes információt használata a klasszikus telepítési modell virtuális gépek.
>
>

Mielőtt védeni, vagy készítsen biztonsági másolatot egy erőforrás-kezelő telepített virtuális gép (VM), ellenőrizze, az Előfeltételek létezik:

* A recovery services-tároló létrehozása (vagy egy meglévő recovery services-tároló azonosítása) *hello a és a virtuális gép ugyanazon a helyen*.
* Válassza ki a forgatókönyvet, hello biztonsági mentési házirend, és elemek tooprotect meghatározását.
* Ellenőrizze a virtuális gép hello Virtuálisgép-ügynök telepítését.
* Ellenőrizze a hálózati kapcsolatot
* Linux virtuális gépekhez, abban az esetben, ha azt szeretné, hogy toocustomize a biztonsági környezetet az alkalmazás konzisztens biztonsági másolatok kövesse hello [lépéseket tooconfigure pillanatkép előtti és utáni pillanatkép-parancsfájlok](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Ha ismeri ezeket a feltételeket még vannak jelen a környezetében, majd a folytatáshoz toohello [készítsen biztonsági másolatot a virtuális gépek cikk](backup-azure-vms.md). Ha tooset kell fel, vagy ellenőrizze, az Előfeltételek bármelyike Ez a cikk végigvezeti Önt a hello lépéseket tooprepare, hogy az megfelel.

##<a name="supported-operating-system-for-backup"></a>Támogatott operációs rendszer biztonsági mentése
 * **Linux**: Az Azure Backup az [Azure által támogatott disztribúciókat](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) támogatja, a Core OS Linux kivételével. _Más kerüljön-a-saját-Linux terjesztéseket is előfordulhat, hogy működni, amíg hello Virtuálisgép-ügynök hello virtuális gépen érhető el, a Python létezik támogatása. Azonban azt hitelesíti ezeket terjesztéseket, a biztonsági mentéshez._
 * **Windows Server**: A Windows Server 2008 R2-nél régebbi verziók nem támogatottak.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Ha a biztonsági mentése és visszaállítása egy virtuális gép korlátozásai
Mielőtt a környezet előkészítése, tartsa szem előtt hello korlátozások.

* Több mint 16 adatlemezekkel rendelkező virtuális gépek biztonsági mentését nem támogatott.
* Virtuális gépek biztonsági mentését adatokkal 1023GB-nál nagyobb mérete nem támogatott.
* A fenntartott IP-cím és a nem definiált végpontot a virtuális gépek biztonsági mentését nem támogatott.
* Csak BEK használatával titkosított virtuális gépek biztonsági mentése nem támogatott. Linux virtuális gépek LUKS titkosítással titkosított biztonsági mentése nem támogatott.
* Kimenő fájlkiszolgáló konfigurációs méretű virtuális gépek biztonsági mentése nem ajánlott.
* Biztonsági másolat adataiban nem tartalmazza a csatlakoztatott hálózati meghajtó csatlakoztatva tooVM.
* Egy meglévő virtuális gép cseréje a visszaállítás során nem támogatott. Ha úgy próbálja toorestore hello VM hello virtuális gép létezik, hello visszaállítási művelet sikertelen.
* Kereszt-régió biztonsági mentése és visszaállítása nem támogatottak.
* Készíthet biztonsági másolatot az összes nyilvános régióiba Azure virtuális gépek (lásd: hello [ellenőrzőlista](https://azure.microsoft.com/regions/#services) a támogatott régiók). Ha a keresett hello régióban jelenleg nem támogatott, már nem jelenik hello legördülő lista tároló létrehozása során.
* A tartományvezérlők visszaállítását (DC) virtuális Gépet, amely része egy multi-tartományvezérlő-konfiguráció támogatott csak a PowerShell segítségével. Tudjon meg többet az [multi-DC tartományvezérlő visszaállítása](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Virtuális gépeken, amelyek a következő speciális hálózati konfigurációk hello visszaállítása csak a PowerShell használatával támogatott. A hello hello visszaállítási munkafolyamat használatával létrehozott virtuális gépek felhasználói felület nem lesz a hálózati konfigurációt hello visszaállítási művelet befejeződése után. több, lásd: toolearn [visszaállítását virtuális gépek speciális hálózati konfigurációkkal](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuális gépek a terheléselosztó-konfigurációja (belső és külső)
  * Virtuális gépek több foglalt IP-címmel
  * Virtuális gépek több hálózati adapterrel

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Létrehoz egy Recovery Services-tárolót egy virtuális géphez.
A recovery services-tároló olyan entitás, amely hello biztonsági másolatok és az adott idő alatt létrehozott helyreállítási pontokat tárolja. hello recovery services-tároló hello társított hello védett virtuális gépek biztonsági mentési házirendek is tartalmaz.

a helyreállítás toocreate tároló szolgáltatások:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Hello központ menüben kattintson a **Tallózás** hello az erőforrások listájához, írja be a **Recovery Services**. Írja be megkezdése előtt, hello listát szűrheti a megadott feltételeknek. Kattintson a **Recovery Services-tároló** elemre.

    ![Hello Tallózás gombra, és írja be a Recovery Services. Amikor megjelenik a hello Recovery Services tároló lehetőséget, kattintson rá a tooopen hello Recovery Services tároló panelen.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Recovery Services-tárolók hello listája jelenik meg.
3. A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.

    ![Recovery Services-tároló létrehozása – 5. lépés](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban. hello nevének kell toobe egyedi hello Azure-előfizetés esetében. Írjon be egy 2–50 karakter hosszúságú nevet. Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.
5. Kattintson a **előfizetés** toosee hello rendelkezésre álló előfizetések listáját. Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés. Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.
6. Kattintson a **erőforráscsoport** toosee elérhető erőforráscsoportok listáját hello, vagy kattintson a **új** toocreate egy új erőforráscsoportot. Átfogó információk az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
7. Kattintson a **hely** tooselect hello földrajzi régióban hello tároló. hello tároló **kell** hello lehet ugyanabban a régióban, amelyet az tooprotect hello virtuális gépként.

   > [!IMPORTANT]
   > Ha biztos benne, hogy hello helyét, amelyben a virtuális gép található, hello tároló létrehozása párbeszédpanel bezárásához, és válassza a toohello hello portál virtuális gépek listáját. Ha több régióba virtuális gépek, szüksége lesz a Recovery Services-tároló minden régióban toocreate. Mielőtt továbblép a következő helyre toohello hello első helyen hello tárolóban hozza létre. Nem kell toospecify tárfiókok toostore hello biztonsági mentési adatok – hello Recovery Services-tároló és a hello Azure Backup szolgáltatás automatikusan kezeli ezt.
   >
   >

8. Kattintson a **Create** (Létrehozás) gombra. A Recovery Services-tároló létrehozása toobe hello egy ideig is igénybe vehet. Hello állapot értesítések hello jobb felső területen hello portálon figyelése. A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg. Ha a tároló nem látható, kattintson a **frissítése** számára

    ![A Backup-tárolók listája](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Most, hogy létrehozta a tárolót, megtudhatja, hogyan tooset hello tárolóreplikálást.

## <a name="set-storage-replication"></a>Tárreplikáció beállítása
hello tárolási replikációs beállítás lehetővé teszi toochoose georedundáns tárolás és a helyileg redundáns tárolás között. Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Hello beállítás set toogeo redundáns tárolási hagyja, ha ez az elsődleges biztonsági mentés. Ha egy olcsóbb, rövidebb élettartamú megoldást szeretne, válassza a helyileg redundáns tárolást.

tooedit hello tárolási replikációs beállítását:

1. A hello **Recovery Services-tárolók** panelen válassza ki a tárolóban.
    A tároló gombra a beállítások panelen hello (*hello tároló nevére hello hello felső tartalmaz*) és hello tároló Részletek panel megnyitása.

    ![Válassza ki a tároló hello mentési tárolók listája](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. A hello **beállítások** panelen használata hello függőleges csúszkát tooscroll le toohello **kezelése** szakasz. Kattintson a **biztonsági infrastruktúra** tooopen a panelt. A hello **általános** szakaszban kattintson **biztonsági mentési konfigurációhoz** tooopen a panelt. A hello **biztonsági mentési konfigurációhoz** panelen válassza ki a replikációs tárolómegoldást hello a tároló számára. Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik. Ha módosítja a hello replikációs tárolótípus, kattintson a **mentése**.

    ![A Backup-tárolók listája](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Ha az Azure-t használja az elsődleges biztonsági mentési tároló végpontjaként, folytassa a georedundáns tárolás használatát. Ha egy nem elsődleges biztonsági másolatok tárolásának végpontként Azure használ, majd válassza a helyileg redundáns tárolás. Tudjon meg többet az [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolásáról: hello [Azure Storage replikáció – áttekintés](../storage/common/storage-redundancy.md).
    Miután kiválasztotta a tároló számára hello tárolási lehetőség, készen áll a tooassociate hello VM hello tárolóban. toobegin hello társítás, érdemes felderítése, és regisztrálja az Azure virtuális gépek hello.

## <a name="select-a-backup-goal-set-policy-and-define-items-tooprotect"></a>Biztonsági mentési cél, házirend, és adja meg az elemek tooprotect
Mielőtt regisztrálná a virtuális gép egy tárolóban, futtassa a hello felderítési folyamat tooensure, hogy minden új virtuális gépek toohello előfizetés hozzáadott azonosítja. hello folyamat lekérdezések Azure hello hello az előfizetést, valamint további információkat a virtuális gépek listája például a felhőalapú szolgáltatás- és hello régió hello. Hello Azure-portálon, a forgatókönyv tooput hello recovery services-tároló be fog toowhat hivatkozik. A házirend olyan hello milyen gyakran és mikor készít-e a helyreállítási pontok ütemezését. Házirend hello megőrzési időtartam hello helyreállítási pontok esetében is.

1. Ha már rendelkezik nyissa meg a Recovery Services-tároló, a folytatáshoz toostep 2. Ha nem rendelkezik nyissa meg a Recovery Services-tároló, majd nyissa meg a hello [Azure-portálon](https://portal.azure.com/) hello központ menüben kattintson **további szolgáltatások**.

   * Írja be az erőforrások listájához hello, **Recovery Services**.
   * Írja be megkezdése előtt, hello listát szűrheti a megadott feltételeknek. Amikor meglátja a **Recovery Services-tárolót**, kattintson rá.

     ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     Recovery Services-tárolók hello listája jelenik meg. Ha az előfizetésében nincsenek tárolók, ez a lista üres lesz.

    ![Helyreállítási szolgáltatások hello ábrázolása tárolók listája](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * A Recovery Services-tárolók hello listáról válassza ki a tároló tooopen az irányítópulton.

     hello-beállítások panel és hello tároló kiválasztott hello tárolóban, megnyílik az irányítópulton.

     ![Tároló panelének megnyitása](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Kattintson hello tároló irányítópult menü **biztonsági mentés** tooopen hello biztonsági mentés panelen.

    ![Biztonsági mentés panel megnyitása](./media/backup-azure-arm-vms-prepare/backup-button.png)

    hello biztonsági mentési és a biztonsági mentési cél panel nyílik meg.

    ![Forgatókönyv panel megnyitása](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. Hello biztonsági mentési cél paneljén állítsa be **a számítási feladatok futtató** tooAzure és **miről szeretne toobackup** tooVirtual gépre, majd kattintson **OK**.

    Ezzel regisztrálja hello Virtuálisgép-bővítmény hello tárolóban. hello panel bezárása után a biztonsági másolat cél és hello **biztonsági mentési házirend** panel nyílik meg.

    ![Forgatókönyv panel megnyitása](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Hello biztonsági szabályzat paneljén válassza hello tooapply toohello tároló kívánt biztonsági mentési házirendet.

    ![Biztonsági mentési házirend kiválasztása](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    hello alapértelmezett házirend hello adatait hello legördülő menüjében találhatók. Ha egy új házirendet toocreate, jelölje be **hozzon létre új** hello legördülő menüből. A biztonsági mentési házirendek meghatározását segítő utasításokat itt találja: [Biztonsági mentési házirend meghatározása](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Kattintson a **OK** tooassociate hello biztonsági mentési házirend hello tárolóban.

    Biztonsági szabályzat panel bezárása és hello hello **válassza ki a virtuális gépek** panel nyílik meg.
5. A hello **válassza ki a virtuális gépek** panelen válassza ki a hello virtuális gépek tooassociate hello a megadott házirend, és kattintson a **OK**.

    ![Számítási feladat kiválasztása](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    hello kijelölt virtuális gép van hitelesítve. Ha nem látja, hogy a toosee várt hello virtuális gépek, ellenőrizze, hogy léteznek-e a hello azonos Azure-beli helyhez hello Recovery Services tároló, és még nem védett, egy másik tárolóban. Recovery Services-tároló hello hello helye hello tároló irányítópulton látható.

6. Most, hogy a beállított hello tároló összes beállítást, a biztonsági mentés panel hello kattintson **biztonsági mentés engedélyezése**. Ez telepíti a hello házirend toohello tárolóban és hello virtuális gépeket. Ez nem hoz létre hello első helyreállítási pont hello virtuális géphez.

    ![Biztonsági mentés engedélyezése](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Miután sikeresen engedélyezte a hello biztonsági mentés, a biztonsági mentési házirend végrehajtja a ütemezés szerint. Ha most szeretné toogenerate egy igény szerinti biztonsági mentési feladat tooback hello virtuális gépek, lásd: [Triggering hello biztonságimásolat-készítő feladat](./backup-azure-arm-vms.md#triggering-the-backup-job).

Ha problémába ütközik hello virtuális gép rögzítése, lásd: hello hello Virtuálisgép-ügynök telepítése és a hálózati kapcsolat a következő információkat. Általában nem szükséges a következő információ, ha az Azure-ban létrehozott virtuális gépek védelmét hello. Azonban ha az Azure a virtuális gépeket telepített át, akkor ne feledje megfelelően telepítette hello Virtuálisgép-ügynök és, hogy a virtuális gép virtuális hálózati hello kommunikál.

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Hello Virtuálisgép-ügynök telepítése hello virtuális gépen
hello Azure Virtuálisgép-ügynök hello hello biztonsági mentés bővítmény toowork Azure virtuális gép telepítve kell lennie. Ha a virtuális Gépet az Azure katalógusában hello jött létre, majd hello Virtuálisgép-ügynök már telepítve hello virtuális gépet. Ezen információ hello olyan esetekben, ahol *nem* hello Azure katalógusában használatával egy virtuális Géphez létre – például végezte az áttelepítést egy virtuális Gépet egy olyan helyszíni adatközpontban. Ebben az esetben hello Virtuálisgép-ügynök toobe rendelés tooprotect hello virtuális gépen telepítve van szüksége. További tudnivalók: hello [Virtuálisgép-ügynök](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Ha problémába ütközik hello Azure virtuális gép biztonsági mentéséről, ellenőrizze, hogy a hello Azure Virtuálisgép-ügynök megfelelően telepítve van a hello virtuális gépen (lásd az alábbi táblázat hello). a következő táblázat hello VM ügynök a Windows hello és a Linux virtuális gépek további információkat tartalmaz.

| **Művelet** | **Windows** | **Linux** |
| --- | --- | --- |
| Hello Virtuálisgép-ügynök telepítése |Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Rendszergazdai jogosultságok toocomplete hello telepítési kell. |<li> Telepítse legújabb hello [Linux-ügynök](../virtual-machines/linux/agent-user-guide.md). Rendszergazdai jogosultságok toocomplete hello telepítési kell. Azt javasoljuk, hogy a terjesztési tárházból ügynök telepítése. A Microsoft **nem javasoljuk** közvetlenül a githubból telepítése Linux Virtuálisgép-ügynök.  |
| Hello Virtuálisgép-ügynök frissítése |Frissítési hello Virtuálisgép-ügynök újratelepítését hello egyszerűen [Virtuálisgép-ügynök bináris](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Győződjön meg arról, hogy fut-e biztonsági mentési művelet, hello Virtuálisgép-ügynök frissítése közben. |Hello utasításokat kövesse a megjelenő [Linux Virtuálisgép-ügynök frissítése hello](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Azt javasoljuk, hogy a terjesztési tárházból ügynök frissítése. A Microsoft **nem javasoljuk** közvetlenül a githubból frissítése Linux Virtuálisgép-ügynök.<br>Győződjön meg arról, hogy fut-e biztonsági mentési művelet során hello Virtuálisgép-ügynök frissítése folyamatban van. |
| Hello Virtuálisgép-ügynök telepítésének ellenőrzése |<li>Keresse meg a toohello *C:\WindowsAzure\Packages* hello Azure virtuális gép mappájában. <li>Keresse meg hello WaAppAgent.exe fájl található.<li> Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd válassza ki a hello **részletek** külön-külön hello termékverzió mező lehet 2.6.1198.718 vagy újabb. |N/A |

### <a name="backup-extension"></a>Backup bővítmény
Virtuálisgép-ügynök telepítve van a hello virtuális gépen, hello Azure Backup szolgáltatás hello egyszer hello tartalék mellék toohello Virtuálisgép-ügynök telepítése. hello Azure Backup szolgáltatás zökkenőmentesen frissíti, és javításokkal hello tartalék mellék.

tartalék mellék hello hello biztonsági másolat szolgáltatás telepítve van, hello virtuális gép fut-e. A virtuális gép hello legnagyobb esély arra az alkalmazáskonzisztens helyreállítási pontot biztosít. Azonban hello Azure Backup szolgáltatás továbbra is fel hello VM tooback még akkor is, ha ki van kapcsolva, és hello bővítmény telepítése sikertelen volt. Ennek neve offline virtuális gép. Ebben az esetben hello helyreállítási pont lesz *összeomlás-konzisztens*.

## <a name="network-connectivity"></a>Hálózati kapcsolat
A sorrend toomanage hello VM pillanatképek hello tartalék mellék kell kapcsolatot toohello Azure nyilvános IP-címeket. Hello jobb Internet kapcsolat nélkül hello virtuális gép HTTP kérelem túllépi az időkorlátot és hello biztonsági mentés sikertelen lesz. Ha a központi telepítés rendelkezik korlátozza a hozzáférést (a hálózati biztonsági csoport (NSG), például), majd válassza ki biztosítani azt egy tiszta elérési utat a biztonsági mentési forgalom ezen beállítások valamelyikét:

* [Engedélyezési lista hello Azure datacenter IP-címtartományok](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -hello cikk útmutatásért tekintse meg a hogyan toowhitelist hello IP-címek.
* A forgalom útválasztási HTTP-proxy kiszolgáló telepítése.

Amikor arról dönt, hogy melyik lehetőség toouse, hello kompromisszumot kezelhetőség, a tanúsítványhasználat pontos szabályzása és költsége közötti vannak.

| Beállítás | Előnyei | Hátrányok |
| --- | --- | --- |
| Engedélyezett IP-címtartományok |Nincsenek további költségek.<br><br>A hozzáférés egy NSG, használja a hello <i>Set-AzureNetworkSecurityRule</i> parancsmag. |Összetett toomanage, érintett hello IP-címtartományok módosítása adott idő alatt.<br><br>Azure, és nem csak a tárolási toohello teljes hozzáférést biztosít. |
| HTTP-proxy |Részletes szabályozását a hello proxy hello tároló URL-címek engedélyezett.<br>Egyetlen pont, Internet-hozzáférés tooVMs.<br>Nem tulajdonos tooAzure IP-címet érintő módosításait. |További költségekkel egy virtuális Gépet futtató hello proxy szoftverrel. |

### <a name="whitelist-hello-azure-datacenter-ip-ranges"></a>Engedélyezési lista hello Azure datacenter IP-címtartományok
toowhitelist hello Azure datacenter IP-címtartományai, tekintse meg a hello [Azure-webhelyen](http://www.microsoft.com/en-us/download/details.aspx?id=41653) hello IP-címtartományok és utasításokat.

### <a name="using-an-http-proxy-for-vm-backups"></a>A virtuális gép biztonsági mentésekhez egy HTTP-proxy használatával
Virtuális gépek biztonsági mentéséről, hello tartalék mellék a virtuális gép hello küld hello pillanatkép felügyeleti parancsok tooAzure HTTPS API segítségével. Hello tartalék mellék forgalom hello HTTP-proxyn keresztül történő továbbításához, mert hello csak összetevő hozzáférés toohello konfigurált nyilvános internethez.

> [!NOTE]
> Nincs Nincs javaslat hello proxy szoftverek kell használni. Győződjön meg arról, hogy válasszon-e a proxy, amely kompatibilis a hello alábbi konfigurációs lépéseket.
>
>

az alábbi példa képen hello szükséges toouse HTTP proxy hello három konfigurációs lépéseit mutatja be:

* Alkalmazás VM útvonalak kapcsolódik az összes HTTP-forgalom a nyilvános interneten keresztül Proxy VM hello.
* Proxy VM lehetővé teszi bejövő forgalmat a virtuális gépekről érkező hello virtuális hálózat.
* Hálózati biztonsági csoport (NSG) nevű Elégtelen-zárolási hello kell egy biztonsági szabály engedélyezése kimenő internetforgalom Proxy virtuális gépről.

![NSG a HTTP-proxy telepítési diagram](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

toouse HTTP proxy toocommunicating toohello nyilvános Internet, kövesse az alábbi lépéseket:

#### <a name="step-1-configure-outgoing-network-connections"></a>1. lépés A kimenő hálózati kapcsolatok konfigurálása
###### <a name="for-windows-machines"></a>A Windows-alapú gépek
Ez a helyi rendszerfiókhoz proxykiszolgálót állítják be.

1. Töltse le [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Futtassa a következő parancs rendszergazda jogú parancssorból

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Megnyílik az internet explorer ablakot.
3. Nyissa meg tooTools Internet-beállítások -> -> kapcsolatok LAN-beállítások ->.
4. Ellenőrizze a proxybeállítások helyességét a rendszerfiók. Állítsa be a Proxy IP-cím és port.
5. Zárja be az Internet Explorert.

Beállításához nyújt útmutatást a gépre kiterjedő proxy konfigurációját, és egyetlen kimenő HTTP/HTTPS-forgalmat fog történni.

Ha a telepítő egy proxykiszolgáló a jelenlegi felhasználói fiók (nem a helyi rendszerfiók), használja hello parancsfájl tooapply következő őket tooSYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Ha "(407) Proxyhitelesítés szükséges" proxy server naplóban, ellenőrizze a hitelesítési helyesen beállítva.
>
>

###### <a name="for-linux-machines"></a>Linux-gépekhez
Adja hozzá a következő sor toohello hello ```/etc/environment``` fájlt:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Adja hozzá a következő sorokat toohello hello ```/etc/waagent.conf``` fájlt:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-hello-proxy-server"></a>2. lépés Bejövő kapcsolatok engedélyezése hello proxykiszolgáló:
1. Nyissa meg a Windows tűzfal hello proxy kiszolgálón. hello legegyszerűbb módja tooaccess hello tűzfal toosearch a fokozott biztonságú Windows tűzfal.

    ![Nyissa meg a hello tűzfal](./media/backup-azure-vms-prepare/firewall-01.png)
2. Hello Windows tűzfal párbeszédpanelen kattintson a jobb gombbal **bejövő szabályok** kattintson **új szabály létrehozása...** .

    ![Új szabály létrehozása](./media/backup-azure-vms-prepare/firewall-02.png)
3. A hello **új bejövő szabály varázsló**, válassza ki a hello **egyéni** hello beállítása **szabály típusa** kattintson **következő**.
4. A hello lap tooselect hello **Program**, válassza a **minden program** kattintson **következő**.
5. A hello **protokoll és portok** lapon adja meg a következő információ hello, majd kattintson **következő**:

    ![Új szabály létrehozása](./media/backup-azure-vms-prepare/firewall-03.png)

   * a *protokolltípus* válasszon *TCP*
   * a *helyi port* válasszon *adott*, az alábbi hello mezőben adja meg a hello ```<Proxy Port>``` be van állítva.
   * a *távoli port* válasszon *minden port*

     Hello többi hello varázsló kattintson az összes hello módon toohello végfelhasználói, és nevezze el ez a szabály.

#### <a name="step-3-add-an-exception-rule-toohello-nsg"></a>3. lépés Egy kivétel szabály toohello NSG hozzáadása:
Az Azure PowerShell-parancssort írja be a hello a következő parancsot:

a következő parancs hello ad hozzá egy kivétel toohello NSG. Ez a kivétel lehetővé teszi, hogy a TCP-forgalom 10.0.0.5 a bármely portról tooany internetcím a 80-as (HTTP) vagy a 443-as (HTTPS) porton. Ha szüksége van egy adott portot a hello nyilvános Internet kell arról, hogy a port toohello tooadd ```-DestinationPortRange``` is.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*Ezeket a lépéseket ehhez a példához adott neveit és értékeit használja. Használjon hello neveit és értékeit a központi telepítés megadásakor, vagy Kivágás és részletek beillesztése a kódot.*

Most, hogy ismeri a hálózati kapcsolattal rendelkezik, tooback készen áll a virtuális gépet. Lásd: [erőforrás-kezelő telepített virtuális gépek biztonsági mentése](backup-azure-arm-vms.md).

## <a name="questions"></a>Kérdései vannak?
Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Következő lépések
Most, hogy előkészítette a környezetet az biztonsági mentése a virtuális Gépet, a következő logikai lépésre toocreate biztonsági másolat. a cikk tervezési hello biztosít a virtuális gépek biztonsági mentéséről további részletes információk.

* [Készítsen biztonsági másolatot a virtuális gépek](backup-azure-vms.md)
* [A virtuális gép biztonsági mentési infrastruktúra megtervezése](backup-azure-vms-introduction.md)
* [Virtuális gépek biztonsági mentéseinek kezelése](backup-azure-manage-vms.md)
