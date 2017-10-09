Most, hogy egy eseményindítót, az idő toodo valami érdekes hello eseményindító által létrehozott hello adatokkal hozzáadását. Kövesse ezeket a lépéseket tooadd egy hello **SFTP - kivonat mappa** művelet. Ez a művelet egy fájl tartalmának hello ki definiált hello feltételek teljesülése esetén. 

tooconfigure hello Ez a művelet, szüksége lesz a következő információ tooprovide hello. Megfigyelheti, hogy a rendszer az új fájl hello hello tulajdonságok bemeneteként hello eseményindító által létrehozott egyszerű toouse adatokat:

| SFTP - kivonat mappa tulajdonság | Leírás |
| --- | --- |
| Forrás archív fájl elérési útja |Ez a hello elérési hello fájl kibontása közben. Válassza ki a hello jogkivonatok korábbi művelet található, vagy keresse meg a hello SFTP server toofind hello fájl elérési útját. |
| A célmappa útvonala |Ez a hello elérési utat, ahol hello kibontott fájlok kerülnek. Válassza ki a hello jogkivonatok található egy korábbi művelet, mintha a hello cél elérési útja vagy hello SFTP server navigálhat, és válasszon ki egy útvonalat. |
| Felülírja? |Azt jelzi, ha egy fájlt, mely hello azonos neve hello kibontott fájl található a hello célmappa elérési útja, ha hello létező fájl vagy nem írható felül. |

Lássunk neki hello művelet tooextract hello fájlok hozzáadása, ha korábban meghatározott hello feltétel értéke túl*igaz*. 

1. Válassza ki **művelet hozzáadása**.        
   ![SFTP művelet feltétel kép 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Jelölje be hello **SFTP - kivonat mappa** művelet      
   ![SFTP művelet feltétel kép 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Válassza ki **forrás archív fájl elérési útja**              
   ![SFTP művelet feltétel kép 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Jelölje be hello **fájl elérési útját** token. Ez azt jelzi, hogy hello fájl elérési útját hello található mint hello forrás archív fájl elérési útja eseményindító hello használt.           
   ![SFTP művelet feltétel kép 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Válassza ki **célmappa elérési útja**           
   ![SFTP művelet feltétel kép 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Jelölje be hello **fájl elérési útját** token. Ez azt jelzi, hogy hello fájl elérési útját hello hello kibontott fájlok hello cél elérési útjaként található eseményindító hello használt.   
7. Adja meg *\ExtractedFile* a hello **a célmappa útvonala** vezérlő. Ehhez a célként megadott mappa elérési útja vezérlő hello hello fájl elérésiút-token után.         
   ![SFTP művelet feltétel kép 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Adja meg *igaz* a hello **felülírása?* vezérlő tooindicate, amely a meglévő fájlok felülírt lehetnek, ha azonos nevet, amint hello hello kibontott fájlokat.      
   ![SFTP művelet feltétel kép 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Hello módosítások tooyour munkafolyamat mentése  

