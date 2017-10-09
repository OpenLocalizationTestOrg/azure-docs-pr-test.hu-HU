## <a name="meaning-of-migration-of-iaas-resources-from-hello-classic-deployment-model-tooresource-manager"></a>Az IaaS-erőforrások hello klasszikus telepítési modell tooResource Manager az áttelepítés jelentése
Ahhoz, hogy részletekbe menően tárhatják fel hello részleteit, nézzük adatok-vezérlősík és felügyeleti-vezérlősík hello IaaS az erőforrásokon végrehajtott műveletek hello különbsége.

* *Felügyeleti és szabályozási vezérlősík* hello hívások, amelyek hello felügyeleti és szabályozási vezérlősík vagy hello API a erőforrások módosítását ismerteti. Például a műveletek, például a virtuális gép létrehozása, a virtuális gépek újraindításával és virtuális hálózat frissítése egy új alhálózatot hello rendszert futtató erőforrások kezelése. Közvetlenül csatlakozó toohello példányok nincsenek hatással.
* *Adatok vezérlősík* (alkalmazás) hello futásidejű hello alkalmazás maga írja le, és magában foglalja a nem halad át hello Azure API-val példányokra való együttműködéshez. Egy webhely elérése vagy adatok lekérése egy futó SQL Server-példányból vagy egy MongoDB-kiszolgálóról adatsík- vagy alkalmazásinterakciónak minősül. Egy blobot másol a tárfiókból, és egy nyilvános IP-cím tooRDP és az SSH használata a hello virtuális géppé is adatok vezérlősík. Ezek a műveletek számítási, hálózati és tárolási futtatásáért hello alkalmazás megtartása.

Hello háttérben hello adatok vezérlősík hello azonos hello klasszikus telepítési modell és a Resource Manager-készletben között. Áttelepítési folyamat során hello erőforrások hello klasszikus telepítési modell toothat hello Resource Manager-készletben a hello ábrázolását lefordítani azt. Ennek eredményeképpen kell toouse új eszközök, API-k, az SDK-k toomanage az erőforrások hello Resource Manager-készletben.

![A felügyeleti/vezérlősík és az adatsík közötti különbségeket bemutató képernyőkép](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> Néhány áttelepítési forgatókönyv hello Azure platformon leállítja, felszabadítja a, és újraindítja a virtuális gépek. Ez rövid állásidőt eredményez az adatsíkon.
>

## <a name="hello-migration-experience"></a>hello áttelepítési élmény
A kezdéshez hello áttelepítési élmény hello következő ajánlott:

* Győződjön meg arról, hogy, hogy szeretné-e toomigrate hello erőforrások nem bármely nem támogatott funkciókat vagy konfigurációk. Hello platform általában ezeket a problémákat észlel, és hibát generál.
* Ha virtuális gépeket, amelyek nincsenek rajta egy virtuális hálózathoz, azok leállítása és felszabadítása. lehetséges, hello részét készítse elő a művelet. Ha toolose hello nyilvános IP-cím nem szeretné, tekintse meg a hello IP-cím foglalása ahhoz, hogy kiváltsa hello előkészítése művelet. Azonban ha hello virtuális gépek virtuális hálózatban, ezeket nem leállítása és felszabadítása.
* Tervezze meg az áttelepítés során a váratlan meghibásodások esetén fordulhat elő, az áttelepítés során a munkaidőn kívüli tooaccommodate.
* Töltse le a virtuális gépek hello aktuális konfigurációja PowerShell, a parancssori felület (CLI) parancsok vagy a REST API-k toomake könnyebb a érvényesítése után hello előkészítése lépés befejeződött.
* Frissítse az automation/operationalization parancsfájlok toohandle hello Resource Manager telepítési modell hello áttelepítés megkezdése előtt. Opcionálisan teheti GET művelethez, ha az állapot előkészített hello hello erőforrások vannak.
* Értékelje ki hello RBAC házirendek hogyan vannak konfigurálva a hello klasszikus IaaS-erőforrásokra, és megtervezheti, hogy hello áttelepítés befejezése után.

hello áttelepítési munkafolyamat a következőképpen történik

![Képernyőkép a hello áttelepítési munkafolyamat](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Hello a következő részekben leírt összes hello művelet idempotent. Ha a probléma nem támogatott szolgáltatása vagy konfigurációs hiba, javasoljuk, hogy újra hello előkészítése, megszakítása vagy véglegesítése a műveletet. hello Azure platformon próbálja meg újra a művelettel hello.
>
>

### <a name="validate"></a>Érvényesítés
hello művelet érvényesítése hello hello áttelepítési folyamat első lépése. hello e lépés célja hello erőforrások toomigrate kívánja hello klasszikus üzembe helyezési modellel, és a sikeres és sikertelen vissza, ha hello erőforrások képesek áttelepítési tooanalyze hello állapotát.

Választja hello virtuális hálózat vagy a felhőszolgáltatás (Ha nincs a virtuális hálózat), amelyet az toovalidate az áttelepítéshez.

* Ha hello erőforrás nem kompatibilis az áttelepítés, a hello Azure platform minden hello okai miért nem támogatott az áttelepítés sorolja fel.

#### <a name="checks-not-done-in-validate"></a>Ellenőrzi, hogy nem történik a ellenőrzése

Ellenőrizze a művelet csak elemzi hello erőforrások hello klasszikus üzembe helyezési modellel hello állapotát. Ellenőrizheti, hogy az összes hiba és toovarious konfigurációk hello klasszikus üzembe helyezési modellel miatt nem támogatott forgatókönyvek. Azure Resource Manager az áttelepítés során a hello erőforrásokat előfordulhat, hogy ugyanazok a verem hello összes probléma lehetséges toocheck nincs. Ezek a problémák csak a rendszer ellenőrzi, amikor hello erőforrások változni átalakítása hello következő lépésben az áttelepítés, ez azt jelenti, hogy előkészítése. hello az alábbi táblázat ellenőrzése nem ellenőrzött minden hello probléma.


|Hálózatkezelés ellenőrzései nem ellenőrzése|
|-|
|ER és a VPN-átjárók rendelkező virtuális hálózat|
|A virtuális hálózati átjárókapcsolat leválasztása állapota|
|Összes ER áramkör előre áttelepített tooAzure Resource Manager-készletben|
|Az Azure erőforrás-kezelő kvótája keres a hálózati erőforrások, ez azt jelenti, hogy statikus nyilvános IP-címet, dinamikus nyilvános IP-címek, Load Balancer, hálózati biztonsági csoportok, Útvonaltábláit, hálózati adapterek |
| Ellenőrizze, hogy az összes terheléselosztási szabály érvényes telepítési/virtuális hálózat között |
| Ellenőrizze a ütköző privát IP-címek között a virtuális gépek leállítási felszabadítása hello azonos virtuális hálózaton |

### <a name="prepare"></a>Előkészítés
hello készítse elő a művelet hello hello áttelepítési folyamat második lépése. Ebben a lépésben hello célját toosimulate hello átalakítása hello IaaS-erőforrásokra, a klasszikus üzembe helyezési modell tooResource Manager források és jelent-e ez egymás mellett, toovisualize.

> [!NOTE] 
> A klasszikus erőforrások ezzel a lépéssel nem módosulnak. Ezért egy biztonságos lépés toorun, ha a kimenő áttelepítési próbált. 

Választja hello virtuális hálózat vagy hello felhőszolgáltatás (Ha a virtuális hálózat nincs), amelyet az tooprepare az áttelepítéshez.

* Hello erőforrás nincs áttelepítési képes, hello Azure platformon hello áttelepítési folyamat leállítja, majd megjeleníti hello oka, hogy hello miért készítse elő a művelet végrehajtása sikertelen volt.
* Ha hello erőforrás áttelepítési képes, hello Azure platformon először zárolja hello felügyeleti-vezérlősík műveletek hello erőforrások áttelepítés alatt. Például áll nem tud tooadd egy adatok lemez tooa virtuális gép áttelepítése alatt.

hello Azure platformon ezután elindítja a metaadatok hello áttelepítési a klasszikus üzembe helyezési modell tooResource Manager hello erőforrások áttelepítése.

Hello előkészítése után művelet befejeződött, lehetősége van hello alkalmazott hello erőforrások a klasszikus üzembe helyezési modellben és a Resource Manager. Minden felhőszolgáltatás hello klasszikus üzembe helyezési modellel, hello Azure platformon hoz létre egy erőforráscsoport neve, amelynek hello mintát `cloud-service-name>-Migrated`.

> [!NOTE]
> Nincs az áttelepített erőforrások létrehozott erőforráscsoport lehetséges tooselect hello nevét (például "-áttelepített"), de az áttelepítés befejezése után, használhatja az Azure Resource Manager áthelyezés szolgáltatás toomove erőforrások tooany kívánt erőforráscsoportot. További információk a részletek tooread [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../articles/resource-group-move-resources.md)

Az alábbiakban a két képernyőn, amelyek megjelenítik a hello eredménye egy sikeres előkészítési művelet után. Első képernyőn látható hello eredeti felhőszolgáltatás tartalmazó erőforráscsoport. Második a képernyőn látható hello új "-áttelepített" hello azonos Azure Resource Manager erőforrásokat tartalmazó erőforráscsoportot.

![Képernyőkép a Portal klasszikus felhőszolgáltatásáról](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Képernyőkép a Portal Azure Resource Manager-erőforrásairól előkészítés közben](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Íme egy háttérben nézze meg az erőforrások előkészítési fázisban hello befejezése után. Ne feledje, hogy hello erőforrás hello adatok vezérlősík van hello azonos. A hello felügyeleti vezérlősík (klasszikus üzembe helyezési modellel) és a hello vezérlő vezérlősík (erőforrás-kezelő) jelzi.

![Előkészítési szakaszában hello háttérben](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Virtuális gépek, amelyek nem szerepelnek a klasszikus virtuális hálózatot olyan, az áttelepítés ezen szakaszában leállt-felszabadítása.
>

### <a name="check-manual-or-scripted"></a>Ellenőrzés (manuális vagy szkriptalapú)
Hello ellenőrzés lépésben is használhat, hogy letöltötte-e, hogy helyes-e jelenik-e hello áttelepítési korábbi toovalidate hello konfigurációs. Azt is megteheti hogy jelentkezzen be toohello portál és a helyszíni ellenőrzés hello tulajdonságainak és erőforrásainak toovalidate, hogy a metaadatok áttelepítési megfelelőnek tűnik.

Ha virtuális hálózatot migrál, a virtuális gépek legtöbb konfigurációja nem indul újra. A virtuális gépek alkalmazásokhoz ellenőrizheti, hogy hello alkalmazás továbbra is működik, és van-e.

Tesztelheti a figyelés/automatizálási és működési parancsfájlok toosee hello virtuális gépek használata a várt módon, és ha a frissített parancsfájlok megfelelő működéséhez. Csak GET műveletek támogatottak, ha az állapot előkészített hello hello erőforrások vannak.

Nem állítsa időtartományt, ameddig szüksége toocommit hello áttelepítési van. Bármennyi időt eltölthet ebben az állapotban. Azonban hello felügyeleti vezérlősík zárolva ezeket az erőforrásokat amíg megszakítása vagy véglegesítése a határidő.

Ha probléma merül fel, mindig hello áttelepítési megszakítás és térjen vissza toohello klasszikus üzembe helyezési modellben. Után visszaléphet, hello Azure platformon hello felügyeleti-vezérlősík hello az erőforrásokon végrehajtott műveletek nyílik meg, hogy újból engedélyezheti a virtuális gépek hello klasszikus üzembe helyezési modellben a normál működést.

### <a name="abort"></a>Megszakítás
(Nem kötelező), hogy toorevert a módosítások toohello klasszikus üzembe helyezési modellt használja, és állítsa le a hello áttelepítési megszakítást. Ez a művelet törli az erőforrás-kezelő metaadatainak erőforrások hello előző Prepare lépésben létrehozott hello. 

![Megszakítási szakaszában hello háttérben](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Ez a művelet nem hajtható végre, miután léptetnie hello véglegesítési művelet.     
>

### <a name="commit"></a>Véglegesítés
Hello érvényesítése után véglegesíthető a hello áttelepítési. Erőforrások nem jelenik meg többé a klasszikus üzembe helyezési modellel, és csak a hello Resource Manager üzembe helyezési modellben. hello áttelepített erőforrások kezelhető csak az új portál hello.

> [!NOTE]
> Ez egy idempotens művelet. Ha nem sikerül, javasoljuk, hogy újra hello művelet. Ha toofail tovább, hozzon létre egy támogatási jegy, vagy hozzon létre egy fórum ClassicIaaSMigration címke a post a [VM fórum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![A véglegesítési fázis hello háttérben](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="where-toobegin-migration"></a>Ha toobegin áttelepítési?

Ez a folyamatábra bemutatja, hogyan tooproceed áttelepítéssel

![Képernyőkép a hello áttelepítés lépései](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-deployment-model-tooazure-resource-manager-resources"></a>Klasszikus telepítési modell tooAzure Resource Managerhez tartozó erőforrások fordítása
A következő táblázat hello hello klasszikus telepítési modell és erőforrás-kezelő ábrázolásai hello erőforrások találhatók. Az egyéb szolgáltatások és erőforrások jelenleg nem támogatottak.

| Klasszikus ábrázolás | Resource Manager-ábrázolás | Részletes megjegyzések |
| --- | --- | --- |
| Felhőszolgáltatás neve |DNS-név |Az áttelepítés során egy új erőforráscsoport létrejön minden felhőszolgáltatás hello elnevezési `<cloudservicename>-migrated`. Ez az erőforráscsoport tartalmazza az összes erőforrást. hello felhőszolgáltatás neve hello nyilvános IP-címhez társított egy DNS-neve lesz. |
| Virtuális gép |Virtuális gép |A virtuális gépre jellemző tulajdonságok a migrálás során nem változnak. Bizonyos osProfile adatok, például a számítógép nevét, hello klasszikus telepítési modell nem tárolja, és az áttelepítés után üres marad. |
| Lemezerőforrásokat csatolt tooVM |Csatlakoztatott lemezek implicit tooVM |Lemezek nem legfelső szintű erőforrások hello Resource Manager üzembe helyezési modellben van modellezve. Az áttelepítés alatt a virtuális gép hello implicit lemezként. Csak azok a lemezeken, amelyek a virtuális gép jelenleg támogatott tooa csatolni. Erőforrás-kezelő virtuális gépek már a klasszikus tárfiókokat, amely lehetővé teszi, hogy hello lemezek toobe könnyen át minden olyan frissítés nélkül. |
| Virtuálisgép-bővítmények |Virtuálisgép-bővítmények |Az összes hello erőforrás-bővítmény, XML-bővítmények, kivéve hello klasszikus telepítési modellből települnek át. |
| A virtuális gép tanúsítványai |Tanúsítványok az Azure Key Vaultban |Ha egy felhőalapú szolgáltatás tartalmazza a szolgáltatási tanúsítványok, egy új, az Azure key vault egy felhőalapú szolgáltatás, és hello tanúsítványok helyezi a hangsúlyt hello kulcstároló. hello virtuális gépek a frissített tooreference hello tanúsítványokat hello kulcstároló. <br><br> **Megjegyzés:** nem törölje hello keyvault mint okozhat a hibás állapot hello VM toogo. Folyamatosan bővítjük a hello háttérbeli dolgot javítása, hogy a kulcs tárolók biztonságosan törölhető vagy együtt hello tooa új előfizetés VM áthelyezése. |
| WinRM-konfiguráció |WinRM-konfiguráció osProfile alatt |A rendszer áthelyezi a távoli felügyeleti konfigurációs Windows hello-áttelepítés részeként változatlan. |
| Rendelkezésre állási csoport tulajdonsága |Rendelkezésre állási csoport erőforrás | Rendelkezésre állási-csoport megadását a virtuális gép hello hello klasszikus üzembe helyezési modellel tulajdonság volt. Rendelkezésre állási készletek vált egy legfelső szintű erőforrás hello áttelepítés részeként. hello következő konfigurációk nem támogatottak: több rendelkezésre állási készletek egy felhőalapú szolgáltatás, vagy egy vagy több rendelkezésre állási készletek együtt a virtuális gépek, amelyek nincsenek a rendelkezésre állási csoportban, felhőszolgáltatásban. |
| Hálózati konfiguráció egy virtuális gépen |Elsődleges hálózati adapter |A virtuális gép hálózati konfiguráció hello elsődleges hálózati illesztő erőforrás jelzi az áttelepítés után. Virtuális gépek, amelyek nincsenek rajta egy virtuális hálózati hello belső IP-cím módosításai az áttelepítés során. |
| Több hálózati adapter egy virtuális gépen |Hálózati illesztők |A virtuális gépek több hálózati csatolóval van társítva, ha mindegyik hálózati interfész válik az hello áttelepítés hello Resource Manager üzembe helyezési modellel, minden hello tulajdonságai mellett a legfelső szintű erőforrás. |
| Elosztott terhelésű végpont csoport |Terheléselosztó |Hello klasszikus üzembe helyezési modellel hello platform rendelt minden felhőszolgáltatás egy implicit terheléselosztót. Az áttelepítés során egy új terheléselosztó létrehozása történt meg, és hello terheléselosztás végpont készletének válik terheléselosztó szabályok. |
| Bejövő NAT-szabályok |Bejövő NAT-szabályok |Hello VM definiált bemeneti végpont olyan konvertált tooinbound hálózati cím fordítási szabályok alapján hello terheléselosztó hello az áttelepítés során. |
| Virtuális IP-cím |Nyilvános IP-cím DNS-névvel |hello virtuális IP-cím egy nyilvános IP-cím lesz, és hello terheléselosztó társítva. Egy virtuális IP-cím csak telepíthető át, ha nincs hozzárendelt tooit bemeneti végpontja. |
| Virtuális hálózat |Virtuális hálózat |virtuális hálózati hello át összes tulajdonságával, toohello Resource Manager üzembe helyezési modellben. Új erőforráscsoport létrejön hello nevű `-migrated`. |
| Fenntartott IP-címek |Nyilvános IP-cím statikus kiosztási módszerrel |Fenntartott IP-címek hello terheléselosztóhoz társított hello áttelepítési hello felhőszolgáltatás vagy hello virtuális gép együtt települnek. A nem társított fenntartott IP-címek migrálása jelenleg nem támogatott. |
| Nyilvános IP-cím virtuális gépenként |Nyilvános IP-cím dinamikus kiosztási módszerrel |hello VM alakítja át, egy nyilvános IP-cím erőforrás a hello elosztási módszer beállítása toostatic hello társított nyilvános IP-címet. |
| NSG-k |NSG-k |Alhálózat társított hálózati biztonsági csoportok vannak klónozott hello áttelepítési toohello Resource Manager üzembe helyezési modellben részeként. hello NSG hello klasszikus üzembe helyezési modellben nem lesz eltávolítva a hello az áttelepítés során. Azonban hello felügyeleti-vezérlősík műveletek hello NSG blokkolják a hello áttelepítés van folyamatban. |
| DNS-kiszolgálók |DNS-kiszolgálók |Egy virtuális hálózathoz tartozó DNS-kiszolgálók vagy hello VM az hello megfelelő erőforrás áttelepítés, minden hello tulajdonságok együtt települnek. |
| UDR-ek |UDR-ek |Alhálózat társított felhasználó által definiált útvonalak vannak klónozott hello áttelepítési toohello Resource Manager üzembe helyezési modellben részeként. hello UDR hello klasszikus üzembe helyezési modellben nem lesz eltávolítva a hello az áttelepítés során. hello felügyeleti-vezérlősík műveletek hello UDR blokkolt hello áttelepítés van folyamatban. |
| IP-továbbítási tulajdonság a virtuális gép hálózati konfigurációjában |IP-továbbítás hello NIC tulajdonság |hello IP-továbbítást a virtuális gép tulajdonság konvertált tooa tulajdonság hello hálózati illesztőn hello az áttelepítés során. |
| Terheléselosztó több IP-címmel |Terheléselosztó több nyilvános IP-erőforrással |Minden nyilvános IP-cím hello terheléselosztóhoz társított konvertált tooa nyilvános IP-erőforrás és hello terheléselosztóhoz társított áttelepítés után. |
| A virtuális gép hello belső DNS-nevek |Hello hálózati adapter a belső DNS-nevek |Az áttelepítés során hello belső DNS-utótagok hello virtuális gépek áttelepített tooa csak olvasható tulajdonság "belső tartománynév-utótag" megnevezett hello hálózati adaptert. hello utótag változatlan marad az áttelepítés után, és a virtuális gép feloldási továbbra is mint korábban toowork. |
| Virtuális hálózati átjáró |Virtuális hálózati átjáró |A virtuális hálózati átjáró tulajdonságai a migrálás során nem változnak. hello hello átjáróhoz társított VIP nem módosíthatja. |
| Helyi hálózati hely |Helyi hálózati átjáró |Helyi hálózati hely tulajdonságainak áttelepített változatlan tooa helyi hálózati átjáró nevű új erőforrást. Ez a helyszíni címelőtagokat és a távoli átjárók IP-címét képviseli. |
| Kapcsolati hivatkozások |Kapcsolat |Az átjáró és a helyi hálózati hely közötti kapcsolati hivatkozásokat a hálózati konfigurációban egy Connection nevű újonnan létrehozott erőforrás képviseli a Resource Managerben a migrálás után. Kapcsolat hivatkozást a hálózati konfigurációs fájlok tulajdonságainak az újonnan létrehozott másolt változatlan toohello kapcsolati erőforrást. A klasszikus virtuális hálózat tooVNet kapcsolat két IPsec-alagutak hello Vnetek képviselő toolocal hálózati telephelyek létrehozásával érhető el. Ez az átalakított tooVnet2Vnet kapcsolat a resource manager modellt anélkül, hogy a helyi hálózati átjáró típusát. |

## <a name="changes-tooyour-automation-and-tooling-after-migration"></a>Módosítások tooyour automatizálási és tooling áttelepítés után
Az erőforrások áttelepítése hello klasszikus telepítési modell toohello Resource Manager telepítési modell részeként van tooupdate a meglévő automatizációk vagy tooling tooensure, hogy az továbbra is toowork hello áttelepítés után.
