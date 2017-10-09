Adatok lemezek tooa Linux virtuális gép hozzáadásakor esetleg felmerülő problémákat, ha egy lemez nem létezik: LUN 0. Ha egy lemez hello segítségével manuálisan hozzáadni `azure vm disk attach-new` parancsot, és adja meg a LUN-t (`--lun`) ahelyett, hogy így hello Azure platformon toodetermine hello megfelelő logikai Egységet, gondoskodunk, hogy a lemez már létezik / LUN azonosítójú 0 van jelen. 

Vegye figyelembe a következő példa egy részlet hello kimenetét megjelenítő hello `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

hello két adatlemezek lehet létrehozni a LUN 0 és LUN-1 (hello első oszlopa hello `lsscsi` részletek kimeneti `[host:channel:target:lun]`). Mindkét lemeznek accessbile a virtuális gép hello belül kell lennie. Ha manuálisan kellett megadott hello hello második lemez LUN 2 és a logikai egység 1 hozzáadott első lemez toobe, a virtuális Gépen belül nem jelenhet meg hello lemezei megfelelően.

> [!NOTE]
> hello Azure `host` érték 5 ezekben a példákban, de ez választja tárolási hello típusától függően változhat.
> 
> 

Ez lemez azonban nem egy Azure probléma, de a mely hello Linux kernel követi hello SCSI-specifikációk hello módon. Hello Linux kernel hello SCSI-buszhoz csatlakoztatott eszközöket keres, ha egy eszköz kell található LUN azonosítójú 0 ahhoz, hogy hello rendszer toocontinue további eszközök keresése. Ilyen:

* Tekintse át a hello kimenete `lsscsi` egy adatok lemez tooverify, hogy rendelkezik-e a lemez LUN azonosítójú 0 hozzáadása után.
* Ha a lemez nem jelenik meg helyesen a virtuális Gépen belül, a lemez LUN azonosítójú 0 meglétének ellenőrzése.

