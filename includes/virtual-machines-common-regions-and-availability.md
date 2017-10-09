# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Az Azure-beli virtuális gépek régiók szerinti csoportosítása és rendelkezésre állása
Azure hello világ több adatközpontot működik. Ezek adatközpontok vannak csoportosítva toogeographic régiókban, így rugalmasságot helyének kiválasztása toobuild az alkalmazások. Fontos toounderstand hogyan, és ahol a virtuális gépek (VM) működik, Azure, a beállítások toomaximize teljesítményt, rendelkezésre állás és redundancia együtt. Ez a cikk nyújt hello rendelkezésre állási áttekintését és Azure redundancia funkcióit.

## <a name="what-are-azure-regions"></a>Mik azok az Azure-régiók?
A megadott földrajzi régióban "USA nyugati régiója", "Észak-Európa" vagy "Délkelet-Ázsia" Azure-erőforrások hoz létre. Tekintse át hello [régiók és a helyek listáját](https://azure.microsoft.com/regions/). Minden egyes régión belül több adatközpontot létezik tooprovide redundancia és rendelkezésre állás. Ez a módszer rugalmasságot biztosít tervezési alkalmazások toocreate virtuális gépek legközelebbi tooyour felhasználók és toomeet bármely jogi, megfelelőségét, vagy adó szempontjából.

## <a name="special-azure-regions"></a>Különleges Azure-régiók
Azure néhány speciális régióban, hogy Kezdésként toouse létrehozásakor ki az alkalmazások megfelelőségi vagy jogi céllal rendelkezik. Ezek a különleges régiók a következők:

* **USA-beli államigazgatás – Virginia** és **USA-beli államigazgatás – Iowa**
  * Az Azure fizikailag és logikailag elszigetelt példánya az USA-beli államigazgatási szervek és partnereik számára, amelyet biztonsági szempontból átvilágított, USA-beli személyek kezelnek. Olyan további megfelelőségi tanúsítványokat is tartalmaz, mint a [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) vagy a [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Tudjon meg többet az [Azure Governmentről](https://azure.microsoft.com/features/gov/).
* **Kelet-Kína** és **Észak-Kína**
  * Ezek a területek a Microsoft és a 21Vianet, amelynek során a Microsoft nem közvetlenül karbantartása hello adatközpontok egyedi partnerség keresztül érhetők el. Tudjon meg többet a [Microsoft Azure kínai működéséről](http://www.windowsazure.cn/).
* **Közép-Németország** és **Északkelet-Németország**
  * Ezek a területek amellyel ügyféladatok Németországban ellenőrzés alá eső T-rendszerek, a német Telekom vállalati hello német adatok adatkezelő működött adatkezelő adatmodellt keresztül érhetők el.

## <a name="region-pairs"></a>Régiópárok
Minden Azure-régió, egy másik régióban belül hello párosított azonos földrajzi hely (például az Egyesült Államok, Európa vagy Ázsia). Ez a megközelítés lehetővé teszi hello replikációja, erőforrások, például Virtuálisgép-tároló, a földrajzi hely, amely természeti katasztrófa, polgári unrest, áramkimaradások vagy mindkét régió egyszerre érintő fizikai hálózati kimaradások hello valószínűségét csökkenteni kell. A régiópárok további előnyei még a következők:

* Egy szélesebb körű Azure leállás hello esetben egy régió van prioritása kívül minden pár toohelp csökkentése hello idő toorestore alkalmazásokhoz. 
* Tervezett Azure frissítések idő toominimize állásidő és alkalmazás kimaradás kockázatát toopaired régiók egy már megkezdődött.
* Adatok továbbra is tooreside hello belül azonos földrajzi hely, az adó és a törvény végrehajtás szempontjából joghatóság (kivéve a Dél-Brazília) pár.

Néhány példa a régiópárokra:

| Elsődleges | Másodlagos |
|:--- |:--- |
| USA nyugati régiója |USA keleti régiója |
| Észak-Európa |Nyugat-Európa |
| Délkelet-Ázsia |Kelet-Ázsia |

Megtekintheti a teljes hello [területi listája itt párokat](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

## <a name="feature-availability"></a>Szolgáltatások rendelkezésre állása
Néhány szolgáltatás és virtuálisgép-funkció, például meghatározott méretű virtuális gépek vagy adott tárolótípusok csak bizonyos régiókban érhetők el. Van még néhány globális Azure szolgáltatást, nincs szükség tooselect egy adott régióban, mint például [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md), vagy [Azure DNS](../articles/dns/dns-overview.md). tooassist meg az alkalmazás-környezet tervezésének, ellenőrizheti a hello [mindegyik régióhoz több Azure-szolgáltatások rendelkezésre állásának](https://azure.microsoft.com/regions/#services). 

## <a name="storage-availability"></a>Tárterület rendelkezésre állása
Azure-régiók és földrajzi ismertetése akkor fontos, ha rendelkezésre álló tár replikációs beállítások hello érdemes. Hello tárolási típus, attól függően, hogy különböző replikációs beállítások.

**Azure Managed Disks**
* Helyileg redundáns tárolás (LRS)
  * Az adatok a storage-fiók létrehozására használt hello régión belül háromszor replikálja.

**Tárfiókalapú lemezek**
* Helyileg redundáns tárolás (LRS)
  * Az adatok a storage-fiók létrehozására használt hello régión belül háromszor replikálja.
* Zónaredundáns tárolás (ZRS)
  * Az adatok háromszor keresztül replikálásra két toothree létesítményekben, egy vagy két régióban.
* Georedundáns tárolás (GRS)
  * A tooa másodlagos adatterületen, amely innen máshová hello elsődleges régió miles több száz replikálja.
* Írásvédett georedundáns tárolás (RA-GRS)
  * A tooa másodlagos adatterületen, replikálja a GRS, de az is, majd adja meg a csak olvasási hozzáféréssel toohello adatait hello másodlagos helyen.

hello következő tábla gyors áttekintést nyújt az hello különbségek hello tárolási replikációs típusok között:

| Replikációs stratégia | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| A rendszer több intézményben replikálja az adatokat. |Nem |Igen |Igen |Igen |
| Hello másodlagos helyről, és hello elsődleges helyről is lehet adatokat olvasni. |Nem |Nem |Nem |Igen |
| A külön csomópontokon fenntartott adatmásolatok száma. |3 |3 |6 |6 |

[Az Azure tárreplikációs lehetőségeiről itt](../articles/storage/common/storage-redundancy.md) olvashat bővebben. További információ a felügyelt lemezekről: [Azure Managed Disks – áttekintés](../articles/virtual-machines/windows/managed-disks-overview.md).

### <a name="storage-costs"></a>Tárolási költségek
A díjak hello tárolási típus, és a kiválasztott rendelkezésre állási függenek.

**Azure Managed Disks**
* Prémium szintű felügyelt lemez üzemelnek Solid-State meghajtók (SSD), és a standard szintű felügyelt lemez rendszeres forgó lemezek üzemelnek. Prémium és standard szintű felügyelt lemez is van szó, hello kiosztott kapacitást hello lemez alapján.

**Nem felügyelt lemezek**
* Prémium szintű storage Solid-State meghajtók (SSD-k) által támogatott, és a hello lemez kapacitását hello alapján terheli.
* Standard szintű tárolást rendszeres forgó lemezek alapját és díjfizetéssel hello használatban lévő kapacitás alapján, és szükséges tárterület rendelkezésre állását.
  * RA-GRS nincs hello sávszélesség replikálása adott adatok tooanother Azure-régió, egy további Georeplikációs adatátviteli díjat.

Lásd: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/) a díjszabásról hello különböző típusú tárolókat, és a rendelkezésre állási beállításait.

## <a name="availability-sets"></a>Rendelkezésre állási csoportok
Egy rendelkezésre állási csoportok pedig a virtuális gépek logikai csoportosítása, amely lehetővé teszi az Azure toounderstand az alkalmazást hogyan beépített tooprovide redundancia és rendelkezésre állás. Azt javasoljuk, hogy két vagy több virtuális gépeken belül az egy magas rendelkezésre állású alkalmazások és toomeet hello egy rendelkezésre állási készlet tooprovide jöttek létre [99,95 % Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Ha egy virtuális gép által használt [prémium szintű Azure Storage](../articles/storage/common/storage-premium-storage.md), hello Azure SLA vonatkozik, nem tervezett karbantartási események. 

Két további csoporthoz, amely a hardver meghibásodása elleni védelemhez és toosafely kell frissítések engedélyezése állnak rendelkezésre állási csoportok alkalmazott - fault tartományok (FDs) és a tartományok (UDs) frissítése.

![Fogalmi megrajzolása hello frissítési tartomány és a tartalék tartomány beállításait.](./media/virtual-machines-common-regions-and-availability/ud-fd-configuration.png)

További tudnivalók hogyan toomanage hello rendelkezésre állását [Linux virtuális gépek](../articles/virtual-machines/linux/manage-availability.md) vagy [Windows virtuális gépek](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Tartalék tartományok
Tartalék tartomány egy alapul szolgáló hardverben, amelyek egy közös áramforrásról logikai csoportját, és a hálózati kapcsoló, hasonló tooa állvány belül egy helyszíni adatközpontot. Virtuális gépek rendelkezésre állási csoportok belül létrehozott, hello Azure platformon a virtuális gépek automatikusan a tartalék tartományok elosztása. Ez a megközelítés korlátozza a lehetséges fizikai hardver hibák, a hálózati kimaradások vagy a power megszakításokat hello hatását.

### <a name="update-domains"></a>Frissítési tartományok
Egy frissítési tartományt egy olyan logikai csoport is változni karbantartási, vagy újra kell indítani a hello alapul szolgáló hardver ugyanannyi időt vesz igénybe. Virtuális gépek rendelkezésre állási csoportok belül létrehozott, hello Azure platformon automatikusan elosztja a virtuális gépek ezek tartományok frissítése. Ez a megközelítés biztosítja, hogy legalább egy példányt az alkalmazás mindig a futó hello Azure platformon megy keresztül rendszeres karbantartási marad. frissítési tartományok újraindítása folyamatban nem folytathatja egymás után tervezett karbantartás alatt, de csak egy frissítési tartományt hello sorrendjének egyszerre újraindítása után.

### <a name="managed-disk-fault-domains"></a>Lemez tartalék tartományok kezelése
Az [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md) használatával létrehozott virtuális gépek felügyelt rendelkezésre állási csoportok használata esetén felügyelt lemezes tartalék tartományokra vannak elosztva. Az összehangolás biztosítja, hogy az összes hello felügyelt lemezt csatlakoztatott tooa VM belül hello felügyelt lemezes azonos tartalék tartományban. Kizárólag felügyelt lemezeken futó virtuális gépek hozhatók létre felügyelt rendelkezésre állási csoportokban. felügyelt lemezes tartalék tartományainak számát hello függ a terület - két vagy három felügyelt lemezes tartalék tartományok régiónként.

![Felügyelt lemezek tartalék tartományai](./media/virtual-machines-common-manage-availability/md-fd.png)

> [!IMPORTANT]
> a felügyelt rendelkezésre állási csoportok tartalék tartományok száma hello függ a terület - két vagy három régiónként. a következő táblázat azt mutatja be hello szám régiónként hello

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

## <a name="next-steps"></a>Következő lépések
Most elindíthatja toouse a rendelkezésre állás, és redundanciát szolgáltatásainak toobuild az Azure környezetben. Javasoljuk, hogy tájékozódjon [az Azure rendelkezésre állásával kapcsolatos ajánlott eljárásokról](../articles/best-practices-availability-checklist.md).

