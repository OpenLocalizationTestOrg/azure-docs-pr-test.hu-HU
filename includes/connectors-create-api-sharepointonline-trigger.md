Ebben a példában I bemutatja, hogyan toouse hello **SharePoint online-hoz - egy új elem létrehozásakor** indul el egy logic app munkafolyamat tooinitiate, ha egy új elem jön létre a SharePoint Online listáját.

> [!NOTE]
> Rákérdezéses toosign fog kapni a SharePoint-fiókjába, ha még nem hozott egy *kapcsolat* tooSharePoint Online.  
> 
> 

1. Adja meg *sharepoint* hello keresőmezőbe hello logic apps designer válassza hello **SharePoint online-hoz - egy új elem létrehozásakor** eseményindító.  
   ![SharePoint online eseményindító kép](./media/connectors-create-api-sharepointonline/trigger-1.png)  
2. Hello **új elem létrehozásakor** vezérlő jelenik meg.  
   ![SharePoint online eseményindító kép 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
3. Válassza ki a **webhely URL-címe**. Ez a hello SharePoint online-webhely új elemek tootrigger a munkafolyamat kívánt toomonitor.  
   ![SharePoint online eseményindító kép 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
4. Válassza ki a **neve**. A rendszer hello a hello toomonitor szánt új cikkeket, amelyek kiváltják a munkafolyamatot SharePoint Online-webhelyhez.  
   ![SharePoint online eseményindító kép 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

Ezen a ponton a Logic Apps alkalmazást az eseményindító, megkezdődik a hello futását más eseményindítók és műveletek hello munkafolyamat konfigurációja. Ez akkor kerül sor egy új elem létrehozásakor kijelölt SharePoint Online listában.  

