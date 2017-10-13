### <a name="prerequisites"></a><span data-ttu-id="4766f-101">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4766f-101">Prerequisites</span></span>
* <span data-ttu-id="4766f-102">A [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) fiók</span><span class="sxs-lookup"><span data-stu-id="4766f-102">A [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) account</span></span>  

<span data-ttu-id="4766f-103">Az SMTP-fiók a logikai alkalmazás használata előtt engedélyeznie kell a hello logic app tooconnect tooyour SMTP-fiókja. Szerencsére ehhez egyszerűen a a logikai alkalmazásban a hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="4766f-103">Before you can use your SMTP account in a logic app, you must authorize hello logic app tooconnect tooyour SMTP account.Fortunately, you can do this easily from within your logic app on hello Azure Portal.</span></span>  

<span data-ttu-id="4766f-104">Az alábbiakban hello lépéseket tooauthorize a logic app tooconnect tooyour SMTP-fiók:</span><span class="sxs-lookup"><span data-stu-id="4766f-104">Here are hello steps tooauthorize your logic app tooconnect tooyour SMTP account:</span></span>  

1. <span data-ttu-id="4766f-105">toocreate egy kapcsolat tooSMTP hello logic app Designer kiválasztása **megjelenítése Microsoft felügyelt API-k** hello a legördülő listából, majd adja meg *SMTP* hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="4766f-105">toocreate a connection tooSMTP, in hello logic app designer, select **Show Microsoft managed APIs** in hello drop down list then enter *SMTP* in hello search box.</span></span> <span data-ttu-id="4766f-106">Hello eseményindító vagy lesz, például a toouse művelet kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="4766f-106">Select hello trigger or action you'll like toouse:</span></span>  
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. <span data-ttu-id="4766f-107">Bármely kapcsolatok tooSMTP előtt még nem hozott létre, ha az SMTP hitelesítő adatok felszólító tooprovide fogja lekérni.</span><span class="sxs-lookup"><span data-stu-id="4766f-107">If you haven't created any connections tooSMTP before, you'll get prompted tooprovide your SMTP credentials.</span></span> <span data-ttu-id="4766f-108">Ezeket a hitelesítő adatokat kell használt tooauthorize a logic app tooconnect számára, és az SMTP-fiókja adatainak eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="4766f-108">These credentials will be used tooauthorize your logic app tooconnect to, and access your SMTP account's data:</span></span>  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. <span data-ttu-id="4766f-109">Figyelje meg hello kapcsolat létrejött, és most a Logic Apps alkalmazást az egyéb hello szabad tooproceed szükséges lépések:</span><span class="sxs-lookup"><span data-stu-id="4766f-109">Notice hello connection has been created and you are now free tooproceed with hello other steps in your logic app:</span></span>  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  
