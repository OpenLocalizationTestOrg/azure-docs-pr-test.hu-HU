Ez az útmutató megtudhatja, hogyan toouse hello **Salesforce - amikor létrejön egy objektum** tooinitiate logic app munkafolyamat indít, amikor egy új vezető létrehozza a Salesforce.

> [!NOTE]
> Rákérdezéses toosign fog kapni a Salesforce-fiókjába, ha nem hozott létre már egy *kapcsolat* tooSalesforce.  
> 
> 

1. Adja meg *salesforce* hello keresőmezőbe hello logic apps designer válassza hello **Salesforce - amikor létrejön egy objektum** eseményindító.  
   ![Salesforce eseményindító kép 1](./media/connectors-create-api-salesforce/trigger-1.png)   
2. Hello **egy objektumának létrejöttekor** vezérlő jelenik meg.  
   ![Salesforce eseményindító kép 2](./media/connectors-create-api-salesforce/trigger-2.png)   
3. SELECT hello **objektumtípus** válassza *vezethet* hello objektumok listája. Ebben a lépésben vannak jelzi, hogy hoz létre egy eseményindítót, amely értesíti a Logic Apps alkalmazást, amikor egy új vezető jön létre a Salesforce-ban.   
   ![Salesforce eseményindító kép 3](./media/connectors-create-api-salesforce/trigger-3.png)   
4. Ennyi az egész. Hello eseményindító létrehozott. Azonban meg kell toocreate legalább egy műveletet a rendezés toomake Ez egy érvényes logikai alkalmazást.    
   ![Salesforce eseményindító kép 4](./media/connectors-create-api-salesforce/trigger-4.png)   

Ezen a ponton a Logic Apps alkalmazást van beállítva, amely akkor kezdődik, hello futását más eseményindítók és műveletek hello munkafolyamat amikor új elem jön létre a Salesforce eseményindítót.  

