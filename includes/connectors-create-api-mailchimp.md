### <a name="prerequisites"></a>Előfeltételek
* A [MailChimp](https://www.MailChimp.com/) fiók 

A MailChimp-fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour MailChimp fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour MailChimp-fiókot:

1. toocreate egy kapcsolat tooMailChimp hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *MailChimp* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![1. lépés MailChimp](./media/connectors-create-api-mailchimp/mailchimp-1.png)
2. Bármely kapcsolatok tooMailChimp előtt még nem hozott létre, ha később is lesz felszólító tooprovide a MailChimp hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect való, és a MailChimp fiók adatok eléréséhez:  
   ![2. lépés MailChimp](./media/connectors-create-api-mailchimp/mailchimp-2.png)
3. Adja meg a MailChimp-felhasználó nevét és jelszavát tooauthorize a Logic Apps alkalmazást:  
   ![3. lépés MailChimp](./media/connectors-create-api-mailchimp/mailchimp-3.png)   
4. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![4. lépés MailChimp](./media/connectors-create-api-mailchimp/mailchimp-4.png)

