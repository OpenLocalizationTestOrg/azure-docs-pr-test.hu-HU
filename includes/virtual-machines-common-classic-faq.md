


Ez a cikk foglalkozik felhasználók kérdezzen hello klasszikus telepítési modellel létrehozott Azure virtuális gépek gyakori kérdésekre.

## <a name="can-i-migrate-my-vm-created-in-hello-classic-deployment-model-toohello-new-resource-manager-model"></a>Áttelepítheti a virtuális gép hello klasszikus üzembe helyezési modell toohello új erőforrás-kezelő modellel létrehozott?
Igen. Útmutatást toomigrate, lásd:

* [Klasszikus tooAzure erőforrás-kezelő Azure PowerShell használatával telepítse át](../articles/virtual-machines/windows/migration-classic-resource-manager-ps.md).
* [Klasszikus tooAzure Azure parancssori felület használatával erőforrás-kezelő áttelepítése](../articles/virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Mit futtathatok egy Azure-beli virtuális gépen?
Minden előfizető kiszolgálószoftvereket futtathat az Azure-beli virtuális gépeken. Futtathatja a Windows Server legújabb verzióit, valamint különböző Linux-disztribúciókat. Támogatási részletek:

• Windows rendszerű virtuális gépek – [Microsoft kiszolgálószoftveres támogatás az Azure Virtual Machines szolgáltatáshoz](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• Linux rendszerű virtuális gépek – [Linux az Azure által támogatott disztribúciókon](http://go.microsoft.com/fwlink/p/?LinkId=393551)

Windows ügyfél lemezképek a Windows 7 és Windows 8.1 egyes verziói elérhető tooMSDN Azure juttatás előfizetők és az MSDN fejlesztői és teszt – használatalapú fizetés-előfizetők fejlesztési és tesztelési feladatok. Részletekért, többek között az utasításokért és korlátozásokért tekintse meg az [MSDN-előfizetők számára elérhető Windows-rendszerképeket](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) ismertető cikket.

## <a name="why-are-affinity-groups-being-deprecated"></a>Miért avultak el az affinitáscsoportok?
Az affinitáscsoport egy örökölt fogalom egy ügyfél felhőszolgáltatási környezeteinek és tárfiókjainak az Azure-on belüli földrajzi csoportosítására. Tooimprove VM-VM hálózati teljesítmény hello korai Azure hálózati kialakításokat azokat eredetileg adtak meg. Ezeket a virtuális hálózatok (Vnetek), amely korlátozott tooa kis számú régióban hardver hello eredeti kiadását is támogatja.

hello Azure aktuális hálózati régión belül célja, hogy az affinitáscsoportok már nem szükségesek. A virtuális hálózatok szintén regionális hatókörben vannak megtervezve, így nem szükséges affinitáscsoport egy virtuális hálózat használatához. Toothese fejlesztései miatt már nem javasoljuk, hogy az ügyfelek affinitáscsoportok mert azok bizonyos esetekben kell korlátozza. Affinitáscsoportok használatával feleslegesen társítsa a virtuális gépek toospecific hardver, amely korlátozza, amelyek a rendelkezésre álló tooyou Virtuálisgép-méretek hello választott. Toocapacity kapcsolatos hibákat azt is vezethet, ha hello affinitáscsoport hello adott hardverekhez kapcsolódó új virtuális gépek van kapacitása tooadd tett kísérlet során.

Affinitáscsoport szolgáltatások hello Azure Resource Manager üzembe helyezési modellel, és az Azure-portálon hello már elavult. Hello klasszikus Azure portál azt még elavulttá affinitáscsoportok létrehozása és tároló-erőforrások, amelyek rögzített tooan affinitáscsoport létrehozása támogatása. Nincs szükség a toomodify meglévő felhőszolgáltatás, amely egy affinitáscsoporthoz használ. Azonban ne használjon affinitáscsoportokat új felhőszolgáltatásokhoz, kivéve, ha egy Azure-terméktámogatási szakértő ezt javasolja.

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Mennyi tárhelyet használhatok egy virtuális gép esetén?
Minden egyes adatlemez mentése too1 TB lehet. adatlemezek használhatja hello száma hello hello virtuális gép méretétől függ. Részletek: [Virtuális gépek méretei](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure-tárfiók tárolási biztosít hello operációsrendszer-lemez és adatlemezeket. Minden lemez egy lapblobként tárolt .vhd-fájl. A díjszabás részleteiért lásd [a Storage szolgáltatás díjszabását](http://go.microsoft.com/fwlink/p/?LinkId=396819).

## <a name="which-virtual-hard-disk-types-can-i-use"></a>Milyen típusú virtuális merevlemezeket használhatok?
Az Azure csak a rögzített méretű, VHD formátumú virtuális merevlemezek használatát támogatja. Ha olyan vhdx-fájlt, amelyet az Azure-ban toouse, kell-e toofirst átalakíthatja a Hyper-V kezelője vagy hello segítségével [convert-VHD](http://go.microsoft.com/fwlink/p/?LinkId=393656) parancsmag. Miután ezt megtenné, [Add-AzureVHD](https://msdn.microsoft.com/library/azure/dn495173.aspx) parancsmag (szolgáltatásfelügyelet módban) tooupload hello tooa tárfiók VHD-t, hogy használhassa a virtuális gépek Azure-ban.

* Linux útmutatásért lásd: [létrehozása, majd ismét feltölteni a virtuális merevlemez, hogy tartalmazza a Linux operációs rendszer hello](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
* Windows útmutatásért lásd: [létrehozása és feltöltése a Windows Server VHD tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="are-these-virtual-machines-hello-same-as-hyper-v-virtual-machines"></a>Azok a virtuális gépek hello ugyanaz, mint a Hyper-V virtuális gépek?
Az sokféleképpen fontosságúak hasonló túl "1. generációs" Hyper-V virtuális gépek, de még nem pontosan hello azonos. Mindkét típusát adja meg a virtualizált hardverrel, és kompatibilis hello VHD formátumú virtuális merevlemezeket. Ez azt jelenti, hogy áthelyezheti őket a Hyper-V és az Azure között. Az alábbi három fő különbség azonban sokszor meglepetésként éri a Hyper-V-felhasználókat:

* Azure konzol hozzáférési tooa virtuális gép nem biztosít. Nincs nincs módja tooaccess egy virtuális Gépet, amíg elkészült rendszerindítás.
* Az Azure-beli virtuális gépek a legtöbb [méretben](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) csak 1 virtuális hálózati adapterrel rendelkeznek, ami azt jelenti, hogy csak 1 külső IP-címmel rendelkezhetnek. (hello A8 és A9 méretű használható egy második hálózati adapter a alkalmazás kommunikáció korlátozott esetekben példányai között.)
* Az Azure-beli virtuális gépek nem támogatják a 2. generációs Hyper-V-beli virtuálisgép-funkciókat. A funkciókkal kapcsolatban lásd a [virtuális gépek műszaki adatait Hyper-V használata estén](http://technet.microsoft.com/library/dn592184.aspx) és a [2. generációs virtuális gépek áttekintését](https://technet.microsoft.com/library/dn282285.aspx).

## <a name="can-these-virtual-machines-use-my-existing-on-premises-networking-infrastructure"></a>Használhatják ezek a virtuális gépek a meglévő, helyszíni hálózati infrastruktúrát?
Hello klasszikus üzembe helyezési modellel létrehozott virtuális gépeket használhatja az Azure Virtual Network tooextend a meglévő infrastruktúrát. hello megoldás, például egy fiókiroda beállítása. Meg lehet kiépíteni és virtuális magánhálózatok (VPN) kezelése az Azure-ban, valamint biztonságosan csatlakoztassa őket tooon helyszíni informatikai infrastruktúrát. További részleteket a [virtuális hálózatok áttekintésében](../articles/virtual-network/virtual-networks-overview.md) talál.

Toospecify hello hálózathoz, amelyet hello virtuális gépet hoz létre a virtuális gép toobelong toowhen hello lesz szüksége. Nem lehet csatlakozni a meglévő virtuális gép tooa virtuális hálózat. Azonban megoldható, a leválasztás hello virtuális merevlemez (VHD) hello meglévő virtuális gépből, és majd használjon egy új virtuális gép toocreate hello olyan hálózati beállításokat szeretné.

## <a name="how-can-i-access--my-virtual-machine"></a>Hogyan érhetem el a virtuális gépem?
Távoli asztali kapcsolattal egy Windows virtuális gép vagy egy Secure Shell (SSH) a Linux virtuális gépek tooestablish egy távoli kapcsolat toolog toohello virtuális gépen van szüksége. További útmutatásért lásd:

* [Hogyan tooLog a virtuális gép futó Windows Server tooa](../articles/virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 2 egyidejű kapcsolatok maximális támogatottak, kivéve, ha egy távoli asztali munkamenetgazda hello kiszolgáló van konfigurálva.  
* [Hogyan tooLog a futó Linux virtuális gép tooa](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Alapértelmezés szerint az SSH legfeljebb 10 párhuzamos kapcsolatot tesz lehetővé. Ezt a számot növelheti hello konfigurációs fájl szerkesztésével.

Ha a távoli asztal vagy SSH problémákat tapasztal, telepítése, és hello használata [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bővítmény toohelp javítás hello probléma.

Windows rendszerű virtuális gépek esetén a további lehetőségek is a rendelkezésére állnak:

* A klasszikus Azure portálon hello, hello virtuális gép található, majd kattintson az **távelérés alaphelyzetbe** hello parancs segítségével.
* Felülvizsgálati [hibaelhárítása távoli asztali kapcsolatok tooa Windows-alapú Azure virtuális gépek](../articles/virtual-machines/windows/troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Windows PowerShell távoli eljáráshívás tooconnect toohello virtuális gép használja, vagy az egyéb erőforrások tooconnect toohello VM további végpontokat hoz létre. További információkért lásd: [hogyan tooSet be végpontok tooa virtuális gép](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Ha jártas a Hyper-V, akkor előfordulhat, hogy keres egy eszköz hasonló tooVMConnect. Azure nem kínál hasonló eszközt, mert a konzol hozzáférési tooa virtuális gép nem támogatott.

## <a name="can-i-use-hello-temporary-disk-hello-d-drive-for-windows-or-devsdb1-for-linux-toostore-data"></a>Hello mennyiségű ideiglenes lemezes (d meghajtó Windows vagy Linux /dev/sdb1 hello) toostore adatok használata
Hello mennyiségű ideiglenes lemezes (hello alapértelmezett Windows vagy Linux /dev/sdb1 d meghajtó) toostore adatok ne használja. Ezek csak ideiglenes tárak, és ha így tesz, azt kockáztatja, hogy az adatok elvesznek, és nem lehet őket helyreállítani. Ez akkor fordulhat elő, ha hello virtuális gép tooa másik gazdagépre helyezi. A virtuális gépek átméretezésével, hello állomás vagy hello gazdagépen hardverhiba frissítése közé tartoznak a hello okok miatt virtuális gépek áthelyezése előfordulhat, hogy.

## <a name="how-can-i-change-hello-drive-letter-of-hello-temporary-disk"></a>Hogyan módosítható hello mennyiségű ideiglenes lemezes hello betűjele?
Windows virtuális gépen módosíthatja a hello meghajtóbetűjel által áthelyezése hello oldal fájlja és a meghajtó-betűjelek újbóli, de meg arról, hogy a meghatározott sorrendben lépéseket hello toomake lesz szüksége. Útmutatásért lásd: [hello meghajtóbetűjel hello Windows ideiglenes lemez](../articles/virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="how-can-i-upgrade-hello-guest-operating-system"></a>Hogyan frissíthetem hello vendég operációs rendszer?
hello kifejezés frissítés általában azt jelenti, hogy áthelyezése tooa több, miközben a hello marad az operációs rendszer legújabb verzióra ugyanazt a hardvert. Azure virtuális gépek, a folyamat tooa áthelyezésére hello újabb kiadásra eltér a Linux és a Windows:

* Linux virtuális gépekhez használja a hello csomag felügyeleti eszközökkel és eljárásokkal megfelelő hello terjesztéshez.
* Egy Windows rendszerű virtuális gép toomigrate hello-kiszolgáló Windows Server áttelepítési eszközök hello hasonlót kell. Ne kísérelje meg a tooupgrade hello vendég operációs rendszer, amíg az Azure-on található. Hozzáférés toohello virtuális gép elvesztését kockáztatja hello miatt nem támogatott. Ha problémák merülnek fel hello frissítés során, elveszhet hello képességét toostart egy távoli asztali munkamenetet, és így nem tudja tootroubleshoot hello problémákat.

További hello eszközeivel és eljárásaival áttelepítéséhez egy Windows Server kapcsolatos általános információkért lásd: [szerepkörök és szolgáltatások áttelepítése tooWindows Server](http://go.microsoft.com/fwlink/p/?LinkId=396940).

## <a name="whats-hello-default-user-name-and-password-on-hello-virtual-machine"></a>Mi az az hello alapértelmezett felhasználónevet és jelszót hello virtuális gépen?
Azure által biztosított hello lemezképeket nem kell egy előre megadott felhasználónév és jelszó. Ha használja ezeket a képeket egyik virtuális gépet hoz létre, szüksége lesz tooprovide egy felhasználónevet és jelszót, amelyre azt ismertetjük toolog toohello virtuális gépen.

Ha elfelejtette hello felhasználói nevet és jelszót, és hello Virtuálisgép-ügynök telepítése, telepítheti és hello használata [VMAccess](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bővítmény toofix hello probléma.

További részletek:

* Hello Linux képek hello klasszikus Azure-portált használja, ha "azureuser" alapértelmezett felhasználónév van megadva, de ezt hello módon toocreate hello virtuális gép "A gyűjtemény" gyors létrehozása helyett segítségével módosíthatja. "A gyűjtemény" használatával is eldöntheti, hogy toouse jelszót, egy SSH-kulccsal vagy mindkét toolog a. hello felhasználói fiók pedig egy jogosulatlan felhasználó, amely hozzáfér a "sudo" toorun kiemelt parancsok. hello "Gyökér" fiók le van tiltva.
* A Windows lemezképeit szüksége lesz tooprovide egy felhasználónevet és jelszót hello virtuális gép létrehozásakor. hello fiókot hozzá toohello rendszergazdák csoportnak.

## <a name="can-azure-run-anti-virus-on-my-virtual-machines"></a>Tud víruskeresőt futtatni az Azure a virtuális gépeimen?
Az Azure vírusvédelmi megoldások számos lehetőséget kínál, de tooyou toomanage fel azt. Például előfordulhat, hogy különálló előfizetés szükséges kártevőirtó szoftver, és toodecide kell toorun vizsgálja, és telepítse a frissítéseket. A Microsoft Antimalware, a Symantec Endpoint Protection vagy a TrendMicro Deep Security Agent VM-bővítményével a windowsos virtuális gép létrehozásakor vagy később is biztosíthatja a víruskereső-támogatást. hello Symantec és TrendMicro bővítmények lehetővé teszik, hogy egy korlátozott idejű ingyenes próbaelőfizetés igényléséhez vagy a meglévő nagyvállalati előfizetéssel. A Microsoft Antimalware ingyenes. Részletes információ:

* [Hogyan tooinstall és a Symantec Endpoint Protection konfigurálása egy Azure virtuális gépen](http://go.microsoft.com/fwlink/p/?LinkId=404207)
* [Hogyan tooinstall és a Trend Micro mély biztonságának konfigurálása szolgáltatásként egy Azure virtuális gépen](http://go.microsoft.com/fwlink/p/?LinkId=404206)
* [Kártevőirtó megoldások telepítése Azure-beli virtuális gépeken](https://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## <a name="what-are-my-options-for-backup-and-recovery"></a>Milyen biztonsági mentési és helyreállítási lehetőségek közül választhatok?
Az Azure Backup bizonyos régiókban elérhető előzetes verzióként. További részletek: [Azure-beli virtuális gépek biztonsági mentése](../articles/backup/backup-azure-vms.md). Hivatalos partnereinktől egyéb megoldások is elérhetők. jelenleg elérhető toofind keresési hello Azure piactéren.

Egy további lehetőség toouse hello pillanatképkezelési funkciókat a blob Storage tárolóban. toodo, tooshut hello VM bármely művelet, amely egy blob pillanatkép támaszkodik előtt le kell. Függőben lévő adatírás menti, és a hello fájlrendszer konzisztens állapotba helyezi.

## <a name="how-does-azure-charge-for-my-vm"></a>Hogyan számítja az Azure a virtuális gép díjszabását?
Azure óradíjat számol fel hello virtuális gép mérete és az operációs rendszer alapján. A megkezdett órák esetében az Azure csak hello percig használati díja. Ha egy Virtuálisgép-lemezkép bizonyos előre telepített szoftvert tartalmazó hello VM hoz létre, további óránkénti szoftver költségek merülhetnek fel. Azure percalapú külön hello virtuális gép operációs rendszer és adatlemezek tárolására. Az ideiglenes lemezes tárolás ingyenes.

Ha virtuális gép állapotának hello futó vagy leállt, de nem van szó hello virtuális gép állapotának leállításakor számítjuk fel (deszerializálni lefoglalt). tooput hello a virtuális gép leállított (való hozzárendelése) állapotához, hello a következőket:

* Állítsa le, illetve törölhet hello VM hello a klasszikus Azure portálon.
* Hello Stop-AzureVM parancsmagot használhatja hello Azure PowerShell modul érhető el.
* Hello leállítási szerepkör művelet található hello szolgáltatásfelügyelet REST API, és adja meg a StoppedDeallocated hello PostShutdownAction elemhez.

További információ: [A Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="will-azure-reboot-my-vm-for-maintenance"></a>Az Azure újraindítja a virtuális gépet karbantartás céljából?
Azure néha újraindítja a virtuális gép hello Azure adatközpontjaiban frissítések rendszeres, tervezett karbantartás részeként.

Nem tervezett karbantartási események előfordulhatnak, ha az Azure komoly hardveres problémát észlel, amely befolyásolja a virtuális gépét. Nem tervezett események Azure automatikusan áttelepíti hello VM tooa kifogástalan állapotú gazdagép és hello virtuális gép újraindul.

Minden különálló virtuális gép (azaz hello virtuális gép nem részei egy rendelkezésre állási csoportnak), Azure értesíti hello előfizetés szolgáltatás-rendszergazda e-mailben tervezett karbantartás előtt legalább egy héttel mert hello virtuális gépek sikerült újraindítani hello frissítése közben. Hello virtuális gépeken futó alkalmazások sikerült leállás következik be.

Is használhatja hello a klasszikus Azure portálon vagy az Azure PowerShell tooview hello újraindítás naplók amikor hello újraindítás tooplanned karbantartás miatt történt. További részletek: [Virtuális gépek újraindítási naplóinak megtekintése](https://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/).

tooprovide redundancia két put vagy hasonló módon konfigurált virtuális gépek hello azonos rendelkezésre állási csoportot. Ezzel biztosítja, hogy legalább egy virtuális gép elérhető maradjon a tervezett vagy nem tervezett karbantartás alatt. Az Azure garantálja a virtuális gépek bizonyos szintű rendelkezésre állását ebben a konfigurációban. További információkért lásd: [hello virtuális gépek rendelkezésre állásának kezelése](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="additional-resources"></a>További források
[Az Azure-beli virtuális gépek bemutatása](../articles/virtual-machines/virtual-machines-linux-about.md)

[Hozzon létre, és a Linux virtuális gépek kezelése a hello Azure parancssori felület](../articles/virtual-machines/linux/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Windows rendszerű virtuális gépek létrehozása és felügyelete Azure PowerShell-lel ](../articles/virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

