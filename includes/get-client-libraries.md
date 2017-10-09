### <a name="install-via-composer"></a>Keresztül szerkesztő telepítése
1. [Telepítse a Git][install-git]. Vegye figyelembe, hogy a Windows rendszeren is hozzá kell hello Git végrehajtható tooyour PATH környezeti változóhoz. 
2. Hozzon létre egy fájlt **composer.json** a hello a projekt gyökérkönyvtárában, és adja hozzá a következő kód tooit hello:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. Töltse le  **[composer.phar] [ composer-phar]**  a projekt gyökérkönyvtárában.
4. Nyisson meg egy parancssort, és hajtsa végre a következő parancsot a projekt gyökérkönyvtárában hello
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
