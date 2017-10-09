### <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a>Engedélyezze a Mobile Engagement hozzáférés tooyour GCM API-kulcs
tooallow a Mobile Engagement toosend leküldéses értesítések az Ön nevében, meg kell azt tooyour API-kulcs hozzáférési toogrant. Ez történik, konfigurálásával és a kulcs megadása hello a Mobile Engagement portált.

1. A klasszikus Azure portálon, ellenőrizze a van a hello alkalmazás azt a projekt használ, és kattintson a hello **Engage** hello alsó gombra:
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. Kattintson a hello **beállítások** -> **natív leküldés** szakasz tooenter a GCM-kulcs:
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. Hello kattintson **szerkesztése** elé ikon **API-kulcs** a hello **GCM-beállítások** szakasz a lent látható módon:
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. Hello előugró ablakban illessze be a hello előtt beszerzett GCM kiszolgálói kulcsot, és kattintson a **Ok**.
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <a id="send"></a>Egy értesítési tooyour app küldése
Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses értesítési tooour alkalmazást.

1. Keresse meg a toohello **ELÉRNI** a Mobile Engagement portálra a lap.
2. Kattintson a **új hirdetmény** toocreate a leküldéses értesítési kampány.
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. A következő lépéseket hello kampány első mezőjét hello beállítása:
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    a. Nevezze el a kampányt.
   
    b. Jelölje be hello **kézbesítési típust** , *Rendszerértesítés -> egyszerű*: Ez az hello egyszerű Android leküldéses értesítési típus, amely egy címet és egy rövid szöveges sort.
   
    c. Válassza ki **kézbesítési időhöz** , *bármikor* tooallow hello app tooreceive értesítést, hogy hello alkalmazás elindult-e.
   
    d. A hello értesítési szöveg típusú hello **cím** amely szerepelni fog a hello leküldéses üzenetben félkövérrel.
   
    e. Ezután írja be az üzenetét a **Message** (Üzenet) mezőbe
4. Görgessen lefelé, és a hello **tartalom** szakaszban jelölje be **csak értesítés**.
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. Elkészült a beállítással hello lehető legegyszerűbb kampányt lehetséges. Görgessen lefelé, és kattintson a hello **létrehozása** toosave gombra a kampány.
6. Utolsó lépés: kattintson a **aktiválás** tooactivate a kampány toosend leküldéses értesítéseket.
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

