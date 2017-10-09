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
## <a name="setting-up-your-ios-application"></a>Az iOS-alkalmazás beállítása

Ez a témakör részletes utasításokkal szolgál egy új projekt toodemonstrate toocreate hogyan toointegrate iOS-alkalmazás (Swift) rendelkező *jelentkezzen be Microsoft* azt lekérdezhesse a webes API-k jogkivonat igénylő.

> Inkább toodownload XCode-projektjét, ez a minta helyett? [Töltse le a projekt](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.


## <a name="install-carthage-toodownload-and-build-msal"></a>Carthage toodownload telepítése és MSAL összeállítása
Carthage Csomagkezelő MSAL hello előzetes verzió időtartama során használt – hello lehetőségét, hogy a Microsoft toomake módosítások toohello könyvtár megőrzésével integrálható az XCode.

- Töltse le és telepítse a legújabb kiadásának hello Carthage [Itt](https://github.com/Carthage/Carthage/releases "Carthage letöltési URL-címe")

## <a name="creating-your-application"></a>Az alkalmazás létrehozása

1.  Nyissa meg az xcode-ban, és válassza ki`Create a new Xcode project`
2.  Válassza ki `iOS`  >  `Single view Application` kattintson *tovább*
3.  Nevezze el a termék, és kattintson a *következő*
4.  Válasszon egy mappát toocreate az alkalmazáshoz, majd kattintson *létrehozása*

## <a name="build-hello-msal-framework"></a>Hello MSAL keretrendszer összeállítása

Kövesse az alábbi toopull hello utasításokat, és majd kialakítható Carthage használatával MSAL könyvtárak legújabb verzióját hello:

1.  Nyissa meg a hello bash terminált, és toohello alkalmazás gyökérmappájában go
2.  Másolja az alábbi hello, és illessze be a hello bash terminál toocreate "Cartfile" fájlba:

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Másolja és illessze be az alábbi hello. Ez a parancs beolvassa a függőségek egy Carthage/kivételek mappába, majd összeállít hello MSAL könyvtár:
</li>
</ol>

```bash
carthage update
```

> a fenti hello folyamat használt toodownload és build hello Microsoft hitelesítési könyvtár (MSAL). MSAL kezeli az beszerzése, gyorsítótárazás, és a felhasználó használt a jogkivonatokat tooaccess védi hello Azure Active Directory v2 API-k frissítésekor.

## <a name="add-hello-msal-framework-tooyour-application"></a>Hello MSAL keretrendszer tooyour alkalmazás hozzáadása
1.  Az Xcode-ban nyissa meg a hello `General` lap
2.  Nyissa meg toohello `Linked Frameworks and Libraries` szakaszt, és kattintson`+`
3.  A következők szerint válasszon: `Add other…`
4.  Válassza a következőt: `Carthage`  >  `Build`  >  `iOS`  >  `MSAL.framework` kattintson *nyitott*. Megtekintheti az `MSAL.framework` toohello lista hozzá.
5.  Nyissa meg túl`Build Phases` fülre, majd `+` ikonra, válassza a`New Run Script Phase`
6.  Adja hozzá a következő tartalma toohello hello *parancsfájl-terület*:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Adja hozzá a hello túl a következő<code>Input Files</code> kattintva <code>+</code>:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Az alkalmazás felhasználói felület létrehozása
Egy Main.storyboard fájl automatikusan a projekt sablon részeként hozható létre. Kövesse az alábbi toocreate hello alkalmazás felhasználói felületén hello utasításokat:

1.  CTRL + kattintson `Main.storyboard` toobring hello helyi menü kialakításához, és kattintson:`Open As` > `Source Code`
2.  Cserélje le a hello `<scenes>` csomópont hello kódot:

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
