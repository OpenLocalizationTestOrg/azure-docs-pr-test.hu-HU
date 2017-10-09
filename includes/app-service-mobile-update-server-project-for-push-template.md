Ebben a szakaszban frissíti kód a meglévő Mobile Apps háttér-projekt toosend leküldéses értesítés minden alkalommal, amikor egy új elem hozzá van adva. Ez az hello technológiával [sablon](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funkció az Azure Notification Hubs, platformfüggetlen leküldéses értesítések engedélyezése. hello különböző ügyfelek be vannak jegyezve a sablonok használatával leküldéses értesítéseket, és egyetlen univerzális leküldéses kérheti le a tooall ügyfélplatformokon.

Válasszon egyet az alábbi eljárásokat a háttér-projekt típusának megfelelő hello&mdash;vagy [.NET háttér](#dotnet) vagy [Node.js háttér](#nodejs).

### <a name="dotnet"></a>.NET háttér-projekt
1. A Visual Studióban, kattintson a jobb gombbal a projekt hello, és kattintson **NuGet-csomagok kezelése**. Keresse meg `Microsoft.Azure.NotificationHubs`, és kattintson a **telepítése**. Ezzel telepít hello Notification Hubs könyvtár az értesítések küldése a háttérből.
2. Hello projektben nyissa meg a **tartományvezérlők** > **TodoItemController.cs**, és adja hozzá hello következő using utasításokat:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. A hello **PostTodoItem** módszer, vegye fel a kód túl hello hívása után következő hello**InsertAsync**:  

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

    Ez a hello elemet tartalmazó sablon értesítést küld. Szöveg, ha egy új elem szerepel.
4. Hello kiszolgálóprojektet közzé.

### <a name="nodejs"></a>NODE.js háttér-projekt
1. Ha még nem tette meg, [hello gyors üzembe helyezés háttér-projekt letöltése](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), vagy ellenkező esetben használja a hello [hello Azure-portálon az online szerkesztő](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Cserélje le a meglévő kód hello todoitem.js hello következőre:

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

    Ez az új elem behelyezésekor hello item.text tartalmazó sablon értesítést küld.
3. A helyi számítógépen hello fájl szerkesztésekor közzé hello kiszolgálóprojektet.
