
1. <span data-ttu-id="d1ab9-101">Lépjen be a [Google Cloud Console](https://console.developers.google.com/project) (Google felhőkonzol) felületére, és jelentkezzen be Google-fiókja hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-101">Navigate to the [Google Cloud Console](https://console.developers.google.com/project), sign in with your Google account credentials.</span></span> 
2. <span data-ttu-id="d1ab9-102">Kattintson a **Create Project** (Projekt létrehozása) elemre, adjon meg egy nevet a projekthez, és kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-102">Click **Create Project**, type a project name, then click **Create**.</span></span> <span data-ttu-id="d1ab9-103">Ha a rendszer kéri, végezze el az SMS-es hitelesítést, és kattintson újra a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-103">If requested, carry out the SMS Verification, and click **Create** again.</span></span>
   
    ![Új projekt létrehozása](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     <span data-ttu-id="d1ab9-105">Adja meg az új projekt nevét a **Project name** mezőben, és kattintson a **Create project** (Projekt létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-105">Type in your new **Project name** and click **Create project**.</span></span>
3. <span data-ttu-id="d1ab9-106">Kattintson a **Utilities and More** (Segédprogramok és egyebek) gombra, majd a **Project Information** (Projektinformációk) elemre.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-106">Click the **Utilities and More** button and then click **Project Information**.</span></span> <span data-ttu-id="d1ab9-107">Jegyezze fel a projekt számát, amely a **Project Number** mezőben található.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-107">Make a note of the **Project Number**.</span></span> <span data-ttu-id="d1ab9-108">Ezt az értéket kell majd megadnia a `SenderId` változóként az ügyfélalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-108">You will need to set this value as the `SenderId` variable in the client app.</span></span>
   
    ![Segédprogramok és egyéb programok](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. <span data-ttu-id="d1ab9-110">A projekt irányítópultján, a **Mobile APIs** (Mobil API-k) elem alatt kattintson a **Google Cloud Messaging**, a következő oldalon pedig az **Enable API** (API engedélyezése) lehetőségre, majd fogadja el a szolgáltatási feltételeket.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-110">In the project dashboard, under **Mobile APIs**, click **Google Cloud Messaging**, then on the next page click **Enable API** and accept the terms of service.</span></span> 
   
    ![GCM engedélyezése](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![GCM engedélyezése](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. <span data-ttu-id="d1ab9-113">A projekt irányítópultján kattintson a **Credentials** (Hitelesítő adatok) > **Create Credential** (Hitelesítő adat létrehozása) > **API Key** (API-kulcs) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-113">In the project dashboard, Click **Credentials** > **Create Credential** > **API Key**.</span></span> 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. <span data-ttu-id="d1ab9-114">A **Create a new key** (Új kulcs létrehozása) ablakban kattintson a **Server key** (Kiszolgálókulcs) lehetőségre, adja meg a kulcs nevét, majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-114">In **Create a new key**, click **Server key**, type a name for your key, then click **Create**.</span></span>
7. <span data-ttu-id="d1ab9-115">Jegyezze fel az **API KEY** (API-KULCS) értékét.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-115">Make a note of the **API KEY** value.</span></span>
   
    <span data-ttu-id="d1ab9-116">Ezt az API-kulcs-értéket fogja használni, hogy engedélyezze az Azure-nak a GCM-mel való hitelesítést, és hogy leküldéses értesítéseket küldjön az alkalmazása nevében.</span><span class="sxs-lookup"><span data-stu-id="d1ab9-116">You will use this API key value to enable Azure to authenticate with GCM and send push notifications on behalf of your app.</span></span>

