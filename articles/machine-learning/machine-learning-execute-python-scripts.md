---
title: "aaaExecute Python gépi tanulási parancsfájlok |} Microsoft Docs"
description: "Vázol fel tervezési alapelvek az alapul szolgáló Azure Machine Learning és alapvető használati forgatókönyvek, képességekre és korlátozásokra Python parancsfájlok támogatása."
keywords: "Python gépi tanulási, pandas, a python pandas, a python-parancsfájlok, python-parancsfájl végrehajtása"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bradsev
ms.openlocfilehash: 8d23aaa972a46cb1a07ea0f18cc1e24933fe3e6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>A Python Machine Learning parancsfájlok végrehajtása az Azure Machine Learning Studióban

Ez a témakör ismerteti az alapul szolgáló hello aktuális támogatja a Python-parancsfájl az Azure Machine Learning hello tervezési alapelvek. hello fő képesség azt is, beleértve:

- a végrehajtás alapvető használati forgatókönyvek
- a kísérlet pontozása egy webszolgáltatás
- importálja a meglévő kód támogatása
- képi megjelenítések exportálása
- hajtsa végre a felügyelt szolgáltatás kiválasztása
- bizonyos korlátozások megértése

[Python](https://www.python.org/) a hello eszköz mellkasát sok adatszakértőkön elengedhetetlen eszköz. Rendelkezik:

* egy elegáns és tömör szintaxis 
* többplatformos támogatást, 
* hatékony szalagtárak hatalmas és 
* érett Fejlesztőeszközök. 

Python használatban van a machine learning modellezési jellemzően használt munkafolyamat minden egyes szakaszába:

- adatok betöltési és feldolgozása 
- szolgáltatás létrehozása
- modell betanítási 
- Modellellenőrzés
- hello modellek központi telepítése

Azure Machine Learning Studio támogatja a beágyazási Python-parancsfájlok tanulási kísérlet és zökkenőmentesen is közzé őket a Microsoft Azure webszolgáltatásként gépek különböző részre.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Python gépi tanulási parancsfájlok tervezési alapelvei

az Azure Machine Learning Studióban hello elsődleges kapcsolódási felületet tooPython hello keresztül van [Python-parancsfájl végrehajtására] [ execute-python-script] modul 1. ábrán látható.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

1. ábra. Hello **Python-parancsfájl végrehajtására** modul.

Hello [Python-parancsfájl végrehajtására] [ execute-python-script] modul az Azure ML Studio mentése toothree bemeneti adatokat fogad, és hozza be tootwo kimenetek (hello a következő szakaszban ismertetett), például az R analóg hello [ R-parancsfájl végrehajtása] [ execute-r-script] modul. hello Python kódját toobe végre bekerül a hello paraméter mezőben kifejezetten nevű belépési pont hívott függvény `azureml_main`. Az alábbiakban hello tervezési alapelvek használt tooimplement ezzel a modullal:

1. *A Python felhasználók idiomatikus kell lennie.* A Python-felhasználók kódjukat funkciók modulok belül, figyelembe. Ezért helyezése egy legfelső szintű modul végrehajtható kimutatások számos olyan viszonylag ritkán fordul elő. Ennek eredményeképpen hello parancsfájl mezőben is fogadja egy speciális Python függvény adatként megakadályozását toojust utasítások sorozata. hello objektum hello függvény felfedett Python könyvtár alaptípusok például [Pandas](http://pandas.pydata.org/) adatkeretek és [NumPy](http://www.numpy.org/) tömbök.
2. *Valósághű helyi között kell lennie, és a felhő végrehajtások.* hello használt háttér tooexecute hello Python kódját alapuló [Anaconda](https://store.continuum.io/cshop/anaconda/), a platformok közötti tudományos Python elosztási széles körben használt. Bezárás too200 hello leggyakoribb Python-csomagokat, a mellékelt. Ezért az adatelemzők debug és a helyi Azure Machine Learning-kompatibilis Anaconda-környezetre kódjukat minősítéséhez. Majd használjon például egy meglévő fejlesztőkörnyezet [IPython](http://ipython.org/) notebook vagy [a Python Tools for Visual Studio](http://aka.ms/ptvs), toorun Azure ML kísérlet részeként. Hello `azureml_main` belépési pont, a Python eredeti függvény, ezért x Azure ML-specifikus kód nélkül hozhatóak létre vagy hello SDK telepítve.
3. *A többi Azure Machine Learning modulok zökkenőmentesen összeállítható kell lennie.* Hello [Python-parancsfájl végrehajtására] [ execute-python-script] modul fogad el, bemenetekhez és kimenetekhez, szabványos Azure Machine Learning adatkészletek. hello alapul szolgáló keretrendszer átlátható és hatékonyan kulcsösszetevő hello Azure ML és Python futtatókörnyezetek. Ezért a Python meglévő Azure ML munkafolyamathoz, beleértve azokat is, hívja az R és SQLite együtt használható. Eredményeként adatok tudósok munkafolyamatok állítható össze, amelyek:
   * Python és Pandas használja az adatok előzetes feldolgozás és tisztítás
   * adatcsatorna hello adatok tooa SQL átalakítása, több adatkészletek tooform funkció csatlakoztatása
   * hello algoritmusok használata az Azure Machine Learning modellek betanítása 
   * értékelje ki és utófeldolgozási hello eredményeket R.


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>Python parancsfájlok ml alapvető használati forgatókönyvek

Ez a szakasz azt megtekintheti, néhány alapvető használatát hello hello [Python-parancsfájl végrehajtására] [ execute-python-script] modul. Bemenetek toohello Python modul mint Pandas adatkeretek érhetők el. hello függvénynek kell adnia egy Python belül csomagolt egyetlen Pandas adatok keret [feladatütemezési](https://docs.python.org/2/c-api/sequence.html) például egy rekord, lista vagy NumPy tömb. Ebben a sorozatban hello első elemének ezután visszaérkezik az első kimeneti portjára hello hello modul. Ez a rendszer a 2. ábrán látható.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

2. ábra Leképezési bemeneti portok tooparameters és a visszatérési érték toooutput port.

Hogyan hello bemeneti portok működtetni a hello csatlakoztatott tooparameters szemantikáját részletes `azureml_main` függvény az 1 láthatók:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

1. táblázat. A bemeneti portok toofunction paraméterek leképezését.

hello bemeneti portok és függvényparamétereket között lesz pozicionális:

- hello első csatlakoztatott bemeneti porthoz csatlakoztatott toohello hello függvény első paramétere. 
- második bemenet hello (Ha csatlakozott) csatlakoztatott toohello hello függvény második paramétere.

Lásd: *adatelemzéshez Python* (O'Reilly, 2012) további információt a Python Pandas és hogyan lehet használt toomanipulate adatok hatékonyabb és gazdaságosabb Nyugat McKinney által. 


## <a name="translation-of-input-and-output-types"></a>Bemeneti és kimeneti típusú fordítás 
Azure ml bemeneti adatkészletek Pandas konvertált toodata keretek. Kimeneti adatkeretek hátsó tooAzure ML adatkészletek lesznek átalakítva. hello átalakítás esetén a következő történik:

1. Konvertálja a karakterláncot, és a numerikus oszlopok-van, és egy adatkészletet a hiányzó értékeket konvertált too'NA "Pandas szereplő értékek. hello azonos átalakítás történik, az hello módon vissza (NA Pandas értékei Azure ml konvertált toomissing értékek).
2. Index vektorok Pandas az Azure ml nem támogatottak. Az összes bemeneti adatkeretek hello Python függvény mindig rendelkezik egy 64 bites mínusz 1 sorok száma 0 toohello a numerikus index. 
3. Az Azure ML adatkészletek nem lehet ismétlődő oszlopneveket tartalmaz, az oszlopnevek, amelyek nem karakterlánc. Ha egy kimeneti adatok keret nem numerikus oszlopot tartalmaz, hello keretrendszer hívások `str` a hello oszlopneveket. Hasonlóképpen, minden ismétlődő oszlopnevek-e automatikusan összekeveredett tooinsure hello nevek egyediek. hello (2) utótagot toohello első ismétlődő, (3) toohello második duplikált, és így tovább.


## <a name="operationalizing-python-scripts"></a>Python parancsfájlok végrehajtott

Bármely [Python-parancsfájl végrehajtására] [ execute-python-script] pontozási kísérletben használt modulok webszolgáltatásként közzétételekor nevezzük. 3. ábrán például egy pontozási kísérletet, amely tartalmazza a hello kód tooevaluate egyetlen Python kifejezés látható. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

3. ábra. Webszolgáltatás egy Python-kifejezés kiértékelésekor.

Ez a kísérlet létrehozása webszolgáltatás:

- a Python kifejezés (karakterláncként) bemenetként vesz igénybe
- elküldi toohello Python parancsértelmező 
- hello kifejezés mind kiértékelése hello eredmény tartalmazó táblát adja vissza.


## <a name="importing-existing-python-script-modules"></a>Meglévő Python-parancsfájl modulok importálása

Egy gyakori használati eset sok adatszakértőkön az Azure ML kísérletek tooincorporate meglévő Python parancsfájlhoz. Ahelyett, hogy az összes kód összefűzendő és illeszthetők be egy parancsfájl dobozban, hello [Python-parancsfájl végrehajtására] [ execute-python-script] modul fogad hello harmadik bemeneti portját a Python-modulok tartalmazó zip-fájl. hello fájl van unzipped hello végrehajtási keretrendszer futásidőben, és hello tartalma toohello könyvtár elérési útja hello Python parancsértelmező kerülnek. Hello `azureml_main` függvény importálhatja ezeket a modulokat közvetlenül a belépési pont.

Tegyük fel fontolja meg egy egyszerű "Hello, World" függvényt tartalmazó Hello.py hello fájl.

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

4. ábra. Felhasználó által definiált függvény Hello.py fájlban.

A következő létrehozhatunk Hello.zip Hello.py tartalmazó fájlt:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

5. ábra. Felhasználó által definiált Python kódot tartalmazó zip-fájl.

Töltse fel hello zip-fájl adatkészletként Azure Machine Learning Studio. Ezután hozzon létre és hello Python kódot használó hello Hello.zip fájlban toohello harmadik bemeneti portját a hello csatolásával a kísérlet futtatásához **Python-parancsfájl végrehajtására** modul, az ábrán látható módon.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

6. ábra. Felhasználó által definiált Python kóddal mintakísérletet feltöltött csomagot .zip fájlként.

hello modul kimeneti jeleníti meg, hogy hello zip-fájl már csomagolatlan és hello funkcióval `print_hello` futott.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)

7. ábra. Felhasználó által definiált függvény hello belül használt [Python-parancsfájl végrehajtására] [ execute-python-script] modul.


## <a name="working-with-visualizations"></a>Képi megjelenítés használata

Létrehozott, amely képes lehet ábrázolt MatplotLib hello böngésző előkészítésére hello által visszaadható [Python-parancsfájl végrehajtására][execute-python-script]. A teljesség hello előkészítésére automatikusan átirányított tooimages, R. használatakor Így hello felhasználói explicit módon menteni kell bármely előkészítésére tooPNG fájlokat, ha vannak toobe hátsó tooAzure Machine Learning adott vissza. 

MatplotLib toogenerate képek, végre kell hajtania a következő eljárás hello:

* Váltás hello háttér túl "AGG" hello az alapértelmezett Qt-alapú megjelenítő 
* Hozzon létre egy új. ábra-objektumot 
* hello tengely első és bele felvételt minden készítése 
* hello. ábra tooa PNG-fájl mentése 

Ezt a folyamatot mutatja be, amely létrehoz egy pont rajzot mátrix hello scatter_matrix funkcióval Pandas a 8. ábra a következő hello.

![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

8. ábra. Kód MatplotLib adatok tooimages toosave.

9. ábra mutatja a kísérlet, amely előzőleg bemutatott tooreturn tevékenységtérkép hello második kimenő porton keresztül hello parancsfájlt használ.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

9. ábra. Python kódból generált előkészítésére megjelenítése.

Lehetséges tooreturn több adatok kijelzőként különböző képek, hello Azure Machine Learning futásidejű felveszi az összes képek és fűzi össze azokat a képi megjelenítéshez tartozó.


## <a name="advanced-examples"></a>Speciális példák

az Azure Machine Learning telepített hello Anaconda környezete például NumPy SciPy vagy Scikits további közös csomagot tartalmaz. Ezeket a csomagokat a machine learning-feldolgozási folyamat különböző adatfeldolgozási feladatok hatékony használható. Tegyük fel, hello következő kísérlet és parancsprogram szemlélteti dataset hello ensemble tanulókkal toocompute szolgáltatás fontosság pontszámok Scikits ismerje meg a használatát. hello pontszámok felügyelt használt tooperform szolgáltatás kiválasztása előtt egy másik ML modellbe táplált folyamatban lehet.

Itt pedig a hello Python használt függvény toocompute hello fontosság pontszámokat rendelés hello szolgáltatások hello pontszámok alapján:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

10. ábra. Pontszámok függvény toorank funkcióihoz.
 hello következő kísérlet majd kiszámítja és hello fontosság pontszámok funkciók hello "Pima indiai cukorbetegség" dataset az Azure Machine Learning adja vissza:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    

11. ábra. Kísérlet toorank szolgáltatások hello Pima indiai cukorbetegség adatkészletben.

## <a name="limitations"></a>Korlátozások
Hello [Python-parancsfájl végrehajtására] [ execute-python-script] van hello a következő korlátozások vonatkoznak:

1. *Elkülönített végrehajtása.* hello Python-futtatókörnyezet jelenleg védőfal mögött, és emiatt nem engedélyezi a hozzáférést toohello hálózati vagy toohello helyi fájlrendszer tartósan. Minden fájl mentése helyileg elkülönített és törölni hello modul befejeződése után. hello kivétel alatt hello az aktuális könyvtárban és annak alkönyvtáraiban, Python kódját hello nem férhet hozzá a legtöbb könyvtárak hello fut a gépen.
2. *Kifinomult fejlesztési és a hibaelhárítási támogatás hiánya.* Python modul hello jelenleg nem támogatja IDE-szolgáltatásokkal, például az intellisense és hibakeresés folyamatát. Hello modul futási időben nem sikerül, ha hello teljes Python Veremkivonat is elérhető. De a hello kimeneti naplót hello modul lehet megtekinteni. Jelenleg javasoljuk, hogy Ön által fejlesztett és egy környezetben, például IPython Python-parancsfájlok hibakeresése és hello kód majd importálnia hello modul.
3. *Keret kimeneti egyetlen adatokat.* hello Python belépési pont csak engedélyezett tooreturn egyetlen keret output típusúként. Tetszőleges Python objektumok, például a betanított modellek közvetlenül biztonsági toohello Azure Machine Learning futásidejű jelenleg lehetséges tooreturn nincs. Például [R-parancsfájl végrehajtása][execute-r-script], amelynek van hello ugyanez a helyzet, akkor lehet sok esetben egy olyan bájtot toopickle objektumok tömb, és térjen vissza, amely adatok kereten belül.
4. *Nem tudja toocustomize Python telepítési*. Hello csak úgy tooadd egyéni Python modult jelenleg a korábban ismertetett hello zip-fájl mechanizmus révén. Ez nem valósítható meg, hogy kis modulok, akkor nagy modulok (különösen a natív DLL-ek) és a modulok nagy számú nehézkes. 

## <a name="conclusions"></a>Következtetéseket
Hello [Python-parancsfájl végrehajtására] [ execute-python-script] modul lehetővé teszi, hogy egy adatok tudósok tooincorporate meglévő Python a felhőben üzemeltetett machine learning munkafolyamatok az Azure Machine Learning és tooseamlessly azok a webes szolgáltatás részeként. hello Python-parancsfájl modul természetes együttműködik az Azure Machine Learning más modulok. a kutatási funkciójával toopre adatfeldolgozási és a szolgáltatás kivonása, majd tooevaluation feladatok számos és utófeldolgozás hello eredmények hello modul használható. hello háttér futásidejű végrehajtási használt Anaconda, tesztelt és széles körben használt Python elosztási alapul. A háttérrendszer egyszerűen meg tooon-tábla meglévő kódot eszközök hello felhőbe.

További funkciók toohello tooprovide várhatóan [Python-parancsfájl végrehajtására] [ execute-python-script] modul például képes tootrain hello, és azok a Python modellek és tooadd jobb hello támogatása fejlesztési és az Azure Machine Learning Studióban kód hibakereséséhez.

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [Python fejlesztői központ](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
