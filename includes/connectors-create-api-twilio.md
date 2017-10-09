### <a name="prerequisites"></a>Előfeltételek
* A Twilio-fiókja
* Ellenőrzött Twilio-telefonszámot, amelyet a SMS fogadására
* Az SMS küldő ellenőrzött Twilio telefonszám rendelve

> [!NOTE]
> Ha a Twilio-próbafiókra használ, csak elküldheti SMS túl**ellenőrzése** telefonszámokat.  
> 
> 

A Twilio-fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour Twilio-fiókja. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour Twilio-fiókja:

1. toocreate egy kapcsolat tooTwilio hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *Twilio* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. A kapcsolatok tooTwilio előtt még nem hozott létre, ha a Twilio hitelesítő adatokat felszólító tooprovide fogja lekérni. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect számára, és a Twilio-fiókja adatainak eléréséhez:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Szüksége lesz a hello **Twilio fiókazonosító** és **Twilio hozzáférési jogkivonat** a Twilio hello irányítópultról, jelentkezzen be tooyour Twilio-fiókja most toograb két adatot:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio és a Logic apps használjon különböző neveket tooidentify két adatot. Itt látható, hogyan kell társítani őket toohello Logic apps párbeszédpanel:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Jelölje be hello **hozható létre kapcsolat** gombra:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

