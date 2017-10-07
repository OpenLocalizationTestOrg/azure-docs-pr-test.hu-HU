---
title: "aaaFailover csoportok és aktív georeplikáció - Azure SQL Database |} Microsoft Docs"
description: "Aktív georeplikáció automatikus feladatátvételt csoportok lehetővé teszi az adatbázis bármely hello Azure adatközpontjaiban és a nem tervezett kimaradás esetén hello autoomatically feladatátvételi toosetup 4 replikái."
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2a29f657-82fb-4283-9a83-e14a144bfd93
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 07/10/2017
ms.author: sashan
ms.openlocfilehash: 7a39af8505624c0f19c5abccff051af836b1f9bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-failover-groups-and-active-geo-replication"></a>Áttekintés: Feladatátvételi csoportok és aktív georeplikáció
Aktív georeplikáció lehetővé teszi a hello másodlagos adatbázisok olvasható toofour tooconfigure ugyanazon vagy másik adatközpont-helyek (régió). Másodlagos adatbázisok a lekérdezésre, és feladatátvételi hello esetben data center kimaradás vagy hello meggátoló tooconnect toohello elsődleges adatbázis érhetők el. hello kell kezdeményezni a feladatátvételt manuálisan hello alkalmazás hello felhasználó. A feladatátvétel után hello új elsődleges van egy másik kapcsolati végpontot. 

> [!NOTE]
> Aktív georeplikáció érhető el az összes olyan adatbázis összes szolgáltatásrétegekben minden régióban.
>  

Az Azure SQL adatbázis automatikus feladatátvételt csoportok (az előzetes verzió), egy SQL-adatbázis szolgáltatás tooautomatically kezelése georeplikáció kapcsolat, a kapcsolódási és a feladatátvételi méretekben. Vele hello ügyfelek nyereség hello képességét tooautomatically hello másodlagos régióban több kapcsolódó adatbázisok helyreállítását követően katasztrofális regionális meghibásodásokkal vagy nem tervezett eseményeket, amelyek az SQL Database szolgáltatás hello teljes vagy részleges adatvesztéshez vezethet rendelkezésre állási hello elsődleges régióban. Emellett használhatnak hello olvasható másodlagos adatbázisok toooffload írásvédett munkaterhelések.  Mivel az automatikus feladatátvételt csoportok tartalmaz, amely több adatbázist kell ezeket konfigurálni hello elsődleges kiszolgálón. Elsődleges és másodlagos kiszolgálók kell hello ugyanahhoz az előfizetéshez. Automatikus feladatátvételt csoportok hello csoport tooonly egy másodlagos kiszolgáló egy másik régióban lévő összes adatbázis replikációs támogatja. Aktív georeplikáció, automatikus feladatátvételt csoportok nélkül lehetővé teszi, hogy másolatot too4 másodlagos bármely régióban.

Aktív georeplikáció használata és az elsődleges adatbázis OK, vagy egyszerűen offline állapotba kell toobe, a másodlagos adatbázisok feladatátvételi tooany is kezdeményezhető. Aktivált tooone hello másodlagos adatbázisok esetén a feladatátvétel más másodlagos-e automatikusan csatolt toohello új elsődleges. Automatikus feladatátvételt használata groups (a – előzetes verzió) toomanage adatbázis helyreállítása és bármely kimaradás, amely hatással van egy vagy több hello adatbázisok automatikus feladatátvétel hello csoport eredményezi. Konfigurálhatja az alkalmazás igényeinek leginkább megfelelő hello automatikus feladatátvételt házirendet, vagy nem vesznek részt, és manuális-aktiválást használjon. Emellett automatikus feladatátvételt csoportok (az előzetes verzió) adja meg az olvasási és írási, és csak olvasható figyelő végpontokat a továbbra is változatlan feladatátvételek során. Akár manuális vagy automatikus feladatátvétel aktiválási, feladatátvevő összes másodlagos adatbázis a hello csoport tooprimary vált. Hello adatbázis feladatátvétel befejezését követően a hello DNS-rekord az automatikusan frissített tooredirect hello végpontjainak toohello új régió.

Kezelheti a replikációs és feladatátvételi egy önálló adatbázis vagy egy adatbázis-kiszolgálón vagy egy aktív georeplikációt használ a rugalmas készletben. Megteheti, hogy használatával hello [Azure-portálon](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md), vagy hello [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). A feladatátvétel után ellenőrizze a kiszolgáló és az adatbázis hello hitelesítési követelmények hello új elsődleges. További információkért lásd: [SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md). Ez vonatkozik, hogy mindkét tooactive georeplikáció és automatikus feladatátvételt csoportok (az előzetes verzió).

Aktív georeplikáció használja hello [mindig a](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) technológia az SQL Server tooasynchronously replikálja a véglegesített tranzakciókat hello elsődleges adatbázis tooa másodlagos adatbázison olvasás pillanatképe elkülönítéssel (RCSI). Automatikus feladatátvételt csoportok hello csoport szemantikáját fölött aktív georeplikáció, de ugyanazon az aszinkron replikációs eljárás hello biztosítják. Álljon, a hello másodlagos adatbázis némileg esetleg hello elsődleges adatbázis mögött, hello másodlagos adatok garantáltan toonever részleges tranzakciók rendelkezik. Határokon területi redundanciát lehetővé teszi, hogy az egész adatközpont vagy természeti katasztrófa, katasztrofális emberi hibákat vagy rosszindulatú tevékenységéért által okozott adatközpontban részei egy állandó adatvesztést alkalmazások tooquickly helyreállítása. hello RPO adatokat található [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).

hello alábbi ábrán is látható példáját mutatja be aktív georeplikáció konfigurált a hello északi középső Régiójában régióban elsődleges és másodlagos régióban hello déli középső Régiójában.

![a georeplikáció kapcsolat](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Mivel hello másodlagos adatbázisok olvasható, a használt toooffload írásvédett munkaterhelésekkel, például jelentéskészítéssel feladatok is lehetnek. Ha az aktív georeplikációt használ, a rendszer lehetséges toocreate hello másodlagos adatbázis hello ugyanabban a régióban, hello elsődleges, de nem növeli a hello alkalmazás rugalmasság toocatastrophic hibák. Ha az automatikus feladatátvételt csoportokat (az előzetes verzió) használ, a másodlagos adatbázisnak mindig létrejön egy másik régióban.

Ezenkívül a következő forgatókönyvek hello toodisaster helyreállítási aktív georeplikáció használhatók:

* **Adatbázis-áttelepítési**: használhatja az egyik kiszolgáló tooanother online adatbázis aktív georeplikációt toomigrate minimális állásidővel.
* **Alkalmazásfrissítések**: alkalmazás frissítése során sikertelen hátsó másolatként egy extra másodlagos is létrehozhat.

tooachieve valódi üzleti folytonossági, adatbázis-redundancia adatközpont között hozzáadása csak hello megoldás részét képezi. Helyreállítás egy alkalmazás (szolgáltatás)-végpontok után végzetes hiba hello szolgáltatást alkotó összetevők és a függő szolgáltatás sem szükséges. Ezen összetevők közé hello ügyfélszoftver (például egy egyéni JavaScript böngésző), az előtér-webkiszolgálók, a tároló és a DNS. Nagyon fontos, hogy az összes összetevő rugalmas toohello azonos hibákat észlel és belül hello helyreállítási idő célkitűzése (RTO) az alkalmazás elérhetővé válnak. Ezért kell tooidentify minden függő szolgáltatás és hello garanciák és képességeket biztosítanak. Ezt követően, amely során a szolgáltatás funkciók hello hello szolgáltatás, amelytől feladatátvételi tooensure megfelelő lépéseket kell végrehajtania. Vész-helyreállítási megoldások tervezésével kapcsolatos további információkért lásd: [felhőalapú megoldások kialakítása vész-helyreállítási használatával aktív georeplikáció](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktív georeplikáció képességek
hello aktív georeplikáció szolgáltatás a következő alapvető képességeit hello biztosít:
* **Automatikus aszinkron replikáció**: csak létrehozhat egy másodlagos adatbázis tooan meglévő adatbázis hozzáadásával. bármely Azure SQL adatbázis-kiszolgáló másodlagos hello hozhatók létre. Létrehozása után hello másodlagos adatbázis feltöltődik a másolt hello elsődleges adatbázisból hello adatokra. Ez a folyamat összehangolása néven ismert. Miután másodlagos adatbázis létrehozták, és a rendezés, toohello elsődleges adatbázis aszinkron módon van frissítések automatikusan replikált toohello másodlagos adatbázis. Aszinkron replikáció, az azt jelenti, hogy tranzakciók vannak véglegesítve lett az elsődleges adatbázis hello még nem replikált toohello másodlagos adatbázis. 
* **Másodlagos adatbázisok olvasható**: egy alkalmazás is elérhető egy másodlagos adatbázis csak olvasható műveletekbe hello azonos vagy különböző rendszerbiztonsági hello elsődleges adatbázis eléréséhez használt. hello másodlagos adatbázisok fog működni a pillanatkép-Elkülönítés mód tooensure hello frissítések az elsődleges hello (napló ismétlés) nem replikáció végrehajtva a következő másodlagos hello lekérdezések.

   > [!NOTE]
   > hello napló ismétlési késik hello másodlagos adatbázison, ha nincsenek a séma frissítése az elsődleges, másodlagos adatbázis hello séma zárolását igénylő hello fogadja. 
   > 

* **Több olvasható másodlagos adatbázis**: két vagy több másodlagos adatbázist növelni a redundanciát és hello elsődleges adatbázis és az alkalmazás adatvédelmi szintet. Ha több másodlagos adatbázist, hello alkalmazás akkor is védett még akkor is, ha egy másodlagos adatbázisok hello meghibásodik. Ha csak egy másodlagos adatbázist, és nem sikerül, a hello alkalmazás fel van fedve toohigher kockázatot jelent, amíg egy új másodlagos adatbázis nincs létrehozva.

   > [!NOTE]
   > Aktív georeplikáció toobuild globálisan elosztott alkalmazást használ, és kell tooprovide olvasási hozzáférési toodata a 4-nél több régióban, ha egy másodlagos (Ez a folyamat láncolás) másodlagos hozhat létre. Így az adatbázis-replikáció gyakorlatilag korlátlan méretezési érhető el. Ezenkívül láncolás csökkenti hello hello elsődleges adatbázis-replikáció. hello kompromisszum hello megnövekedett replikációs késés hello levél szélső másodlagos adatbázisok. . 
   >

* **Az adatbázisok rugalmas készlethez támogatási**: aktív georeplikáció a rugalmas készletben bármely adatbázis konfigurálhatók. hello másodlagos adatbázis lehet egy másik rugalmas készletben. Az rendszeres adatbázisok másodlagos hello rugalmas készletek legyen, és ez fordítva is igaz, ha hello szolgáltatásszintek hello azonos. 
* **Hello másodlagos adatbázis konfigurálható teljesítményszintjének**: egy másodlagos adatbázis hozhatók létre mint hello alacsonyabb teljesítményszintre elsődleges. Elsődleges és másodlagos adatbázisok a következők szükséges toohave hello ugyanazt a szolgáltatási rétegben. Ezt a beállítást nem ajánlott az alkalmazások magas adatbázis írási tevékenység, mert hello megnövekedett replikációs késés jelentős adatvesztés kockázatát hello növeli a feladatátvétel után. Ezenkívül Miután feladatátvételi hello alkalmazás teljesítményre csak új hello elsődleges frissített tooa nagyobb teljesítményt szint van. hello napló IO százalékos diagram Azure-portál biztosít egy jó módszer tooestimate hello minimális teljesítményszintjének hello másodlagos, amely szükséges toosustain hello replikációs terhelés. Például, ha az elsődleges adatbázis P6 (1000 DTU), és a naplót IO százalék 50 % hello másodlagos kell toobe legalább P4 (500 DTU). Kérheti is le hello napló IO adatok [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) vagy [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) adatbázis-nézetek.  Hello SQL-adatbázisa teljesítményszintjeit további információkért lásd: [SQL Database beállításai és teljesítménye](sql-database-service-tiers.md). 
* **Felhasználó által szabályozott feladatátvétel és a feladat-visszavétel**: egy másodlagos adatbázis explicit módon lehet kapcsolt toohello elsődleges szerepkör hello alkalmazás vagy hello felhasználó bármikor. Egy valódi kimaradás hello során "nem tervezett" beállítást kell használni, amely azonnal elősegíthető egy másodlagos toobe hello elsődleges. Amikor hello sikertelen elsődleges állítja helyre, és elérhető újra, hello rendszer automatikusan jelek hello elsődleges, másodlagos helyre, és érdekében, hogy az új elsődleges hello naprakész. Lejáró replikációs jellege aszinkron toohello kisebb mennyiségű adatot elvesznek, nem tervezett feladatátvételek során Ha egy elsődleges sikertelen hello legutóbbi módosítások toohello másodlagos replikálás előtt. Ha több másodlagos adatbázist az elsődleges átadja, hello rendszer automatikusan újrakonfigurálja hello replikációs kapcsolat, és hivatkozások hello újonnan fennmaradó másodlagos toohello elsődleges előléptetett felhasználói beavatkozás nélkül. Miután hello kimaradás hello feladatátvételi okozó elhárítására, kívánatos tooreturn hello alkalmazás toohello elsődleges régió lehet. hogy hello feladatátvételi parancsot kell hívható hello toodo "tervezett" lehetőséget. 
* **Hitelesítő adatok és a tűzfalszabályok szinkronban tartja**: javasoljuk [adatbázis-tűzfalszabályok](sql-database-firewall-configure.md) georeplikált adatbázisok, ezek a szabályok lehessen replikálni hello adatbázis tooensure minden másodlagos adatbázisok olyan hello elsődleges hello tűzfal szabályai. Ez a megközelítés hello szükségtelenné teszi az ügyfelek toomanually konfigurálása és karbantartása a tűzfalszabályok mind az elsődleges és másodlagos adatbázisok hello futtató kiszolgálókon. Ehhez hasonlóan használatával [tartalmazott adatbázis-felhasználók](sql-database-manage-logins.md) adatok hozzáférést biztosítja mind az elsődleges, mind a másodlagos adatbázisok mindig azonos felhasználó hitelesítő adatait, így a feladatátvétel során nincs szolgáltatások hello bejelentkezések a esedékes toomismatches és jelszavak. Hello hozzáadása a [Azure Active Directory](../active-directory/active-directory-whatis.md), ügyfelek kezelhető a felhasználói hozzáférés tooboth elsődleges és másodlagos adatbázisok és hello kiküszöbölése szükséges teljesen kezelése az adatbázis-felhasználó hitelesítő adatait.

## <a name="auto-failover-group-capabilities"></a>Automatikus feladatátvételt csoport képességek

Automatikus feladatátvételt csoportok szolgáltatása egy hatékony elvonása, aktív georeplikáció szintű replikációs csoport és az automatikus feladatátvételre. Emellett eltávolítja a hello szükségességét toochange hello SQL kapcsolati karakterláncot a feladatátvételt követően hello további figyelő végpontokat megadásával. 

* **Feladatátvételi csoport**: egy vagy több feladatátvételi csoportok hozhatók létre különböző régiókban (elsődleges és másodlagos kiszolgálók) két kiszolgáló között. Minden egyes csoport tartalmazhat egy vagy több adatbázis helyreállított egységként abban az esetben, ha minden egyes elsődleges adatbázisát tooan kívül az elsődleges régióban hello miatt elérhetetlenné válik.  
* **Elsődleges kiszolgáló**: üzemeltető kiszolgáló hello hello feladatátvételi csoport elsődleges adatbázisoknál.
* **Másodlagos kiszolgáló**: A másodlagos adatbázisok hello hello feladatátvételi csoport üzemeltető kiszolgálón. hello másodlagos kiszolgáló hello nem lehet ugyanabban a régióban hello elsődleges kiszolgálóként.
* **Hozzáadását adatbázisok toofailover csoport**: helyezze egy kiszolgálót több adatbázis, vagy belül egy rugalmas készlet hello be azonos feladatátvételi csoport. Ha egy önálló adatbázis toohello csoportot ad hozzá, automatikusan létrehoz egy másodlagos adatbázis használatával hello edition és a teljesítmény ugyanazon a szinten. Hello elsődleges adatbázis esetén a rugalmas készletekben található másodlagos hello automatikusan létrejön az hello rugalmas készlet hello az azonos név. Ha egy adatbázist, amely már rendelkezik egy másodlagos adatbázis hello másodlagos kiszolgáló, a georeplikáció hello csoport örökli.

   > [!NOTE]
   > Egy adatbázis, amely már rendelkezik egy másodlagos adatbázis-kiszolgálók nem hello feladatátvételi csoport részét képező felvételekor hello másodlagos kiszolgáló egy új másodlagos jön létre. 
   >

* **Feladatátvételi csoport olvasási és írási figyelő**: A DNS CNAME rekord jöttek létre,  **&lt;feladatátvételi csoportnév&gt;. database.windows.net** toohello aktuális elsődleges kiszolgáló URL-címe mutat. Lehetővé teszi hello írható-olvasható SQL alkalmazások tootransparently toohello elsődleges adatbázis csatlakozzon újra, ha hello elsődleges vált, a feladatátvételt követően. 
* **Feladatátvételi csoport olvasási figyelő**: A DNS CNAME rekord jöttek létre,  **&lt;feladatátvételi csoportnév&gt;. secondary.database.windows.net** toohello másodlagos kiszolgáló URL-mutat. Lehetővé teszi hello írásvédett tootransparently Csatlakozás SQL alkalmazások toohello másodlagos adatbázis hello segítségével megadott terheléselosztási szabályok. Opcionálisan megadhatja, ha azt szeretné, hello írásvédett forgalom toobe automatikusan-e irányítva toohello elsődleges kiszolgáló Ha hello másodlagos kiszolgáló nem érhető el.
* **Automatikus feladatátvétel házirend**: alapértelmezés szerint az Automatikus feladatátvétel házirendjével hello feladatátvételi csoport van konfigurálva. hello rendszer eseményindítók feladatátvételi, amint hello hiba lép fel. Ha azt szeretné, hogy toocontrol hello feladatátvételi munkafolyamat hello alkalmazásból, kikapcsolhatja automatikus feladatátvételre. 
* **Kézi feladatátvételre**: feladatátvételi manuálisan is kezdeményezhető, tetszőleges időpontban, függetlenül attól, hello automatikus feladatátvétel konfigurálása. Automatikus feladatátvétel házirend nem konfigurált kézi feladatátvételre akkor hello feladatátvételi csoport szükséges toorecover adatbázisoknál. A kényszerített vagy rövid feladatátvételt (a teljes adatszinkronizálás) is kezdeményezhető. Ez utóbbi hello használt toorelocate hello aktív kiszolgáló toohello elsődleges régió lehet. Amikor befejeződött a feladatátvétel hello DNS-rekordok automatikusan frissített tooensure kapcsolat toohello megfelelő kiszolgálót.
* **Türelmi időszak adatvesztéssel**: mert hello elsődleges és másodlagos adatbázis-kel vannak szinkronizálva aszinkron replikáció hello feladatátvételi adatvesztést eredményezhet. Az alkalmazás tolerancia toodata adatvesztés hello automatikus feladatátvétel házirend tooreflect szabhatja testre. Konfigurálásával **GracePeriodWithDataLossHours**, szabályozhatja, hogy mennyi ideig hello rendszer vár, amely valószínűleg tooresult adatvesztés hello feladatátvétel kezdeményezése előtt. 

   > [!NOTE]
   > Amikor rendszer azt észleli, hogy továbbra is online-e hello csoport hello adatbázisok (például hello kimaradás csak érintett hello szolgáltatás vezérlő vezérlősík), azonnal aktiválja hello feladatátvétel függetlenül hello érték teljes adatszinkronizálás (rövid feladatátvétel) által beállított **GracePeriodWithDataLossHours**. Ez a viselkedés garantálja, hogy nincs-e adatvesztés nélküli hello helyreállítás során. hello türelmi időszak lép életbe, csak akkor, ha a rövid feladatátvétel során nem lehetséges. Ha hello kimaradás elhárítására hello türelmi időszak lejárta előtt, hello feladatátvételi nincs aktiválva.
   >

* **Több feladatátvételi csoport**: beállíthatja a hello több feladatátvételi csoport ugyanarra a feladatátvételek kiszolgálók toocontrol hello méretezésének két. Minden csoport külön átvétele. A több-bérlős-alkalmazás rugalmas készletek használja, ha a funkció toomix elsődleges és másodlagos adatbázisok mindegyik készlet használhatja. Ily módon csökkentheti egy kimaradás tooonly hello hatását hello bérlők felét.

## <a name="best-practices-of-building-highly-available-service"></a>Ajánlott eljárások a magas rendelkezésre állású szolgáltatás létrehozása

egy magas rendelkezésre állású szolgáltatás által használt Azure SQL adatbázis hello ügyfelek toobuild kell kövesse az alábbi irányelveket:
- **Használjon feladatátvételi csoport**: egy vagy több feladatátvételi csoportok hozhatók létre különböző régiókban (elsődleges és másodlagos kiszolgálók) két kiszolgáló között. Minden egyes csoport tartalmazhat egy vagy több adatbázis helyreállított egységként abban az esetben, ha minden egyes elsődleges adatbázisát tooan kívül az elsődleges régióban hello miatt elérhetetlenné válik. hello feladatátvételi csoport hello ugyanaz, mint hello elsődleges célja szolgáltatás geo-másodlagos adatbázis hoz létre. Ha ad hozzá egy meglévő georeplikáció kapcsolat toohello feladatátvételi csoport győződjön meg arról, hogy hello földrajzi-másodlagos van konfigurálva, szolgáltatásiszint-célkitűzés szolgáltatás azonos hello hello elsődleges.
- **Olvasási és írási figyelő használja az OLTP-munkaterhelés**: OLTP műveletek használata végrehajtása során  **&lt;feladatátvételi csoportnév&gt;. database.windows.net** módon lesz hello URL-címe és hello kapcsolatok automatikusan irányított toohello elsődleges. Az URL-cím nem változik hello feladatátvételt követően.  
Megjegyzés: hello feladatátvételi magában foglalja a DNS-rekordja, ezért a hello ügyfélkapcsolatok során átirányított toohello új hello ügyfél után csak elsődleges DNS-gyorsítótár frissítése hello frissítése.
- **Használja a csak olvasható figyelő csak olvasható munkaterhelés**: Ha egy logikailag elkülönített írásvédett munkaterhelés, amely az adatok hibatűrő toocertain elavulási, hello másodlagos adatbázis hello alkalmazásban használhatja. Csak olvasható munkamenetek használható  **&lt;feladatátvételi csoportnév&gt;. secondary.database.windows.net** , hello URL-címe és hello kapcsolat lesz automatikusan irányított toohello másodlagos. Javasolt továbbá jelző olvassa el a cél a kapcsolat-karakterláncban **ApplicationIntent = ReadOnly**. 
- **Elő kell készíteni a teljesítmény romlását**: SQL feladatátvételi döntési nem függ össze hello rest hello alkalmazás vagy többi használt szolgáltatások. hello alkalmazás "keverhetők" néhány egy régió tartozik, és néhány más összetevőkkel. tooavoid hello teljesítménycsökkenést, győződjön meg arról hello redundáns alkalmazástelepítés hello vész-Helyreállítási régióban, és kövesse az ebben a cikkben hello irányelveket.  
Megjegyzés: hello alkalmazás hello vész-Helyreállítási régióban nem rendelkezik toouse egy másik kapcsolati karakterláncot.  
- **Készítse elő a adatvesztés**: kimaradás észlelhető, ha az SQL automatikusan elindítja a írható-olvasható feladatátvételi ha nulla adatok elvesztését toohello ajánlott az ismeretek. Ellenkező esetben megvárja, Ön által megadott hello időtartam **GracePeriodWithDataLossHours**. Ha a megadott **GracePeriodWithDataLossHours**, elő kell készíteni a adatvesztés. Általában során kimaradások, Azure rendelkezésre állási előtérbe. Ha Ön az adatveszteség, győződjön meg arról, hogy tooset **GracePeriodWithDataLossHours** tooa megfelelően nagy számát, például 24 óra. 


## <a name="upgrading-or-downgrading-a-primary-database"></a>Frissítés vagy alacsonyabb verziójúra változtatása az elsődleges adatbázis
Frissítse, vagy egy elsődleges adatbázis tooa különböző teljesítményszintet visszaminősítését (hello belül ugyanazt a szolgáltatási réteg) bármely másodlagos adatbázisok leválasztása nélkül. Frissítésekor, azt javasoljuk, hogy először hello másodlagos adatbázis frissítése, és frissítse a hello elsődleges. Amikor alacsonyabb verziójúra változtatása, hello sorrend megfordításához: hello elsődleges először megállapításában, és hello másodlagos majd használni. Frissítésekor vagy visszaminősítését hello adatbázis tooa különböző szolgáltatási rétegben Ez a javaslat van kényszerítve. 

> [!NOTE]
> Hello feladatátvételi csoport konfigurációjának részeként létrehozott másodlagos adatbázis esetén nem javasolt toodowngrade hello másodlagos adatbázishoz. Ez a tooensure az adatréteg van elegendő kapacitása tooprocess a normál munkaterhelését feladatátvételi aktiválása után. 
>

## <a name="preventing-hello-loss-of-critical-data"></a>Kritikus hello adatvesztés megakadályozása
Lejáró toohello nagy késleltetésű nagy kiterjedésű hálózatok a folyamatos másolás egy aszinkron replikációs mechanizmust alkalmaz. Aszinkron replikáció révén adatvesztést elkerülhetetlen, ha hiba történik. Egyes alkalmazások azonban adatvesztés nélküli lehet szükség. tooprotect a kritikus frissítések, az alkalmazásfejlesztő meghívhatja hello [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) rendszer eljárás után azonnal hello tranzakció véglegesítésekor. Hívása **sp_wait_for_database_copy_sync** szál hívja, amíg el nem hello utolsó véglegesített tranzakció blokkok hello továbbított toohello másodlagos adatbázishoz. Azonban ez nem vár továbbított hello tranzakciók toobe visszajátszani és véglegesítve lett a másodlagos hello. **az sp_wait_for_database_copy_sync** hatókörön belüli tooa adott folyamatos másolás hivatkozás. Bármely felhasználó hello kapcsolat jogok toohello elsődleges adatbázissal hívhatják ezt az eljárást.

> [!NOTE]
> **az sp_wait_for_database_copy_sync** adatvesztés megakadályozza a feladatátvételt követően, de nem garantálják a teljes szinkronizálás az olvasási hozzáférést. hello által okozott késleltetés egy **sp_wait_for_database_copy_sync** eljáráshívás jelentős lehet, és hello hívás hello időpontban hello tranzakciós napló mérete hello függ. 
> 

## <a name="programmatically-managing-failover-groups-and-active-geo-replication"></a>Programozott módon a feladatátvételi csoportok és aktív georeplikáció kezelése
Korábban bemutatott automatikus feladatátvételt csoportok (az előzetes verzió) és az aktív georeplikáció is kezelhetők programozott módon, Azure PowerShell és hello REST API-t. a következő táblák hello hello készlete használható parancsok ismertetik.

**Az Azure Resource Manager API és a szerepkör alapú biztonsági**: aktív georeplikáció magában foglalja a kezelése, beleértve a hello Azure Resource Manager API-készlet [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) és [Azure PowerShell-parancsmagok](https://docs.microsoft.com/powershell/azure/overview). Ezen API-k kötelező erőforráscsoportok hello használata és szerepköralapú biztonsági (RBAC) támogatja. Hogyan érik el tooimplement szerepkörök további információkért lásd: [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md).

> [!NOTE]
> Számos új szolgáltatást nyújt aktív georeplikáció esetén támogatottak használatával [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) alapú [Azure SQL REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx) és [Azure SQL Database PowerShell parancsmagjait](https://msdn.microsoft.com/library/azure/mt574084.aspx). Hello [REST API-t (klasszikus)](https://msdn.microsoft.com/library/azure/dn505719.aspx) és [Azure SQL Database (classic) parancsmagjai](https://msdn.microsoft.com/library/azure/dn546723.aspx) használata támogatott a visszamenőleges kompatibilitás érdekében használatával hello Azure Resource Manager-alapú API-k használata ajánlott. 
> 

## <a name="manage-sql-database-failover-using-using-transact-sql"></a>SQL adatbázis feladatátvétel használatával felügyelt Transact-SQL használatával

| Parancs | Leírás |
| --- | --- |
| [Az ALTER DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) |Meglévő adatbázis másodlagos ON SERVER hozzáadása argumentum toocreate egy másodlagos adatbázis használja, és elindítja a adatreplikáció |
| [Az ALTER DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) |Használjon FELADATÁTVÉTELI vagy FORCE_FAILOVER_ALLOW_DATA_LOSS tooswitch egy másodlagos adatbázis toobe elsődleges tooinitiate feladatátvétel |
| [Az ALTER DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) |Egy SQL-adatbázis és a hello megadott másodlagos adatbázis replikálása másodlagos ON SERVER eltávolítása tooterminate használja. |
| [sys.geo_replication_links (az Azure SQL Database)](https://msdn.microsoft.com/library/mt575501.aspx) |Az összes meglévő replikációs hivatkozások az egyes adatbázisok hello Azure SQL Database logikai kiszolgáló információt ad vissza. |
| [sys.dm_geo_replication_link_status (az Azure SQL Database)](https://msdn.microsoft.com/library/mt575504.aspx) |Lekérdezi hello utolsó replikációs időpontja, utolsó replikációs késés és egyéb hello replikációs hivatkozás a megadott SQL-adatbázis adatait. |
| [sys.dm_operation_status (az Azure SQL Database)](https://msdn.microsoft.com/library/dn270022.aspx) |Az összes adatbázis-műveleteket, köztük a hello replikációs hivatkozások állapotának hello hello állapotát jeleníti meg. |
| [az sp_wait_for_database_copy_sync (az Azure SQL Database)](https://msdn.microsoft.com/library/dn467644.aspx) |hello alkalmazás toowait azt eredményezi, amíg a végrehajtott tranzakciók replikációja és hello aktív másodlagos adatbázis nyugtázni. |
|  | |

## <a name="manage-sql-database-failover-using-using-powershell"></a>SQL adatbázis feladatátvétel használatával felügyelt PowerShell használatával

| Parancsmag | Leírás |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Egy vagy több adatbázis lekérdezi. |
| [Új AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary) |Létrehoz egy meglévő adatbázis másodlagos adatbázist, és elindítja a adatreplikáció. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary) |A másodlagos adatbázis toobe elsődleges tooinitiate feladatátvételt vált. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) |Egy SQL-adatbázis és a megadott másodlagos adatbázis hello közötti adatreplikáció leáll. |
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) |Lekérdezi a hello földrajzi-replikációs hivatkozások az Azure SQL Database és egy erőforráscsoport vagy SQL Server között. |
| [Új AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Ez a parancs létrehoz egy feladatátvételi csoport, és regisztrálja azt az elsődleges és másodlagos kiszolgálók|
| [Remove-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/remove-azurermsqldatabasefailovergroup) | Hello feladatátvételi csoport eltávolítása hello kiszolgáló és az összes másodlagos adatbázisok szereplő hello csoport törlése |
| [Get-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Lekéri a hello feladatátvételi csoport konfigurálása |
| [Set-AzureRmSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/set-azurermsqldatabasefailovergroup) |   Módosítja a hello konfigurációt hello feladatátvételi csoport |
| [Kapcsoló-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Hello feladatátvételi csoport toohello másodlagos kiszolgáló eseményindítók feladatainak átvétele |
|  | |

> [!IMPORTANT]
> Mintaparancsfájlok, lásd: [konfigurálása és a feladatátvétel egy önálló adatbázis aktív georeplikációt használ](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [konfigurálása és a feladatátvétel egy készletezett adatbázis aktív georeplikációt használ](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md), és a [konfigurálása és a feladatátvétel egy feladatátvételi csoport egyetlen-adatbázis (előzetes verzió)] (parancsfájlok/sql-database-setup-geodr-failover-database-failover-csoportok-powershell.md.
>

## <a name="manage-sql-database-failover-using-hello-rest-api"></a>SQL-adatbázis feladatátvételi hello REST API használatával kezelése
| API | Leírás |
| --- | --- |
| [Hozzon létre vagy az adatbázis frissítése (createMode = visszaállítása)](/rest/api/sql/Databases/CreateOrUpdate) |Hoz, frissítéseket, vagy egy elsődleges vagy másodlagos adatbázis visszaállítása. |
| [Get létrehozása, vagy az adatbázis állapotának frissítése](/rest/api/sql/Databases/CreateOrUpdate) |A létrehozási művelet során értéket ad vissza a hello állapota. |
| [Állítsa be a másodlagos adatbázist az elsődleges (tervezett feladatátvétel)](https://docs.microsoft.com/rest/api/sql/replicationlinkss#ReplicationLinks_Failover) |Melyik adatbázis-replikát az elsődleges adatbázisból hello jelenlegi elsődleges másodpéldányt feladatátvételét által beállítása. |
| [Állítsa be a másodlagos adatbázist az elsődleges (a nem tervezett feladatátvétel)](https://docs.microsoft.com/rest/api/sql/replicationlinks#ReplicationLinks_FailoverAllowDataLoss) |Melyik adatbázis-replikát az elsődleges adatbázisból hello jelenlegi elsődleges másodpéldányt feladatátvételét által beállítása. Ez a művelet adatvesztést eredményezhet. |
| [Replikációs hivatkozás beszerzése](/rest/api/sql/replicationlinks/get) |Lekéri egy adott replikációs hivatkozás egy georeplikáció partneri kapcsolat áll fenn az adott SQL-adatbázis. Lekérdezi a hello információ látható hello sys.geo_replication_links katalógusnézet használatával derítheti ki. |
| [Replikációs hivatkozások - adatbázis által listája](/rest/api/sql/replicationlinks/listbydatabase) | Lekérdezi az összes replikációs hivatkozás a georeplikáció együttműködve adott SQL-adatbázis. Lekérdezi a hello információ látható hello sys.geo_replication_links katalógusnézet használatával derítheti ki. |
| [Replikációs hivatkozás törlése](/rest/api/sql/databases/delete) | Egy adatbázis-replikációs hivatkozást törli. Nem hajtható végre, a feladatátvétel során. |
| [Hozzon létre vagy feladatátvételi csoport] / rest/api/sql/failovergroups/createorupdate) | Létrehozza vagy frissíti egy feladatátvételi csoport |
| [Feladatátvételi csoport törlése](/rest/api/sql/failovergroups/delete) | Hello feladatátvételi csoport eltávolítása hello kiszolgáló |
| [A feladatátvétel (tervezett)](/rest/api/sql/failovergroups/failover) | Feladatátadás hello aktuális elsődleges kiszolgáló toothis kiszolgálóhoz. |
| [Kényszerített feladatátvételi adatvesztés engedélyezése](/rest/api/sql/failovergroups/forcefailoverallowdataloss) |a jelenlegi elsődleges kiszolgáló toothis kiszolgálóról hello ails keresztül. Ez a művelet adatvesztést eredményezhet. |
| [Get feladatátvételi csoport](/rest/api/sql/failovergroups/get) | Egy feladatátvételi csoport lekérése. |
| [Listázza a csoportokat feladatátvételi kiszolgáló](/rest/api/sql/failovergroups/listbyserver) | A Server hello feladatátvételi csoportok listája. |
| [Frissítés feladatátvételi csoport](/rest/api/sql/failovergroups/update) | Egy feladatátvételi csoport frissíti. |
|  | |

## <a name="next-steps"></a>Következő lépések
* Mintaparancsfájlok lásd:
   - [Konfigurálja és feladatátvételi egy önálló adatbázis aktív georeplikációt használ](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
   - [Konfigurálja és feladatátvételi egy készletezett adatbázis aktív georeplikációt használ](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
   - [Konfigurálja és a feladatátvételi feladatátvételi csoport számára egy adatbázist (előzetes verzió)](scripts/sql-database-setup-geodr-failover-database-failover-group-powershell.md)
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md)
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis biztonsági mentések automatikus](sql-database-automated-backups.md).
* toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból hello szolgáltatás által kezdeményezett](sql-database-recovery-using-backups.md).
* egy új elsődleges kiszolgáló és -adatbázis hitelesítési követelményeivel kapcsolatos toolearn lásd [SQL-adatbázis biztonsági katasztrófa utáni helyreállítás után](sql-database-geo-replication-security-config.md).

