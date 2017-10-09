<span data-ttu-id="c1451-101">A háttér-projekt típusának megfelelő hello eljárás&mdash;vagy [.NET háttér](#dotnet) vagy [Node.js háttér](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="c1451-101">Use hello procedure that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="c1451-102"><a name="dotnet"></a>.NET háttér-projekt</span><span class="sxs-lookup"><span data-stu-id="c1451-102"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="c1451-103">A Visual Studióban, kattintson a jobb gombbal a hello kiszolgálóprojektet, majd kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c1451-103">In Visual Studio, right-click hello server project, and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="c1451-104">Keresse meg `Microsoft.Azure.NotificationHubs`, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="c1451-104">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="c1451-105">Ezzel telepít hello Notification Hubs ügyféloldali kódtárára.</span><span class="sxs-lookup"><span data-stu-id="c1451-105">This installs hello Notification Hubs client library.</span></span>
2. <span data-ttu-id="c1451-106">Hello tartományvezérlők mappában nyissa meg a TodoItemController.cs, és adja hozzá a következő hello `using` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="c1451-106">In hello Controllers folder, open TodoItemController.cs and add hello following `using` statements:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;
3. <span data-ttu-id="c1451-107">Cserélje le a hello `PostTodoItem` hello kód a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="c1451-107">Replace hello `PostTodoItem` method with hello following code:</span></span>  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send hello push notification and log hello results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write hello success result toohello logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write hello failure result toohello logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. <span data-ttu-id="c1451-108">Hello kiszolgálóprojektet közzé.</span><span class="sxs-lookup"><span data-stu-id="c1451-108">Republish hello server project.</span></span>

### <span data-ttu-id="c1451-109"><a name="nodejs"></a>NODE.js háttér-projekt</span><span class="sxs-lookup"><span data-stu-id="c1451-109"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="c1451-110">Ha még nem tette meg, [hello gyorsútmutató-projekt letöltése](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), vagy ellenkező esetben használja a hello [hello Azure-portálon az online szerkesztő](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="c1451-110">If you haven't already done so, [download hello quickstart project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use hello [online editor in hello Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="c1451-111">Cserélje le a meglévő kódot hello hello todoitem.js fájlban hello következőre:</span><span class="sxs-lookup"><span data-stu-id="c1451-111">Replace hello existing code in hello todoitem.js file with hello following:</span></span>

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    <span data-ttu-id="c1451-112">Ez egy új teendőelemet behelyezésekor hello item.text tartalmazó GCM értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="c1451-112">This sends a GCM notification that contains hello item.text when a new todo item is inserted.</span></span>
3. <span data-ttu-id="c1451-113">A helyi számítógép hello fájl szerkesztésekor közzé hello kiszolgálóprojektet.</span><span class="sxs-lookup"><span data-stu-id="c1451-113">When editing hello file in your local computer, republish hello server project.</span></span>
