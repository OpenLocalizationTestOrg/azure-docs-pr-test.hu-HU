---
title: aaaIntro tooMicrosoft Azure |} Microsoft Docs
description: "Új Azure tooMicrosoft? Egy alapszintű hello áttekintése Get általa nyújtott kapcsolódó hogyan hasznosak."
services: " "
documentationcenter: .net
author: rboucher
manager: carolz
editor: 
ms.assetid: 6f47f711-2208-4c21-8c1d-826a54c05c29
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2015
ms.author: robb
ms.openlocfilehash: 6791506abe813e19ca7b04d78d35cb46352a9028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introducing-microsoft-azure"></a>Introducing Microsoft Azure
A Microsoft Azure a Microsoft alkalmazás platformja hello nyilvános felhő.  hello Ez a cikk célja az Azure-ban hello alapjai ismertetése, még akkor is, ha nem tudja semmit kapcsolatos alaprendszert, a felhőalapú informatika toogive.

**Hogyan tooread Ez a cikk**

Azure mindig hello nő, hogy könnyen tooget túlterhelt.  Első lépésként hello alapvető szolgáltatásokat, amelyek először a cikkben találhatók, és folytassa a tooadditional szolgáltatásoknak. Ez nem jelenti azt, önmagában nem használhatja csak hello további szolgáltatásokat, de hello alapvető szolgáltatások jött létre az Azure-ban futó alkalmazások hello core.

**Visszajelzés küldése**

Visszajelzése fontos. Ez a cikk Azure hatékony áttekintést adjon meg. Ha nem, adja meg, alján hello hello hello megjegyzések szakaszában. Néhány részletes osszon ki a mi toosee, és hogyan tooimprove hello cikk várt.  

## <a name="hello-components-of-azure"></a>hello összetevők az Azure
Azure services csoportok hello felügyeleti portál, valamint különböző visual eszközökkel, például a hello kategóriákba [Mi az Azure Infographic](https://azure.microsoft.com/documentation/infographics/azure/) . Kezelési portál hello valóban használt funkciókért toomanage leginkább (de nem minden) szolgáltatások az Azure-ban.

Ez a cikk fogja használni a **másik vállalatnál** tootalk szolgáltatásokkal kapcsolatos hasonló függvény, és a fontos alárendelt szolgáltatásokat nyújtó nagyobb ők részét képező toocall alapján.  

![Az Azure-összetevők](./media/fundamentals-introduction-to-azure/AzureComponentsIntroNew780.png)   
 *Ábra: Az Azure internetről elérhető, az Azure adatközpontjaiban futó alkalmazásszolgáltatások biztosít.*

## <a name="management-portal"></a>Felügyeleti portál
Azure rendelkezik egy webes felület hello nevű [kezelési portál](http://manage.windowsazure.com) , amely lehetővé teszi a rendszergazdák tooaccess és a legtöbb, de nem minden Azure-szolgáltatások.  A Microsoft hello újabb UI portal a béta általában kiadását régebbi egy kivonása előtt. hello újabb egy nevezik hello ["Azure betekintő portál"](https://portal.azure.com/).

Általában egy hosszú átfedés van amikor mindkét portálok aktívak. Alapvető szolgáltatások mindkét portálok jelenik meg, amíg nem minden funkció is érhetők el. Újabb szolgáltatások jelenik meg a hello újabb portál első és a régebbi szolgáltatások és funkciók csak a régebbi egy hello előfordulhat, hogy létezik.  Itt üdvözlőüzenetére, ha nem találja a valami hello régebbi portálon, ellenőrizze hello újabb egy és fordítva.

## <a name="compute"></a>Számítás
A felhőplatform does hello legalapvetőbb dolog alkalmazások végrehajtására. Hello Azure számítási modellek mindegyiknek saját szerepkör tooplay.

Ezek a technológiák külön használhatja, vagy az alkalmazás megfelelő foundation szükség szerint toocreate hello egyesíthetők. hello megközelítés határozza meg, hogy milyen problémákat a kívánt toosolve.

### <a name="azure-virtual-machines"></a>Azure-alapú virtuális gépek
![Az Azure virtuális gépek ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_VirtualMachinesIntroNew_12345.png)   
*Ábra: Az Azure virtuális gépek lehetővé teszi az hello felhő virtuálisgép-példányok teljes hozzáféréssel.*

hello képességét toocreate egy virtuális gép az igény szerinti szabványos lemezkép akár egy ad meg, hogy nagyon hasznos lehet. Ezt a módszert gyakran nevezik infrastruktúrák (IaaS), az Azure virtuális gépek biztosít. 2. ábrán látható kombinációja, hogyan fut a virtuális gép (VM), és hogyan toocreate olyan virtuális merevlemezről.  

toocreate egy virtuális Gépet, akkor adja meg a melyik virtuális merevlemez toouse és hello a virtuális gép méretét.  Majd kell fizetnie hello ideje, hogy virtuális gép fut hello. Abban az esetben, ha nincs elérhető VHD hello tartása minimális tárolási díjat hello percenként, és csak akkor futása közben, a fizetnie. Az Azure gyűjteményt tartalmazó virtuális merevlemezeket (más néven "képek") egy rendszerindításra alkalmas operációs rendszer toostart a készlet kínál. Ezek közé tartozik a Microsoft és a partner beállítások, például a Windows Server és Linux, SQL Server, Oracle és sok más. Szabad toocreate VHD-k és lemezképeket, és majd feltölthetők magát. Töltse fel a VHD-k, melyek csak az adatokat és lehessen elérni a futó virtuális gépek is.

Bárhol hello VHD származik, tartósan tárolhatja a virtuális gép futása közben végrehajtott módosításokat. hello virtuális gép létrehozása az adott virtuális merevlemezből, amikor legközelebb dolgot átvételéhez ahol abbahagyta. Azure Storage blobs, amelyek később döntésről bővebben hello vissza hello a virtuális gépek virtuális merevlemezeket tárolja.  Ez azt jelenti, hogy a virtuális gépek toohardware és lemez hibák miatt nem eltűnnek redundancia tooensure kap. Az is lehetséges toocopy hello Azure, majd futtassa helyileg kívüli virtuális merevlemez módosítása.

Az alkalmazás egy vagy több virtuális gépekről, attól függően, hogy milyen módon létrehozta előtt, és döntse el, toocreate az alapoktól most már fut.

Ez számos különböző probléma lehet használt tooaddress számítástechnikai általános megközelítés toocloud.

**Virtuálisgép-forgatókönyvek esetén**

1. **Fejlesztési és tesztelési célú** -használhatjuk ezeket toocreate egy alacsony költségű fejlesztési és tesztelési platform, amely leállíthat befejezése után használja azt. Előfordulhat, hogy hozzon létre, és bármilyen nyelvet és a könyvtárak tetszés használó alkalmazások futtatásához. Ezek az alkalmazások Azure által biztosított hello-kezelési lehetőségek bármelyikét használhatja, és SQL Server vagy egy másik adatbázis-kezelő rendszer egy vagy több virtuális gépeken futó toouse is beállíthatja.
2. **Helyezze át az alkalmazások tooAzure (növekedési-és-shift)** -"Növekedési-és-shift" hivatkozik toomoving az alkalmazás sokkal például egy áttekintésében toomove egy nagy objektumot használja.  "Növekedési", a helyi adatközpontban, a virtuális merevlemez hello és "tolás" azt tooAzure és nem futtatható.  Általában akkor toodo néhány munkahelyi tooremove függőségek egyéb rendszereken. Ha túl sok, Ehelyett választhatja az 3. lehetőség.  
3. **Az Adatközpont kiterjesztése** -használata Azure futó virtuális gépeket a helyszíni adatközpont kiterjesztése SharePoint vagy más alkalmazásokat. toosupport, az Active Directory az Azure virtuális gépeken futó Windows-tartományokban lehetséges toocreate hello felhőben. Használhatja az Azure Virtual Network (később említett) tootie a helyi hálózat és a hálózat az Azure-ban együtt.

### <a name="web-apps"></a>Web Apps
![Az Azure Web Apps ROBBCSIART_TEST](./media/fundamentals-introduction-to-azure/mscsiart_AzureWebsitesIntroNew_12345.png)   
 *. Ábra: Az Azure Web Apps fut egy webhely alkalmazást hello felhőben anélkül, hogy toomanage hello webes kiszolgálóra.*

Hello leggyakoribb dolog, amelyek személyek hello felhőben fut webhelyekhez és webes alkalmazásokhoz. Ez lehetővé teszi az Azure virtuális gépek, de továbbra is hagyja meg hello ezzel egy vagy több virtuális gépek és az operációs rendszer alapjául szolgáló hello felügyeletének. Cloud services webes szerepkörök ehhez, de telepíteni és fenntartani, azokat továbbra is szükséges adminisztratív munka.  Mi történik, ha csak kívánt webhely Ha valaki másnak gondoskodik hello adminisztratív munka meg?

Ez az pontosan, Web Apps biztosít. A számítási modellt kínál egy felügyelt webes környezetben hello Azure Management portal, valamint az API-k használatával. Egy meglévő webhely alkalmazást áthelyezi változatlan Web Apps, vagy létrehozhat egy új közvetlenül hello felhőben. Után egy webhelye fut-e, adja hozzá, vagy példányt eltávolítja, dinamikusan, mindegyik függő Azure Web Apps tooload érkező kérések elosztása. Azure alkalmazások egy megosztott lehetőséget, ha a webhely más helyekre rendelkező virtuális gép fut, és egy normál beállítás, amely lehetővé teszi, hogy egy webhely toorun a saját virtuális gép is kínál. hello normál beállítás lehetővé teszi növelje hello méretét (számítástechnikai power) a példányok, ha szükséges.

Fejlesztési Web Apps relációs tároló .NET, PHP, Node.js, Java és Python, valamint az SQL-adatbázis és MySQL (a ClearDB, egy Microsoft partnert) támogatja. Azt is beépített támogatást nyújt több népszerű alkalmazások, beleértve a WordPress, a Joomla vagy a Drupal. hello célja tooprovide egy alacsony költségű, méretezhető, és körben használható platform hello nyilvános felhőben webhelyek és webalkalmazások készítéséhez.

**Web Apps forgatókönyvek**

Webes alkalmazások hasznosak lehetnek a vállalatok, a fejlesztők és a webes tervezési szerveinek tervezett toobe. A vállalatok egy könnyen kezelhető, méretezhető, rendkívül biztonságos és magas rendelkezésre állású megoldás futtatásához jelenléte webhelyek esetén. Tooset be egy webhelyet van szüksége, amikor az Azure Web Apps legjobb toostart és tooCloud szolgáltatások folytassa, ha egy szolgáltatás, amely nem érhető el kell. Lásd: hello végét hello "Számítási" szakasz további hivatkozások, amelyek segítségével toochoose hello-beállítások között.

### <a name="cloud-services"></a>Cloud Services
![Azure-Felhőszolgáltatás](./media/fundamentals-introduction-to-azure/CloudServicesIntroNew.png)   
*. Ábra: Az Azure Cloud Services biztosít egy hely toorun magas szinten méretezhető egyéni kódot platformon, egy Platformszolgáltatási környezetben*

Tegyük fel, hogy toobuild egy felhő-alkalmazás, amely képes támogatni az egyidejű felhasználók sok nem igényel sok felügyeleti és soha nem működik. Előfordulhat, hogy egy meglévő szoftverszállítótól, például meg vállalat úgy döntött, tooembrace épület egy verziója, a saját alkalmazásai hello felhőben szolgáltatott (SaaS) szoftver. Vagy előfordulhat, hogy a kezdeti egy fogyasztó alkalmazás várt mérete akkor nő, gyors létrehozása. Ha Azure van építve, mely végrehajtási modell használja?

Az Azure Web Apps lehetővé teszi, hogy az ilyen webalkalmazás létrehozása, de néhány megkötések. Nem rendelkezik rendszergazdai hozzáféréssel, például, ami azt jelenti, hogy tetszőleges szoftver nem telepíthető. Az Azure virtuális gépek lehetővé teszi az rugalmasan, beleértve a rendszergazdai hozzáféréssel, és biztosan használható jól méretezhető alkalmazás toobuild, de összekapcsolta toohandle sok szempontból megbízhatóság és a felügyeleti szolgáltatást. Mit szeretne olyan beállítás, amely lehetővé teszi az hello vezérlő szükséges, de a megbízhatóság és a felügyeleti szükséges hello munka nagyobb része is kezeli.

Ennek az az pontosan mit Azure Cloud Services biztosítja. Ez a technológia toosupport méretezhető, megbízható és alacsony – a rendszergazdai alkalmazások kifejezetten készült, és milyen gyakran hívják Platform (PaaS) szolgáltatás egy példát. toouse, létrehoz egy alkalmazást úgy dönt, például a C#, Java, PHP, Python, Node.js, vagy valami más hello technológiával. A kód majd végrehajtja a virtuális gépeken (hivatkozott tooas példányokat), Windows Server-verziót futtat.

De virtuális gépeken elkülönülnek a hello Azure virtuális gépek létrehozása néhányat a meglévők közül. Az egyetlen művelet, Azure maga kezeli azokat, többek között a javítások operációs rendszer telepítése során, és automatikusan új terítésével lett képek. Ez azt jelenti, hogy az alkalmazás nem szabad állapotban webes vagy feldolgozói szerepkör példányát; kell helyette megmarad hello Azure felügyeleti lehetőségeket hello következő szakaszban leírt egyikében. Azure virtuális gépeken, újraindítása lehetséges, hogy sikertelen is méri. Beállíthatja a felhőalapú szolgáltatások tooautomatically válasz toodemand hozzon létre több vagy kevesebb példánnyal. Ez lehetővé teszi nagyobb toohandle használati, majd a skála vissza, hogy annyi nem fizető kevesebb használat esetén.

Amikor létrehoz egy példányát a két szerepkörök toochoose van, mindkettő a Windows Server alapján. hello hello két fő különbség az, hogy egy webes szerepkör példánya fut-e, az IIS, míg a feldolgozói szerepkör példánya nem. Mindkettő kezeli a hello azonos módon, azonban, és a gyakori, hogy egy alkalmazás toouse mindkét. Például a webes szerepkör példánya lehet, hogy fogadja el a felhasználók által érkező kérések, majd adja meg azokat tooa feldolgozói szerepkör példánya feldolgozásra. tooscale felfelé vagy lefelé az alkalmazás kérheti, hogy Azure létrehozása vagy a szerepkör több példányát, vagy állítsa le a meglévő példányt. És hasonló tooAzure virtuális gépek, akkor van szó, csak a hello idő, hogy fut-e minden webes vagy feldolgozói szerepkör példánya.

**Cloud Services – forgatókönyvek**

Felhőszolgáltatások esetén ideális toosupport jelentős mértékű ki több hello platform felett ellenőrzést kell, mint az Azure Web Apps által biztosított, de nem kell az ellenőrzése alatt tartja a hello alapul szolgáló operációs rendszert.

#### <a name="choosing-a-compute-model"></a>A számítási modellt kiválasztása
hello lap [Azure Web Apps, a Cloud Services és a virtuális gépek összevetése](app-service-web/choose-web-site-cloud-service-vm.md) vonatkozó részletes toochoose egy számítási modellt.

## <a name="data-management"></a>Adatkezelés
Alkalmazások adatokra van szükségük, és a különböző típusú alkalmazások kell különböző típusú adatok. Ebből kifolyólag Azure biztosít számos különböző módon toostore adatokhoz és kezelheti. Azure számos tárolási lehetőség biztosítja, de minden nagyon tartós tárolására készültek.  A ezen lehetőségek egyikének esetén mindig szinkronban között egy Azure-adatközpontban – 6 engedélyezése az Azure toouse-georedundancia tooback tooanother datacenter legalább 300 miles azonnal fel az adatok 3 másolatát.     

### <a name="in-virtual-machines"></a>A virtuális gépek
hello képességét toorun SQL Server vagy egy másik adatbázis-kezelő egy virtuális gép létrehozása az Azure virtuális gépekkel rendelkezik már említett lett. Vegye figyelembe, hogy ez a beállítás nem korlátozott toorelational rendszerek; Ön is szabad toorun NoSQL technológiák, például a MongoDB és Cassandra. A saját adatbázis rendszert futtató egyszerű informatikai eljut jelenleg éppen használt tooin saját adatközpontjainkba-is van szükség, az adott adatbázis-kezelő hello felügyeleti kezelése.  Egyéb beállítások Azure kezeli, több vagy az összes hello felügyeleti meg.

Ebben az esetben hello hello virtuális gép állapotát és minden további adatlemezt létrehozása és feltöltése üzemelnek a blob storage (amely a döntésről bővebben később).  

### <a name="azure-sql-database"></a>Azure SQL Database
![Az Azure Storage SQL-adatbázis](./media/fundamentals-introduction-to-azure/StorageAzureSQLDatabaseIntroNew.png)   

*Ábra: Azure SQL Database biztosít hello felhőben felügyelt relációs adatbázis-szolgáltatás.*

Azure SQL Database. hello szolgáltatást biztosít a relációs tároló. Ne legyen hello elnevezési adatlopási. Ez eltér a Windows Server felett futó SQL Server által biztosított tipikus SQL-adatbázis.  

Korábbi nevén SQL Azure, az Azure SQL Database biztosít minden hello kulcsfontosságú szolgáltatásokat egy relációs adatbázis-kezelő rendszer, atomi tranzakciók egyidejű adatelérési adatok integritását, ANSI SQL-lekérdezések és a megszokott programozási egyszerre több felhasználó modell. Például az SQL Server, SQL-adatbázis segítségével férhetők el Entity Framework, ADO.NET, JDBC és más jól ismert adat-hozzáférési technológiák. A legtöbb hello T-SQL nyelv, SQL Server-eszközök, például az SQL Server Management Studio együtt is támogatja. Az, hogy ismeri a SQL Server (vagy egy másik relációs adatbázis) SQL-adatbázis használata egyszerű.

SQL-adatbázis nem csak egy adatbázis-kezelő, de a hello felhő-it a PaaS szolgáltatás. Az adatokat, és azt hozzáférő felhasználók továbbra is szabályozhatja, de hello felügyeleti grunt munkaelem, például kezelheti a hello hardver infrastruktúra, és automatikusan megőrzi a hello adatbázis és az operációs rendszer szoftver toodate mentése SQL-adatbázis gondoskodik. SQL-adatbázis is magas rendelkezésre állást biztosít, az automatikus biztonsági mentésekhez, időpontban képességek visszaállítása, és másolja replikálhatja a földrajzi régiók között.  

**SQL-adatbázis forgatókönyvei**

Ha az Azure-alkalmazások (bármely hello számítási modell használatával), amelyet a relációs tároló hoz létre, SQL-adatbázis jó választás lehet. Külső hello felhőben futó alkalmazások is használhatja a szolgáltatást, azonban, a többi egyéb forgatókönyvek. Például különböző ügyfélrendszer, beleértve az asztali számítógépek, laptopok, táblagépek és telefonok SQL-adatbázisban tárolt adatok elérhető. És mivel replikáció útján magas rendelkezésre állást biztosít, SQL-adatbázis használata segíthet minimalizálják az állásidőt.

### <a name="tables"></a>Táblák
![Az Azure Storage-táblákat](./media/fundamentals-introduction-to-azure/StorageTablesIntroNew.png)  

*. Ábra: Az Azure-táblákban egy egyszerű NoSQL módon toostore-adatokat biztosít.*

Ez a szolgáltatás részeként az "Azure Storage" nevezett nagyobb szolgáltatással is nevezik különböző feltételeket. Ha "tábla", "Azure-táblákban" vagy "storage tables" jelenik meg, a rendszer minden hello ugyanaz.  

Nem keverendő hello neve és: Ez a technológia nem relációs tárhely biztosításához. Az valójában egy példa egy nosql-alapú módszer kulcs/érték tárolónak hívják. Azure-táblákban lehetővé teszik, hogy egy alkalmazás, például karakterláncok egész számok vagy dátumok különböző típusú tulajdonságok tárolásához. Egy alkalmazás majd lekérheti a csoport tulajdonságai egyedi kulcs biztosítva, hogy a csoport. Összetett műveletek során illesztések nem támogatottak, például táblák kínálnak a gyors hozzáférés tootyped adatokat. Fontosságúak is jól méretezhető, egyetlen tábla képes toohold, amennyire csak egy terabájt adatok. És az egyszerűség kedvéért egyeztetéséhez, táblák rendszerint olcsóbb toouse, mint az SQL-adatbázis relációs tároló.

**Táblák forgatókönyvei**

Tegyük fel, hogy azt szeretné, hogy toocreate az Azure-alkalmazások, amelyet a gyors lehet, hogy azt a nagyszámú tootyped adatok eléréséhez, de nem szükséges összetett SQL lekérdezések tooperform ezeken az adatokon. Tegyük fel, hogy hoz létre, egy fogyasztó alkalmazás, amely az egyes felhasználók toostore felhasználói profil információra van szüksége. Az alkalmazás érintetlen toobe népszerű, így nagy mennyiségű adatot tooallow szükséges, de akkor nem nem sok és az adatok tárolásához, majd beolvasása egyszerű módon. Ez a forgatókönyv, ahol az így Azure táblák pontosan hello típusú.

### <a name="blobs"></a>Blobok
![Az Azure Storage blobs szolgáltatásban](./media/fundamentals-introduction-to-azure/StorageBlobsIntroNew.png)    
*Ábra: Az Azure BLOB strukturálatlan bináris adatokat biztosít.*  

Azure-blobokat (újra "Blob tároló" és "Storage Blobsba" csak is hello ugyanaz) kialakított toostore van strukturálatlan bináris adatok. Például a táblákat Blobok alacsony költségű tárolására szolgál, és lehet, hogy egyetlen blob akkora, mint 1 TB-os (egy terabájtnál). Azure-alkalmazások Azure meghajtót, adja meg az állandó tároló-e csatlakoztatva egy Azure-példányt Windows fájlrendszer blobok módosítását is lehet használni. hello alkalmazás látja a szokásos Windows-fájlokat, de hello tartalma ténylegesen tárolódnak a blob.

A BLOB storage használják sok más Azure-szolgáltatások (beleértve a virtuális gépek), így az alapértékekkel kezelni tud a munkaterhelések túl.

**Blobok forgatókönyvei**

Videók, nagy fájlok, vagy más bináris tároló alkalmazás egyszerű, olcsó tárolási BLOB is használhat. Blobok általában is használhatók, például a Content Delivery Network, amely előadás később más szolgáltatásokkal együtt.  

### <a name="import--export"></a>Import / Export (Importálás és exportálás)
![Azure importálási Export szolgáltatásról](./media/fundamentals-introduction-to-azure/ImportExportIntroNew.png)  

*Ábra: Az Azure Import / Export biztosít hello képességét tooship egy fizikai merevlemez-meghajtóról tooor gyorsabb és olcsóbb tömeges adatok importálása vagy exportálása az Azure-ból.*  

Egyes esetekben a kívánt toomove nagy mennyiségű adat Azure. Amely ehhez hosszú ideig akár napokig eltarthat, és a nagy mennyiségű sávszélesség használata. Ebben az esetben használhatja az Azure Import/Export, amely lehetővé teszi, hogy Ön tooship Bitlocker-titkosítású 3.5-ös "SATA merevlemez-meghajtók közvetlenül tooAzure adatközpontok, ahol a Microsoft fog hello adatok átviteléhez a blob-tárolóba.  Hello feltöltés befejezését követően a Microsoft hello meghajtók hátsó tooyou érhető el.  Nagy mennyiségű adat blobtárolóból exportált rögzített meghajtókra és hátsó tooyou mail keresztül küldött is kérheti.

**Forgatókönyv-importálás / exportálás**

* **Nagy adatáttelepítés** - bármikor nagy mennyiségű adat (TB) rendelkezik, hogy azt szeretné, hogy tooupload tooAzure, illetve hello Import/Export szolgáltatás gyakran sokkal gyorsabb és lehet, hogy olcsóbbak hello átvitele az internet. Ha blobok hello adatokat, például a Table storage vagy egy SQL-adatbázis más űrlapba tud feldolgozni.
* **Archivált adat-helyreállítás** -importálási/exportálási toohave Microsoft átviteli nagy mennyiségű adat tárolása az Azure Blob Storage-i hátsó tooa helyet felügyelni tooa tárolóeszköz küldeni, és akkor kell az eszköz használható. Ez eltarthat egy ideig, mert nincs vész-helyreállítási jó választás. Érdemes az archivált adatok gyors eléréséhez nem szükséges.

### <a name="file-service"></a>File szolgáltatás
![Az Azure File Service](./media/fundamentals-introduction-to-azure/FileServiceIntroNew.png)    
*Ábra: Az Azure a Fájlszolgáltatások olyan SMB \\ \\server\share utat hello felhőben futó alkalmazások számára.*

A helyszíni, közös toohave nagy mennyiségű hello Server Message Block (SMB) protokoll használatával keresztül érhető el a file storage egy \\ \\Server\share formátumban. Azure most már rendelkezik egy szolgáltatás, amely lehetővé teszi a toouse protokoll hello felhőben. Az Azure-ban futó alkalmazások használható tooshare fájlok jól ismert fájlrendszer API-k például ReadFile és WriteFile használatával virtuális gépek között. Ezenkívül hello fájlok is elérhető: hello segítségével egy REST-felület, amely lehetővé teszi a helyszíni tooaccess hello megosztások egy virtuális hálózatot is beállításakor ugyanannyi időt vesz igénybe. Az Azure Files hello blob szolgáltatás épül, ezért örökli hello azonos rendelkezésre állási, a tartósságra, a méretezhetőség és a georedundancia épített Azure Storage.

**Az Azure Files forgatókönyvei**

* **Meglévő alkalmazások toohello áttelepítése felhő** -a könnyebb toomigrate a helyszíni alkalmazások toohello felhő fájlt használó tooshare adatok hello alkalmazás részei között megosztja. Minden virtuális gép csatlakozik toohello fájlmegosztás és majd az olvasási és írási fájljait, ahogy azt elleni egy helyi fájl megosztásához.
* **Megosztott Alkalmazásbeállítások** -elosztott alkalmazás általános felépítését toohave konfigurációs fájlok egy központi helyen, ha azok elérhetők a sok különböző virtuális gépekről. Ezeket a konfigurációs fájlokat egy Azure-fájlmegosztáson tárolódnak, és minden alkalmazáspéldányok olvasni. hello-beállítások segítségével hello REST-felület, amely lehetővé teszi, hogy a globális hozzáférési toohello konfigurációs fájlok is kezelhetők.
* **Diagnosztikai megosztás** - mentheti, és diagnosztikai fájlmegosztásra például naplók, metrikák és összeomlási memóriaképek. Ezek a fájlok hello SMB és a REST-felületen keresztül elérhető segítségével alkalmazások toouse számos különféle feldolgozási és hello diagnosztikai adatok elemzése elemzésére szolgáló eszközöket.
* **Fejlesztési/tesztelési/Debug** – amikor a fejlesztők és rendszergazdák dolgoznak hello felhő, virtuális gépek gyakran kell eszközök vagy segédprogramok készlete. Telepítése és terjesztése ezeket a segédprogramokat mindegyik virtuális gépre sok időt vesz igénybe. Az Azure Files a fejlesztők és a rendszergazda tárolhatnak a kedvenc eszközök fájlmegosztáson, és toothem csatlakozzon a virtuális gépről.

## <a name="networking"></a>Hálózat
Azure ma hello world elosztva sok adatközpontokban futtatja. Az alkalmazás futtatásakor, vagy az adatok tárolásához, kiválaszthatja a ezek adatközpontok toouse közül legalább egyet. Az alábbi hello services használatával számos módon toothese számára fenntartott adatközpontokban is kapcsolódhatnak.

### <a name="virtual-network"></a>Virtual Network
![VirtualNetwork](./media/fundamentals-introduction-to-azure/VirtualNetworkIntroNew.png)   

*. Ábra: Virtuális hálózatok biztosít egy magánhálózaton hello felhőben különböző szolgáltatások tooeach működik, más vagy tooon helyszíni erőforrások Ha úgy konfigurálja a VPN-létesítmények közötti kapcsolat.*  

Egy hasznos módja toouse nyilvános felhő tootreat rá a saját adatközpont kiterjesztése.

Virtuális gépek igény szerint hozhat létre, mert távolítsa el őket (és fizető leállítása) azok már nincs szükség, ha csak akkor, ha azt szeretné, a számítástechnikai power lehet. Azure virtuális gépek lehetővé teszi, hogy a SharePoint, az Active Directory és más jól ismert helyszíni szoftver futó virtuális gépeket létrehozni, mert ez a módszer segítségével és hello alkalmazások már rendelkeznek.

toomake ez valóban akkor hasznos, azonban a felhasználók kell kerülnie toobe képes tootreat ezeket az alkalmazásokat, mintha a saját adatközpont futtatnák. Ennek az az pontosan Azure Virtual Network lehetővé teszi. Egy VPN-átjáró eszközzel, a rendszergazda állíthat be a virtuális magánhálózat (VPN) a helyi hálózat és a telepített tooa virtuális hálózatot az Azure-ban képező virtuális gépek között. A saját IP v4 cím toohello felhő virtuális gépek rendelje hozzá, mert a saját hálózaton toobe jelennek meg. A szervezet felhasználói hello alkalmazáshoz hozzáférhetnek virtuális gépek, mintha a helyi futtatnák tartalmaz.

Tervezési és a virtuális hálózat, amely megfelelő létrehozásával kapcsolatos további információkért lásd: [virtuális hálózati](virtual-network/virtual-networks-overview.md).

### <a name="express-route"></a>Express Route
![ExpressRoute](./media/fundamentals-introduction-to-azure/ExpressRouteIntroNew.png)   

*Ábra: ExpressRoute használ egy Azure virtuális hálózatra, de útvonalak kapcsolatok keresztül gyorsabban dedikált helyett hello nyilvános internethez.*  

Ha további sávszélesség vagy biztonságos, mint a kapcsolat biztosíthat egy Azure virtuális hálózatra van szüksége, megtekintheti az ExpressRoute. Bizonyos esetekben ExpressRoute is mentheti, pénzt. Továbbra is szüksége lesz egy virtuális hálózatot az Azure-ban, de hello hivatkozás Azure és a webhely között, amelyek nem halad át hello dedikált kapcsolatot használ nyilvános internethez. A rendezés toouse ezt a szolgáltatást, szüksége lesz a hálózati szolgáltatóhoz, vagy exchange szolgáltató megállapodást toohave.

Az ExpressRoute-kapcsolat beállítása további időt igényel, és tervezési, ezért érdemes toostart pont-pont VPN-nel rendelkező, majd telepítse át a tooan ExpressRoute-kapcsolatot.

ExpressRoute kapcsolatos további információkért lásd: [ExpressRoute műszaki áttekintés](expressroute/expressroute-introduction.md).

### <a name="traffic-manager"></a>Traffic Manager
![TrafficManager](./media/fundamentals-introduction-to-azure/TrafficManagerIntroNew.png)   

*Ábra: Az Azure Traffic Manager lehetővé teszi, tooroute globális forgalom tooyour szolgáltatás intelligens szabályok alapján.*

Ha több adatközpontot az Azure alkalmazás fut, akkor használhatja Azure Traffic Manager tooroute felhasználók által érkező kérések intelligens módon hello alkalmazás példánya között. Is irányíthatja a forgalmat nem fut az Azure-ban, amíg azok elérhetők a tooservices hello internet.  

Csak egyetlen részét hello world felhasználóival az Azure-alkalmazások futtatása előfordulhat, hogy csak egy Azure-adatközpontban. Az alkalmazás felhasználók többi részén Elszórva hello world, azonban, a több adatközpontokban, nagy valószínűséggel toorun lehet, hogy még az összes. A második esetben probléma szembesülhetnek: hogyan tegye intelligens módon közvetlen felhasználók tooapplication példányok? Legtöbbször ennek hello érdemes minden felhasználó tooaccess hello datacenter legközelebbi tooher, mivel azt valószínűleg ad saját hello legjobb válaszidő. De mi történik, ha az hello alkalmazás ezen példányát túlterhelt vagy nem érhető el? Ebben az esetben kellene lennie töltött toodirect saját kérelem automatikusan tooanother datacenter. Ez az pontosan, mi történik az Azure Traffic Manager által.

az alkalmazás tulajdonosa hello meghatározza, hogy adja meg, hogy a felhasználók által érkező kérések hogyan legyen irányított toodatacenters, majd ki ezeket a szabályokat a Traffic Manager toocarry támaszkodik szabályok. Például a felhasználók általában lehet, hogy irányított toohello legközelebbi Azure-adatközpontban, de ha hello válaszidőt azok alapértelmezett datacenter hello válaszidejét más adatközpontok meghaladja tooanother egy küldött. A sok felhasználóval rendelkező globálisan elosztott alkalmazások problémák egy beépített szolgáltatás toohandle ezek például akkor hasznos.

A TRAFFIC manager Directory szolgáltatás (DNS) tooroute felhasználók tooservice végpontok használja, de további forgalom nem halad keresztül Traffic Manager a kapcsolat létrehozása után. Így a Traffic Manager szűk keresztmetszet, amely a szolgáltatás kommunikációs lelassíthatják nem.

## <a name="developer-services"></a>Fejlesztői szolgáltatások
Azure számos eszközök toohelp fejlesztők és informatikai szakemberek létre és kezelheti az alkalmazások hello felhőben.  

### <a name="azure-sdk"></a>Azure SDK
Vissza a 2008 hello Azure a legelső megjelenés előtti verziója támogatott csak a .NET development. Napjainkban azonban hozhat létre Azure-alkalmazások lényegében bármilyen nyelven. A Microsoft jelenleg biztosít nyelvspecifikus SDK-k .NET, Java, PHP, Node.js, Ruby és Python. Egy általános Azure SDK-t, amely alapszintű támogatást nyújt bármely nyelvhez, például a C++ is van.  

Ezek az SDK-k segítségével létrehozása, telepítése és kezelése az Azure-alkalmazások. Azok elérhetők vagy a [www.microsoftazure.com](https://azure.microsoft.com/downloads/) vagy GitHub, és a Visual Studio és az eclipse-ben használható. Azure parancssori eszközöket, amelyekkel a fejlesztők bármely szerkesztő vagy a fejlesztői környezetet, beleértve az eszközök telepítéséhez a Linux és a Macintosh-rendszerekből alkalmazások tooAzure is biztosít.

Segít az Azure-alkalmazások létrehozását, valamint ezek SDK-k is nyújt, amelyek segítenek ügyfélkódtáraival létre szoftver, amely Azure-szolgáltatásokat használ. Például akkor lehet, hogy létre egy alkalmazás, amely beolvassa és az Azure BLOB, vagy hozzon létre egy eszközt, amely telepíti az Azure-alkalmazások hello Azure kiszolgálókezelési felületen segítségével.

### <a name="visual-studio-team-services"></a>Visual Studio Team Services
A Visual Studio Team Services alhálózatnév marketing egy szám szolgáltatásokat, amelyek hello Azure toodevelop alkalmazások súgójának lefedik.

tooavoid zavart -, nem biztosít a Visual Studio egy szolgáltatott vagy a Web-alapú verziójával. A Visual Studio helyi futó példányát kell. De számos más eszközt, amely nagyon hasznos lehet.

Az üzemeltetett forrás ellenőrző rendszer verziókezelést és a munka elem követési biztosít a Team Foundation Service nevű tartalmazza.  Akkor is használható Git verziókezelést Ha jobban szeret. És hello projekt által használt adatforrás vezérlő rendszer eltérőek lehetnek. Létrehozhat korlátlan titkos csapatprojektek érhető el a bárhol az hello world.  

A Visual Studio Team Services egy terheléselosztási tesztelési szolgáltatást biztosít. Ön is végrehajthatja a terhelés tesztek létrehozása a Visual Studio hello felhő virtuális gépeken. Hello tooload vizsgálat a kívánt felhasználók teljes száma, és a Visual Studio Team Services automatikusan határozza meg, hány ügynök van szükség, léptetési hello szükséges virtuális gépek és a betöltés tesztek végrehajtása. Ha az MSDN-előfizető, szabad felhasználói perc minden hónap tesztelés terhelés ezer kap.

A Visual Studio Team Services is biztosít, szolgáltatásokat, például a folyamatos integrációt alkot, Kanban modulok és virtuális team helyiségekben gyors fejlesztési támogatása.

**A Visual Studio Team Services-forgatókönyvek**

A Visual Studio Team Services a vállalatok számára toocollaborate világszerte kell, és még nincs hello infrastruktúra a hely toodo így jó választás. A telepítő percben beolvasása, válassza ki a forrásrendszerben vezérlő, és indítsa programozás és felépítése adott napon.  hello team eszközök koordinációs adjon meg egy olyan helyre, és együttműködés és további eszközök hello hello elemzés szükséges tootest biztosít, és gyorsan hangolja az alkalmazás.

De az a helyi rendszer már rendelkező szervezeteknek tesztelheti a Visual Studio Team Services toosee új projektek hatékonyabb, ha.   

### <a name="application-insights"></a>Application Insights
![Application Insights](./media/fundamentals-introduction-to-azure/ApplicationInsights.png)  

*Ábra: Az Application Insights figyelők teljesítmény- és használati az élő web vagy az eszköz alkalmazás.*

Ha az alkalmazás - tette közzé, hogy a mobileszközökön, az asztali számítógépek és a webböngésző - fut az Application Insights megtudhatja, teljesítményét és a felhasználók tevékenységeit vele. Ez akkor is megtartja az összeomlásokat és a lassú válasz számát, riasztás, ha hello ábra kereszt-elfogadhatatlan küszöbértékeket, és segít a problémák diagnosztizálásához.

Egy új szolgáltatás fejlesztésekor megtervezése toomeasure felhasználóival annak sikerességét. Használati minták elemzésével, mi működik a legjobban az ügyfeleknek megértéséhez, és javíthatja az alkalmazás minden fejlesztési ciklusban.

Bár az Azure-ban található, az Application Insights-alkalmazások esetén is be- és kikapcsolását Azure széles és az egyre növekvő számos működik. J2EE és az ASP.NET web apps ismertetnek, valamint iOS, Android, os x és Windows-alkalmazások. Telemetria küldi hello alkalmazás-val készült SDK toobe elemzése és hello jelenik meg az Application Insights szolgáltatás az Azure-ban.

Ha azt szeretné, további speciális elemzés, exportálja a hello telemetriai adatfolyam tooa adatbázis, vagy tooPower BI vagy bármely más eszközök.

**Application Insights-forgatókönyvek**

Ha az alkalmazások fejlesztéséhez. Előfordulhat, hogy a webes alkalmazás vagy egy eszköz-alkalmazást, vagy egy eszköz alkalmazást a webes háttérből.

* Az alkalmazás hello teljesítmény hangolására, hogy közzététele után, vagy amíg módban betölteni tesztelése.  Az Application Insights telemetria összesíti az összes telepített hello példányokból, és diagramokat válaszidejét, kérelem kivételek száma, függőség válaszidejét és más teljesítménymutatók nyújt. Ezek segítenek az alkalmazás teljesítmény hangolására. Kód tooreport beszúrásához pontosabb adatokat, ha esetleg szükség lenne rá.
* Észleli, és az élő alkalmazásban problémák diagnosztizálásához. E-mailben is riasztásokat kaphat, ha teljesítménymutatók telephelyek közötti elfogadható küszöbértékeket. Használatával megvizsgálhatja a felhasználói munkamenetek, például toosee hello kérelem kivételt okozott.
* Nyomon követi a használati tooassess hello sikeres minden új funkció. Amikor egy új felhasználó szövegegység, tervezze meg toomeasure mennyi szolgál, és hogy felhasználók várható céljaik. Az Application Insights teszi alapvető használati adatok, például a weblap nézetek, valamint részletesebben kód tootrack hello felhasználói élmény is beszúrhat.

### <a name="automation"></a>Automatizálás
Senki kedveli hello módon toowaste idő azonos manuális és újra dolgozza fel. Azure Automation szolgáltatásbeli lehetőséget biztosít az Ön toocreate figyelése, kezelése és központi telepítése az erőforrások az Azure környezetben.  

Automatizálási hello színfalak "runbook", amely használja a Windows PowerShell-munkafolyamatok (és csak rendszeres PowerShell) használja. Runbookok végrehajtása felhasználói beavatkozás nélkül toobe célja. PowerShell-munkafolyamatok lehetővé teszi, hogy egy parancsfájl toobe mentén hello pontokon mentett állapotának hello módon. Ha hiba lép fel, nem kell toostart hello elejétől parancsfájlt. Később is újraindíthatja azt hello utolsó ellenőrzőpont. Ezáltal nagy mennyiségű munka közben toomake hello parancsfájl leíró minden lehetséges hibát.

**Automation-forgatókönyvek**

Azure Automation szolgáltatásbeli jó választás tooautomate hello kézi, hosszan futó, hibákhoz vezethet, és gyakran ismétlődő feladatokat az Azure-ban.

### <a name="api-management"></a>API Management
Létrehozásával és alkalmazás-programozó felületek (API) való közzététel hello internet egy közös módon tooprovide szolgáltatások tooapplications. Ha ezek a szolgáltatások resellable (például időjárási adatokat), a szervezet engedélyezhetik más harmadik felek tooaccess ugyanezeket szolgáltatások díjköteles. Méretezhető toomore partnerek, akkor lesz általában toooptimize kell és hozzáférés szabályozása.  Egyes partnerek még esetleg egy másik formátumú hello adatokat.

Az Azure API Management megkönnyíti a szervezetek toopublish API-k toopartners, az alkalmazottak és a külső fejlesztők biztonságosan és léptékű. Egy másik API-végpont szolgál, és tevékenységéért, egy proxy toocall tényleges végpontja hello ugyanakkor biztosítható a szolgáltatásokat, mint a gyorsítótár, transformation, szabályozás, hozzáférés-vezérlési, és elemzés összesítést.

**API-kezelési forgatókönyveket**

Tegyük fel, a vállalat rendelkezik-e az eszközök, például egy olyan szállítási rendelkezik olyan eszközökkel, minden teherautó hello közúti a vállalat összes kell toocall hátsó tooa központi tooget szolgáltatásadatok--.  Biztosan hello vállalati tooset érdemes fel a rendszer tootrack saját teherautók, hogy azok megbízhatóan előre jelezni, és frissítse a szállítási idő. Azt tudja hány teherautók rendelkezik, és tervezze meg megfelelően.  Minden egyes teherautó visszahívja tooa központi helyen az Elhelyezés és sebességét, adatok, és lehet, hogy több eszköz szükséges.

A vállalat szállítási hello az ügyfél valószínűleg akkor is előnyös a pozicionáló adatok.  hello ügyfél használhatja azt tooknow milyen távolságban termékek rendelkezik tootravel, ahol azok elakadnak, mennyi azok bizonyos útvonalon fizető (Ha mi fizetett tooship együtt). Vállalati szállítási hello már összesíti az adatokat, ha sok ügyfél lehet, hogy díj ellenében azt.  De majd vállalati szállítási hello tooprovide toogive ügyfelek hello adatokat úgy. Hozzáférés toocustomers biztosítanak, ha azok nem rendelkezhet szabályozhatják, hogy milyen gyakran hello adatok le kell kérdezni. Tooprovide szabályok való hozzáférés adatokkal rendelkeznek. Ezek a szabályok összes kellene külső API épített toobe. Ez azért, ahol az API Management segítségével.  

## <a name="identity-and-access"></a>Identitás és hozzáférés
Identitás használata a legtöbb alkalmazás részét képezi. Egy felhasználó ismerete lehetővé teszi, hogy a döntse el, hogyan azt kell működjön együtt, hogy a felhasználó egy alkalmazást. Azure services toohelp követése azonosító biztosít, valamint a már használhat identitástárolók integrálva.

### <a name="active-directory"></a>Active Directory
A legtöbb címtárszolgáltatások, például a Azure Active Directory felhasználók és a hello szervezetek tartoznak kapcsolatos információkat tárolja. Lehetővé teszi a felhasználóknak a bejelentkezéshez, majd eszközök jogkivonatokkal úgy is dönthetnek tooapplications tooprove az identitásukat. Lehetővé teszi a Windows Server Active Directoryval a helyszínen a helyi hálózaton futó felhasználói adatok szinkronizálása. Hello mechanizmusok és az Azure Active Directory által használt adatok formátumok nem azonos a Windows Server Active Directory, végez hello funkciók is nagyon hasonló.

Az fontos toounderstand, hogy Azure Active Directory által a felhőalapú alkalmazásokhoz elsősorban használatra szolgál. Lehetséges például, vagy más felhőalapú platformokon a Azure-ban futó alkalmazások használhatják. Microsoft saját felhőalapú alkalmazásokhoz, például az Office 365-ben is használják. Ha a kívánt tooextend az Adatközpont hello felhőalapú Azure virtuális gépek és az Azure Virtual Network használatával, azonban Azure Active Directory nem hello megfelelő választás. Ehelyett érdemes toorun Windows Server Active Directory virtuális gépeken.

Azure Active Directory biztosít az Azure Active Directory Graph nevű RESTful API, toolet alkalmazásokhoz férhet hozzá a hello információt tartalmaz. Ez az API lehetővé teszi, hogy minden platform hozzáférés címtárobjektumok és a közöttük lévő kapcsolatok hello futó alkalmazások.  Például az engedélyezett alkalmazás használhatja az API toolearn egy felhasználó, hello csoportok tagja és más információkat. Alkalmazások is láthatja, felhasználók a társadalombiztosítási graph-bérbeadása, azok több intelligens módon működik hello kapcsolatok ember közötti kapcsolatokat.

Ez a szolgáltatás Azure Active Directory hozzáférés-vezérlés, egy másik képessége egyszerűbbé teszi a az alkalmazás tooaccept azonosító adatok a Facebook, Google, a Windows Live ID és egyéb népszerű identitás-szolgáltatóktól. Ahelyett, hogy az igénylő hello alkalmazás toounderstand hello különböző adatok formátumokat és ezek a szolgáltatók által használt protokollokat, hozzáférés-vezérlés az eszköz az összes olyan egyetlen közös formátumra. Azt is lehetővé teszi, hogy egy alkalmazás egy vagy több Active Directory-tartományok bejelentkezések fogadja el. A gyártó biztosít egy SaaS-alkalmazáshoz például Azure Active Directory hozzáférés-vezérlés toogive felhasználók az egyes ügyfelek egyszeri bejelentkezés toohello alkalmazásának használhatja.

Directory szolgáltatások egy alapvető megerősítő a helyszíni számítási. Nem lehet meglepő, hogy azok viselkedése is fontos hello felhőben.

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
![Azure Multi-Factor Authentication](./media/fundamentals-introduction-to-azure/MFAIntroNew.png)   

*. Ábra: A multi-factor Authentication hello funkció a az alkalmazás tooverify egynél több formáját nyújtja azonosítása*

Biztonsági mindig fontos. Többtényezős hitelesítés (MFA) segítségével biztosítja, hogy csak azok a felhasználók maguk hozzáférési fiókjukat. Többtényezős Hitelesítést (más néven a kéttényezős hitelesítés vagy "2FA") esetén a felhasználóknak megadniuk a két ezeket a felhasználói bejelentkezéseket és tranzakciókat identitás ellenőrzése mindhárom módszerét.

* Valami tudja (általában jelszót)
* Valami (megbízható eszközzel rendelkezik, amely nem egyszerű ismétlődik, például a telefon)
* Valami áll (biometria)

Ezért amikor a felhasználó bejelentkezik, is szüksége van rájuk a tooalso igazolják az identitásukat egy mobilalkalmazás, telefonhívást vagy SMS, valamint a jelszavát. Alapértelmezés szerint a Azure Active Directory támogatja a felhasználói bejelentkezések hello annak egyetlen hitelesítési módszerként a jelszavak használatát. Egyéni alkalmazások vagy együtt az Azure AD MFA használható, és a könyvtárak hello MFA SDK. Használhatja azt a helyszíni alkalmazások együtt multi-factor Authentication kiszolgáló használatával.

**MFA-forgatókönyvek**

Bejelentkezés a védelmet a bizalmas fiókokat vehetnek célba például banki bejelentkezések és ahol jogosulatlan bejegyzés rendelkezhetnek a költségeket, magas pénzügyi vagy szellemi tulajdonság forráskódjához hozzáfér.   

## <a name="mobile"></a>Mobiltelefon
A mobileszközön lévő alkalmazások létrehozásakor Azure segítségével adat tárolása hello felhő hitelesíti a felhasználókat, és leküldéses értesítések küldéséhez anélkül, hogy toowrite egyéni kód nagy mennyiségű.

A virtuális gépek, a Cloud Services és a Web Apps a mobilalkalmazások hello háttér alapértékekkel hozhat létre, amíg tölthet sokkal kevesebb idő írás hello alapul szolgáló szolgáltatás-összetevők Azure-szolgáltatások használatával.

### <a name="mobile-apps"></a>Mobile Apps
![Mobile Apps](./media/fundamentals-introduction-to-azure/MobileServicesIntroNew.png)

*. Ábra: A mobilalkalmazások általában alkalmazások, amelyek kommunikáljon a mobileszközök által igényelt funkciókat biztosítja.*

Az Azure Mobile Apps biztosít számos hasznos funkciót, amely takaríthat meg időben a mobilalkalmazás háttérkiszolgálón felépítése közben. Lehetővé teszi toodo egyszerű üzembe helyezést és kezelést az SQL-adatbázisban tárolt adatok. Kiszolgálóoldali kód könnyen használhatja például a blob storage vagy a MongoDB további adatok tárolási lehetőségek. Mobile Apps támogatja az értesítéseket, abban az esetben, ha bizonyos esetekben használhatja a Notification Hubs alább leírtak szerint.  hello szolgáltatást is rendelkezik egy REST API-t, hogy a mobilalkalmazás meghívhatja tooget dolgozott. Mobile Apps is segítségével hello tooauthenticate felhasználók Microsoft és az Active Directory használatával, valamint más jól ismert Identitásszolgáltatók, például a Facebook, a Twitter és a Google.   

Egyéb Azure-szolgáltatásokat, mint a Service Bus és feldolgozói szerepkörök használja, és tooon helyszíni rendszerek. 3. fél bővítmények hello Azure Store (például az e-mailek SendGrid) tooprovide további funkciókat is felhasználhat.

Natív ügyfél-könyvtárakban Android, iOS, HTML/JavaScript, Windows Phone és Windows Áruházbeli alkalmazások könnyebb toodevelop elérhetővé minden nagyobb mobilplatform. A REST API lehetővé teszi a Mobile Services-adatok toouse és az alkalmazások különböző platformokon hitelesítési funkcióját. Egyetlen mobilszolgáltatás biztonsági több ügyfél alkalmazásokat, egy egységes felhasználói élményt biztosíthat az eszközön.

Azure már támogatja a jelentős mértékű, mert képes kezelni hello forgalom, amint az az alkalmazás népszerűbbnek válik.  Figyelés és naplózás támogatottak toohelp problémák elhárításához, és a teljesítmény kezelése.

### <a name="notification-hubs"></a>Notification Hubs
![NotificationHubs](./media/fundamentals-introduction-to-azure/NotificationHubsIntroNew.png)  

*. Ábra: A Notification Hubs gyakran alkalmazások, amelyek kommunikáljon a mobileszközök által igényelt funkciókat biztosítja.*

Írhat kódot toodo értesítések az Azure Mobile Apps, amíg a Notification Hubs optimalizált toobroadcast több millió nagy mértékben testre szabott leküldéses értesítések percen belül.  Nincs tooworry vonatkozó adatait, például a mobil szolgáltatónként vagy a készülék gyártójához. Egy vagy több millió felhasználó egyetlen API-hívással Megcélozhat.

Notification hubs használatával bármilyen háttérrendszerből a tervezett toowork. Azure Mobile Apps, egy egyéni háttér hello felhőben futó bármely szolgáltató vagy egy helyszíni háttér is használhatja.

**Értesítési központ forgatókönyvek** ahol játékosok tartott viszont mobil játék írna, szükség lehet toonotify player 2 adott player 1 befejezte a saját kapcsolja. Ha mást nem kell toodo, használhatja a Mobile Apps csak. De ha 100 000 felhasználó kellett játék a, és szeretné toosend egy időpontot, bizalmas a különleges ajánlat tooeveryone, a Notification Hubs hello megfelelőbb választás.

Legfrissebb hírek küldhet, eseményeket és a termék bejelentési értesítések toomillions felhasználók sport és kis késésű. A vállalatok értesítheti új idő-és nagybetűket indított kommunikációval kapcsolatban, például az értékesítési érdeklődők az alkalmazottak számára, az alkalmazottaknak nem kell ellenőrizze e-mailben vagy más alkalmazások toostay küldjenek tooconstantly. Egy-egyszer használatos jelszavak a többtényezős hitelesítés is küldhet.

## <a name="back-up"></a>Biztonsági mentése
Minden vállalati toobackup és visszaállítási adatokra van szüksége. Az Azure toobackup használja, és állítsa vissza az alkalmazás a hello felhőalapú vagy helyszíni. Az Azure biztonsági mentés hello típusától függően különböző beállítások toohelp kínál.

### <a name="site-recovery"></a>Site Recovery
Az Azure Site Recovery (korábbi nevén Hyper-V helyreállítás-kezelő) segítségével védi a fontos alkalmazások hello replikációs és helyreállítási összehangolása a különböző helyek között. A Site Recovery funkció tooprotect alkalmazások Hyper-v, VMWare vagy SAN tooyour saját másodlagos hely, tooa üzemeltető hely vagy tooAzure alapján, és elkerülheti a hello költségeket és összetett létrehozása és kezelése a saját másodlagos helyet biztosít. Azure titkosítja az adatokat, és a kommunikációt, és rendelkezik túl engedélyezheti a titkosítást az adatokat nyugalmi hello lehetőséget.

Folyamatosan figyeli a szolgáltatások hello állapotát, és segítségével automatizálhatja hello rendezett helyreállítási szolgáltatások hello eseményeket, ha egy hely leállás hello elsődleges adatközpontjában. Virtuális gépek elérhetővé tehető egy vezénylését módon toohelp visszaállítási szolgáltatásban gyorsan, még akkor is összetett több rétegből álló munkaterheléseknél.

A Site Recovery működik együtt a meglévő technológiák, például a Hyper-V replika, a System Center és SQL Server Always On. Tekintse meg [Azure Site Recovery áttekintését](site-recovery/site-recovery-overview.md) további részleteket.

### <a name="azure-backup"></a>Azure Backup
![Azure Backup](./media/fundamentals-introduction-to-azure/AzureBackupIntroNew.png)  

*. Ábra: Az Azure Backup lemezein lévő adatokat a helyszíni Windows Server kiszolgálókról hello felhőbe.*  

Azure biztonsági mentés a Windows Server hello felhő rendszert futtató helyszíni kiszolgálók lemezein lévő adatokat. Kezelheti a biztonsági másolatokat közvetlenül hello a Windows Server 2012, Windows Server 2012 Essentials vagy a System Center 2012 – Data Protection Manager biztonsági mentési eszközök. Másik megoldásként egy speciális biztonságimásolat-készítő ügynök is használhatja.

Adatok azért biztonságosabb, mert a biztonsági mentések átvitele előtt titkosított és tárolt titkosított az Azure és a feltöltött tanúsítvány által védett. hello szolgáltatás ugyanazon redundáns és magas rendelkezésre állású az adatvédelem az Azure Storage található hello használja.  Is biztonsági másolatot készíteni fájlok és mappák rendszeres ütemezés szerint vagy azonnal, vagy teljes vagy növekményes biztonsági mentések futtatása. Adatok biztonsági mentésének toohello felhő, miután jogosult felhasználók egyszerűen helyre lehet állítani biztonsági mentések tooany kiszolgáló. Is biztosít, konfigurálható az adatmegőrzési házirendek, az adattömörítés és az adatok átvitele sávszélesség-szabályozás így hello költség toostore és átviteli adatok kezelhetők.

**A forgatókönyvek az Azure Backup szolgáltatásra**

Ha még a Windows Server vagy a System Center használ, az Azure backup egy természetes megoldás a kiszolgálók fájlrendszer, a virtuális gépek és az SQL Server-adatbázisok biztonsági mentése.  A titkosított, ritka és tömörített fájlok működik. Nincsenek bizonyos korlátozások mellett, így akkor [hello Azure biztonsági mentés szükséges előfeltételek ellenőrzése](http://technet.microsoft.com/library/dn296608.aspx) első.

## <a name="messaging-and-integration"></a>Üzenettovábbítás és integráció
Függetlenül attól, milyen műveletet a kód gyakran toointeract más kóddal kell.  Bizonyos helyzetekben szükség van csak a alapvető aszinkron üzenetkezelési. Más esetekben összetettebb kapcsolati szükség. Azure biztosít néhány különböző módokon toosolve ezeket a problémákat. 5. ábra hello lehetőségeit szemlélteti.

### <a name="queues"></a>Üzenetsorok
![Az Azure Service Bus-továbbító](./media/fundamentals-introduction-to-azure/QueuesIntroNew.png)

*. Ábra: A várólisták laza alkalmazás részei között csatolási engedélyezése, és lehetővé teszi a méretezés.*  

Egy egyszerű ötlet Queuing: egy alkalmazás egy üzenet helyezi a sorhoz, és az üzenet egy másik alkalmazás végül olvasható. Ha az alkalmazásnak csak az egyszerű szolgáltatás, Azure várólisták lehet hello legjobb választás.

Hello módon hello Azure nőtt adott idő alatt, mert a Azure tárolási sorok és a Service Bus-üzenetsorok hasonló üzenetsor-kezelési szolgáltatások biztosítják. Miért van szüksége a toouse hello keresztül más hello okok ismertetnek hello viszonylag műszaki papír [Azure várólisták és a Service Bus-üzenetsorok - az képest és ellentétben](http://msdn.microsoft.com/library/azure/hh767287.aspx).  A sok esetben vagy fog működni.

**Várólista forgatókönyvek**

Cloud Services az alkalmazás egy közös várólisták ma használata, ha a feldolgozói szerepkör példánya belüli kommunikációhoz a webes szerepkör példánya toolet hello.

Tegyük fel, hogy megosztható videó az Azure alkalmazás létrehozása. hello alkalmazást futtató felhasználók feltöltése és nézzenek, és egy C# nyelven íródtak, amely a feldolgozói szerepkört feltöltött webes szerepkörrel rendelkező videó különböző formátumokba PHP kód áll.

A webes szerepkör példánya új videó lekérdezi egy felhasználó, amikor azt is hello videó tárolása egy blobot, majd küldje el egy üzenet tooa feldolgozói szerepkör keresztül szólítja fel, ahol ez új toofind videó. A feldolgozói szerepkör példány-it nem számít, mely egy-fognak majd olvasási üdvözlőüzenetére hello várólistából, és elvégzi a szükséges hello videó fordítások hello háttérben.

Egy alkalmazás, így szerkezetének kialakítása lehetővé teszi, hogy az aszinkron feldolgozás, és azt is lehetővé teszi hello alkalmazás könnyebb tooscale, mivel webes szerepkörpéldányokat és a szerepkör feldolgozópéldányok száma hello egymástól függetlenül eltérőek lehetnek. Hello várólista mérete növekszik és csökken a feldolgozói szerepkörök eseményindító tooscale hello számos is használhatja. Túl magas, és a további szerepkörök hozzáadásához. Ha annak mérete kisebb, csökkentheti a futó szerepkörök toosave pénz hello száma.  

Használhatja a ugyanilyen mintájú az alkalmazás számos különböző részei között, még akkor is, ha a webes és feldolgozói szerepkörök nem használnak.  Tooscale hello részek egyik oldala sem hello várólista növekszik és csökken, igény szerint lehetővé teszi, és feldolgozási időt igényel.

### <a name="service-bus"></a>Service Bus
Hello felhőalapú, mobil eszköz, vagy a fürtnevek máshol, az Adatközpont futtatja alkalmazások kell-e toointeract. hello Azure Service Bus célja futó lényegében bárhonnan toolet alkalmazások exchange-adatok.

Továbbá toohello várólistán található (egy az egyhez típusú) a korábban ismertetett, Service Bus is biztosít tooother kommunikációs módszer.

#### <a name="service-bus-relay"></a>Service Bus Relay
![Az Azure Service Bus-továbbító](./media/fundamentals-introduction-to-azure/ServiceBusRelayIntroNew.png)

*Ábra: Service Bus Relay lehetővé teszi, hogy egy tűzfal másik oldalon található alkalmazások közötti kommunikációt.*

A Service Bus lehetővé teszi, hogy a továbbítási szolgáltatáshoz, a közvetlen kommunikáció biztosítása a biztonságosan toointeract tűzfalon keresztül. Service Bus-továbbítók üzenetváltásokban keresztül végpont hello felhőben ahelyett, hogy helyileg üzemeltetett alkalmazások toocommunicate engedélyezéséhez.

**Service Bus Relay forgatókönyvek**

Alkalmazások Service Buson keresztül kommunikáló lehet Azure alkalmazásokat és szoftvereket. néhány más felhőalapú platformot. Ezek azonban hello felhő kívül futó alkalmazások is lehetnek. Például egy légitársaság foglalási szolgáltatások saját adatközponton belüli számítógépek megvalósító gondol. hello légitársaság tooexpose ezen szolgáltatások toomany ügyfelek, beleértve a bejelentkezési pultokról repülőtereken, a foglalási ügynök terminálok és a talán még akkor is ügyfelek telefonok van szüksége. Használhatja a Service Bus toodo a különböző alkalmazások közötti hello lazán összekapcsolt kapcsolat létrehozása.

#### <a name="service-bus-topics-and-subscriptions"></a>Service Bus-üzenettémák és -előfizetések
![Az Azure Service Bus-Üzenettémák](./media/fundamentals-introduction-to-azure/ServiceBusTopicsSubsIntroNew.png)   
 *. Ábra: A Service Bus-üzenettémakörök lehetővé teszi a több alkalmazások toopost üzenetek és más alkalmazások toosubscribe tooreceive üzeneteket, amelyek megfelelnek egy adott feltételnek.*

A Service Bus témakörök és előfizetések közzététel és előfizetés mechanizmust biztosít. Publish-subscribe egy alkalmazás küldhet üzeneteket tooa témakör, míg más alkalmazások előfizetések toothis témakör hozhat létre. Ez lehetővé teszi, hogy egy-a-többhöz kommunikáció egy adott alkalmazáscsoportra, ha engedélyezi, hogy ugyanaz az üzenet olvasható által több címzettnek hello között.

**Service Bus-Üzenettémák és előfizetések forgatókönyvek**

Bármikor hoz létre Ha sok üzeneteket, amelyek minden fontos, de különböző alárendelt rendszereket csak kell azokat toolisten toodiffering részhalmaza, Service Bus-témakörbe és előfizetések nem jó választás.

### <a name="biztalk-services"></a>BizTalk Services
![BizTalk szolgáltatások](./media/fundamentals-introduction-to-azure/BizTalkServicesIntroNew.png)   
 *. Ábra: A BizTalk szolgáltatások hello képességét tootransform XML-üzenetek formátumainak hello felhőben biztosít.*

Egyes esetekben kell csatlakozás rendszerek, amelyek segítségével különböző üzenetkezelési formátumok kommunikálnak. Esetében gyakori, üzleti toohave különböző adatbázis sémák és XML messaging formátumok, akkor is, ha egy általános szabvány érhető el. Helyett a egyéni kódot ír, használhatja a BizTalk Serveren helyszíni toointegrate különböző rendszerek.  Azure BizTalk-szolgáltatásokat biztosít a hello szolgáltatást, de hello felhőben ugyanolyan típusú. Is kell fizetnie csak mi használ, és nem kell foglalkoznia méretezési például tooon helyszíni kellene lennie.

**BizTalk szolgáltatások forgatókönyvek**

Üzleti vállalatközi (B2B) kapcsolati gyakran igényelnek fordítási az ilyen típusú.  A vállalat felépítése repülőgépek igények tooorder részeit, például különböző részeit beszállítók. Sok részből szállítók lesz.  Rendeléseket rendszerekből közvetlenül hello repülőgép szerkesztők hello szállítók rendszerekbe automatizált toogo kell lennie.  Sem üzleti toochange szeretne rendelni a core rendszerek és üzenetformátumok, és nem nagyon valószínű, hogy ezek a formátumok vannak hello azonos. BizTalk szolgáltatások üzenetek igénybe, és hello új formátumok mindkét irányba közötti lefordítani. Vagy hello repülőgép szállító munkahelyi tootranslate hello vagy különböző hello is, attól függően, hogy ki szeretne rendelni a fordítási szükséges további ellenőrzési és hello mennyiségét.     

## <a name="compute-assistance"></a>Számítási támogatás
Azure szolgáltatásokról, amelyek nem igényelnek toorun mindig hello segítséget nyújt.  

### <a name="scheduler"></a>Scheduler
![Azure Scheduler](./media/fundamentals-introduction-to-azure/SchedulerIntroNew.png)   
*. Ábra: Azure Schedulerrel tartalmaz egy módja tooschedule feladatok megadott idő az adott időpontban.*

Egyes esetekben alkalmazásokat csak kell toorun egy bizonyos időpontban. A Azure-ban költségtakarékosabb munkavégzésben az ilyen típusú alkalmazások ahelyett, hogy egy alkalmazás csak 24 x 7 adatok tooprocess várakozik tovább futnak. Azure Schedulerrel lehetővé teszi tooschedule futtatásakor az alkalmazás kell alapján időközönként idő vagy a naptárban. Megbízható, és ellenőrzi, hogy egy folyamat fut-e akkor is, ha a hálózati, a gép és a data center hibákat észlel. Hello Feladatütemező REST API toomanage használja ezeket a műveleteket.

Ütemezett riasztás esetén a Feladatütemező HTTP vagy HTTPS üzenetek adott tooa-végpont elküld, vagy egy üzenet lehet helyezni a tároló várólista.  Így toohave kell az alkalmazás vagy egy elérhető végpontot, vagy azt a tároló várólista figyelése. Majd hello üzenetet kap, miután műveleteket hajthat végre azt programozott intézkedéseket.

**A Feladatütemező forgatókönyvek**

* Ismétlődő alkalmazás műveletek: Tegyük fel, a szolgáltatás előfordulhat, hogy rendszeres időközönként adatokat lekérni a twitter és hello adatgyűjtést rendszeres adatcsatornára be.
* A napi karbantartás: napló dolgoz fel, vagy csatlakozási, elvégzése biztonsági mentések és más időnként feladatok ütemezése.
* Feladatok, amelyek éjszaka futtatni.
* Webes alkalmazások feladatokat, például napi törlése a naplókat, a biztonsági mentés, és egyéb karbantartási műveleteket. A rendszergazda dönthet toobackup saját adatbázis hello a naponta 1 órakor következő 9-es hónapos, például.

hello Scheduler API lehetővé teszi a toocreate, frissítése, törlése, megtekintése és kezelése feladatgyűjteményei és ütemezett feladatok programozott módon.

## <a name="performance"></a>Teljesítmény
Az alkalmazás mindig fontos teljesítményét. Alkalmazások általában tooaccess és újra hello ugyanazokat az adatokat. Egyirányú tooimprove teljesítmény tookeep, hogy szorosabb toohello alkalmazás másolatát, minimalizálja a hello időre szükség tooretrieve azt. Azure ezzel különböző szolgáltatásokat biztosít.

### <a name="azure-caching"></a>Azure Caching
![Azure Caching](./media/fundamentals-introduction-to-azure/AzureCacheIntroNew.png)   
 **Ábra: Is az Azure-alkalmazások gyorsítótárazza az adatokat a memóriában, és még ossza sok feldolgozói szerepkörök között**

Bármely Azure adatkezelés szolgáltatások-SQL-adatbázis, a táblák vagy a Blobok tárolt adatok elérése-meglehetősen gyorsan elvégezhető. Még a memóriában tárolt adatok elérése még gyorsabb. Ebből kifolyólag tartja a gyakran használt adatokhoz egy memórián belüli másolatot javíthatja az alkalmazások teljesítményének. Ezzel Azure memórián belüli gyorsítótárazáshoz toodo.

A Cloud Services alkalmazás adatok tárolása a gyorsítótár, majd közvetlenül az állandó tároló tooaccess anélkül lekéréséhez. az alkalmazáshoz tartozó virtuális gépek vagy virtuális gépek biztosítani dedikált kizárólag toocaching hello gyorsítótár tarthatjuk fenn. Mindkét esetben hello gyorsítótár terjeszthető, hello adatokkal tartalmaz terjedésének egy Azure-adatközpontban több virtuális gépek között.

Azure számos különböző gyorsítótár technológia, amely rendelkezik vette idővel rendelkezik. Hello ahhoz, azok lett bevezetve, van egy megosztott szerepköralapú, felügyelt és a Redis gyorsítótár. megosztott hello gyorsítótárazás régebbi technológia, és ne hozzon létre új megvalósítások vele. hello felügyelt gyorsítótár tartalmaz hello szolgáltatást a hello szerepköralapú gyorsítótár, de kívül felügyelt szolgáltatásként hello Azure felügyeleti portálon. Redis gyorsítótár hello jelenleg előzetes verzióban érhető. hello Redis megvalósítási funkciók legnagyobb számú hello és ajánlott, ha új gyorsítótárazási kódot ír.

**Az Azure Cache forgatókönyvek**

Termékkatalógus ismételten olvasó alkalmazás előfordulhat, hogy az ilyen gyorsítótárazás előnye, például óta hello adatokat kell lesz elérhető gyorsabban. hello technológiát is támogatja a zárolás, ha engedélyezi, hogy az olvasási/írási, valamint a csak olvasható adatok használható. És az ASP.NET-alkalmazások hello szolgáltatás toostore munkamenetadatok egy konfigurációmódosítás használható.

### <a name="content-delivery-network"></a>Content Delivery Network
![Azure CDN](./media/fundamentals-introduction-to-azure/CDNIntroNew.png)   
 **Ábra: Blob példánya hello világ különböző helyeken gyorsítótárazható.**

Tegyük fel, hello világ különböző felhasználók által használt blobadatokat toostore van szüksége. Lehet, hogy hello legújabb világbajnokság match, például vagy illesztőprogram-frissítést, vagy egy népszerű e-könyv videó. A több Azure adatközpontjaiban történő tárolása hello adatok másolatát segít, de az számos felhasználók, esetén valószínűleg nem elegendő. A még jobb teljesítmény érdekében hello Azure CDN is használhatja.

hello CDN helyek több tucatnyi hello világ, minden egyes kompatibilis Azure BLOB tárolásának rendelkezik. hello hello world egyes részei a felhasználó hozzáfér egy adott blob, először hello információkat tartalmaz a rendszer átmásolja az egy Azure-adatközpontban adott földrajzi helyi CDN tárba. Ezt követően fér hozzá a része, hello world hello blob másolási tárolja a hello CDN fogja használni-nem szükséges toogo összes hello módon toohello legközelebbi Azure-adatközpontban. hello eredménye felhasználók bárhol hello world gyorsabb hozzáférés toofrequently elért adatok.

**CDN-forgatókönyvek**

A Media Services toodeliver videó világszerte közös toouse CDN. Videó általában nagy, és nagy mennyiségű sávszélesség szükséges.  A Media Services rendszer túlmutatnak máshol ezen a lapon.

## <a name="big-data-and-big-compute"></a>Big Data típusú adatok és nagy számítási
### <a name="hdinsight-hadoop"></a>HDInsight (Hadoop)
![HDInsight](./media/fundamentals-introduction-to-azure/HDInsightIntroNew.png)   
 **Ábra: HDInsight segítségével hello tömeges feldolgozási túl nagy mennyiségű adatok**

Sok éve hello tömeges adatok elemzési még nem lett végrehajtva egy relációs adatbázis-kezelő építését adatraktárban tárolt relációs adatok. Az ilyen üzleti elemzés továbbra is fontos, és a hosszú idő toocome az lesz. De mi történik, ha azt szeretné, hogy tooanalyze hello adatok nagy, hogy relációs adatbázisok csak nem tudja kezelni azt? És tegyük fel, hogy hello adatok nem relációs? Előfordulhat, hogy kiszolgálónaplókban egy adatközpontban, például vagy korábbi eseményadatokat az érzékelők, illetve valami mással. Ilyen esetben van big Data típusú adatok probléma ún. Egy másik módszer van szüksége.

hello meghatározó technológia ma a big Data típusú adatok elemzésére szolgáló Hadoop. Apache nyissa meg a forrás-projektet, ez a technológia hello Hadoop elosztott fájlrendszerrel (HDFS) használatával adatokat tárolja, majd lehetővé teszi, hogy a fejlesztőket a MapReduce-feladatok tooanalyze adatok. HDFS adatok több kiszolgáló között terjed ki, majd az egyes, ha engedélyezi, hogy a hello big Data típusú adatok hello MapReduce feladatot futtat adattömböket írnak párhuzamosan kell.

A HDInsight az Apache Hadoop-alapú szolgáltatás hello Azure hello név. A HDInsight lehetővé teszi, hogy a HDFS hello fürtön adatokat tárolhatnak, és szét a több virtuális géphez. Virtuális gépek között a MapReduce-feladatok hello logikát is terjed. Ugyanúgy, mint a helyszíni Hadoop adatok feldolgozott helyileg-hello programot, és működik a hello adatok is hello az ugyanazon Virtuálisgép- és a jobb teljesítmény párhuzamosan. HDInsight is tárolhatja az adatokat az Azure tárolási tárolóban (ASV), amely blobokat használ.  ASV használata lehetővé teszi toosave pénz törlése a HDInsight-fürthöz, ha nincsenek használatban, de még mindig tartsa adatait hello felhőben.

HDinsight hello Hadoop ökoszisztémájának, valamint egyéb összetevői támogatja, beleértve a Hive és a Pig. A Microsoft is létrehozott összetevőket könnyebb toowork HDInsight által előállított adatokat tartalmazó hagyományos Üzletiintelligencia-eszközök, például a hello HiveODBC adapter és a Data Explorer együttműködik az Excel használatával.

### <a name="high-performance-computing-big-compute"></a>Nagy teljesítményű számítástechnikai (nagy számítási)
Hello legtöbb vonzó módon toouse a felhőplatform egyik toorun nagy teljesítményű számítástechnikai (HPC) és más "Nagy számítási" alkalmazásokat. Példák a speciális mérnöki készült alkalmazások toouse hello szabványos Message Passing Interface (MPI), valamint az úgynevezett embarrassingly párhuzamos alkalmazások, például pénzügyi kockázat modellek.

hello lényege nagy számítási kód végrehajtása folyik sok számítógépre következő hello ugyanannyi időt vesz igénybe. Az Azure Ez azt jelenti, több virtuális gép fut egyszerre, minden működik-e a párhuzamos toosolve kapcsolatos problémára. Ez szükséges módon tooresources és tooschedule alkalmazásmódjai, azaz toodistribute a munkahelyi ezek a példányok között. A Microsoft szabad HPC Pack és a számítási fürt megoldásait elvégezhetnek is Azure számítási és infrastruktúra-szolgáltatásokat tooadd kapacitás igény szerinti tooan a helyszíni számítási fürt, vagy nagy számítási alkalmazások futtatásához teljes egészében a hello Azure, kihasználva felhő.

Azure biztosít egy virtuális gép mag, memória, lemez kapacitását, és más jellemzőkkel toomeet hello követelmények a különböző alkalmazások különböző konfigurációkkal rendelkező példány mérete. hello új A8 és A9 példányok tevékenységeket is számos számítási igényes munkaterhelések és párhuzamos MPI alkalmazások különösen fontos, mivel nagy sebességű, multicore processzort és nagy mennyiségű memóriával rendelkeznek. Egyes konfigurációk hello példányok kihasználása egy alacsony késésű és nagy átviteli alkalmazás hálózati hello felhőben, amely tartalmazza a távoli közvetlen memória-hozzáférés (RDMA) technológia a maximális hatékonyság párhuzamos MPI-alkalmazások.

Az Azure is kínál alkalmazásfejlesztők nagy számítási és a partnerek számítási képességekre, szolgáltatások, architektúra lehetőségek és fejlesztői eszközök teljes készletét. Azure speciális adatok munkafolyamatok érintő egyéni nagy számítási munkafolyamatok támogatja, és minták is méretezhető toothousands az ütemezés feladat- és számítási magok.

## <a name="media"></a>Média
![Az Azure Media Services](./media/fundamentals-introduction-to-azure/MediaServicesIntroNew.png)   
 **Ábra: A Media Services szolgáltatás egy platform, az alkalmazásokat, amelyek a videó és egyéb adathordozó tooclients körül hello world.**

Videó ma teszi ki internetes forgalmat nagy része, és százalékosan holnap még nagyobb lesz. Még biztosít hello Web nem egyszerű. Az változók számos, például a hello kódolási algoritmus és hello megjelenítési hello felhasználói képernyő felbontása. Videó is általában toohave felszakadásáig igény, például egy szombat éjszakai csúcs, amikor több személlyel döntse el, az online movie toowatch szeretnének.

Megadott annak időben népszerűvé vált, akkor egy biztonságos tipp, hogy számos új alkalmazás jön létre a videoeszközök használata. Még néhány mindegyike el lesz szüksége a toosolve hello az azonos problémákat, és elvégzése önállóan problémák megoldásához mindegyiknél nincs értelme. A jobb megoldás, toocreate közös megoldást nyújt az alkalmazások sok toouse platformot. És ezen a platformon hello felhő felépítése néhány egyszerű előnyeit. Széles körben elérhető, használatalapú fizetési alapon lehet, és az igény szerinti, amelyek a video-alkalmazások gyakran szembesülhetnek hello variancia is kezelni tud.

Az Azure Media Services kezeli ezt a problémát. Felhő összetevők megkönnyítő élettartama egyszerűbb létrehozását és a futó alkalmazások video- és egyéb adathordozó használatával biztosít.

Hello. ábrán látható, a Media Services összetevők biztosít videó és egyéb adathordozó együtt használható alkalmazások. Például az tartalmaz egy adathordozó összetevő tooupload videó betöltési Media Services (helyén az Azure-Blobokkal), egy kódolási összetevője, amely támogatja a különböző video- és formátumok, a tartalomvédelem összetevője, amely digitális tartalomvédelmi szolgáltatás ads beszúrni a video-adatfolyamot, az adatfolyamként történő összetevők és egyéb szükséges összetevőt. Microsoft-partnerek is is adjon meg összetevők hello platform, majd küldje el az összetevőket, és a nevében kiszámlázni Microsoft rendelkezik.

Ezen a platformon használó alkalmazások az Azure-on vagy máshol futtathatók. Például egy asztali alkalmazás számára egy videó éles házat előfordulhat, hogy lehetővé teszik a videó tooMedia szolgáltatások, töltse fel a felhasználók majd feldolgozni azt különböző módokon. Azt is megteheti az Azure-on futó tartalomkezelési felhőalapú szolgáltatás előfordulhat, hogy a Media Services tooprocess alapulnak, és videó terjesztése. Ahol csak akkor fut, és bármilyen kezeli, minden egyes alkalmazás úgy dönt, mely összetevők toouse el ezeket a RESTful-felületeket kell.

toodistribute mire képes, az alkalmazás használhat hello Azure CDN, egy másik CDN, vagy csak küldése közvetlenül a bits toousers. Azonban nem kap, a Media Services használatával létrehozott videó képes használni a különböző ügyfél Systems, Windows, Macintosh, HTML 5, iOS, Android, Windows Phone, Flash és a Silverlight. hello célja toomake azt könnyebb toocreate modern médiaalkalmazások.

**Hivatkozások**

Ha több megjeleníti a Media Services működésével, töltse le a hello [Azure Media Services poszter][Azure Media Services Poster].

## <a name="commerce"></a>Kereskedelmi
a szolgáltatott szoftver hello megnövekedhet van átalakítása hogyan létrehozhatunk alkalmazások. Is átalakítása hogyan adunk el alkalmazások. Egy SaaS-alkalmazáshoz hello felhő él, mivel lehet, hogy a lehetséges ügyfeleket kell keresnie az online megoldások lehetővé teszi. És ez a változás toodata, valamint a tooapplications vonatkozik. Miért nem szabad hely személyek toohello felhő kereskedelmi forgalomban kapható adatkészletek esetében? Microsoft-címek is a problémák a hello [Azure piactér](https://azure.microsoft.com/marketplace/).

![Az Azure kereskedelmi](./media/fundamentals-introduction-to-azure/CommerceIntroNew.png)   
 **Ábra: Az Azure piactér és az Azure-tároló lehetővé teszik, hogy megkeresheti és Azure-alkalmazások és a kereskedelmi adatkészletek vásárolni, azokat az Azure-alkalmazások részeként.**

hello közötti hello két különbség, hogy a piactér hello Azure felügyeleti portálon kívül esik, de hello tároló elérhető hello portálon belül. A lehetséges ügyfeleket kereshet toofind Azure alkalmazások, amelyek megfelelnek az igényeinek. Az ügyfelek például demográfiai, pénzügyi adatokat, földrajzi adatok és egyéb, valamint a kereskedelmi adatkészletek kereshet. Találnak valamit, például akkor, ha azok érhető el vagy hello szállítótól hello piactér vagy tároló webhelyeken keresztül közvetlenül, vagy bizonyos esetekben a kezelési portál hello. Bing keresési API-JÁNAK hello keresztül hello piactér hozzáférés toohello eredményeit webes jogosultságot ad alkalmazások is használhatják.

**Kereskedelmi forgatókönyvek**

SendGrid lévő hello Azure tároló, amely lehetővé teszi toosend e-mail alkalmazás. További funkciók, például a megbízható kézbesítési és a statisztika kínál.  Akkor is az alkalmazás és a kapcsolódó szolgáltatások megvásárlása ahelyett próbálja toobuild ilyen infrastruktúra magát.  

## <a name="getting-started"></a>Első lépések
Most, hogy hello nagy vonalakban tekinti, hello következő lépésre van toowrite az első Azure-alkalmazásában. Válassza ki azt a nyelvet, [beolvasása megfelelő SDK hello](/downloads/), és válassza ki azt. Felhőalapú informatika hello új alapértelmezett – első lépések megkezdése.

[Azure Media Services Poster]: http://azure.microsoft.com/documentation/infographics/media-services/
