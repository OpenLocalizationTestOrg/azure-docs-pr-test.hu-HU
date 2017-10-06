---
title: aaaHow toouse hello Azure Mobile Apps SDK for Android |} Microsoft Docs
description: Hogyan toouse hello Azure Mobile Apps SDK Android
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: glenga
ms.openlocfilehash: 56eb73c4e1703d69877be499a09fc2130f1d68e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-sdk-for-android"></a>Hogyan toouse hello Azure Mobile Apps SDK Android

Ez az útmutató bemutatja, hogyan a toouse hello Android ügyfél SDK Mobile Apps tooimplement szabhatják, például:

* Lekérdezi az adatok (beszúrása, frissítése és törlése).
* Hitelesítés.
* Kezeli a hibákat.
* Hello ügyfél testreszabása.

Ez az útmutató hello ügyféloldali Android SDK összpontosít.  toolearn arról hello kiszolgálóoldali SDK-k a Mobile Apps, lásd: [működéséhez a .NET-háttérrendszerrel SDK] [ 10] vagy [hogyan toouse hello Node.js-háttéralkalmazáshoz SDK] [ 11].

## <a name="reference-documentation"></a>Referenciadokumentációt

Hello található [Javadocs API-referencia] [ 12] a hello Android ügyféloldali kódtára a Githubon.

## <a name="supported-platforms"></a>A támogatott platformok

hello Azure Mobile Apps SDK for Android API szintek 19-24 (KitKat nugát keresztül) helyigénnyel telefon és táblagép támogatja.  Hitelesítés, különösen, egy közös webes keretrendszer megközelítés toogather hitelesítő adatokat használja.  Kiszolgáló-folyamat hitelesítési kis űrlap tényező eszközök, például az órákat nem működik.

## <a name="setup-and-prerequisites"></a>A telepítő és Előfeltételek

Teljes hello [Mobile Apps gyors üzembe helyezés](app-service-mobile-android-get-started.md) oktatóanyag.  Ez a feladat biztosítja, hogy a fejlesztés az Azure Mobile Apps minden előfeltétel teljesült.  gyors üzembe helyezés hello is segítséget nyújt a fiókjának a konfigurálása és az első Mobile Apps-háttéralkalmazás létrehozása.

Ha úgy dönt, hogy nem toocomplete hello gyors üzembe helyezési útmutató, hajtsa végre a következő feladatok hello:

* [Mobile Apps-háttéralkalmazás létrehozása] [ 13] az androidos toouse.
* Az Android Studióban [frissítés hello Gradle build fájlok](#gradle-build).
* [Engedélyezi az internet engedélycsoportot](#enable-internet).

### <a name="gradle-build"></a>Frissítés hello Gradle fájl létrehozása

Mindkettőt módosíthatja **build.gradle** fájlok:

1. Adja hozzá a kódot toohello *projekt* szint **build.gradle** belüli hello *buildscript* címke:

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. Adja hozzá a kódot toohello *modul app* szint **build.gradle** belüli hello *függőségek* címke:

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    Hello legújabb verziója jelenleg 3.3.0. felsorolt hello támogatott verziók [a Files][14].

### <a name="enable-internet"></a>Internet engedélycsoportot engedélyezése

tooaccess Azure, az alkalmazás lehetővé kell tenniük hello INTERNET engedélyezve van. Ha még nincs engedélyezve, vegye fel a következő kód tooyour üzletági hello **AndroidManifest.xml** fájlt:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>Egy ügyfél-kapcsolat létrehozása

Az Azure Mobile Apps tooyour mobilalkalmazás négy funkciókat biztosít:

* Adathozzáférés és -kapcsolat nélküli szinkronizálás az Azure Mobile Apps-szolgáltatások.
* Azure Mobile Apps Server SDK hello készült egyéni API-k hívása.
* Hitelesítés az Azure App Service-hitelesítéshez és engedélyezéshez.
* Leküldéses értesítés regisztrálása a Notification hubs használatával.

Ezek először megköveteli, hogy hozzon létre egy `MobileServiceClient` objektum.  Csak egy `MobileServiceClient` objektumot kell létrehozni a mobil ügyfél (Ez azt jelenti, hogy legyen egy egypéldányos mintát).  toocreate egy `MobileServiceClient` objektum:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with hello Site URL
    this);                  // Your application Context
```

Hello `<MobileAppUrl>` egy karakterlánc vagy egy URL-cím objektum, amely tooyour mobil-háttéralkalmazást mutat.  Ha az Azure App Service toohost a mobil-háttéralkalmazást használ, majd győződjön meg arról hogy biztonságos hello `https://` hello URL-cím verzióját.

hello ügyfél is szükséges hozzáférés toohello tevékenység vagy a környezet – hello `this` hello példában paraméter.  hello MobileServiceClient konstrukció hello belül megtörténik `onCreate()` hello hivatkozott tevékenység hello metódusában `AndroidManifest.xml` fájlt.

Ajánlott eljárásként a kiszolgáló közötti kommunikációt a saját (Egypéldányos – minta) osztályba kell absztrakt.  Ebben az esetben kell átadni hello tevékenység belül hello konstruktor tooappropriately hello szolgáltatás konfigurálásához.  Példa:

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

Most hívása `AzureServiceAdapter.Initialize(this);` a hello `onCreate()` a fő tevékenységnél metódusában.  Semmilyen más metódus hozzáférési toohello ügyfél kellene használni `AzureServiceAdapter.getInstance();` tooobtain egy hivatkozás toohello szolgáltatás adapter.

## <a name="data-operations"></a>Adatok műveletek

az Azure Mobile Apps SDK hello hello core tooprovide hozzáférés toodata hello Mobile Apps-háttéralkalmazás SQL Azure-ban tárolt.  Hozzáférnének ehhez ezen adatokhoz szigorú típusmegadású osztályokat (ajánlott) vagy típus nélküli lekérdezéseket (nem ajánlott).  a jelen szakasz hello tömeges foglalkozik szigorú típusmegadású osztályokat.

### <a name="define-client-data-classes"></a>Ügyfél adatosztályok definiálása

SQL Azure-táblák, tooaccess adatait ügyfél adatok definiálására, amelyek megfelelnek a Mobile Apps-háttéralkalmazás hello toohello táblák. Ebben a témakörben szereplő példák feltételezik nevű tábla **MyDataTable**, a következő oszlopok hello tartalmaz:

* id
* Szöveg
* Végezze el

hello megfelelő típusos ügyféloldali objektum található nevű fájlban **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Adja hozzá az egyes mezők hozzáadott elérő és beállító metódusokat.  Ha az SQL Azure táblában több oszlopot tartalmaz, hello megfelelő mezőket toothis osztály szeretné beállítani.  Például, ha hello DTO (adatobjektum átviteli) kellett szerepel egészszám-oszloppal prioritású virtuális gép, majd a mezőt, valamint beolvasó és beállító metódusa:

```java
private Integer priority;

/**
* Returns hello item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets hello item priority
*
* @param priority
*            priority tooset
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

Hogyan toocreate további táblákat a Mobile Apps-háttéralkalmazásának: toolearn [hogyan: Adja meg egy tábla vezérlő] [ 15] (.NET-háttérrendszer) vagy [dinamikus sémával definiálása táblák] [ 16] (Node.js háttérrendszer).

Egy Azure Mobile Apps-háttéralkalmazás tábla négyet elérhető tooclients öt különleges mező határozza meg:

* `String id`: hello hello rekord globálisan egyedi azonosítója.  Ajánlott eljárásként, ellenőrizze a hello azonosító hello karakterláncos ábrázolása egy [UUID] [ 17] objektum.
* `DateTimeOffset updatedAt`: hello hello utolsó frissítés időpontja.  hello updatedAt mező hello kiszolgáló állítja be, és az Ügyfélkód soha nem kell beállítani.
* `DateTimeOffset createdAt`: hello dátum/idő hello objektum lett létrehozva.  hello createdAt mező hello kiszolgáló állítja be, és az Ügyfélkód soha nem kell beállítani.
* `byte[] version`: Megszokott módon jelenik meg egy karakterláncot, hello verziója is állítva hello kiszolgáló.
* `boolean deleted`: Azt jelzi, hogy hello rekord törölve lett, de még nem törlődnek.  Ne használjon `deleted` tulajdonságként a osztályban.

Hello `id` mező kitöltése kötelező.  Hello `updatedAt` mező és `version` mező a kapcsolat nélküli szinkronizáláshoz használt (a növekményes szinkronizálás és az ütközés feloldásához rendre).  Hello `createdAt` mező hivatkozás mező, hello ügyfél nem használja.  hello nevek hello tulajdonságainak "között tömörített" nevében és nem állítható.  Azonban hozhat létre az objektum és hello közötti leképezéseket "közötti átvitel közbeni" nevek használatával hello [gson] [ 3] könyvtárban.  Példa:

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getUpdatedAt() { return mCreatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getText() { return mVersion; }
    public final void setText(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a>Egy tábla hivatkozás létrehozása

tooaccess tábla, először létre kell hoznia egy [MobileServiceTable] [ 8] objektum által hívó hello **getTable** hello metódusa [MobileServiceClient][9].  Ez a módszer két túlterheléssel rendelkezik:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

A kódot, a következő hello **mClient** hivatkozás tooyour MobileServiceClient objektum.  hello első túlterhelési használatos, ahol hello osztálynév és hello tábla neve is hello azonos, és hello egyik használatos hello gyors üzembe helyezés:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

hello második túlterhelési használja, amikor hello táblanév hello osztály nevétől eltérő: hello első paramétere hello tábla neve.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Egy háttér-táblából

Először szerezze be a táblahivatkozás.  Majd hello táblahivatkozás a lekérdezés végrehajtása.  A lekérdezés bármilyen kombinációját:

* A `.where()` [szűrőfeltétel](#filtering).
* Egy `.orderBy()` [záradék rendelési](#sorting).
* A `.select()` [mező kiválasztása záradék](#selection).
* A `.skip()` és `.top()` a [lapozható eredmények](#paging).

rendelés megelőző hello biztosítani kell az hello záradékban.

### <a name="filter"></a>Szűrési eredmények

hello általános a lekérdezések formátuma:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts hello async into a sync result
```

hello példában az összes eredményt visszaadja (felfelé toohello maximális méretének beállítása hello kiszolgáló).  Hello `.execute()` metódus hello lekérdezés végrehajtása a hello háttér.  hello lekérdezés az átalakított tooan [OData v3] [ 19] lekérdezés átviteli toohello Mobile Apps-háttéralkalmazás előtt.  Kézhezvétele után hello Mobile Apps-háttéralkalmazás konvertálja hello lekérdezés előtt futtatnia kell a hello SQL Azure-példány SQL-utasítást.  Mivel a hálózati tevékenységek hosszabb időt vesz igénybe, hello `.execute()` metódus értéket ad vissza egy [ `ListenableFuture<E>` ] [ 18].

### <a name="filtering"></a>Visszaadott adatok szűrése

a következő lekérdezés-végrehajtás hello összes elemet ad vissza, hello **ToDoItem** where tábla **teljes** egyenlő **hamis**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** hello hivatkozás toohello mobilszolgáltatás táblázat, amely a korábban létrehozott.

Hello használatával kapcsolódó szűrő megadásához **ahol** hello táblahivatkozás metódus hívása. Hello **ahol** metódus követi a **mező** metódus egy metódust, amely meghatározza a hello logikai predikátum követ. Lehetséges predikátum módszerekre **eq** (egyenlő), **ne** (egyenlő), **gt** (több mint), **ge** (nagyobb vagy egyenlő), **lt** (kevesebb mint), **le** (kisebb vagy egyenlő). Ezek a módszerek összehasonlítása számát és a karakterlánc mezők toospecific értékeket.

A dátumok végezhet. hello következőkkel lehetővé teszik, hogy összehasonlítható hello teljes dátummezője vagy hello dátum részei: **év**, **hónap**, **nap**, **óra**, **perc**, és **második**. hello következő példakóddal felveheti a cikkek szűrő amelynek *határidő* 2013 egyenlő.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

hello következőkkel támogatja a speciális szűrők karakterláncmezőket: **megadott módon kezdődő**, **megadott módon végződő**, **concat**, **subString**, **indexOf**, **cserélje le**, **toLower**, **toUpper**, **trim**, és  **hossz**. a tábla sort, amelyben hello a következő példa szűrők hello *szöveg* oszlopok kezdődő "PRI0."

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

a mező támogatja a következő operátor módszerek hello: **hozzáadása**, **sub**, **MUL számú**, **div**, **mod**, **emelet**, **felső határ**, és **kerekíteni**. a tábla sort, amelyben hello a következő példa szűrők hello **időtartam** páros szám-e.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

A következő logikai módszerekkel kombinálhatja a predikátum: **és**, **vagy** és **nem**. a következő példa hello két példák megelőző hello egyesíti.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Logikai operátorok csoport és kivételblokkokra:

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

Az ismertető és példákat a szűrés további: [hello Android ügyfél lekérdezési modelljét hello gazdagsága felfedezése][20].

### <a name="sorting"></a>A rendezési adatokat adott vissza

hello következő kódot ad vissza az összes elem táblázatát **ToDoItems** növekvő sorrend szerint hello *szöveg* mező. *mToDoTable* hello hivatkozás toohello háttér táblázatot, amelyet korábban hozott létre:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

az első paraméter hello hello **orderBy** módszer a következő: a karakterlánc egyenlő toohello nevét, mely toosort hello mező. a második paraméter hello használ hello **QueryOrder** számbavételi toospecify e toosort növekvő vagy csökkenő.  Szűrése hello használata esetén ***ahol*** metódust, hello ***ahol*** metódust kell meghívni, mielőtt hello ***orderBy*** metódust.

### <a name="selection"></a>Egyes oszlopok kiválasztásához

hello alábbi kód bemutatja, hogyan összes tooreturn elemek-e a táblázatát **ToDoItems**, csak a hello jeleníti meg, de **teljes** és **szöveg** mezőket. **mToDoTable** hello hivatkozás toohello háttér táblázat, amely a korábban létrehozott.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

hello paraméterek toohello válassza függvény hello karakterlánc neve, amelyet az tooreturn hello a táblázat oszlopaihoz tartozó.  Hello **válasszon** metódust kell toofollow módszerek, például a **ahol** és **orderBy**. Lapozófájl módszerek, például követhetnek **kihagyása** és **felső**.

### <a name="paging"></a>A lapok visszatérési adatai

Adatok **mindig** lapok adott vissza.  hello maximális száma hello kiszolgáló állítja be.  Ha hello ügyfél további rekordok kér, hello kiszolgáló hello maximális számú rekordot ad vissza.  Alapértelmezés szerint a hello maximális lapméretét hello kiszolgálón 50 rögzíti.

hello első példa bemutatja, hogyan tooselect hello felső öt elemet egy táblázatban. hello lekérdezés táblázatát adja vissza hello elemek **ToDoItems**. **mToDoTable** hello hivatkozás toohello háttér táblázatot, amelyet korábban hozott létre:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

Itt az a lekérdezés átugrása hello első öt elemeket, és ezután hello az értéket ad vissza a következő öt:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Ha a tooget összes rekord a táblázatban, valósítja meg a kódot tooiterate keresztül az összes:

```java
List<MyDataModel> results = new List<MyDataModel>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

Ezzel a módszerrel az összes rekord kérelmet legalább két kérelmek toohello Mobile Apps-háttéralkalmazás hoz létre.

> [!TIP]
> Választhatja hello jobb oldal mérete memóriahasználat hello kérelem melletti közben, a sávszélesség-használat és a hello adatfogadásra teljesen késleltetése egyensúlyára.  hello (50 rekordok) alapértelmezés szerint minden eszköz alkalmas.  Kizárólag fog működni, a nagyobb memória-eszközökön, ha növelése too500 fel.  Találtunk, amelyeknek a növekvő hello oldalméret 500 rekordok eredmények túl elfogadható késések és nagy memóriaproblémák léptek fel.

### <a name="chaining"></a>Hogyan: összefűzésére lekérdezési módszerek

háttér táblák lekérdezésére hello módszerek összefűzendő is lehet. Láncolás lekérdezés módszer lehetővé teszi a tooselect adott oszlopok rendezésének és lapozható szűrt sor. Létrehozhat összetett logikai szűrőket.  Minden egyes lekérdezés módszer egy lekérdezés objektumot ad vissza. tooend hello azokat a módszereket, és ténylegesen futtatási hello lekérdezés, hívás hello **hajtható végre** metódust. Példa:

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

hello kapcsolt lekérdezési módszerek az alábbiak szerint lehet rendezni:

1. Szűrés (**ahol**) módszerek.
2. Rendezés (**orderBy**) módszerek.
3. Kijelölés (**válasszon**) módszerek.
4. lapozás (**kihagyása** és **felső**) módszerek.

## <a name="binding"></a>Köthető adatok toohello felhasználói felület

Az adatkötés magában foglalja a három összetevővel:

* hello adatforrás
* hello kezdőképernyő elrendezésének
* hello adapter, hogy ki hello két együtt.

A minta kódban, a rendszer visszaadja a hello adatok hello Mobile Apps SQL Azure táblából **ToDoItem** egy tömbbe. Ez a tevékenység az adatok alkalmazások közös mintázatát.  Az adatbázis-lekérdezések gyakran kell visszaadnia azon sorait, amelyek hello listában vagy tömb kapja. Ez a példa hello tömbjének értéke hello adatforrás.  hello kód határozza meg a kezdőképernyő elrendezésének, amely meghatározza a hello nézetben megjelenő adatok hello hello eszközön.  hello két kötött együtt egy-adapternek, amely ezt a kódot a hello bővítménye **ArrayAdapter&lt;ToDoItem&gt;**  osztály.

#### <a name="layout"></a>Adja meg a hello elrendezés

az XML-kódot néhány kódtöredékek hello elrendezés határozza meg. Egy meglévő elrendezés megadott, a következő kód hello jelöli hello **ListView** azt szeretnénk, ha toopopulate a a kiszolgáló adataival.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

A kód megelőző hello, hello *listitem* attribútum hello listában az egyes sorok hello elrendezése hello azonosítóját adja meg. Ez a kód egy jelölőnégyzetet és a hozzá tartozó szöveg határozza meg, és egyszer lekérdezi példányosítani hello lista minden eleme. Ebben az elrendezésben nem jelennek meg hello **azonosító** mező, és egy összetettebb elrendezése lehet megadni további mezők hello jelennek meg. Ez a kód van hello **row_list_to_do.xml** fájlt.

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <a name="adapter"></a>Hello adapter meghatározása
Mivel a nézet hello adatforrás tömbje **ToDoItem**, azt alosztály az adapter egy **ArrayAdapter&lt;ToDoItem&gt;**  osztály. Az alosztály egy nézetet hoz létre minden **ToDoItem** hello segítségével **row_list_to_do** elrendezés.  A kódban, a következő osztály, amely a hello bővítménye hello meghatároztuk **ArrayAdapter&lt;E&gt;**  osztály:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Bírálja felül a hello adapterek **getView** metódust. Példa:

```
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

A Microsoft hozzon létre egy példányt az osztály a tevékenység az alábbiak szerint:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

hello második toohello ToDoItemAdapter konstruktort hivatkozás toohello elrendezés. A Microsoft most példányosítható hello **ListView** , és rendelje hozzá a hello adapter toohello **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Hello Adapter tooBind toohello felhasználói felület használata

Most már áll készen toouse adatkötés. hello következő kód bemutatja, hogyan hello tábla és kitöltés tooget elemeinek hello-helyi adapter hello elemet adott vissza.

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

Hívás hello adapter bármikor módosíthatja a hello **ToDoItem** tábla. Rekord által rekord alapon végzett módosításokat, mert egy gyűjtemény helyett egy sor kezeli. Egy elem beszúrásakor hívás hello **hozzáadása** metódus az hello adapter; törlésekor, hívja hello **eltávolítása** metódus.

Teljes példa található hello [Android Gyorsútmutató-projekt][21].

## <a name="inserting"></a>Adatok beszúrása hello háttér

Hello példányának példányosítható *ToDoItem* osztály és tulajdonságainak beállítása.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Ezután **az insert()** tooinsert objektum:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

hello adatokat adott vissza entitás egyezések hello beszúrt hello háttér tábla, belefoglalt hello azonosítója és egyéb értékek (például hello `createdAt`, `updatedAt`, és `version` mezők) hello háttérrendszer beállítása.

Mobile Apps táblák szükséges nevű elsődleges kulcsként megadott oszlop **azonosító**. Ez az oszlop karakterláncnak kell lennie. hello alapértelmezett hello azonosító oszlop értéke egy GUID Azonosítót.  Megadhatja, hogy más egyedi értékeket, például az e-mail címek vagy felhasználónevek. Egy beszúrt bejegyzés nincs megadva érvényes karakterlánc-azonosító értéke, hello háttér hoz létre egy új GUID Azonosítót.

Karakterlánc-azonosító értékek hello a következő előnyöket biztosítják:

* Azonosítók anélkül, hogy az oda-vissza toohello adatbázis hozhatók létre.
* Rekordok könnyebb toomerge különböző táblákhoz vagy adatbázisok.
* Azonosító értékek jobban integrálható egy alkalmazás logikáját.

Karakterlánc azonosító értékek a következők **REQUIRED** kapcsolat nélküli szinkronizálás támogatásához.  Egy azonosító nem módosítható, ha a hello háttérbeli adatbázis tárolja.

## <a name="updating"></a>A mobilalkalmazások adatok frissítése

egy tábla tooupdate adatok átadni hello új objektum toohello **az update()** metódust.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

Ebben a példában *elem* hello hivatkozás tooa sorában van *ToDoItem* táblázatot, amely az egyes módosítások tooit volt.  ugyanaz a hello sor hello **azonosító** frissül.

## <a name="deleting"></a>A mobilalkalmazások adatok törlése

hello a következő kód bemutatja, hogyan toodelete adatok megadásával egy táblázatból hello adatobjektum.

```java
mToDoTable
    .delete(item);
```

Egy elem hello megadásával is törölheti **azonosító** hello sor toodelete mezőjében.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Kereshet egy adott cikk azonosítója

Kereshet egy adott cikk **azonosító** mezőt a hello **lookUp()** módszert:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Útmutató: a típus nélküli adatok használata

hello típusos programozási modell lehetővé teszi a JSON-szerializálást pontos vezérlését.  Nincsenek olyan gyakori forgatókönyveket tartalmaz, ahol Kezdésként toouse egy típusos programozási modellt. Ha például a háttérbeli tábla sok oszlop szerepel, és csak akkor kell tooreference hello oszlopok csoportja.  hello típusos modell toodefine hello Mobile Apps-háttéralkalmazás az adatok osztályban definiált összes hello oszlopot igényel.  API-hívások adatainak eléréséhez hello többsége hasonló programozási hívások toohello írta-e be. hello fő különbség az, hogy hello típusos modellben indításakor hello metódusai **MobileServiceJsonTable** objektum helyett hello **MobileServiceTable** objektum.

### <a name="json_instance"></a>A típus nélküli tábla példányt létrehozni

Modell hasonló toohello írta-e be, akkor indítja el a táblahivatkozás, de ebben az esetben egy **MobileServicesJsonTable** objektum. Szerezze be hello hivatkozás hívó hello **getTable** hello ügyfél példányának metódust:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Miután létrehozta a hello példányának **MobileServiceJsonTable**, gyakorlatilag az rendelkezik hello azonos API érhető el, hello típusos programozási modellel. Bizonyos esetekben hello módszerek veszik egy Típusos paraméter egy Típusos paraméter helyett.

### <a name="json_insert"></a>A típus nélküli táblába beszúrása
a következő kód bemutatja hogyan hello toodo egy INSERT utasítás. hello első lépése az toocreate egy [JsonObject][1], amely része hello [gson] [ 3] könyvtárban.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Ezt követően **az insert()** tooinsert hello típusos objektum hello táblába.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Ha kell beszúrni hello objektum tooget hello azonosítója, akkor hello **getAsJsonPrimitive()** metódust.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>A típus nélküli táblából törlése
hello következő kód bemutatja, hogyan toodelete egy példányt, ebben az esetben hello ugyanazt a példányát egy **JsonObject** hello előtt létrehozott *beszúrása* példa. hello kódja hello: ugyanaz, mint a hello adta-e eset, de hello metódus aláírása egy másik óta az általa hivatkozott egy **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

Azonosítóval segítségével közvetlenül is törölhet egy példányát:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Visszaadja az összes sort típusos táblából
a következő kód bemutatja hogyan hello tooretrieve teljes táblát. Óta egy JSON-tábla használja, szelektív módon le csak néhány hello tábla oszlopait.

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

hello ugyanazokat a szűréshez, szűrési és lapozás hello típusos modell elérhető módszerek állnak rendelkezésre hello típusos modell.

## <a name="offline-sync"></a>Kapcsolat nélküli szinkronizálás megvalósítása

hello Azure Mobile Apps ügyfél SDK-t is egy SQLite-adatbázis toostore hello server adatok másolatát helyi használatával valósítja meg a kapcsolat nélküli szinkronizálását.  Egy kapcsolat nélküli táblán végrehajtott műveletek nem igényelnek mobil kapcsolat toowork.  Kapcsolat nélküli szinkronizálás segít az rugalmasság és a teljesítmény hello költségén olyan összetettebb logika ütközésfeloldáshoz.  hello Azure Mobile Apps-ügyfél SDK valósítja meg a következő funkciók hello:

* Növekményes szinkronizálás: Csak a frissített és új rekordok lesznek letöltve, sávszélesség- és memória-felhasználás mentése.
* Egyidejű hozzáférések optimista: Műveletek rendszer feltételezi, hogy toosucceed.  Ütközések feloldása késleltetve amíg frissítései hello kiszolgálón.
* Ütközések feloldása: hello SDK észleli, ha egy ütköző módosítás hello kiszolgálón történt, és itt tooalert hello felhasználó csatlakozik.
* Helyreállítható törlés: A törölt rekordok lesznek megjelölve törölt, így más eszközök tooupdate offline gyorsítótárukat.

### <a name="initialize-offline-sync"></a>Kapcsolat nélküli szinkronizálás inicializálása

Minden egyes offline tábla hello offline gyorsítótár használat előtt definiálni kell.  Általában a tábladefiníció hello ügyfél hello létrehozása után azonnal történik:

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with hello model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define hello table in hello local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize hello local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-toohello-offline-cache-table"></a>Egy hivatkozási toohello Offline gyorsítótár táblájának beszerzése

Egy online tábla használja `.getTable()`.  Egy kapcsolat nélküli tábla használja `.getSyncTable()`:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

Az összes hello felhasználható módszerek közül (beleértve a szűrési, rendezési, lapozás, adatok beszúrása, adatok frissítése és adatok törlése) online táblák egyaránt működik jól az online és offline táblákat.

### <a name="synchronize-hello-local-offline-cache"></a>Helyi kapcsolat nélküli gyorsítótár hello szinkronizálása

Az alkalmazás hello vezérlőben van a szinkronizálás.  Íme egy példa szinkronizálási módszert:

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

Ha a lekérdezés nevét megadva toohello `.pull(query, queryname)` módszer, majd a növekményes szinkronizálás használt tooreturn csak azt jelzi, hogy létrehozott vagy utolsó sikeresen hello lekéréses óta megváltozott.

### <a name="handle-conflicts-during-offline-synchronization"></a>Kapcsolat nélküli szinkronizálás során ütközések kezelésére

Ha ütközés során megtörténik egy `.push()` műveletet, a `MobileServiceConflictException` vált ki.   hello kiszolgáló által kiállított elem hello kivétel van beágyazva, és lekérhetők `.getItem()` hello kivétel miatt.  Állítsa be a hello leküldéses hívó hello elemek hello MobileServiceSyncContext objektum a következő szerint:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

Amennyiben az összes ütközésnél vannak megjelölve kívánja, hívja `.push()` újra tooresolve összes hello ütközések.

## <a name="custom-api"></a>Egy egyéni API hívása

Egy egyéni API lehetővé teszi a toodefine egyéni végpontokat visszaállítását kiszolgálói funkciók nem leképezése tooan insert, update, törlése vagy olvasási művelete. Egy egyéni API használatával is befolyásolni további üzenetküldés, beleértve az olvasási és HTTP-üzenet fejlécek beállítása meghatározó JSON nem üzenet törzsének formátumban.

Android-ügyfélről, meghívja a hello **invokeApi** metódus toocall hello egyéni API-végpont. hello következő példa bemutatja, hogyan toocall egy API-végpont nevű **completeAll**, egy gyűjtemény osztályt adja vissza, amely **MarkAllResult**.

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

Hello **invokeApi** módszer hello ügyfél, amely POST kérelem toohello új egyéni API neve. hello egyéni API által visszaadott hello eredmény jelenik meg egy üzenet párbeszédpanelen hibák. Egyéb verziói **invokeApi** lehetővé teszik, hogy opcionálisan hello kérés törzsében egy objektum küldését, adja meg a hello HTTP-metódus, és lekérdezési paraméterek hello kérelem küldése. Típus nélküli verziói **invokeApi** is találhatók.

## <a name="authentication"></a>Hitelesítési tooyour alkalmazás hozzáadása

Oktatóanyagok már részletesen leírja hogyan tooadd ezeket a szolgáltatásokat.

Támogatja az App Service [app felhasználók hitelesítéséhez](app-service-mobile-android-get-started-users.md) használatával különböző külső Identitásszolgáltatók: Facebook, a Google, a Microsoft Account, a Twitter és az Azure Active Directory. Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók. A kiszolgáló is használhatja hello identitás hitelesített felhasználók tooimplement az engedélyezési szabályok.

Két hitelesítési forgalom támogatottak: egy **server** folyamata és a **ügyfél** folyamata. hello kiszolgáló folyamata nyújt hello legegyszerűbb hitelesítési élmény hello identity providers webes felület támaszkodnak.  Nincsenek további SDK-k olyan szükséges tooimplement kiszolgálóhitelesítés folyamata. Kiszolgálóhitelesítés folyamata nem biztosít egy hello mobil eszközre való mély integráció, és csak ajánlott forgatókönyvek koncepció igazolása.

módon támaszkodnak az SDK-k hello identitásszolgáltató által biztosított eszközspecifikus képességekkel bővült, például egyszeri bejelentkezés szorosabb integrációt hello ügyféltanúsítvány folyamat teszi lehetővé.  Például hello Facebook SDK is integrálható a mobilalkalmazást.  hello mobil ügyfél cseréje hello Facebook-alkalmazásba, és megerősíti, hogy a bejelentkezés előtt vissza tooyour mobilalkalmazás csere.

Négy lépést az alkalmazás szükséges tooenable hitelesítési:

* Hitelesítés az alkalmazás regisztrálása egy identitásszolgáltatóval.
* App Service-háttéralkalmazásának konfigurálása.
* Tábla engedélyek tooauthenticated felhasználók csak az App Service-háttérrendszer hello korlátozása.
* Hitelesítési kód tooyour alkalmazás hozzáadása.

Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók. Is használhatja a SID-je egy hitelesített felhasználó toomodify hello kérelmeket.  További információkért tekintse át a [Bevezetés a hitelesítés használatába] és hello Server SDK útmutató dokumentációját.

### <a name="caching"></a>Hitelesítési: Server folyamat

hello alábbira elindítja a kiszolgáló folyamata bejelentkezési folyamatot hello Google-szolgáltató használatával.  Nincs szükség további konfigurációra hello Google szolgáltató hello biztonsági követelmények miatt:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Továbbá adja hozzá a következő metódus toohello fő tevékenységosztállyal hello:

```java
// You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check hello request code matches hello one we send in hello login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check hello error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

Hello `GOOGLE_LOGIN_REQUEST_CODE` a fő definiált tevékenységgel a hello `login()` metódus és hello belül `onActivityResult()` metódust.  Minden egyedi szám, mindaddig, amíg hello ugyanannyi belül használják hello választhat `login()` metódus és hello `onActivityResult()` metódust.  Hello Ügyfélkód szolgáltatás kártyához (a korábban bemutatott) tehetik absztrakttá, hello megfelelő módszereket hello szolgáltatás adapteren kell hívjuk.

Szüksége is tooconfigure hello projekt customtabs.  Először adja meg egy átirányítási URL.  Adja hozzá a következő kódrészletet túl hello`AndroidManifest.xml`:

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

Adja hozzá a hello **redirectUriScheme** toohello `build.gradle` fájl az alkalmazáshoz:

```text
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

Végül adja hozzá `com.android.support:customtabs:23.0.1` toohello alkalmazásfüggőségek listáján a hello `build.gradle` fájlt:

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.2.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

Hello azonosítója hello bejelentkezett felhasználó az beszerzése egy **MobileServiceUser** hello segítségével **getUserId** metódust. Hogyan toouse határidő toocall hello aszinkron bejelentkezési API-k példát talál [Bevezetés a hitelesítés használatába].

> [!WARNING]
> hello említett URL-séma a kis-és nagybetűket.  Győződjön meg arról, hogy minden előfordulását `{url_scheme_of_you_app}` nagybetű.

### <a name="caching"></a>A hitelesítési tokenek gyorsítótárazása

A hitelesítési tokenek gyorsítótárazás meg toostore hello felhasználói Azonosítót és a hitelesítési jogkivonat helyileg hello eszközön. hello hello alkalmazás elindul, amikor legközelebb bejelöli hello gyorsítótár, és ezek az értékek jelen, ha hello bejelentkezési eljárást kihagyhatja, és rehydrate hello ügyfél, és ezeket az adatokat. Azonban ezek az adatok bizalmas, és azt kell tárolni, titkosított biztonsági abban az esetben hello phone biztosítása.  Láthatja, hogy hogyan toocache hitelesítési jogkivonatok az átfogó példát [gyorsítótárazza a hitelesítési tokenek szakaszban][7].

Amikor toouse egy lejárt jogkivonatot, kap egy *401 nem engedélyezett* válasz. Hitelesítési hibák szűrők segítségével kezelheti.  Szűrők intercept kérelmek toohello App Service-háttérrendszer. hello kód teszteli a 401-es válasz hello hello bejelentkezési folyamat elindítja és majd tovább folytatja a 401-es hello létrehozó hello kérelem.

### <a name="refresh"></a>Használja a frissítési jogkivonatokat

hello Azure App Service hitelesítés és engedélyezés által visszaadott jogkivonatának egy meghatározott élettartama 1 óra.  Ezt követően újból hitelesítésre hello felhasználó.  Ha egy ügyfél-folyamat hitelesítési keresztül érkezett, akkor is hitelesítését hosszú élettartamú jogkivonatot használata az Azure App Service Authentication és engedélyezési használata hello ugyanezt a tokent.  Egy másik Azure App Service-jogkivonat új élettartamán jön létre.

Hello szolgáltató toouse frissítési jogkivonatok is rögzítheti.  A frissítési Token nem mindig rendelkezésre áll.  Nincs szükség további konfigurációra:

* A **Azure Active Directory**, a titkos ügyfélkulcsot az Azure Active Directory-alkalmazás hello konfigurálása.  Adja meg hello ügyfélkulcs hello Azure App Service, Azure Active Directory-hitelesítés konfigurálásakor.  Meghívásakor `.login()`, adja át `response_type=code id_token` paramétert:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* A **Google**, adja át a hello `access_type=offline` paramétert:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* A **Microsoft Account**, jelölje be hello `wl.offline_access` hatókör.

egy token toorefresh hívja `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

Ajánlott eljárásként hozzon létre egy szűrőt, amely észleli a 401-es válasz hello kiszolgálóról, és megpróbál toorefresh hello felhasználói jogkivonat.

## <a name="log-in-with-client-flow-authentication"></a>Jelentkezzen be az ügyféltanúsítvány-folyamat hitelesítés

hello általános folyamat az ügyfél-folyamat hitelesítési bejelentkezik a következőképpen történik:

* Azure App Service hitelesítés és engedélyezés konfigurálhatók, mint server-folyamat hitelesítés.
* Integrálni hello hitelesítési szolgáltató SDK a hitelesítési tooproduce olyan hozzáférési jogkivonatot.
* Hello hívás `.login()` módszert az alábbiak szerint:

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

Cserélje le a hello `onSuccess()` metódus függetlenül kódját, toouse kívánja a sikeres bejelentkezés.  Hello `{provider}` karakterlánca egy érvényes szolgáltatói: **aad** (az Azure Active Directory), **facebook**, **google**, **microsoftaccount**, vagy **twitter**.  Ha egyéni hitelesítési megvalósítását, majd is használhatja hello egyéni hitelesítési szolgáltató címkéje.

### <a name="adal"></a>Hitelesíti a felhasználókat az Active Directory Authentication Library (ADAL) hello

Az alkalmazás Azure Active Directory használatával hello Active Directory Authentication Library (ADAL) toosign felhasználók is használhatja. Egy ügyfél folyamata bejelentkezési használata gyakran érdemes toousing hello `loginAsync()` módszerek, mert több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.

1. A mobil-háttéralkalmazás számára az AAD-bejelentkezés konfigurálása következő hello [hogyan tooconfigure App Service az Active Directory bejelentkezési] [ 22] oktatóanyag. Győződjön meg arról, hogy toocomplete hello opcionális lépés egy natív ügyfélalkalmazás regisztrációján.
2. Telepítse az adal-t a build.gradle tooinclude hello, a következő definíciókat módosításával:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

1. Adja hozzá a következő kód tooyour alkalmazás biztosít, ami a következő cserékhez hello hello:

* Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** hello nevű hello bérlő, amelyben az alkalmazás kiépítve. hello formátumúnak kell lennie a https://login.microsoftonline.com/contoso.onmicrosoft.com.
* Cserélje le **INSERT-erőforrás-azonosító-Itt** hello ügyfél-azonosítójú a mobil-háttéralkalmazást. Ügyfél-azonosító hello szerezhet be a hello **speciális** lap **Azure Active Directory beállításai** hello portálon.
* Cserélje le **INSERT-ügyfél-azonosító-Itt** hello ügyfél-azonosítójú hello natív ügyfélalkalmazás másolta.
* Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont hello HTTPS protokollt használ. Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*.

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <a name="filters"></a>Ügyfél – kiszolgáló kommunikáció hello beállítása

hello ügyfélkapcsolat általában egy alapszintű hello az alapul szolgáló HTTP szalagtár pedig a hello Android SDK használatával HTTP-kapcsolat.  Miért van szüksége a toochange számos oka lehet, hogy:

* Egy másik HTTP könyvtár tooadjust időtúllépések kívánja toouse.
* Egy folyamatjelző kívánja tooprovide.
* Tooadd kívánja egy egyéni fejlécet toosupport API felügyeleti funkciót.
* Kívánja toointercept sikertelen választ, hogy újrahitelesítés valósíthat meg.
* Toolog kérelmek tooan analytics háttérszolgáltatást kívánja.

### <a name="using-an-alternate-http-library"></a>Egy másik HTTP-könyvtár használatával

Hello hívás `.setAndroidHttpClientFactory()` ügyfél referenciaként a létrehozása után azonnal metódust.  Például tooset hello kapcsolat időtúllépése too60 (mp) (helyett hello alapértelmezett érték 10 másodperc):

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClinet();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>A folyamatban lévő szűrő megvalósítása

Megvalósíthat egy intercept minden kérelem implementálásával egy `ServiceFilter`.  Hello következő például egy előre létrehozott folyamatjelző frissíti:

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    pubic void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

A szűrő toohello ügyfél lehet társítani a következőképpen:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>Kérelemfejléc testreszabása

Hello következő `ServiceFilter` , és csatlakoztassa a hello hello szűrő hello azonos módon `ProgressFilter`:

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <a name="conversions"></a>Automatikus szerializálási konfigurálása

Megadhatja a hello segítségével tooevery oszlop alkalmazó átalakítás stratégiát [gson] [ 3] API. hello Android ügyféloldali kódtár által használt [gson] [ 3] hello háttérben tooserialize Java-objektumokat tooJSON adatok tooAzure App Service hello adatok elküldése előtt.  hello következő használ a hello **setFieldNamingStrategy()** metódus tooset hello stratégia. Ez a példa törli hello kezdeti (az "m"), és karakter majd kisbetűs hello tovább, minden mező neve. Például azt szeretné ikonná "left" "id".  Egy konverzió stratégia tooreduce hello szükséges megvalósítása `SerializedName()` jegyzetek a legtöbb mező.

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStategy)
);
```

Ez a kód hello segítségével mobil ügyfél hivatkozás létrehozása előtt végre kell hajtani **MobileServiceClient**.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[Bevezetés a hitelesítés használatába]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: http://www.odata.org/documentation/odata-version-3-0/
[20]: http://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
