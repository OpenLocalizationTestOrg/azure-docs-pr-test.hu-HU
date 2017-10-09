## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>A virtuális gépek újraindításának ismertetése – karbantartás és állásidő
E toovirtual gép befolyásolja az Azure-ban három forgatókönyv: a hardver nem tervezett karbantartás, váratlan állásidőt és a tervezett karbantartások.

* **Nem tervezett hardver karbantartási esemény** akkor fordul elő, amikor hello, az Azure platform képes adott hello hardver- vagy bármely platformra összetevő társított tooa fizikai gép, toofail tárgya. Hello platform előrejelzi a hibát, ha azt egy hardver nem tervezett karbantartási esemény tooreduce hello hatás toohello futtatott virtuális gépek, hogy a hardver állít ki. Azure élő áttelepítés technológia toomigrate hello virtuális gépek a hardver tooa megfelelő fizikai számítógép sikertelen hello használja. Élő áttelepítés a virtuális gép megőrzi az, hogy csak a szünet hello virtuális gép rövid időn belül művelet. Memória, a megnyitott fájlokat és a hálózati kapcsolatok megmaradnak, de teljesítménye csökkenhet, mielőtt és/vagy hello esemény után. Azokban az esetekben, ahol az élő áttelepítés nem használható hello VM fog tapasztalni váratlan állásidőt, az alább ismertetett.


* **Egy nem várt leállás** ritkán akkor fordul elő, amikor valamilyen módon hello hardver- vagy hello fizikai infrastruktúra az alapul szolgáló a virtuális gép hibát jelzett. Ez lehet helyi hálózati hiba, a helyi lemezek meghibásodása, vagy egyéb állványszintű meghibásodások. Ilyen hiba lép fel, amikor hello Azure platformon automatikusan áttelepíti (heals) a virtuális gép tooa megfelelő fizikai számítógép. Során hello javító eljárás, virtuális gépek leállásra (újraindítás) és néhány esetben adatvesztéssel hello ideiglenes meghajtón. hello csatolja az operációs rendszer, és adatlemezek mindig megmaradnak. 

* **Tervezett karbantartási események** Azure platformon tooimprove alapul szolgáló Microsoft toohello által végzett rendszeres frissítések teljes megbízhatóság, teljesítmény és a virtuális gépeken futó hello platform infrastruktúra biztonságát. A frissítések a legtöbb esetben semmilyen hatással nincsenek a virtuális gépek vagy a felhőszolgáltatások működésére (lásd: [VM-megőrző karbantartás](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/preserving-maintenance)). Hello Azure platform minden lehetséges alkalommal toouse VM megóvása karbantartási kísérleteket, amíg nincsenek ritkán ezeket a frissítéseket a virtuális gép tooapply hello szükséges frissítések toohello alapul szolgáló infrastruktúra újraindítás szükséges. Ebben az esetben lehet végrehajtani Azure tervezett karbantartás karbantartási-helyezze üzembe újra műveletet hello karbantartási kezdeményezése a virtuális gépek hello megfelelő idő ablakban. További információkért lásd: [Virtuális gépek tervezett karbantartása](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/planned-maintenance/).


tooreduce hello hatása megfelelő tooone vagy ezek eseményeket állásidőt, ajánlott magas rendelkezésre állású gyakorlati tanácsok a virtuális gépek számára a következő hello:

* [Több virtuális gép rendelkezésre állási csoportba konfigurálása a redundancia biztosítása érdekében]
* [Felügyelt lemezek használata rendelkezésre állási csoporthoz tartozó virtuális gépekkel]
* [Használja az ütemezett események tooproactively válasz tooVM érintő események] (https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-scheduled-events)
* [Az egyes alkalmazásrétegek külön rendelkezésre állási csoportokba konfigurálása]
* [Terheléselosztók és rendelkezésre állási csoportok együttes alkalmazása]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Több virtuális gép rendelkezésre állási csoportba konfigurálása a redundancia biztosítása érdekében
tooprovide redundancia tooyour alkalmazás, javasoljuk, hogy legalább két virtuális gép rendelkezésre állási csoportba. Ez a konfiguráció biztosítja, hogy vagy a tervezett vagy nem tervezett karbantartási események esetén legalább egy virtuális gép elérhető, és megfelel-e 99,95 % hello Azure SLA-t. További információkért lásd: hello [SLA-t a virtuális gépek](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Ha lehet, önmagában ne helyezzen egyetlen különálló virtuális gépet egy rendelkezésre állási csoportba. Az így konfigurált virtuális gépek nem jogosultak az SLA-garanciára, és leállhatnak az Azure tervezett karbantartási eseményei során, hacsak az önálló virtuális gép nem [Azure Premium Storage](../articles/storage/common/storage-premium-storage.md) csomagot használ. Prémium szintű storage használatával egyetlen virtuális gép esetében hello Azure SLA vonatkozik.

A rendelkezésre állási csoport összes virtuális gépén hozzá van rendelve egy **frissítési tartomány** és egy **tartalék tartomány** hello Azure platformon alapul szolgáló által. Egy adott rendelkezésre állási csoport öt nem felhasználó által konfigurálható frissítési tartományok alapértelmezés szerint tartoznak (Resource Manager üzembe helyezések majd lehet a megnövekedett tooprovide too20 frissítési tartomány) a virtuális gépek és a mögöttes fizikai hardver tooindicate csoportok amely is újra kell indítani a hello ugyanannyi időt vesz igénybe. Ha ötnél több virtuális gépek egy egyetlen rendelkezésre állási csoport belül vannak beállítva, hello hatodik virtuális gépek ugyanazt a frissítési tartomány hello első virtuális gépként, hello hetedik ugyanazt a tartományi frissítési hello második virtuális gép, ezért a hello a hello elhelyezi a. frissítési tartományok újraindítása folyamatban nem folytathatja egymás után tervezett karbantartás alatt, de csak egy frissítési tartományt hello sorrendjének egyszerre újraindítása után. Egy újraindított frissítési tartomány 30 perc toorecover kap előtt karbantartási különböző frissítési tartomány lehet kezdeményezni.

Tartalék tartományok definiálása hello csoport a virtuális gépek, amelyek egy közös power forrás- és a hálózati kapcsolóhoz. Alapértelmezés szerint a rendelkezésre állási csoport belül konfigurált hello virtuális gépek toothree tartalék tartományainak Resource Manager üzembe helyezések (két fault tartományok a klasszikus) mentése egymástól között. Miközben a virtuális gépek elhelyezését a rendelkezésre állási csoport nem nyújt védelmet az alkalmazás az operációs rendszer vagy alkalmazás-specifikus hibák, hogy korlátozza lehetséges fizikai hardver hibák, a hálózati kimaradások vagy a power megszakításokat hello hatását.

<!--Image reference-->
   ![Fogalmi megrajzolása hello frissítési tartomány és a tartalék tartomány beállításait.](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Felügyelt lemezek használata rendelkezésre állási csoporthoz tartozó virtuális gépekkel
Ha a nem felügyelt lemezzel rendelkező virtuális gépek jelenleg használ, erősen ajánlott, [alakítsa át a virtuális gépek rendelkezésre állási csoport toouse kezelt lemezeken](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md).

[Által kezelt lemezeken](../articles/virtual-machines/windows/managed-disks-overview.md) nagyobb megbízhatóságot meg egymástól különítve rendelkezésre állási készletek biztosításával, amely egy rendelkezésre állási csoportban lévő virtuális gépek hello lemezek vannak elég tooavoid egyetlen ponton felmerülő hibákat. Ennek érdekében automatikusan hello lemezek különböző tároló fürtök helyezi el. Ha egy tárolási fürt toohardware-vagy szoftverhiba miatt meghiúsul, csak hello Virtuálisgép-példányok az adott bélyegzők lemezzel sikertelen.

![Felügyelt lemezek tartalék tartományai](./media/virtual-machines-common-manage-availability/md-fd.png)

> [!IMPORTANT]
> a felügyelt rendelkezésre állási csoportok tartalék tartományok száma hello függ a terület - két vagy három régiónként. a következő táblázat azt mutatja be hello szám régiónként hello

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

Ha azt tervezi, hogy a virtuális gépek toouse [lemezek nem felügyelt](../articles/virtual-machines/windows/about-disks-and-vhds.md#types-of-disks), kövesse az alábbi ajánlott eljárások a Storage-fiókok, mint a virtuális merevlemezek (VHD) a virtuális gépek tárolására [lapblobokat](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs).

1. **Tartsa társított hello a virtuális gép összes lemeze (az operációs rendszer és data) ugyanabban a tárfiókban**
2. **Felülvizsgálati hello [korlátok](../articles/storage/common/storage-scalability-targets.md) a nem felügyelt lemezek tárfiókokban hello száma** további virtuális merevlemezek tooa tárfiók hozzáadása előtt
3. **Használjon külön tárfiókot minden egyes virtuális géphez a rendelkezésre állási csoportban.** Storage-fiókok ne ossza meg a hello több virtuális gépek azonos rendelkezésre állási csoportban. Elfogadható, virtuális gépek különböző rendelkezésre állási készletek tooshare tárfiókok között ha a fenti gyakorlati tanácsok

## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Az egyes alkalmazásrétegek külön rendelkezésre állási csoportokba konfigurálása
Ha a virtuális gépek összes csaknem azonosak és ki tudja szolgálni hello azonos célját az alkalmazásról, azt javasoljuk, hogy konfigurálja a rendelkezésre állási készlet minden egyes réteg az alkalmazás.  Ha két különböző tiers hello az azonos rendelkezésre állási csoportban, hello egyszerre OK azonos alkalmazáshoz tartozó összes virtuális gépnek. Ha legalább két virtuális gépet konfigurál az egyes rendelkezésre állási csoportokba minden egyes réteghez, ezzel garantálja, hogy minden egyes rétegben legalább egy virtuális gép elérhető lesz.

Például hello előtérrendszerét az alkalmazást az IIS, Apache, Nginx egyetlen rendelkezésre állási csoportban lévő összes hello virtuális gépek ütemezésbe helyezheti. Győződjön meg arról, hogy csak az előtér virtuális gépeket helyezi hello azonos rendelkezésre állási csoportot. Ugyanígy gondoskodjon róla, hogy csak az adatrétegben futó virtuális gépek kerüljenek a nekik dedikált rendelkezésre állási csoportba, például replikált SQL Server-virtuálisgépek vagy MySQL-virtuálisgépek.

<!--Image reference-->
   ![Alkalmazásrétegek](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Terheléselosztók és rendelkezésre állási csoportok együttes alkalmazása
Hello egyesítése [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) a rendelkezésre állási beállítása tooget hello legtöbb alkalmazás rugalmas. hello Azure Load Balancer osztja el a több virtuális gépek közötti forgalmat. A Standard szint virtuális gépekhez hello Azure Load Balancer része. Nem minden virtuális gép szolgáltatásszintek hello Azure Load Balancer. A virtuális gépek terheléselosztásáról további információkért lásd a [virtuális gépek terheléselosztását](../articles/virtual-machines/virtual-machines-linux-load-balance.md) ismertető témakört.

Ha hello terheléselosztó nem több virtuális gép között toobalance forgalom konfigurálni, akkor minden tervezett karbantartási események hello csak forgalom-szolgáló virtuális gép, egy szolgáltatáskimaradás tooyour alkalmazás réteghez, amely hatással van. Azonos szinten hello a több virtuális gép elhelyezéséhez hello azonos terheléselosztó és a rendelkezésre állási készlet lehetővé teszi, hogy forgalom toobe folyamatosan szolgálja ki legalább egy példány betöltése alatt.


<!-- Link references -->
[Több virtuális gép rendelkezésre állási csoportba konfigurálása a redundancia biztosítása érdekében]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Az egyes alkalmazásrétegek külön rendelkezésre állási csoportokba konfigurálása]: #configure-each-application-tier-into-separate-availability-sets
[Terheléselosztók és rendelkezésre állási csoportok együttes alkalmazása]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Felügyelt lemezek használata rendelkezésre állási csoporthoz tartozó virtuális gépekkel]: #use-managed-disks-for-vms-in-an-availability-set
