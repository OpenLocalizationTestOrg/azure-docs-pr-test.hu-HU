---
title: "aaaAzure AD v2 Android bevezetés - telepítő |} Microsoft Docs"
description: "Az Android-alkalmazásokat hogyan szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API-val vagy a hozzáférési jogkivonatok az Azure Active Directory v2 végpont igénylő API-k"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: df2670d6d35b7a9a81158d4d7eb190540ca9c695
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="2f009-103">A projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="2f009-103">Set up your project</span></span>

> <span data-ttu-id="2f009-104">Inkább toodownload Ez a minta Android Studio project helyett?</span><span class="sxs-lookup"><span data-stu-id="2f009-104">Prefer toodownload this sample's Android Studio project instead?</span></span> <span data-ttu-id="2f009-105">[Töltse le a projekt](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="2f009-105">[Download a project](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing    .</span></span>


### <a name="create-a-new-project"></a><span data-ttu-id="2f009-106">Új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f009-106">Create a new project</span></span> 
1.  <span data-ttu-id="2f009-107">Android Studio megnyitásához, írja be:`File` > `New` > `New Project`</span><span class="sxs-lookup"><span data-stu-id="2f009-107">Open Android Studio, go to: `File` > `New` > `New Project`</span></span>
2.  <span data-ttu-id="2f009-108">Az alkalmazás neve, és kattintson`Next`</span><span class="sxs-lookup"><span data-stu-id="2f009-108">Name your application and click `Next`</span></span>
3.  <span data-ttu-id="2f009-109">Győződjön meg arról, hogy tooselect *API 21 vagy újabb (Android 5.0)* kattintson`Next`</span><span class="sxs-lookup"><span data-stu-id="2f009-109">Make sure tooselect *API 21 or newer (Android 5.0)* and click `Next`</span></span>
4.  <span data-ttu-id="2f009-110">Hagyja `Empty Activity`, kattintson a `Next`, majd`Finish`</span><span class="sxs-lookup"><span data-stu-id="2f009-110">Leave `Empty Activity`, click `Next`, then `Finish`</span></span>


### <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a><span data-ttu-id="2f009-111">Hello Microsoft hitelesítési könyvtár (MSAL) tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f009-111">Add hello Microsoft Authentication Library (MSAL) tooyour project</span></span>
1.  <span data-ttu-id="2f009-112">Az Android Studióban Ugrás:`Gradle Scripts` > `build.gradle (Module: app)`</span><span class="sxs-lookup"><span data-stu-id="2f009-112">In Android Studio, go to: `Gradle Scripts` > `build.gradle (Module: app)`</span></span>
2.  <span data-ttu-id="2f009-113">Másolás és beillesztés hello következő kód a `Dependencies`:</span><span class="sxs-lookup"><span data-stu-id="2f009-113">Copy and paste hello following code under `Dependencies`:</span></span>

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a><span data-ttu-id="2f009-114">A csomaggal kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="2f009-114">About this package</span></span>

<span data-ttu-id="2f009-115">a fenti hello a csomag telepíti a hello Microsoft hitelesítési könyvtár (MSAL).</span><span class="sxs-lookup"><span data-stu-id="2f009-115">hello package above installs hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="2f009-116">MSAL kezeli az beszerzése, gyorsítótárazás és felhasználói használt a jogkivonatokat tooaccess API-k védi az Azure Active Directory v2 végpont frissítése.</span><span class="sxs-lookup"><span data-stu-id="2f009-116">MSAL handles acquiring, caching and refreshing user tokens used tooaccess APIs protected by Azure Active Directory v2 endpoint.</span></span>
<!--end-collapse-->

## <a name="create-your-applications-ui"></a><span data-ttu-id="2f009-117">Az alkalmazás felhasználói Felületüket létrehozni</span><span class="sxs-lookup"><span data-stu-id="2f009-117">Create your application’s UI</span></span>

1.  <span data-ttu-id="2f009-118">Nyissa meg: `activity_main.xml` alatt`res` > `layout`</span><span class="sxs-lookup"><span data-stu-id="2f009-118">Open: `activity_main.xml` under `res` > `layout`</span></span>
2.  <span data-ttu-id="2f009-119">Változás hello tevékenység elrendezését, `android.support.constraint.ConstraintLayout` vagy más túl`LinearLayout`</span><span class="sxs-lookup"><span data-stu-id="2f009-119">Change hello activity layout from `android.support.constraint.ConstraintLayout` or other too`LinearLayout`</span></span>
3.  <span data-ttu-id="2f009-120">Adja hozzá `android:orientation="vertical"` tulajdonság túl`LinearLayout` csomópont</span><span class="sxs-lookup"><span data-stu-id="2f009-120">Add `android:orientation="vertical"` property too`LinearLayout` node</span></span>
4.  <span data-ttu-id="2f009-121">Másolás és beillesztés hello következő kódot a hello `LinearLayout` csomópont hello aktuális tartalom cseréje:</span><span class="sxs-lookup"><span data-stu-id="2f009-121">Copy and paste hello following code into hello `LinearLayout` node, replacing hello current content:</span></span>

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```

