
1. <span data-ttu-id="2a038-101">Az a [Azure-portálon](https://portal.azure.com/), kattintson a **összes tallózása** > **alkalmazásszolgáltatások**, és kattintson a Mobile Apps háttér.</span><span class="sxs-lookup"><span data-stu-id="2a038-101">In the [Azure portal](https://portal.azure.com/), click **Browse All** > **App Services**, and then click your Mobile Apps back end.</span></span> <span data-ttu-id="2a038-102">A **beállítások**, kattintson a **App Service leküldéses**, majd kattintson az értesítési központ nevére.</span><span class="sxs-lookup"><span data-stu-id="2a038-102">Under **Settings**, click **App Service Push**, and then click your notification hub name.</span></span>
2. <span data-ttu-id="2a038-103">Ugrás a **Google (GCM)**, adja meg a **Kiszolgálókulcs** érték, amely az előző eljárásban Firebase kapott, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2a038-103">Go to **Google (GCM)**, enter the **Server Key** value that you obtained from Firebase in the previous procedure, and then click **Save**.</span></span>

    ![Állítsa be a GCM API-kulcsot a portálon](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

<span data-ttu-id="2a038-105">A Mobile Apps háttér most Firebase Cloud Messaging használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="2a038-105">The Mobile Apps back end is now configured to use Firebase Cloud Messaging.</span></span> <span data-ttu-id="2a038-106">Ez lehetővé teszi, hogy az alkalmazás Android-eszközön fut, az értesítési központ használatával leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a038-106">This enables you to send push notifications to your app running on an Android device, by using the notification hub.</span></span>

<!-- URLs. -->


<!-- images -->
