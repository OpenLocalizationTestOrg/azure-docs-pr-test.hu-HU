Kétféle tárfióktípus létezik:

### <a name="general-purpose-storage-accounts"></a>Általános célú tárfiókok
Egy általános célú tárfiókok biztosít tooAzure tárolási szolgáltatások, például a táblák, üzenetsorok, fájlok, Blobok és Azure hozzáférést egyetlen fiókban virtuálisgép-lemezeket. Ez a tárfióktípus két teljesítményszinttel rendelkezik:

* Egy standard tárolási teljesítményszinttel, amely lehetővé teszi toostore táblák, üzenetsorok, fájlok, Blobok és Azure virtuálisgép-lemezeket.
* Egy prémium szintű Storage teljesítményszinttel, amely jelenleg csak az Azure virtuális gépek lemezeit támogatja. A Premium Storage részletesebb áttekintéséért lásd: [Premium Storage: High-performance Storage for Azure Virtual Machine Workloads](../articles/storage/common/storage-premium-storage.md) (Premium Storage: Nagy teljesítményű tárterület az Azure virtuális gépek számítási feladataihoz).

### <a name="blob-storage-accounts"></a>Blob Storage-fiókok
A Blob Storage-fiók egy speciális tárfiók a strukturálatlan adatok blobként (objektumként) való tárolására az Azure Storage-ban. BLOB storage-fiókok hasonló tooyour meglévő általános célú tárfiókok esetében, és minden hello nagy tartósságot, rendelkezésre állási, méretezhetőségét és teljesítményét szolgáltatások használja ma beleértve a 100 %-os API-konzisztenciát a blokkblobokhoz, és a hozzáfűző blobok. A csak blokkok és hozzáfűző blobok tárolását igénylő alkalmazásokhoz javasoljuk a Blob Storage-fiókok használatát.

> [!NOTE]
> A Blob Storage-fiókok csak a blokkblobokat és a hozzáfűző blobokat támogatják, a lapblobokat nem.
> 
> 

A BLOB storage-fiókokban elérhető hello **hozzáférési szint** attribútum, amely számítógépfiók létrehozása közben megadott, és később szükség szerint. Kétféle hozzáférési szint adható meg az adathozzáférési minták alapján:

* A **gyakran használt adatok** hozzáférési szint, amely azt jelzi, hogy hello objektumok hello tárfiókban lévő gyakrabban éri el. Ez lehetővé teszi toostore adatok alacsonyabb hozzáférési költség mellett.
* A **lassú** hozzáférési szint, amely azt jelzi, hogy hello objektumok hello tárfiókban lesz ritkább elérésére. Ez lehetővé teszi toostore adatokat, egy alacsonyabb adattárolási költség.

Ha az adatok használati módja hello változása, visszaválthat bármikor a hozzáférési rétegek közt. Változó hello hozzáférési szint további díjakat vonhat. További részletek: [Blob Storage-fiókok – Árak és számlázás](../articles/storage/blobs/storage-blob-storage-tiers.md#pricing-and-billing).

Blob Storage-fiókok további részletei: [Azure Blob Storage: Cool and Hot tiers](../articles/storage/blobs/storage-blob-storage-tiers.md) (Azure Blob Storage: Ritka és Gyakori hozzáférési szintek).

A storage-fiók létrehozása előtt rendelkeznie kell Azure-előfizetéssel, ez a csomag, amely lehetővé teszi az tooa különböző Azure-szolgáltatások eléréséhez. Az Azure használatát egy [ingyenes fiók](https://azure.microsoft.com/pricing/free-trial/) létrehozásával is elkezdheti. Ha eldöntötte, toopurchase előfizetési csomagot, választhat számos [beszerzési lehetőségek](https://azure.microsoft.com/pricing/purchase-options/). Ha Ön [MSDN-előfizető](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), ingyenes havi krediteket kap, amelyeket olyan Azure-szolgáltatásokhoz használhat, mint az Azure Storage. A mennyiségi díjszabásról lásd: [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).

Hogyan toocreate egy tárfiókot meg toolearn [hozzon létre egy tárfiókot](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) további részleteket. Egyedi nevű tárfiókok egyetlen előfizetés too200 másolatot hozhat létre. A tárfiókok kapacitásával kapcsolatos további információkért lásd: [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Az Azure Storage méretezhetőségi és teljesítménykorlátai).

