Az Azure virtuális gépek (VM) egyes esetekben előfordulhat, hogy újraindítás OK, a rendelkező hello újraindítás műveletet kezdeményezett a bizonyító adatok nélkül. Ez a cikk hello műveletek és a virtuális gépek tooreboot okozhat, és milyen módon váratlan tooavoid indítsa újra a problémákat, és ezek a problémák hello hatásának csökkentéséhez betekintést nyújt eseményeket sorolja fel.

## <a name="configure-hello-vms-for-high-availability"></a>Hello VM a magas rendelkezésre állás konfigurálása
hello legjobb módja tooprotect Azure VM elleni futó alkalmazás újraindul, ráadásul leállításra a magas rendelkezésre állású virtuális gépek tooconfigure hello.

Ez a szint redundancia tooyour alkalmazás tooprovide, javasoljuk, hogy két vagy több virtuális gép rendelkezésre állási csoportba. Ez a konfiguráció biztosítja, hogy vagy a tervezett vagy nem tervezett karbantartási események esetén legalább egy virtuális gép elérhető, és megfelel-e hello 99,95 % [Azure garantált szolgáltatási szintje](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Rendelkezésre állási készletek kapcsolatos további információkért tekintse meg a következő cikkek hello:

- [Hello virtuális gépek rendelkezésre állásának kezelése](../articles/virtual-machines/windows/manage-availability.md)
- [A virtuális gépek rendelkezésre állásának konfigurálása](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Erőforrás-állapot adatai 
Az Azure Resource Health olyan szolgáltatás, amely elérhetővé teszi az egyes Azure-erőforrások állapotának hello és végrehajthatóként útmutatást kínál a problémák hibaelhárítása. Ha nem lehetséges toodirectly kiszolgálók vagy az infrastruktúra elemeinek felhőalapú környezetben hello erőforrás állapota célja tooreduce hello elhárításával kapcsolatos töltött időt. Különösen hello célja tooreduce hello időt, hogy meghatározása, hogy hello probléma hello gyökérmappájában értékű hello alkalmazás vagy a hello belüli esemény Azure platformon. További információkért lásd: [megértése és használja erőforráskészlet állapotát](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-hello-vm-tooreboot"></a>Műveletek és hello VM tooreboot okozó események

### <a name="planned-maintenance"></a>Tervezett karbantartás
A Microsoft Azure rendszeresen hello földgömb tooimprove hello megbízhatóságát, teljesítményét és hello állomás infrastruktúrájának alapjául szolgáló virtuális gépek biztonsági elvégzi a frissítést. A virtuális gépeken gyakorolt hatás nélkül hajtja végre ezeket a frissítéseket, beleértve a memória-megőrzi a frissítések, számos, vagy a felhőalapú szolgáltatások.

Egyes frissítések azonban a számítógép újraindítása szükséges. Ezekben az esetekben hello virtuális gépek vannak állítsa le azt hello infrastruktúra javítás során, majd hello virtuális gépek újraindítására.

toounderstand milyen Azure tervezett karbantartás, és milyen hatással vannak a Linux virtuális gépek hello rendelkezésre állását olvassa el a az itt felsorolt hello cikkeket. hello cikkekben hello Azure tervezett karbantartás folyamattal kapcsolatos háttér- és hogyan tooschedule tervezett karbantartás toofurther csökkentése hello hatása.

- [Az Azure virtuális gépek tervezett karbantartása](../articles/virtual-machines/windows/planned-maintenance.md)
- [Hogyan tooschedule a tervezett karbantartások Azure virtuális gépeken](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Memóriamegőrző frissítések   
Ez az osztály a frissítések a Microsoft Azure-ban, a felhasználók ne legyen hatással a futó virtuális gépek a felhasználói élmény. Ezek a frissítések számos toocomponents vagy szolgáltatásokhoz, amelyek-példányt futtató hello zavarása nélkül is frissíthető. Némelyike platform infrastruktúrát érintő frissítéseket hello állomás operációs rendszeren, amely a virtuális gépek hello újraindítás nélkül lehet alkalmazni.

A memória-megőrzi az frissítések technológia, amely lehetővé teszi a helyszíni élő áttelepítés úgy hajthatja végre. Amikor frissül, hello virtuális gép bekerül egy *szünetel* állapota. Ebben az állapotban megőrzi a RAM memória hello hello mögöttes gazdagép operációs rendszere hello szükséges frissítések és javítások lesz. virtuális gép hello felfüggesztés 30 másodpercen belül folytatja a működését. Hello VM folytatja a működését, miután az óra szinkronizálva van a automatikusan.

Miatt hello rövid késleltetés időszak frissítéseinek telepítéséhez ezzel a mechanizmussal nagy mértékben csökkenti a virtuális gépek hello hello gyakorolt. Nem minden frissítés azonban így is telepíthető. 

Többpéldányos (a virtuális gépek rendelkezésre állási csoportba) frissítései alkalmazott frissítési tartományok egyszerre.

> [!NOTE]
> A kernel pánikot Linux gépeken, amelyek a régi kernel-verziók alatt ez a frissítési mód van hatással. tooavoid ezt a problémát, a frissítés tookernel verzió 3.10.0-327.10.1 vagy újabb. További információkért lásd: [An Azure Linux virtuális gép egy 3.10-alapú kernel panics egy gazdagép-csomópont frissítése után](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>A felhasználó által kezdeményezett újraindítás vagy -leállítás műveletek
 
Ha újraindítás hello Azure-portálon, az Azure PowerShell, a parancssori felület vagy a alaphelyzetbe API elvégezhető, az hello hello esemény található [Azure tevékenységnapló](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Hello virtuális gép operációs rendszerről hello műveletet hajt végre, ha található hello esemény hello rendszer naplóit.

Egyéb forgatókönyvek, amelyek általában hello VM tooreboot több konfiguráció-módosítási műveletek közé tartozik. Szokásos megjelenik egy figyelmeztető üzenet, amely jelzi, hogy egy bizonyos művelet végrehajtása hello virtuális gép újraindítását eredményezi. Például a virtuális gép átméretezési műveleteket, megváltoztatása hello hello rendszergazdai fiók jelszavát, és a statikus IP-cím beállítása.

### <a name="azure-security-center-and-windows-update"></a>Az Azure Security Center és a Windows Update
Az Azure Security Center napi Windows és Linux virtuális gépek figyeli a hiányzó operációsrendszer-frissítések. A Security Center lekér egy listát az elérhető biztonsági és kritikus frissítések a Windows Update webhelyről vagy a Windows Server Update Services (WSUS), attól függően, amelyek a szolgáltatás úgy van konfigurálva a Windows virtuális gép. A Security Center hello Linux rendszerek legújabb frissítéseit is keres. A virtuális gép hiányzik a rendszer frissítését, ha a Security Center javasolja, hogy a rendszer frissítéseinek alkalmazása. a rendszer frissítések alkalmazása hello hello Azure-portálon a Security Center hello szabályozza. Egyes frissítések telepítése után újraindul a virtuális gép lehet szükség. További információkért lásd: [alkalmazza a rendszer frissítéseket az Azure Security Centerben](../articles/security-center/security-center-apply-system-updates.md).

A helyszíni kiszolgálók, például Azure nem küldi el a frissítéseket a Windows Update tooWindows Azure virtuális gépeken, mivel ezek a gépek tervezett toobe a felhasználókat kezeli. Áll, azonban javasolt tooleave hello automatikus Windows Update-beállítás engedélyezve van. A Windows Update frissítések automatikus telepítése a is eredményezheti újraindítások toooccur hello frissítések alkalmazása után. További információkért lásd: [Windows Update GYIK](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-hello-availability-of-your-vm"></a>Más helyzetekben érintő hello a virtuális gép rendelkezésre állása
Nincsenek más esetekben, ahol Azure aktívan előfordulhat, hogy felfüggeszti hello használata a virtuális gépek. Kap értesítő e-mailek műveletet, mielőtt, konfigurálnia kell egy alapul szolgáló problémák hello alkalommal tooresolve. Virtuális gép rendelkezésre állási befolyásoló problémákat például biztonsági problémát és a fizetési módok hello lejáratának.

### <a name="host-server-faults"></a>Állomás server hibák 
virtuális gép hello belül egy Azure-adatközpontban futtató fizikai kiszolgálón tárolja. hello fizikai kiszolgálón fut az ügynök nevű tooa hozzáadása a gazdagép ügynöke hello néhány Azure összetevőt. A fizikai kiszolgálón hello Azure szoftver összetevők válaszol, amikor a megfigyelési rendszere hello hello állomás tooattempt helyreállítása újraindítását váltja ki. hello VM érhető általában öt percen belül újra, és a korábban már azonos a gazdagép hello toolive továbbra is.

Kiszolgáló hibák általában hardverhiba, például a merevlemezek vagy SSD-meghajtó hello hiba okozza. Azure folyamatosan ezeket az eseményeket figyeli, alapul szolgáló hibák hello azonosítja és frissítések bevezeti a hello megoldás megvalósítása és tesztelése után.

Állomás server hibákat adott toothat kiszolgáló lehet, mert a virtuális gép ismételt újraindítás helyzet előfordulhat, hogy javítani manuálisan ismételt üzembe helyezéssel hello VM tooanother gazdagép-kiszolgálón. Ez a művelet is elindítható a hello segítségével **újratelepíteni** hello Részletek lapján hello VM lehetőséget, vagy leállításával és újraindításával hello hello Azure-portálon a virtuális gép.

### <a name="auto-recovery"></a>Automatikus helyreállítás
Hello gazdagép-kiszolgálón a bármilyen okból nem újra, ha hello Azure platformon indít el az automatikus helyreállítás művelet tootake hello hibás gazdagép-kiszolgálón kívül további vizsgálatok elforgatás. 

Minden virtuális gép ugyanazon a gazdagépen olyan automatikusan áthelyezett tooa különböző, kifogástalan állapotú gazdagép-kiszolgálón. Befejeződött a folyamat általában 15 percen belül. További információ az automatikus helyreállítás folyamat hello toolearn lásd [virtuális gépek automatikus helyreállítás](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Nem tervezett karbantartás
Ritka esetekben hello Azure műveleti csapata, előfordulhat, hogy kell tooperform karbantartási tevékenységek tooensure hello hello Azure platformon általános állapotát. Ez a viselkedés hatással lehetnek a virtuális gép rendelkezésre állási, és általában eredményezi hello azonos automatikus helyreállítás művelet ismertetett módon.  

Nem tervezett maintenances hello alábbiakat foglalja magába:

- Sürgős csomópont töredezettségmentesítés
- Sürgős hálózati kapcsoló frissítések

### <a name="vm-crashes"></a>Virtuális gép összeomlik
Virtuális gépek virtuális gépért hello belül problémák miatt előfordulhat, hogy újraindul. hello munkaterhelés vagy az, hogy fut-hello VM szerepkör válthat ki hibakeresés hello vendég operációs rendszerben. Hello összeomlási hello okának meghatározásához, hello rendszer és az alkalmazások naplók megtekintéséhez a Windows virtuális gépekhez, és soros naplók hello Linux virtuális gépekhez.

### <a name="storage-related-forced-shutdowns"></a>A kényszerített leállítások tárolással kapcsolatos
Az Azure virtuális gépek operációs rendszer és az adatok tárolási hello Azure tároló-infrastruktúra a tárolt virtuális lemezek támaszkodnak. Hello rendelkezésre állás vagy a virtuális gép hello és hello társított virtuális lemezek közötti kapcsolat több mint 120 másodpercig van hatással, amikor hello Azure platformon a virtuális gépek tooavoid adatsérülés hello kényszerített leállítására hajt végre. hello virtuális gépeket automatikusan a újra be vannak kapcsolva, után tárolási kapcsolat visszaállítása sikeresen megtörtént. 

hello leállítási hello időtartama a lehető legrövidebb legyen öt perc is lehet, de sokkal hosszabb lehet. hello következő hello bizonyos esetekben kényszerített leállítások tárolással kapcsolatos társított egyike: 

**IO meghaladó korlátozza.**

Virtuális gépek előfordulhat, hogy átmenetileg állítható le, mert hello mennyiségű i/o-műveletek száma másodpercenként (IOPS) meghaladja a hello i/o-korlátok hello lemez i/o-kérelmek szabályozott következetesen amikor. (Standard méretű lemez tároló mérete legfeljebb too500 IOPS.) toomitigate probléma lemez csíkozást használhatja, de hello tárolóhely hello Vendég virtuális gép, attól függően, hogy hello munkaterhelés. További információkért lásd: [konfigurálása a Azure virtuális gépek tárolási teljesítményének optimalizálása](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Magasabb IOPS-korlátok vonatkoznak érhetők el a prémium szintű Azure Storage too80, 000 IOPS fel. További információkért lásd: [prémium szintű Storage nagy teljesítményű](../articles/storage/common/storage-premium-storage.md).

### <a name="other-incidents"></a>Egyéb események
Ritka esetekben előfordulhat, hogy a széles körű probléma hatással lehet a több kiszolgálót egy Azure-adatközpontban. Ez a probléma akkor fordul elő, ha Azure-csapat hello e-mailt küld értesítéseket hatással toohello előfizetések. Ellenőrizheti a hello [Azure szolgáltatás állapota irányítópult](https://azure.microsoft.com/status/) és hello Azure-portál a folyamatban lévő kimaradások és túli incidensek hello állapotának.
