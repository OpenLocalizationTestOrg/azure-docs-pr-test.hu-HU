### <a name="prerequisites"></a>Előfeltételek
* A [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) fiók  

Az SMTP-fiók a logikai alkalmazás használata előtt engedélyeznie kell a hello logic app tooconnect tooyour SMTP-fiókja. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon.  

Az alábbiakban hello lépéseket tooauthorize a logic app tooconnect tooyour SMTP-fiók:  

1. toocreate egy kapcsolat tooSMTP hello logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *SMTP* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Bármely kapcsolatok tooSMTP előtt még nem hozott létre, ha az SMTP hitelesítő adatok felszólító tooprovide fogja lekérni. Ezeket a hitelesítő adatokat kell használt tooauthorize a logic app tooconnect számára, és az SMTP-fiókja adatainak eléréséhez:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  

