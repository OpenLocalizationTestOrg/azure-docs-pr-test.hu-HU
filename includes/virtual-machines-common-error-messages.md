>[!NOTE]
> Ezen a lapon, a visszajelzés vagy a megjegyzések hagyhatja [Azure visszajelzés](https://feedback.azure.com/forums/216843-virtual-machines) #azerrormessage címkével.

## <a name="error-response-format"></a>Hiba történt a válasz formátuma 
Azure virtuális gépek a következő hiba válasz JSON formátumban hello használata:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Egy hiba történt egy válasz mindig tartalmazza a állapotkódot és hiba objektum. Minden egyes hiba objektum mindig tartalmazza a hibakódot és egy üzenetet. Hello virtuális gép létrehozása egy sablonhoz, ha a hello hiba az objektum is tartalmaz, amely egy belső szintű hibakódok és az üzenet tartalmaz egy részletes adatait tartalmazó részben. Általában hello legtöbb belső hibaüzenet szintje hello legfelső szintű sikertelen.


## <a name="common-virtual-machine-management-errors"></a>Általános virtuális gép felügyeleti hibák

Ez a rész felsorolja hello leggyakoribb hibaüzenetek jelentkezhetnek, ha a virtuális gépek kezelése:

|  Hibakód:  |  Hibaüzenet  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Nem sikerült tooacquire bérleti lemez használata a blob URI {1} "{0}" létrehozásakor. A BLOB már használatban van.  |  
|  AllocationFailed  |  A lefoglalás sikertelen. Próbálja meg csökkenteni a hello virtuális gép méretét vagy a virtuális gépek számát, próbálkozzon később, vagy próbálja üzembe helyezni a tooa másik rendelkezésre állási csoportban vagy más Azure-beli hely.  |  
|  AllocationFailed  |  virtuális gép foglalása hello tooan belső hiba miatt sikertelen volt. Próbálkozzon újra később, vagy próbálja üzembe helyezni a tooa másik helyre.  |
|  ArtifactNotFound  |  hello a közzétevő "{0}" és a típus "{1}" Virtuálisgép-bővítmény nem található "{2}" helyen.  |
|  ArtifactNotFound  |  A közzétevő "{0}", kiterjesztés írja be a "{1}", és típuskezelő verziója "{2}" nem található a bővítménytárban hello.  |
|  ArtifactVersionNotFound  |  Nem található, amely eleget tesz a hello hello összetevőtárban verzió kért verzió "{0}".  |
|  ArtifactVersionNotFound  |  Nem található, amely eleget tesz a hello hello összetevőtárban verzió kért verzió a(z) "{0}' Virtuálisgép-bővítmény közzétevő"{1}"és"{2}"típus.  |
|  AttachDiskWhileBeingDetached  |  Adatok lemez "{0}" tooVM "{1}" nem csatolható, mert hello lemez van jelenleg leválasztás alatt áll. Várjon, amíg a hello lemez leválasztása, és próbálkozzon újra.  |
|  BadRequest  |  Illesztett "rendelkezésre állási készletek még nem támogatottak ebben a régióban.  |
|  BadRequest  |  A felügyelt lemezek toonon által felügyelt rendelkezésre állási csoport vagy a virtuális gép és blobalapú lemezek toomanaged rendelkezésre állási csoport hozzáadása nem támogatott a virtuális gépek hozzáadása. Hozzon létre rendelkezésre állási csoport "kezelt" tulajdonságkészlet rendelés tooadd a virtuális gép és a felügyelt lemezek tooit.  |
|  BadRequest  |  Felügyelt lemezek nem támogatottak ebben a régióban.  |
|  BadRequest  |  Kezelőnként több operációs rendszer nem támogatott írja be a "{0}". VMExtension "{1}" leírójú "{2}" már hozzáadott, vagy a megadott bemeneti adatok.  |
|  BadRequest  |  "{0}" művelet nem támogatott az erőforrás "{1}" felügyelt lemezzel.  |
|  CertificateImproperlyFormatted  |  hello kulcshoz JSON-megjelenítés {0}. olvassa van egy data mező, amely nem megfelelően formázott PFX-fájlba, vagy a megadott hello jelszó nem dekódolja megfelelően hello PFX-fájlt.  |
|  CertificateImproperlyFormatted  |  hello {0}. olvassa adata nem deszerializálható JSON formátumba.  |
|  Ütközés  |  A lemezek átméretezése csak virtuális gép létrehozásakor vagy felszabadított virtuális gép hello esetén engedélyezett.  |
|  ConflictingUserInput  |  Hello lemez már van tulajdonosa a virtuális gép "{1}" lemez "{0}" nem lehet csatolni.  |
|  ConflictingUserInput  |  Forrás- és erőforrás-csoportok vannak hello azonos.  |
|  ConflictingUserInput  |  A(z) {0} forrás és cél tárfiókjai különbözők.  |
|  ContainerAlreadyOnLease  |  Már van egy bérleti hello blob URI-azonosítóhoz: {0} okozó hello tároló.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  hello áthelyezési kérelem tartalmazza a Key Vault-erőforrásokat, amelyek egy vagy több {0} s hello kérelem által hivatkozott. Ez nem támogatott a jelenleg Áthelyezés az előfizetések közötti. Ellenőrizze a KeyVault erőforrás-azonosítók hello hello kapcsolatos hiba részleteit.  |
|  DiagnosticsOperationInternalError  |  Belső hiba történt a(z) {0} virtuális gép diagnosztikai profiljának feldolgozása során.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  BLOB {0} már használja egy másik lemezt tartozó tooVM "{1}". Láthatja, hogy hello blob metaadatait hello lemez referencia jellegű információt.  |
|  DiskBlobNotFound  |  Virtuális merevlemez nem toofind a lemez "{1}" {0} URI blob.  |
|  DiskBlobNotFound  |  Virtuális merevlemez nem toofind blob URI-azonosítóhoz: {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} titkos kulcs hello {1} címkék nem rendelkezik. Hello titkos kulcs verzióját frissítse, hello szükséges címkéket, majd próbálja megismételni.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Való kibontása nem sikerült kulcs {1} használatával titkos {0} értéket.  |
|  DiskImageNotReady  |  Lemez kép {0} {1} állapotban van. Próbálkozzon újra, amikor készen áll a lemezképet.  |
|  DiskPreparationError  |  Egy vagy több hiba történt a Virtuálisgép-lemezek előkészítése során. Tekintse meg a lemez példányait tartalmazó nézetet.  |
|  DiskProcessingError  |  Lemez feldolgozás leállt hello VM levő többi lemezzel rendelkezik, a hibás lemezeket.  |
|  ImageBlobNotFound  |  Virtuális merevlemez nem toofind a lemez "{1}" {0} URI blob.  |
|  ImageBlobNotFound  |  Virtuális merevlemez nem toofind blob URI-azonosítóhoz: {0}.  |
|  IncorrectDiskBlobType  |  Az oldalakra vonatkozó blob típusú lemezblobok csak lehet. Lemez "{1}" {0} BLOB blokkblob típusú paraméter.  |
|  IncorrectDiskBlobType  |  Az oldalakra vonatkozó blob típusú lemezblobok csak lehet. A BLOB {0} paraméter típusa "{1}".  |
|  IncorrectImageBlobType  |  Az oldalakra vonatkozó blob típusú lemezblobok csak lehet. Lemez "{1}" {0} BLOB blokkblob típusú paraméter.  |
|  IncorrectImageBlobType  |  Az oldalakra vonatkozó blob típusú lemezblobok csak lehet. A BLOB {0} paraméter típusa "{1}".  |
|  InternalOperationError  |  Nem sikerült feloldani a tárfiók {0}. Győződjön meg arról, mert készült keresztül hello szolgáltató hello azonos helyen hello számítási erőforrás.  |
|  InternalOperationError  |  {0} célkeresési feladat sikertelen volt.  |
|  InternalOperationError  |  Hiba történt a virtuális gép "{0}" hello hálózati profiljának érvényesítése során.  |
|  InvalidAccountType  |  hello AccountType {0} érvénytelen.  |
|  InvalidParameter  |  hello paraméter: {0} értéke érvénytelen.  |
|  InvalidParameter  |  a megadott hello rendszergazdai jelszó nem engedélyezett.  |
|  InvalidParameter  |  "hello megadott jelszónak kell lennie a(z) {0} közötti-\ {1\} karakterből állnak, és meg kell felelnie a legalább {2} a hello következő összetettségi követelményeknek: <ol><li> Nagybetűs karaktert tartalmaz</li><li>A kisbetűs karaktert tartalmaz</li><li>Egy numerikus számjegyet tartalmaz</li><li>Speciális karaktert tartalmaz.</li></ol>  |
|  InvalidParameter  |  a megadott rendszergazdai felhasználónév hello nem engedélyezett.  |
|  InvalidParameter  |  Egy meglévő operációsrendszer-lemez nem csatolható, ha hello virtuális gép létrehozása az platform vagy felhasználói rendszerkép.  |
|  InvalidParameter  |  Érvénytelen a tároló neve {0}. A tároló neve 3 – 63 karakter hosszúságúnak kell lennie, és csak kisbetűs alfanumerikus karaktereket és kötőjelet tartalmazhat. Kötőjel előtt és után alfanumerikus karakternek.  |
|  InvalidParameter  |  Tároló neve {0} {1} URL-cím érvénytelen. A tároló neve 3 – 63 karakter hosszúságúnak kell lennie, és csak kisbetűs alfanumerikus karaktereket és kötőjelet tartalmazhat. Kötőjel előtt és után alfanumerikus karakternek.  |
|  InvalidParameter  |  hello blob neve az URL-cím {0} perjelet tartalmaz. Ez jelenleg nem támogatott a lemezeket.  |
|  InvalidParameter  |  hello URI {0} toobe megfelelő blob URI nem tűnik.  |
|  InvalidParameter  |  A lemez "{0}" nevű már használja ugyanezt a LUN hello: {1}.  |
|  InvalidParameter  |  A lemez "{0}" nevű már létezik.  |
|  InvalidParameter  |  Nem adható meg, a lemez már definiálva van a megadott hello felülbírálások felhasználói lemezkép referencia lemezképet.  |
|  InvalidParameter  |  A lemez "{0}" nevű már használja ugyanazt a virtuális merevlemez URL-cím {1} hello.  |
|  InvalidParameter  |  hello megadott tartalék tartományt száma {0} kell lennie hello tartomány {1} too\ {2\}.  |
|  InvalidParameter  |  hello {0} licenc típusa érvénytelen. Érvényes licenctípusok a következők: Windows_Client vagy Windows_Server, és a nagybetűk között.  |
|  InvalidParameter  |  Linux-gazdagépnév nem lehet {0} karakternél hosszabb és nem tartalmazhatja a következő karaktereket hello: {1}.  |
|  InvalidParameter  |  Az Ssh nyilvános kulcsok cél elérési út jelenleg korlátozott tooits alapértelmezett érték {0} esedékes tooa problémát a Linux-telepítőügynök ismert.  |
|  InvalidParameter  |  Már létezik lemez a(z) {0} LUN-értékkel.  |
|  InvalidParameter  |  Előfizetés {0} hello kérelem hello előfizetés {1} hello felügyelt lemezazonosítóban szereplő egyeznie kell.  |
|  InvalidParameter  |  Az operációsrendszer-profilban (OSProfile) szereplő egyéni adatoknak a Base64 kódolást kell követniük, és legfeljebb {0} karakteresek lehetnek.  |
|  InvalidParameter  |  Az URL-cím {0} BLOB neve "{1}" kiterjesztést kell végződnie.  |
|  InvalidParameter  |  {0} "értéke nem egy érvényes rögzített VHD blob előtagja. Egy érvényes előtagnak meg regex "{1}".  |
|  InvalidParameter  |  Tanúsítványok nem adható hozzá tooyour VM Ha hello Virtuálisgép-ügynök nincs telepítve.  |
|  InvalidParameter  |  Már létezik lemez a(z) {0} LUN-értékkel.  |
|  InvalidParameter  |  Virtuális gép nem toocreate hello hello kért méret {0} nem érhető el hello fürt, ahol hello rendelkezésre állási csoport jelenleg lefoglalt. hello elérhető méretek: {1}. Olvassa el a következő https://aka.ms/azure-resizevm stratégia átméretezése további a virtuális gép.  |
|  InvalidParameter  |  a kért hello virtuális gép mérete {0} nem érhető el hello aktuális régióban. hello aktuális régióban használható hello méret: {1}. További információkért a hello elérhető Virtuálisgép-méretek https://aka.ms/azure-regions, minden régióban.  |
|  InvalidParameter  |  a kért hello virtuális gép mérete {0} nem érhető el hello aktuális régióban. További információkért a hello elérhető Virtuálisgép-méretek https://aka.ms/azure-regions, minden régióban.  |
|  InvalidParameter  |  Windows Rendszergazda felhasználóneve nem lehet több mint {0} karakter hosszú, végződhet ponttal (.), vagy hello a következő karaktereket tartalmaz: {1}.  |
|  InvalidParameter  |  Windows-számítógép neve nem lehet több mint {0} karakter hosszú, nem állhat csak számokból, vagy hello a következő karaktereket tartalmaz: {1}.  |
|  MissingMoveDependentResources  |  hello áthelyezési kérelem nem tartalmaz hello tőle függő összes erőforrásról. Ellenőrizze a hiba részleteit a hiányzó erőforrás-azonosítók.  |
|  MoveResourcesHaveInvalidState  |  hello áthelyezési kérelem tartalmazza a virtuális gépek, amely érvénytelen storage-fiókok kapcsolódik. A részletekben ezen erőforrás-azonosítók és a hivatkozott tárfiókok neve.  |
|  MoveResourcesHavePendingOperations  |  hello áthelyezési kérelem tartalmazza az erőforrásokat, amelyhez egy művelet folyamatban. Részletekben ezen erőforrás-azonosítók. Próbálja megismételni a műveletet, hello függőben lévő műveletek befejezése után.  |
|  MoveResourcesNotFound  |  hello kérelem olyan erőforrásokat tartalmaz, nem található erőforrások áthelyezése. Részletekben ezen erőforrás-azonosítók.  |
|  NetworkingInternalOperationError  |  Ismeretlen hálózatfoglalási hiba.  |
|  NetworkingInternalOperationError  |  Ismeretlen hálózatfoglalási hiba  |
|  NetworkingInternalOperationError  |  Belső hiba történt a hello virtuális gép hálózati profiljának feldolgozása.  |
|  notFound  |  hello rendelkezésre állási csoport {0} nem található.  |
|  notFound  |  Azure ezen a helyen nem létezik a forrás virtuális gép hello kérelemben megadott "{0}".  |
|  notFound  |  Nem található {0} azonosítóval rendelkező bérlő.  |
|  notFound  |  hello {0} lemezkép nem található.  |
|  NotSupported  |  hello licenc típusa: {0}, viszont hello kép blob {1} nem helyszíni.  |
|  OperationNotAllowed  |  Rendelkezésre állási készlet {0} nem lehet törölni. Rendelkezésre állási csoport törlése előtt győződjön meg arról, hogy a virtuális gép nem tartalmaz.  |
|  OperationNotAllowed  |  Módosítása a rendelkezésre állási csoport Termékváltozata "Igazított" too'Classic a "nem engedélyezett.  |
|  OperationNotAllowed  |  Hello virtuális gép bővítményei nem módosíthatók, ha nem fut a virtuális gép hello.  |
|  OperationNotAllowed  |  hello rögzítési művelet csak egy virtuális gépet a blobalapú lemezek esetén támogatott. Használjon hello "Kép" erőforrás API-k toocreate lemezkép felügyelt virtuális gépről.  |
|  OperationNotAllowed  |  hello erőforrás {0} nem hozható létre kép {1} alapján, amíg a kép létrehozása sikeresen befejeződött.  |
|  OperationNotAllowed  |  Frissítések tooencryptionSettings nem engedélyezett, ha a virtuális gép le van foglalva. Próbálkozzon újra, miután a virtuális gép felszabadítása  |
|  OperationNotAllowed  |  A blobalapú lemezek felügyelt lemezes tooa virtuális gép hozzáadása nem támogatott.  |
|  OperationNotAllowed  |  adatlemezek maximális száma hello engedélyezett csatolt toobe tooa ekkora VM {0}.  |
|  OperationNotAllowed  |  A blobalapú lemezek tooVM kezelt lemezek hozzáadása nem támogatott.  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett "{1}" rendszerképre a törlésre megjelölt lemezkép hello óta. Csak próbálkozhat hello törlési művelet (vagy egy folyamatban lévő egyik toocomplete várja).  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}" virtuális gép általánosítva van hello óta.  |
|  OperationNotAllowed  |  "{0}" művelet nem lehetséges, mert a törlésre megjelölt visszaállítási pont gyűjtemény "{1}".  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a Virtuálisgép-bővítmény "{1}", mert meg van jelölve törlésre. Csak próbálkozhat hello törlési művelet (vagy egy folyamatban lévő egyik toocomplete várja).  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett, mert hello virtuális gépek "{1}" hello kép "{2}" használatával vannak folyamatban.  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett, mert a virtuális gép ScaleSet "{1}" hello pillanatnyilag hello kép "{2}".  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}" virtuális gép ki van jelölve törlésre hello óta. Csak próbálkozhat hello törlési művelet (vagy egy folyamatban lévő egyik toocomplete várja).  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}", mivel hello virtuális gép felszabadítása felszabadított, vagy a megjelölt toobe.  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}" virtuális gép fut hello óta. Adja a kikapcsolás explicit módon abban az esetben állítsa le hello virtuális gép hello vendég operációs rendszerében.  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}" virtuális gép nem felszabadítása hello óta.  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}", mert a virtuális gép kiterjesztése "{2}" hibaállapotban van.  |
|  OperationNotAllowed  |  "{0}" művelet nem engedélyezett a virtuális gép "{1}", mert egy másik művelet van folyamatban.  |
|  OperationNotAllowed  |  hello a művelethez "{0}" hello virtuális gép "{1}" toobe Generalized szükséges.  |
|  OperationNotAllowed  |  hello művelet megköveteli a hello VM toobe fut (vagy toorun beállítása).  |
|  OperationNotAllowed  |  Lemez méretével, {0} GB, amely kisebb, mint hello van mérete {1}GB megfelelő lemez a lemezképet, a nem engedélyezett.  |
|  OperationNotAllowed  |  Méretezési csoportjának bővítményei leíró "{0}" csak hello időben méretezési csoportjának létrehozásakor adhatók meg.  |
|  OperationNotAllowed  |  Méretezési csoportjának bővítményei leíró "{0}" csak hello időben a virtuális gépek méretezési csoportjának törlésekor törölhetők.  |
|  OperationNotAllowed  |  Virtuális gép "{0}" felügyelt lemezek már használja.  |
|  OperationNotAllowed  |  Virtuális gép "{0}" tartozik too'Classic "rendelkezésre állási csoport"{1}". Frissítés hello rendelkezésre állási toouse "Igazított" SKU állítsa, majd próbálkozzon újra a hello átalakítás.  |
|  OperationNotAllowed  |  VM-lemezkép alapján létre blobalapú lemezek nem rendelkezhet. Az összes lemez felügyelt toobe lemezekkel rendelkeznek.  |
|  OperationNotAllowed  |  Rögzítési művelet nem hajtható végre, mert hello virtuális gép nincs általánosítva.  |
|  OperationNotAllowed  |  A virtuális gép "{0}" felügyeleti műveleteihez vannak nem engedélyezett, mert a virtuális gépek lemezei folyamatban van a konvertált toomanaged lemezek.  |
|  OperationNotAllowed  |  Folyamatban lévő művelet van virtuális gép {0} too\ {1\} / kikapcsolt állapotának módosítása. Hajtson végre műveletet {2} egy kis idő múlva.  |
|  OperationNotAllowed  |  Nem lehet tooadd vagy frissítés hello virtuális gép. hello irányuló kérés. Előfordulhat, hogy a virtuális gép mérete {0} nem lesz elérhető a hello meglévő foglalási egység. Olvassa el a következő https://aka.ms/azure-resizevm stratégia átméretezése további a virtuális gép.  |
|  OperationNotAllowed  |  Virtuális gép nem tooresize hello hello kért méret {0} nem érhető el hello fürt, ahol hello rendelkezésre állási csoport jelenleg lefoglalt. hello elérhető méretek: {1}. Olvassa el a következő https://aka.ms/azure-resizevm stratégia átméretezése további a virtuális gép.  |
|  OperationNotAllowed  |  Virtuális gép nem tooresize hello hello kért méret {0} nem érhető el hello fürt, ahol hello VM jelenleg lefoglalt. a virtuális gép too\ {1\} adjon felszabadítani tooresize (Ez a Stop műveletet hello Azure-portál), majd próbálja újra a hello átméretezés. Olvassa el a következő https://aka.ms/azure-resizevm stratégia átméretezése további a virtuális gép.  |
|  OSProvisioningClientError  |  Virtuális gép "{0}" operációs rendszer telepítése sikertelen, mert hello vendég operációs rendszer jelenleg folyamatban van.  |
|  OSProvisioningClientError  |  Nem sikerült az operációs rendszer üzembe helyezni a virtuális gép "{0}". Hiba legutolsó részletes adatai: {1} győződjön meg arról, hogy hello lemezkép megfelelően elő van készítve (általánosítva van). <ul><li>Windows vonatkozó utasításokat: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH állomás kulcs létrehozása sikertelen volt. Hiba részletei: {0}. tooresolve probléma ellenőrizze, hogy ha a Linux-ügynök megfelelően van-e beállítva. <ul><li>Ellenőrizheti a hello található utasítások segítségével:: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Felhasználónév megadott hello a Linux-disztribúció érvénytelen virtuális gép. Hiba részletei: {0}.  |
|  OSProvisioningInternalError  |  Operációs rendszer telepítése sikertelen volt a virtuális gép "{0}" tooan belső hiba miatt.  |
|  OSProvisioningTimedOut  |  A virtuális gép "{0}" operációs rendszer telepítése nem fejeződött be a kiosztott idő hello. virtuális gép hello előfordulhat, hogy után is sikeres. Ellenőrizze később a telepítés állapotát.  |
|  OSProvisioningTimedOut  |  A virtuális gép "{0}" operációs rendszer telepítése nem fejeződött be a kiosztott idő hello. virtuális gép hello előfordulhat, hogy után is sikeres. Ellenőrizze később a telepítés állapotát. Ne felejtse hello lemezkép megfelelően elő van készítve (általánosítva van).   <ul><li>Windows vonatkozó utasításokat: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Útmutatás Linux rendszerhez: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  A virtuális gép "{0}" operációs rendszer telepítése nem fejeződött be a kiosztott idő hello. Azonban hello virtuális gép vendégügynökének futtató volt észlelhető. Ez azt sugallja hello vendég operációs rendszer nem lett megfelelően előkészítve toobe használt Virtuálisgép-lemezképet (az CreateOption = FromImage). tooresolve a probléma, vagy használjon hello VHD-t, és a CreateOption = Attach vagy képként használatra megfelelően előkészítése:   <ul><li>Windows vonatkozó utasításokat: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Útmutatás Linux rendszerhez: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  hello szükséges Virtuálisgép-méret nem érhető el jelenleg kijelölt hello helyen.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Erőforrás tooongoing platform frissítés miatt most nem lehet frissíteni. Később próbálja meg újra.  |
|  StorageAccountLimitation  |  Storage-fiók "{0}" nem támogatja a lapblobokat, amelyek szükséges toocreate lemez.  |
|  StorageAccountLimitation  |  A(z) {0} tárfiók túllépte a számára lefoglalt kvótát.  |
|  StorageAccountLocationMismatch  |  Nem sikerült feloldani a tárfiók {0}. Győződjön meg arról, mert készült keresztül hello szolgáltató hello azonos helyen hello számítási erőforrás.  |
|  StorageAccountNotFound  |  Tárolási fiók {0} nem található. Győződjön meg arról, a tárfiók nem törlődik, és toohello tartozik hello VM azonos Azure-beli helyhez.  |
|  StorageAccountNotRecognized  |  A szolgáltató által kezelt tárfiókot használja. : {0} használata nem támogatott.  |
|  StorageAccountOperationInternalError  |  A(z) {0} tárfiók elérése során hiba történt.  |
|  StorageAccountSubscriptionMismatch  |  Tárolási fiók {0} toosubscription {1} nem tartozik.  |
|  StorageAccountTooBusy  |  "{0}" tárfiók jelenleg túlságosan elfoglalt. Érdemes lehet egy másik fiókot.  |
|  StorageAccountTypeNotSupported  |  Lemez {0} {1}, amely egy Blob storage-fiókot használ. Próbálja meg újra egy általános célú tárfiókkal.  |
|  StorageAccountTypeNotSupported  |  Tárolási fiók {0} {1} típusa nem. Rendszerindítási diagnosztika {2} tárfióktípusokat támogatja.  |
|  SubscriptionNotAuthorizedForImage  |  hello előfizetésében nincs engedélyezve.  |
|  TargetDiskBlobAlreadyExists  |  A BLOB {0} már létezik. Adjon meg egy másik blob URI toocreate egy új üres adatok lemez "{1}".  |
|  TargetDiskBlobAlreadyExists  |  Rögzítési művelet nem folytatható, mert a cél lemezképet blob {0} már létezik, és hello jelző toooverwrite VHD-blobok nincs beállítva. Vagy hello blob törlése vagy hello jelző beállítása toooverwrite VHD-blobok, és próbálja meg újra.  |
|  TargetDiskBlobAlreadyExists  |  A rögzítési művelet nem folytatódhat, mert a cél lemezképblobra ({0}) egy aktív bérleti jog vonatkozik.   |
|  TargetDiskBlobAlreadyExists  |  A BLOB {0} már létezik. Adjon meg egy másik blob URI célként lemez "{1}".  |
|  TooManyVMRedeploymentRequests  |  Túl sok újbóli üzembe helyezése kérést kapott a virtuális gép "{0}", vagy hello virtuális gépeinek hello azonos availabilityset rendelkező virtuális Gépet. Próbálkozzon újra később.  |
|  VHDSizeInvalid  |  a megadott hello lemez méret: {0} lemez a blob {2} "{1}" értéke érvénytelen. Lemez méretének {3}, {4} és között kell lennie.  |
|  VMAgentStatusCommunicationError  |  Virtuális gép ({0}) nem küldött állapotjelentést a Virtuálisgép-ügynök vagy a bővítmények. Ellenőrizze a hello VM van-e virtuális gép futó ügynök, és kimenő kapcsolatok tooAzure tárolási hozhatnak létre.  |
|  VMArtifactRepositoryInternalError  |  Hiba történt a hello összetevő tárház tooretrieve VM összetevő részletek folytatott kommunikáció során.  |
|  VMArtifactRepositoryInternalError  |  Belső hiba történt a hello összetevőtárban hello virtuális gép összetevőadatai beolvasásakor.  |
|  VMExtensionHandlerNonTransientError  |  Eseménykezelő "{0}" kapcsolatos hibát jelzett Virtuálisgép-bővítmény "{1}" Terminálszolgáltatások hiba kód "{2}" és a hibaüzenet: "{3}"  |
|  VMExtensionManagementInternalError  |  Belső hiba történt a(z) {0} virtuálisgép-bővítmény feldolgozása során.  |
|  VMExtensionManagementInternalError  |  Hello Virtuálisgép-bővítmények előkészítése során több hiba történt. Tekintse meg a virtuális gép bővítmény példányait tartalmazó nézetet.  |
|  VMExtensionProvisioningError  |  Virtuális gép hibát jelzett a(z) "{0}' bővítmény feldolgozása során. Hibaüzenet: "{1}".  |
|  VMExtensionProvisioningError  |  Több Virtuálisgép-bővítmény nem tudta toobe hello VM lett kiépítve. Hello VM bővítmények példányait tartalmazó nézetet a részletekért tekintse meg.  |
|  VMExtensionProvisioningTimeout  |  A(z) "{0}' Virtuálisgép-bővítmény telepítése túllépte az időkorlátot. Bővítmény telepítése előfordulhat, hogy túl sokáig tart, vagy a bővítmény állapota nem határozható meg.  |
|  VMMarketplaceInvalidInput  |  Egy virtuális gép létrehozása egy nem Piactéri rendszerképből nem kell tervezze meg információkat, távolítsa el hello terv adatainak hello kérelem. Az operációs rendszer lemezének neve a következő: {0}.  |
|  VMMarketplaceInvalidInput  |  hello vásárlási információk nem egyezik. Nem lehet toodeploy hello Piactéri lemezképről. Az operációs rendszer lemezének neve a következő: {0}.  |
|  VMMarketplaceInvalidInput  |  Terv adatainak hello kérelem egy virtuális gép létrehozásához a Piactéri lemezképből van szükség. Az operációs rendszer lemezének neve a következő: {0}.  |
|  VMNotFound  |  hello virtuális gép ({0}) nem található.  |
|  VMRedeploymentFailed  |  Virtuális gép "{0}" újratelepítés tooan belső hiba miatt sikertelen volt. Próbálkozzon újra később.  |
|  VMRedeploymentTimedOut  |  Virtuális gép a(z) "{0}' újbóli üzembe helyezése nem fejeződött be hello kiosztott idő. Akkor lehet, hogy a Befejezés sikeresen egy kis ideig. Más esetben újra hello kérelem.  |
|  VMStartTimedOut  |  Virtuális gép "{0}" nem indult el a kiosztott idő hello. hello virtuális gép továbbra is kezdheti el sikeresen. Hello energiaállapot később próbálja meg.  |


## <a name="next-steps"></a>Következő lépések
Ha további segítségre van szüksége, forduljon az Azure-szakértők hello [MSDN Azure és a Stack Overflow fórumok hello](https://azure.microsoft.com/support/forums/). Másik lehetőségként is fájl az Azure támogatási incidens. Nyissa meg toohello [az Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) válassza ki **támogatja az beszerzése**.
