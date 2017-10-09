<span data-ttu-id="22bfa-101">Ebben a szakaszban frissíti kód a meglévő Mobile Apps háttér-projekt toosend leküldéses értesítés minden alkalommal, amikor egy új elem hozzá van adva.</span><span class="sxs-lookup"><span data-stu-id="22bfa-101">In this section, you update code in your existing Mobile Apps back-end project toosend a push notification every time a new item is added.</span></span> <span data-ttu-id="22bfa-102">Ez az hello technológiával [sablon](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funkció az Azure Notification Hubs, platformfüggetlen leküldéses értesítések engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="22bfa-102">This is powered by hello [template](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs, enabling cross-platform pushes.</span></span> <span data-ttu-id="22bfa-103">hello különböző ügyfelek be vannak jegyezve a sablonok használatával leküldéses értesítéseket, és egyetlen univerzális leküldéses kérheti le a tooall ügyfélplatformokon.</span><span class="sxs-lookup"><span data-stu-id="22bfa-103">hello various clients are registered for push notifications using templates, and a single universal push can get tooall client platforms.</span></span>

<span data-ttu-id="22bfa-104">Válasszon egyet az alábbi eljárásokat a háttér-projekt típusának megfelelő hello&mdash;vagy [.NET háttér](#dotnet) vagy [Node.js háttér](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="22bfa-104">Choose one of hello following procedures that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="22bfa-105"><a name="dotnet"></a>.NET háttér-projekt</span><span class="sxs-lookup"><span data-stu-id="22bfa-105"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="22bfa-106">A Visual Studióban, kattintson a jobb gombbal a projekt hello, és kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="22bfa-106">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="22bfa-107">Keresse meg `Microsoft.Azure.NotificationHubs`, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="22bfa-107">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="22bfa-108">Ezzel telepít hello Notification Hubs könyvtár az értesítések küldése a háttérből.</span><span class="sxs-lookup"><span data-stu-id="22bfa-108">This installs hello Notification Hubs library for sending notifications from your back end.</span></span>
2. <span data-ttu-id="22bfa-109">Hello projektben nyissa meg a **tartományvezérlők** > **TodoItemController.cs**, és adja hozzá hello következő using utasításokat:</span><span class="sxs-lookup"><span data-stu-id="22bfa-109">In hello server project, open **Controllers** > **TodoItemController.cs**, and add hello following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="22bfa-110">A hello **PostTodoItem** módszer, vegye fel a kód túl hello hívása után következő hello**InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="22bfa-110">In hello **PostTodoItem** method, add hello following code after hello call too**InsertAsync**:</span></span>  

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending hello message so that all template registrations that contain "messageParam"
        // will receive hello notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added toohello list.";

        try
        {
            // Send hello push notification and log hello results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write hello success result toohello logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write hello failure result toohello logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    <span data-ttu-id="22bfa-111">Ez a hello elemet tartalmazó sablon értesítést küld. Szöveg, ha egy új elem szerepel.</span><span class="sxs-lookup"><span data-stu-id="22bfa-111">This sends a template notification that contains hello item.Text when a new item is inserted.</span></span>
4. <span data-ttu-id="22bfa-112">Hello kiszolgálóprojektet közzé.</span><span class="sxs-lookup"><span data-stu-id="22bfa-112">Republish hello server project.</span></span>

### <span data-ttu-id="22bfa-113"><a name="nodejs"></a>NODE.js háttér-projekt</span><span class="sxs-lookup"><span data-stu-id="22bfa-113"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="22bfa-114">Ha még nem tette meg, [hello gyors üzembe helyezés háttér-projekt letöltése](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), vagy ellenkező esetben használja a hello [hello Azure-portálon az online szerkesztő](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="22bfa-114">If you haven't already done so, [download hello quickstart back-end project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use hello [online editor in hello Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="22bfa-115">Cserélje le a meglévő kód hello todoitem.js hello következőre:</span><span class="sxs-lookup"><span data-stu-id="22bfa-115">Replace hello existing code in todoitem.js with hello following:</span></span>

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget tooreturn hello results from hello context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    <span data-ttu-id="22bfa-116">Ez az új elem behelyezésekor hello item.text tartalmazó sablon értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="22bfa-116">This sends a template notification that contains hello item.text when a new item is inserted.</span></span>
3. <span data-ttu-id="22bfa-117">A helyi számítógépen hello fájl szerkesztésekor közzé hello kiszolgálóprojektet.</span><span class="sxs-lookup"><span data-stu-id="22bfa-117">When editing hello file on your local computer, republish hello server project.</span></span>
