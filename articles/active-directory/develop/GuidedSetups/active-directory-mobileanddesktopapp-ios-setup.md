---
title: "aaaAzure AD v2 iOS Getting Started - telepítő |} Microsoft Docs"
description: "Hogyan iOS (Swift) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 62c4ee9a2d4ccaec780bee09fb4bc34cff2eb6df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="setting-up-your-ios-application"></a><span data-ttu-id="be3d0-103">Az iOS-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="be3d0-103">Setting up your iOS application</span></span>

<span data-ttu-id="be3d0-104">Ez a témakör részletes utasításokkal szolgál egy új projekt toodemonstrate toocreate hogyan toointegrate iOS-alkalmazás (Swift) rendelkező *jelentkezzen be Microsoft* azt lekérdezhesse a webes API-k jogkivonat igénylő.</span><span class="sxs-lookup"><span data-stu-id="be3d0-104">This section provides step-by-step instructions for how toocreate a new project toodemonstrate how toointegrate an iOS application (Swift) with *Sign-In with Microsoft* so it can query Web APIs that require a token.</span></span>

> <span data-ttu-id="be3d0-105">Inkább toodownload XCode-projektjét, ez a minta helyett?</span><span class="sxs-lookup"><span data-stu-id="be3d0-105">Prefer toodownload this sample's XCode project instead?</span></span> <span data-ttu-id="be3d0-106">[Töltse le a projekt](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="be3d0-106">[Download a project](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>


## <a name="install-carthage-toodownload-and-build-msal"></a><span data-ttu-id="be3d0-107">Carthage toodownload telepítése és MSAL összeállítása</span><span class="sxs-lookup"><span data-stu-id="be3d0-107">Install Carthage toodownload and build MSAL</span></span>
<span data-ttu-id="be3d0-108">Carthage Csomagkezelő MSAL hello előzetes verzió időtartama során használt – hello lehetőségét, hogy a Microsoft toomake módosítások toohello könyvtár megőrzésével integrálható az XCode.</span><span class="sxs-lookup"><span data-stu-id="be3d0-108">Carthage package manager is used during hello preview period of MSAL – it integrates with XCode while maintaining hello ability for Microsoft toomake changes toohello library.</span></span>

- <span data-ttu-id="be3d0-109">Töltse le és telepítse a legújabb kiadásának hello Carthage [Itt](https://github.com/Carthage/Carthage/releases "Carthage letöltési URL-címe")</span><span class="sxs-lookup"><span data-stu-id="be3d0-109">Download and install hello latest release of Carthage [here](https://github.com/Carthage/Carthage/releases "Carthage download URL")</span></span>

## <a name="creating-your-application"></a><span data-ttu-id="be3d0-110">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="be3d0-110">Creating your application</span></span>

1.  <span data-ttu-id="be3d0-111">Nyissa meg az xcode-ban, és válassza ki`Create a new Xcode project`</span><span class="sxs-lookup"><span data-stu-id="be3d0-111">Open Xcode and select `Create a new Xcode project`</span></span>
2.  <span data-ttu-id="be3d0-112">Válassza ki `iOS`  >  `Single view Application` kattintson *tovább*</span><span class="sxs-lookup"><span data-stu-id="be3d0-112">Select `iOS` > `Single view Application` and click *Next*</span></span>
3.  <span data-ttu-id="be3d0-113">Nevezze el a termék, és kattintson a *következő*</span><span class="sxs-lookup"><span data-stu-id="be3d0-113">Give a product name and click *Next*</span></span>
4.  <span data-ttu-id="be3d0-114">Válasszon egy mappát toocreate az alkalmazáshoz, majd kattintson *létrehozása*</span><span class="sxs-lookup"><span data-stu-id="be3d0-114">Select a folder toocreate your app and click *Create*</span></span>

## <a name="build-hello-msal-framework"></a><span data-ttu-id="be3d0-115">Hello MSAL keretrendszer összeállítása</span><span class="sxs-lookup"><span data-stu-id="be3d0-115">Build hello MSAL Framework</span></span>

<span data-ttu-id="be3d0-116">Kövesse az alábbi toopull hello utasításokat, és majd kialakítható Carthage használatával MSAL könyvtárak legújabb verzióját hello:</span><span class="sxs-lookup"><span data-stu-id="be3d0-116">Follow hello instructions below toopull and then build hello latest version of MSAL libraries using Carthage:</span></span>

1.  <span data-ttu-id="be3d0-117">Nyissa meg a hello bash terminált, és toohello alkalmazás gyökérmappájában go</span><span class="sxs-lookup"><span data-stu-id="be3d0-117">Open hello bash terminal and go toohello App’s root folder</span></span>
2.  <span data-ttu-id="be3d0-118">Másolja az alábbi hello, és illessze be a hello bash terminál toocreate "Cartfile" fájlba:</span><span class="sxs-lookup"><span data-stu-id="be3d0-118">Copy hello below and paste in hello bash terminal toocreate a ‘Cartfile’ file:</span></span>

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
<span data-ttu-id="be3d0-119">Másolja és illessze be az alábbi hello.</span><span class="sxs-lookup"><span data-stu-id="be3d0-119">Copy and paste hello below.</span></span> <span data-ttu-id="be3d0-120">Ez a parancs beolvassa a függőségek egy Carthage/kivételek mappába, majd összeállít hello MSAL könyvtár:</span><span class="sxs-lookup"><span data-stu-id="be3d0-120">This command fetches dependencies into a Carthage/Checkouts folder, then builds hello MSAL library:</span></span>
</li>
</ol>

```bash
carthage update
```

> <span data-ttu-id="be3d0-121">a fenti hello folyamat használt toodownload és build hello Microsoft hitelesítési könyvtár (MSAL).</span><span class="sxs-lookup"><span data-stu-id="be3d0-121">hello process above is used toodownload and build hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="be3d0-122">MSAL kezeli az beszerzése, gyorsítótárazás, és a felhasználó használt a jogkivonatokat tooaccess védi hello Azure Active Directory v2 API-k frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="be3d0-122">MSAL handles acquiring, caching and refreshing user tokens used tooaccess APIs protected by hello Azure Active Directory v2.</span></span>

## <a name="add-hello-msal-framework-tooyour-application"></a><span data-ttu-id="be3d0-123">Hello MSAL keretrendszer tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="be3d0-123">Add hello MSAL framework tooyour application</span></span>
1.  <span data-ttu-id="be3d0-124">Az Xcode-ban nyissa meg a hello `General` lap</span><span class="sxs-lookup"><span data-stu-id="be3d0-124">In Xcode, open hello `General` tab</span></span>
2.  <span data-ttu-id="be3d0-125">Nyissa meg toohello `Linked Frameworks and Libraries` szakaszt, és kattintson`+`</span><span class="sxs-lookup"><span data-stu-id="be3d0-125">Go toohello `Linked Frameworks and Libraries` section and click `+`</span></span>
3.  <span data-ttu-id="be3d0-126">A következők szerint válasszon: `Add other…`</span><span class="sxs-lookup"><span data-stu-id="be3d0-126">Select `Add other…`</span></span>
4.  <span data-ttu-id="be3d0-127">Válassza a következőt: `Carthage`  >  `Build`  >  `iOS`  >  `MSAL.framework` kattintson *nyitott*.</span><span class="sxs-lookup"><span data-stu-id="be3d0-127">Select: `Carthage` > `Build` > `iOS` > `MSAL.framework` and click *Open*.</span></span> <span data-ttu-id="be3d0-128">Megtekintheti az `MSAL.framework` toohello lista hozzá.</span><span class="sxs-lookup"><span data-stu-id="be3d0-128">You should see `MSAL.framework` added toohello list.</span></span>
5.  <span data-ttu-id="be3d0-129">Nyissa meg túl`Build Phases` fülre, majd `+` ikonra, válassza a`New Run Script Phase`</span><span class="sxs-lookup"><span data-stu-id="be3d0-129">Go too`Build Phases` tab, and click `+` icon, choose `New Run Script Phase`</span></span>
6.  <span data-ttu-id="be3d0-130">Adja hozzá a következő tartalma toohello hello *parancsfájl-terület*:</span><span class="sxs-lookup"><span data-stu-id="be3d0-130">Add hello following contents toohello *script area*:</span></span>

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
<span data-ttu-id="be3d0-131">Adja hozzá a hello túl a következő<code>Input Files</code> kattintva <code>+</code>:</span><span class="sxs-lookup"><span data-stu-id="be3d0-131">Add hello following too<code>Input Files</code> by clicking <code>+</code>:</span></span>
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a><span data-ttu-id="be3d0-132">Az alkalmazás felhasználói felület létrehozása</span><span class="sxs-lookup"><span data-stu-id="be3d0-132">Creating your application’s UI</span></span>
<span data-ttu-id="be3d0-133">Egy Main.storyboard fájl automatikusan a projekt sablon részeként hozható létre.</span><span class="sxs-lookup"><span data-stu-id="be3d0-133">A Main.storyboard file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="be3d0-134">Kövesse az alábbi toocreate hello alkalmazás felhasználói felületén hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="be3d0-134">Follow hello instructions below toocreate hello app UI:</span></span>

1.  <span data-ttu-id="be3d0-135">CTRL + kattintson `Main.storyboard` toobring hello helyi menü kialakításához, és kattintson:`Open As` > `Source Code`</span><span class="sxs-lookup"><span data-stu-id="be3d0-135">Control+click `Main.storyboard` toobring up hello contextual menu, and then click: `Open As` > `Source Code`</span></span>
2.  <span data-ttu-id="be3d0-136">Cserélje le a hello `<scenes>` csomópont hello kódot:</span><span class="sxs-lookup"><span data-stu-id="be3d0-136">Replace hello `<scenes>` node with hello code below:</span></span>

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign-Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
