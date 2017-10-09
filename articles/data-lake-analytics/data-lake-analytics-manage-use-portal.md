---
title: "aaaManage Azure Data Lake Analytics használatával hello Azure portálon |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage Data Lake Analytics könyvvitelének, adatok adatforrásokat, felhasználók, és a feladatokat."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: f63ccdfae79772c92e92462194e8cdc636a73dc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-hello-azure-portal"></a>Azure Data Lake Analytics kezelése hello Azure-portál használatával
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, a fiók adatforrások, a felhasználók és a feladatok használatával hello Azure-portálon. toosee kapcsolatos témakörök más eszközök használatával kapcsolatos kattintson egy fülre az hello oldal hello tetején.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Data Lake Analytics-fiókok kezelése

### <a name="create-an-account"></a>Fiók létrehozása

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson az **Új** > **Intelligencia és elemzés** > **Data Lake Analytics** elemre.
3. Válassza ki a következő elemek hello értékeit: 
   1. **Név**: hello hello Data Lake Analytics-fiók nevét.
   2. **Előfizetés**: hello hello fiókhoz használt Azure-előfizetést.
   3. **Erőforráscsoport**: hello Azure erőforráscsoport mely toocreate hello fiók. 
   4. **Hely**: hello Azure datacenter hello Data Lake Analytics-fiók. 
   5. **Data Lake Store**: hello alapértelmezett tároló toobe hello Data Lake Analytics-fiókhoz használt. hello Azure Data Lake Store-fiók és a fióknak szerepelnie kell a Data Lake Analytics hello hello ugyanazon a helyen.
4. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="delete-a-data-lake-analytics-account"></a>A Data Lake Analytics-fiók törlése

Data Lake Analytics-fiók törlése előtt törölje az alapértelmezett Data Lake Store-fiók.

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **Törlés** gombra.
3. Hello fiók neve.
4. Kattintson a **Törlés** gombra.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Adatforrások kezelése

A Data Lake Analytics a következő adatforrások hello támogatja:

* Data Lake Store
* Azure Storage

Data Explorer toobrowse adatforrások használja, és hajtsa végre az alapvető felügyeleti műveletet. 

### <a name="add-a-data-source"></a>Egy adatforrás hozzáadása

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **adatforrások**.
3. Kattintson a **adja hozzá az adatforrást**.
    
   * hello fiók nevét és a hozzáférési toohello fiók toobe képes tooquery tooadd Data Lake Store-fiók, kell azt.
   * tooadd Azure Blob Storage tárolóban, és van szükség hello tárfiók hello kulcsára. toofind őket, lépjen toohello tárfiók hello portálon.

## <a name="set-up-firewall-rules"></a>Tűzfalszabályok beállítása

Hello hálózati szintű hozzáférés tooyour Data Lake Analytics-fiók zárolása Data Lake Analytics toofurther is használhatja. Tűzfal engedélyezése, IP-cím vagy IP-címtartomány megadása a megbízható ügyfeleket. Ezeket a mértékeket engedélyezése után csak hello definiált tartományon belüli hello IP-címmel rendelkező ügyfelek kapcsolódhatnak toohello tároló.

Ha más Azure-szolgáltatásokkal, például az Azure Data Factory vagy a virtuális gépek, toohello Data Lake Analytics-fiók, győződjön meg arról, hogy **Azure-szolgáltatások engedélyezése** van kapcsolva **a**. 

### <a name="set-up-a-firewall-rule"></a>A tűzfalszabályok beállítása

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. A hello hello bal oldali menüben kattintson **tűzfal**.

## <a name="add-a-new-user"></a>Új felhasználó hozzáadása

Használhatja a hello **varázslót** tooeasily konfigurálta a új Data Lake-felhasználókat.

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. A bal oldali a hello **bevezetés**, kattintson a **varázslót**.
3. Válasszon ki egy felhasználót, és kattintson **válasszon**.
4. Válassza ki a szerepkört, és kattintson **válasszon**. fel egy új Azure Data Lake, jelölje be hello fejlesztői toouse tooset **Data Lake Analytics fejlesztői** szerepkör.
5. Válassza ki a hello hozzáférés-vezérlési listák (ACL) hello U-SQL-adatbázisok számára. Ha elégedett a kiválasztott beállításokat, kattintson **válasszon**.
6. Válassza ki a fájlok hello ACL-listát. Nem hello alapértelmezett tároló, hello gyökérmappához hello ACL-ek módosítása "/" és hello vagy mappához. Kattintson a **Kiválasztás** gombra.
7. Tekintse át a kiválasztott módosításokat, és kattintson **futtatása**.
8. Ha hello varázsló végzett, kattintson a **végzett**.

## <a name="manage-role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés kezelése

Más Azure-szolgáltatásokkal, például a felhasználók hogyan használják a szerepköralapú hozzáférés-vezérlést (RBAC) toocontrol hello szolgáltatást is használhatja.

hello szabványos RBAC-szerepkörök hello a következő képességekkel rendelkezik:
* **Tulajdonos**: feladatok küldéséhez, feladatok figyelése, szakítsa bármely felhasználó, és hello fiók konfigurálása.
* **A közreműködői**: feladatok küldéséhez, feladatok figyelése, szakítsa bármely felhasználó, és hello fiók konfigurálása.
* **Olvasó**: figyelheti a feladat.

Hello Data Lake Analytics fejlesztői szerepkör tooenable U-SQL fejlesztők toouse hello Data Lake Analytics szolgáltatás használata. Hello Data Lake Analytics fejlesztői szerepkört is használhatja:
* Feladatok elküldéséhez.
* Bármely felhasználó által küldött feladatok feladat állapotát és hello állapotát figyeli.
* Lásd: hello U-SQL-parancsfájlok feladatokból bármely felhasználó által küldött.
* Csak a saját feladatok megszakítása

### <a name="add-users-or-security-groups-tooa-data-lake-analytics-account"></a>Felhasználók vagy biztonsági csoportok tooa Data Lake Analytics-fiók hozzáadása

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **hozzáférés-vezérlés (IAM)** > **Hozzáadás**.
3. Szerepkör kiválasztása.
4. Felhasználó hozzáadása.
5. Kattintson az **OK** gombra.

>[!NOTE]
>Ha egy felhasználónak vagy biztonsági csoportot kell toosubmit feladatok, engedéllyel rendelkezik a hello store-fiók is szükséges. További információkért lásd: [adatok a védelméről a Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Feladatok kezelése

### <a name="submit-a-job"></a>Feladat elküldése

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.

2. Kattintson a **új feladat**. Az egyes feladatokhoz konfigurálása:

    1. **Feladat neve**: hello feladat hello nevét.
    2. **Prioritás**: alacsonyabb számok magasabb prioritással rendelkezik. Ha a két feladat várólistára kerülnek, hello alacsonyabb prioritással rendelkező első futtatja.
    3. **Párhuzamossági**: hello számítási maximális száma a feladat tooreserve dolgozza fel.

3. Kattintson a **Feladat elküldése** elemre.

### <a name="monitor-jobs"></a>Feladatok figyelése

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **összes feladat megtekintéséhez**. Hello aktív és a közelmúltban befejezett levő összes feladatnak hello fiók listája látható.
3. Ha úgy gondolja, a **szűrő** hello feladatok által talált toohelp **időtartomány**, **feladat neve**, és **Szerző** értékek. 

### <a name="monitoring-pipeline-jobs"></a>Feldolgozási sor feladatok figyelése
Feladatok, amelyek egy folyamat része a munka együtt, általában sorrendben történik, tooaccomplish egy adott forgatókönyvhöz. Például hogy egy folyamatot, amely megtisztítja, kiolvassa, átalakítja, összesíti az ügyfél insights használata. Feldolgozási sor feladatok azonosított hello "Futószalag" tulajdonság használatát, ha hello feladat el lett küldve. ADF V2 használatával ütemezett feladatok automatikusan lesz feltöltve ezt a tulajdonságot. 

tooview folyamatok részét képező U-SQL-feladatok listája: 

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiókok.
2. Kattintson a **Insights feladat**. "Az összes feladat" lapon alapértelmezett lesz, futó, listáját tartalmazó hello sorba, és a feladat befejeződött.
3. Kattintson a hello **csővezeték feladatok** fülre. Feldolgozási sor feladatokra minden adatcsatorna összesített statisztikák együtt jelenik meg.

### <a name="monitoring-recurring-jobs"></a>Ismétlődő feladatok figyelése
Ismétlődő feladat egyike, amely rendelkezik hello azonos üzleti logika de használ a különböző bemeneti adatokat, minden egyes futásakor. Ideális esetben ismétlődő feladatok mindig sikeres legyen, és viszonylag állandó végrehajtási idővel; Ezek közül a viselkedésmódok a figyelés segítségével ellenőrizze, hogy hello feladat kifogástalan. Ismétlődő feladatok azonosított hello "Ismétlődési" tulajdonság használatával. ADF V2 használatával ütemezett feladatok automatikusan lesz feltöltve ezt a tulajdonságot.

tooview ismétlődő U-SQL feladatok listáját: 

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiókok.
2. Kattintson a **Insights feladat**. "Az összes feladat" lapon alapértelmezett lesz, futó, listáját tartalmazó hello sorba, és a feladat befejeződött.
3. Kattintson a hello **ismétlődő feladatok** fülre. Ismétlődő feladatok listája jelenik meg minden ismétlődő feladat összesített statisztikák együtt.

## <a name="manage-policies"></a>Házirendek kezelése

### <a name="account-level-policies"></a>Fiók szintű szabályzatok

Ezek a házirendek alkalmazása tooall feladatok Data Lake Analytics-fiók.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>A Data Lake Analytics-fiók ausztráliai maximális száma
A szabályzatok vezérlik hello Analytics egységek száma (ausztráliai) a Data Lake Analytics-fiók használható. Alapértelmezés szerint hello értéke too250. Például, ha az alapérték too250 ausztráliai, akkor 250 hozzárendelt ausztráliai tooit fut egy feladat, és 25 futtató 10 feladatok minden ausztráliai. További feladatok tartalmazó sorba hello futó feladat befejezéséig. Futó feladat befejeződése után ausztráliai feliratkozott hello felszabadítását feladatok toorun sorba.

toochange hello száma ausztráliai a Data Lake Analytics-fiókhoz:

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **Tulajdonságok** elemre.
3. A **maximális ausztráliai**, helyezze át a hello csúszkát tooselect értéket, vagy hello érték hello mezőben adja meg. 
4. Kattintson a **Save** (Mentés) gombra.

> [!NOTE]
> Ha meg kell ennél nagyobb (250) alapértelmezett hello ausztráliai, hello portálon kattintson **súgó + támogatás** toosubmit egy támogatási kérést. Ausztráliai érhető el a Data Lake Analytics-fiók hello száma növelhető.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Az egyidejűleg futtatható feladatok maximális számát
Egy házirend szabályozza, hogy hány feladatok futtatható hello ugyanannyi időt vesz igénybe. Alapértelmezés szerint ez az érték too20 van beállítva. Ha a Data Lake Analytics ausztráliai elérhető, új feladatok csak ütemezett toorun azonnal hello teljes futó feladatok száma eléri ezt a házirendet hello értéket. Amikor elérte az egyidejűleg futtatható feladatok maximális száma hello, további feladat várólistára prioritási sorrendben mindaddig, amíg legalább egy futó feladat befejeződik (attól függően, hogy AU rendelkezésre állás).

egyidejűleg futtatható feladatok toochange hello száma:

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **Tulajdonságok** elemre.
3. A **maximális számát a futó feladatok**, helyezze át a hello csúszkát tooselect értéket, vagy hello érték hello mezőben adja meg. 
4. Kattintson a **Save** (Mentés) gombra.

> [!NOTE]
> Ha több, mint az alapértelmezett (20) hello portálon, a feladatok száma hello kattintson toorun kell **súgó + támogatás** toosubmit egy támogatási kérést. a Data Lake Analytics-fiók egyidejűleg futtatható feladatok hello száma növelhető.
>

#### <a name="how-long-tookeep-job-metadata-and-resources"></a>Mennyi ideig tookeep feladat metaadatok és erőforrások 
A felhasználók a U-SQL feladatok futtatása, megtartja a hello Data Lake Analytics szolgáltatás és az összes kapcsolódó fájlt. Kapcsolódó fájlok hello U-SQL parancsfájl, a hello U-SQL parancsfájl, a lefordított erőforrások és a statisztika hivatkozott hello DLL-fájlok közé tartoznak. hello fájlok mappájában hello /system/ hello alapértelmezett Azure Data Lake-tárfiókra. Ez az irányelv szabályozza, hogy mennyi ideig tárolja ezeket az erőforrásokat automatikusan törlés előtt (hello alapértelmezett érték 30 nap). Ezeket a fájlokat is használhatja, a hibakeresés, valamint a teljesítményének hangolása lesz futtassa újra a jövőbeli hello feladatok.

toochange mennyi ideig tookeep feladat metaadatok és erőforrások:

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **Tulajdonságok** elemre.
3. A **nap tooRetain Feladatlekérdezéseit**, helyezze át a hello csúszkát tooselect értéket, vagy hello érték hello mezőben adja meg.  
4. Kattintson a **Save** (Mentés) gombra.

### <a name="job-level-policies"></a>Feladat szintű szabályzatok
A feladat szintű házirendek, megadhatja a maximális ausztráliai hello és hello a legnagyobb prioritású virtuális gép, amely az egyes felhasználók (vagy a meghatározott biztonsági csoportok tagjai) feladatok, ugyanúgy lehet beállítani. Ez lehetővé teszi, megadhatja a felhasználók hello költségeit. Ezenkívül lehetővé teszi, hogy az ütemezett feladatok vezérlő hello hatással lehet a magas prioritású termelési a futó feladatok hello ugyanazt a Data Lake Analytics-fiókot.

A Data Lake Analytics rendelkezik, amely lehet hello feladat szinten két házirendek:

* **AU felső határ az egyes feladatok**: a felhasználók csak elküldhetik ausztráliai toothis számú feladatok. Alapértelmezés szerint ezt a határt hello ugyanaz, mint a hello AU maximális száma hello fiók.
* **Prioritás**: csak a küldje el a kisebb vagy egyenlő toothis prioritásérték feladatok. Vegye figyelembe, hogy a nagyobb szám azt jelenti, hogy a kisebb prioritással. Alapértelmezés szerint értéke too1, amely hello lehető legmagasabb prioritású.

Nincs minden fiókhoz beállított alapértelmezett házirend. hello alapértelmezett házirend tooall felhasználók hello fiók vonatkozik. További házirendjeinek beállítása meghatározott felhasználókhoz és csoportokhoz. 

> [!NOTE]
> Fiók szintű és a feladat szintű házirendeket alkalmazza egyszerre.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Egy szabályzat egy adott felhasználó vagy csoport hozzáadása

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **Tulajdonságok** elemre.
3. A **feladat elküldése korlátok**, hello kattintson **házirend hozzáadása** gombra. Ezután válassza ki, vagy adja meg a következő beállítások hello:
    1. **Házirend neve számítási**: Adja meg a házirend nevét, tooremind a hello házirend hello céljának.
    2. **Válassza ki a felhasználó vagy csoport**: válassza hello felhasználót vagy csoportot a házirend vonatkozik.
    3. **Hello feladat AU korlát beállítása**: toohello alkalmazó hello AU korlát beállítása a kijelölt felhasználót vagy csoportot.
    4. **Hello prioritás korlát beállítása**: hello prioritás korlát beállítása toohello alkalmazó kiválasztott felhasználó vagy csoport.

4. Kattintson az **OK** gombra.

5. hello új házirend megjelenik hello **alapértelmezett** házirend a tábla **feladat elküldése korlátok**. 

#### <a name="delete-or-edit-an-existing-policy"></a>Törölje vagy egy meglévő házirend szerkesztése

1. A hello Azure-portálon válassza a tooyour Data Lake Analytics-fiók.
2. Kattintson a **Tulajdonságok** elemre.
3. A **feladat elküldése korlátok**, keresés hello kívánt házirendet, tooedit.
4.  toosee hello **törlése** és **szerkesztése** beállítások hello tábla hello jobb szélső oszlopában kattintson **...** .

### <a name="additional-resources-for-job-policies"></a>További erőforrásokat a feladat házirendek
* [Házirend által írt blogbejegyzés áttekintése](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Fiók szintű szabályzatok által írt blogbejegyzés](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Feladat szintű szabályzatok által írt blogbejegyzés](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Következő lépések

* [Az Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md)
* [Ismerkedés a Data Lake Analytics hello Azure-portál használatával](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics kezelése az Azure PowerShell használatával](data-lake-analytics-manage-use-powershell.md)

