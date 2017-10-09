---
title: "aaaAzure AD v2 Windows asztali bevezetés - telepítő |} Microsoft Docs"
description: "Hogyan Windows asztali .NET (XAML) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
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
ms.openlocfilehash: 097ea99bef01e15edaa5ff914ff4e18392b77c5a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a>A projekt beállítása

Ez a témakör részletes utasításokkal szolgál egy új projekt toodemonstrate toocreate hogyan toointegrate a Windows asztali .NET (XAML) alkalmazás *jelentkezzen be Microsoft* azt lekérdezhesse jogkivonat szükséges, webes API-k.

Ez az útmutató által létrehozott hello alkalmazás közzététele a képernyőn és kijelentkezési gomb gomb toograph és az eredmény.

> Inkább toodownload Ez a minta Visual Studio-projekt helyett? [Töltse le a projekt](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.


### <a name="create-your-application"></a>Az alkalmazás létrehozása
1. A Visual Studio:`File` > `New` > `Project`<br/>
2. A *sablonok*, jelölje be`Visual C#`
3. Válassza ki `WPF App` (vagy *WPF-alkalmazás* attól függően, hogy a Visual Studio hello verziója)

## <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a>Hello Microsoft hitelesítési könyvtár (MSAL) tooyour projekt hozzáadása
1. A Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Másolja és illessze be a következő hello a hello Package Manager Console ablakban:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> a fenti hello a csomag telepíti a hello Microsoft hitelesítési könyvtár (MSAL). MSAL kezeli az beszerzése, gyorsítótárazás, és a felhasználó használt toskens tooaccess védi az Azure Active Directory v2 API-k frissítésekor.

## <a name="add-hello-code-tooinitialize-msal"></a>Hello kód tooinitialize MSAL hozzáadása
Ebben a lépésben segítségével hozhat létre egy osztály toohandle interakcióba MSAL könyvtárba, például a jogkivonatok kezelése.

1. Nyissa meg hello `App.xaml.cs` fájlt, és adjon hozzá MSAL könyvtár toohello osztály hello hivatkozást:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Hello App osztály toohello következő frissítés:
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is hello clientId of your app registration. 
    //You have tooreplace hello below with hello Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a>Az alkalmazás felhasználói Felületüket létrehozni
az alábbi hello szakasz bemutatja, hogyan tudja lekérdezni egy alkalmazás például a Microsoft Graph védett háttérkiszolgálóhoz. Egy MainWindow.xaml fájl automatikusan a projekt sablon részeként hozható létre. Ezt a fájlt megnyitni a fájlt, és kövesse az alábbi hello utasításokat:

Cserélje le az alkalmazás `<Grid>` hello következő lesz:

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
