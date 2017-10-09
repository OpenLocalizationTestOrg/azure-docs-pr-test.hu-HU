Ez a cikk ismerteti a Windows rendszerű virtuális gép (VM) Azure-on futó, fizető figyelmet tooscalability, a rendelkezésre állási, a kezelhetőségi és a biztonsági bevált gyakorlatok csoportja.

> [!NOTE]
> Azure két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager] [ resource-manager-overview] és klasszikus. Ez a cikk a Microsoft által az új üzembe helyezésekhez javasolt Resource Managert használja.
>
>

Nem javasoljuk, hogy egyetlen virtuális gépet használjon kritikus fontosságú számítási feladatokhoz, mert ez egyetlen hibaérzékeny pontot hoz létre. A magasabb rendelkezésre állás érdekében helyezzen üzembe több virtuális gépet egy [rendelkezésre állási csoportban][availability-set]. További információkért tekintse meg a [több virtuális gép Azure-on való futtatását][multi-vm] ismertető szakaszt.

## <a name="architecture-diagram"></a>Architektúradiagram

Egy Azure-ban kiépítés magában foglalja a mint csak a virtuális gépért hello több mozgó elemeket. Számítási, hálózati és tárolási eleme van.

> A Visio dokumentum, amely tartalmazza az architektúra diagramja hello letölthető [Microsoft letöltőközpontból][visio-download]. Ez az ábra megtalálható hello "Számítási - egyetlen virtuális gép" lapon.
>
>

![[0]][0]

* **Erőforráscsoport.** Az [*erőforráscsoport*][resource-manager-overview] egy tároló, amely kapcsolódó erőforrásokat tárol. Csoport toohold hello erőforrások az erőforrás létrehozása a virtuális gép.
* **VM**. Megadhat egy virtuális Gépet, a közzétett képek listáját, illetve, hogy tooAzure blobtárolóba feltöltött virtuális merevlemez (VHD) fájl.
* **Operációsrendszer-lemez.** hello operációsrendszer-lemez a tárolt virtuális merevlemez [Azure Storage][azure-storage]. Ez azt jelenti, hogy az továbbra is fennáll, akkor is, ha hello gazdaszámítógépen leáll.
* **Ideiglenes lemez.** hello virtuális gép létrehozása egy ideiglenes lemezzel (hello `D:` Windows meghajtóján). Ezt a lemezt egy fizikai meghajtóhoz, hello gazdagépen tárolja. *Nincs* mentve az Azure Storage-ban, és előfordulhat, hogy törlődik az újraindítások és más VM-életciklusesemények során. Ez a lemez csak ideiglenes adatokat, például lapozófájlokat tárol.
* **Adatlemezek.** Az [adatlemez][data-disk] egy állandó, az alkalmazásadatokhoz használt VHD. Adatlemezek Azure Storage, mint az operációs rendszer hello lemezt tárolja.
* **Virtuális hálózat (VNet) és alhálózat.** Az Azure-ban minden VM egy alhálózatokra osztott virtuális hálózatban van üzembe helyezve.
* **Nyilvános IP-cím.** A nyilvános IP-cím a virtuális gép hello szükséges toocommunicate&mdash;például a távoli asztal (RDP) keresztül.
* **Hálózati adapter (NIC)**. a hálózati adapter hello lehetővé teszi, hogy a hello VM toocommunicate hello virtuális hálózattal.
* **Hálózati biztonsági csoport (NSG)**. Hello [NSG] [ nsg] használt tooallow/megtagadási hálózati forgalom toohello alhálózat. Egy NSG-t társíthat egy különálló hálózati adapterhez vagy egy alhálózathoz. Ha hozzá kell rendelni egy alhálózatot, hello NSG-szabályok érvényesek tooall virtuális gépek alhálózat.
* **Diagnosztika.** Diagnosztikai naplózás kezelése és a virtuális gép hello hibaelhárítás elengedhetetlen.

## <a name="recommendations"></a>Javaslatok

a következő ajánlások hello alkalmazni a legtöbb forgatókönyvhöz. Kövesse ezeket a javaslatokat, ha nincsenek ezeket felülíró követelményei.

### <a name="vm-recommendations"></a>Virtuális gépekre vonatkozó javaslatok

Azure lehetővé teszi a sok különböző virtuális gépek méretét, de hello DS - és GS-méretek azt javasoljuk, mert ezek méreteket támogatja [prémium szintű Storage][premium-storage]. Válassza ezen gépméretek egyikét, kivéve, ha speciális számítási feladatokhoz, például nagy teljesítményű feldolgozáshoz kívánja a gépeket használni. Részletekért tekintse meg a [virtuálisgép-méretekkel kapcsolatos][virtual-machine-sizes] szakaszt.

Ha egy meglévő munkaterhelés tooAzure, hello Virtuálisgép-méretet, amely hello legközelebbi egyezés tooyour útmutató a helyszíni kiszolgálók. Ezután mérték hello teljesítményétől, a tényleges munkaterhelés tiszteletben tartják tooCPU, a memória és a lemezek bemeneti/kimeneti műveletek száma másodpercenként (IOPS), és szükség esetén állítsa be a hello méretét. Ha több hálózati adapter van szüksége a virtuális gép számára, vegye figyelembe, hogy a hálózati adapterek maximális száma hello hello függvénye [Virtuálisgép-méretet][vm-size-tables].   

Ha virtuális gép hello és más erőforrások, meg kell adnia egy régiót. Általában, válasszon egy régiót legközelebbi tooyour belső felhasználók vagy az ügyfelek. Azonban nem minden Virtuálisgép-méretek érhetők el minden régióban. További információkért lásd: [régiói][services-by-region]. toosee hello Virtuálisgép-méretek érhető el egy adott régióban, futtassa a következő Azure parancssori felület (CLI) parancs hello listáját:

```
azure vm sizes --location <location>
```

További információ a közzétett Virtuálisgép-lemezkép kiválasztása: [keresse meg és jelölje be a Windows virtuális gép képfájljait Azure Powershell vagy a CLI-][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>A lemezre és a tárolásra vonatkozó javaslatok

A legjobb lemezes i/o-teljesítmény érdekében javasoljuk [prémium szintű Storage][premium-storage], amely tárolja az adatokat a tartós állapotú meghajtót (SSD). Költség hello kiépített lemez méretének hello alapul. Iops-érték és az átvitel is függ a lemez mérete, így amikor egy lemezt, tényezőket kell figyelembe venni az összes három (kapacitás, IOPS és átviteli).

Hozzon létre külön az Azure storage-fiókok minden virtuális gép toohold hello virtuális merevlemezek (VHD) sorrendben tooavoid szerezze meg a tárfiókok korlátairól hello iops-érték.

Adjon hozzá egy vagy több adatlemezt. Amikor létrehoz egy új virtuális Merevlemezt, akkor formázatlan. Jelentkezzen be hello VM tooformat hello lemez. Ha nagyszámú adatlemezek, figyelembe hello teljes i/o határértékeinek hello tárfiók. További információkért lásd a [virtuálisgép-lemez korlátaival kapcsolatos][vm-disk-limits] szakaszt.

Ha lehetséges, alkalmazásokat telepít a adatlemezt, nem hello operációsrendszer-lemezzel. Előfordulhat azonban, hogy néhány örökölt alkalmazást tooinstall-összetevők a C: meghajtó hello kell. Ebben az esetben is [hello operációsrendszer-lemez átméretezése] [ resize-os-disk] PowerShell használatával.

A legjobb teljesítmény érdekében hozzon létre egy külön tárfiókot toohold diagnosztikai naplókat. Egy standard helyileg redundáns tárolási (LRS) fiók elegendő a diagnosztikai naplókhoz.

### <a name="network-recommendations"></a>Hálózatokra vonatkozó javaslatok

hello nyilvános IP-cím dinamikus vagy statikus lehet. hello alapértelmezés szerint dinamikus.

* Tartalék egy [statikus IP-cím] [ static-ip] szüksége van-e a rögzített IP-címet, amely nem változik &mdash; például, ha toocreate kell egy A rekordot a DNS vagy kell hello IP-címek toobe hozzáadott tooa biztonságos listájából.
* Egy teljesen minősített tartománynevét (FQDN) hello IP-címet is létrehozhat. Ezután rögzítheti egy [CNAME rekord] [ cname-record] a DNS-ben, amely toohello FQDN mutat. További információkért lásd: [hello Azure-portálon hozzon létre egy teljesen minősített tartománynevét][fqdn].

Minden NSG tartalmaz egy [alapértelmezett szabálykészletet][nsg-default-rules], amelyben szerepel egy minden bejövő internetes forgalmat blokkoló szabály. hello alapértelmezett szabályokat nem lehet törölni, de más szabályok is felülírja azt. tooenable internetes forgalmat, hozzon létre szabályokat, amelyek engedélyezik a bejövő forgalmat toospecific portok &mdash; például a HTTP a 80-as porton.  

tooenable RDP, vegyen fel egy NSG-t, amely lehetővé teszi, hogy a bejövő forgalom tooTCP 3389-es portot.

## <a name="scalability-considerations"></a>Méretezési szempontok

Méretezheti a virtuális gépek felfelé vagy lefelé által [hello virtuális gép méretének módosítása](../articles/virtual-machines/windows/sizes.md). kimenő tooscale vízszintesen, üzembe két vagy több virtuális gépek rendelkezésre állási készlet egy terheléselosztó mögött. További információkért lásd: [több virtuális gépek Azure-on futó, a méretezhetőség és a rendelkezésre állási][multi-vm].

## <a name="availability-considerations"></a>Rendelkezésre állási szempontok

A magasabb rendelkezésre állás érdekében helyezzen üzembe több virtuális gépet egy rendelkezésre állási csoportban. Ez is biztosít egy magasabb [szolgáltatói szerződést] [ vm-sla] (SLA).

A virtuális gépre hatással lehet egy [tervezett karbantartás][planned-maintenance] vagy egy [nem tervezett karbantartás][manage-vm-availability]. Használhat [virtuális gép újraindítási naplók] [ reboot-logs] toodetermine, hogy indítsa újra a virtuális gépek tervezett karbantartás okozta.

A VHD-ket a rendszer az [Azure Storage][azure-storage]-ban tárolja, és az Azure Storage a tartósság és a rendelkezésre állás érdekében replikálva van.

tooprotect véletlen adatvesztéstől (például hibás felhasználói) miatt a normál működés során, akkor is inkább időpontban – a biztonsági mentéseket, [blob-pillanatképek] [ blob-snapshot] vagy egy másik eszköz.

## <a name="manageability-considerations"></a>Felügyeleti szempontok

**Erőforráscsoportok.** Szorosan erőforrásaikat, hogy ugyanazon élettartama ciklus történő megosztás hello hello azonos [erőforráscsoport][resource-manager-overview]. Erőforráscsoportok lehetővé teszik toodeploy és a figyelő erőforrásokat csoportként és számlázási költségek erőforráscsoport szerint összesítő. Az erőforrásokat törölheti is készletenként. Ez nagyon hasznos tesztkörnyezetek esetében. Az erőforrásoknak adjon kifejező nevet, Amely lehetővé teszi könnyebb toolocate egy adott erőforrás, és a szerepkör ismertetése. Lásd: [Az Azure-erőforrások ajánlott elnevezési konvenciói][naming conventions].

**Virtuális gépek diagnosztikája.** Engedélyezze a megfigyelést és a diagnosztikát, beleértve az alapvető állapotmetrikákat, a diagnosztikai infrastruktúra naplófájljait és a [rendszerindítási diagnosztikát][boot-diagnostics]. Rendszerindítási diagnosztika segíthetnek diagnosztizálni rendszerindítási hiba, ha a virtuális gép lekérdezi a nonbootable állapot. További információkat [a megfigyelés és a diagnosztika engedélyezésével kapcsolatos][enable-monitoring] szakaszban találhat. Használjon hello [Azure Naplógyűjtést] [ log-collector] bővítmény toocollect Azure platformon naplózza, és feltöltheti ezeket tooAzure tárolási.   

a következő parancsot a CLI hello lehetővé teszi, hogy a diagnosztika:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Virtuális gépek leállítása.** Az Azure különbséget tesz a „leállított” és a „felszabadított” állapot között. Virtuális gép állapotának hello le van állítva, de nem felszabadított virtuális gép hello esetén van szó.

A következő parancssori parancsot toodeallocate egy virtuális gép hello használata:

```
azure vm deallocate <resource-group> <vm-name>
```

Hello Azure-portálon, a hello **leállítása** gomb felszabadítja a virtuális gép hello. Azonban ha leállítja az operációs rendszer bejelentkezett hello keresztül, hello virtuális gép le van állítva, de *nem* felszabadítása. lehetséges, így továbbra is fizetnie kell.

**Virtuális gépek törlése.** Ha töröl egy virtuális Gépet, hello VHD-k nem törlődnek. Ez azt jelenti, hogy biztonságosan törölhető hello VM adatok elvesztése nélkül. A tárolásért azonban továbbra is díjat kell fizetnie. toodelete hello VHD, törölje a hello fájlt [Blob-tároló][blob-storage].

tooprevent véletlen törlés, használja a [erőforrás zárolási] [ resource-lock] toolock hello teljes erőforrás csoport vagy zárolási egyéni erőforrások, például hello virtuális gép.

## <a name="security-considerations"></a>Biztonsági szempontok

Használjon [az Azure Security Center] [ security-center] tooget az Azure-erőforrások biztonsági állapotának hello központi nézetét. A Security Center a potenciális biztonsági problémákat figyeli, és hello biztonsági állapotát a központi telepítés átfogó képet nyújt. A Security Center / Azure-előfizetés úgy van beállítva. Biztonsági adatok gyűjtésének engedélyezése a [a Security Center használata]. Adatgyűjtés engedélyezésekor a rendszer a Security Center automatikusan ellenőrzi a bármely adott előfizetésen belül létrehozott virtuális gépek.

**Javítás kezelése.** Ha engedélyezve van, a Security Center ellenőrzi, hogy biztonsági és kritikus frissítések hiányoznak. Használjon [csoportházirend-beállítások] [ group-policy] hello VM tooenable automatikus rendszer-frissítések.

**Kártevőirtó.** Ha engedélyezve van, a Security Center ellenőrzi, hogy telepítve van-e a kártevőirtó szoftver. Is használhatja a Security Center tooinstall kártevőirtó szoftverek belül hello Azure-portálon.

**Műveletek.** Használjon [szerepköralapú hozzáférés-vezérlés] [ rbac] (RBAC) toocontrol hozzáférés toohello Azure üzembe helyezett erőforrások. Az RBAC rendelhet engedélyezési szerepkörök toomembers a DevOps csoport. Például hello olvasó szerepkört is Azure-erőforrások megtekintése, de nem létrehozása, kezelése vagy törölje őket. Egyes szerepkörök olyan konkrét tooparticular Azure-erőforrás. Például hello virtuális gép közreműködő szerepkört is indítsa újra a virtuális gép felszabadítása, hello rendszergazdai jelszó visszaállítása, hozzon létre egy új virtuális Gépet, illetve stb. Ehhez a referenciaarchitektúrához hasznos lehet még a [DevTest Labs-felhasználó][rbac-devtest] és a [Hálózati közreműködő][rbac-network] [beépített RBAC-szerepkör][rbac-roles]. A felhasználó toomultiple szerepkörök rendelhetők, és még több részletes engedélyeket egyéni szerepköröket is létrehozhat.

> [!NOTE]
> Az RBAC nem korlátozza a hello műveleteket hajthat végre a felhasználó bejelentkezik egy virtuális Gépet. Ezeket az engedélyeket hello vendég operációs rendszer hello fiók típusa határozza meg.   
>
>

tooreset hello helyi rendszergazda jelszavát, futtassa a hello `vm reset-access` Azure CLI parancsot.

```
azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Használjon [naplók] [ audit-logs] toosee kiépítési műveletek és a virtuális gép az eseményeket.

**Adattitkosítás.** Érdemes lehet [Azure Disk Encryption] [ disk-encryption] Ha tooencrypt hello az operációs rendszer és adatlemezek van szükség.

## <a name="solution-deployment"></a>A megoldás üzembe helyezése

A központi telepítés a referenciaarchitektúra megtalálható [GitHub][github-folder]. Tartalmaz egy virtuális hálózatot, egy NSG-t és egy virtuális gépet. toodeploy hello architektúra, kövesse az alábbi lépéseket:

1. Kattintson a jobb gombbal az alábbi hello gombra, jelölje ki vagy "nyitott kapcsolatot új lapon" vagy "Hivatkozás megnyitása új ablak."  
   [![TooAzure telepítése](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. Hello hivatkozás hello Azure-portálon nyitotta meg, ha az egyes hello beállításai értékeket kell megadnia:

   * Hello **erőforráscsoport** neve már definiálva van hello paraméterfájl, ezért select **hozzon létre új** , és írja be `ra-single-vm-rg` hello szövegmezőben.
   * Jelölje be hello régió a hello **hely** legördülő listája.
   * Ne módosítsa a hello **sablon legfelső szintű Uri** vagy hello **paraméter legfelső szintű Uri** szövegmezőket.
   * Válassza ki **windows** a hello **operációsrendszer-típus** legördülő listája.
   * Tekintse át a hello használati feltételeket, majd kattintson az hello **toohello feltételek és kikötések fenti elfogadom** jelölőnégyzetet.
   * Kattintson a hello **beszerzési** gombra.
3. Várjon, amíg hello telepítési toocomplete.
4. hello paraméter fájlok közé tartoznak, a kódolt rendszergazda felhasználónevet és jelszót, és erősen ajánlott, hogy azonnal módosíthatja is. Kattintson a hello nevű virtuális gép `ra-single-vm0 `a hello Azure-portálon. Kattintson a **jelszó-átállítási** a hello **támogatási + hibaelhárítási** panelen. Válassza ki **jelszó-átállítási** a hello **mód** legördülő mezőben, majd válasszon ki egy új **felhasználónév** és **jelszó**. Kattintson a hello **frissítés** gomb toopersist hello új felhasználónevet és jelszót.

További lehetőségek toodeploy olvashat a architektúra hivatkoznak, tekintse meg a hello hello információs fájl [útmutatást-single-vm][github-folder]] GitHub mappát.

## <a name="customize-hello-deployment"></a>Hello telepítés testreszabása
Ha toochange hello telepítési toomatch kell az igényeinek, kövesse a hello hello utasításait [információs][github-folder].

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
