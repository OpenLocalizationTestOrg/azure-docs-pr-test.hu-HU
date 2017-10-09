
1. Keresse meg a toohello [Google Cloud Console](https://console.developers.google.com/project), jelentkezzen be Google-fiók hitelesítő adataival. 
2. Kattintson a **Create Project** (Projekt létrehozása) elemre, adjon meg egy nevet a projekthez, és kattintson a **Create** (Létrehozás) gombra. Ha szükséges, hello SMS-ellenőrzést végez, majd kattintson **létrehozása** újra.
   
    ![Új projekt létrehozása](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     Adja meg az új projekt nevét a **Project name** mezőben, és kattintson a **Create project** (Projekt létrehozása) gombra.
3. Kattintson a hello **segédprogramok és egyebek** gombra, majd **projektadatokat**. Jegyezze fel a hello **Projektszám**. Szüksége lesz tooset ezt az értéket hello `SenderId` hello ügyfélalkalmazás változóját.
   
    ![Segédprogramok és egyéb programok](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. Hello a projekt irányítópultján a **mobil API-k**, kattintson a **Google Cloud Messaging**, hello következő lapon kattintson a **engedélyezése API** és fogadnia hello szolgáltatást. 
   
    ![GCM engedélyezése](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![GCM engedélyezése](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. Hello projekt irányítópultján kattintson **hitelesítő adatok** > **hitelesítő adat létrehozása** > **API-kulcs**. 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. A **Create a new key** (Új kulcs létrehozása) ablakban kattintson a **Server key** (Kiszolgálókulcs) lehetőségre, adja meg a kulcs nevét, majd kattintson a **Create** (Létrehozás) gombra.
7. Jegyezze fel a hello **API-kulcs** érték.
   
    Az API-kulcs értéke tooenable Azure tooauthenticate GCM-mel használni fog, és küldhet leküldéses értesítéseket küldjön az alkalmazása nevében.

