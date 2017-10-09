---
title: "aaaPython Flask webalkalmazásokra vonatkozó oktatóanyag az Azure Cosmos DB |} Microsoft Docs"
description: "Tekintse át egy adatbázis-oktatóanyag az Azure-platformon futó Python Flask-webalkalmazások toostore és a hozzáférési adatok Azure Cosmos DB használatával. Alkalmazásfejlesztési megoldások keresése."
keywords: "Alkalmazásfejlesztés, python flask, python-webalkalmazás, python-webfejlesztés"
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/09/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87b73c656ed96a7efbd162843a1529d435f027f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Python Flask-webalkalmazás létrehozása az Azure Cosmos DB használatával
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Az oktatóanyag bemutatja, hogyan toouse Azure Cosmos DB toostore és a hozzáférési adatok egy Python webes alkalmazás Azure-platformon futó, és feltételezi, hogy rendelkezik némi tapasztalattal a Python és az Azure-webhelyek használatát.

Az adatbázis-oktatóanyag az alábbiakat ismerteti:

1. Létrehozása, és üzembe helyezése egy Cosmos-DB-fiókot.
2. A Python Flask-alkalmazás létrehozása.
3. Csatlakozás tooand Cosmos DB használatával a webalkalmazásból.
4. Hello webes alkalmazás tooAzure telepítését.

Az oktatóanyag utasításait követve egy egyszerű szavazóalkalmazást, amely lehetővé teszi a voksát egy szavazáson toovote fog létrehozni.

![Képernyőfelvétel a hello szavazóalkalmazást adatbázis-oktatóprogram során létrehozott](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Az adatbázis-oktatóanyag előfeltételei
Ez a cikk hello utasításait követve, előtt győződjön meg, hogy rendelkezik a következőkkel hello:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
 
    VAGY 

    Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md).
* [A Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).  
* [A Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).  
* [Python 2.7-hez készült Microsoft Azure SDK](https://azure.microsoft.com/downloads/). 
* [Python 2.7.13](https://www.python.org/downloads/windows/). 

> [!IMPORTANT]
> Ha telepíti Python 2.7 hello először, győződjön meg arról, hogy hello testreszabása Python 2.7.13 képernyőjén választja **python.exe tooPath hozzáadása**.
> 
> ![Képernyőfelvétel a hello testreszabása Python 2.7.11 képernyő, ahol tooselect Add python.exe tooPath kell](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* [A Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása
Először hozzon létre egy Cosmos DB-fiókot. Ha már rendelkezik fiókkal, vagy használatakor hello Azure Cosmos DB emulátor ehhez az oktatóanyaghoz, ugorjon túl[2. lépés: új Python Flask-webalkalmazás létrehozása](#step-2-create-a-new-python-flask-web-application).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
Most végigvezetjük hogyan toocreate egy új Python Flask-webalkalmazás a hello szabad-e.

## <a name="step-2-create-a-new-python-flask-web-application"></a>2. lépés: Új Python Flask-webalkalmazás létrehozása
1. A Visual Studio, a hello **fájl** menüben mutasson túl**új**, és kattintson a **projekt**.
   
    Hello **új projekt** párbeszédpanel jelenik meg.
2. Hello bal oldali ablaktáblán bontsa ki a **sablonok** , majd **Python**, és kattintson a **webes**. 
3. Válassza ki **Flask webes projekt** hello középső ablaktáblába, majd a hello **neve** mezőbe írja be **oktatóanyag**, és kattintson a **OK**. Ne feledje, hogy Python-csomagok nevében csak kisbetű szerepelhet, a hello [Stílusútmutató Python kód](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).
   
    Ezen új tooPython Flask egy webalkalmazás-fejlesztési keretrendszer, amely segít a webalkalmazások pythonban gyorsabban esetén.
   
    ![Képernyőfelvétel a Visual Studio és Python lévő balra, a Python Flask webes projekt hello középső és hello neve oktatóanyag hello név mezőben kiválasztott hello hello új projekt ablakról](./media/documentdb-python-application/image9.png)
4. A hello **a Python Tools for Visual Studio** ablak, kattintson a **a virtuális környezetbe telepítése**. 
   
    ![Képernyőfelvétel a hello adatbázisról-oktatóanyagról – Python Tools for Visual Studio ablak](./media/documentdb-python-application/python-install-virtual-environment.png)
5. A hello **virtuális környezet hozzáadása** ablakban hello alapértelmezések elfogadásához és a Python 2.7-es alapszintű hello környezetben használható, mert a PyDocumentDB jelenleg nem támogatja a Python 3.x, és kattintson **létrehozása**. Hello szükséges Python virtuális környezetet a projekt állít be.
   
    ![Képernyőfelvétel a hello adatbázisról-oktatóanyagról – Python Tools for Visual Studio ablak](./media/documentdb-python-application/image10_A.png)
   
    a kimeneti ablakban jelennek meg hello `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` amikor hello környezet sikeres telepítését követően.

## <a name="step-3-modify-hello-python-flask-web-application"></a>3. lépés: Hello Python Flask-webalkalmazás módosítása
### <a name="add-hello-python-flask-packages-tooyour-project"></a>Hello Python Flask-csomagok tooyour projekt hozzáadása
A projekt beállítását követően tooadd hello szükséges Flask csomagok tooyour projekt, beleértve a pydocumentdb hello Python-csomag a DocumentDB lesz szüksége.

1. A Solution Explorerben nyissa meg a hello fájlt nevű **requirements.txt** , és cserélje ki hello tartalmát hello következőre:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. Mentse a hello **requirements.txt** fájlt. 
3. A Solution Explorer (Megoldáskezelő) nézetben kattintson a jobb gombbal az **env** elemre, majd kattintson az **Install from requirements.txt** (Telepítés a requirements.txt fájlból) lehetőségre.
   
    ![Képernyőfelvétel az ENV elem (Python 2.7) ki a telepítés a requirements.txt hello listában kijelölt](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    A sikeres telepítés után hello a kimeneti ablakban hello következő:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > Ritka esetekben hello kimeneti ablakban hiba jelenhet meg. Ha ez történik, ellenőrizze, hogy hello hiba-e a kapcsolódó toocleanup. Egyes esetekben nem sikerül hello karbantartása, de hello telepítése akkor is sikeres (görgessen fel a hello kimeneti ablak tooverify ez). Ellenőrizheti a telepítés állapotát [ellenőrzése hello virtuális környezet](#verify-the-virtual-environment). Ha hello telepítése sikertelen volt, de hello ellenőrzés sikeres, akkor OK toocontinue.
   > 
   > 

### <a name="verify-hello-virtual-environment"></a>Hello virtuális környezet ellenőrzése
Ellenőrizzük, hogy minden megfelelően telepítve van-e.

1. Hello megoldás kiépítését, billentyűkombináció lenyomásával **Ctrl**+**Shift**+**B**.
2. Sikeres hello fordítás után indítsa el a webhelyet hello billentyűkombináció lenyomásával **F5**. Ez elindítja a hello Flask fejlesztési kiszolgálót, és a webböngészőt. A következő lap hello kell megjelennie.
   
    ![hello üres Python Flask webes fejlesztési projekt a böngészőben megjelenített](./media/documentdb-python-application/image12.png)
3. Nyomja le a hello webhely hibakeresésének leállításához **Shift**+**F5** a Visual Studióban.

### <a name="create-database-collection-and-document-definitions"></a>Adatbázis-, gyűjtemény- és dokumentum-definíciók létrehozása
Ideje létrehozni a szavazóalkalmazást az új fájlok hozzáadásával, valamint a többi fájl frissítésével.

1. A Megoldáskezelőben kattintson a jobb gombbal hello **oktatóanyag** projektre, kattintson **Hozzáadás**, és kattintson a **új elem**. Válassza ki **üres Python-fájl** és nevű hello fájl **forms.py**.  
2. Adja hozzá a következő kód toohello forms.py fájl hello, és mentse hello fájlt.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-hello-required-imports-tooviewspy"></a>Adja hozzá a szükséges hello importálja tooviews.py
1. A Megoldáskezelőben bontsa ki a hello **oktatóanyag** mappára, majd nyissa meg hello **views.py** fájlt. 
2. Adja hozzá a következő importálási utasítások toohello felső részén hello hello **views.py** fájlt, majd mentse hello fájlt. Ezek importálása Cosmos DB Python SDK-IT és hello Flask-csomagokat.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Adatbázisok, gyűjtemények és dokumentumok létrehozása
* Még mindig **views.py**, adja hozzá a következő kód toohello hello fájl vége hello. Ezzel létrehozza hello űrlap által használt hello adatbázist hoz létre. Ne törölje a meglévő kód hello **views.py**. Egyszerűen csak fűzze hozzá toohello ennek.

```python
@app.route('/create')
def create():
    """Renders hello contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt toodelete hello database.  This allows this toobe used toorecreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a>Adatbázis, gyűjtemény és dokumentum beolvasása, valamint az űrlap elküldése
* Még mindig **views.py**, adja hozzá a következő kód toohello hello fájl vége hello. Ezzel létrehozza hello űrlap hello adatbázis, gyűjtemény és dokumentum olvasása beállítása. Ne törölje a meglévő kód hello **views.py**. Egyszerűen csak fűzze hozzá toohello ennek.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take hello data from hello deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model toopass tooresults.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-hello-html-files"></a>Hello HTML-fájlok létrehozása
1. A Megoldáskezelőben a hello **oktatóanyag** mappa, a jobb oldali kattintson hello **sablonok** mappát, kattintson a **hozzáadása**, és kattintson a **új elem**. 
2. Válassza ki **HTML-weblap**, majd a hello név mezőbe írja be a **create.html**. 
3. Ismételje meg az 1. és 2 toocreate két további HTML-fájlok: ezek a results.html és a vote.html.
4. Adja hozzá a következő kód túl hello**create.html** a hello `<body>` elemet. Ez megjelenít egy üzenetet, miszerint sikeresen létrehozott egy új adatbázist, gyűjteményt és dokumentumot.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Adja hozzá a következő kód túl hello**results.html** a hello `<body`> elemet. Hello hello lekérdezési eredményeit jeleníti meg.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of hello vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Adja hozzá a következő kód túl hello**vote.html** a hello `<body`> elemet. Hello lekérdezési jeleníti meg, és elfogadja hello szavazatot. Hello szavazatok regisztrálása, hello vezérlő tooviews.py, ahol rendszer hello leadott ismeri fel és ennek megfelelően hozzáfűzése hello dokumentum keresztül lett átadva.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way toohost an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. A hello **sablonok** mappa, a név felülírandó hello tartalmát **index.html** hello következőre. Ez az alkalmazás kezdőlapján hello funkcionál.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear hello Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-hello-initpy"></a>Konfigurációs fájl felvétele és módosítása hello \_ \_init\_\_.py
1. A Megoldáskezelőben kattintson a jobb gombbal hello **oktatóanyag** projektre, kattintson **hozzáadása**, kattintson **új elem**, jelölje be **üres Python-fájl**, majd nevű hello fájl **config.py**. A Flask űrlapjainak szüksége van erre a konfigurációs fájlra. Használat tooprovide egy titkos kulcsot is. A jelen oktatóanyaghoz azonban nincs szükség ilyen kulcsra.
2. Adja hozzá a következő hello kód tooconfig.py, szüksége lesz tooalter hello értékének **DOCUMENTDB\_állomás** és **DOCUMENTDB\_kulcs** hello következő lépésben.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. Hello a [Azure-portálon](https://portal.azure.com/), keresse meg a toohello **kulcsok** panelre. Ehhez kattintson **Tallózás**, **Azure Cosmos DB fiókok**, kattintson duplán a hello neve a hello toouse fiókra, majd hello **kulcsok** hello gombjára **Essentials** területen. A hello **kulcsok** panelen, a Másolás hello **URI** értékét, és illessze be hello **config.py** fájl hello hello értékként **DOCUMENTDB\_állomás**  tulajdonság. 
4. Vissza az Azure portálra, az hello hello **kulcsok** panelen másolási hello értékének hello **elsődleges kulcs** vagy hello **másodlagos kulcs**, és illessze be hello **config.py**  fájl hello hello értékként **DOCUMENTDB\_kulcs** tulajdonság.
5. A hello  **\_ \_init\_\_.py** fájlt, adja hozzá a következő sor hello. 
   
        app.config.from_object('config')
   
    Így hello hello fájl tartalma:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. A felvett összes hello fájlok, Megoldáskezelőben kell kinéznie:
   
    ![Képernyőfelvétel a hello Visual Studio Solution Explorer ablak](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>4. lépés: A webalkalmazás helyileg történő futtatása
1. Hello megoldás kiépítését, billentyűkombináció lenyomásával **Ctrl**+**Shift**+**B**.
2. Sikeres hello fordítás után indítsa el a webhelyet hello billentyűkombináció lenyomásával **F5**. Hello következő kell megjelennie a képernyőn.
   
    ![Képernyőfelvétel a hello Python + Azure Cosmos DB Szavazóalkalmazásról megjelenik a webböngészőben](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. Kattintson a **Létrehozás/törlés hello szavazás adatbázis** toogenerate hello adatbázis.
   
    ![Képernyőfelvétel a hello létrehozása lap hello webalkalmazás – fejlesztési részletek](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. Ezután kattintson a **Vote** (Szavazás) elemre, és válassza ki a kívánt elemet.
   
    ![Képernyőfelvétel a hello webalkalmazásról és a szavazási kérdés feltételéről](./media/documentdb-python-application/cosmos-db-vote.png)
5. Minden leadott szavazattal az hello megfelelő számlálót növeli.
   
    ![Képernyőfelvétel a hello hello szavazási lapon látható eredményei](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. Nyomja le a Shift + F5 hello projekt hibakeresésének leállításához.

## <a name="step-5-deploy-hello-web-application-tooazure"></a>5. lépés: Hello webes alkalmazás tooAzure telepítése
Most, hogy hello teljes alkalmazás megfelelően működik-e Cosmos DB szemben, az oktatóanyagban módosítjuk toodeploy a tooAzure.

1. Kattintson a jobb gombbal hello projektre a Solution Explorer (Győződjön meg arról, hogy Ön nem továbbra is helyben fut), majd **közzététel**.  
   
     ![Képernyőfelvétel a hello "tutorial" projektről a Solution Explorer hello közzététel lehetőséggel kiemelve](./media/documentdb-python-application/image20.png)
2. A hello **közzététel** párbeszédpanelen jelölje ki **Microsoft Azure App Service**, jelölje be **hozzon létre új**, és kattintson a **közzététel**.
   
    ![Képernyőfelvétel a hello webhely közzététele ablak kiemelt Microsoft Azure App Service szolgáltatással](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. A hello **létrehozása az App Service** párbeszédpanelen adja meg a webalkalmazás, valamint hello nevét a **előfizetés**, **erőforráscsoport**, és **App Service-csomag** , majd kattintson a **létrehozása**.
   
    ![Képernyőfelvétel a hello Microsoft Azure Web Apps ablak ablak](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. Néhány másodpercen belül a Visual Studio befejezi az app service közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!

    ![Képernyőfelvétel a hello Microsoft Azure Web Apps ablak ablak](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a>Hibaelhárítás
Ha ez hello első Python-alkalmazás futtatását a számítógépen, győződjön meg arról, hogy hello következő mappák (vagy hello azokkal egyenértékű telepítési helyek) szerepelnek a PATH változóban:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Ha hibaüzenetet kap a szavazási lapon, és elnevezett a projekt valami eltérő **oktatóanyag**, győződjön meg arról, hogy  **\_ \_init\_\_.py** hivatkozások hello megfelelő projektnévre hello sorban: `import tutorial.view`.

## <a name="next-steps"></a>Következő lépések
Gratulálunk! Ebben az esetben az első Python webes alkalmazás Cosmos DB használatával befejeződött, és közzétette azt tooAzure.

Gyakran frissítjük és javítjuk a jelen témakört a visszajelzések alapján.  Egyszer, elsajátította hello oktatóanyagban hello szavazás hello felső és a lap alján gombok használatával, és lehet, hogy tooinclude Várjuk visszajelzését a végrehajtott toosee kívánt milyen fejlesztéseket. Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude a megjegyzéseit.

tooadd további funkciók tooyour webalkalmazás, tekintse át hello hello elérhető API-k [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).

Azure, a Visual Studio és a Pythonnal kapcsolatos további információkért lásd: hello [Python fejlesztői központ](https://azure.microsoft.com/develop/python/). 

További további Python Flask-oktatóanyagok: [hello Flask Mega-oktatóanyagban rész I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
