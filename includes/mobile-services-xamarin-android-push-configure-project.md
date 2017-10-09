
1. <span data-ttu-id="d17f9-101">A megoldás nézet hello (vagy **Solution Explorer** a Visual Studio), kattintson a jobb gombbal hello **összetevők** mappát, kattintson **további összetevők beszerzése...** , keressen a hello **Google Cloud Messaging Client** összetevő, és adja hozzá toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="d17f9-101">In hello Solution view (or **Solution Explorer** in Visual Studio), right-click hello **Components** folder, click  **Get More Components...**, search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span>
2. <span data-ttu-id="d17f9-102">Nyissa meg a hello ToDoActivity.cs projektfájlt, és adja hozzá a következő hello utasítás toohello osztály használatával:</span><span class="sxs-lookup"><span data-stu-id="d17f9-102">Open hello ToDoActivity.cs project file and add hello following using statement toohello class:</span></span>
   
        using Gcm.Client;
3. <span data-ttu-id="d17f9-103">A hello **ToDoActivity** osztály, adja hozzá a következő új kódot hello:</span><span class="sxs-lookup"><span data-stu-id="d17f9-103">In hello **ToDoActivity** class, add hello following new code:</span></span> 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return hello current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return hello Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    <span data-ttu-id="d17f9-104">Ez lehetővé teszi tooaccess hello mobil ügyfél példányának regisztrációját a hello leküldéses kezelő szolgáltatási folyamathoz.</span><span class="sxs-lookup"><span data-stu-id="d17f9-104">This enables you tooaccess hello mobile client instance from hello push handler service process.</span></span>
4. <span data-ttu-id="d17f9-105">Adja hozzá a következő kód toohello hello **OnCreate** metódus után hello **MobileServiceClient** jön létre:</span><span class="sxs-lookup"><span data-stu-id="d17f9-105">Add hello following code toohello **OnCreate** method, after hello **MobileServiceClient** is created:</span></span>
   
       // Set hello current instance of TodoActivity.
       instance = this;
   
       // Make sure hello GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register hello app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

<span data-ttu-id="d17f9-106">A **ToDoActivity** most felkészül a leküldéses értesítések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d17f9-106">Your **ToDoActivity** is now prepared for adding push notifications.</span></span>

