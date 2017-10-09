---
title: "a Machine Learning Python ügyféloldali kódtár aaaAccess adatkészletek |} Microsoft Docs"
description: "Telepítési és hello Python ügyfél könyvtár tooaccess használja, és Azure Machine Learning adatok biztonságos kezelésére a helyi Python-környezetben."
services: machine-learning
documentationcenter: python
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: huvalo;bradsev
ms.openlocfilehash: f55067118f13c52bf677930a20836ce6989f8187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-datasets-with-python-using-hello-azure-machine-learning-python-client-library"></a>Hozzáférés adatkészletek Python hello Azure Machine Learning Python ügyféloldali kódtár használata
hello preview Microsoft Azure Machine Learning Python ügyféloldali kódtár engedélyezheti a biztonságos hozzáférést tooyour helyi Python-környezetben az Azure Machine Learning adatkészletek, és lehetővé teszi, hogy a hello létrehozását és kezelését egy munkaterület adathalmazok.

Ez a témakör a kapcsolatos utasításokat tartalmazza:

* hello Machine Learning Python ügyféloldali kódtár telepítése 
* hozzáférés, és töltse fel az adatkészleteket, beleértve a útmutatást tooget engedélyezési tooaccess a helyi Python-környezet az Azure Machine Learning adatkészletek
* köztes adatkészletek elérje kísérletek
* hello Python ügyfél könyvtár tooenumerate adatkészletek használata, metaadatok elérése, olvassa el a DataSet adatkészlet hello tartalmát, hozzon létre új adatkészletek és meglévő adatkészletek módosítása

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Előfeltételek
hello Python ügyféloldali kódtár tesztelték hello környezetben a következő alapján:

* Windows, Mac és Linux
* Python 2.7, 3.3 és 3.4

A következő csomagok hello függőség van:

* kérések
* Python-dateutil
* pandas

Javasoljuk, használjon, mint a rendelkező Python elosztási [Anaconda](http://continuum.io/downloads#all) vagy [lombkoronaszint](https://store.enthought.com/downloads/), amelyek származnak, Python, IPython és a fenti három hello csomagot telepítve. Bár IPython nem feltétlenül szükséges, kezelésére és adatok interaktív megjelenítése egy nagyszerű környezetet is.

### <a name="installation"></a>Hogyan tooinstall hello Azure Machine Learning Python ügyféloldali kódtár
hello Azure Machine Learning Python ügyféloldali kódtár kell telepített toocomplete hello feladatok az ebben a témakörben ismertetett módon. Elérhető a hello [Python-Csomagindexet](https://pypi.python.org/pypi/azureml). tooinstall a Python-környezetben, akkor futtassa hello parancsot a helyi Python-környezetben a következő:

    pip install azureml

Azt is megteheti, töltse le és telepítse az hello forrásból származó [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Ha git a gépen telepítve van, a pip tooinstall közvetlenül a git-tárház hello is használhatja:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Studio Code kódtöredékek tooaccess adatkészletek használata
hello Python ügyféloldali kódtár lehetővé teszi az programozott hozzáférés tooyour meglévő adatkészletek az adott időpontig futtatott kísérletek.

Hello Studio webes felhasználói felületen keresztüli kódrészletek, amelyek közé tartozik az összes hello szükséges információkat toodownload és adatkészletek deszerializálni a hely számítógépén Pandas DataFrame objektumként is létrehozhat.

### <a name="security"></a>Az adatelérési biztonsági
hello Python ügyféloldali kódtár használatára is tartalmaz, a munkaterület azonosítója és engedélyezési Studio által előírt kódtöredékek hello token. Ezek adja meg a teljes körű hozzáférési tooyour munkaterület, és védeni kell, például a jelszót.

Biztonsági okokból hello kód részlet funkció csak érhető el, amelyek rendelkeznek a szerepkörük állítja be toousers **tulajdonos** hello munkaterülethez. A szerepkör hello megjelenik az Azure Machine Learning Studióban **felhasználók** lapon az **beállítások**.

![Biztonság][security]

Ha nincs beállítva az a szerepkör **tulajdonos**, vagy kérheti tulajdonos következőre toobe, vagy kérje meg a hello munkaterület tooprovide hello tulajdonosának hello kódrészlettel meg.

tooobtain hello engedélyezési jogkivonat teheti hello a következők egyikét:

* Kérje meg a jogkivonat a tulajdonosa. Tulajdonosok érhetik el a hitelesítési tokenek saját munkaterület Studio hello-beállítások lapon. Válassza ki **beállítások** hello bal oldali ablaktáblán, és kattintson a **engedélyezési JOGKIVONATOK** toosee hello elsődleges és másodlagos jogkivonatokat.  Bár a hello elsődleges vagy másodlagos engedélyezési jogkivonatok hello használható hello kódrészletet, azt javasoljuk, hogy a tulajdonosok csak közös hello másodlagos engedélyezési jogkivonatokat.

![Engedélyezési jogkivonatok](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* Kérje meg a tulajdonosa előléptetni toobe toorole.  toodo ezt, a jelenlegi tulajdonos hello munkaterület igények toofirst távolítsa el, hello munkaterületen, majd újra meghívhatjuk tooit tulajdonos.

A fejlesztők beszerezte hello munkaterület azonosítója és az engedélyezés után token,-e képes tooaccess hello munkaterület hello kódrészletet, függetlenül azok szerepkör használatával.

Engedélyezési jogkivonatok objektumainak kezelése az hello **engedélyezési JOGKIVONATOK** lapon az **beállítások**. Újragenerálás őket, de ez az eljárás jogkivonat toohello előző visszavonja.

### <a name="accessingDatasets"></a>Hozzáférés adatkészletek helyi Python-alkalmazás
1. A Machine Learning Studióban, kattintson a **ADATKÉSZLETEK** hello bal oldali navigációs sávján hello.
2. Válassza ki azt szeretné, hogy tooaccess hello adatkészlet. Hello adatkészletek bármelyikét kiválaszthatja hello **saját ADATKÉSZLETEK** lista vagy a hello **minták** listája.
3. Kattintson az alsó eszköztár hello, **adatok hozzáférési kód generálása**. Ha hello adatok formátuma nem kompatibilis a hello Python ügyféloldali kódtár, ez a gomb le van tiltva.
   
    ![Adathalmazok][datasets]
4. A hello megjelenő ablakban válassza ki a hello kódrészletet, és tooyour vágólapra másolásához.
   
    ![Hozzáférési kód][dataset-access-code]
5. Hello kód beillesztése hello notebook a helyi Python-alkalmazás.
   
    ![Notebook][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>A Machine Learning kísérleteket a hozzáférés köztes adatkészletek
Egy kísérletben a Machine Learning Studio hello futtatása után is lehetséges tooaccess hello köztes adatkészletek modulok hello kimeneti csomópontjából. Köztes adatkészletek olyan adatok, amelyek a létrehozott és használt köztes lépések egy modell eszköz futtatásakor.

Mindaddig, amíg hello adatformátum összeegyeztethető hello Python ügyféloldali kódtára a köztes adatkészletek érhető el.

hello következő formátum támogatott (Ezek állandók szerepelnek hello `azureml.DataTypeIds` osztály):

* Egyszerű szöveg
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Hello formátum azt is meghatározhatja, hogy egy modul kimeneti csomópont fölött. Hello csomópontnév eszközleírásként együtt jelenik meg.

Néhány hello modulok, például hello [vegyes] [ split] modul, kimeneti tooa formátum nevű `Dataset`, hello Python ügyféloldali kódtár által nem támogatott.

![A DataSet formátumban][dataset-format]

Toouse átalakítás modul, például a kell [tooCSV átalakítása][convert-to-csv], tooget egy kimeneti egy támogatott formátumra.

![GenericCSV formátum][csv-format]

hello következő lépések bemutatják egy példa, amely létrehozza a kísérlet, futtatja, és hello köztes dataset hozzáfér.

1. Hozzon létre egy új kísérlet.
2. Helyezze be egy **bináris osztályozási felnőtt nyilvántartásba bevétel dataset** modul.
3. Helyezze be egy [vegyes] [ split] modul, és csatlakoztassa a bemeneti toohello dataset modul kimenetét.
4. Helyezze be egy [tooCSV átalakítása] [ convert-to-csv] modul, és csatlakoztassa a bemeneti tooone hello a [vegyes] [ split] modul kimenete.
5. Hello kísérlet mentse, majd futtassa, és várjon, amíg az futtató toofinish.
6. Hello kimeneti csomópontot a hello [tooCSV átalakítása] [ convert-to-csv] modul.
7. Amikor hello helyi menü megjelenik, válassza ki a **adatok hozzáférési kód generálása**.
   
    ![A helyi menü][experiment]
8. Válassza ki a hello kódrészletet, és a megjelenő ablakban hello tooyour vágólapra másolja.
   
    ![Hozzáférési kód][intermediate-dataset-access-code]
9. Illessze be a notebook hello kódot.
   
    ![Notebook][ipython-intermediate-dataset]
10. Hello adatok matplotlib használatával jelenítheti meg. Ez a hisztogram hello kor oszlop jelenik meg:
    
    ![Hisztogram][ipython-histogram]

## <a name="clientApis"></a>Hello Machine Learning Python ügyfél könyvtár tooaccess használja, olvasását, létrehozását és adatkészletek kezelése
### <a name="workspace"></a>Munkaterület
hello munkaterület hello belépési pont hello Python ügyféloldali kódtára a rendszer. Adja meg a hello `Workspace` a munkaterület azonosítója és engedélyezési jogkivonat toocreate az osztály egy példányát:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Adatkészletek számbavétele
tooenumerate egy adott munkaterület minden adathalmazok:

    for ds in ws.datasets:
        print(ds.name)

tooenumerate csak hello felhasználó által létrehozott adatkészletek:

    for ds in ws.user_datasets:
        print(ds.name)

tooenumerate csak hello példa adatkészletek:

    for ds in ws.example_datasets:
        print(ds.name)

A DataSet adatkészlet neve (kis-és nagybetűket) érhető el:

    ds = ws.datasets['my dataset name']

Vagy hozzá tud férni az index szerinti:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metaadatok
Adatkészletek metaadatok, továbbá toocontent rendelkezik. (Köztes adatkészletek toothis kivételi szabályt, és minden metaadatot.)

Néhány metaadatok érték hello felhasználói rendeli hozzá a létrehozás időpontjában:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Azure ML által hozzárendelt értékek:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Lásd: hello `SourceDataset` osztály több on hello elérhető metaadatok.

### <a name="read-contents"></a>Tartalmának olvasása
a Machine Learning Studio által biztosított automatikusan hello kódtöredékek töltse le és hello dataset tooa Pandas DataFrame objektum deszerializálása. Ez a lépés hello `to_dataframe` módszert:

    frame = ds.to_dataframe()

Ha inkább toodownload hello nyers adatokat, és hajtsa végre hello deszerializálás, saját magának, ez egy lehetőséget. Hello jelenleg ez a lehetőség hello csak formátum például "ARFF", mely hello Python ügyféloldali kódtár nem deszerializálható.

szövegként tooread a hello tartalma:

    text_data = ds.read_as_text()

bináris tooread a hello tartalma:

    binary_data = ds.read_as_binary()

Egy adatfolyam toohello tartalmát is ugyanúgy nyithatja meg:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Hozzon létre egy új adatkészlet
hello Python ügyféloldali kódtár lehetővé teszi a Python programból tooupload adatkészletek. Ezek az adatkészletek állnak majd a munkaterületen.

Ha az adatok egy Pandas DataFrame van, használja a hello a következő kódot:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Ha az adatok már tartozik, akkor használhatja:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

hello Python ügyféloldali kódtár egy Pandas DataFrame toohello következő formátumokat tudja tooserialize (Ezek állandók szerepelnek hello `azureml.DataTypeIds` osztály):

* Egyszerű szöveg
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Egy meglévő adatkészlet frissítése
Ha egy meglévő adatkészlet egyező nevű új adatkészlet tooupload, egy ütközés hiba szerezheti be.

tooupdate egyik meglévő adatkészletét, először kell tooget hivatkozás toohello meglévő adatkészlet:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Ezután `update_from_dataframe` hello dataset Azure tooserialize és csere hello tartalma:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Ha azt szeretné, hogy tooserialize hello tooa különböző adatformátum, adja meg a választható hello értékét `data_type_id` paraméter.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

Meg nem kötelezően leírást adhat meg új hello értékének megadásával `description` paraméter.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toofeb 2015'

Opcionálisan megadhatja egy új nevet hello értékének megadásával `name` paraméter. Ettől kezdve a be fogja olvasni hello adatkészlet az új néven hello. a következő kód hello frissíti hello adatok, nevét és leírását.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up toofeb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

Hello `data_type_id`, `name` és `description` paraméterek megadása nem kötelező, és az alapértelmezett tootheir előző érték. Hello `dataframe` paraméter megadása mindig kötelező.

Ha az adatok már szerializált, `update_from_raw_data` helyett `update_from_dataframe`. Ha a átadni `raw_data` helyett `dataframe`, hasonló módon működik.

<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

