Ez a cikk ismerteti a bevált gyakorlatok a futó Windows virtuális gépek (VM) készlete Azure, a méretezhetőséget, a rendelkezésre állási, a kezelhetőségi és a biztonsági figyelmet.

> [!NOTE]
> Azure két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager] [ resource-manager-overview] és klasszikus. Ez a cikk a Microsoft által az új üzembe helyezésekhez javasolt Resource Managert használja.
>
>

Nem javasoljuk, hogy egyetlen virtuális gépet használjon kritikus fontosságú számítási feladatokhoz, mert ez egyetlen hibaérzékeny pontot hoz létre. A magasabb rendelkezésre állás érdekében helyezzen üzembe több virtuális gépet egy [rendelkezésre állási csoportban][availability-set]. További információkért tekintse meg a [több virtuális gép Azure-on való futtatását][multi-vm] ismertető szakaszt.

## <a name="architecture-diagram"></a>Architektúradiagram

A virtuális gépek Azure-beli üzembe helyezése során nem csak magára a virtuális gépre van szükség. Számítási, hálózati és tárolási eleme van.

> A [Microsoft letöltőközpontból][visio-download] letölthető egy Visio dokumentum, amely tartalmazza ezt az architektúradiagramot. A diagram a „Számítás – egy VM” oldalon található.
>
>

![[0]][0]

* **Erőforráscsoport.** Az [*erőforráscsoport*][resource-manager-overview] egy tároló, amely kapcsolódó erőforrásokat tárol. Hozzon létre egy erőforráscsoportot, amely a virtuális géphez kapcsolódó erőforrásokat tárolja.
* **VM**. A virtuális gépet üzembe helyezheti a közzétett rendszerképek listájáról, vagy egy, az Azure Blob Storage-ba feltöltött virtuálismerevlemez-fájlból (VHD).
* **Operációsrendszer-lemez.** Az operációsrendszer-lemez egy, az [Azure Storage-ban][azure-storage] tárolt VHD. Ez azt jelenti, hogy akkor is működik, ha a gazdagép leáll.
* **Ideiglenes lemez.** A virtuális gép létrejön egy ideiglenes lemezt (a `D:` Windows meghajtóján). A lemez a gazdagép egyik fizikai meghajtóján található. *Nincs* mentve az Azure Storage-ban, és előfordulhat, hogy törlődik az újraindítások és más VM-életciklusesemények során. Ez a lemez csak ideiglenes adatokat, például lapozófájlokat tárol.
* **Adatlemezek.** Az [adatlemez][data-disk] egy állandó, az alkalmazásadatokhoz használt VHD. Az adatlemezeket (pl. az operációsrendszer-lemezt) az Azure Storage tárolja.
* **Virtuális hálózat (VNet) és alhálózat.** Az Azure-ban minden VM egy alhálózatokra osztott virtuális hálózatban van üzembe helyezve.
* **Nyilvános IP-cím.** Egy nyilvános IP-cím van szükség a virtuális gép kommunikálni&mdash;például a távoli asztal (RDP) keresztül.
* **Hálózati adapter (NIC)**. A hálózati adapter teszi lehetővé a virtuális gép számára a virtuális hálózattal való kommunikációt.
* **Hálózati biztonsági csoport (NSG)**. Az [NSG][nsg] segítségével lehet engedélyezni vagy letiltani az alhálózat felé irányuló hálózati forgalmat. Egy NSG-t társíthat egy különálló hálózati adapterhez vagy egy alhálózathoz. Ha egy alhálózathoz társítja, az NSG-szabályok az alhálózat összes virtuális gépére vonatkoznak.
* **Diagnosztika.** A diagnosztikai naplózás létfontosságú a virtuális gép kezeléséhez és hibaelhárításához.

## <a name="recommendations"></a>Javaslatok

Az alábbi javaslatok a legtöbb forgatókönyvre vonatkoznak. Kövesse ezeket a javaslatokat, ha nincsenek ezeket felülíró követelményei.

### <a name="vm-recommendations"></a>Virtuális gépekre vonatkozó javaslatok

Az Azure számos különféle virtuálisgép-méretet biztosít, de mi a DS és a GS sorozatot ajánljuk, mert ezek a gépméretek támogatják a [Premium Storage][premium-storage] tárolást. Válassza ezen gépméretek egyikét, kivéve, ha speciális számítási feladatokhoz, például nagy teljesítményű feldolgozáshoz kívánja a gépeket használni. Részletekért tekintse meg a [virtuálisgép-méretekkel kapcsolatos][virtual-machine-sizes] szakaszt.

Ha meglévő számítási feladatot helyez át az Azure-ba, kezdetnek azt a virtuálisgép-méretet válassza, amely a leginkább egyezik a helyszíni kiszolgálói méretével. Ezután mérje meg a valós számítási feladat teljesítményét a CPU, a memória és a lemez másodpercenkénti bemeneti/kimeneti műveletei (IOPS) figyelembe vételével, és módosítsa a méretet, ha szükséges. Ha több hálózati adapterre van szükség a virtuális géphez, vegye figyelembe, hogy a hálózati adapterek maximális száma a [virtuálisgép-méret][vm-size-tables] függvénye.   

Amikor üzembe helyezi a virtuális gépet és az egyéb erőforrásokat, meg kell adnia egy régiót. A legjobb megoldás, ha a belső felhasználóihoz vagy ügyfeleihez legközelebb eső régiót választja. Azonban nem minden Virtuálisgép-méretek érhetők el minden régióban. További információkért lásd: [régiói][services-by-region]. A Virtuálisgép-méretek érhető el egy adott régióban listájának megtekintéséhez a következő parancsot az Azure parancssori felület (CLI):

```
azure vm sizes --location <location>
```

További információ a közzétett Virtuálisgép-lemezkép kiválasztása: [keresse meg és jelölje be a Windows virtuális gép képfájljait Azure Powershell vagy a CLI-][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>A lemezre és a tárolásra vonatkozó javaslatok

A legjobb lemezes i/o-teljesítmény érdekében javasoljuk [prémium szintű Storage][premium-storage], amely tárolja az adatokat a tartós állapotú meghajtót (SSD). A költség az üzembe helyezett lemez méretétől függően változik. Iops-érték és az átvitel is függ a lemez mérete, így amikor egy lemezt, tényezőket kell figyelembe venni az összes három (kapacitás, IOPS és átviteli).

Hozzon létre külön Azure Storage-fiókot minden virtuális géphez a virtuális merevlemezek (VHD-k) tárolására, hogy elkerülje az IOPS-korlátok elérését a tárfiókban.

Adjon hozzá egy vagy több adatlemezt. Amikor létrehoz egy új virtuális Merevlemezt, akkor formázatlan. Jelentkezzen be a virtuális gépek a formázza a lemezt. Ha nagy számú adatlemezzel rendelkezik, vegye figyelembe a tárfiók teljes I/O-korlátját. További információkért lásd a [virtuálisgép-lemez korlátaival kapcsolatos][vm-disk-limits] szakaszt.

Ha lehetséges, alkalmazásokat telepít a adatlemezt, nem az operációsrendszer-lemezképet. Azonban néhány örökölt alkalmazás-összetevők telepítéséhez a C: meghajtón módosítania kell. Ebben az esetben is [az operációsrendszer-lemez átméretezése] [ resize-os-disk] PowerShell használatával.

A legjobb teljesítmény érdekében hozzon létre egy önálló tárfiókot a diagnosztikai naplók tárolására. Egy standard helyileg redundáns tárolási (LRS) fiók elegendő a diagnosztikai naplókhoz.

### <a name="network-recommendations"></a>Hálózatokra vonatkozó javaslatok

A nyilvános IP-cím lehet dinamikus vagy statikus. Alapértelmezés szerint dinamikus.

* Akkor foglaljon le [statikus IP-címet][static-ip], ha rögzített, nem módosuló IP-címre van szüksége – például ha egy A rekordot kell létrehoznia a DNS-ben, vagy ha hozzá kell adnia az IP-címet a biztonságos elemek listájához.&mdash;
* Létrehozhat egy teljes tartománynevet (FQDN) is az IP-címhez. Ezután a DNS-ben regisztrálhat egy, az FQDN-re mutató [CNAME rekordot][cname-record]. További információért tekintse meg a [teljes tartománynév az Azure Portalon való létrehozásával kapcsolatos][fqdn] szakaszt.

Minden NSG tartalmaz egy [alapértelmezett szabálykészletet][nsg-default-rules], amelyben szerepel egy minden bejövő internetes forgalmat blokkoló szabály. Az alapértelmezett szabályok nem törölhetők, azonban más szabályokkal felülírhatók. Az internetes forgalom engedélyezéséhez hozzon létre olyan szabályokat, amelyek adott portokon engedélyezik a bejövő forgalmat – például a HTTP-hez a 80-as porton.&mdash;  

RDP engedélyezéséhez vegyen fel egy NSG-t, amely lehetővé teszi a bejövő forgalom 3389-es TCP-port.

## <a name="scalability-considerations"></a>Méretezési szempontok

Méretezheti a virtuális gépek felfelé vagy lefelé által [módosítása a Virtuálisgép-méretet](../articles/virtual-machines/windows/sizes.md). Horizontális felskálázáshoz helyezzen két vagy több virtuális gépet egy rendelkezésre állási csoportba egy terheléselosztó mögé. További információkért lásd: [több virtuális gépek Azure-on futó, a méretezhetőség és a rendelkezésre állási][multi-vm].

## <a name="availability-considerations"></a>Rendelkezésre állási szempontok

A magasabb rendelkezésre állás érdekében helyezzen üzembe több virtuális gépet egy rendelkezésre állási csoportban. Ez is biztosít egy magasabb [szolgáltatói szerződést] [ vm-sla] (SLA).

A virtuális gépre hatással lehet egy [tervezett karbantartás][planned-maintenance] vagy egy [nem tervezett karbantartás][manage-vm-availability]. Annak megállapításához, hogy a virtuális gép újraindítását egy tervezett karbantartás okozta-e, használja a [virtuális gépek újraindítási naplóit][reboot-logs].

A VHD-ket a rendszer az [Azure Storage][azure-storage]-ban tárolja, és az Azure Storage a tartósság és a rendelkezésre állás érdekében replikálva van.

A normál műveletek során történő véletlen (például hiba miatti) adatvesztés elleni védelem érdekében érdemes időponthoz kötött biztonsági mentéseket is megvalósítani [blob-pillanatképekkel][blob-snapshot] vagy más eszközzel.

## <a name="manageability-considerations"></a>Felügyeleti szempontok

**Erőforráscsoportok.** Szorosan erőforrásokat, amelyek azonos életciklusának osztani azonos PUT [erőforráscsoport][resource-manager-overview]. Erőforráscsoportok üzembe helyezését és megfigyelje az erőforrásokat csoportként és számlázási költségek erőforráscsoport szerint összesítő teszik lehetővé. Az erőforrásokat törölheti is készletenként. Ez nagyon hasznos tesztkörnyezetek esetében. Az erőforrásoknak adjon kifejező nevet, így könnyebb megtalálni egy adott erőforrást és megérteni a szerepét. Lásd: [Az Azure-erőforrások ajánlott elnevezési konvenciói][naming conventions].

**Virtuális gépek diagnosztikája.** Engedélyezze a megfigyelést és a diagnosztikát, beleértve az alapvető állapotmetrikákat, a diagnosztikai infrastruktúra naplófájljait és a [rendszerindítási diagnosztikát][boot-diagnostics]. Rendszerindítási diagnosztika segíthetnek diagnosztizálni rendszerindítási hiba, ha a virtuális gép lekérdezi a nonbootable állapot. További információkat [a megfigyelés és a diagnosztika engedélyezésével kapcsolatos][enable-monitoring] szakaszban találhat. Használja a [Azure Naplógyűjtést] [ log-collector] Azure platformon gyűjtését, és feltölti azokat az Azure storage bővítmény.   

A diagnosztika a következő parancssori felületi paranccsal engedélyezhető:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Virtuális gépek leállítása.** Az Azure különbséget tesz a „leállított” és a „felszabadított” állapot között. A leállított virtuális gépek után fizetni kell, a felszabadítottak után azonban nem.

Virtuális gép felszabadításához használja az alábbi parancssori felületi parancsot:

```
azure vm deallocate <resource-group> <vm-name>
```

Az Azure Portalon a **Leállítás** gombbal szabadítható fel a virtuális gép. Ha azonban az operációs rendszerből állítja le, amikor be van jelentkezve, azzal virtuális gépet leállítja, de *nem* szabadítja fel, tehát továbbra is fizetnie kell a díját.

**Virtuális gépek törlése.** Ha töröl egy virtuális gépet, a VHD-k nem törlődnek. Ez azt jelenti, hogy biztonságosan törölheti a virtuális gépet anélkül, hogy adatot vesztene. A tárolásért azonban továbbra is díjat kell fizetnie. A VHD törléséhez törölje a fájlt a [Blob Storage-ból][blob-storage].

A véletlen törlés megelőzése érdekében használjon [erőforrászárat][resource-lock]. Ezzel zárolhat egy egész erőforráscsoportot, vagy egyes erőforrásokat, például a virtuális gépet.

## <a name="security-considerations"></a>Biztonsági szempontok

Használjon [az Azure Security Center] [ security-center] való központi láthatja az Azure-erőforrások biztonsági állapotát. A Security Center a potenciális biztonsági problémákat figyeli, és biztonsági állapotát a központi telepítés átfogó képet nyújt. A Security Center / Azure-előfizetés úgy van beállítva. Biztonsági adatok gyűjtésének engedélyezése a [a Security Center használata]. Adatgyűjtés engedélyezésekor a rendszer a Security Center automatikusan ellenőrzi a bármely adott előfizetésen belül létrehozott virtuális gépek.

**Javítás kezelése.** Ha engedélyezve van, a Security Center ellenőrzi, hogy biztonsági és kritikus frissítések hiányoznak. Használjon [csoportházirend-beállítások] [ group-policy] automatikus rendszer-frissítések engedélyezéséhez a virtuális gépen.

**Kártevőirtó.** Ha engedélyezve van, a Security Center ellenőrzi, hogy telepítve van-e a kártevőirtó szoftver. A Security Center az Azure-portálon belül a kártevőirtó szoftver telepítéséhez is használható.

**Műveletek.** A [szerepköralapú hozzáférés-vezérléssel][rbac] (RBAC) szabályozható az üzembe helyezett Azure-erőforrásokhoz való hozzáférés. Az RBAC lehetővé teszi, hogy engedélyezési szerepköröket rendeljen a fejlesztő és üzemeltető csapata tagjaihoz. Az Olvasó szerepkör például áttekintheti az Azure-erőforrásokat, de nem hozhatja létre, nem kezelheti és nem törölheti őket. Bizonyos szerepkörök kifejezetten egy adott Azure-erőforrástípusra jellemzők. Például a virtuális gép közreműködő szerepkört is indítsa újra a virtuális gép felszabadítása, a rendszergazdai jelszó visszaállítása, hozzon létre egy új virtuális Gépet, illetve stb. Ehhez a referenciaarchitektúrához hasznos lehet még a [DevTest Labs-felhasználó][rbac-devtest] és a [Hálózati közreműködő][rbac-network] [beépített RBAC-szerepkör][rbac-roles]. Egy felhasználóhoz több szerepkört is rendelhető, és létrehozhat egyéni szerepköröket a még részletesebb engedélyek érdekében.

> [!NOTE]
> Az RBAC nem korlátozza a virtuális gépre bejelentkezett felhasználó által végezhető műveleteket. Azokat az engedélyeket a vendég operációs rendszeren lévő fiók típusa határozza meg.   
>
>

A helyi rendszergazdai jelszó visszaállítása, futtassa a `vm reset-access` Azure CLI parancsot.

```
azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

A [vizsgálati naplók][audit-logs] segítségével megtekintheti az üzembe helyezési műveleteket és más virtuálisgép-eseményeket.

**Adattitkosítás.** Ha titkosítania kell az operációs rendszert és az adatlemezeket, érdemes megfontolnia az [Azure Disk Encryption][disk-encryption] használatát.

## <a name="solution-deployment"></a>A megoldás üzembe helyezése

A központi telepítés a referenciaarchitektúra megtalálható [GitHub][github-folder]. Tartalmaz egy virtuális hálózatot, egy NSG-t és egy virtuális gépet. Az architektúra üzembe helyezéséhez kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal a lenti gombra, és válassza a „Link megnyitása új lapon” vagy a „Link megnyitása új ablakban” lehetőséget.  
   [![Üzembe helyezés az Azure-ban](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. Ha a hivatkozás megnyílt az Azure Portalon, meg kell adnia néhány beállítás értékét:

   * Az **Erőforráscsoport** neve már meg van adva a paraméterfájlban, ezért válassza az **Új létrehozása** lehetőséget és a szövegmezőbe írja az `ra-single-vm-rg` karakterláncot.
   * Válassza ki a régiót a **Hely** legördülő listából.
   * Ne szerkessze a **Sablon gyökér szintű URI-je** vagy a **Paraméter gyökér szintű URI-je** szövegmezőt.
   * Válassza ki **windows** a a **operációsrendszer-típus** legördülő listája.
   * Tekintse át a használati feltételeket, majd kattintson az **Elfogadom a fenti feltételeket** lehetőségre.
   * Kattintson a **Vásárlás** gombra.
3. Várjon, amíg az üzembe helyezés befejeződik.
4. A paraméter-fájlok közé tartoznak, a kódolt rendszergazda felhasználónevet és jelszót, és erősen ajánlott, hogy azonnal módosíthatja is. Kattintson az Azure Portalon az `ra-single-vm0 ` nevű virtuális gépre. Kattintson a **jelszó-átállítási** a a **támogatási + hibaelhárítási** panelen. A **Mód** legördülő listában válassza a **Jelszó alaphelyzetbe állítása** lehetőséget, majd adjon meg új értéket a **Felhasználónév** és a **Jelszó** mezőben. Az új felhasználó nevének és jelszavának megőrzéséhez kattintson a **Frissítés** gombra.

Kapcsolatban további módon telepíthet a referencia-architektúrában információk, az információs fájl a a [útmutatást-single-vm][github-folder]] GitHub mappát.

## <a name="customize-the-deployment"></a>A központi telepítés testreszabása
Ha módosítania kell a központi telepítés az igényeinek megfelelő, kövesse az utasításokat a a [információs][github-folder].

## <a name="next-steps"></a>Következő lépések
A magasabb rendelkezésre állás érdekében helyezzen üzembe két vagy több virtuális gépet egy terheléselosztó mögött. További információkért tekintse meg a [több virtuális gép Azure-on való futtatását][multi-vm] ismertető szakaszt.

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]:../articles/virtual-machines/windows/tutorial-availability-sets.md
[azure-cli]: /cli/azure/get-started-with-az-cli2
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/storage/storage-about-disks-and-vhds-windows.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]:../articles/virtual-machines/windows/portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]:../articles/virtual-machines/windows/manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]:../articles/virtual-machines/windows/planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-labs-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]:../articles/virtual-machines/windows/expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]:../articles/virtual-machines/windows/cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-account-limits]: ../articles/azure-subscription-service-limits.md#storage-limits
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[a Security Center használata]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/windows/sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]:../articles/virtual-machines/linux/change-vm-size.md
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[vm-size-tables]: ../articles/virtual-machines/windows/sizes.md
[0]: ./media/guidance-blueprints/compute-single-vm.png "Az Azure-ban egy Windows virtuális gép architektúrája"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
