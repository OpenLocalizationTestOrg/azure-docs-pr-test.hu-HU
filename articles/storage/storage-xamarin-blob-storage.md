---
title: "Blob Storage-ának Xamarin aaaHow toouse |} Microsoft Docs"
description: "hello Azure Storage ügyféloldali kódtára a Xamarin lehetővé teszi a fejlesztők toocreate iOS, Android és Windows Áruházbeli alkalmazások a a saját felhasználói felülethez. Ez az oktatóanyag bemutatja, hogyan toouse Xamarin toocreate Azure Blob Storage tárolót használó alkalmazások."
services: storage
documentationcenter: xamarin
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 44cb845d-cf78-4942-95b8-952da4f9a2c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 751f66d1d2392c8bcf6e5f8d1b185af73582fab3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-xamarin"></a><span data-ttu-id="2b494-104">Hogyan toouse Blob Storage-ának Xamarin</span><span class="sxs-lookup"><span data-stu-id="2b494-104">How toouse Blob Storage from Xamarin</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a><span data-ttu-id="2b494-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2b494-105">Overview</span></span>
<span data-ttu-id="2b494-106">Xamarin lehetővé teszi, hogy a fejlesztők toouse megosztott C# kódbázis toocreate iOS, Android és Windows Áruházbeli alkalmazások a a saját felhasználói felülethez.</span><span class="sxs-lookup"><span data-stu-id="2b494-106">Xamarin enables developers toouse a shared C# codebase toocreate iOS, Android, and Windows Store apps with their native user interfaces.</span></span> <span data-ttu-id="2b494-107">Az oktatóanyag bemutatja, hogyan toouse Xamarin-alkalmazás az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2b494-107">This tutorial shows you how toouse Azure Blob storage with a Xamarin application.</span></span> <span data-ttu-id="2b494-108">Ha azt szeretné, hogy Azure Storage-ról további toolearn hello kód ról előtt, tekintse meg a [Azure Storage bemutatása tooMicrosoft](storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2b494-108">If you'd like toolearn more about Azure Storage, before diving into hello code, see [Introduction tooMicrosoft Azure Storage](storage-introduction.md).</span></span>

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a><span data-ttu-id="2b494-109">Új Xamarin-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b494-109">Create a new Xamarin Application</span></span>
<span data-ttu-id="2b494-110">Ebben az oktatóanyagban azt fogja létrehozni egy alkalmazást, amelynek célja az Android, iOS és Windows.</span><span class="sxs-lookup"><span data-stu-id="2b494-110">For this tutorial, we'll be creating an app that targets Android, iOS, and Windows.</span></span> <span data-ttu-id="2b494-111">Az alkalmazás egyszerűen tárolókat hozhat létre, és egy blob feltöltése a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="2b494-111">This app will simply create a container and upload a blob into this container.</span></span> <span data-ttu-id="2b494-112">Fogjuk használni a Visual Studio a Windows, de hello azonos learnings alkalmazhatók a Xamarin Studio segítségével macOS az alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2b494-112">We'll be using Visual Studio on Windows, but hello same learnings can be applied when creating an app using Xamarin Studio on macOS.</span></span>

<span data-ttu-id="2b494-113">Kövesse ezeket a lépéseket toocreate az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="2b494-113">Follow these steps toocreate your application:</span></span>

1. <span data-ttu-id="2b494-114">Ha még nem tette, töltse le és telepítse [Visual Studio Xamarin](https://www.xamarin.com/download).</span><span class="sxs-lookup"><span data-stu-id="2b494-114">If you haven't already, download and install [Xamarin for Visual Studio](https://www.xamarin.com/download).</span></span>
2. <span data-ttu-id="2b494-115">Nyissa meg a Visual Studio, és hozzon létre egy üres alkalmazás (natív hordozható): **fájl > Új > Projekt > platformfüggetlen > üres App(Native Portable)**.</span><span class="sxs-lookup"><span data-stu-id="2b494-115">Open Visual Studio, and create a Blank App (Native Portable): **File > New > Project > Cross-Platform > Blank App(Native Portable)**.</span></span>
3. <span data-ttu-id="2b494-116">Kattintson a jobb gombbal a megoldás hello Solution Explorer ablaktáblában, és válassza ki **NuGet-csomagok kezelése megoldáshoz**.</span><span class="sxs-lookup"><span data-stu-id="2b494-116">Right-click your solution in hello Solution Explorer pane and select **Manage NuGet Packages for Solution**.</span></span> <span data-ttu-id="2b494-117">Keresse meg **windowsazure.Storage kifejezésre** és telepítse a legújabb stabil verzióját tooall projektek hello a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="2b494-117">Search for **WindowsAzure.Storage** and install hello latest stable version tooall projects in your solution.</span></span>
4. <span data-ttu-id="2b494-118">Hozza létre, és futtatja a projektet.</span><span class="sxs-lookup"><span data-stu-id="2b494-118">Build and run your project.</span></span>

<span data-ttu-id="2b494-119">Most rendelkeznie kell egy alkalmazás, amely lehetővé teszi a gomb, ami növeli a számlálót tooclick.</span><span class="sxs-lookup"><span data-stu-id="2b494-119">You should now have an application that allows you tooclick a button which increments a counter.</span></span>

## <a name="create-container-and-upload-blob"></a><span data-ttu-id="2b494-120">Tároló létrehozása és feltöltése a blob</span><span class="sxs-lookup"><span data-stu-id="2b494-120">Create container and upload blob</span></span>
<span data-ttu-id="2b494-121">Ezután bontsa a `(Portable)` projekt, néhány kódot túl fogja hozzáadni`MyClass.cs`.</span><span class="sxs-lookup"><span data-stu-id="2b494-121">Next, under your `(Portable)` project, you'll add some code too`MyClass.cs`.</span></span> <span data-ttu-id="2b494-122">Ez a kód létrehoz egy tárolót, és feltölt egy blobot a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="2b494-122">This code creates a container and uploads a blob into this container.</span></span> <span data-ttu-id="2b494-123">`MyClass.cs`hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="2b494-123">`MyClass.cs` should look like hello following:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System.Threading.Tasks;

namespace XamarinApp
{
    public class MyClass
    {
        public MyClass ()
        {
        }

        public static async Task performBlobOperation()
        {
            // Retrieve storage account from connection string.
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

            // Create hello blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference tooa previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create hello container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference tooa blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create hello "myblob" blob with hello text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

<span data-ttu-id="2b494-124">Győződjön meg arról, hogy tooreplace "your_account_name_here" és "your_account_key_here" a tényleges fióknevet és a fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="2b494-124">Make sure tooreplace "your_account_name_here" and "your_account_key_here" with your actual account name and account key.</span></span> 

<span data-ttu-id="2b494-125">Az iOS, Android és Windows Phone-projektek összes rendelkezik hivatkozások tooyour hordozható projekt – azaz írhat összes megosztott kódot egy helyezze el, és használja az összes projektben.</span><span class="sxs-lookup"><span data-stu-id="2b494-125">Your iOS, Android, and Windows Phone projects all have references tooyour Portable project - meaning you can write all of your shared code in one place and use it across all of your projects.</span></span> <span data-ttu-id="2b494-126">Mostantól hozzáadhatja a következő kód tooeach projekt toostart kihasználva üzletági hello:`MyClass.performBlobOperation()`</span><span class="sxs-lookup"><span data-stu-id="2b494-126">You can now add hello following line of code tooeach project toostart taking advantage: `MyClass.performBlobOperation()`</span></span>

### <a name="xamarinappdroid--mainactivitycs"></a><span data-ttu-id="2b494-127">XamarinApp.Droid > MainActivity.cs</span><span class="sxs-lookup"><span data-stu-id="2b494-127">XamarinApp.Droid > MainActivity.cs</span></span>

```csharp
using Android.App;
using Android.Widget;
using Android.OS;

namespace XamarinApp.Droid
{
    [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        int count = 1;

        protected override async void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from hello "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from hello layout resource,
            // and attach an event tooit
            Button button = FindViewById<Button> (Resource.Id.myButton);

            button.Click += delegate {
                button.Text = string.Format ("{0} clicks!", count++);
            };

            await MyClass.performBlobOperation();
            }
        }
    }
}
```

### <a name="xamarinappios--viewcontrollercs"></a><span data-ttu-id="2b494-128">XamarinApp.iOS > ViewController.cs</span><span class="sxs-lookup"><span data-stu-id="2b494-128">XamarinApp.iOS > ViewController.cs</span></span>

```csharp
using System;
using UIKit;

namespace XamarinApp.iOS
{
    public partial class ViewController : UIViewController
    {
        int count = 1;

        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override async void ViewDidLoad ()
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading hello view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.performBlobOperation();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a><span data-ttu-id="2b494-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span><span class="sxs-lookup"><span data-stu-id="2b494-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span></span>

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// hello Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated toowithin a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        int count = 1;

        public MainPage()
        {
            this.InitializeComponent();

            this.NavigationCacheMode = NavigationCacheMode.Required;
        }

        /// <summary>
        /// Invoked when this page is about toobe displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used tooconfigure hello page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about toobe displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used tooconfigure hello page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling hello hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using hello NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.performBlobOperation();
            }
        }
    }
}
```

## <a name="run-hello-application"></a><span data-ttu-id="2b494-130">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="2b494-130">Run hello application</span></span>
<span data-ttu-id="2b494-131">Most futtathatja az alkalmazást egy Android- vagy Windows Phone-emulátoron.</span><span class="sxs-lookup"><span data-stu-id="2b494-131">You can now run this application in an Android or Windows Phone emulator.</span></span> <span data-ttu-id="2b494-132">Ez az alkalmazás egy iOS-emulátoron is futtatható, de ehhez az szükséges, Mac</span><span class="sxs-lookup"><span data-stu-id="2b494-132">You can also run this application in an iOS emulator, but this will require a Mac.</span></span> <span data-ttu-id="2b494-133">Hogyan toodo, olvassa el az hello dokumentációjában kapcsolatos tudnivalókat [Visual Studio tooa Mac csatlakozás](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span><span class="sxs-lookup"><span data-stu-id="2b494-133">For specific instructions on how toodo this, please read hello documentation for [connecting Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span></span>

<span data-ttu-id="2b494-134">Az alkalmazás futtatása után hello tároló hoz létre `mycontainer` tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="2b494-134">Once you run your app, it will create hello container `mycontainer` in your Storage account.</span></span> <span data-ttu-id="2b494-135">Hello blob tartalmaznia kell `myblob`, hello szöveget tartalmaz `Hello, world!`.</span><span class="sxs-lookup"><span data-stu-id="2b494-135">It should contain hello blob, `myblob`, which has hello text, `Hello, world!`.</span></span> <span data-ttu-id="2b494-136">Hello segítségével ellenőrizheti a [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="2b494-136">You can verify this by using hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b494-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b494-137">Next steps</span></span>
<span data-ttu-id="2b494-138">Ebben az oktatóanyagban megtanulta, hogyan toocreate egy platformfüggetlen-alkalmazást a Xamarin használó Azure Storage Blob Storage egy forgatókönyv kifejezetten előtérbe.</span><span class="sxs-lookup"><span data-stu-id="2b494-138">In this tutorial, you learned how toocreate a cross-platform application in Xamarin that uses Azure Storage, specifically focusing on one scenario in Blob Storage.</span></span> <span data-ttu-id="2b494-139">Azonban végezhet sokkal több nem csak a Blob Storage a, hanem a tábla, a fájl és a Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="2b494-139">However, you can do a lot more with not only Blob Storage, but also with Table, File, and Queue Storage.</span></span> <span data-ttu-id="2b494-140">Ellenőrizze a következő cikkek toolearn további hello ki:</span><span class="sxs-lookup"><span data-stu-id="2b494-140">Please check out hello following articles toolearn more:</span></span>

* [<span data-ttu-id="2b494-141">Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="2b494-141">Get started with Azure Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="2b494-142">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="2b494-142">Get started with Azure Table storage using .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="2b494-143">Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="2b494-143">Get started with Azure Queue storage using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="2b494-144">Ismerkedés a Windowshoz készült Azure File Storage szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="2b494-144">Get started with Azure File storage on Windows</span></span>](storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

