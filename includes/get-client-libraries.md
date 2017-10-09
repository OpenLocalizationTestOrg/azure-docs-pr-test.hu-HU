### <a name="install-via-composer"></a><span data-ttu-id="6daae-101">Keresztül szerkesztő telepítése</span><span class="sxs-lookup"><span data-stu-id="6daae-101">Install via Composer</span></span>
1. <span data-ttu-id="6daae-102">[Telepítse a Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="6daae-102">[Install Git][install-git].</span></span> <span data-ttu-id="6daae-103">Vegye figyelembe, hogy a Windows rendszeren is hozzá kell hello Git végrehajtható tooyour PATH környezeti változóhoz.</span><span class="sxs-lookup"><span data-stu-id="6daae-103">Note that on Windows, you must also add hello Git executable tooyour PATH environment variable.</span></span> 
2. <span data-ttu-id="6daae-104">Hozzon létre egy fájlt **composer.json** a hello a projekt gyökérkönyvtárában, és adja hozzá a következő kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="6daae-104">Create a file named **composer.json** in hello root of your project and add hello following code tooit:</span></span>
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. <span data-ttu-id="6daae-105">Töltse le  **[composer.phar] [ composer-phar]**  a projekt gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="6daae-105">Download **[composer.phar][composer-phar]** in your project root.</span></span>
4. <span data-ttu-id="6daae-106">Nyisson meg egy parancssort, és hajtsa végre a következő parancsot a projekt gyökérkönyvtárában hello</span><span class="sxs-lookup"><span data-stu-id="6daae-106">Open a command prompt and execute hello following command in your project root</span></span>
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
