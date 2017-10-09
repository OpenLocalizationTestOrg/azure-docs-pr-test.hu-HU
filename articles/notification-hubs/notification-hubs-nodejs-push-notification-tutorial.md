---
title: "az Azure Notification Hubs és a Node.js aaaSending leküldéses értesítések"
description: "Tudnivalók a Notification Hubs toosend toouse a leküldéses értesítések Node.js-alkalmazás."
keywords: "leküldéses értesítési, leküldéses notifications,node.js leküldéses, ios leküldéses"
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: 151d224fa6dd07e4acdc3a4887c4e95ee03168c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a><span data-ttu-id="63ec8-104">Leküldéses értesítések küldése az Azure Notification Hubs és a Node.js</span><span class="sxs-lookup"><span data-stu-id="63ec8-104">Sending push notifications with Azure Notification Hubs and Node.js</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a><span data-ttu-id="63ec8-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="63ec8-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="63ec8-106">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="63ec8-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="63ec8-107">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="63ec8-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="63ec8-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span><span class="sxs-lookup"><span data-stu-id="63ec8-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span></span>
> 
> 

<span data-ttu-id="63ec8-109">Ez az útmutató bemutatja, mindez toosend a leküldéses értesítések hello segítségével az Azure Notification Hubs közvetlenül a Node.js-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="63ec8-109">This guide will show you how toosend push notifications with hello help of Azure Notification Hubs directly from a Node.js application.</span></span> 

<span data-ttu-id="63ec8-110">hello jelzett esetek tooapplications a leküldéses értesítések küldése a következő platformokon hello:</span><span class="sxs-lookup"><span data-stu-id="63ec8-110">hello scenarios covered include sending push notifications tooapplications on hello following platforms:</span></span>

* <span data-ttu-id="63ec8-111">Android</span><span class="sxs-lookup"><span data-stu-id="63ec8-111">Android</span></span>
* <span data-ttu-id="63ec8-112">iOS</span><span class="sxs-lookup"><span data-stu-id="63ec8-112">iOS</span></span>
* <span data-ttu-id="63ec8-113">Windows Phone</span><span class="sxs-lookup"><span data-stu-id="63ec8-113">Windows Phone</span></span>
* <span data-ttu-id="63ec8-114">Az univerzális Windows Platform</span><span class="sxs-lookup"><span data-stu-id="63ec8-114">Universal Windows Platform</span></span> 

<span data-ttu-id="63ec8-115">Notification hubs használatával kapcsolatban lásd: hello [lépések](#next) szakasz.</span><span class="sxs-lookup"><span data-stu-id="63ec8-115">For more information on notification hubs, see hello [Next Steps](#next) section.</span></span>

## <a name="what-are-notification-hubs"></a><span data-ttu-id="63ec8-116">Mi a Notification Hubs szolgáltatás?</span><span class="sxs-lookup"><span data-stu-id="63ec8-116">What are Notification Hubs?</span></span>
<span data-ttu-id="63ec8-117">Az Azure Notification Hubs egy egyszerűen használható, többplatformos, méretezhető infrastruktúrát biztosít a leküldéses értesítések toomobile eszközök.</span><span class="sxs-lookup"><span data-stu-id="63ec8-117">Azure Notification Hubs provide an easy-to-use, multi-platform, scalable infrastructure for sending push notifications toomobile devices.</span></span> <span data-ttu-id="63ec8-118">Hello szolgáltatás-infrastruktúrát a részletekért lásd: hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="63ec8-118">For details on hello service infrastructure, see hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) page.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="63ec8-119">Node.js-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="63ec8-119">Create a Node.js Application</span></span>
<span data-ttu-id="63ec8-120">hello első lépés az oktatóanyag egy új, üres Node.js-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="63ec8-120">hello first step in this tutorial is creating a new blank Node.js application.</span></span> <span data-ttu-id="63ec8-121">A Node.js-alkalmazás létrehozása, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás tooAzure webhely][nodejswebsite], [Node.js Felhőszolgáltatás] [ Node.js Cloud Service] Windows PowerShell használatával vagy [webhelyet a WebMatrix].</span><span class="sxs-lookup"><span data-stu-id="63ec8-121">For instructions on creating a Node.js application, see [Create and deploy a Node.js application tooAzure Web Site][nodejswebsite], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or [Web Site with WebMatrix].</span></span>

## <a name="configure-your-application-toouse-notification-hubs"></a><span data-ttu-id="63ec8-122">Alkalmazás tooUse Notification Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="63ec8-122">Configure Your Application tooUse Notification Hubs</span></span>
<span data-ttu-id="63ec8-123">Azure Notification Hubs toouse, és van szükség toodownload használata hello Node.js [azure-csomag](https://www.npmjs.com/package/azure), amely tartalmaz egy beépített hello leküldéses értesítési REST szolgáltatásokkal kommunikálni segítő szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="63ec8-123">toouse Azure Notification Hubs, you need toodownload and use hello Node.js [azure package](https://www.npmjs.com/package/azure), which includes a built-in set of helper libraries that communicate with hello push notification REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="63ec8-124">Csomópont Package Manager (NPM) tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="63ec8-124">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="63ec8-125">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Linux), és keresse meg a toohello mappa, amelyben létrehozta az üres alkalmazás .</span><span class="sxs-lookup"><span data-stu-id="63ec8-125">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Linux) and navigate toohello folder where you created your blank application.</span></span>
2. <span data-ttu-id="63ec8-126">Típus **npm telepítése azure-sb** hello parancssori ablakban.</span><span class="sxs-lookup"><span data-stu-id="63ec8-126">Type **npm install azure-sb** in hello command window.</span></span>
3. <span data-ttu-id="63ec8-127">Manuális futtatásával hello **ls** vagy **dir** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre.</span><span class="sxs-lookup"><span data-stu-id="63ec8-127">You can manually run hello **ls** or **dir** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="63ec8-128">A mappában található hello **azure** csomagot, amely tartalmaz hello szalagtárak tooaccess kell hello értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="63ec8-128">Inside that folder, find hello **azure** package, which contains hello libraries you need tooaccess hello Notification Hub.</span></span>

> [!NOTE]
> <span data-ttu-id="63ec8-129">További hello hivatalos NPM telepítéséről [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span><span class="sxs-lookup"><span data-stu-id="63ec8-129">You can learn more about installing NPM on hello official [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span></span> 
> 
> 

### <a name="import-hello-module"></a><span data-ttu-id="63ec8-130">Importálás hello modul</span><span class="sxs-lookup"><span data-stu-id="63ec8-130">Import hello module</span></span>
<span data-ttu-id="63ec8-131">Egy szövegszerkesztővel, adja hozzá a következő toohello felső részén hello hello **server.js** hello alkalmazás fájlt:</span><span class="sxs-lookup"><span data-stu-id="63ec8-131">Using a text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a><span data-ttu-id="63ec8-132">Az Azure Notification Hub-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="63ec8-132">Setup an Azure Notification Hub connection</span></span>
<span data-ttu-id="63ec8-133">Hello **NotificationHubService** objektum lehetővé teszi, hogy a notification hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="63ec8-133">hello **NotificationHubService** object lets you work with notification hubs.</span></span> <span data-ttu-id="63ec8-134">hello alábbi kód létrehoz egy **NotificationHubService** hello nofication központ nevű objektum **hubname**.</span><span class="sxs-lookup"><span data-stu-id="63ec8-134">hello following code creates a **NotificationHubService** object for hello nofication hub named **hubname**.</span></span> <span data-ttu-id="63ec8-135">Hello hello tetején adja hozzá **server.js** fájl hello utasítás tooimport hello azure modul után:</span><span class="sxs-lookup"><span data-stu-id="63ec8-135">Add it near hello top of hello **server.js** file, after hello statement tooimport hello azure module:</span></span>

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

<span data-ttu-id="63ec8-136">kapcsolat hello **connectionstring** érték hello szerezhető [Azure Portal] hello lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="63ec8-136">hello connection **connectionstring** value can be obtained from hello [Azure Portal] by performing hello following steps:</span></span>

1. <span data-ttu-id="63ec8-137">Hello bal oldali navigációs ablaktábláján kattintson **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="63ec8-137">In hello left navigation pane, click **Browse**.</span></span>
2. <span data-ttu-id="63ec8-138">Válassza ki **Notification Hubs**, és majd keresés hello hub hello minta toouse kívánja.</span><span class="sxs-lookup"><span data-stu-id="63ec8-138">Select **Notification Hubs**, and then find hello hub you wish toouse for hello sample.</span></span> <span data-ttu-id="63ec8-139">Olvassa el a toohello [Windows Store bevezető oktatóanyag](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Ha segítségre van szüksége egy új értesítési központ létrehozása.</span><span class="sxs-lookup"><span data-stu-id="63ec8-139">You can refer toohello [Windows Store Getting Started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) if you need help creating a new Notification Hub.</span></span>
3. <span data-ttu-id="63ec8-140">Válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="63ec8-140">Select **Settings**.</span></span>
4. <span data-ttu-id="63ec8-141">Kattintson a **hozzáférési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="63ec8-141">Click on **Access Policies**.</span></span> <span data-ttu-id="63ec8-142">Mindkét megosztott és teljes körű hozzáférést kapcsolati karakterláncok jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="63ec8-142">You will see both shared and full access connection strings.</span></span>

![Azure portál – Notification hubs használatával](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> <span data-ttu-id="63ec8-144">Beolvasni a hello kapcsolati karakterláncot hello **Get-AzureSbNamespace** parancsmag által biztosított [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy hello **azure sb névtér megjelenítése** parancsot Hello [Azure parancssori felület (CLI)](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="63ec8-144">You can also retrieve hello connection string using hello **Get-AzureSbNamespace** cmdlet provided by [Azure PowerShell](/powershell/azureps-cmdlets-docs) or hello **azure sb namespace show** command with hello [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md).</span></span>
> 
> 

## <a name="general-architecture"></a><span data-ttu-id="63ec8-145">Általános architektúra</span><span class="sxs-lookup"><span data-stu-id="63ec8-145">General architecture</span></span>
<span data-ttu-id="63ec8-146">Hello **NotificationHubService** vezérlőnek hello küldéséhez a leküldéses értesítések toospecific eszközök és alkalmazások figyelőobjektum-példányok a következő:</span><span class="sxs-lookup"><span data-stu-id="63ec8-146">hello **NotificationHubService** object exposes hello following object instances for sending push notifications toospecific devices and applications:</span></span>

* <span data-ttu-id="63ec8-147">**Android** -hello használata **GcmService** objektum, amely érhető el **notificationHubService.gcm**</span><span class="sxs-lookup"><span data-stu-id="63ec8-147">**Android** - use hello **GcmService** object, which is available at **notificationHubService.gcm**</span></span>
* <span data-ttu-id="63ec8-148">**iOS** -hello használata **ApnsService** objektum, amely elérhető a **notificationHubService.apns**</span><span class="sxs-lookup"><span data-stu-id="63ec8-148">**iOS** - use hello **ApnsService** object, which is accessible at **notificationHubService.apns**</span></span>
* <span data-ttu-id="63ec8-149">**Windows Phone** -hello használata **MpnsService** objektum, amely érhető el **notificationHubService.mpns**</span><span class="sxs-lookup"><span data-stu-id="63ec8-149">**Windows Phone** - use hello **MpnsService** object, which is available at **notificationHubService.mpns**</span></span>
* <span data-ttu-id="63ec8-150">**Univerzális Windows Platform** -hello használata **WnsService** objektum, amely érhető el **notificationHubService.wns**</span><span class="sxs-lookup"><span data-stu-id="63ec8-150">**Universal Windows Platform** - use hello **WnsService** object, which is available at **notificationHubService.wns**</span></span>

### <a name="how-to-send-push-notifications-tooandroid-applications"></a><span data-ttu-id="63ec8-151">Hogyan: leküldéses értesítések tooAndroid alkalmazások küldése</span><span class="sxs-lookup"><span data-stu-id="63ec8-151">How to: Send push notifications tooAndroid applications</span></span>
<span data-ttu-id="63ec8-152">Hello **GcmService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooAndroid alkalmazások lehet.</span><span class="sxs-lookup"><span data-stu-id="63ec8-152">hello **GcmService** object provides a **send** method that can be used toosend push notifications tooAndroid applications.</span></span> <span data-ttu-id="63ec8-153">Hello **küldése** metódus hello a következő paraméterek fogadja el:</span><span class="sxs-lookup"><span data-stu-id="63ec8-153">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="63ec8-154">**Címkék** -hello címke azonosítója.</span><span class="sxs-lookup"><span data-stu-id="63ec8-154">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="63ec8-155">Nincs címke van megadva, ha hello értesítést küld tooall ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="63ec8-155">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="63ec8-156">**Hasznos** -hello üzenet JSON vagy raw hasznos adatcsomag található.</span><span class="sxs-lookup"><span data-stu-id="63ec8-156">**Payload** - hello message's JSON or raw string payload.</span></span>
* <span data-ttu-id="63ec8-157">**A visszahívási** -hello visszahívási függvény.</span><span class="sxs-lookup"><span data-stu-id="63ec8-157">**Callback** - hello callback function.</span></span>

<span data-ttu-id="63ec8-158">Hello az adattartalom formátuma további információkért lásd: hello **hasznos** hello szakasza [GCM-kiszolgáló végrehajtási](http://developer.android.com/google/gcm/server.html#payload) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="63ec8-158">For more information on hello payload format, see hello **Payload** section of hello [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload) document.</span></span>

<span data-ttu-id="63ec8-159">hello következő használ a hello **GcmService** hello által elérhetővé tett példány **NotificationHubService** toosend egy leküldéses értesítési tooall regisztrált ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="63ec8-159">hello following code uses hello **GcmService** instance exposed by hello **NotificationHubService** toosend a push notification tooall registered clients.</span></span>

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-tooios-applications"></a><span data-ttu-id="63ec8-160">Hogyan: leküldéses értesítések tooiOS alkalmazások küldése</span><span class="sxs-lookup"><span data-stu-id="63ec8-160">How to: Send push notifications tooiOS applications</span></span>
<span data-ttu-id="63ec8-161">Ugyanaz, mint a fenti, az Android-alkalmazások hello **ApnsService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooiOS alkalmazások lehet.</span><span class="sxs-lookup"><span data-stu-id="63ec8-161">Same as with Android applications described above, hello **ApnsService** object provides a **send** method that can be used toosend push notifications tooiOS applications.</span></span> <span data-ttu-id="63ec8-162">Hello **küldése** metódus hello a következő paraméterek fogadja el:</span><span class="sxs-lookup"><span data-stu-id="63ec8-162">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="63ec8-163">**Címkék** -hello címke azonosítója.</span><span class="sxs-lookup"><span data-stu-id="63ec8-163">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="63ec8-164">Nincs címke van megadva, ha hello értesítést küld tooall ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="63ec8-164">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="63ec8-165">**Hasznos** - üzenet JSON hello vagy karakterlánc-hasznos.</span><span class="sxs-lookup"><span data-stu-id="63ec8-165">**Payload** - hello message's JSON or string payload.</span></span>
* <span data-ttu-id="63ec8-166">**A visszahívási** -hello visszahívási függvény.</span><span class="sxs-lookup"><span data-stu-id="63ec8-166">**Callback** - hello callback function.</span></span>

<span data-ttu-id="63ec8-167">További információ hello az adattartalom formátuma, lásd: hello **értesítési tartalom** hello szakasza [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="63ec8-167">For more information hello payload format, see hello **Notification Payload** section of hello [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document.</span></span>

<span data-ttu-id="63ec8-168">hello következő használ a hello **ApnsService** hello által elérhetővé tett példány **NotificationHubService** toosend riasztást message tooall ügyfelek:</span><span class="sxs-lookup"><span data-stu-id="63ec8-168">hello following code uses hello **ApnsService** instance exposed by hello **NotificationHubService** toosend an alert message tooall clients:</span></span>

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-toowindows-phone-applications"></a><span data-ttu-id="63ec8-169">Hogyan: küldése leküldéses értesítések tooWindows Phone rendszerre készült alkalmazások</span><span class="sxs-lookup"><span data-stu-id="63ec8-169">How to: Send push notifications tooWindows Phone applications</span></span>
<span data-ttu-id="63ec8-170">Hello **MpnsService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooWindows Phone rendszerre készült alkalmazások is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="63ec8-170">hello **MpnsService** object provides a **send** method that can be used toosend push notifications tooWindows Phone applications.</span></span> <span data-ttu-id="63ec8-171">Hello **küldése** metódus hello a következő paraméterek fogadja el:</span><span class="sxs-lookup"><span data-stu-id="63ec8-171">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="63ec8-172">**Címkék** -hello címke azonosítója.</span><span class="sxs-lookup"><span data-stu-id="63ec8-172">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="63ec8-173">Nincs címke van megadva, ha hello értesítést küld tooall ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="63ec8-173">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="63ec8-174">**Hasznos** -üzenet XML-adattartalmat hello.</span><span class="sxs-lookup"><span data-stu-id="63ec8-174">**Payload** - hello message's XML payload.</span></span>
* <span data-ttu-id="63ec8-175">**TargetName**  -  `toast` bejelentési értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="63ec8-175">**TargetName** - `toast` for toast notifications.</span></span> <span data-ttu-id="63ec8-176">`token`a csempe értesítések.</span><span class="sxs-lookup"><span data-stu-id="63ec8-176">`token` for tile notifications.</span></span>
* <span data-ttu-id="63ec8-177">**NotificationClass** -hello hello értesítési prioritását.</span><span class="sxs-lookup"><span data-stu-id="63ec8-177">**NotificationClass** - hello priority of hello notification.</span></span> <span data-ttu-id="63ec8-178">Lásd: hello **HTTP-fejléc elemek** hello szakasza [leküldéses értesítések kiszolgálóról](http://msdn.microsoft.com/library/hh221551.aspx) érvényes értékek a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="63ec8-178">See hello **HTTP Header Elements** section of hello [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) document for valid values.</span></span>
* <span data-ttu-id="63ec8-179">**Beállítások** – nem kötelező kérelem fejlécei.</span><span class="sxs-lookup"><span data-stu-id="63ec8-179">**Options** - optional request headers.</span></span>
* <span data-ttu-id="63ec8-180">**A visszahívási** -hello visszahívási függvény.</span><span class="sxs-lookup"><span data-stu-id="63ec8-180">**Callback** - hello callback function.</span></span>

<span data-ttu-id="63ec8-181">Érvényes listája **TargetName**, **NotificationClass** és fejléc lehetőségekről, tekintse meg a hello [leküldéses értesítések kiszolgálóról](http://msdn.microsoft.com/library/hh221551.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="63ec8-181">For a list of valid **TargetName**, **NotificationClass** and header options, check out hello [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) page.</span></span>

<span data-ttu-id="63ec8-182">a következő példakód hello használ hello **MpnsService** hello által elérhetővé tett példány **NotificationHubService** toosend egy bejelentési leküldéses értesítést:</span><span class="sxs-lookup"><span data-stu-id="63ec8-182">hello following sample code uses hello **MpnsService** instance exposed by hello **NotificationHubService** toosend a toast push notification:</span></span>

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-toouniversal-windows-platform-uwp-applications"></a><span data-ttu-id="63ec8-183">Hogyan: küldése leküldéses értesítések tooUniversal Windows-Platform (UWP) alkalmazások</span><span class="sxs-lookup"><span data-stu-id="63ec8-183">How to: Send push notifications tooUniversal Windows Platform (UWP) applications</span></span>
<span data-ttu-id="63ec8-184">Hello **WnsService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooUniversal Windows Platform alkalmazások lehet.</span><span class="sxs-lookup"><span data-stu-id="63ec8-184">hello **WnsService** object provides a **send** method that can be used toosend push notifications tooUniversal Windows Platform applications.</span></span>  <span data-ttu-id="63ec8-185">Hello **küldése** metódus hello a következő paraméterek fogadja el:</span><span class="sxs-lookup"><span data-stu-id="63ec8-185">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="63ec8-186">**Címkék** -hello címke azonosítója.</span><span class="sxs-lookup"><span data-stu-id="63ec8-186">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="63ec8-187">Nincs címke van megadva, ha hello értesítést küld regisztrált tooall ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="63ec8-187">If no tag is provided, hello notification will be sent tooall registered clients.</span></span>
* <span data-ttu-id="63ec8-188">**Hasznos** -hello XML üzenetadatokat.</span><span class="sxs-lookup"><span data-stu-id="63ec8-188">**Payload** - hello XML message payload.</span></span>
* <span data-ttu-id="63ec8-189">**Típus** -hello értesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="63ec8-189">**Type** - hello notification type.</span></span>
* <span data-ttu-id="63ec8-190">**Beállítások** – nem kötelező kérelem fejlécei.</span><span class="sxs-lookup"><span data-stu-id="63ec8-190">**Options** - optional request headers.</span></span>
* <span data-ttu-id="63ec8-191">**A visszahívási** -hello visszahívási függvény.</span><span class="sxs-lookup"><span data-stu-id="63ec8-191">**Callback** - hello callback function.</span></span>

<span data-ttu-id="63ec8-192">Az érvényes típusok és kérelemfejléc listájáért lásd: [leküldéses értesítési szolgáltatás kérés- és válaszfejlécekről](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span><span class="sxs-lookup"><span data-stu-id="63ec8-192">For a list of valid types and request headers, see [Push notification service request and response headers](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span></span>

<span data-ttu-id="63ec8-193">hello következő használ a hello **WnsService** hello által elérhetővé tett példány **NotificationHubService** toosend egy bejelentési leküldéses értesítési tooa UWP-alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="63ec8-193">hello following code uses hello **WnsService** instance exposed by hello **NotificationHubService** toosend a toast push notification tooa UWP app:</span></span>

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a><span data-ttu-id="63ec8-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="63ec8-194">Next Steps</span></span>
<span data-ttu-id="63ec8-195">hello minta kódtöredékek fenti meg tooeasily build szolgáltatás infrastruktúra toodeliver leküldéses értesítések tooa számos különféle eszközök engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="63ec8-195">hello sample snippets above allow you tooeasily build service infrastructure toodeliver push notifications tooa wide variety of devices.</span></span> <span data-ttu-id="63ec8-196">Most, hogy megismerte a Notification Hubs használata node.js hello alapjait, kövesse ezeket hivatkozások toolearn további kapcsolatban, hogyan bővítheti a további ezeket a képességeket.</span><span class="sxs-lookup"><span data-stu-id="63ec8-196">Now that you've learned hello basics of using Notification Hubs with node.js, follow these links toolearn more about how you can extend these capabilities further.</span></span>

* <span data-ttu-id="63ec8-197">Lásd az MSDN-hivatkozást hello [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span><span class="sxs-lookup"><span data-stu-id="63ec8-197">See hello MSDN Reference for [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span></span>
* <span data-ttu-id="63ec8-198">A Microsoft hello [Azure SDK for csomópont] GitHub tárházából további mintákat és megvalósítás részletei.</span><span class="sxs-lookup"><span data-stu-id="63ec8-198">Visit hello [Azure SDK for Node] repository on GitHub for more samples and implementation details.</span></span>

[Azure SDK for csomópont]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain hello Default Management Credentials for hello Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application tooUse Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages tooa Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[webhelyet a WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
