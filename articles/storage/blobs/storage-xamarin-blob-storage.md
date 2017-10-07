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
ms.openlocfilehash: 688484fc560b5c89ed1692f5cbf5713aa8fc90a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-xamarin"></a>Hogyan toouse Blob Storage-ának Xamarin
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Áttekintés
Xamarin lehetővé teszi, hogy a fejlesztők toouse megosztott C# kódbázis toocreate iOS, Android és Windows Áruházbeli alkalmazások a a saját felhasználói felülethez. Az oktatóanyag bemutatja, hogyan toouse Xamarin-alkalmazás az Azure Blob Storage tárolóban. Ha azt szeretné, hogy Azure Storage-ról további toolearn hello kód ról előtt, tekintse meg a [Azure Storage bemutatása tooMicrosoft](../common/storage-introduction.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Új Xamarin-alkalmazás létrehozása
Ebben az oktatóanyagban azt fogja létrehozni egy alkalmazást, amelynek célja az Android, iOS és Windows. Az alkalmazás egyszerűen tárolókat hozhat létre, és egy blob feltöltése a tárolóba. Fogjuk használni a Visual Studio a Windows, de hello azonos learnings alkalmazhatók a Xamarin Studio segítségével macOS az alkalmazás létrehozásakor.

Kövesse ezeket a lépéseket toocreate az alkalmazást:

1. Ha még nem tette, töltse le és telepítse [Visual Studio Xamarin](https://www.xamarin.com/download).
2. Nyissa meg a Visual Studio, és hozzon létre egy üres alkalmazás (natív hordozható): **fájl > Új > Projekt > platformfüggetlen > üres App(Native Portable)**.
3. Kattintson a jobb gombbal a megoldás hello Solution Explorer ablaktáblában, és válassza ki **NuGet-csomagok kezelése megoldáshoz**. Keresse meg **windowsazure.Storage kifejezésre** és telepítse a legújabb stabil verzióját tooall projektek hello a megoldásban.
4. Hozza létre, és futtatja a projektet.

Most rendelkeznie kell egy alkalmazás, amely lehetővé teszi a gomb, ami növeli a számlálót tooclick.

## <a name="create-container-and-upload-blob"></a>Tároló létrehozása és feltöltése a blob
Ezután bontsa a `(Portable)` projekt, néhány kódot túl fogja hozzáadni`MyClass.cs`. Ez a kód létrehoz egy tárolót, és feltölt egy blobot a tárolóba. `MyClass.cs`hello hasonlóan kell kinéznie:

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

Győződjön meg arról, hogy tooreplace "your_account_name_here" és "your_account_key_here" a tényleges fióknevet és a fiókkulcsot. 

Az iOS, Android és Windows Phone-projektek összes rendelkezik hivatkozások tooyour hordozható projekt – azaz írhat összes megosztott kódot egy helyezze el, és használja az összes projektben. Mostantól hozzáadhatja a következő kód tooeach projekt toostart kihasználva üzletági hello:`MyClass.performBlobOperation()`

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

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

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

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

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

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

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
Most futtathatja az alkalmazást egy Android- vagy Windows Phone-emulátoron. Ez az alkalmazás egy iOS-emulátoron is futtatható, de ehhez az szükséges, Mac Hogyan toodo, olvassa el az hello dokumentációjában kapcsolatos tudnivalókat [Visual Studio tooa Mac csatlakozás](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Az alkalmazás futtatása után hello tároló hoz létre `mycontainer` tárfiókba. Hello blob tartalmaznia kell `myblob`, hello szöveget tartalmaz `Hello, world!`. Hello segítségével ellenőrizheti a [Microsoft Azure Tártallózó](http://storageexplorer.com/).

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban megtanulta, hogyan toocreate egy platformfüggetlen-alkalmazást a Xamarin használó Azure Storage Blob Storage egy forgatókönyv kifejezetten előtérbe. Azonban végezhet sokkal több nem csak a Blob Storage a, hanem a tábla, a fájl és a Queue Storage. Ellenőrizze a következő cikkek toolearn további hello ki:

* [Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel](storage-dotnet-how-to-use-blobs.md)
* [Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [Az Azure Queue Storage használatának első lépései a .NET-keretrendszerrel](../queues/storage-dotnet-how-to-use-queues.md)
* [Ismerkedés a Windowshoz készült Azure File Storage szolgáltatással](../files/storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

