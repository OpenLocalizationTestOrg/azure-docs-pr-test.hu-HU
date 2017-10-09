Most, hogy egy feltétel, az idő toodo valami érdekes hello eseményindító által létrehozott hello adatokkal hozzáadását. Kövesse az alábbi lépéseket tooadd hello **Salesforce - Get objektum** művelet. Ez a művelet egy új vezető létrehozásakor kap hello adatokat. Egy második művelet hello Salesforce - Get egy objektum művelet toosend hello adatait használó hello Office 365-összekötővel e-mailt is hozzáadhat.  

tooconfigure hello Ez a művelet, szüksége lesz a következő információ tooprovide hello. Megfigyelheti, hogy a rendszer az új fájl hello hello tulajdonságok bemeneteként hello eseményindító által létrehozott egyszerű toouse adatokat:

| Fájl tulajdonság létrehozása | Leírás |
| --- | --- |
| Objektumtípus |Ez az hello típusú érdekli Salesforce-objektum. Többek között az érdeklődési, a fiók, stb. |
| objektum azonosítója |Hello objektum azonosítót jelképez. |

1. Válassza ki **művelet hozzáadása** hivatkozásra. A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné. Ebben a példában a Salesforce-műveletek végezhetők, iránt.      
   ![Salesforce művelet kép 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Adja meg *salesforce* toosearch kapcsolódó toosalesforce műveletek számára.
3. Válassza ki **Salesforce - Get objektum** művelet tootake hello szerint.   **Megjegyzés:**: akkor lesz felszólító tooauthorize a logic app tooaccess a Salesforce fiókot, ha még nem meg korábban.    
   ![Salesforce művelet kép 2](./media/connectors-create-api-salesforce/action-2.png)    
4. Hello **Get objektum** megnyílik szabályozzák.  
5. Válassza ki *vezethet* hello objektum típusa.
6. Jelölje be hello **Objektumazonosító** vezérlő.
7. Válassza ki **...**  műveletekhez használható bemenetként jogkivonatok tooexpand hello listája.       
   ![Salesforce művelet kép 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Válassza ki **vezethet azonosító** megnyílik szabályozzák.   
   ![Salesforce művelet kép 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Figyelje meg, hogy hello vezethet azonosító token már hello Objektumazonosító vezérlő, jelezve, hogy hello Get Objektumesemény meg fogja keresni egy vezető egyenlő toohello átfutási azonosítója a logic app kiváltó átfutási azonosítóval.  
   ![Salesforce művelet kép 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Mentse a munkáját. Ennyi az egész, hello Get objektum művelet tooyour logikai alkalmazás hozzáadását. A Get-objektum vezérlő kell kinéznie:    
    ![Salesforce művelet kép 6](./media/connectors-create-api-salesforce/action-6.png)  

Most, hogy egy művelet tooget érdeklődők jelentek meg, érdemes lehet toodo valami érdekes az újonnan létrehozott hello átfutási. Vállalati érdemes lehet egy e-mail toonotify egy terjesztési listát, hogy létrejött-e egy új vezető toosend. Most használja hello Office 365-összekötő toosend egy e-mailt egy hello hello új átfutási objektum a Salesforce-ban lévő kapcsolódó információkkal.  

1. Válassza ki **művelet hozzáadása** írja be *e-mail* hello keresési vezérlőben. A szűrés hello műveletek toothose kapcsolódó toosending és küld e-maileket.  
2. Jelölje be hello **Office 365 Outlook - egy e-mailek küldése** listaelemet. Ha még nem hozott létre egy *kapcsolat* tooyour Office 365-fiókra, fog felszólító tooenter kell az Office 365 hitelesítő adatok toocreate informatikai most. Miután elkészült, hello **egy e-mailek küldése** megnyílik szabályozzák.        
   ![Salesforce művelet kép 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Adja meg, hogy szeretné-e toosend e-mail tooin hello hello e-mail címét **való** vezérlő.
4. A hello **tulajdonos** szabályozása, adja meg *létrehozott új vezethet* – adja meg, majd hello *vállalati* token. Megjelenik az hello *vállalati* hello a Salesforce-ban létrehozott új vezető a mezőt.  
5. A hello **törzs** vezérlő, akkor válassza ki bármelyik hello jogkivonatok hello új átfutási objektumot, és megadhat bármilyen szöveg hello üzenettörzsben toodisplay szeretné hello e-mailt. Íme egy példa:  
   ![Salesforce művelet kép 8](./media/connectors-create-api-salesforce/action-8.png)   
6. A munkafolyamat mentése.  

Ennyi az egész. A Logic Apps alkalmazást befejeződött.  

Most, tesztelheti a Logic Apps alkalmazást: Salesforce, hozzon létre egy új vezető létrehozott hello feltételt.  Ha ez az útmutató teljesen követi, készítsen egy vezető tartalmazó e-mail címmel rendelkező *amazon.com* azt. Néhány másodpercen belül a Logic Apps alkalmazást váltódik ki, és hello eredmények lehet, hogy keresse meg a hasonló toothis:  
![Salesforce művelet kép 9](./media/connectors-create-api-salesforce/action-9.png)  

