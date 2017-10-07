---
title: "Azure Cosmos DB használatával aaaJava alkalmazásfejlesztési oktatóanyag |} Microsoft Docs"
description: "Ez a Java webalkalmazásokra vonatkozó oktatóanyag bemutatja, hogyan toouse hello Azure Cosmos DB és DocumentDB API toostore hello és elérni az adatokat az Azure websitesban tárolt Java-alkalmazás."
keywords: "Alkalmazásfejlesztés, adatbázis-oktatóanyag, java-alkalmazás, java-webalkalmazás oktatóanyag, documentdb, azure, Microsoft Azure"
services: cosmos-db
documentationcenter: java
author: dennyglee
manager: jhubbard
editor: mimig
ms.assetid: 0867a4a2-4bf5-4898-a1f4-44e3868f8725
ms.service: cosmos-db
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/22/2017
ms.author: denlee
ms.openlocfilehash: e073de23beb0037ee1e37b48a69e8fe7cdc3fc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-hello-documentdb-api"></a>Azure Cosmos DB és hello DocumentDB API használatával Java-webalkalmazás létrehozása
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Ez a Java webalkalmazásokra vonatkozó oktatóanyag bemutatja, hogyan toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás toostore és hozzáférés az Azure App Service Web Apps tárolt Java-alkalmazás. A témakörben érintett témák köre:

* Hogyan toobuild egy alapszintű JavaServer lapok (JSP) alkalmazást az eclipse-ben.
* Hogyan toowork hello Azure Cosmos DB a szolgáltatás használatával hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).

A Java vonatkozó oktatóanyag bemutatja, hogyan toocreate webalapú Feladatkezelő alkalmazást, amely lehetővé teszi, hogy toocreate, lekérése és be van jelölve feladatok el azokat, ahogy az a következő kép hello. Egyes hello feladatokat a teendőlista hello Azure Cosmos DB JSON-dokumentumokként tárolja.

![Saját teendőlista Java-alkalmazása](./media/documentdb-java-application/image1.png)

> [!TIP]
> Ez az alkalmazásfejlesztési oktatóanyag feltételezi, hogy rendelkezik korábbi tapasztalattal a Java használatát illetően. Ha új tooJava vagy hello [előfeltételt jelentő eszközöket](#Prerequisites), érdemes letöltenie a teljes hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektet a Githubról, majd lefordítani azt [hello hello végén ez utasításokat a cikk](#GetProject). Ha felépítette, hello cikk toogain insight hello kódja hello projekt környezetében hello tekintheti meg.  
> 
> 

## <a id="Prerequisites"></a>A jelen Java-webalkalmazásokra vonatkozó oktatóanyag előfeltételei
Az alkalmazásfejlesztési oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* Aktív Azure-fiók. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További részletekért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/)

    VAGY

    Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md).
* [Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Eclipse IDE for Java EE Developers.](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [Egy Azure Java-futtatókörnyezettel (pl. Tomcat vagy Jetty) engedélyezve van a webhelyen.](../app-service-web/web-sites-java-get-started.md)

Telepítése hello az eszközök először, coreservlets.com webhelyen hello telepítési folyamat hello gyors üzembe helyezés szakaszában a [oktatóanyag: TomCat7 telepítése és használata a Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) cikk.

## <a id="CreateDB"></a>1. lépés: Azure Cosmos DB-fiók létrehozása
Először hozzon létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik fiókkal, vagy használatakor hello Azure Cosmos DB emulátor ehhez az oktatóanyaghoz, ugorjon túl[2. lépés: hello Java JSP-alkalmazás létrehozása](#CreateJSP).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <a id="CreateJSP"></a>2. lépés: Hello Java JSP-alkalmazás létrehozása
toocreate hello JSP-alkalmazás:

1. Először is hozzon létre egy Java-projektet. Indítsa el az Eclipse-t, kattintson a **File** (Fájl), **New** (Új), majd a **Dynamic Web Projekt** (Dinamikus webes projekt) lehetőségre. Ha nem lát **dinamikus webes projekt** szerepel a listában az elérhető projektek, a következő hello: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt**..., bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.
   
    ![JSP Java-alkalmazások fejlesztése](./media/documentdb-java-application/image10.png)
2. Adja meg a projekt nevét a hello **projektnevet** mezőbe, és a hello **Target Runtime** legördülő menüben válassza ki (pl. Apache Tomcat v7.0) értéket nem kötelező, és kattintson **Befejezés**. A tervezett futásidőt kiválasztása lehetővé teszi, hogy Ön toorun a projekten helyileg eclipse-ben.
3. Az eclipse-ben hello Project Explorer nézet, bontsa ki a projektet. Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.
4. A hello **új JSP-fájl** párbeszédpanelen nevű hello fájl **index.jsp**. Hello fölérendelt mappája, tartsa **WebContent**, ahogy a következő ábra hello, és kattintson a **következő**.
   
    ![Új JSP-fájl létrehozása – Java-alkalmazásokra vonatkozó oktatóanyag](./media/documentdb-java-application/image11.png)
5. A hello **JSP-sablon kiválasztása** párbeszédpanelen hello céllal történik, az oktatóanyag válassza **új JSP-fájl (html)**, és kattintson a **Befejezés**.
6. Hello index.jsp fájl megnyitásakor az eclipse-ben, vegye fel a szöveges toodisplay **Hello World!** hello meglévő belül <body> elemet. A frissített <body> tartalom a következő kód hello hasonlóan kell kinéznie:
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. Mentse a hello index.jsp fájlt.
8. Ha megadta a tervezett futásidőt a 2. lépésben, **projekt** , majd **futtatása** toorun a JSP-alkalmazás helyileg:
   
    ![Hello World – Java-alkalmazásokra vonatkozó oktatóanyag](./media/documentdb-java-application/image12.png)

## <a id="InstallSDK"></a>3. lépés: Hello DocumentDB Java SDK telepítése
hello legegyszerűbb módja toopull hello DocumentDB Java SDK-t és annak függőségeit van keresztül [Apache Maven](http://maven.apache.org/).

toodo, szüksége lesz tooconvert a projekt tooa maven project hello lépések végrehajtásával:

1. Kattintson a jobb gombbal a projektre a Project Explorer hello parancsra **konfigurálása**, kattintson a **tooMaven projekt átalakítása**.
2. A hello **új POM létrehozása** ablakot, fogadja el hello alapértelmezett beállításokat, és kattintson a **Befejezés**.
3. A **Project Explorer**, nyissa meg hello pom.xml fájlt.
4. A hello **függőségek** lap hello **függőségek** ablaktáblán kattintson a **Hozzáadás**.
5. A hello **függőség kiválasztása** ablakban, a következő hello:
   
   * A hello **csoportazonosító** mezőbe írja be a következőt: com.microsoft.azure.
   * A hello **összetevő-azonosító** mezőbe írja be az azure-documentdb.
   * A hello **verzió** 1.5.1 adja meg.
     
   ![A DocumentDB Java Application SDK telepítése](./media/documentdb-java-application/image13.png)
     
   * Vagy hello függőség XML hozzáadása csoporthoz és összetevő-azonosító közvetlenül toohello pom.xml fájlhoz egy szövegszerkesztő:
     
        <dependency><groupId>következőt: com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version></dependency>
6. Kattintson a **OK** és után a Maven feltelepíti a DocumentDB Java SDK hello.
7. Mentse a hello pom.xml fájlt.

## <a id="UseService"></a>4. lépés: Hello Azure Cosmos DB szolgáltatás használata Java-alkalmazások
1. Először is határozza meg hello TodoItem objektum TodoItem.java:
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    A projekt használatával hoztuk [Project Lombok](http://projectlombok.org/) toogenerate hello konstruktor, beolvasóinak, Setter elemek és a felépítőt. Azt is megteheti, ezzel a kóddal manuálisan írási, vagy rendelkezik hello IDE hozható létre.
2. tooinvoke hello Azure Cosmos DB szolgáltatás, létre kell hozni egy új **DocumentClient**. Ez általában legjobb tooreuse hello **DocumentClient** - ahelyett, hogy minden későbbi kérés esetén új ügyfelet létre. Hello ügyfél használatával hello ügyfél újrafelhasználásához egy **DocumentClientFactory**. DocumentClientFactory.java, toopaste hello URI és PRIMARY KEY érték tooyour vágólapra mentett kell [1. lépés](#CreateDB). Cserélje ki a [YOUR\_ENDPOINT\_HERE] részt az URI-re, a [YOUR\_KEY\_HERE] részt pedig az elsődleges kulcsra.
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. Most hozzon létre egy Data Access objektum (DAO) tooabstract a ToDo elemeket tooAzure Cosmos DB megőrzése.
   
    A rendezés toosave ToDo elemeket tooa gyűjtemény, hello ügyfélnek kell tooknow melyik adatbázis és gyűjtemény toopersist túl (által hivatkozott erre az önhivatkozások). Ez általában legjobb toocache hello adatbázist és a gyűjteményt, ha lehetséges tooavoid fölösleges üzenetváltás toohello adatbázis.
   
    hello alábbi kód bemutatja, hogyan tooretrieve az adatbázist és a gyűjteményt, ha létezik, vagy hozzon létre egy újat, ha még nem létezik:
   
        public class DocDbDao implements TodoDao {
            // hello name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // hello name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // hello Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for hello database object, so we don't have tooquery for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for hello collection object, so we don't have tooquery for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get hello database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache hello database object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create hello database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get hello collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache hello collection object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create hello collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. következő lépés hello toowrite van néhány kódot toopersist hello TodoItems toohello gyűjteményben. A jelen példában használjuk [Gson](https://code.google.com/p/google-gson/) tooserialize és deszerializálni a TodoItem teendő Pojo objektumait (POJOs) tooJSON dokumentumok.
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize hello TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate hello document as a TodoItem for retrieval (so that we can
            // store multiple entity types in hello collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist hello document using hello DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. Csakúgy, mint az Azure Cosmos DB-adatbázisok és -gyűjtemények esetén, a dokumentumok önhivatkozásokkal is hivatkoznak magukra. hello következő segítő függvény segítségével velünk lekérése dokumentumok egy másik attribútum (pl. "id"), hanem önhivatkozást:
   
        private Document getDocumentById(String id) {
            // Retrieve hello document using hello DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
   
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }
6. A Microsoft hello segédmetódus használja az 5. lépés tooretrieve egy teendő JSON-dokumentum-azonosító szerint, és tooa pojo-vá deszerializálni:
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve hello document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize hello document in tooa TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. Hello DocumentClient tooget azt is használható, egy gyűjtemény vagy a DocumentDB SQL használatával TodoItems listáját:
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve hello TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize hello documents in tooTodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. Nincsenek számos módon tooupdate hello DocumentClient rendelkező dokumentumot. A teendőlista alkalmazásában szeretnénk tudni tootoggle toobe e beállíthatnánk. Ez hello hello dokumentum "kész" attribútumának frissítésével érhető el:
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve hello document from hello database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update hello document as a JSON document directly.
            // For more complex operations - you could de-serialize hello document in
            // tooa POJO, update hello POJO, and then re-serialize hello POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace hello updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. Végül azt szeretnénk hello képességét toodelete Beállíthatnánk listájából. toodo, korábban megírt segédmetódus hello használhatjuk tooretrieve hello önhivatkozást, és ezután adja meg az ügyfél toodelete hello azt:
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers toodocuments by self link rather than id.
   
            // Query for hello document tooretrieve hello self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete hello document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <a id="Wire"></a>5. lépés: A Java-alkalmazásfejlesztési projekt hello hello részeinek együtt huzalozási
Most, hogy befejeztük hello visszatöltött bits - összes marad toobuild gyors felhasználói felületet, és hozzá kell fűznie azt tooour DAO fel.

1. Első lépésként kezdjük egy tartományvezérlő toocall DAO felépítése:
   
        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
   
            private static TodoItemController todoItemController;
   
            private final TodoDao todoDao;
   
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
   
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
   
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
   
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
   
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
   
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }
   
    Egy összetettebb alkalmazásban hello vezérlő bonyolult üzleti logikát is hello DAO mellett.
2. Ezután létrehozunk egy servlet tooroute HTTP kérelmeket toohello tartományvezérlő:
   
        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
   
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
   
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
   
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
   
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
   
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
   
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
   
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
   
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
   
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
   
                response.getWriter().println(apiResponse);
            }
   
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }
3. Szükség lesz egy webes felhasználói felület toodisplay toohello felhasználó. Ideje újra lefuttatni hello index.jsp korábban létrehozott:
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding toobody for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
   
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
   
            <hr/>
   
            <!-- hello ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
   
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
   
            </div>
   
            <hr/>
   
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
   
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
   
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
   
          </div>
   
          <!-- Placed at hello end of hello document so hello pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. És végezetül ügyféloldali JavaScript tootie hello webes felhasználói felület egyes írási és a servlet együtt hello:
   
        var todoApp = {
          /*
           * API methods toocall Java backend.
           */
          apiEndpoint: "api",
   
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
   
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
   
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
   
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
   
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
   
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
   
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
   
              // Call api tooupdate todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
   
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
   
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
   
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
   
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
   
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
   
            });
          },
   
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
   
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
   
          ui_createButton: function() {
            return $(".todoForm button");
          },
   
          ui_table: function() {
            return $(".todoList table tbody");
          },
   
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
   
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
   
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
   
          /*
           * Install hello TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
   
            todoApp.getTodoItems();
          }
        };
   
        $(document).ready(function() {
          todoApp.install();
        });
5. Nagyszerű! Most, hogy marad az összes alkalmazás, amely tootest hello. Hello alkalmazás helyileg történő futtatása, és adja hozzá az egyes Todo elemeket kitöltése hello elemek nevét és kategóriáját, majd kattintson **feladat hozzáadása**.
6. Hello elem jelenik meg, miután-e már azt való átváltással hello jelölőnégyzetet, majd frissítheti **feladatok frissítése**.

## <a id="Deploy"></a>6. lépés: A Java-alkalmazás tooAzure webhelyek központi telepítése
Az Azure-webhelyek teszi a Java-alkalmazások telepítését más dolga, mint exportálni az alkalmazást WAR-fájlként, majd feltölteni azt a verziókövetési rendszerrel (pl. Git) vagy FTP segítségével.

1. tooexport az alkalmazás WAR-fájlként kattintson a jobb gombbal a projektre a **Project Explorer**, kattintson a **exportálása**, és kattintson a **WAR-fájlt**.
2. A hello **WAR exportálása** ablakban, a következő hello:
   
   * Hello webes projekt mezőben adja meg azure-documentdb-java-sample.
   * Hello cél mezőben válassza ki a cél toosave hello WAR-fájlt.
   * Kattintson a **Befejezés** gombra.
3. Most, hogy a WAR-fájl jár, egyszerűen feltöltheti azt tooyour Azure webhelyére **webapps** könyvtár. Hello fájl feltöltésével kapcsolatos útmutatásért lásd: [hozzáadása a Java-alkalmazás tooAzure App Service Web Apps](../app-service-web/web-sites-java-add-app.md).
   
    Ha hello WAR-fájl feltöltése toohello webapps könyvtárba, hello futtatókörnyezet észleli majd annak hozzáadását, és automatikusan betölti azt.
4. tooview a kész termék, keresse meg a toohttp://YOUR\_hely\_NAME.azurewebsites.net/azure-java-sample/ és a feladatok hozzáadását start!

## <a id="GetProject"></a>Hello projekt beszerzése a Githubról
Összes hello minta ebben az oktatóanyagban szereplő hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projekt a Githubon. tooimport hello teendők projekt eclipse-ben történő ellenőrizze, hogy hello szoftver- és erőforrás hello [Előfeltételek](#Prerequisites) területen, majd a következő hello:

1. Telepítse a [Project Lombok](http://projectlombok.org/) nevű projektet. Lombok használt toogenerate konstruktorainak, beolvasóinak, Setter hello projektben. Hello lombok.jar fájl letöltését követően kattintson rá duplán tooinstall, vagy telepítse a hello parancssorból.
2. Ha az Eclipse meg nyitva, zárja be, és indítsa újra a tooload Lombok.
3. Az eclipse-ben, a hello **fájl** menüben kattintson a **importálási**.
4. A hello **importálási** ablakban kattintson **Git**, kattintson a **Projects from Git**, és kattintson a **tovább**.
5. A hello **tárház forrásának kijelölése** kattintson **Klónozás URI**.
6. A hello **forrás Git-tárház** képernyő hello **URI** mezőbe, írja be a https://github.com/Azure-Samples/java-todo-app.git, és kattintson **következő**.
7. A hello **ág kiválasztása** képernyőn **fő** van kiválasztva, és kattintson **következő**.
8. A hello **helyi cél** kattintson **Tallózás** tooselect egy mappát, amelyben hello tárház másolhatók, és kattintson a **következő**.
9. A hello **válassza ki a projektek importálásához varázsló toouse** képernyőn **létező projektek importálása** van kiválasztva, és kattintson **következő**.
10. A hello **Import Projects** képernyő, visszavonása hello **DocumentDB** projektre, és kattintson a **Befejezés**. hello DocumentDB-projekt tartalmazza hello Azure Cosmos DB Java SDK, amelyre a adunk hozzá a függőség beállításához helyette.
11. A **Project Explorer**tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java váltson, és cserélje le hello HOST és MASTER_KEY értékeket hello URI és PRIMARY KEY a Azure Cosmos DB-fiókot, majd mentés hello fájlt. További információ: [1. lépés Egy Azure Cosmos DB adatbázisfiók létrehozása](#CreateDB).
12. A **Project Explorer**, kattintson jobb gombbal a hello **azure-documentdb-java-sample**, kattintson **Build elérési**, és kattintson a **konfigurálása Build elérési**.
13. A hello **Java Build elérési** hello jobb oldali ablaktáblában képernyőn válassza hello **szalagtárak** fülre, majd **külső JARs hozzáadása**. Keresse meg a toohello hello lombok.jar fájl helyét, és kattintson a **nyitott**, és kattintson a **OK**.
14. Használjon 12. lépés tooopen hello **tulajdonságok** ablak újra, majd a hello bal oldali ablaktáblán kattintson **megcélzott futtatókörnyezetek**.
15. A hello **megcélzott futtatókörnyezetek** kattintson **új**, jelölje be **Apache Tomcat v7.0**, és kattintson a **OK**.
16. Használjon 12. lépés tooopen hello **tulajdonságok** ablak újra, majd a hello bal oldali ablaktáblán kattintson **Project Facets**.
17. A hello **Project Facets** képernyőn válassza ki **dinamikus webmodul** és **Java**, és kattintson a **OK**.
18. A hello **kiszolgálók** üdvözlő képernyőt hello alján lapon kattintson a jobb gombbal **Tomcat v7.0 Server localhost:** majd **hozzáadása és eltávolítása a**.
19. A hello **hozzáadása és eltávolítása a** ablakban helyezze át **azure-documentdb-java-sample** toohello **beállított** gombra, majd **Befejezés**.
20. A hello **kiszolgálók** lapon kattintson a jobb gombbal **Tomcat v7.0 Server localhost:**, és kattintson a **indítsa újra a**.
21. Egy böngészőben nyissa meg a toohttp://localhost:8080 / azure-documentdb-java-sample / és tooyour feladatlista felvételének megkezdése. Figyelje meg, hogy ha módosította az alapértelmezett értékeket, módosítsa a 8080-as toohello értéket választotta.
22. toodeploy a projekt tooan Azure-webhelyre, lásd: [6. lépés. Az alkalmazás tooAzure webhelyek telepítése](#Deploy).

[1]: media/documentdb-java-application/keys.png
