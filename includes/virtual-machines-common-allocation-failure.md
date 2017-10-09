
Ha az Azure nem problémával az ebben a cikkben, látogasson el a hello [MSDN és a Stack Overflow Azure fórumok](https://azure.microsoft.com/support/forums/). A probléma a fórumokban beküldheti vagy too@AzureSupport a Twitteren. Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a hello [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.

## <a name="general-troubleshooting-steps"></a>Általános hibaelhárítási lépéseket
### <a name="troubleshoot-common-allocation-failures-in-hello-classic-deployment-model"></a>Hello klasszikus üzembe helyezési modellel közös helyfoglalási hibák elhárítása
Ezeket a lépéseket megoldhatók a virtuális gépek sok pufferallokációs hibák:

* Automatikus oszlopszélesség hello VM tooa más Virtuálisgép-méretet.<br>
    Kattintson a **összes tallózása** > **virtuális gépek (klasszikus)** > a virtuális gép > **beállítások** > **mérete**. Részletes útmutató: [átméretezni a virtuális gépet hello](https://msdn.microsoft.com/library/dn168976.aspx).
* Minden virtuális gép törlése hello felhőalapú szolgáltatás, és hozza létre a virtuális gépek.<br>
    Kattintson a **összes tallózása** > **virtuális gépek (klasszikus)** > a virtuális gép > **törlése**. Kattintson a **új** > **számítási** > [virtuálisgép-lemezkép].

### <a name="troubleshoot-common-allocation-failures-in-hello-azure-resource-manager-deployment-model"></a>Hello Azure Resource Manager üzembe helyezési modellel közös helyfoglalási hibák elhárítása
Ezeket a lépéseket megoldhatók a virtuális gépek sok pufferallokációs hibák:

* Állítsa le (felszabadítása) hello ugyanabban a rendelkezésre állási állítsa be, majd indítsa újra a minden egyes virtuális gépeinek.<br>
    toostop: kattintson a **erőforráscsoportok** > az erőforráscsoport > **erőforrások** > a rendelkezésre állási csoport > **virtuális gépek** > a virtuális gép >  **Állítsa le**.
  
    Után minden virtuális gép leállításához válassza hello első virtuális gép kattintson **Start**.

## <a name="background-information"></a>Háttér-információkat
### <a name="how-allocation-works"></a>Foglalási működése
az Azure adatközpontjaiban hello kiszolgálók particionáltak csoportokba. Általában egy memóriafoglalási kérelem több fürt kísérlet történik, de előfordulhat, hogy bizonyos megkötéseknek hello foglalási kérelem kényszerítése hello Azure platformon tooattempt hello kérelem csak egy fürt. Ebben a cikkben kifejezés toothis fürtként"rögzített tooa." 1. ábra alatt hello kis-és a normál foglalási több fürt megkísérlő mutatja be. 2. ábrán látható, hello kis-és memóriakiosztást, amely tooCluster 2 van rögzítve, mivel az a Cloud Service CS_1 vagy rendelkezésre állási készlet meglévő hello ahol tárolása.
![Foglalási diagramja](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Miért fordulhat elő, pufferallokációs hibák
Amikor egy memóriafoglalási kérelem rögzített tooa fürt, magasabb esély van a toofind szabad erőforrást működni, mert hello erőforrás rendelkezésre álló készlet mérete. Továbbá ha a memóriafoglalási kérelem rögzített tooa fürt, de hello típus a kért erőforrás nem érhető el, hogy a fürt, a kérelem sikertelen lesz akkor is, ha hello fürt szabad erőforrást. 3. ábra alatt hello esetet, ahol egy rögzített foglalás sikertelen lesz, mivel hello csak jelölt fürtnek nincs szabad erőforrást mutatja be. 4. ábrán látható, ahol egy rögzített foglalás sikertelen lesz, mivel nem támogatja a hello csak jelölt fürt hello eset hello a kért Virtuálisgép-méretet, annak ellenére, hogy a fürt hello ingyenes erőforrással rendelkezik.

![Rögzített memóriafoglalási hiba](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-hello-classic-deployment-model"></a>Részletes lépéseket adott foglalási hiba forgatókönyvek hello klasszikus üzembe helyezési modellel hibaelhárítása
Az alábbiakban gyakori foglalási forgatókönyvek, amelyek egy foglalási kérelem toobe rögzítve. Igazolnia kell alaposabban egyes forgatókönyvek a cikk későbbi részében.

* Méretezze át egy virtuális Gépet, vagy adja hozzá a virtuális gép vagy szerepkör példányok tooan létező felhőalapú szolgáltatást
* Indítsa újra a részlegesen leállított (felszabadított) virtuális gépek
* Indítsa újra a teljes leállított (felszabadított) virtuális gépek
* Átmeneti vagy üzemi környezetek (csak szolgáltatásként platform)
* Az affinitáscsoport (virtuális gép vagy szolgáltatás beléptetőkártyával)
* Kapcsolat-csoport-alapú virtuális hálózat

Foglalási hiba megjelenésekor tekintse meg, ha leírt hello esetekben érvényes tooyour hiba. Hello memóriafoglalási hiba hello Azure platformon tooidentify hello megfelelő forgatókönyv által visszaadott használja. Ha a kérés van rögzítve, távolítson el néhányat hello kitűzési megkötések tooopen a a kérelem toomore fürtökhöz, növelve a hello eséllyel sikerül, felosztási.

Általánosságban elmondható, mindaddig, amíg hello hiba nem jelent a "hello a kért Virtuálisgép-méret nem támogatott", mindig újból megpróbálkozhat egy későbbi időpontban erőforrásokkal lehetett négyféleképpen hello fürt tooaccommodate a kérést. Ha hello probléma, hogy hello a kért Virtuálisgép-méret nem támogatott, próbálkozzon egy másik Virtuálisgép-méretet. Ellenkező esetben hello csak akkor tooremove hello megkötés rögzítését.

Két gyakori hiba forgatókönyvek olyan kapcsolódó tooaffinity csoportok. Az elmúlt hello affinitáscsoport használt tooprovide közel tooVMs/szolgáltatáspéldány, vagy használt tooenable hello létrehozását a virtuális hálózat volt. A regionális virtuális hálózatokba hello bevezetése affinitáscsoportok esetén már nem szükséges toocreate egy virtuális hálózatot. Hello Azure-infrastruktúra a hálózati késés csökkentésére hello ajánlás toouse affinitáscsoportok a virtuális gép vagy szolgáltatás közelségi kapcsolat megváltozott.

Az alábbi 5. ábra hello besorolás (rögzített) hello foglalási forgatókönyvet mutatja be.
![Rögzített foglalás besorolás](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> egy rövid formája, amely minden foglalási forgatókönyvben hello hiba. Tekintse meg a toohello [hiba karakterlánc keresési](#Error string lookup) a részletes hibaüzeneteket tartalmazza.
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-tooan-existing-cloud-service"></a>Foglalási forgatókönyv: méretezze át egy virtuális Gépet, vagy adja hozzá a virtuális gép vagy szerepkör-példányok tooan létező felhőalapú szolgáltatást
**Hiba történt**

Upgrade_VMSizeNotSupported vagy GeneralError

**A fürt rögzítési OK**

A kérelem tooresize egy virtuális Gépet, vagy adja hozzá a virtuális gép vagy szerepkör példány tooan meglévő felhőszolgáltatás toobe kísérelték meg futtatni: hello eredeti fürt hello létező felhőalapú szolgáltatást üzemeltető rendelkezik. Új felhőalapú szolgáltatás létrehozása lehetővé teszi, hogy hello Azure platformon toofind egy másik fürtöt, amely ingyenes erőforrással rendelkezik, vagy az Ön által kért hello Virtuálisgép-méretet támogatja.

**Megkerülő megoldás**

Ha hello hiba Upgrade_VMSizeNotSupported *, próbálkozzon egy másik Virtuálisgép-méretet. Ha egy másik Virtuálisgép-méretet használó lehetőség nem érhető el, de ha elfogadható toouse egy másik virtuális IP-cím (VIP), hozzon létre egy új felhőalapú szolgáltatás toohost hello új virtuális Gépet, és hello új felhőalapú szolgáltatás toohello regionális virtuális hálózat hozzáadása ahol hello meglévő virtuális gépek futnak. Ha a meglévő felhőalapú szolgáltatást használja a regionális virtuális hálózatot, továbbra is létrehozhat egy új virtuális hálózat hello új felhőalapú szolgáltatás, és csatlakoztassa a [meglévő virtuális hálózat toohello új virtuális hálózati](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). További tudnivalók [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Ha hello hiba GeneralError *, valószínű, hello típusú erőforrások (például egy adott virtuális gép méretét) hello fürt támogatja, de hello fürtnek nincs szabad erőforrást hello pillanatban. Fenti esetben hasonló toohello keresztül új felhőalapú szolgáltatás (vegye figyelembe, hogy új felhőszolgáltatás hello rendelkezik toouse egy másik VIP) létrehozásához szükséges hello számítási erőforrás hozzáadása, és a felhőszolgáltatásokat használ a regionális virtuális hálózat tooconnect.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Foglalási forgatókönyv: újraindítás részlegesen leállítva (felszabadított) virtuális gépek
**Hiba történt**

GeneralError *

**A fürt rögzítési OK**

Részleges felszabadítás azt jelenti, hogy a felhőszolgáltatás (felszabadított) egy vagy több, de nem minden virtuális gépe leállt. Amikor leállít (felszabadítása) egy virtuális Gépet, a kapcsolódó hello erőforrások kiadásakor. A leállított (felszabadított) virtuális gép újraindítása ezért egy új memóriafoglalási kérelem. Virtuális gépek újraindításával részben felszabadított felhőszolgáltatásban egyenértékű tooadding virtuális gépek tooan létező felhőalapú szolgáltatást. hello memóriafoglalási kérelem toobe kísérelték meg futtatni: hello eredeti fürt hello létező felhőalapú szolgáltatást üzemeltető rendelkezik. Másik felhőalapú szolgáltatás létrehozása lehetővé teszi, hogy hello Azure platformon toofind egy másik fürtöt, amely ingyenes erőforrással rendelkezik, és támogatja a kért hello Virtuálisgép-méretet.

**Megkerülő megoldás**

Elfogadható, ha egy másik VIP, a delete hello toouse (felszabadított) VMs (de tartsa hello kapcsolódó lemezek) leállt, és adja hozzá a virtuális gépek hello vissza egy másik felhőt szolgáltatáson keresztül. A regionális virtuális hálózat tooconnect a felhőszolgáltatások használata:

* Ha a meglévő felhőalapú szolgáltatást használ egy regionális virtuális hálózatot, egyszerűen adja hozzá hello új felhőalapú szolgáltatás toohello ugyanazt a virtuális hálózatot.
* Ha a meglévő felhőalapú szolgáltatás nem használja a regionális virtuális hálózatot, hozzon létre egy új virtuális hálózat hello új felhőalapú szolgáltatás, majd [a meglévő virtuális hálózat toohello új virtuális hálózat](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). További tudnivalók [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Foglalási forgatókönyv: újraindítás teljesen leállt (felszabadított) virtuális gépek
**Hiba történt**

GeneralError *

**A fürt rögzítési OK**

Teljes felszabadítás azt jelenti, hogy leállította (felszabadítva) összes virtuális gépet egy felhőalapú szolgáltatás. hello foglalási kérelmek toorestart virtuális gépeken toobe kísérelték meg futtatni: hello eredeti fürt hello felhőszolgáltatást üzemeltető rendelkezik. Új felhőalapú szolgáltatás létrehozása lehetővé teszi, hogy hello Azure platformon toofind egy másik fürtöt, amely ingyenes erőforrással rendelkezik, vagy az Ön által kért hello Virtuálisgép-méretet támogatja.

**Megkerülő megoldás**

Ha elfogadható toouse másik virtuális IP-címhez, törlési hello eredeti (felszabadított) VMs (de tartsa hello kapcsolódó lemezek) leállt, és törölje hello megfelelő felhőszolgáltatást (hello tartozó számítási erőforrásokat már kiadott, (felszabadított) leállításakor hello virtuális gépeken). Hozzon létre egy új felhőalapú szolgáltatás tooadd hello virtuális gépek vissza.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Foglalási forgatókönyv: átmeneti vagy üzemi környezetek (csak szolgáltatásként platform)
**Hiba történt**

New_General * vagy New_VMSizeNotSupported *

**A fürt rögzítési OK**

központi telepítés és hello éles felhőalapú szolgáltatások átmeneti hello tárolt hello ugyanabban a fürtben. Hello második központi telepítési hozzáadásakor hello megfelelő memóriafoglalási kérelem kísérli meg a rendszer a hello ugyanabban a fürtben, amelyen hello első központi telepítése.

**Megkerülő megoldás**

Törli a hello első központi telepítési és hello eredeti felhőalapú szolgáltatás, és telepítse újra a hello felhőalapú szolgáltatás. Ez a művelet lehet, hogy ártó hello első telepített, a fürt, amelyen elegendő szabad erőforrást toofit mindkét központi telepítés vagy a fürt, amely támogatja a kért hello Virtuálisgép-méretek.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Foglalási forgatókönyv: affinitáscsoport (virtuális gép vagy szolgáltatás beléptetőkártyával)
**Hiba történt**

New_General * vagy New_VMSizeNotSupported *

**A fürt rögzítési OK**

A számítási erőforrás hozzárendelt tooan affinitáscsoport tooone fürt kötődik. Új számítási erőforrás kérelmek affinitás csoport hello azonos fürt hello meglévő erőforrásokat a rendszer hol tárolja a rendszer megkísérelte. Ez érvényét veszti, hogy hello új erőforrások jönnek létre egy új felhőalapú szolgáltatás vagy egy meglévő felhőszolgáltatáshoz keresztül.

**Megkerülő megoldás**

Az affinitáscsoportokban nincs szükség, ha nem használható affinitáscsoport, vagy csoportosíthatja a számítási erőforrásokat több affinitáscsoportok.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Foglalási forgatókönyv: affinitás csoport-alapú virtuális hálózat
**Hiba történt**

New_General * vagy New_VMSizeNotSupported *

**A fürt rögzítési OK**

Regionális virtuális hálózatokba jelentek meg, mielőtt elvégezhetné szükséges tooassociate affinitáscsoporttal egy virtuális hálózatot. Ennek eredményeképpen az affinitáscsoportokban csoportosítson számítási erőforrásokat kötik azonos megkötések hello hello leírtak "foglalási forgatókönyv: affinitáscsoport (virtuális gép vagy szolgáltatás beléptetőkártyával)" a fenti szakaszban. hello számítási erőforrásokat a feltételekhez tooone fürtöket.

**Megkerülő megoldás**

Affinitáscsoport nem szükséges, ha új regionális virtuális hálózat létrehozása a hello új erőforrás hozzáadása esetén, majd [a meglévő virtuális hálózat toohello új virtuális hálózat](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). További tudnivalók [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Alternatív megoldásként, [telepítse át a virtuális hálózati kapcsolat csoport-alapú tooa regionális virtuális hálózat](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), majd újra adja hozzá hello szükséges erőforrásokat.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-hello-azure-resource-manager-deployment-model"></a>Részletes hibaelhárítási lépéseket adott foglalási hiba forgatókönyvek hello Azure Resource Manager üzembe helyezési modellel
Az alábbiakban gyakori foglalási forgatókönyvek, amelyek egy foglalási kérelem toobe rögzítve. Igazolnia kell alaposabban egyes forgatókönyvek a cikk későbbi részében.

* Méretezze át egy virtuális Gépet, vagy adja hozzá a virtuális gép vagy szerepkör példányok tooan létező felhőalapú szolgáltatást
* Indítsa újra a részlegesen leállított (felszabadított) virtuális gépek
* Indítsa újra a teljes leállított (felszabadított) virtuális gépek

Foglalási hiba megjelenésekor tekintse meg, ha leírt hello esetekben érvényes tooyour hiba. Hello memóriafoglalási hiba hello Azure platformon tooidentify hello megfelelő forgatókönyv által visszaadott használja. Ha a kérés rögzített tooan meglévő fürt, távolítson el néhányat hello kitűzési megkötések tooopen a a kérelem toomore fürtökhöz, növelve a foglalási sikeres hello esélyét.

Általánosságban elmondható, mindaddig, amíg hello hiba nem jelent a "hello a kért Virtuálisgép-méret nem támogatott", mindig újból megpróbálkozhat egy későbbi időpontban erőforrásokkal lehetett négyféleképpen hello fürt tooaccommodate a kérést. Ha hello probléma, hogy hello a kért Virtuálisgép-méret nem támogatott, lásd az alábbi a lehetséges megoldások.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-tooan-existing-availability-set"></a>Foglalási forgatókönyv: méretezze át egy virtuális Gépet, vagy adja hozzá a virtuális gépek tooan rendelkezésre állási készlet
**Hiba történt**

Upgrade_VMSizeNotSupported * vagy GeneralError *

**A fürt rögzítési OK**

A kérelem egy virtuális gép tooresize vagy hozzáadása egy meglévő rendelkezésre állási csoport Virtuálisgép-tooan toobe kísérelték meg futtatni: hello eredeti fürt, amelyen hello meglévő rendelkezésre állási csoport rendelkezik. Egy új rendelkezésre állási készlet létrehozása lehetővé teszi, hogy hello Azure platformon toofind egy másik fürtöt, amely ingyenes erőforrással rendelkezik, vagy az Ön által kért hello Virtuálisgép-méretet támogatja.

**Megkerülő megoldás**

Ha hello hiba Upgrade_VMSizeNotSupported *, próbálkozzon egy másik Virtuálisgép-méretet. Ha egy másik Virtuálisgép-méretet használó lehetőség nem érhető el, állítsa le a hello rendelkezésre állási csoport virtuális gépeinek. Ezután a módosítási hello mérete hello virtuális gép, amely hello VM tooa fürt, amely támogatja a hello foglal le a szükséges Virtuálisgép-méretet.

Ha hello hiba GeneralError *, valószínű, hello típusú erőforrások (például egy adott virtuális gép méretét) hello fürt támogatja, de hello fürtnek nincs szabad erőforrást hello pillanatban. Ha hello virtuális gép egy másik rendelkezésre állási készlet része lehet, hozzon létre egy új virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban). Új virtuális gép majd felveheti toohello ugyanazt a virtuális hálózatot.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Foglalási forgatókönyv: újraindítás részlegesen leállítva (felszabadított) virtuális gépek
**Hiba történt**

GeneralError *

**A fürt rögzítési OK**

Részleges felszabadítás azt jelenti, hogy (felszabadított) egy vagy több leállt, de nem minden, a rendelkezésre állási virtuális gépeket. Amikor leállít (felszabadítása) egy virtuális Gépet, a kapcsolódó hello erőforrások kiadásakor. A leállított (felszabadított) virtuális gép újraindítása ezért egy új memóriafoglalási kérelem. Részlegesen felszabadított rendelkezésre állási csoportban lévő virtuális gépek újraindításával egyenértékű tooadding virtuális gépek tooan meglévő rendelkezésre állási beállítása. hello memóriafoglalási kérelem toobe kísérelték meg futtatni: hello eredeti fürt, amelyen hello meglévő rendelkezésre állási csoport rendelkezik.

**Megkerülő megoldás**

Minden virtuális gép leállítása hello rendelkezésre állási csoportban, mielőtt első újraindítása hello. Ezzel biztosíthatja, hogy fut-e egy új foglalási kísérlet és, hogy egy új fürt választhat ki, amely rendelkezik a rendelkezésre álló kapacitásból.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Foglalási forgatókönyv: újraindítás teljesen leállt (felszabadított)
**Hiba történt**

GeneralError *

**A fürt rögzítési OK**

Teljes felszabadítás azt jelenti, hogy leállította (felszabadítva) minden virtuális gép rendelkezésre állási csoportba. hello memóriafoglalási kérelem virtuális gépeken által megcélzott összes fürt, amely támogatja a toorestart hello kívánt méretet.

**Megkerülő megoldás**

Válasszon egy új virtuális gép mérete tooallocate. Ha ez nem működik, próbálkozzon újra később.

## <a name="error-string-lookup"></a>Hibák karakterlánc keresése
**New_VMSizeNotSupported***

"hello VM méretet (vagy a Virtuálisgép-méretek kombinációja) szükséges a központi telepítési nem lehet kiépíteni toodeployment kérelem megkötések miatt. Ha lehetséges, próbálja meg lazítani a megszorításokon (például virtuális hálózati kötéseken), nincs másik üzemelő példány azt a tooa üzemeltetett szolgáltatás telepítése és a különböző affinitás tooa csoport- vagy affinitáscsoport, és nélkül próbálja tooa más régióban üzembe helyezése. "

**New_General***

"Kiosztása nem sikerült; a kérelem nem toosatisfy megkötések. hello kért új szolgáltatástelepítés kötött tooan affinitáscsoport, vagy azt a virtuális hálózat célozza, vagy nincs az üzemeltetett szolgáltatásban meglévő telepítéshez. Az alábbi feltételek valamelyikének korlátozza hello új központi telepítési toospecific Azure erőforrásokat. Próbálkozzon újra később, vagy próbálja meg csökkenteni a hello virtuális gép méretét vagy a szerepkörpéldányok számát. Azt is megteheti Ha lehetséges, távolítsa el a fentebb említett korlátozások hello, vagy próbálja tooa más régióban üzembe helyezni."

**Upgrade_VMSizeNotSupported***

"Nem tooupgrade hello telepítés. a kért hello Virtuálisgép-méretet XXX nem támogató hello meglévő központi telepítési hello erőforrások érhetők el. Próbálkozzon újra később, próbálkozzon más Virtuálisgép-méretet vagy kevesebb szerepkörpéldányt beállítani, vagy hozzon létre egy üzemelő példányt egy üres üzemeltetett szolgáltatásban új affinitáscsoporttal vagy affinitáscsoport nélkül az kötés."

**GeneralError***

"a hello kiszolgáló belső hibába ütközött. Próbálja meg újra hello kérelem." Vagy a "Sikertelen tooproduce hello szolgáltatás hozzárendelését."

