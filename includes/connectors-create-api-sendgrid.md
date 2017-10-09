### <a name="prerequisites"></a>Előfeltételek
* A [SendGrid](https://www.SendGrid.com/) fiók 

A SendGrid-fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour SendGrid fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour SendGrid fiók:

1. toocreate egy kapcsolat tooSendGrid hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *SendGrid* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![1. lépés SendGrid](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. Ha bármely kapcsolatok tooSendGrid előtt még nem hozott létre, később is lesz felszólító tooprovide a Sendgridbeli hitelesítő adataival. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect való, és a SendGrid fiók adatok eléréséhez:  
   ![2. lépés SendGrid](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![3. lépés SendGrid](./media/connectors-create-api-sendgrid/sendgrid-3.png)   

