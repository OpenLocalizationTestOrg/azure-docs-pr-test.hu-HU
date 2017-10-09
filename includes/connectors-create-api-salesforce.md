### <a name="prerequisites"></a>Előfeltételek
* A [Salesforce](https://salesforce.com) fiók  

A Salesforce-fiókhoz a logikai alkalmazás használata előtt engedélyeznie kell a hello logic app tooconnect tooyour Salesforce-fiókhoz. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon.  

Az alábbiakban hello lépéseket tooauthorize a logic app tooconnect tooyour Salesforce-fiókhoz:  

1. toocreate egy kapcsolat tooSalesforce hello logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *Salesforce* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![Kép: a 1 Salesforce kapcsolat](./media/connectors-create-api-salesforce/salesforce-1.png)  
2. A kapcsolatok tooSalesforce előtt még nem hozott létre, ha később is lesz felszólító tooprovide a Salesforce hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a logic app tooconnect számára, és a Salesforce-fiókhoz adatok eléréséhez:  
   ![Kép: Salesforce kapcsolat 2](./media/connectors-create-api-salesforce/salesforce-2.png)  
3. Adja meg a Salesforce-felhasználó nevét és jelszavát tooauthorize a Logic Apps alkalmazást:  
   ![Kép: a 3 Salesforce kapcsolat](./media/connectors-create-api-salesforce/salesforce-3.png)  
4. Lehetővé teszik a számunkra tooconnect tooSalesforce:  
   ![Kép: a 4 Salesforce kapcsolat](./media/connectors-create-api-salesforce/salesforce-4.png)  
5. Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:  
   ![Kép: a 5 Salesforce kapcsolat](./media/connectors-create-api-salesforce/salesforce-5.png)  

