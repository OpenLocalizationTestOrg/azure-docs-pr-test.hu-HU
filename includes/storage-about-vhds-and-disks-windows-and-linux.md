
## <a name="about-vhds"></a>Tudnivalók a VHD-kről

hello Azure-ban használt virtuális merevlemezek a .vhd fájlok lapblobokat egy standard vagy prémium szintű storage-fiókban az Azure-ban tárolja. A lapblobokkal kapcsolatos további részletekért tekintse meg [a blokkblobokat és a lapblobokat bemutató cikket](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/). A prémium szintű tárolással kapcsolatos részletekért tekintse meg [a nagy teljesítményű Premium Storage szolgáltatással és az Azure virtuális gépekkel kapcsolatos cikket](../articles/storage/common/storage-premium-storage.md).

Azure rögzített VHD formátummal hello támogatja. Meghatározza a rögzített hello formátum hello lineárisan belül hello fájl ki logikai lemez, úgy, hogy a lemez eltolás X blob eltolás X tárolja. Hello blob hello végén kis élőláb hello VHD hello tulajdonságait ismerteti. Gyakran hello rögzített formátum Helypazarlás mert legtöbb lemez nagy nem használt tartományokat őket. Azonban Azure tárolja .vhd fájlok ritka formátuma, hogy mindkét rögzített hello hello előnyei, és a dinamikus lemezek, a hello azonos idő. További részletekért tekintse meg [a virtuális merevlemezek használatába bevezető cikket](https://technet.microsoft.com/library/dd979539.aspx).

Az összes .vhd fájlt, hogy egy forrás toocreate lemezként szeretne toouse vagy lemezképek írásvédettek Azure-ban. Amikor létrehoz egy lemezen vagy lemezképen, Azure lemásolja hello .vhd-fájlokat. Példányainak lehet csak olvasható vagy és-írhatónak és olvashatónak, attól függően, hogy a virtuális merevlemez hello használatáról.

Ha lemezképből hoz létre egy virtuális gépet, az Azure hello virtuális gépre, amelyik hello forrás .vhd fájl egy példányát a lemezt hozott létre. tooprotect véletlen törlése ellen, Azure helyezi a címbérlet toocreate használatban van, kép, operációsrendszer-lemez vagy adatlemezt forrás .vhd fájlok.

.Vhd fájlok törlése előtt törölni kell hello lemezen vagy lemezképen tooremove hello bérleti lesz szüksége. operációsrendszer-lemez a virtuális gép által éppen használt .vhd-fájllá. toodelete, törölheti hello virtuális gép, hello operációsrendszer-lemez, és hello forrás .vhd fájl egyszerre hello virtuális gép törlésével, és minden kapcsolódó lemezek törlése. Egy adatlemez forrásaként használt .vhd fájl törléséhez azonban több lépést is el kell végezni, egy megadott sorrendben. Először meg hello lemez hello virtuális gépről, majd törölje hello lemez leválasztása, és törölje a hello .vhd fájl.

> [!WARNING]
> Ha töröl egy forrás .vhd fájlt a tárolóból, vagy törli a saját tárfiókját, a Microsoft nem tudja visszaállítani az adatait.
> 

## <a name="types-of-disks"></a>Lemeztípusok 

Az Azure Disks 99,999%-os elérhetőséggel büszkélkedhet. Azure-lemezeket következetesen szállított vállalati szintű tartósságot, és egy iparágvezető Annualized hibaaránya %.

A lemezek létrehozásakor kétféle teljesítményszint közül választhat: Standard Storage és Premium Storage. Emellett két lemeztípus használható, a nem felügyelt és a felügyelt, amelyek mindkét teljesítményszinten elérhetőek.


### <a name="standard-storage"></a>Standard szintű Storage 

A merevlemez-meghajtókra épülő Standard Storage költséghatékony tárolási megoldás, amely emellett jó teljesítményt nyújt. A Standard Storage replikálható helyileg egy adatközpontban, vagy lehet georedundáns egy elsődleges és egy másodlagos adatközponttal. A tárolók replikálásával kapcsolatos további információkért tekintse át [az Azure Storage replikációjával kapcsolatos cikket](../articles/storage/common/storage-redundancy.md). 

A Standard Storage és a VM-lemezek együttes használatával kapcsolatos információkért tekintse át [a Standard Storage és a lemezek együttes használatát bemutató cikket](../articles/storage/common/storage-standard-storage.md).

### <a name="premium-storage"></a>Prémium szintű Storage 

Az SSD-kre épülő Premium Storage nagy teljesítményű, kis késleltetésű lemeztámogatást biztosít a nagy adatátviteli teljesítményt igénylő számítási feladatokat futtató virtuális gépek számára. Prémium szintű Storage használata DS, DSv2, GS, Ls vagy FS adatsorozat Azure virtuális gépeken. További információk: [Premium Storage](../articles/storage/common/storage-premium-storage.md).

### <a name="unmanaged-disks"></a>Nem felügyelt lemezek

Nem felügyelt lemezek hello hagyományos típusú virtuális gépek által használt lemezek. Ezekkel a saját storage-fiók létrehozása, és adjon meg, hogy a tárfiókot, hello lemezek létrehozása során. Toomake meg arról, hogy nem helyezett túl sok rendelkezik hello ugyanazt a tárfiókot, mert túllépi sikerült hello [méretezhetőségi célok](../articles/storage/common/storage-scalability-targets.md) hello tárfiókja (a 20 000 IOPS például), ami azt eredményezi, hello szabályozva virtuális gépeket. Nem felügyelt lemezek esetében az informatikai részleg toofigure hogyan toomaximize hello egy vagy több tároló fiókok tooget hello legjobb teljesítményt kívül a virtuális gépek használják ki.

### <a name="managed-disks"></a>Felügyelt lemezek 

A felügyelt hello tárolási fiókkezelés létrehozása/hello háttérben meg, és biztosítja, hogy nem rendelkezik tooworry kapcsolatos hello méretezhetőségi korlátok hello tárfiók lemezek kezeli. Egyszerűen hello lemezméretet és hello teljesítményszinttel (Standard vagy prémium), és a Azure hoz létre, és kezeli a hello lemez meg. Akár lemezek hozzáadása, illetve a virtuális gép hello felfelé és lefelé méretezési, nincs használatban hello tárolás tooworry. 

Is kezelheti az egyéni lemezképek az egyes Azure-régió, egy tárfiókot, és használhatja őket több száz. toocreate hello a virtuális gépek ugyanahhoz az előfizetéshez. A felügyelt lemezek kapcsolatos további információkért lásd: hello [felügyelt lemezekhez – áttekintés](../articles/virtual-machines/windows/managed-disks-overview.md).

Javasoljuk, hogy Azure kezelt lemezek használatakor az új virtuális gépekhez, és, hogy az előző nem felügyelt lemezek toomanaged lemezek konvertálása, hello tootake előnyeit felügyelt lemezek számos szolgáltatásához.

### <a name="disk-comparison"></a>Lemezek összehasonlítása

a következő táblázat hello itt prémium és Standard összehasonlítása mindkettő nem felügyelt, és úgy dönt, hogy milyen toouse lemezek toohelp felügyelt.

|    | Prémium szintű Azure-lemez | Standard szintű Azure-lemez |
|--- | ------------------ | ------------------- |
| Lemez típusa | SSD-k | HDD-k  |
| Áttekintés  | SSD-alapú, nagy teljesítményű, kis késleltetésű lemeztámogatás a nagy adatátviteli teljesítményt igénylő számítási feladatokat vagy az üzletmenet szempontjából kritikus fontosságú éles környezeteket futtató virtuális gépek számára | HDD-alapú, költséghatékony lemeztámogatás fejlesztési/tesztelési virtuálisgép-forgatókönyvekhez |
| Forgatókönyv  | Éles, teljesítményérzékeny számítási feladatok | Fejlesztés/tesztelés, nem kritikus, <br>rendszertelen hozzáférés |
| Lemezméret | P4: 32 GB (csak a felügyelt lemezek)<br>P6: 64 GB (csak a felügyelt lemezek)<br>P10: 128 GB<br>P20: 512 GB<br>P30: 1024 GB<br>P40: 2048 GB<br>P50: 4095 GB | Nem felügyelt lemezek: 1 GB-os – 4 TB-os (4095 GB) <br><br>Managed Disks:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1024 GB <br>S40: 2048 GB<br>S50: 4095 GB| 
| Lemezenkénti maximális átviteli sebesség | 250 MB/s | 60 MB/s | 
| Lemezenkénti maximális IOPS-érték | 7500 IOPS | 500 IOPS | 

