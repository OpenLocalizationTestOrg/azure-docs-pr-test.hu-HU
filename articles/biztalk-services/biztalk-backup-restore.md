---
title: "aaaCreate és a biztonsági másolat visszaállításával a BizTalk szolgáltatások |} Microsoft Docs"
description: "BizTalk szolgáltatások részét képező, biztonsági mentését és helyreállítását. Megtudhatja, hogyan toocreate és a biztonsági másolat visszaállításával és határozza meg, mi lekérdezi a biztonsági mentés. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 32356ad870678fa5fd5bbbbf13d9377188f770a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-backup-and-restore"></a>BizTalk Services: Biztonsági mentés és visszaállítás

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk-szolgáltatásokhoz kínálja biztonsági mentését és helyreállítását. Ez a témakör ismerteti, hogyan toobackup és visszaállítási BizTalk szolgáltatások használatával hello a klasszikus Azure portálon.

Is biztonsági másolatot készíthet BizTalk szolgáltatásokat hello segítségével [BizTalk szolgáltatás REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [!NOTE]
> Hibrid kapcsolatok nem készül biztonsági mentés, hello Edition függetlenül. A hibrid kapcsolatok újra létre kell hoznia.


## <a name="before-you-begin"></a>Előkészületek
* Biztonsági mentés és visszaállítás nem lehet elérhető összes kiadása esetén. Lásd: [BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md).
* Hello klasszikus Azure-portált használja, hozzon létre egy igény szerinti biztonsági mentést, vagy hozzon létre ütemezett biztonsági mentés. 
* Biztonsági mentés a tartalom lehet a visszaállított toohello BizTalk szolgáltatás azonos vagy tooa új BizTalk szolgáltatás. toorestore hello BizTalk szolgáltatás használata hello azonos nevű, meglévő BizTalk szolgáltatás hello törölni kell, és hello neve elérhetőnek kell lennie. Ha töröl egy BizTalk szolgáltatás, a hello szeretett volna azonos hosszabb időt is igénybe vehet neve toobe érhető el. Ha nem várja meg a hello azonos toobe rendelkezésre álló nevet, majd tooa visszaállítása új BizTalk szolgáltatás.
* BizTalk szolgáltatások lehet visszaállított toohello azonos kiadást vagy újabb kiadását. BizTalk szolgáltatások tooa visszaállítása korábbi kiadását, amikor hello a biztonsági mentés készült, nem támogatott.
  
    Például egy biztonsági alapszintű kiadására lehet hello segítségével vissza toohello verziót prémium kiadásúra. Prémium szintű kiadása nem lehet hello segítségével biztonsági toohello Standard Edition visszaállítása.
* hello EDI-vezérlő számok készül biztonsági másolat hello vezérlő számok toomaintain folyamatosságát. Ha üzenetek feldolgozása hello utolsó biztonsági mentés óta, a biztonsági mentési tartalom visszaállítása okozhat ismétlődő vezérlő számokat.
* Ha egy kötegelt aktív üzenetek, dolgozza fel hello kötegelt **előtt** biztonsági másolat készítése. (A szükséges vagy ütemezett) a biztonsági másolat létrehozásakor a kötegek üzenetek soha nem kerülnek. 
  
    **Biztonsági másolatot készít aktív üzeneteket kötegekben, ha ezek az üzenetek nem készül biztonsági mentés, és ezért elvesznek.**
* Választható lehetőség: A hello BizTalk szolgáltatások portálja, állítsa le felügyeleti műveleteket sem.

## <a name="create-a-backup"></a>Biztonsági mentés létrehozása
A biztonsági mentés akkor van szükség, tetszőleges időpontban, és teljesen vezérli akkor. Ez a rész felsorolja hello lépéseket toocreate a biztonsági mentéseket hello klasszikus Azure portál, beleértve:

[Az igény szerinti biztonsági mentése](#backupnow)

[A biztonsági mentés ütemezése](#backupschedule)

#### <a name="backupnow"></a>Az igény szerinti biztonsági mentése
1. Hello a klasszikus Azure portálon, válassza ki **BizTalk szolgáltatások**, majd válassza ki hello BizTalk szolgáltatás kívánt toobackup és.
2. A hello **irányítópult** lapon jelölje be **készítsen biztonsági másolatot** hello lap hello alján.
3. Adja meg a biztonsági másolat neve. Adja meg például *myBizTalkService*BU*dátum*.
4. Válasszon ki egy blob storage-fiók és a select hello pipa toostart hello biztonsági mentés.

Hello biztonsági mentés befejeztével, miután egy tároló meg hello biztonsági másolat nevű hello storage-fiók jön létre. Ebben a tárolóban a BizTalk szolgáltatás biztonsági mentési konfigurációját tartalmazza.

#### <a name="backupschedule"></a>A biztonsági mentés ütemezése
1. Hello a klasszikus Azure portálon, válassza ki **BizTalk szolgáltatások**, válassza ki a BizTalk szolgáltatás kívánt nevet tooschedule hello biztonsági mentést, majd válassza ki a hello hello **konfigurálása** fülre.
2. Set hello **biztonsági mentés állapotának** túl**automatikus**. 
3. Jelölje be hello **Tárfiók** toostore hello biztonsági mentési, adja meg a hello **gyakorisága** toocreate hello, és mennyi ideig tookeep hello biztonsági mentés (**megőrzési nap**):
   
    ![][AutomaticBU]
   
    **Megjegyzések**     
   
   * A **megőrzési nap**, hello megőrzési időszak hello biztonsági mentési gyakoriság nagyobbnak kell lennie.
   * Válassza ki **mindig maradjon meg legalább egy biztonsági másolat**, akkor is, ha hello megőrzési időszak lejárt.
4. Kattintson a **Mentés** gombra.

Egy ütemezett biztonsági mentési feladat futtatása megadott hello tárfiókban lévő létrehoz egy tároló (toostore biztonsági mentési adatok). hello hello tároló neve *nevét – dátum és idő BizTalk szolgáltatás*. 

Ha hello BizTalk szolgáltatás irányítópultját mutatja egy **sikertelen** állapota:

![Utolsó ütemezett biztonsági mentés állapota][BackupStatus] 

hello a hivatkozás megnyitja hello felügyeleti szolgáltatások műveletnaplók toohelp hibaelhárítása. Lásd: [BizTalk szolgáltatások: műveletnaplók használata – hibaelhárítás](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Visszaállítás
Visszaállíthatja a biztonsági mentések hello a klasszikus Azure portálon, illetve hello [visszaállítása BizTalk szolgáltatás REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582). Ez a szakasz hello lépéseket toorestore hello klasszikus portál használatával.

#### <a name="before-restoring-a-backup"></a>Biztonsági másolat visszaállítása előtt
* Új követési archiválás és tárolók figyelése BizTalk szolgáltatás visszaállítása közben lehet megadni.
* hello azonos EDI futási adataihoz helyreáll. hello EDI futásidejű biztonsági mentés hello vezérlő számokat tárolja. vissza hello vezérlő számok sorozatát hello biztonsági másolat készítésének idején hello szerepelnek. Ha üzenetek feldolgozása hello utolsó biztonsági mentés óta, a biztonsági mentési tartalom visszaállítása okozhat ismétlődő vezérlő számokat.

#### <a name="restore-a-backup"></a>Visszaállítás biztonsági másolatból
1. Hello a klasszikus Azure portálon, válassza ki **új** > **alkalmazásszolgáltatások** > **BizTalk szolgáltatás** > **visszaállítása **:
   
    ![Visszaállítás biztonsági másolatból][Restore]
2. A **biztonsági mentési URL-cím**, válassza ki a hello ikonja, és bontsa ki, hogy a tároló BizTalk szolgáltatás konfigurációs biztonsági másolat hello hello Azure storage-fiók. Hello tároló és hello jobb oldali ablaktáblában jelölje ki a megfelelő biztonsági mentése .txt fájl hello. 
   <br/><br/>
   Válassza ki **nyitott**.
3. A hello **visszaállítás BizTalk szolgáltatás** lapján adjon meg egy **BizTalk szolgáltatás neve** , és ellenőrizze a hello **tartomány URL-cím**, **Edition**, és **Régió** hello vissza BizTalk szolgáltatás számára. **Hozzon létre egy új SQL-adatbázispéldány** az adatbázis követési hello:
   
    ![][RestoreBizTalkService]
   
    Válassza ki a hello tovább nyílra.
4. Ellenőrizze az SQL-adatbázis hello hello nevét, adja meg, hogy a kiszolgáló hello fizikai kiszolgálón, ahol hello SQL-adatbázis fog létrejönni, és a felhasználónév/jelszó.

    Ha tooconfigure hello SQL adatbázis-kiadás, méretének és egyéb tulajdonságai, jelölje be **speciális adatbázis-beállítások konfigurálása**. 

    Válassza ki a hello tovább nyílra.

1. Hozzon létre egy új tárfiókot, vagy adja meg egy meglévő tárfiók hello BizTalk szolgáltatás.
2. Válassza ki a hello pipa toostart hello visszaállítása.

Miután hello visszaállítása sikeresen befejeződött, egy új BizTalk szolgáltatás szerepel a hello BizTalk szolgáltatások lapon a klasszikus Azure portálon hello felfüggesztett állapotban.

### <a name="postrestore"></a>Biztonsági másolat visszaállítása után
hello BizTalk szolgáltatás mindig visszaállítása van egy **felfüggesztett** állapotát. Ebben az állapotban végrehajtott bármilyen konfigurációs módosításokat előtt hello új környezetben is használható, beleértve teheti meg:

* Ha BizTalk szolgáltatás alkalmazások hello Azure BizTalk Services SDK használatával hozott létre, szükség lehet a tootooupdate hello Access Control (ACS) hitelesítő adatokat ezen alkalmazások toowork vissza hello környezettel.
* A BizTalk szolgáltatás tooreplicate egy meglévő BizTalk Service-környezet visszaállítása Ebben a helyzetben megállapodások hello eredeti BizTalk szolgáltatások portálon konfigurált FTP forrásmappa, használó esetén szükség lehet az újonnan visszaállított hello környezet toouse egy másik FTP forrásmappa tooupdate hello megállapodásokat. Ellenkező esetben előfordulhat toopull próbált két különböző megállapodások hello ugyanazt az üzenetet.
* Toohave több BizTalk szolgáltatás környezet visszaállítás után ellenőrizze, hogy hello megfelelő környezet hello Visual Studio alkalmazást, a PowerShell-parancsmagok, a REST API-k vagy a kereskedelmi Partner Management OM API-k célozhat meg.
* Egy jó gyakorlat tooconfigure automatikus biztonsági mentés az újonnan visszaállított hello BizTalk szolgáltatás környezet is.

toostart hello BizTalk szolgáltatás hello klasszikus Azure portálon válassza hello a visszaállított BizTalk szolgáltatás, és válassza ki **folytatása** hello tálcán. 

## <a name="what-gets-backed-up"></a>Mi a biztonsági mentés beolvasása
Biztonsági másolat létrehozásakor hello következő elemek készül biztonsági másolat:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Biztonsági mentésben szereplő elemek</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Az Azure BizTalk szolgáltatások portálja</strong></td>
</tr> 
<tr>
<td>Konfigurációs és futásidejű</td> 
<td>
<ul>
<li>Partnerek és a profil részletei</li>
<li>Egyezmények</li>
<li>Egyéni szerelvényeket telepített</li>
<li>Telepített hidak</li>
<li>Tanúsítványok</li>
<li>Telepített átalakítások</li>
<li>Folyamatok</li>
<li>BizTalk szolgáltatások portálja hello az sablonok</li>
<li>X12 ST01 és GS01</li>
<li>Vezérlő számok (EDI)</li>
<li>AS2-üzenet MIC értékek</li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2">
 <strong>Azure BizTalk szolgáltatás</strong></td>
</tr> 
<tr>
<td>SSL-tanúsítvány</td> 
<td>
<ul>
<li>SSL-Tanúsítványadatok</li>
<li>SSL-tanúsítvány jelszava</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk szolgáltatás beállításai</td> 
<td>
<ul>
<li>Méretezési egységek száma</li>
<li>Kiadás</li>
<li>Termékverzió</li>
<li>Régió/adatközpont</li>
<li>Access Control Service (ACS) névtér és a kulcs</li>
<li>Adatbázis-kapcsolati karakterlánc nyomon követése</li>
<li>Archiválási tárolási fiók kapcsolati karakterlánc</li>
<li>Figyelési tárolási fiók kapcsolati karakterlánc</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>További elemek</strong></td>
</tr> 
<tr>
<td>Adatbázis nyomon követése</td> 
<td>Hello BizTalk szolgáltatás létrehozásakor hello követési adatainak van-e megadva, beleértve a hello Azure SQL adatbázis-kiszolgáló és a hello követési nevét. hello adatbázis nyomon követése nem automatikusan biztonsági másolat.
<br/><br/>
<strong>Fontos</strong><br/>
Ha hello követési adatbázis törlődik, és hello adatbázisban kell helyreállítani, a korábbi biztonsági léteznie kell. Ha a biztonsági mentés nem létezik, hello adatbázis nyomon követése és annak adatai nincsenek helyreállítható. Ebben a helyzetben hozzon létre új követési adatbázis hello azonos adatbázisnevet. A Georeplikáció ajánlott.</td>
</tr> 
</table>

## <a name="next"></a>Következő lépés
toocreate Azure BizTalk szolgáltatások a klasszikus Azure portál, nyissa meg túl hello[BizTalk szolgáltatások: kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280). alkalmazások, lépjen túl létrehozásának toostart[Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Lásd még:
* [Biztonsági mentési BizTalk szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Állítsa vissza biztonsági másolatból BizTalk szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Kiépítési állapot diagramja](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Irányítópult, Figyelés és Méret lapok](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: Szabályozás](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Kiállító neve és kiállító kulcsa](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

