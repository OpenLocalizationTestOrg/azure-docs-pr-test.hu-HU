# <a name="frequently-asked-questions-about-classic-tooazure-resource-manager-migration"></a>Klasszikus tooAzure erőforrás-kezelő áttelepítésének kapcsolatos gyakori kérdések

## <a name="does-this-migration-plan-affect-any-of-my-existing-services-or-applications-that-run-on-azure-virtual-machines"></a>Érinti ez a migrálási terv az Azure virtuális gépeken futó meglévő szolgáltatásaimat és alkalmazásaimat? 

Nem. hello VMs (klasszikus) olyan legteljesebb körűen támogatott szolgáltatások általános rendelkezésre állási. A Folytatás toouse ezen erőforrások tooexpand a Microsoft Azure-erőforrást.

## <a name="what-happens-toomy-vms-if-i-dont-plan-on-migrating-in-hello-near-future"></a>Toomy virtuális gépek mi történik, ha szeretnék nem tervezzük áttelepítéssel a jövőbeli közelében hello? 

Azt a rendszer nem elavulttá hello létező klasszikus API-k és erőforrás modellt. Szeretnénk toomake áttelepítési könnyen elérhető hello Resource Manager üzembe helyezési modellel speciális hello figyelembe véve. Erősen ajánlott áttekinteni [néhány hello fejlesztések](../articles/azure-resource-manager/resource-manager-deployment-model.md) IaaS az erőforrás-kezelő részét képező.

## <a name="what-does-this-migration-plan-mean-for-my-existing-tooling"></a>Milyen hatással lesz a migrálási terv a meglévő eszközállományomra? 

A tooling toohello Resource Manager üzembe helyezési modellben frissítése az egyik legfontosabb módosításait hello tooaccount a rendelkezésre álló az áttelepítési terveket.

## <a name="how-long-will-hello-management-plane-downtime-be"></a>Mennyi ideig hello felügyeleti-vezérlősík állásidő lesz? 

Ez az áttelepítés alatt álló erőforrások hello száma függ. Kisebb környezetekhez (virtuális gépek közül néhány több tíz) a hello teljes áttelepítés gyorsabban kevesebb mint egy óra. Nagyobb környezetekben (virtuális gépek több száz) hello áttelepítési órát is igénybe vehet néhány.

## <a name="can-i-roll-back-after-my-migrating-resources-are-committed-in-resource-manager"></a>Visszaválthatok majd, miután a migrált erőforrások véglegesítve lettek a Resource Managerben? 

Az áttelepítés megszakítható mindaddig, amíg hello erőforrások hello előkészített állapotban vannak. Miután hello erőforrások keresztül hello véglegesítési művelet sikeresen áttelepítette a rollback utasítás nem támogatott.

## <a name="can-i-roll-back-my-migration-if-hello-commit-operation-fails"></a>Képes vagyok a áttelepítésének visszaállítása Ha hello véglegesítési művelet sikertelen? 

Áttelepítés nem állítható le, ha hello véglegesítési művelet sikertelen. Az összes áttelepítési műveletet, hello véglegesítési művelet, beleértve az idempotent. Ezért azt javasoljuk, hogy hello művelet rövid idő múlva újra. Továbbra is szembesülhetnek hiba, ha egy támogatási jegy létrehozását, vagy hozzon létre egy fórumbejegyzést hello ClassicIaaSMigration a címke a [VM fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).

## <a name="do-i-have-toobuy-another-express-route-circuit-if-i-have-toouse-iaas-under-resource-manager"></a>Van toobuy egy másik express route-körhöz Ha toouse IaaS az erőforrás-kezelő? 

Nem. Nemrég engedélyezett [ExpressRoute-Kapcsolatcsoportok áthelyezése hello klasszikus toohello Resource Manager üzembe helyezési modellben](../articles/expressroute/expressroute-move.md). Nincs toobuy egy új ExpressRoute-kapcsolatcsoportot Ha már van.

## <a name="what-if-i-had-configured-role-based-access-control-policies-for-my-classic-iaas-resources"></a>Mi a helyzet, ha szerepköralapú hozzáférés-vezérlési szabályzatokat konfiguráltam klasszikus IaaS-erőforrásaimhoz? 

Az áttelepítés során hello erőforrások klasszikus tooResource Manager alakítja át. Ezért ajánlott hello RBAC házirend-frissítési áttelepítés után toohappen igénylő megtervezni.

## <a name="i-backed-up-my-classic-vms-in-a-backup-vault-can-i-migrate-my-vms-from-classic-mode-tooresource-manager-mode-and-protect-them-in-a-recovery-services-vault"></a>Biztonsági másolatot készítettem a klasszikus virtuális gépemről egy biztonsági mentési tárban. A virtuális gépek áttelepítése a klasszikus mód tooResource Manager mód és a Recovery Services-tároló a védelmüket? 

Klasszikus virtuális gép helyreállítási pontokat a biztonságimásolat-tárolóban nem automatikusan telepíti át tooa Recovery Services-tároló hello VM áthelyezése klasszikus tooResource kezelő módban. Kövesse ezeket a lépéseket tootransfer a virtuális gép biztonsági mentések:

1. Hello Backup-tárolóban, lépjen a toohello **védett elemek** fülre, és válassza ki a virtuális gép hello. Kattintson a [Védelem kikapcsolása](../articles/backup/backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines) parancsra. Hagyja a *Delete associated backup data* (Társított biztonsági mentési adatok törlése) beállítást **bejelöletlenül**.
2. Hello biztonsági másolat vagy pillanatkép-bővítmény törlése a virtuális gép hello.
3. Hello virtuális gép áttelepítése a klasszikus mód tooResource Manager üzemmódból. Ellenőrizze, hogy a megfelelő toohello virtuális gépek egyben hello tárolási és hálózati információk át tooResource kezelő módban.
4. Recovery Services-tároló létrehozása és konfigurálása a hello biztonsági mentési áttelepítése a virtuális gép **biztonsági mentési** művelet tároló irányítópult felett. Részletes információk a virtuális gép tooa Recovery Services biztonsági mentésével tároló, hello cikke [Azure virtuális gépek védelme Recovery Services-tároló](../articles/backup/backup-azure-vms-first-look-arm.md).

## <a name="can-i-validate-my-subscription-or-resources-toosee-if-theyre-capable-of-migration"></a>Azt is szeretnék ellenőrzi az előfizetés vagy az erőforrások toosee képes áttelepítési Ha? 

Igen. Hello platform által támogatott áttelepítési lehetőségek hello első lépése az áttelepítés előkészítése hello erőforrások áttelepítési képesek toovalidate. Abban az esetben hello érvényesítése művelet sikertelen lesz, üzeneteket fogadni összes hello okokból hello áttelepítési nem fejezhető be.

## <a name="what-happens-if-i-run-into-a-quota-error-while-preparing-hello-iaas-resources-for-migration"></a>Mi történik, ha egy kvótát hiba hello IaaS-erőforrásokra áttelepítésének előkészítése során futtatni? 

Azt javasoljuk, hogy az áttelepítés megszakítja, és jelentkezzen be egy támogatási kérést tooincrease hello kvóták hello régióban áttelepítése hello virtuális gépek esetén. Miután hello kvótakérelemhez jóváhagyják, megkezdheti a hello áttelepítési lépések végrehajtása újra.

## <a name="how-do-i-report-an-issue"></a>Hogyan jelenthetem be a hibákat? 

A problémák és áttelepítési tooour kapcsolatos kérdéseket küldje [VM fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows), ClassicIaaSMigration hello kulcsszóval. Javasoljuk, hogy minden kérdését ezen a fórumon tegye fel. Ha egy támogatási szerződése, Ön üdvözlő toolog, valamint a támogatási jegy.

## <a name="what-if-i-dont-like-hello-names-of-hello-resources-that-hello-platform-chose-during-migration"></a>Mi történik, ha nem megfelelő hello nevek hello platform hello erőforrások döntött, hogy az áttelepítés során? 

Összes hello erőforrás neve hello klasszikus üzembe helyezési modellel explicit módon adja meg az áttelepítés során megőrzi. Egyes esetekben új erőforrások jönnek létre. Például: minden virtuális géphez létrejön egy hálózati adapter. Jelenleg nem támogatjuk hello képességét toocontrol hello neve az áttelepítés során létrehozott új erőforrások. A szavazatok hello a szolgáltatáshoz tartozó naplófájl [Azure visszajelzési fórumon](http://feedback.azure.com).

## <a name="can-i-migrate-expressroute-circuits-used-across-subscriptions-with-authorization-links"></a>Migrálhatom az engedélyezési hivatkozásokkal rendelkező előfizetések között használt ExpressRoute-kapcsolatcsoportokat? 

Az előfizetések közötti engedélyezési hivatkozásokat használó ExpressRoute-kapcsolatcsoportok állásidő nélküli automatikus migrálása nem lehetséges. Az ezek manuális migrálására vonatkozóan vannak útmutatóink. Lásd: [áttelepítése ExpressRoute áramkörök és társított virtuális hálózatokat hello klasszikus toohello Resource Manager üzembe helyezési modellben](../articles/expressroute/expressroute-migration-classic-resource-manager.md) lépéseit, valamint további információt.

## <a name="i-got-a-message-vm-is-reporting-hello-overall-agent-status-as-not-ready-hence-hello-vm-cannot-be-migrated-ensure-that-hello-vm-agent-is-reporting-overall-agent-status-as-ready-or-vm-contains-extension-whose-status-is-not-being-reported-from-hello-vm-hence-this-vm-cannot-be-migrated-"></a>Olyan üzenet *"virtuális gép az jelentéskészítési hello általános ügynök állapota, nem áll készen. Emiatt hello virtuális gép nem telepíthető át. Győződjön meg arról, hogy hello Virtuálisgép-ügynök az jelentéskészítési készként általános ügynök állapota"* vagy *"a virtuális gép az állapotú nem ügynökökről származó hello VM kiterjesztést tartalmaz. Ezért ez a virtuális gép nem migrálható.” *

Ez az üzenet érkezik, ha hello virtuális gép nem tud a kimenő kapcsolat toohello internet. hello Virtuálisgép-ügynök frissítése hello ügynök állapota, ötpercenként kimenő kapcsolat tooreach hello Azure storage-fiókot használ.
