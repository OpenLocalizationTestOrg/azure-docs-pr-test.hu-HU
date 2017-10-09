
1. A megoldás nézet hello (vagy **Solution Explorer** a Visual Studio), kattintson a jobb gombbal hello **összetevők** mappát, kattintson **további összetevők beszerzése...** , keressen a hello **Google Cloud Messaging Client** összetevő, és adja hozzá toohello projekt.
2. Nyissa meg a hello ToDoActivity.cs projektfájlt, és adja hozzá a következő hello utasítás toohello osztály használatával:
   
        using Gcm.Client;
3. A hello **ToDoActivity** osztály, adja hozzá a következő új kódot hello: 
   
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
   
    Ez lehetővé teszi tooaccess hello mobil ügyfél példányának regisztrációját a hello leküldéses kezelő szolgáltatási folyamathoz.
4. Adja hozzá a következő kód toohello hello **OnCreate** metódus után hello **MobileServiceClient** jön létre:
   
       // Set hello current instance of TodoActivity.
       instance = this;
   
       // Make sure hello GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register hello app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

A **ToDoActivity** most felkészül a leküldéses értesítések hozzáadása.

