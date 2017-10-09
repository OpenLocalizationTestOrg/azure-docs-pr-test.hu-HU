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
## <a name="set-up-your-project"></a>A projekt beállítása

> Inkább toodownload Ez a minta Android Studio project helyett? [Töltse le a projekt](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.


### <a name="create-a-new-project"></a>Új projekt létrehozása 
1.  Android Studio megnyitásához, írja be:`File` > `New` > `New Project`
2.  Az alkalmazás neve, és kattintson`Next`
3.  Győződjön meg arról, hogy tooselect *API 21 vagy újabb (Android 5.0)* kattintson`Next`
4.  Hagyja `Empty Activity`, kattintson a `Next`, majd`Finish`


### <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a>Hello Microsoft hitelesítési könyvtár (MSAL) tooyour projekt hozzáadása
1.  Az Android Studióban Ugrás:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Másolás és beillesztés hello következő kód a `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>A csomaggal kapcsolatban

a fenti hello a csomag telepíti a hello Microsoft hitelesítési könyvtár (MSAL). MSAL kezeli az beszerzése, gyorsítótárazás és felhasználói használt a jogkivonatokat tooaccess API-k védi az Azure Active Directory v2 végpont frissítése.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Az alkalmazás felhasználói Felületüket létrehozni

1.  Nyissa meg: `activity_main.xml` alatt`res` > `layout`
2.  Változás hello tevékenység elrendezését, `android.support.constraint.ConstraintLayout` vagy más túl`LinearLayout`
3.  Adja hozzá `android:orientation="vertical"` tulajdonság túl`LinearLayout` csomópont
4.  Másolás és beillesztés hello következő kódot a hello `LinearLayout` csomópont hello aktuális tartalom cseréje:

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

