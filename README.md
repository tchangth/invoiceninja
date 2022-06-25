# invoiceninja
Invoiceninja and queue connection


#### Invoiceninja and queue connection

I followed the excellent installation guide from : [How to Install InvoiceNinja on Ubuntu 22.04 Server with Apache/Nginx (linuxbabe.com)](https://www.linuxbabe.com/ubuntu/install-invoiceninja-ubuntu-22-04-apache-nginx)

The author took pain to test it out but it seems the queue connection seems not enabled.

If you click on the **Queue not enabled**, it will go to : [Free Source Available Invoicing, Expenses & Time-Tracking | Invoice Ninja](https://invoiceninja.github.io/docs/self-host-installation/#file-permissions)

Double check on the file permissions etc and put in the cron job - but the problem persists:

<img width="576" alt="invoice_wrong" src="https://user-images.githubusercontent.com/11190418/175763940-56df25e1-89a6-49b1-a675-8e2b1e572708.png">

[Queue not enabled but I configured like the documentation · Issue #7506 · invoiceninja/invoiceninja (github.com)](https://github.com/invoiceninja/invoiceninja/issues/7506)

It seems someone has solved the problem, but the root cause it not obvious

I follow the instruction here: [Queues - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/8.x/queues#supervisor-configuration)



<img width="576" alt="invoice" src="https://user-images.githubusercontent.com/11190418/175763969-494d0826-fce7-4c52-81b5-22c2c33baa19.png">



Remember to modify the parameter queue_connection=database, you can see the above example the Queue has switched to database (the earlier example was sync)

But there is a possibility that **Clear Cache** does not clear the cache - and therefore even you did the apt install supervisor and .env queue_connection=database **AND** sync stay there:

```php
composer dump-autoload
php artisan optimize
php artisan clear-compiled
php artisan cache:clear
php artisan view:clear
php artisan route:cache
php artisan queue:restart
```

I took the quote from : [php - Clear Laravel Queue Cache without restarting - Stack Overflow](https://stackoverflow.com/questions/47435222/clear-laravel-queue-cache-without-restarting)

Sometimes there are errors about file permission such as services.php etc :

goto boostrap/cache directory and chown www-data:www-data services.php and chmod 755 services.php (ditto packages.php) - you should be good to go.

Invoiceninja is based on laravel framework - it is strict on file permissions, so when there is an upgrade through sudo command, it will be root:root - and invoiceninja will complain of the errors
