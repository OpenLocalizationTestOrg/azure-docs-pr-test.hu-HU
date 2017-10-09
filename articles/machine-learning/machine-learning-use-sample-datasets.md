---
title: "aaaUse hello mintaként használható adathalmazt a Machine Learning Studióban |} Microsoft Docs"
description: "A Machine Learning Studio szereplő mintákat hello adatkészletekből leírását. A kísérletek ezek mintaként használható adathalmazt is használhatja."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 03a0b844-e8a7-4896-996f-d3c7a0db7a50
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: c7786478db82d40aaf27c37b3947ded5f042dd70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-sample-datasets-in-azure-machine-learning-studio"></a>Hello mintaként használható adathalmazt használja az Azure Machine Learning Studióban
[top]: #machine-learning-sample-datasets

Amikor az Azure Machine Learning hoz létre egy új munkaterületet, alapértelmezés szerint számos mintaként használható adathalmazt és kísérletek jelennek meg. A mintaként használható adathalmazt számos hello mintákat a hello által használt [Azure Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/). Mások, különböző típusú adatok gépi tanulás általában szerepel példaként szerepelnek.

Ezek az adatkészletek között az Azure Blob storage érhető el. Ezen adatkészletek esetében a következő táblázat hello egy közvetlen hivatkozást biztosít. Hello segítségével a kísérletek ezek az adatkészletek is használhatja [és adatokat importálhat] [ import-data] modul.

a mintaként használható adathalmazt hello részeinek érhetők el a munkaterület **mentett adatkészletek** hello modul paletta toohello balra hello kísérletvászonra megnyitásakor, vagy hozzon létre egy új kísérletben a Machine Learning Studióban található.
Használhatja bármelyik ezek az adatkészletek saját kísérletben tooyour kísérletvászonra húzásával.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<table>

<tr>
  <th align=left>A DataSet neve</th>
  <th align=left>Adatkészlet leírása</th>
</tr>

<tr>
  <td valign=top>Felnőtt nyilvántartásba bevétel bináris osztályozási adatkészlet</td>
  <td valign=top>
Hello 1994 nyilvántartásba adatbázist, működő felnőttek hello korát 16 > 100 módosított bevétel indexű keresztül egy részét.<p> </p><b>Használat:</b> demográfiai toopredict használatával, hogy egy személy szerez évente több mint 50 K személyek besorolását.<p> </p><b>Kapcsolódó kutatási:</b> Kohavi, R., Becker, b, (1996). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Repülőtéri kódok adatkészlet</td>
  <td valign=top>
USA repülőtéri kódokat.<p> </p>Ez az adatkészlet egyes USA repülőtéri hello repülőtéri azonosítóját és a neve mellett hello hely várost és kezeléséről egy sort tartalmaz.
  </td>
</tr>

<tr>
  <td valign=top>Autó price data (Raw)</td>
  <td valign=top>
Ellenőrizze és modell hello ár, beleértve a szolgáltatások, mint a hengerszám és MPG, valamint egy biztosítási kockázati pontszám hello száma autók által kapcsolatos információk.<p> </p>hello kockázati pontszám kezdetben társított automatikus ár, és ezután igazodik a tényleges kockázat tooactuaries symboling, ez a folyamat. + 3 érték azt jelzi, hogy hello automatikus kockázatos, és a -3, hogy az informatikai érték valószínűleg biztonságos.<p> </p><b>Használat:</b> Predict hello kockázati pontszám funkcióihoz regressziós vagy multivariate besorolásával. <p> </p><b>Kapcsolódó kutatási:</b> Schlimmer, J.C. (1987). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Annak bérleti UCI adatkészlet</td>
  <td valign=top>
A dataset UCI kerékpárt bérleti beruházási Bikeshare vállalati Washingtoni kerékpárt bérleti hálózat által származó valós adatok alapján.<p> </p>hello adatkészlet egyes órában 17,379 sorok összesen 2011 és 2012, naponta egy sorból áll. az óránkénti kerékpárt bérlését hello tartomány: 1 too977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB kép</td>
  <td valign=top>
Nyilvánosan elérhető képfájl tooCSV adatok alakítja.<p> </p>hello hello kép alakításának kódja megtalálható hello <strong>szín a K-közép fürtszolgáltatása mennyiségmeghatározási</strong> modell információs lapját.
  </td>
</tr>

<tr>
  <td valign=top>Vér adományozás adatok</td>
  <td valign=top>
Hello vér donor adatbázis hello VÉRTRANSZFÚZIÓS Service Center a Hsin-csú város, Tajvan alapján adatok egy részét.<p> </p>Donor szerepel hello hónap utolsó adományozás óta), és gyakoriságot, vagy hello száma véradás, utolsó adományozás óta eltelt idő, és elajándékozni vér mennyiségét.<p> </p><b>Használat:</b> hello célja toopredict besorolás keresztül e hello donor elajándékozni vér március 2007, ahol 1, és a hello Célidőszak, 0 ország nem ország. <p> </p><b>Kapcsolódó kutatási:</b> Yeh, úgy, (2008). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai <p> </p>Yeh, I-Cheng, Yang, képeznie-Jang, és tudomásul véve, Tao-Ming, "Tudásbázis felderítési az RFM-modellben Bernoulli feladatütemezési," 2008, az alkalmazásokkal szakértői rendszereket <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Az Amazon könyv értékelést</td>
  <td valign=top>
Az Amazon, hello amazon.com webhelyről Pennsylvania és egyetemi kutatói által végrehajtott könyvek értékelést (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">véleményeket</a>). Lásd: hello tanulmány, "adatainak, Bollywood, elterjedése-Box és Blenders: tartomány kiigazítása véleményeket besorolási" John Blitzer, be van jelölve Dredze és Fernando Pereira; Számítási Linguistics (ACL), a társítást a 2007.<p> </p>hello eredeti dataset adatkészletben rangsort 1, 2, 3, 4 vagy 5 975K ellenőrzések. hello értékelést angol nyelven írt és hello 1997-2007 időszakra vonatkozóan. Ez az adatkészlet lett le mintát too10K értékelést.
  </td>
</tr>

<tr>
  <td valign=top>Mell kapcsolatos adatok</td>
  <td valign=top>
Az egyik hello Oncology Intézet, amelyek gyakran megjelenik a machine learning szakirodalomban által biztosított három kapcsolatos kapcsolatos adatkészletek. Diagnosztikai információ laboratóriumi elemzési szolgáltatás, körülbelül 300 darab egyesíti.<p> </p><b>Használat:</b> kapcsolatos hello típusú besorolását, 9-es attribútumok alapján, amelyeket lineáris és kategorikus. <p> </p><b>Kapcsolódó kutatási:</b> Wohlberg, W.H., utca, W.N. & Mangasarian, O.L. (1995). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Mell kapcsolatos szolgáltatások <td valign=top>
hello dataset 102K gyanús területek (jelöltek) röntgen-lemezképek, információkat tartalmaz, minden egyes leírt 117 funkcióihoz. hello szolgáltatások saját fejlesztésű, és azok jelentését ne derüljön ki a hello dataset creators (Siemens egészségügy) által. 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Mell kapcsolatos információ</td>
  <td valign=top>
hello dataset röntgen lemezkép minden gyanús területre további információkat tartalmaz. Minden egyes példa információkat nyújt (pl. címke, beteg koordináták javítás relatív toohello teljes lemezkép-azonosító) kapcsolatos hello hello mell kapcsolatos funkciók adatkészlet megfelelő sorszám. Minden egyes türelmet számos példát. Egy kapcsolatos rendelkező betegeknél néhány példa pozitív és negatív között. Betegek, akik nem rendelkeznek egy kapcsolatos, az összes többek között negatív. hello dataset adatkészletben 102K példák. hello dataset optimalizálva, 0,6 % hello pontok pozitív, hello rest negatív. hello dataset lett rendelkezésére Siemens egészségügy.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>Megosztott CRM Appetency címkék</td>
  <td valign=top>
Hello KDD Cup 2009 felhasználói kapcsolat előrejelzés challenge címkéi (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>Megosztott CRM-forgalom címkék</td>
  <td valign=top>
Hello KDD Cup 2009 felhasználói kapcsolat előrejelzés challenge címkéi (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>Megosztott CRM-adatkészlet</td>
  <td valign=top>
Ezek az adatok hello KDD Cup 2009 felhasználói kapcsolat előrejelzés challenge származik (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>az adatkészletben hello hello francia Telecom vállalati narancssárga 50K-ügyfelek. Minden egyes ügyfélnek van 230 anonimizált funkciók, amelyek 190 numerikus és 40 kategorikus. hello szolgáltatások akkor nagyon ritka.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>Megosztott CRM Upselling címkék</td>
  <td valign=top>
Hello KDD Cup 2009 felhasználói kapcsolat előrejelzés challenge címkéi (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energiát hatékonyságát regressziós adatok</td>
  <td valign=top>
Szimulált energia profilok, 12 különböző épület alakzatok alapján gyűjteménye. 8 szolgáltatásait, például az üveg terület, terület-felosztáshoz és a tájolás üveg hello szerint megkülönböztetett forgalomosztályból a hello épületek.<p> </p><b>Használat:</b> regressziós vagy a besorolási toopredict hello-energiahatékonyság alapján rendelkezésre álló két valós értékelni válaszok minősítés használja. A többszörös osztály besorolás van round hello válasz változó toohello a legközelebbi egész számra. <p> </p><b>Kapcsolódó kutatási:</b> Xifara, A. & Tsanas, azonosítójához. (2012). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Felé továbbított adatok késlelteti</td>
  <td valign=top>
Utas repülési időben hello TranStats adatok gyűjteménye, USA hello származó teljesítményadatokat Részleg a szállítására (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">időben</a>).<p> </p>hello dataset hello időszak 2013. április-október magában foglalja. A Machine Learning Studio tooAzure feltöltését, mielőtt hello dataset feldolgozott az alábbiak szerint:<ul><li>hello dataset lett szűrt toocover csak hello 70 legforgalmasabb repülőtereken hello szárazföldi USA</li><li>Több mint 15 perces késleltetett volt címkézett visszavont járatok</li><li>Elterelt járatok szűrhető volt.</li><li>hello következő oszlopok kiválasztott: év, hónap, DayofMonth, DayOfWeek, vivőjel, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, megszakítva</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Repülési időben teljesítmény (Raw)</td>
  <td valign=top>
Repülőgép repülési érkezők és a 2011. októberi az Amerikai Egyesült Államokból eltérést rögzíti.<p> </p><b>Használat:</b> repülési késések előre jelezni. <p> </p><b>Kapcsolódó kutatási:</b> a szállítására részleg USA <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Erdőtüzek adatai</td>
  <td valign=top>
Időjárás adatokat tartalmaz, például a hőmérséklet és a páratartalom indexek és szél sebessége, Északkelet-Portugáliában előfordult erdőtüzek rekordjának együtt területe.<p> </p><b>Használat:</b> Ez az regressziós nehéz feladat, ahol hello célja toopredict hello vonatkozó erdőtüzek területén. <p> </p><b>Kapcsolódó kutatási:</b> Cortez, p, & Morais, azonosítójához. (2008). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai <p> </p>[Cortez és Morais, 2007] P Cortez és A. Morais. Adatok adatbányászati megközelítés tooPredict erdőtüzek időjárási adatokkal. A J. Neves, M. f Santos és J. Machado Eds., új trendeket mesterséges intelligencia, az eljárás hello 13 EPIA 2007 - portugál konferencia mesterséges intelligens, December, Guimarães, Portugália, oldal 512-523, 2007. APPIA, ISBN-13 978-989-95618-0-9. Megtekinthető a következő helyen: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Német hitelkártya UCI adatkészlet</td>
  <td valign=top>
hello UCI Statlog (német hitelkártya) dataset (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + német + jóváírás + adatok</a>), hello german.data fájl használatával.<p> </p>hello dataset személyek, attribútumok, mint magas vagy alacsony hitelkockázatok szerint osztályozza. Minden egyes példa egy személy jelöli. Nem találhatók 20 funkciókat, mind a numerikus, és a kategorikus, és egy bináris címke (hello jóváírás kockázati érték). Hitelkockázati kockázat bejegyzések rendelkezik címke = 2, alacsony követel kockázat tételek rendelkezik címke = 1. egy alacsony kockázat példa annyira misclassifying hello költségének értéke 1, mivel a magas kockázatú példa legalacsonyabb misclassifying hello költségét 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB Movie címek</td>
  <td valign=top>
hello dataset információkat tartalmaz a Twitter Twitter-üzeneteket volt minősített filmek: IMDB movie azonosító, a movie nevét, a genre és a gyártási év. Nincsenek 17K filmek hello adatkészlet. hello dataset bemutatott hello papír "használatát. Dooms, T. De Pessemier és L. Martens-féle. MovieTweetings: Dataset minősítés film begyűjti a Twitteren. Workshop közösségi és emberi számítási ajánló rendszerek esetén: RecSys 2013 CrowdRec."
  </td>
</tr>

<tr>
  <td valign=top>A két Iris osztály adatokat</td>
  <td valign=top>
Ez akkor lehet, hogy hello hello mintát felismerés szakirodalomban található legjobb ismert adatbázis toobe. hello dataset viszonylag kicsi, 50 példák minden szirom mérések három iris fajtáinak tartalmazó.<p> </p><b>Használat:</b> hello iris típus hello mérésekből előre jelezni.  <p> </p><b>Kapcsolódó kutatási:</b> Fisher, R.A. (1988). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Film Twitter-üzenetek</td>
  <td valign=top>
hello dataset jelent hello Movie Tweetings adatkészlet. hello dataset adatkészletben filmek, jól strukturált Twitter-üzeneteket a Twitteren kinyert 170K minősítését. Minden példány jelöli egy tweetet, és egy rekord: felhasználói azonosító IMDB movie azonosító, minősítés, timestamp, számának Kedvencek tweetet, és a retweets a tweetet a száma. hello dataset lett által elérhetővé tett A. említett, S. Dooms a, b Loni és D. Tikk ajánló rendszerek Challenge 2014.
  </td>
</tr>

<tr>
  <td valign=top>Különböző autók fogyasztási adatai</td>
  <td valign=top>
Ez az adatkészlet hello StatLib könyvtárának Carnegie Mellon egyetemi által biztosított hello dataset kis mértékben módosított verziója telepítve. hello dataset használt hello 1983 American statisztikai társítás kézikönyve.<p> </p>hello adatok listája miles / gallonra, a különböző autók üzemanyag-fogyasztási jelszavat információkkal ilyen hengerszám, motor elmozdulását, lóerő, teljes súlya és gyorsítás hello száma.<p> </p><b>Használat:</b> üzemanyag-fogyasztási 3 többértékű diszkrét attribútumok és 5 folytonos attribútumok alapján előre jelezni. <p> </p><b>Kapcsolódó kutatási:</b> StatLib, Carnegie Mellon egyetemi, (1993). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr>
  <td valign=top>Pima indiai termelőktől cukorbetegség bináris osztályozási adatkészlet</td>
  <td valign=top>
Hello cukorbetegség National Institute és emésztőtraktus és vese betegségek adatbázisból származó adatok egy részét. hello dataset gyakorolt nőivarú Pima indiai örökségének szűrt toofocus volt. hello egészségügyi adatok, például glükóz és inulin szintek, valamint lifestyle tényezők szerepel.<p> </p><b>Használat:</b> előre jelezni, hogy van-e a hello tulajdonos cukorbetegség (bináris osztályozás). <p> </p><b>Kapcsolódó kutatási:</b> Sigillito, V. (1990). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai </td>
</tr>

<tr>
  <td valign=top>Éttermi ügyféladatok</td>
  <td valign=top>
Ügyfelek, beleértve a demográfiai és beállítások metaadatainak készlete.<p> </p><b>Használat:</b> használják ezt az adatkészletet, együtt hello más két éttermi adathalmazt tootrain, és tesztelje a ajánló rendszert. <p> </p><b>Kapcsolódó kutatási:</b> Bache, K. és Lichman, M. (2013). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, iskolai az adatokat és számítástechnikai.
  </td>
</tr>

<tr>
  <td valign=top>Éttermi szolgáltatás adatok</td>
  <td valign=top>
A metaadatok éttermekben és a hozzájuk tartozó funkciók áttekintése, például étele típusa, a étkezési stílus és a hely készlete.<p> </p><b>Használat:</b> használják ezt az adatkészletet, együtt hello más két éttermi adathalmazt tootrain, és tesztelje a ajánló rendszert. <p> </p><b>Kapcsolódó kutatási:</b> Bache, K. és Lichman, M. (2013). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, iskolai az adatokat és számítástechnikai.
  </td>
</tr>

<tr>
  <td valign=top>Éttermi minősítése</td>
  <td valign=top>
A 0 too2 méretű felhasználók toorestaurants által adott minősítések tartalmazza.<p> </p><b>Használat:</b> használják ezt az adatkészletet, együtt hello más két éttermi adathalmazt tootrain, és tesztelje a ajánló rendszert. <p> </p><b>Kapcsolódó kutatási:</b> Bache, K. és Lichman, M. (2013). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, iskolai az adatokat és számítástechnikai.
  </td>
</tr>

<tr>
  <td valign=top>Készült Annealing több osztály adatkészlet</td>
  <td valign=top>
Ez az adatkészlet olyan fizikai attribútumokat hello (width, vastagsága, típusa (gyújtótekercs, lap stb.) hello eredő készült típusok a próbaverzió primerek acélból bejegyzéseit sorozata.<p> </p><b>Használat:</b> bármely két numerikus osztályattribútumokhoz; keménység vagy erőssége előre jelezni. Előfordulhat, hogy is elemez korrelációk attribútumok között.<p> </p>Készült besorolási hajtsa végre egy szabvány, SAE és más szervezetekkel. Egy adott "osztály" (hello osztály változó) keres, és szeretné, hogy szükséges toounderstand hello értékeket. <p> </p><b>Kapcsolódó kutatási:</b> Sterling, d & Buntine, w (NÉV). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, a iskolai az adatokat és a számítástechnikai <p> </p>A hasznos útmutató toosteel besorolási itt található: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Teleszkóppal adatok</td>
  <td valign=top>
A magas energia gamma alkotóelem felszakadásáig együtt háttérzaj rekordjait, mindkettő szimulált Monte Carlo eljárással.<p> </p>hello szimuláció hello szándékával lett tooimprove hello pontosságát ground alapú légköri Cherenkov gamma teleszkópok, statisztikai módszerek toodifferentiate hello kívánt jel (Cherenkov sugárzási zuhanyozók) és (hadronic háttérzaj közötti használatával hello felső környezetben cosmic sugarak által kezdeményezett zuhanyozók).<p> </p>hello már Előfeldolgozott toocreate egy nyújtott alakú fürt hello hosszú tengely nincs objektumorientált hello kamera center felé. a három pont (gyakran nevezik Hillas paraméterek) hello jellemzői megkülönböztetés használható hello kép paraméterek közé tartoznak.<p> </p><b>Használat:</b> előre jelezni, hogy mulatni képe jelöl jel vagy a háttérben zaj.<p> </p><b>Megjegyzések:</b> egyszerű besorolás pontossága értéke nem értelmezhető, ezeket az adatokat, egy háttér esemény zárolásának, jel egy jel esemény háttereként zárolásának óta. Különböző osztályozó összehasonlítása hello ROC diagramhoz kell használni. háttér-esemény elfogadása jel kell lennie a következő küszöbértékek hello valamelyik valószínűségét hello: 0,01, 0,02, 0,05, 0,1 vagy 0,2.<p> </p>Azt is vegye figyelembe, hogy a háttér-események (hadronic zuhanyozók h) hello száma kellőképpen van, mivel valós mérések hello h vagy zaj osztály események hello többsége jelöli. <p> </p><b>Kapcsolódó kutatási:</b> Bock, R.K. (1995). UCI gépi tanulási a tárház <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Egyetemi a kaliforniai, iskolai vonatkozó információk </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Időjárás-adatkészlet</td>
  <td valign=top>
Óránkénti szárazföldi időjárási észrevételt NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 too201310">201304 too201310 származó adatok egyesítésekor</a>).<p> </p>hello időjárási adatok repülőtéri időjárási esetében kiterjedő hello időszak 2013. április-október észrevételeket ismerteti. A Machine Learning Studio tooAzure feltöltését, mielőtt hello dataset feldolgozott az alábbiak szerint:<ul><li>Időjárás állomás azonosítójuk csatlakoztatott toocorresponding repülőtéri azonosítók</li><li>Időjárás állomások nem társított hello 70 legforgalmasabb repülőtéren szűrhető volt.</li><li>hello dátumoszlop volt ossza fel külön év, hónap és nap oszlopok</li><li>hello következő oszlopok kiválasztott: AirportID, év, hónap, nap, idő, időzóna, SkyCondition, látható, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius RelativeHumidity, Szélsebesség, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, magasságmérő</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 adatkészlet</td>
  <td valign=top>
Adatok Wikipedia származik (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) cikkeket S & P 500 vállalatok, tárolt XML-adatok alapján.<p> </p>A Machine Learning Studio tooAzure feltöltését, mielőtt hello dataset feldolgozott az alábbiak szerint:<ul><li>Bontsa ki a szöveges tartalom minden adott vállalat esetében</li><li>Wiki formázás eltávolítása</li><li>Távolítsa el a nem alfanumerikus karaktereket</li><li>Alakítsa át az összes szöveg toolowercase</li><li>Ismert vállalati kategóriák lettek hozzáadva.</li></ul><p> </p>Vegye figyelembe, hogy az egyes vállalatok egy cikk nem található, így hello rekordok száma kisebb, mint az 500-as.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
felhasználói adatok és a válasz tooa közvetlen levelezési kampány kapcsolatos hello adatkészletben. Minden egyes sorára ügyfél jelöli. hello az adatkészletben 9 szolgáltatás kapcsolatos felhasználói demográfiai és a múltbeli és a 3 címke oszlop (fel, az átalakítás és töltött).  Keresse fel a bináris oszlop, amely azt jelzi, hogy az ügyfél hello marketingkampányt, átalakítás azt jelzi, hogy az ügyfél vásárolt, valamint az töltött meglátogatott telt hello összeg.  hello dataset lett rendelkezésére Kevin Hillstrom a MineThatData E-Mail elemzés és a Data Mining kérésekor.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Tesztelési példák hello RCV1-V2 Reuters hírek adatkészlet tulajdonságai. hello dataset adatkészletben 781K hírcikkeket azonosítók együtt (első oszlopa hello adatkészlet). Minden cikk tokenekre bontott stopworded, és kocsány. hello dataset David által történt érhető el. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Példák hello RCV1-V2 Reuters hírek adatkészlet tulajdonságai. hello dataset adatkészletben 23K hírcikkeket azonosítók együtt (első oszlopa hello adatkészlet). Minden cikk tokenekre bontott stopworded, és kocsány. hello dataset David által történt érhető el. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Hello KDD Cup 1999 Tudásbázis felderítési adatkészlet és a Data Mining eszközök verseny (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>hello dataset letöltötte és az Azure Blob storage szolgáltatásban tárolt (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) és a képzési és a teszteléshez adatkészletek is tartalmaz. hello képzési dataset 126K sorok és 43 oszlopok, beleértve a hello címkék körülbelül rendelkezik. Három oszlopot hello címke adatai, és a numerikus és a karakterlánc kategorikus szolgáltatások, amely 40 oszlopok hello modell betanítása érhető el. hello Tesztadatok körülbelül 22.5K tesztelése példák mint hello betanítási adatok hello azonos 43 oszlopokkal rendelkezik.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td valign=top>
A témakör a hozzárendeléseinek hello RCV1-V2 Reuters hírek adatkészlet írt hírcikkeket. Egy hírek cikk tooseveral témakörök rendelhetők hozzá. minden egyes sorára formátum hello "&lt;témakör&gt; &lt;dokumentumazonosítója&gt; 1". hello dataset 2.6-os M témakör hozzárendeléseket tartalmazza. hello dataset David által történt érhető el. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Ezek az adatok hello KDD Cup 2010 Student teljesítmény értékelési challenge származik (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">student teljesítmény értékelési</a>). hello használt adatok hello Algebra_2008_2009 gyakorlókészlethez (Stamper, J., Niculescu-Mizil, A., Ritter, S. Gordon, G.J. & Koedinger, k. r. (2010). Algebra I 2008-2009. KDD Cup 2010 oktatási Data Mining ellenőrző kérdés adatkészlet. A Keresés <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> vagy <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>hello dataset letöltötte és az Azure Blob storage szolgáltatásban tárolt (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) és a rendszer-oktatás student naplófájlokat tartalmazza. megadott hello szolgáltatások közé tartozik a probléma azonosítója és rövid leírása, student azonosítója, timestamp, és hány megpróbál előtt hello hello probléma megoldásához hello student jobb gombbal a módját. hello eredeti dataset adatkészletben 8.9M rekordok; Ez az adatkészlet lett le mintát toohello első 100K sorokat. hello dataset adatkészletben különböző típusú 23 tabulátorral tagolt oszlopok: numerikus, kategorikus, és a Timestamp típusú.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
