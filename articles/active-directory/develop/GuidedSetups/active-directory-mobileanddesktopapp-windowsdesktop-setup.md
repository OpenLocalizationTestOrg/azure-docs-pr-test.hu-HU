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
## <a name="set-up-your-project"></a><span data-ttu-id="dc72f-103">A projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="dc72f-103">Set up your project</span></span>

<span data-ttu-id="dc72f-104">Ez a témakör részletes utasításokkal szolgál egy új projekt toodemonstrate toocreate hogyan toointegrate a Windows asztali .NET (XAML) alkalmazás *jelentkezzen be Microsoft* azt lekérdezhesse jogkivonat szükséges, webes API-k.</span><span class="sxs-lookup"><span data-stu-id="dc72f-104">This section provides step-by-step instructions for how toocreate a new project toodemonstrate how toointegrate a Windows Desktop .NET application (XAML) with *Sign-In with Microsoft* so it can query Web APIs that requires a token.</span></span>

<span data-ttu-id="dc72f-105">Ez az útmutató által létrehozott hello alkalmazás közzététele a képernyőn és kijelentkezési gomb gomb toograph és az eredmény.</span><span class="sxs-lookup"><span data-stu-id="dc72f-105">hello application created by this guide exposes a button toograph and show results on screen and a sign-out button.</span></span>

> <span data-ttu-id="dc72f-106">Inkább toodownload Ez a minta Visual Studio-projekt helyett?</span><span class="sxs-lookup"><span data-stu-id="dc72f-106">Prefer toodownload this sample's Visual Studio project instead?</span></span> <span data-ttu-id="dc72f-107">[Töltse le a projekt](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="dc72f-107">[Download a project](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>


### <a name="create-your-application"></a><span data-ttu-id="dc72f-108">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="dc72f-108">Create your application</span></span>
1. <span data-ttu-id="dc72f-109">A Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="dc72f-109">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
2. <span data-ttu-id="dc72f-110">A *sablonok*, jelölje be`Visual C#`</span><span class="sxs-lookup"><span data-stu-id="dc72f-110">Under *Templates*, select `Visual C#`</span></span>
3. <span data-ttu-id="dc72f-111">Válassza ki `WPF App` (vagy *WPF-alkalmazás* attól függően, hogy a Visual Studio hello verziója)</span><span class="sxs-lookup"><span data-stu-id="dc72f-111">Select `WPF App` (or *WPF Application* depending on hello version of your Visual Studio)</span></span>

## <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a><span data-ttu-id="dc72f-112">Hello Microsoft hitelesítési könyvtár (MSAL) tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dc72f-112">Add hello Microsoft Authentication Library (MSAL) tooyour project</span></span>
1. <span data-ttu-id="dc72f-113">A Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="dc72f-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="dc72f-114">Másolja és illessze be a következő hello a hello Package Manager Console ablakban:</span><span class="sxs-lookup"><span data-stu-id="dc72f-114">Copy/paste hello following in hello Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> <span data-ttu-id="dc72f-115">a fenti hello a csomag telepíti a hello Microsoft hitelesítési könyvtár (MSAL).</span><span class="sxs-lookup"><span data-stu-id="dc72f-115">hello package above installs hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="dc72f-116">MSAL kezeli az beszerzése, gyorsítótárazás, és a felhasználó használt toskens tooaccess védi az Azure Active Directory v2 API-k frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="dc72f-116">MSAL handles acquiring, caching and refreshing user toskens used tooaccess APIs protected by Azure Active Directory v2.</span></span>

## <a name="add-hello-code-tooinitialize-msal"></a><span data-ttu-id="dc72f-117">Hello kód tooinitialize MSAL hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dc72f-117">Add hello code tooinitialize MSAL</span></span>
<span data-ttu-id="dc72f-118">Ebben a lépésben segítségével hozhat létre egy osztály toohandle interakcióba MSAL könyvtárba, például a jogkivonatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="dc72f-118">This step will help you create a class toohandle interaction with MSAL Library, such as handling of tokens.</span></span>

1. <span data-ttu-id="dc72f-119">Nyissa meg hello `App.xaml.cs` fájlt, és adjon hozzá MSAL könyvtár toohello osztály hello hivatkozást:</span><span class="sxs-lookup"><span data-stu-id="dc72f-119">Open hello `App.xaml.cs` file and add hello reference for MSAL library toohello class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="dc72f-120">Hello App osztály toohello következő frissítés:</span><span class="sxs-lookup"><span data-stu-id="dc72f-120">Update hello App class toohello following:</span></span>
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

## <a name="create-your-applications-ui"></a><span data-ttu-id="dc72f-121">Az alkalmazás felhasználói Felületüket létrehozni</span><span class="sxs-lookup"><span data-stu-id="dc72f-121">Create your application’s UI</span></span>
<span data-ttu-id="dc72f-122">az alábbi hello szakasz bemutatja, hogyan tudja lekérdezni egy alkalmazás például a Microsoft Graph védett háttérkiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="dc72f-122">hello section below shows how an application can query a protected backend server like Microsoft Graph.</span></span> <span data-ttu-id="dc72f-123">Egy MainWindow.xaml fájl automatikusan a projekt sablon részeként hozható létre.</span><span class="sxs-lookup"><span data-stu-id="dc72f-123">A MainWindow.xaml file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="dc72f-124">Ezt a fájlt megnyitni a fájlt, és kövesse az alábbi hello utasításokat:</span><span class="sxs-lookup"><span data-stu-id="dc72f-124">Open this file this file and then follow hello instructions below:</span></span>

<span data-ttu-id="dc72f-125">Cserélje le az alkalmazás `<Grid>` hello következő lesz:</span><span class="sxs-lookup"><span data-stu-id="dc72f-125">Replace your application’s `<Grid>` with be hello following:</span></span>

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
