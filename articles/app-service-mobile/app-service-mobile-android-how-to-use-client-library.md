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
# <a name="how-toouse-hello-azure-mobile-apps-sdk-for-android"></a><span data-ttu-id="d3809-103">Hogyan toouse hello Azure Mobile Apps SDK Android</span><span class="sxs-lookup"><span data-stu-id="d3809-103">How toouse hello Azure Mobile Apps SDK for Android</span></span>

<span data-ttu-id="d3809-104">Ez az útmutató bemutatja, hogyan a toouse hello Android ügyfél SDK Mobile Apps tooimplement szabhatják, például:</span><span class="sxs-lookup"><span data-stu-id="d3809-104">This guide shows you how toouse hello Android client SDK for Mobile Apps tooimplement common scenarios, such as:</span></span>

* <span data-ttu-id="d3809-105">Lekérdezi az adatok (beszúrása, frissítése és törlése).</span><span class="sxs-lookup"><span data-stu-id="d3809-105">Querying for data (inserting, updating, and deleting).</span></span>
* <span data-ttu-id="d3809-106">Hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d3809-106">Authentication.</span></span>
* <span data-ttu-id="d3809-107">Kezeli a hibákat.</span><span class="sxs-lookup"><span data-stu-id="d3809-107">Handling errors.</span></span>
* <span data-ttu-id="d3809-108">Hello ügyfél testreszabása.</span><span class="sxs-lookup"><span data-stu-id="d3809-108">Customizing hello client.</span></span>

<span data-ttu-id="d3809-109">Ez az útmutató hello ügyféloldali Android SDK összpontosít.</span><span class="sxs-lookup"><span data-stu-id="d3809-109">This guide focuses on hello client-side Android SDK.</span></span>  <span data-ttu-id="d3809-110">toolearn arról hello kiszolgálóoldali SDK-k a Mobile Apps, lásd: [működéséhez a .NET-háttérrendszerrel SDK] [ 10] vagy [hogyan toouse hello Node.js-háttéralkalmazáshoz SDK] [ 11].</span><span class="sxs-lookup"><span data-stu-id="d3809-110">toolearn more about hello server-side SDKs for Mobile Apps, see [Work with .NET backend SDK][10] or [How toouse hello Node.js backend SDK][11].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="d3809-111">Referenciadokumentációt</span><span class="sxs-lookup"><span data-stu-id="d3809-111">Reference Documentation</span></span>

<span data-ttu-id="d3809-112">Hello található [Javadocs API-referencia] [ 12] a hello Android ügyféloldali kódtára a Githubon.</span><span class="sxs-lookup"><span data-stu-id="d3809-112">You can find hello [Javadocs API reference][12] for hello Android client library on GitHub.</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d3809-113">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="d3809-113">Supported Platforms</span></span>

<span data-ttu-id="d3809-114">hello Azure Mobile Apps SDK for Android API szintek 19-24 (KitKat nugát keresztül) helyigénnyel telefon és táblagép támogatja.</span><span class="sxs-lookup"><span data-stu-id="d3809-114">hello Azure Mobile Apps SDK for Android supports API levels 19 through 24 (KitKat through Nougat) for phone and tablet form factors.</span></span>  <span data-ttu-id="d3809-115">Hitelesítés, különösen, egy közös webes keretrendszer megközelítés toogather hitelesítő adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="d3809-115">Authentication, in particular, utilizes a common web framework approach toogather credentials.</span></span>  <span data-ttu-id="d3809-116">Kiszolgáló-folyamat hitelesítési kis űrlap tényező eszközök, például az órákat nem működik.</span><span class="sxs-lookup"><span data-stu-id="d3809-116">Server-flow authentication does not work with small form factor devices such as watches.</span></span>

## <a name="setup-and-prerequisites"></a><span data-ttu-id="d3809-117">A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d3809-117">Setup and Prerequisites</span></span>

<span data-ttu-id="d3809-118">Teljes hello [Mobile Apps gyors üzembe helyezés](app-service-mobile-android-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d3809-118">Complete hello [Mobile Apps quickstart](app-service-mobile-android-get-started.md) tutorial.</span></span>  <span data-ttu-id="d3809-119">Ez a feladat biztosítja, hogy a fejlesztés az Azure Mobile Apps minden előfeltétel teljesült.</span><span class="sxs-lookup"><span data-stu-id="d3809-119">This task ensures all pre-requisites for developing Azure Mobile Apps have been met.</span></span>  <span data-ttu-id="d3809-120">gyors üzembe helyezés hello is segítséget nyújt a fiókjának a konfigurálása és az első Mobile Apps-háttéralkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3809-120">hello Quickstart also helps you configure your account and create your first Mobile App backend.</span></span>

<span data-ttu-id="d3809-121">Ha úgy dönt, hogy nem toocomplete hello gyors üzembe helyezési útmutató, hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="d3809-121">If you decide not toocomplete hello Quickstart tutorial, complete hello following tasks:</span></span>

* <span data-ttu-id="d3809-122">[Mobile Apps-háttéralkalmazás létrehozása] [ 13] az androidos toouse.</span><span class="sxs-lookup"><span data-stu-id="d3809-122">[create a Mobile App backend][13] toouse with your Android app.</span></span>
* <span data-ttu-id="d3809-123">Az Android Studióban [frissítés hello Gradle build fájlok](#gradle-build).</span><span class="sxs-lookup"><span data-stu-id="d3809-123">In Android Studio, [update hello Gradle build files](#gradle-build).</span></span>
* <span data-ttu-id="d3809-124">[Engedélyezi az internet engedélycsoportot](#enable-internet).</span><span class="sxs-lookup"><span data-stu-id="d3809-124">[Enable internet permission](#enable-internet).</span></span>

### <span data-ttu-id="d3809-125"><a name="gradle-build"></a>Frissítés hello Gradle fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3809-125"><a name="gradle-build"></a>Update hello Gradle build file</span></span>

<span data-ttu-id="d3809-126">Mindkettőt módosíthatja **build.gradle** fájlok:</span><span class="sxs-lookup"><span data-stu-id="d3809-126">Change both **build.gradle** files:</span></span>

1. <span data-ttu-id="d3809-127">Adja hozzá a kódot toohello *projekt* szint **build.gradle** belüli hello *buildscript* címke:</span><span class="sxs-lookup"><span data-stu-id="d3809-127">Add this code toohello *Project* level **build.gradle** file inside hello *buildscript* tag:</span></span>

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. <span data-ttu-id="d3809-128">Adja hozzá a kódot toohello *modul app* szint **build.gradle** belüli hello *függőségek* címke:</span><span class="sxs-lookup"><span data-stu-id="d3809-128">Add this code toohello *Module app* level **build.gradle** file inside hello *dependencies* tag:</span></span>

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.3.0'
    ```

    <span data-ttu-id="d3809-129">Hello legújabb verziója jelenleg 3.3.0.</span><span class="sxs-lookup"><span data-stu-id="d3809-129">Currently hello latest version is 3.3.0.</span></span> <span data-ttu-id="d3809-130">felsorolt hello támogatott verziók [a Files][14].</span><span class="sxs-lookup"><span data-stu-id="d3809-130">hello supported versions are listed [on bintray][14].</span></span>

### <span data-ttu-id="d3809-131"><a name="enable-internet"></a>Internet engedélycsoportot engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d3809-131"><a name="enable-internet"></a>Enable internet permission</span></span>

<span data-ttu-id="d3809-132">tooaccess Azure, az alkalmazás lehetővé kell tenniük hello INTERNET engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d3809-132">tooaccess Azure, your app must have hello INTERNET permission enabled.</span></span> <span data-ttu-id="d3809-133">Ha még nincs engedélyezve, vegye fel a következő kód tooyour üzletági hello **AndroidManifest.xml** fájlt:</span><span class="sxs-lookup"><span data-stu-id="d3809-133">If it's not already enabled, add hello following line of code tooyour **AndroidManifest.xml** file:</span></span>

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a><span data-ttu-id="d3809-134">Egy ügyfél-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3809-134">Create a Client Connection</span></span>

<span data-ttu-id="d3809-135">Az Azure Mobile Apps tooyour mobilalkalmazás négy funkciókat biztosít:</span><span class="sxs-lookup"><span data-stu-id="d3809-135">Azure Mobile Apps provides four functions tooyour mobile application:</span></span>

* <span data-ttu-id="d3809-136">Adathozzáférés és -kapcsolat nélküli szinkronizálás az Azure Mobile Apps-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="d3809-136">Data Access and Offline Synchronization with an Azure Mobile Apps Service.</span></span>
* <span data-ttu-id="d3809-137">Azure Mobile Apps Server SDK hello készült egyéni API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="d3809-137">Call Custom APIs written with hello Azure Mobile Apps Server SDK.</span></span>
* <span data-ttu-id="d3809-138">Hitelesítés az Azure App Service-hitelesítéshez és engedélyezéshez.</span><span class="sxs-lookup"><span data-stu-id="d3809-138">Authentication with Azure App Service Authentication and Authorization.</span></span>
* <span data-ttu-id="d3809-139">Leküldéses értesítés regisztrálása a Notification hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="d3809-139">Push Notification Registration with Notification Hubs.</span></span>

<span data-ttu-id="d3809-140">Ezek először megköveteli, hogy hozzon létre egy `MobileServiceClient` objektum.</span><span class="sxs-lookup"><span data-stu-id="d3809-140">Each of these functions first requires that you create a `MobileServiceClient` object.</span></span>  <span data-ttu-id="d3809-141">Csak egy `MobileServiceClient` objektumot kell létrehozni a mobil ügyfél (Ez azt jelenti, hogy legyen egy egypéldányos mintát).</span><span class="sxs-lookup"><span data-stu-id="d3809-141">Only one `MobileServiceClient` object should be created within your mobile client (that is, it should be a Singleton pattern).</span></span>  <span data-ttu-id="d3809-142">toocreate egy `MobileServiceClient` objektum:</span><span class="sxs-lookup"><span data-stu-id="d3809-142">toocreate a `MobileServiceClient` object:</span></span>

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with hello Site URL
    this);                  // Your application Context
```

<span data-ttu-id="d3809-143">Hello `<MobileAppUrl>` egy karakterlánc vagy egy URL-cím objektum, amely tooyour mobil-háttéralkalmazást mutat.</span><span class="sxs-lookup"><span data-stu-id="d3809-143">hello `<MobileAppUrl>` is either a string or a URL object that points tooyour mobile backend.</span></span>  <span data-ttu-id="d3809-144">Ha az Azure App Service toohost a mobil-háttéralkalmazást használ, majd győződjön meg arról hogy biztonságos hello `https://` hello URL-cím verzióját.</span><span class="sxs-lookup"><span data-stu-id="d3809-144">If you are using Azure App Service toohost your mobile backend, then ensure you use hello secure `https://` version of hello URL.</span></span>

<span data-ttu-id="d3809-145">hello ügyfél is szükséges hozzáférés toohello tevékenység vagy a környezet – hello `this` hello példában paraméter.</span><span class="sxs-lookup"><span data-stu-id="d3809-145">hello client also requires access toohello Activity or Context - hello `this` parameter in hello example.</span></span>  <span data-ttu-id="d3809-146">hello MobileServiceClient konstrukció hello belül megtörténik `onCreate()` hello hivatkozott tevékenység hello metódusában `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="d3809-146">hello MobileServiceClient construction should happen within hello `onCreate()` method of hello Activity referenced in hello `AndroidManifest.xml` file.</span></span>

<span data-ttu-id="d3809-147">Ajánlott eljárásként a kiszolgáló közötti kommunikációt a saját (Egypéldányos – minta) osztályba kell absztrakt.</span><span class="sxs-lookup"><span data-stu-id="d3809-147">As a best practice, you should abstract server communication into its own (singleton-pattern) class.</span></span>  <span data-ttu-id="d3809-148">Ebben az esetben kell átadni hello tevékenység belül hello konstruktor tooappropriately hello szolgáltatás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="d3809-148">In this case, you should pass hello Activity within hello constructor tooappropriately configure hello service.</span></span>  <span data-ttu-id="d3809-149">Példa:</span><span class="sxs-lookup"><span data-stu-id="d3809-149">For example:</span></span>

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

<span data-ttu-id="d3809-150">Most hívása `AzureServiceAdapter.Initialize(this);` a hello `onCreate()` a fő tevékenységnél metódusában.</span><span class="sxs-lookup"><span data-stu-id="d3809-150">You can now call `AzureServiceAdapter.Initialize(this);` in hello `onCreate()` method of your main activity.</span></span>  <span data-ttu-id="d3809-151">Semmilyen más metódus hozzáférési toohello ügyfél kellene használni `AzureServiceAdapter.getInstance();` tooobtain egy hivatkozás toohello szolgáltatás adapter.</span><span class="sxs-lookup"><span data-stu-id="d3809-151">Any other methods needing access toohello client use `AzureServiceAdapter.getInstance();` tooobtain a reference toohello service adapter.</span></span>

## <a name="data-operations"></a><span data-ttu-id="d3809-152">Adatok műveletek</span><span class="sxs-lookup"><span data-stu-id="d3809-152">Data Operations</span></span>

<span data-ttu-id="d3809-153">az Azure Mobile Apps SDK hello hello core tooprovide hozzáférés toodata hello Mobile Apps-háttéralkalmazás SQL Azure-ban tárolt.</span><span class="sxs-lookup"><span data-stu-id="d3809-153">hello core of hello Azure Mobile Apps SDK is tooprovide access toodata stored within SQL Azure on hello Mobile App backend.</span></span>  <span data-ttu-id="d3809-154">Hozzáférnének ehhez ezen adatokhoz szigorú típusmegadású osztályokat (ajánlott) vagy típus nélküli lekérdezéseket (nem ajánlott).</span><span class="sxs-lookup"><span data-stu-id="d3809-154">You can access this data using strongly typed classes (preferred) or untyped queries (not recommended).</span></span>  <span data-ttu-id="d3809-155">a jelen szakasz hello tömeges foglalkozik szigorú típusmegadású osztályokat.</span><span class="sxs-lookup"><span data-stu-id="d3809-155">hello bulk of this section deals with using strongly typed classes.</span></span>

### <a name="define-client-data-classes"></a><span data-ttu-id="d3809-156">Ügyfél adatosztályok definiálása</span><span class="sxs-lookup"><span data-stu-id="d3809-156">Define client data classes</span></span>

<span data-ttu-id="d3809-157">SQL Azure-táblák, tooaccess adatait ügyfél adatok definiálására, amelyek megfelelnek a Mobile Apps-háttéralkalmazás hello toohello táblák.</span><span class="sxs-lookup"><span data-stu-id="d3809-157">tooaccess data from SQL Azure tables, define client data classes that correspond toohello tables in hello Mobile App backend.</span></span> <span data-ttu-id="d3809-158">Ebben a témakörben szereplő példák feltételezik nevű tábla **MyDataTable**, a következő oszlopok hello tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="d3809-158">Examples in this topic assume a table named **MyDataTable**, which has hello following columns:</span></span>

* <span data-ttu-id="d3809-159">id</span><span class="sxs-lookup"><span data-stu-id="d3809-159">id</span></span>
* <span data-ttu-id="d3809-160">Szöveg</span><span class="sxs-lookup"><span data-stu-id="d3809-160">text</span></span>
* <span data-ttu-id="d3809-161">Végezze el</span><span class="sxs-lookup"><span data-stu-id="d3809-161">complete</span></span>

<span data-ttu-id="d3809-162">hello megfelelő típusos ügyféloldali objektum található nevű fájlban **MyDataTable.java**:</span><span class="sxs-lookup"><span data-stu-id="d3809-162">hello corresponding typed client-side object resides in a file called **MyDataTable.java**:</span></span>

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

<span data-ttu-id="d3809-163">Adja hozzá az egyes mezők hozzáadott elérő és beállító metódusokat.</span><span class="sxs-lookup"><span data-stu-id="d3809-163">Add getter and setter methods for each field that you add.</span></span>  <span data-ttu-id="d3809-164">Ha az SQL Azure táblában több oszlopot tartalmaz, hello megfelelő mezőket toothis osztály szeretné beállítani.</span><span class="sxs-lookup"><span data-stu-id="d3809-164">If your SQL Azure table contains more columns, you would add hello corresponding fields toothis class.</span></span>  <span data-ttu-id="d3809-165">Például, ha hello DTO (adatobjektum átviteli) kellett szerepel egészszám-oszloppal prioritású virtuális gép, majd a mezőt, valamint beolvasó és beállító metódusa:</span><span class="sxs-lookup"><span data-stu-id="d3809-165">For example, if hello DTO (data transfer object) had an integer Priority column, then you might add this field, along with its getter and setter methods:</span></span>

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

<span data-ttu-id="d3809-166">Hogyan toocreate további táblákat a Mobile Apps-háttéralkalmazásának: toolearn [hogyan: Adja meg egy tábla vezérlő] [ 15] (.NET-háttérrendszer) vagy [dinamikus sémával definiálása táblák] [ 16] (Node.js háttérrendszer).</span><span class="sxs-lookup"><span data-stu-id="d3809-166">toolearn how toocreate additional tables in your Mobile Apps backend, see [How to: Define a table controller][15] (.NET backend) or [Define Tables using a Dynamic Schema][16] (Node.js backend).</span></span>

<span data-ttu-id="d3809-167">Egy Azure Mobile Apps-háttéralkalmazás tábla négyet elérhető tooclients öt különleges mező határozza meg:</span><span class="sxs-lookup"><span data-stu-id="d3809-167">An Azure Mobile Apps backend table defines five special fields, four of which are available tooclients:</span></span>

* <span data-ttu-id="d3809-168">`String id`: hello hello rekord globálisan egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d3809-168">`String id`: hello globally unique ID for hello record.</span></span>  <span data-ttu-id="d3809-169">Ajánlott eljárásként, ellenőrizze a hello azonosító hello karakterláncos ábrázolása egy [UUID] [ 17] objektum.</span><span class="sxs-lookup"><span data-stu-id="d3809-169">As a best practice, make hello id hello String representation of a [UUID][17] object.</span></span>
* <span data-ttu-id="d3809-170">`DateTimeOffset updatedAt`: hello hello utolsó frissítés időpontja.</span><span class="sxs-lookup"><span data-stu-id="d3809-170">`DateTimeOffset updatedAt`: hello date/time of hello last update.</span></span>  <span data-ttu-id="d3809-171">hello updatedAt mező hello kiszolgáló állítja be, és az Ügyfélkód soha nem kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="d3809-171">hello updatedAt field is set by hello server and should never be set by your client code.</span></span>
* <span data-ttu-id="d3809-172">`DateTimeOffset createdAt`: hello dátum/idő hello objektum lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="d3809-172">`DateTimeOffset createdAt`: hello date/time that hello object was created.</span></span>  <span data-ttu-id="d3809-173">hello createdAt mező hello kiszolgáló állítja be, és az Ügyfélkód soha nem kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="d3809-173">hello createdAt field is set by hello server and should never be set by your client code.</span></span>
* <span data-ttu-id="d3809-174">`byte[] version`: Megszokott módon jelenik meg egy karakterláncot, hello verziója is állítva hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d3809-174">`byte[] version`: Normally represented as a string, hello version is also set by hello server.</span></span>
* <span data-ttu-id="d3809-175">`boolean deleted`: Azt jelzi, hogy hello rekord törölve lett, de még nem törlődnek.</span><span class="sxs-lookup"><span data-stu-id="d3809-175">`boolean deleted`: Indicates that hello record has been deleted but not purged yet.</span></span>  <span data-ttu-id="d3809-176">Ne használjon `deleted` tulajdonságként a osztályban.</span><span class="sxs-lookup"><span data-stu-id="d3809-176">Do not use `deleted` as a property in your class.</span></span>

<span data-ttu-id="d3809-177">Hello `id` mező kitöltése kötelező.</span><span class="sxs-lookup"><span data-stu-id="d3809-177">hello `id` field is required.</span></span>  <span data-ttu-id="d3809-178">Hello `updatedAt` mező és `version` mező a kapcsolat nélküli szinkronizáláshoz használt (a növekményes szinkronizálás és az ütközés feloldásához rendre).</span><span class="sxs-lookup"><span data-stu-id="d3809-178">hello `updatedAt` field and `version` field are used for offline synchronization (for incremental sync and conflict resolution respectively).</span></span>  <span data-ttu-id="d3809-179">Hello `createdAt` mező hivatkozás mező, hello ügyfél nem használja.</span><span class="sxs-lookup"><span data-stu-id="d3809-179">hello `createdAt` field is a reference field and is not used by hello client.</span></span>  <span data-ttu-id="d3809-180">hello nevek hello tulajdonságainak "között tömörített" nevében és nem állítható.</span><span class="sxs-lookup"><span data-stu-id="d3809-180">hello names are "across-the-wire" names of hello properties and are not adjustable.</span></span>  <span data-ttu-id="d3809-181">Azonban hozhat létre az objektum és hello közötti leképezéseket "közötti átvitel közbeni" nevek használatával hello [gson] [ 3] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="d3809-181">However, you can create a mapping between your object and hello "across-the-wire" names using hello [gson][3] library.</span></span>  <span data-ttu-id="d3809-182">Példa:</span><span class="sxs-lookup"><span data-stu-id="d3809-182">For example:</span></span>

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

### <a name="create-a-table-reference"></a><span data-ttu-id="d3809-183">Egy tábla hivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3809-183">Create a Table Reference</span></span>

<span data-ttu-id="d3809-184">tooaccess tábla, először létre kell hoznia egy [MobileServiceTable] [ 8] objektum által hívó hello **getTable** hello metódusa [MobileServiceClient][9].</span><span class="sxs-lookup"><span data-stu-id="d3809-184">tooaccess a table, first create a [MobileServiceTable][8] object by calling hello **getTable** method on hello [MobileServiceClient][9].</span></span>  <span data-ttu-id="d3809-185">Ez a módszer két túlterheléssel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="d3809-185">This method has two overloads:</span></span>

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

<span data-ttu-id="d3809-186">A kódot, a következő hello **mClient** hivatkozás tooyour MobileServiceClient objektum.</span><span class="sxs-lookup"><span data-stu-id="d3809-186">In hello following code, **mClient** is a reference tooyour MobileServiceClient object.</span></span>  <span data-ttu-id="d3809-187">hello első túlterhelési használatos, ahol hello osztálynév és hello tábla neve is hello azonos, és hello egyik használatos hello gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="d3809-187">hello first overload is used where hello class name and hello table name are hello same, and is hello one used in hello Quickstart:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

<span data-ttu-id="d3809-188">hello második túlterhelési használja, amikor hello táblanév hello osztály nevétől eltérő: hello első paramétere hello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="d3809-188">hello second overload is used when hello table name is different from hello class name: hello first parameter is hello table name.</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <span data-ttu-id="d3809-189"><a name="query"></a>Egy háttér-táblából</span><span class="sxs-lookup"><span data-stu-id="d3809-189"><a name="query"></a>Query a Backend Table</span></span>

<span data-ttu-id="d3809-190">Először szerezze be a táblahivatkozás.</span><span class="sxs-lookup"><span data-stu-id="d3809-190">First, obtain a table reference.</span></span>  <span data-ttu-id="d3809-191">Majd hello táblahivatkozás a lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="d3809-191">Then execute a query on hello table reference.</span></span>  <span data-ttu-id="d3809-192">A lekérdezés bármilyen kombinációját:</span><span class="sxs-lookup"><span data-stu-id="d3809-192">A query is any combination of:</span></span>

* <span data-ttu-id="d3809-193">A `.where()` [szűrőfeltétel](#filtering).</span><span class="sxs-lookup"><span data-stu-id="d3809-193">A `.where()` [filter clause](#filtering).</span></span>
* <span data-ttu-id="d3809-194">Egy `.orderBy()` [záradék rendelési](#sorting).</span><span class="sxs-lookup"><span data-stu-id="d3809-194">An `.orderBy()` [ordering clause](#sorting).</span></span>
* <span data-ttu-id="d3809-195">A `.select()` [mező kiválasztása záradék](#selection).</span><span class="sxs-lookup"><span data-stu-id="d3809-195">A `.select()` [field selection clause](#selection).</span></span>
* <span data-ttu-id="d3809-196">A `.skip()` és `.top()` a [lapozható eredmények](#paging).</span><span class="sxs-lookup"><span data-stu-id="d3809-196">A `.skip()` and `.top()` for [paged results](#paging).</span></span>

<span data-ttu-id="d3809-197">rendelés megelőző hello biztosítani kell az hello záradékban.</span><span class="sxs-lookup"><span data-stu-id="d3809-197">hello clauses must be presented in hello preceding order.</span></span>

### <span data-ttu-id="d3809-198"><a name="filter"></a>Szűrési eredmények</span><span class="sxs-lookup"><span data-stu-id="d3809-198"><a name="filter"></a> Filtering Results</span></span>

<span data-ttu-id="d3809-199">hello általános a lekérdezések formátuma:</span><span class="sxs-lookup"><span data-stu-id="d3809-199">hello general form of a query is:</span></span>

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts hello async into a sync result
```

<span data-ttu-id="d3809-200">hello példában az összes eredményt visszaadja (felfelé toohello maximális méretének beállítása hello kiszolgáló).</span><span class="sxs-lookup"><span data-stu-id="d3809-200">hello preceding example returns all results (up toohello maximum page size set by hello server).</span></span>  <span data-ttu-id="d3809-201">Hello `.execute()` metódus hello lekérdezés végrehajtása a hello háttér.</span><span class="sxs-lookup"><span data-stu-id="d3809-201">hello `.execute()` method executes hello query on hello backend.</span></span>  <span data-ttu-id="d3809-202">hello lekérdezés az átalakított tooan [OData v3] [ 19] lekérdezés átviteli toohello Mobile Apps-háttéralkalmazás előtt.</span><span class="sxs-lookup"><span data-stu-id="d3809-202">hello query is converted tooan [OData v3][19] query before transmission toohello Mobile Apps backend.</span></span>  <span data-ttu-id="d3809-203">Kézhezvétele után hello Mobile Apps-háttéralkalmazás konvertálja hello lekérdezés előtt futtatnia kell a hello SQL Azure-példány SQL-utasítást.</span><span class="sxs-lookup"><span data-stu-id="d3809-203">On receipt, hello Mobile Apps backend converts hello query into an SQL statement before executing it on hello SQL Azure instance.</span></span>  <span data-ttu-id="d3809-204">Mivel a hálózati tevékenységek hosszabb időt vesz igénybe, hello `.execute()` metódus értéket ad vissza egy [ `ListenableFuture<E>` ] [ 18].</span><span class="sxs-lookup"><span data-stu-id="d3809-204">Since network activity takes some time, hello `.execute()` method returns a [`ListenableFuture<E>`][18].</span></span>

### <span data-ttu-id="d3809-205"><a name="filtering"></a>Visszaadott adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="d3809-205"><a name="filtering"></a>Filter returned data</span></span>

<span data-ttu-id="d3809-206">a következő lekérdezés-végrehajtás hello összes elemet ad vissza, hello **ToDoItem** where tábla **teljes** egyenlő **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d3809-206">hello following query execution returns all items from hello **ToDoItem** table where **complete** equals **false**.</span></span>

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

<span data-ttu-id="d3809-207">**mToDoTable** hello hivatkozás toohello mobilszolgáltatás táblázat, amely a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="d3809-207">**mToDoTable** is hello reference toohello mobile service table that we created previously.</span></span>

<span data-ttu-id="d3809-208">Hello használatával kapcsolódó szűrő megadásához **ahol** hello táblahivatkozás metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="d3809-208">Define a filter using hello **where** method call on hello table reference.</span></span> <span data-ttu-id="d3809-209">Hello **ahol** metódus követi a **mező** metódus egy metódust, amely meghatározza a hello logikai predikátum követ.</span><span class="sxs-lookup"><span data-stu-id="d3809-209">hello **where** method is followed by a **field** method followed by a method that specifies hello logical predicate.</span></span> <span data-ttu-id="d3809-210">Lehetséges predikátum módszerekre **eq** (egyenlő), **ne** (egyenlő), **gt** (több mint), **ge** (nagyobb vagy egyenlő), **lt** (kevesebb mint), **le** (kisebb vagy egyenlő).</span><span class="sxs-lookup"><span data-stu-id="d3809-210">Possible predicate methods include **eq** (equals), **ne** (not equal), **gt** (greater than), **ge** (greater than or equal to), **lt** (less than), **le** (less than or equal to).</span></span> <span data-ttu-id="d3809-211">Ezek a módszerek összehasonlítása számát és a karakterlánc mezők toospecific értékeket.</span><span class="sxs-lookup"><span data-stu-id="d3809-211">These methods let you compare number and string fields toospecific values.</span></span>

<span data-ttu-id="d3809-212">A dátumok végezhet.</span><span class="sxs-lookup"><span data-stu-id="d3809-212">You can filter on dates.</span></span> <span data-ttu-id="d3809-213">hello következőkkel lehetővé teszik, hogy összehasonlítható hello teljes dátummezője vagy hello dátum részei: **év**, **hónap**, **nap**, **óra**, **perc**, és **második**.</span><span class="sxs-lookup"><span data-stu-id="d3809-213">hello following methods let you compare hello entire date field or parts of hello date: **year**, **month**, **day**, **hour**, **minute**, and **second**.</span></span> <span data-ttu-id="d3809-214">hello következő példakóddal felveheti a cikkek szűrő amelynek *határidő* 2013 egyenlő.</span><span class="sxs-lookup"><span data-stu-id="d3809-214">hello following example adds a filter for items whose *due date* equals 2013.</span></span>

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

<span data-ttu-id="d3809-215">hello következőkkel támogatja a speciális szűrők karakterláncmezőket: **megadott módon kezdődő**, **megadott módon végződő**, **concat**, **subString**, **indexOf**, **cserélje le**, **toLower**, **toUpper**, **trim**, és  **hossz**.</span><span class="sxs-lookup"><span data-stu-id="d3809-215">hello following methods support complex filters on string fields: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim**, and **length**.</span></span> <span data-ttu-id="d3809-216">a tábla sort, amelyben hello a következő példa szűrők hello *szöveg* oszlopok kezdődő "PRI0."</span><span class="sxs-lookup"><span data-stu-id="d3809-216">hello following example filters for table rows where hello *text* column starts with "PRI0."</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="d3809-217">a mező támogatja a következő operátor módszerek hello: **hozzáadása**, **sub**, **MUL számú**, **div**, **mod**, **emelet**, **felső határ**, és **kerekíteni**.</span><span class="sxs-lookup"><span data-stu-id="d3809-217">hello following operator methods are supported on number fields: **add**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling**, and **round**.</span></span> <span data-ttu-id="d3809-218">a tábla sort, amelyben hello a következő példa szűrők hello **időtartam** páros szám-e.</span><span class="sxs-lookup"><span data-stu-id="d3809-218">hello following example filters for table rows where hello **duration** is an even number.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

<span data-ttu-id="d3809-219">A következő logikai módszerekkel kombinálhatja a predikátum: **és**, **vagy** és **nem**.</span><span class="sxs-lookup"><span data-stu-id="d3809-219">You can combine predicates with these logical methods: **and**, **or** and **not**.</span></span> <span data-ttu-id="d3809-220">a következő példa hello két példák megelőző hello egyesíti.</span><span class="sxs-lookup"><span data-stu-id="d3809-220">hello following example combines two of hello preceding examples.</span></span>

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

<span data-ttu-id="d3809-221">Logikai operátorok csoport és kivételblokkokra:</span><span class="sxs-lookup"><span data-stu-id="d3809-221">Group and nest logical operators:</span></span>

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

<span data-ttu-id="d3809-222">Az ismertető és példákat a szűrés további: [hello Android ügyfél lekérdezési modelljét hello gazdagsága felfedezése][20].</span><span class="sxs-lookup"><span data-stu-id="d3809-222">For more detailed discussion and examples of filtering, see [Exploring hello richness of hello Android client query model][20].</span></span>

### <span data-ttu-id="d3809-223"><a name="sorting"></a>A rendezési adatokat adott vissza</span><span class="sxs-lookup"><span data-stu-id="d3809-223"><a name="sorting"></a>Sort returned data</span></span>

<span data-ttu-id="d3809-224">hello következő kódot ad vissza az összes elem táblázatát **ToDoItems** növekvő sorrend szerint hello *szöveg* mező.</span><span class="sxs-lookup"><span data-stu-id="d3809-224">hello following code returns all items from a table of **ToDoItems** sorted ascending by hello *text* field.</span></span> <span data-ttu-id="d3809-225">*mToDoTable* hello hivatkozás toohello háttér táblázatot, amelyet korábban hozott létre:</span><span class="sxs-lookup"><span data-stu-id="d3809-225">*mToDoTable* is hello reference toohello backend table that you created previously:</span></span>

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

<span data-ttu-id="d3809-226">az első paraméter hello hello **orderBy** módszer a következő: a karakterlánc egyenlő toohello nevét, mely toosort hello mező.</span><span class="sxs-lookup"><span data-stu-id="d3809-226">hello first parameter of hello **orderBy** method is a string equal toohello name of hello field on which toosort.</span></span> <span data-ttu-id="d3809-227">a második paraméter hello használ hello **QueryOrder** számbavételi toospecify e toosort növekvő vagy csökkenő.</span><span class="sxs-lookup"><span data-stu-id="d3809-227">hello second parameter uses hello **QueryOrder** enumeration toospecify whether toosort ascending or descending.</span></span>  <span data-ttu-id="d3809-228">Szűrése hello használata esetén ***ahol*** metódust, hello ***ahol*** metódust kell meghívni, mielőtt hello ***orderBy*** metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-228">If you are filtering using hello ***where*** method, hello ***where*** method must be invoked before hello ***orderBy*** method.</span></span>

### <span data-ttu-id="d3809-229"><a name="selection"></a>Egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="d3809-229"><a name="selection"></a>Select specific columns</span></span>

<span data-ttu-id="d3809-230">hello alábbi kód bemutatja, hogyan összes tooreturn elemek-e a táblázatát **ToDoItems**, csak a hello jeleníti meg, de **teljes** és **szöveg** mezőket.</span><span class="sxs-lookup"><span data-stu-id="d3809-230">hello following code illustrates how tooreturn all items from a table of **ToDoItems**, but only displays hello **complete** and **text** fields.</span></span> <span data-ttu-id="d3809-231">**mToDoTable** hello hivatkozás toohello háttér táblázat, amely a korábban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="d3809-231">**mToDoTable** is hello reference toohello backend table that we created previously.</span></span>

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

<span data-ttu-id="d3809-232">hello paraméterek toohello válassza függvény hello karakterlánc neve, amelyet az tooreturn hello a táblázat oszlopaihoz tartozó.</span><span class="sxs-lookup"><span data-stu-id="d3809-232">hello parameters toohello select function are hello string names of hello table's columns that you want tooreturn.</span></span>  <span data-ttu-id="d3809-233">Hello **válasszon** metódust kell toofollow módszerek, például a **ahol** és **orderBy**.</span><span class="sxs-lookup"><span data-stu-id="d3809-233">hello **select** method needs toofollow methods like **where** and **orderBy**.</span></span> <span data-ttu-id="d3809-234">Lapozófájl módszerek, például követhetnek **kihagyása** és **felső**.</span><span class="sxs-lookup"><span data-stu-id="d3809-234">It can be followed by paging methods like **skip** and **top**.</span></span>

### <span data-ttu-id="d3809-235"><a name="paging"></a>A lapok visszatérési adatai</span><span class="sxs-lookup"><span data-stu-id="d3809-235"><a name="paging"></a>Return data in pages</span></span>

<span data-ttu-id="d3809-236">Adatok **mindig** lapok adott vissza.</span><span class="sxs-lookup"><span data-stu-id="d3809-236">Data is **ALWAYS** returned in pages.</span></span>  <span data-ttu-id="d3809-237">hello maximális száma hello kiszolgáló állítja be.</span><span class="sxs-lookup"><span data-stu-id="d3809-237">hello maximum number of records returned is set by hello server.</span></span>  <span data-ttu-id="d3809-238">Ha hello ügyfél további rekordok kér, hello kiszolgáló hello maximális számú rekordot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d3809-238">If hello client requests more records, then hello server returns hello maximum number of records.</span></span>  <span data-ttu-id="d3809-239">Alapértelmezés szerint a hello maximális lapméretét hello kiszolgálón 50 rögzíti.</span><span class="sxs-lookup"><span data-stu-id="d3809-239">By default, hello maximum page size on hello server is 50 records.</span></span>

<span data-ttu-id="d3809-240">hello első példa bemutatja, hogyan tooselect hello felső öt elemet egy táblázatban.</span><span class="sxs-lookup"><span data-stu-id="d3809-240">hello first example shows how tooselect hello top five items from a table.</span></span> <span data-ttu-id="d3809-241">hello lekérdezés táblázatát adja vissza hello elemek **ToDoItems**.</span><span class="sxs-lookup"><span data-stu-id="d3809-241">hello query returns hello items from a table of **ToDoItems**.</span></span> <span data-ttu-id="d3809-242">**mToDoTable** hello hivatkozás toohello háttér táblázatot, amelyet korábban hozott létre:</span><span class="sxs-lookup"><span data-stu-id="d3809-242">**mToDoTable** is hello reference toohello backend table that you created previously:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

<span data-ttu-id="d3809-243">Itt az a lekérdezés átugrása hello első öt elemeket, és ezután hello az értéket ad vissza a következő öt:</span><span class="sxs-lookup"><span data-stu-id="d3809-243">Here's a query that skips hello first five items, and then returns hello next five:</span></span>

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

<span data-ttu-id="d3809-244">Ha a tooget összes rekord a táblázatban, valósítja meg a kódot tooiterate keresztül az összes:</span><span class="sxs-lookup"><span data-stu-id="d3809-244">If you wish tooget all records in a table, implement code tooiterate over all pages:</span></span>

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

<span data-ttu-id="d3809-245">Ezzel a módszerrel az összes rekord kérelmet legalább két kérelmek toohello Mobile Apps-háttéralkalmazás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d3809-245">A request for all records using this method creates a minimum of two requests toohello Mobile Apps backend.</span></span>

> [!TIP]
> <span data-ttu-id="d3809-246">Választhatja hello jobb oldal mérete memóriahasználat hello kérelem melletti közben, a sávszélesség-használat és a hello adatfogadásra teljesen késleltetése egyensúlyára.</span><span class="sxs-lookup"><span data-stu-id="d3809-246">Choosing hello right page size is a balance between memory usage while hello request is happening, bandwidth usage and delay in receiving hello data completely.</span></span>  <span data-ttu-id="d3809-247">hello (50 rekordok) alapértelmezés szerint minden eszköz alkalmas.</span><span class="sxs-lookup"><span data-stu-id="d3809-247">hello default (50 records) is suitable for all devices.</span></span>  <span data-ttu-id="d3809-248">Kizárólag fog működni, a nagyobb memória-eszközökön, ha növelése too500 fel.</span><span class="sxs-lookup"><span data-stu-id="d3809-248">If you exclusively operate on larger memory devices, increase up too500.</span></span>  <span data-ttu-id="d3809-249">Találtunk, amelyeknek a növekvő hello oldalméret 500 rekordok eredmények túl elfogadható késések és nagy memóriaproblémák léptek fel.</span><span class="sxs-lookup"><span data-stu-id="d3809-249">We have found that increasing hello page size beyond 500 records results in unacceptable delays and large memory issues.</span></span>

### <span data-ttu-id="d3809-250"><a name="chaining"></a>Hogyan: összefűzésére lekérdezési módszerek</span><span class="sxs-lookup"><span data-stu-id="d3809-250"><a name="chaining"></a>How to: Concatenate query methods</span></span>

<span data-ttu-id="d3809-251">háttér táblák lekérdezésére hello módszerek összefűzendő is lehet.</span><span class="sxs-lookup"><span data-stu-id="d3809-251">hello methods used in querying backend tables can be concatenated.</span></span> <span data-ttu-id="d3809-252">Láncolás lekérdezés módszer lehetővé teszi a tooselect adott oszlopok rendezésének és lapozható szűrt sor.</span><span class="sxs-lookup"><span data-stu-id="d3809-252">Chaining query methods allows you tooselect specific columns of filtered rows that are sorted and paged.</span></span> <span data-ttu-id="d3809-253">Létrehozhat összetett logikai szűrőket.</span><span class="sxs-lookup"><span data-stu-id="d3809-253">You can create complex logical filters.</span></span>  <span data-ttu-id="d3809-254">Minden egyes lekérdezés módszer egy lekérdezés objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d3809-254">Each query method returns a Query object.</span></span> <span data-ttu-id="d3809-255">tooend hello azokat a módszereket, és ténylegesen futtatási hello lekérdezés, hívás hello **hajtható végre** metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-255">tooend hello series of methods and actually run hello query, call hello **execute** method.</span></span> <span data-ttu-id="d3809-256">Példa:</span><span class="sxs-lookup"><span data-stu-id="d3809-256">For example:</span></span>

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

<span data-ttu-id="d3809-257">hello kapcsolt lekérdezési módszerek az alábbiak szerint lehet rendezni:</span><span class="sxs-lookup"><span data-stu-id="d3809-257">hello chained query methods must be ordered as follows:</span></span>

1. <span data-ttu-id="d3809-258">Szűrés (**ahol**) módszerek.</span><span class="sxs-lookup"><span data-stu-id="d3809-258">Filtering (**where**) methods.</span></span>
2. <span data-ttu-id="d3809-259">Rendezés (**orderBy**) módszerek.</span><span class="sxs-lookup"><span data-stu-id="d3809-259">Sorting (**orderBy**) methods.</span></span>
3. <span data-ttu-id="d3809-260">Kijelölés (**válasszon**) módszerek.</span><span class="sxs-lookup"><span data-stu-id="d3809-260">Selection (**select**) methods.</span></span>
4. <span data-ttu-id="d3809-261">lapozás (**kihagyása** és **felső**) módszerek.</span><span class="sxs-lookup"><span data-stu-id="d3809-261">paging (**skip** and **top**) methods.</span></span>

## <span data-ttu-id="d3809-262"><a name="binding"></a>Köthető adatok toohello felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="d3809-262"><a name="binding"></a>Bind data toohello user interface</span></span>

<span data-ttu-id="d3809-263">Az adatkötés magában foglalja a három összetevővel:</span><span class="sxs-lookup"><span data-stu-id="d3809-263">Data binding involves three components:</span></span>

* <span data-ttu-id="d3809-264">hello adatforrás</span><span class="sxs-lookup"><span data-stu-id="d3809-264">hello data source</span></span>
* <span data-ttu-id="d3809-265">hello kezdőképernyő elrendezésének</span><span class="sxs-lookup"><span data-stu-id="d3809-265">hello screen layout</span></span>
* <span data-ttu-id="d3809-266">hello adapter, hogy ki hello két együtt.</span><span class="sxs-lookup"><span data-stu-id="d3809-266">hello adapter that ties hello two together.</span></span>

<span data-ttu-id="d3809-267">A minta kódban, a rendszer visszaadja a hello adatok hello Mobile Apps SQL Azure táblából **ToDoItem** egy tömbbe.</span><span class="sxs-lookup"><span data-stu-id="d3809-267">In our sample code, we return hello data from hello Mobile Apps SQL Azure table **ToDoItem** into an array.</span></span> <span data-ttu-id="d3809-268">Ez a tevékenység az adatok alkalmazások közös mintázatát.</span><span class="sxs-lookup"><span data-stu-id="d3809-268">This activity is a common pattern for data applications.</span></span>  <span data-ttu-id="d3809-269">Az adatbázis-lekérdezések gyakran kell visszaadnia azon sorait, amelyek hello listában vagy tömb kapja.</span><span class="sxs-lookup"><span data-stu-id="d3809-269">Database queries often return a collection of rows that hello client gets in a list or array.</span></span> <span data-ttu-id="d3809-270">Ez a példa hello tömbjének értéke hello adatforrás.</span><span class="sxs-lookup"><span data-stu-id="d3809-270">In this sample, hello array is hello data source.</span></span>  <span data-ttu-id="d3809-271">hello kód határozza meg a kezdőképernyő elrendezésének, amely meghatározza a hello nézetben megjelenő adatok hello hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="d3809-271">hello code specifies a screen layout that defines hello view of hello data that appears on hello device.</span></span>  <span data-ttu-id="d3809-272">hello két kötött együtt egy-adapternek, amely ezt a kódot a hello bővítménye **ArrayAdapter&lt;ToDoItem&gt;**  osztály.</span><span class="sxs-lookup"><span data-stu-id="d3809-272">hello two are bound together with an adapter, which in this code is an extension of hello **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span>

#### <span data-ttu-id="d3809-273"><a name="layout"></a>Adja meg a hello elrendezés</span><span class="sxs-lookup"><span data-stu-id="d3809-273"><a name="layout"></a>Define hello Layout</span></span>

<span data-ttu-id="d3809-274">az XML-kódot néhány kódtöredékek hello elrendezés határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d3809-274">hello layout is defined by several snippets of XML code.</span></span> <span data-ttu-id="d3809-275">Egy meglévő elrendezés megadott, a következő kód hello jelöli hello **ListView** azt szeretnénk, ha toopopulate a a kiszolgáló adataival.</span><span class="sxs-lookup"><span data-stu-id="d3809-275">Given an existing layout, hello following code represents hello **ListView** we want toopopulate with our server data.</span></span>

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

<span data-ttu-id="d3809-276">A kód megelőző hello, hello *listitem* attribútum hello listában az egyes sorok hello elrendezése hello azonosítóját adja meg.</span><span class="sxs-lookup"><span data-stu-id="d3809-276">In hello preceding code, hello *listitem* attribute specifies hello id of hello layout for an individual row in hello list.</span></span> <span data-ttu-id="d3809-277">Ez a kód egy jelölőnégyzetet és a hozzá tartozó szöveg határozza meg, és egyszer lekérdezi példányosítani hello lista minden eleme.</span><span class="sxs-lookup"><span data-stu-id="d3809-277">This code specifies a check box and its associated text and gets instantiated once for each item in hello list.</span></span> <span data-ttu-id="d3809-278">Ebben az elrendezésben nem jelennek meg hello **azonosító** mező, és egy összetettebb elrendezése lehet megadni további mezők hello jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d3809-278">This layout does not display hello **id** field, and a more complex layout would specify additional fields in hello display.</span></span> <span data-ttu-id="d3809-279">Ez a kód van hello **row_list_to_do.xml** fájlt.</span><span class="sxs-lookup"><span data-stu-id="d3809-279">This code is in hello **row_list_to_do.xml** file.</span></span>

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

#### <span data-ttu-id="d3809-280"><a name="adapter"></a>Hello adapter meghatározása</span><span class="sxs-lookup"><span data-stu-id="d3809-280"><a name="adapter"></a>Define hello adapter</span></span>
<span data-ttu-id="d3809-281">Mivel a nézet hello adatforrás tömbje **ToDoItem**, azt alosztály az adapter egy **ArrayAdapter&lt;ToDoItem&gt;**  osztály.</span><span class="sxs-lookup"><span data-stu-id="d3809-281">Since hello data source of our view is an array of **ToDoItem**, we subclass our adapter from an **ArrayAdapter&lt;ToDoItem&gt;** class.</span></span> <span data-ttu-id="d3809-282">Az alosztály egy nézetet hoz létre minden **ToDoItem** hello segítségével **row_list_to_do** elrendezés.</span><span class="sxs-lookup"><span data-stu-id="d3809-282">This subclass produces a View for every **ToDoItem** using hello **row_list_to_do** layout.</span></span>  <span data-ttu-id="d3809-283">A kódban, a következő osztály, amely a hello bővítménye hello meghatároztuk **ArrayAdapter&lt;E&gt;**  osztály:</span><span class="sxs-lookup"><span data-stu-id="d3809-283">In our code, we define hello following class that is an extension of hello **ArrayAdapter&lt;E&gt;** class:</span></span>

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

<span data-ttu-id="d3809-284">Bírálja felül a hello adapterek **getView** metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-284">Override hello adapters **getView** method.</span></span> <span data-ttu-id="d3809-285">Példa:</span><span class="sxs-lookup"><span data-stu-id="d3809-285">For example:</span></span>

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

<span data-ttu-id="d3809-286">A Microsoft hozzon létre egy példányt az osztály a tevékenység az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d3809-286">We create an instance of this class in our Activity as follows:</span></span>

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

<span data-ttu-id="d3809-287">hello második toohello ToDoItemAdapter konstruktort hivatkozás toohello elrendezés.</span><span class="sxs-lookup"><span data-stu-id="d3809-287">hello second parameter toohello ToDoItemAdapter constructor is a reference toohello layout.</span></span> <span data-ttu-id="d3809-288">A Microsoft most példányosítható hello **ListView** , és rendelje hozzá a hello adapter toohello **ListView**.</span><span class="sxs-lookup"><span data-stu-id="d3809-288">We can now instantiate hello **ListView** and assign hello adapter toohello **ListView**.</span></span>

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <span data-ttu-id="d3809-289"><a name="use-adapter"></a>Hello Adapter tooBind toohello felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="d3809-289"><a name="use-adapter"></a>Use hello Adapter tooBind toohello UI</span></span>

<span data-ttu-id="d3809-290">Most már áll készen toouse adatkötés.</span><span class="sxs-lookup"><span data-stu-id="d3809-290">You are now ready toouse data binding.</span></span> <span data-ttu-id="d3809-291">hello következő kód bemutatja, hogyan hello tábla és kitöltés tooget elemeinek hello-helyi adapter hello elemet adott vissza.</span><span class="sxs-lookup"><span data-stu-id="d3809-291">hello following code shows how tooget items in hello table and fills hello local adapter with hello returned items.</span></span>

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

<span data-ttu-id="d3809-292">Hívás hello adapter bármikor módosíthatja a hello **ToDoItem** tábla.</span><span class="sxs-lookup"><span data-stu-id="d3809-292">Call hello adapter any time you modify hello **ToDoItem** table.</span></span> <span data-ttu-id="d3809-293">Rekord által rekord alapon végzett módosításokat, mert egy gyűjtemény helyett egy sor kezeli.</span><span class="sxs-lookup"><span data-stu-id="d3809-293">Since modifications are done on a record by record basis, you handle a single row instead of a collection.</span></span> <span data-ttu-id="d3809-294">Egy elem beszúrásakor hívás hello **hozzáadása** metódus az hello adapter; törlésekor, hívja hello **eltávolítása** metódus.</span><span class="sxs-lookup"><span data-stu-id="d3809-294">When you insert an item, call hello **add** method on hello adapter; when deleting, call hello **remove** method.</span></span>

<span data-ttu-id="d3809-295">Teljes példa található hello [Android Gyorsútmutató-projekt][21].</span><span class="sxs-lookup"><span data-stu-id="d3809-295">You can find a complete example in hello [Android Quickstart Project][21].</span></span>

## <span data-ttu-id="d3809-296"><a name="inserting"></a>Adatok beszúrása hello háttér</span><span class="sxs-lookup"><span data-stu-id="d3809-296"><a name="inserting"></a>Insert data into hello backend</span></span>

<span data-ttu-id="d3809-297">Hello példányának példányosítható *ToDoItem* osztály és tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="d3809-297">Instantiate an instance of hello *ToDoItem* class and set its properties.</span></span>

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

<span data-ttu-id="d3809-298">Ezután **az insert()** tooinsert objektum:</span><span class="sxs-lookup"><span data-stu-id="d3809-298">Then use **insert()** tooinsert an object:</span></span>

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="d3809-299">hello adatokat adott vissza entitás egyezések hello beszúrt hello háttér tábla, belefoglalt hello azonosítója és egyéb értékek (például hello `createdAt`, `updatedAt`, és `version` mezők) hello háttérrendszer beállítása.</span><span class="sxs-lookup"><span data-stu-id="d3809-299">hello returned entity matches hello data inserted into hello backend table, included hello ID and any other values (such as hello `createdAt`, `updatedAt`, and `version` fields) set on hello backend.</span></span>

<span data-ttu-id="d3809-300">Mobile Apps táblák szükséges nevű elsődleges kulcsként megadott oszlop **azonosító**. Ez az oszlop karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d3809-300">Mobile Apps tables require a primary key column named **id**. This column must be a string.</span></span> <span data-ttu-id="d3809-301">hello alapértelmezett hello azonosító oszlop értéke egy GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d3809-301">hello default value of hello ID column is a GUID.</span></span>  <span data-ttu-id="d3809-302">Megadhatja, hogy más egyedi értékeket, például az e-mail címek vagy felhasználónevek.</span><span class="sxs-lookup"><span data-stu-id="d3809-302">You can provide other unique values, such as email addresses or usernames.</span></span> <span data-ttu-id="d3809-303">Egy beszúrt bejegyzés nincs megadva érvényes karakterlánc-azonosító értéke, hello háttér hoz létre egy új GUID Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d3809-303">When a string ID value is not provided for an inserted record, hello backend generates a new GUID.</span></span>

<span data-ttu-id="d3809-304">Karakterlánc-azonosító értékek hello a következő előnyöket biztosítják:</span><span class="sxs-lookup"><span data-stu-id="d3809-304">String ID values provide hello following advantages:</span></span>

* <span data-ttu-id="d3809-305">Azonosítók anélkül, hogy az oda-vissza toohello adatbázis hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="d3809-305">IDs can be generated without making a round trip toohello database.</span></span>
* <span data-ttu-id="d3809-306">Rekordok könnyebb toomerge különböző táblákhoz vagy adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="d3809-306">Records are easier toomerge from different tables or databases.</span></span>
* <span data-ttu-id="d3809-307">Azonosító értékek jobban integrálható egy alkalmazás logikáját.</span><span class="sxs-lookup"><span data-stu-id="d3809-307">ID values integrate better with an application's logic.</span></span>

<span data-ttu-id="d3809-308">Karakterlánc azonosító értékek a következők **REQUIRED** kapcsolat nélküli szinkronizálás támogatásához.</span><span class="sxs-lookup"><span data-stu-id="d3809-308">String ID values are **REQUIRED** for offline sync support.</span></span>  <span data-ttu-id="d3809-309">Egy azonosító nem módosítható, ha a hello háttérbeli adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="d3809-309">You cannot change an Id once it is stored in hello backend database.</span></span>

## <span data-ttu-id="d3809-310"><a name="updating"></a>A mobilalkalmazások adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="d3809-310"><a name="updating"></a>Update data in a mobile app</span></span>

<span data-ttu-id="d3809-311">egy tábla tooupdate adatok átadni hello új objektum toohello **az update()** metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-311">tooupdate data in a table, pass hello new object toohello **update()** method.</span></span>

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

<span data-ttu-id="d3809-312">Ebben a példában *elem* hello hivatkozás tooa sorában van *ToDoItem* táblázatot, amely az egyes módosítások tooit volt.</span><span class="sxs-lookup"><span data-stu-id="d3809-312">In this example, *item* is a reference tooa row in hello *ToDoItem* table, which has had some changes made tooit.</span></span>  <span data-ttu-id="d3809-313">ugyanaz a hello sor hello **azonosító** frissül.</span><span class="sxs-lookup"><span data-stu-id="d3809-313">hello row with hello same **id** is updated.</span></span>

## <span data-ttu-id="d3809-314"><a name="deleting"></a>A mobilalkalmazások adatok törlése</span><span class="sxs-lookup"><span data-stu-id="d3809-314"><a name="deleting"></a>Delete data in a mobile app</span></span>

<span data-ttu-id="d3809-315">hello a következő kód bemutatja, hogyan toodelete adatok megadásával egy táblázatból hello adatobjektum.</span><span class="sxs-lookup"><span data-stu-id="d3809-315">hello following code shows how toodelete data from a table by specifying hello data object.</span></span>

```java
mToDoTable
    .delete(item);
```

<span data-ttu-id="d3809-316">Egy elem hello megadásával is törölheti **azonosító** hello sor toodelete mezőjében.</span><span class="sxs-lookup"><span data-stu-id="d3809-316">You can also delete an item by specifying hello **id** field of hello row toodelete.</span></span>

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <span data-ttu-id="d3809-317"><a name="lookup"></a>Kereshet egy adott cikk azonosítója</span><span class="sxs-lookup"><span data-stu-id="d3809-317"><a name="lookup"></a>Look up a specific item by Id</span></span>

<span data-ttu-id="d3809-318">Kereshet egy adott cikk **azonosító** mezőt a hello **lookUp()** módszert:</span><span class="sxs-lookup"><span data-stu-id="d3809-318">Look up an item with a specific **id** field with hello **lookUp()** method:</span></span>

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <span data-ttu-id="d3809-319"><a name="untyped"></a>Útmutató: a típus nélküli adatok használata</span><span class="sxs-lookup"><span data-stu-id="d3809-319"><a name="untyped"></a>How to: Work with untyped data</span></span>

<span data-ttu-id="d3809-320">hello típusos programozási modell lehetővé teszi a JSON-szerializálást pontos vezérlését.</span><span class="sxs-lookup"><span data-stu-id="d3809-320">hello untyped programming model gives you exact control over JSON serialization.</span></span>  <span data-ttu-id="d3809-321">Nincsenek olyan gyakori forgatókönyveket tartalmaz, ahol Kezdésként toouse egy típusos programozási modellt.</span><span class="sxs-lookup"><span data-stu-id="d3809-321">There are some common scenarios where you may wish toouse an untyped programming model.</span></span> <span data-ttu-id="d3809-322">Ha például a háttérbeli tábla sok oszlop szerepel, és csak akkor kell tooreference hello oszlopok csoportja.</span><span class="sxs-lookup"><span data-stu-id="d3809-322">For example, if your backend table contains many columns and you only need tooreference a subset of hello columns.</span></span>  <span data-ttu-id="d3809-323">hello típusos modell toodefine hello Mobile Apps-háttéralkalmazás az adatok osztályban definiált összes hello oszlopot igényel.</span><span class="sxs-lookup"><span data-stu-id="d3809-323">hello typed model requires you toodefine all hello columns defined in hello Mobile Apps backend in your data class.</span></span>  <span data-ttu-id="d3809-324">API-hívások adatainak eléréséhez hello többsége hasonló programozási hívások toohello írta-e be.</span><span class="sxs-lookup"><span data-stu-id="d3809-324">Most of hello API calls for accessing data are similar toohello typed programming calls.</span></span> <span data-ttu-id="d3809-325">hello fő különbség az, hogy hello típusos modellben indításakor hello metódusai **MobileServiceJsonTable** objektum helyett hello **MobileServiceTable** objektum.</span><span class="sxs-lookup"><span data-stu-id="d3809-325">hello main difference is that in hello untyped model you invoke methods on hello **MobileServiceJsonTable** object, instead of hello **MobileServiceTable** object.</span></span>

### <span data-ttu-id="d3809-326"><a name="json_instance"></a>A típus nélküli tábla példányt létrehozni</span><span class="sxs-lookup"><span data-stu-id="d3809-326"><a name="json_instance"></a>Create an instance of an untyped table</span></span>

<span data-ttu-id="d3809-327">Modell hasonló toohello írta-e be, akkor indítja el a táblahivatkozás, de ebben az esetben egy **MobileServicesJsonTable** objektum.</span><span class="sxs-lookup"><span data-stu-id="d3809-327">Similar toohello typed model, you start by getting a table reference, but in this case it's a **MobileServicesJsonTable** object.</span></span> <span data-ttu-id="d3809-328">Szerezze be hello hivatkozás hívó hello **getTable** hello ügyfél példányának metódust:</span><span class="sxs-lookup"><span data-stu-id="d3809-328">Obtain hello reference by calling hello **getTable** method on an instance of hello client:</span></span>

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

<span data-ttu-id="d3809-329">Miután létrehozta a hello példányának **MobileServiceJsonTable**, gyakorlatilag az rendelkezik hello azonos API érhető el, hello típusos programozási modellel.</span><span class="sxs-lookup"><span data-stu-id="d3809-329">Once you have created an instance of hello **MobileServiceJsonTable**, it has virtually hello same API available as with hello typed programming model.</span></span> <span data-ttu-id="d3809-330">Bizonyos esetekben hello módszerek veszik egy Típusos paraméter egy Típusos paraméter helyett.</span><span class="sxs-lookup"><span data-stu-id="d3809-330">In some cases, hello methods take an untyped parameter instead of a typed parameter.</span></span>

### <span data-ttu-id="d3809-331"><a name="json_insert"></a>A típus nélküli táblába beszúrása</span><span class="sxs-lookup"><span data-stu-id="d3809-331"><a name="json_insert"></a>Insert into an untyped table</span></span>
<span data-ttu-id="d3809-332">a következő kód bemutatja hogyan hello toodo egy INSERT utasítás.</span><span class="sxs-lookup"><span data-stu-id="d3809-332">hello following code shows how toodo an insert.</span></span> <span data-ttu-id="d3809-333">hello első lépése az toocreate egy [JsonObject][1], amely része hello [gson] [ 3] könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="d3809-333">hello first step is toocreate a [JsonObject][1], which is part of hello [gson][3] library.</span></span>

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

<span data-ttu-id="d3809-334">Ezt követően **az insert()** tooinsert hello típusos objektum hello táblába.</span><span class="sxs-lookup"><span data-stu-id="d3809-334">Then, Use **insert()** tooinsert hello untyped object into hello table.</span></span>

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

<span data-ttu-id="d3809-335">Ha kell beszúrni hello objektum tooget hello azonosítója, akkor hello **getAsJsonPrimitive()** metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-335">If you need tooget hello ID of hello inserted object, use hello **getAsJsonPrimitive()** method.</span></span>

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <span data-ttu-id="d3809-336"><a name="json_delete"></a>A típus nélküli táblából törlése</span><span class="sxs-lookup"><span data-stu-id="d3809-336"><a name="json_delete"></a>Delete from an untyped table</span></span>
<span data-ttu-id="d3809-337">hello következő kód bemutatja, hogyan toodelete egy példányt, ebben az esetben hello ugyanazt a példányát egy **JsonObject** hello előtt létrehozott *beszúrása* példa.</span><span class="sxs-lookup"><span data-stu-id="d3809-337">hello following code shows how toodelete an instance, in this case, hello same instance of a **JsonObject** that was created in hello prior *insert* example.</span></span> <span data-ttu-id="d3809-338">hello kódja hello: ugyanaz, mint a hello adta-e eset, de hello metódus aláírása egy másik óta az általa hivatkozott egy **JsonObject**.</span><span class="sxs-lookup"><span data-stu-id="d3809-338">hello code is hello same as with hello typed case, but hello method has a different signature since it references an **JsonObject**.</span></span>

```java
mToDoTable
    .delete(insertedItem);
```

<span data-ttu-id="d3809-339">Azonosítóval segítségével közvetlenül is törölhet egy példányát:</span><span class="sxs-lookup"><span data-stu-id="d3809-339">You can also delete an instance directly by using its ID:</span></span>

```java
mToDoTable.delete(ID);
```

### <span data-ttu-id="d3809-340"><a name="json_get"></a>Visszaadja az összes sort típusos táblából</span><span class="sxs-lookup"><span data-stu-id="d3809-340"><a name="json_get"></a>Return all rows from an untyped table</span></span>
<span data-ttu-id="d3809-341">a következő kód bemutatja hogyan hello tooretrieve teljes táblát.</span><span class="sxs-lookup"><span data-stu-id="d3809-341">hello following code shows how tooretrieve an entire table.</span></span> <span data-ttu-id="d3809-342">Óta egy JSON-tábla használja, szelektív módon le csak néhány hello tábla oszlopait.</span><span class="sxs-lookup"><span data-stu-id="d3809-342">Since you are using a JSON Table, you can selectively retrieve only some of hello table's columns.</span></span>

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

<span data-ttu-id="d3809-343">hello ugyanazokat a szűréshez, szűrési és lapozás hello típusos modell elérhető módszerek állnak rendelkezésre hello típusos modell.</span><span class="sxs-lookup"><span data-stu-id="d3809-343">hello same set of filtering, filtering and paging methods that are available for hello typed model are available for hello untyped model.</span></span>

## <span data-ttu-id="d3809-344"><a name="offline-sync"></a>Kapcsolat nélküli szinkronizálás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="d3809-344"><a name="offline-sync"></a>Implement Offline Sync</span></span>

<span data-ttu-id="d3809-345">hello Azure Mobile Apps ügyfél SDK-t is egy SQLite-adatbázis toostore hello server adatok másolatát helyi használatával valósítja meg a kapcsolat nélküli szinkronizálását.</span><span class="sxs-lookup"><span data-stu-id="d3809-345">hello Azure Mobile Apps Client SDK also implements offline synchronization of data by using a SQLite database toostore a copy of hello server data locally.</span></span>  <span data-ttu-id="d3809-346">Egy kapcsolat nélküli táblán végrehajtott műveletek nem igényelnek mobil kapcsolat toowork.</span><span class="sxs-lookup"><span data-stu-id="d3809-346">Operations performed on an offline table do not require mobile connectivity toowork.</span></span>  <span data-ttu-id="d3809-347">Kapcsolat nélküli szinkronizálás segít az rugalmasság és a teljesítmény hello költségén olyan összetettebb logika ütközésfeloldáshoz.</span><span class="sxs-lookup"><span data-stu-id="d3809-347">Offline sync aids in resilience and performance at hello expense of more complex logic for conflict resolution.</span></span>  <span data-ttu-id="d3809-348">hello Azure Mobile Apps-ügyfél SDK valósítja meg a következő funkciók hello:</span><span class="sxs-lookup"><span data-stu-id="d3809-348">hello Azure Mobile Apps Client SDK implements hello following features:</span></span>

* <span data-ttu-id="d3809-349">Növekményes szinkronizálás: Csak a frissített és új rekordok lesznek letöltve, sávszélesség- és memória-felhasználás mentése.</span><span class="sxs-lookup"><span data-stu-id="d3809-349">Incremental Sync: Only updated and new records are downloaded, saving bandwidth and memory consumption.</span></span>
* <span data-ttu-id="d3809-350">Egyidejű hozzáférések optimista: Műveletek rendszer feltételezi, hogy toosucceed.</span><span class="sxs-lookup"><span data-stu-id="d3809-350">Optimistic Concurrency: Operations are assumed toosucceed.</span></span>  <span data-ttu-id="d3809-351">Ütközések feloldása késleltetve amíg frissítései hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="d3809-351">Conflict Resolution is deferred until updates are performed on hello server.</span></span>
* <span data-ttu-id="d3809-352">Ütközések feloldása: hello SDK észleli, ha egy ütköző módosítás hello kiszolgálón történt, és itt tooalert hello felhasználó csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="d3809-352">Conflict Resolution: hello SDK detects when a conflicting change has been made at hello server and provides hooks tooalert hello user.</span></span>
* <span data-ttu-id="d3809-353">Helyreállítható törlés: A törölt rekordok lesznek megjelölve törölt, így más eszközök tooupdate offline gyorsítótárukat.</span><span class="sxs-lookup"><span data-stu-id="d3809-353">Soft Delete: Deleted records are marked deleted, allowing other devices tooupdate their offline cache.</span></span>

### <a name="initialize-offline-sync"></a><span data-ttu-id="d3809-354">Kapcsolat nélküli szinkronizálás inicializálása</span><span class="sxs-lookup"><span data-stu-id="d3809-354">Initialize Offline Sync</span></span>

<span data-ttu-id="d3809-355">Minden egyes offline tábla hello offline gyorsítótár használat előtt definiálni kell.</span><span class="sxs-lookup"><span data-stu-id="d3809-355">Each offline table must be defined in hello offline cache before use.</span></span>  <span data-ttu-id="d3809-356">Általában a tábladefiníció hello ügyfél hello létrehozása után azonnal történik:</span><span class="sxs-lookup"><span data-stu-id="d3809-356">Normally, table definition is done immediately after hello creation of hello client:</span></span>

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

### <a name="obtain-a-reference-toohello-offline-cache-table"></a><span data-ttu-id="d3809-357">Egy hivatkozási toohello Offline gyorsítótár táblájának beszerzése</span><span class="sxs-lookup"><span data-stu-id="d3809-357">Obtain a reference toohello Offline Cache Table</span></span>

<span data-ttu-id="d3809-358">Egy online tábla használja `.getTable()`.</span><span class="sxs-lookup"><span data-stu-id="d3809-358">For an online table, you use `.getTable()`.</span></span>  <span data-ttu-id="d3809-359">Egy kapcsolat nélküli tábla használja `.getSyncTable()`:</span><span class="sxs-lookup"><span data-stu-id="d3809-359">For an offline table, use `.getSyncTable()`:</span></span>

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

<span data-ttu-id="d3809-360">Az összes hello felhasználható módszerek közül (beleértve a szűrési, rendezési, lapozás, adatok beszúrása, adatok frissítése és adatok törlése) online táblák egyaránt működik jól az online és offline táblákat.</span><span class="sxs-lookup"><span data-stu-id="d3809-360">All hello methods that are available for online tables (including filtering, sorting, paging, inserting data, updating data, and deleting data) work equally well on online and offline tables.</span></span>

### <a name="synchronize-hello-local-offline-cache"></a><span data-ttu-id="d3809-361">Helyi kapcsolat nélküli gyorsítótár hello szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="d3809-361">Synchronize hello Local Offline Cache</span></span>

<span data-ttu-id="d3809-362">Az alkalmazás hello vezérlőben van a szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="d3809-362">Synchronization is within hello control of your app.</span></span>  <span data-ttu-id="d3809-363">Íme egy példa szinkronizálási módszert:</span><span class="sxs-lookup"><span data-stu-id="d3809-363">Here is an example synchronization method:</span></span>

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

<span data-ttu-id="d3809-364">Ha a lekérdezés nevét megadva toohello `.pull(query, queryname)` módszer, majd a növekményes szinkronizálás használt tooreturn csak azt jelzi, hogy létrehozott vagy utolsó sikeresen hello lekéréses óta megváltozott.</span><span class="sxs-lookup"><span data-stu-id="d3809-364">If a query name is provided toohello `.pull(query, queryname)` method, then incremental sync is used tooreturn only records that have been created or changed since hello last successfully completed pull.</span></span>

### <a name="handle-conflicts-during-offline-synchronization"></a><span data-ttu-id="d3809-365">Kapcsolat nélküli szinkronizálás során ütközések kezelésére</span><span class="sxs-lookup"><span data-stu-id="d3809-365">Handle Conflicts during Offline Synchronization</span></span>

<span data-ttu-id="d3809-366">Ha ütközés során megtörténik egy `.push()` műveletet, a `MobileServiceConflictException` vált ki.</span><span class="sxs-lookup"><span data-stu-id="d3809-366">If a conflict happens during a `.push()` operation, a `MobileServiceConflictException` is thrown.</span></span>   <span data-ttu-id="d3809-367">hello kiszolgáló által kiállított elem hello kivétel van beágyazva, és lekérhetők `.getItem()` hello kivétel miatt.</span><span class="sxs-lookup"><span data-stu-id="d3809-367">hello server-issued item is embedded in hello exception and can be retrieved by `.getItem()` on hello exception.</span></span>  <span data-ttu-id="d3809-368">Állítsa be a hello leküldéses hívó hello elemek hello MobileServiceSyncContext objektum a következő szerint:</span><span class="sxs-lookup"><span data-stu-id="d3809-368">Adjust hello push by calling hello following items on hello MobileServiceSyncContext object:</span></span>

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

<span data-ttu-id="d3809-369">Amennyiben az összes ütközésnél vannak megjelölve kívánja, hívja `.push()` újra tooresolve összes hello ütközések.</span><span class="sxs-lookup"><span data-stu-id="d3809-369">Once all conflicts are marked as you wish, call `.push()` again tooresolve all hello conflicts.</span></span>

## <span data-ttu-id="d3809-370"><a name="custom-api"></a>Egy egyéni API hívása</span><span class="sxs-lookup"><span data-stu-id="d3809-370"><a name="custom-api"></a>Call a custom API</span></span>

<span data-ttu-id="d3809-371">Egy egyéni API lehetővé teszi a toodefine egyéni végpontokat visszaállítását kiszolgálói funkciók nem leképezése tooan insert, update, törlése vagy olvasási művelete.</span><span class="sxs-lookup"><span data-stu-id="d3809-371">A custom API enables you toodefine custom endpoints that expose server functionality that does not map tooan insert, update, delete, or read operation.</span></span> <span data-ttu-id="d3809-372">Egy egyéni API használatával is befolyásolni további üzenetküldés, beleértve az olvasási és HTTP-üzenet fejlécek beállítása meghatározó JSON nem üzenet törzsének formátumban.</span><span class="sxs-lookup"><span data-stu-id="d3809-372">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="d3809-373">Android-ügyfélről, meghívja a hello **invokeApi** metódus toocall hello egyéni API-végpont.</span><span class="sxs-lookup"><span data-stu-id="d3809-373">From an Android client, you call hello **invokeApi** method toocall hello custom API endpoint.</span></span> <span data-ttu-id="d3809-374">hello következő példa bemutatja, hogyan toocall egy API-végpont nevű **completeAll**, egy gyűjtemény osztályt adja vissza, amely **MarkAllResult**.</span><span class="sxs-lookup"><span data-stu-id="d3809-374">hello following example shows how toocall an API endpoint named **completeAll**, which returns a collection class named **MarkAllResult**.</span></span>

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

<span data-ttu-id="d3809-375">Hello **invokeApi** módszer hello ügyfél, amely POST kérelem toohello új egyéni API neve.</span><span class="sxs-lookup"><span data-stu-id="d3809-375">hello **invokeApi** method is called on hello client, which sends a POST request toohello new custom API.</span></span> <span data-ttu-id="d3809-376">hello egyéni API által visszaadott hello eredmény jelenik meg egy üzenet párbeszédpanelen hibák.</span><span class="sxs-lookup"><span data-stu-id="d3809-376">hello result returned by hello custom API is displayed in a message dialog, as are any errors.</span></span> <span data-ttu-id="d3809-377">Egyéb verziói **invokeApi** lehetővé teszik, hogy opcionálisan hello kérés törzsében egy objektum küldését, adja meg a hello HTTP-metódus, és lekérdezési paraméterek hello kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="d3809-377">Other versions of **invokeApi** let you optionally send an object in hello request body, specify hello HTTP method, and send query parameters with hello request.</span></span> <span data-ttu-id="d3809-378">Típus nélküli verziói **invokeApi** is találhatók.</span><span class="sxs-lookup"><span data-stu-id="d3809-378">Untyped versions of **invokeApi** are provided as well.</span></span>

## <span data-ttu-id="d3809-379"><a name="authentication"></a>Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d3809-379"><a name="authentication"></a>Add authentication tooyour app</span></span>

<span data-ttu-id="d3809-380">Oktatóanyagok már részletesen leírja hogyan tooadd ezeket a szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="d3809-380">Tutorials already describe in detail how tooadd these features.</span></span>

<span data-ttu-id="d3809-381">Támogatja az App Service [app felhasználók hitelesítéséhez](app-service-mobile-android-get-started-users.md) használatával különböző külső Identitásszolgáltatók: Facebook, a Google, a Microsoft Account, a Twitter és az Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d3809-381">App Service supports [authenticating app users](app-service-mobile-android-get-started-users.md) using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="d3809-382">Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="d3809-382">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="d3809-383">A kiszolgáló is használhatja hello identitás hitelesített felhasználók tooimplement az engedélyezési szabályok.</span><span class="sxs-lookup"><span data-stu-id="d3809-383">You can also use hello identity of authenticated users tooimplement authorization rules in your backend.</span></span>

<span data-ttu-id="d3809-384">Két hitelesítési forgalom támogatottak: egy **server** folyamata és a **ügyfél** folyamata.</span><span class="sxs-lookup"><span data-stu-id="d3809-384">Two authentication flows are supported: a **server** flow and a **client** flow.</span></span> <span data-ttu-id="d3809-385">hello kiszolgáló folyamata nyújt hello legegyszerűbb hitelesítési élmény hello identity providers webes felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="d3809-385">hello server flow provides hello simplest authentication experience, as it relies on hello identity providers web interface.</span></span>  <span data-ttu-id="d3809-386">Nincsenek további SDK-k olyan szükséges tooimplement kiszolgálóhitelesítés folyamata.</span><span class="sxs-lookup"><span data-stu-id="d3809-386">No additional SDKs are required tooimplement server flow authentication.</span></span> <span data-ttu-id="d3809-387">Kiszolgálóhitelesítés folyamata nem biztosít egy hello mobil eszközre való mély integráció, és csak ajánlott forgatókönyvek koncepció igazolása.</span><span class="sxs-lookup"><span data-stu-id="d3809-387">Server flow authentication does not provide a deep integration into hello mobile device and is only recommended for proof of concept scenarios.</span></span>

<span data-ttu-id="d3809-388">módon támaszkodnak az SDK-k hello identitásszolgáltató által biztosított eszközspecifikus képességekkel bővült, például egyszeri bejelentkezés szorosabb integrációt hello ügyféltanúsítvány folyamat teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="d3809-388">hello client flow allows for deeper integration with device-specific capabilities such as single sign-on as it relies on SDKs provided by hello identity provider.</span></span>  <span data-ttu-id="d3809-389">Például hello Facebook SDK is integrálható a mobilalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d3809-389">For example, you can integrate hello Facebook SDK into your mobile application.</span></span>  <span data-ttu-id="d3809-390">hello mobil ügyfél cseréje hello Facebook-alkalmazásba, és megerősíti, hogy a bejelentkezés előtt vissza tooyour mobilalkalmazás csere.</span><span class="sxs-lookup"><span data-stu-id="d3809-390">hello mobile client swaps into hello Facebook app and confirms your sign-on before swapping back tooyour mobile app.</span></span>

<span data-ttu-id="d3809-391">Négy lépést az alkalmazás szükséges tooenable hitelesítési:</span><span class="sxs-lookup"><span data-stu-id="d3809-391">Four steps are required tooenable authentication in your app:</span></span>

* <span data-ttu-id="d3809-392">Hitelesítés az alkalmazás regisztrálása egy identitásszolgáltatóval.</span><span class="sxs-lookup"><span data-stu-id="d3809-392">Register your app for authentication with an identity provider.</span></span>
* <span data-ttu-id="d3809-393">App Service-háttéralkalmazásának konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d3809-393">Configure your App Service backend.</span></span>
* <span data-ttu-id="d3809-394">Tábla engedélyek tooauthenticated felhasználók csak az App Service-háttérrendszer hello korlátozása.</span><span class="sxs-lookup"><span data-stu-id="d3809-394">Restrict table permissions tooauthenticated users only on hello App Service backend.</span></span>
* <span data-ttu-id="d3809-395">Hitelesítési kód tooyour alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d3809-395">Add authentication code tooyour app.</span></span>

<span data-ttu-id="d3809-396">Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="d3809-396">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="d3809-397">Is használhatja a SID-je egy hitelesített felhasználó toomodify hello kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="d3809-397">You can also use hello SID of an authenticated user toomodify requests.</span></span>  <span data-ttu-id="d3809-398">További információkért tekintse át a [Bevezetés a hitelesítés használatába] és hello Server SDK útmutató dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d3809-398">For more information, review [Get started with authentication] and hello Server SDK HOWTO documentation.</span></span>

### <span data-ttu-id="d3809-399"><a name="caching"></a>Hitelesítési: Server folyamat</span><span class="sxs-lookup"><span data-stu-id="d3809-399"><a name="caching"></a>Authentication: Server Flow</span></span>

<span data-ttu-id="d3809-400">hello alábbira elindítja a kiszolgáló folyamata bejelentkezési folyamatot hello Google-szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="d3809-400">hello following code starts a server flow login process using hello Google provider.</span></span>  <span data-ttu-id="d3809-401">Nincs szükség további konfigurációra hello Google szolgáltató hello biztonsági követelmények miatt:</span><span class="sxs-lookup"><span data-stu-id="d3809-401">Additional configuration is required because of hello security requirements for hello Google provider:</span></span>

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

<span data-ttu-id="d3809-402">Továbbá adja hozzá a következő metódus toohello fő tevékenységosztállyal hello:</span><span class="sxs-lookup"><span data-stu-id="d3809-402">In addition, add hello following method toohello main Activity class:</span></span>

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

<span data-ttu-id="d3809-403">Hello `GOOGLE_LOGIN_REQUEST_CODE` a fő definiált tevékenységgel a hello `login()` metódus és hello belül `onActivityResult()` metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-403">hello `GOOGLE_LOGIN_REQUEST_CODE` defined in your main Activity is used for hello `login()` method and within hello `onActivityResult()` method.</span></span>  <span data-ttu-id="d3809-404">Minden egyedi szám, mindaddig, amíg hello ugyanannyi belül használják hello választhat `login()` metódus és hello `onActivityResult()` metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-404">You can choose any unique number, as long as hello same number is used within hello `login()` method and hello `onActivityResult()` method.</span></span>  <span data-ttu-id="d3809-405">Hello Ügyfélkód szolgáltatás kártyához (a korábban bemutatott) tehetik absztrakttá, hello megfelelő módszereket hello szolgáltatás adapteren kell hívjuk.</span><span class="sxs-lookup"><span data-stu-id="d3809-405">If you abstract hello client code into a service adapter (as shown earlier), you should call hello appropriate methods on hello service adapter.</span></span>

<span data-ttu-id="d3809-406">Szüksége is tooconfigure hello projekt customtabs.</span><span class="sxs-lookup"><span data-stu-id="d3809-406">You also need tooconfigure hello project for customtabs.</span></span>  <span data-ttu-id="d3809-407">Először adja meg egy átirányítási URL.</span><span class="sxs-lookup"><span data-stu-id="d3809-407">First specify a redirect-URL.</span></span>  <span data-ttu-id="d3809-408">Adja hozzá a következő kódrészletet túl hello`AndroidManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="d3809-408">Add hello following snippet too`AndroidManifest.xml`:</span></span>

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

<span data-ttu-id="d3809-409">Adja hozzá a hello **redirectUriScheme** toohello `build.gradle` fájl az alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="d3809-409">Add hello **redirectUriScheme** toohello `build.gradle` file for your application:</span></span>

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

<span data-ttu-id="d3809-410">Végül adja hozzá `com.android.support:customtabs:23.0.1` toohello alkalmazásfüggőségek listáján a hello `build.gradle` fájlt:</span><span class="sxs-lookup"><span data-stu-id="d3809-410">Finally, add `com.android.support:customtabs:23.0.1` toohello dependencies list in hello `build.gradle` file:</span></span>

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

<span data-ttu-id="d3809-411">Hello azonosítója hello bejelentkezett felhasználó az beszerzése egy **MobileServiceUser** hello segítségével **getUserId** metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-411">Obtain hello ID of hello logged-in user from a **MobileServiceUser** using hello **getUserId** method.</span></span> <span data-ttu-id="d3809-412">Hogyan toouse határidő toocall hello aszinkron bejelentkezési API-k példát talál [Bevezetés a hitelesítés használatába].</span><span class="sxs-lookup"><span data-stu-id="d3809-412">For an example of how toouse Futures toocall hello asynchronous login APIs, see [Get started with authentication].</span></span>

> [!WARNING]
> <span data-ttu-id="d3809-413">hello említett URL-séma a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="d3809-413">hello URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="d3809-414">Győződjön meg arról, hogy minden előfordulását `{url_scheme_of_you_app}` nagybetű.</span><span class="sxs-lookup"><span data-stu-id="d3809-414">Ensure that all occurrences of `{url_scheme_of_you_app}` match case.</span></span>

### <span data-ttu-id="d3809-415"><a name="caching"></a>A hitelesítési tokenek gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="d3809-415"><a name="caching"></a>Cache authentication tokens</span></span>

<span data-ttu-id="d3809-416">A hitelesítési tokenek gyorsítótárazás meg toostore hello felhasználói Azonosítót és a hitelesítési jogkivonat helyileg hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="d3809-416">Caching authentication tokens requires you toostore hello User ID and authentication token locally on hello device.</span></span> <span data-ttu-id="d3809-417">hello hello alkalmazás elindul, amikor legközelebb bejelöli hello gyorsítótár, és ezek az értékek jelen, ha hello bejelentkezési eljárást kihagyhatja, és rehydrate hello ügyfél, és ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d3809-417">hello next time hello app starts, you check hello cache, and if these values are present, you can skip hello log in procedure and rehydrate hello client with this data.</span></span> <span data-ttu-id="d3809-418">Azonban ezek az adatok bizalmas, és azt kell tárolni, titkosított biztonsági abban az esetben hello phone biztosítása.</span><span class="sxs-lookup"><span data-stu-id="d3809-418">However this data is sensitive, and it should be stored encrypted for safety in case hello phone gets stolen.</span></span>  <span data-ttu-id="d3809-419">Láthatja, hogy hogyan toocache hitelesítési jogkivonatok az átfogó példát [gyorsítótárazza a hitelesítési tokenek szakaszban][7].</span><span class="sxs-lookup"><span data-stu-id="d3809-419">You can see a complete example of how toocache authentication tokens in [Cache authentication tokens section][7].</span></span>

<span data-ttu-id="d3809-420">Amikor toouse egy lejárt jogkivonatot, kap egy *401 nem engedélyezett* válasz.</span><span class="sxs-lookup"><span data-stu-id="d3809-420">When you try toouse an expired token, you receive a *401 unauthorized* response.</span></span> <span data-ttu-id="d3809-421">Hitelesítési hibák szűrők segítségével kezelheti.</span><span class="sxs-lookup"><span data-stu-id="d3809-421">You can handle authentication errors using filters.</span></span>  <span data-ttu-id="d3809-422">Szűrők intercept kérelmek toohello App Service-háttérrendszer.</span><span class="sxs-lookup"><span data-stu-id="d3809-422">Filters intercept requests toohello App Service backend.</span></span> <span data-ttu-id="d3809-423">hello kód teszteli a 401-es válasz hello hello bejelentkezési folyamat elindítja és majd tovább folytatja a 401-es hello létrehozó hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="d3809-423">hello filter code tests hello response for a 401, triggers hello sign-in process, and then resumes hello request that generated hello 401.</span></span>

### <span data-ttu-id="d3809-424"><a name="refresh"></a>Használja a frissítési jogkivonatokat</span><span class="sxs-lookup"><span data-stu-id="d3809-424"><a name="refresh"></a>Use Refresh Tokens</span></span>

<span data-ttu-id="d3809-425">hello Azure App Service hitelesítés és engedélyezés által visszaadott jogkivonatának egy meghatározott élettartama 1 óra.</span><span class="sxs-lookup"><span data-stu-id="d3809-425">hello token returned by Azure App Service Authentication and Authorization has a defined life time of one hour.</span></span>  <span data-ttu-id="d3809-426">Ezt követően újból hitelesítésre hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d3809-426">After this period, you must reauthenticate hello user.</span></span>  <span data-ttu-id="d3809-427">Ha egy ügyfél-folyamat hitelesítési keresztül érkezett, akkor is hitelesítését hosszú élettartamú jogkivonatot használata az Azure App Service Authentication és engedélyezési használata hello ugyanezt a tokent.</span><span class="sxs-lookup"><span data-stu-id="d3809-427">If you are using a long-lived token that you have received via client-flow authentication, then you can reauthenticate with Azure App Service Authentication and Authorization using hello same token.</span></span>  <span data-ttu-id="d3809-428">Egy másik Azure App Service-jogkivonat új élettartamán jön létre.</span><span class="sxs-lookup"><span data-stu-id="d3809-428">Another Azure App Service token is generated with a new lifetime.</span></span>

<span data-ttu-id="d3809-429">Hello szolgáltató toouse frissítési jogkivonatok is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="d3809-429">You can also register hello provider toouse Refresh Tokens.</span></span>  <span data-ttu-id="d3809-430">A frissítési Token nem mindig rendelkezésre áll.</span><span class="sxs-lookup"><span data-stu-id="d3809-430">A Refresh Token is not always available.</span></span>  <span data-ttu-id="d3809-431">Nincs szükség további konfigurációra:</span><span class="sxs-lookup"><span data-stu-id="d3809-431">Additional configuration is required:</span></span>

* <span data-ttu-id="d3809-432">A **Azure Active Directory**, a titkos ügyfélkulcsot az Azure Active Directory-alkalmazás hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d3809-432">For **Azure Active Directory**, configure a client secret for hello Azure Active Directory App.</span></span>  <span data-ttu-id="d3809-433">Adja meg hello ügyfélkulcs hello Azure App Service, Azure Active Directory-hitelesítés konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="d3809-433">Specify hello client secret in hello Azure App Service when configuring Azure Active Directory Authentication.</span></span>  <span data-ttu-id="d3809-434">Meghívásakor `.login()`, adja át `response_type=code id_token` paramétert:</span><span class="sxs-lookup"><span data-stu-id="d3809-434">When calling `.login()`, pass `response_type=code id_token` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="d3809-435">A **Google**, adja át a hello `access_type=offline` paramétert:</span><span class="sxs-lookup"><span data-stu-id="d3809-435">For **Google**, pass hello `access_type=offline` as a parameter:</span></span>

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* <span data-ttu-id="d3809-436">A **Microsoft Account**, jelölje be hello `wl.offline_access` hatókör.</span><span class="sxs-lookup"><span data-stu-id="d3809-436">For **Microsoft Account**, select hello `wl.offline_access` scope.</span></span>

<span data-ttu-id="d3809-437">egy token toorefresh hívja `.refreshUser()`:</span><span class="sxs-lookup"><span data-stu-id="d3809-437">toorefresh a token, call `.refreshUser()`:</span></span>

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

<span data-ttu-id="d3809-438">Ajánlott eljárásként hozzon létre egy szűrőt, amely észleli a 401-es válasz hello kiszolgálóról, és megpróbál toorefresh hello felhasználói jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="d3809-438">As a best practice, create a filter that detects a 401 response from hello server and tries toorefresh hello user token.</span></span>

## <a name="log-in-with-client-flow-authentication"></a><span data-ttu-id="d3809-439">Jelentkezzen be az ügyféltanúsítvány-folyamat hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d3809-439">Log in with Client-flow Authentication</span></span>

<span data-ttu-id="d3809-440">hello általános folyamat az ügyfél-folyamat hitelesítési bejelentkezik a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="d3809-440">hello general process for logging in with client-flow authentication is as follows:</span></span>

* <span data-ttu-id="d3809-441">Azure App Service hitelesítés és engedélyezés konfigurálhatók, mint server-folyamat hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d3809-441">Configure Azure App Service Authentication and Authorization as you would server-flow authentication.</span></span>
* <span data-ttu-id="d3809-442">Integrálni hello hitelesítési szolgáltató SDK a hitelesítési tooproduce olyan hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="d3809-442">Integrate hello authentication provider SDK for authentication tooproduce an access token.</span></span>
* <span data-ttu-id="d3809-443">Hello hívás `.login()` módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d3809-443">Call hello `.login()` method as follows:</span></span>

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

<span data-ttu-id="d3809-444">Cserélje le a hello `onSuccess()` metódus függetlenül kódját, toouse kívánja a sikeres bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="d3809-444">Replace hello `onSuccess()` method with whatever code you wish toouse on a successful login.</span></span>  <span data-ttu-id="d3809-445">Hello `{provider}` karakterlánca egy érvényes szolgáltatói: **aad** (az Azure Active Directory), **facebook**, **google**, **microsoftaccount**, vagy **twitter**.</span><span class="sxs-lookup"><span data-stu-id="d3809-445">hello `{provider}` string is a valid provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, or **twitter**.</span></span>  <span data-ttu-id="d3809-446">Ha egyéni hitelesítési megvalósítását, majd is használhatja hello egyéni hitelesítési szolgáltató címkéje.</span><span class="sxs-lookup"><span data-stu-id="d3809-446">If you have implemented custom authentication, then you can also use hello custom authentication provider tag.</span></span>

### <span data-ttu-id="d3809-447"><a name="adal"></a>Hitelesíti a felhasználókat az Active Directory Authentication Library (ADAL) hello</span><span class="sxs-lookup"><span data-stu-id="d3809-447"><a name="adal"></a>Authenticate users with hello Active Directory Authentication Library (ADAL)</span></span>

<span data-ttu-id="d3809-448">Az alkalmazás Azure Active Directory használatával hello Active Directory Authentication Library (ADAL) toosign felhasználók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d3809-448">You can use hello Active Directory Authentication Library (ADAL) toosign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="d3809-449">Egy ügyfél folyamata bejelentkezési használata gyakran érdemes toousing hello `loginAsync()` módszerek, mert több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="d3809-449">Using a client flow login is often preferable toousing hello `loginAsync()` methods as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="d3809-450">A mobil-háttéralkalmazás számára az AAD-bejelentkezés konfigurálása következő hello [hogyan tooconfigure App Service az Active Directory bejelentkezési] [ 22] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="d3809-450">Configure your mobile app backend for AAD sign-in by following hello [How tooconfigure App Service for Active Directory login][22] tutorial.</span></span> <span data-ttu-id="d3809-451">Győződjön meg arról, hogy toocomplete hello opcionális lépés egy natív ügyfélalkalmazás regisztrációján.</span><span class="sxs-lookup"><span data-stu-id="d3809-451">Make sure toocomplete hello optional step of registering a native client application.</span></span>
2. <span data-ttu-id="d3809-452">Telepítse az adal-t a build.gradle tooinclude hello, a következő definíciókat módosításával:</span><span class="sxs-lookup"><span data-stu-id="d3809-452">Install ADAL by modifying your build.gradle file tooinclude hello following definitions:</span></span>

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

1. <span data-ttu-id="d3809-453">Adja hozzá a következő kód tooyour alkalmazás biztosít, ami a következő cserékhez hello hello:</span><span class="sxs-lookup"><span data-stu-id="d3809-453">Add hello following code tooyour application, making hello following replacements:</span></span>

* <span data-ttu-id="d3809-454">Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** hello nevű hello bérlő, amelyben az alkalmazás kiépítve.</span><span class="sxs-lookup"><span data-stu-id="d3809-454">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="d3809-455">hello formátumúnak kell lennie a https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="d3809-455">hello format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span>
* <span data-ttu-id="d3809-456">Cserélje le **INSERT-erőforrás-azonosító-Itt** hello ügyfél-azonosítójú a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d3809-456">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="d3809-457">Ügyfél-azonosító hello szerezhet be a hello **speciális** lap **Azure Active Directory beállításai** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="d3809-457">You can obtain hello client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
* <span data-ttu-id="d3809-458">Cserélje le **INSERT-ügyfél-azonosító-Itt** hello ügyfél-azonosítójú hello natív ügyfélalkalmazás másolta.</span><span class="sxs-lookup"><span data-stu-id="d3809-458">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
* <span data-ttu-id="d3809-459">Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="d3809-459">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="d3809-460">Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="d3809-460">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

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

## <span data-ttu-id="d3809-461"><a name="filters"></a>Ügyfél – kiszolgáló kommunikáció hello beállítása</span><span class="sxs-lookup"><span data-stu-id="d3809-461"><a name="filters"></a>Adjust hello Client-Server Communication</span></span>

<span data-ttu-id="d3809-462">hello ügyfélkapcsolat általában egy alapszintű hello az alapul szolgáló HTTP szalagtár pedig a hello Android SDK használatával HTTP-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="d3809-462">hello Client connection is normally a basic HTTP connection using hello underlying HTTP library supplied with hello Android SDK.</span></span>  <span data-ttu-id="d3809-463">Miért van szüksége a toochange számos oka lehet, hogy:</span><span class="sxs-lookup"><span data-stu-id="d3809-463">There are several reasons why you would want toochange that:</span></span>

* <span data-ttu-id="d3809-464">Egy másik HTTP könyvtár tooadjust időtúllépések kívánja toouse.</span><span class="sxs-lookup"><span data-stu-id="d3809-464">You wish toouse an alternate HTTP library tooadjust timeouts.</span></span>
* <span data-ttu-id="d3809-465">Egy folyamatjelző kívánja tooprovide.</span><span class="sxs-lookup"><span data-stu-id="d3809-465">You wish tooprovide a progress bar.</span></span>
* <span data-ttu-id="d3809-466">Tooadd kívánja egy egyéni fejlécet toosupport API felügyeleti funkciót.</span><span class="sxs-lookup"><span data-stu-id="d3809-466">You wish tooadd a custom header toosupport API management functionality.</span></span>
* <span data-ttu-id="d3809-467">Kívánja toointercept sikertelen választ, hogy újrahitelesítés valósíthat meg.</span><span class="sxs-lookup"><span data-stu-id="d3809-467">You wish toointercept a failed response so that you can implement reauthentication.</span></span>
* <span data-ttu-id="d3809-468">Toolog kérelmek tooan analytics háttérszolgáltatást kívánja.</span><span class="sxs-lookup"><span data-stu-id="d3809-468">You wish toolog backend requests tooan analytics service.</span></span>

### <a name="using-an-alternate-http-library"></a><span data-ttu-id="d3809-469">Egy másik HTTP-könyvtár használatával</span><span class="sxs-lookup"><span data-stu-id="d3809-469">Using an alternate HTTP Library</span></span>

<span data-ttu-id="d3809-470">Hello hívás `.setAndroidHttpClientFactory()` ügyfél referenciaként a létrehozása után azonnal metódust.</span><span class="sxs-lookup"><span data-stu-id="d3809-470">Call hello `.setAndroidHttpClientFactory()` method immediately after creating your client reference.</span></span>  <span data-ttu-id="d3809-471">Például tooset hello kapcsolat időtúllépése too60 (mp) (helyett hello alapértelmezett érték 10 másodperc):</span><span class="sxs-lookup"><span data-stu-id="d3809-471">For example, tooset hello connection timeout too60 seconds (instead of hello default 10 seconds):</span></span>

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

### <a name="implement-a-progress-filter"></a><span data-ttu-id="d3809-472">A folyamatban lévő szűrő megvalósítása</span><span class="sxs-lookup"><span data-stu-id="d3809-472">Implement a Progress Filter</span></span>

<span data-ttu-id="d3809-473">Megvalósíthat egy intercept minden kérelem implementálásával egy `ServiceFilter`.</span><span class="sxs-lookup"><span data-stu-id="d3809-473">You can implement an intercept of every request by implementing a `ServiceFilter`.</span></span>  <span data-ttu-id="d3809-474">Hello következő például egy előre létrehozott folyamatjelző frissíti:</span><span class="sxs-lookup"><span data-stu-id="d3809-474">For example, hello following updates a pre-created progress bar:</span></span>

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

<span data-ttu-id="d3809-475">A szűrő toohello ügyfél lehet társítani a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="d3809-475">You can attach this filter toohello client as follows:</span></span>

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a><span data-ttu-id="d3809-476">Kérelemfejléc testreszabása</span><span class="sxs-lookup"><span data-stu-id="d3809-476">Customize Request Headers</span></span>

<span data-ttu-id="d3809-477">Hello következő `ServiceFilter` , és csatlakoztassa a hello hello szűrő hello azonos módon `ProgressFilter`:</span><span class="sxs-lookup"><span data-stu-id="d3809-477">Use hello following `ServiceFilter` and attach hello filter in hello same way as hello `ProgressFilter`:</span></span>

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

### <span data-ttu-id="d3809-478"><a name="conversions"></a>Automatikus szerializálási konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d3809-478"><a name="conversions"></a>Configure Automatic Serialization</span></span>

<span data-ttu-id="d3809-479">Megadhatja a hello segítségével tooevery oszlop alkalmazó átalakítás stratégiát [gson] [ 3] API.</span><span class="sxs-lookup"><span data-stu-id="d3809-479">You can specify a conversion strategy that applies tooevery column by using hello [gson][3] API.</span></span> <span data-ttu-id="d3809-480">hello Android ügyféloldali kódtár által használt [gson] [ 3] hello háttérben tooserialize Java-objektumokat tooJSON adatok tooAzure App Service hello adatok elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="d3809-480">hello Android client library uses [gson][3] behind hello scenes tooserialize Java objects tooJSON data before hello data is sent tooAzure App Service.</span></span>  <span data-ttu-id="d3809-481">hello következő használ a hello **setFieldNamingStrategy()** metódus tooset hello stratégia.</span><span class="sxs-lookup"><span data-stu-id="d3809-481">hello following code uses hello **setFieldNamingStrategy()** method tooset hello strategy.</span></span> <span data-ttu-id="d3809-482">Ez a példa törli hello kezdeti (az "m"), és karakter majd kisbetűs hello tovább, minden mező neve.</span><span class="sxs-lookup"><span data-stu-id="d3809-482">This example will delete hello initial character (an "m"), and then lower-case hello next character, for every field name.</span></span> <span data-ttu-id="d3809-483">Például azt szeretné ikonná "left" "id".</span><span class="sxs-lookup"><span data-stu-id="d3809-483">For example, it would turn "mId" into "id."</span></span>  <span data-ttu-id="d3809-484">Egy konverzió stratégia tooreduce hello szükséges megvalósítása `SerializedName()` jegyzetek a legtöbb mező.</span><span class="sxs-lookup"><span data-stu-id="d3809-484">Implement a conversion strategy tooreduce hello need for `SerializedName()` annotations on most fields.</span></span>

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

<span data-ttu-id="d3809-485">Ez a kód hello segítségével mobil ügyfél hivatkozás létrehozása előtt végre kell hajtani **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="d3809-485">This code must be executed before creating a mobile client reference using hello **MobileServiceClient**.</span></span>

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
