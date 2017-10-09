### <a name="prerequisites"></a>Előfeltételek
* Egy Wunderlist fiók  

A Wunderlist fiókját a logikai alkalmazás használata előtt engedélyeznie kell a hello Logic app tooconnect tooyour Wunderlist fiók. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon. 

Az alábbiakban hello lépéseket tooauthorize a Logic app tooconnect tooyour Wunderlist fiók:

1. toocreate egy kapcsolat tooWunderlist hello Logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *Wunderlist* hello Keresés mezőbe. Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:  
   ![](./media/connectors-create-api-wunderlist/wunderlist-0.png)
2. Ha bármely kapcsolatok tooWunderlist előtt még nem hozott létre, később is lesz felszólító tooprovide Wunderlist hitelesítő adatait. Ezeket a hitelesítő adatokat kell használt tooauthorize a Logic app tooconnect való, és hozzáférési Wunderlist fiókja adatokat:   
   ![](./media/connectors-create-api-wunderlist/wunderlist-1.png)  
3. Adja meg a hitelesítő adatait, majd válassza a hello gomb toosign  
   ![](./media/connectors-create-api-wunderlist/wunderlist-2.png)  
4. Meg kell majd értesítést milyen hello logikai alkalmazás lesz engedélyek toodo Wunderlist fiókjához. Ha elfogadja, jelölje be hello gomb tooindicate a szerződést. 
   ![](./media/connectors-create-api-wunderlist/wunderlist-4.png)  
5. Végül válassza ki a hello **engedélyezés** gomb  
   ![](./media/connectors-create-api-wunderlist/wunderlist-5.png)  

