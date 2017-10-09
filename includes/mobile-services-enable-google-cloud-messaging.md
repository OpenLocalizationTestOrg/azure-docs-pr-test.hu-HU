
1. <span data-ttu-id="50bcf-101">Keresse meg a toohello [Google Cloud Console](https://console.developers.google.com/project), jelentkezzen be Google-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="50bcf-101">Navigate toohello [Google Cloud Console](https://console.developers.google.com/project), sign in with your Google account credentials.</span></span> 
2. <span data-ttu-id="50bcf-102">Kattintson a **Create Project** (Projekt létrehozása) elemre, adjon meg egy nevet a projekthez, és kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="50bcf-102">Click **Create Project**, type a project name, then click **Create**.</span></span> <span data-ttu-id="50bcf-103">Ha szükséges, hello SMS-ellenőrzést végez, majd kattintson **létrehozása** újra.</span><span class="sxs-lookup"><span data-stu-id="50bcf-103">If requested, carry out hello SMS Verification, and click **Create** again.</span></span>
   
    ![Új projekt létrehozása](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     <span data-ttu-id="50bcf-105">Adja meg az új projekt nevét a **Project name** mezőben, és kattintson a **Create project** (Projekt létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="50bcf-105">Type in your new **Project name** and click **Create project**.</span></span>
3. <span data-ttu-id="50bcf-106">Kattintson a hello **segédprogramok és egyebek** gombra, majd **projektadatokat**.</span><span class="sxs-lookup"><span data-stu-id="50bcf-106">Click hello **Utilities and More** button and then click **Project Information**.</span></span> <span data-ttu-id="50bcf-107">Jegyezze fel a hello **Projektszám**.</span><span class="sxs-lookup"><span data-stu-id="50bcf-107">Make a note of hello **Project Number**.</span></span> <span data-ttu-id="50bcf-108">Szüksége lesz tooset ezt az értéket hello `SenderId` hello ügyfélalkalmazás változóját.</span><span class="sxs-lookup"><span data-stu-id="50bcf-108">You will need tooset this value as hello `SenderId` variable in hello client app.</span></span>
   
    ![Segédprogramok és egyéb programok](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. <span data-ttu-id="50bcf-110">Hello a projekt irányítópultján a **mobil API-k**, kattintson a **Google Cloud Messaging**, hello következő lapon kattintson a **engedélyezése API** és fogadnia hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="50bcf-110">In hello project dashboard, under **Mobile APIs**, click **Google Cloud Messaging**, then on hello next page click **Enable API** and accept hello terms of service.</span></span> 
   
    ![GCM engedélyezése](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![GCM engedélyezése](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. <span data-ttu-id="50bcf-113">Hello projekt irányítópultján kattintson **hitelesítő adatok** > **hitelesítő adat létrehozása** > **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="50bcf-113">In hello project dashboard, Click **Credentials** > **Create Credential** > **API Key**.</span></span> 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. <span data-ttu-id="50bcf-114">A **Create a new key** (Új kulcs létrehozása) ablakban kattintson a **Server key** (Kiszolgálókulcs) lehetőségre, adja meg a kulcs nevét, majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="50bcf-114">In **Create a new key**, click **Server key**, type a name for your key, then click **Create**.</span></span>
7. <span data-ttu-id="50bcf-115">Jegyezze fel a hello **API-kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="50bcf-115">Make a note of hello **API KEY** value.</span></span>
   
    <span data-ttu-id="50bcf-116">Az API-kulcs értéke tooenable Azure tooauthenticate GCM-mel használni fog, és küldhet leküldéses értesítéseket küldjön az alkalmazása nevében.</span><span class="sxs-lookup"><span data-stu-id="50bcf-116">You will use this API key value tooenable Azure tooauthenticate with GCM and send push notifications on behalf of your app.</span></span>

