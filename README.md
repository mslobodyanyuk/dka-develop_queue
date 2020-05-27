LARAVEL - QUEUE
======= 

[Laravel queues learn to use and what it is | Laravel Queues | Laravel Jobs 	(6:30)]( https://www.youtube.com/watch?v=DFCH1n3oOnA&list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&index=2&t=9s )
		
- laravel jobs
- laravel очереди
- laravel work
		
An introduction to learning queues in laravel and setting up job storage systems.

Сommands:

	php artisan queue:table
	php artisan migrate
	php artisan config:cache

	sudo apt install redis-server
	composer require predis/predis
	netstat -an | grep 6379
	
	php artisan queue:work	

Queues, in fact, are deferred tasks with which you can defer the execution of tasks to a later time. For example, sending e-mail messages ... (- examples with package delivery - status tracking by order id. (That is, DO NOT wait ...)).
Queues can be used for any time-consuming tasks.

[(2:14)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=134 )
For example, you need to parse a page. - You send the task to the queue and can continue to work on the site. Answer inquiries, generate reports. Once the server has processed the task -
you send a notification through the same events and Laravel echo. - That the task is completed - you can check.

[(2:32)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=152 ) 
Settings for working with queues:
You can use various storage systems to operate the queues. We will use (consider) storage in the database and using the redis server. After we configure the connection, the remaining lessons will be the same for ALL types of connections.
Define - database | redis. 


* ***Actions:***

    `//go to the project folder, like: cd /var/www/LARAVEL/dka-develop_queue.loc`
	
	`composer create-project --prefer-dist laravel/laravel`
																																																											
[(3:30)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=210 )
This command will create the necessary migration files.

	php artisan queue:table

- add to `AppServiceProvider.php`

```php
public function boot()
{
	Schema::defaultStringLength(191);
}
```		

[(3:50)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=229 )
Will create a tables from the database migration files:

	php artisan migrate

`QUEUE_DRIVER=database`

Copy the connection type `database` into `.env` from the configuration file `config /queue`, `QUEUE_DRIVER = database`

`.env`

We execute the command to reset the cache of settings and other configuration files.

	php artisan config:cache

[(4:40)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=280 ) 
Now we’ll look at another way to store tasks, and show how to configure task storage in a redis server. You do NOT need to perform both types of data storage, only the one that you think is more convenient for you.
This manual is suitable for Ubuntu operating systems.

* ***Redis:***

Install redis server:

	sudo apt install redis-server

[Error: How to Fix ‘E: Could not get lock /var/lib/dpkg/lock’ Error in Ubuntu Linux](https://itsfoss.com/could-not-get-lock-error/)

- check the installation, open the redis server console:

	`redis-cli`

- execute the command:

	`ping`

- if "PONG" answered, then the server is working. :)


***Ubuntu Laravel project setup***

1. access to files in folder:

	`sudo chmod -R 777 /var/www/LARAVEL/dka-develop_queue.loc`

2. creating files of the new virtual host, creating the file of the first virtual host.
Let's start by copying the file for the first domain:
	
	`sudo cp /etc/apache2/sites-available/test.loc.conf /etc/apache2/sites-available//var/www/LARAVEL/dka-develop_queue.loc.conf`
	
3. Open a new file in the editor with root privileges:
	
	`sudo nano /etc/apache2/sites-available/dka-develop_queue.loc.conf`
		
4. configure on /etc/apache2/sites-available/dka-develop_queue.loc

```
Ctrl + O
Enter 
Ctrl + X
```		
5. inclusion of new virtual hosts:
		
	`sudo a2ensite dka-develop_queue.loc.conf`

// (Next, deactivate the default site 000-default.conf :) - Most likely only 1 time, the first time you use it.
			sudo a2dissite 000-default.conf					
		
6. After completion, you need to restart Apache for the changes to take effect: In other documentation sources you can see an example of using the service command:

		sudo service apache2 restart
		
This command works the same way, but you may not get the output as when using other systems, because now this command is a wrapper around systemctl.
		
		sudo systemctl restart apache2

7. 	`sudo nano /etc/hosts`
	
8. Testing the results, like :)
				
		http://dka-develop_queue.loc		
				
[Error: cannot allocate memory - proc_open...]( https://nicesnippets.com/blog/proc-open-fork-failed-cannot-allocate-memory-laravel-ubuntu )

	//composer create-project laravel/laravel dka-develop_queue.loc --prefer-dist
				
We install with the help of composer a package that will provide Laravel with a redis server. In the directory with the Laravel project, execute the following command.

	composer require predis/predis	

[(5:50)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=350 ) 
Make sure that the settings for connecting to the redis server are registered in the .env file.

`.env` :

```
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
```
...

Change the driver for the queues to redis:

`QUEUE_DRIVER=redis` 

In Laravel 7.7.1:

`QUEUE_CONNECTION=redis`

In subsequent lessons, the connection type `database` and `redis` will be changed as necessary.
It will be necessary to return again:

`QUEUE_DRIVER | QUEUE_CONNECTION = database`	

OR

`QUEUE_DRIVER | QUEUE_CONNECTION = redis`

We execute the command to reset the cache of settings and other configuration files.

	php artisan config:cache

[(6:00)]( https://youtu.be/DFCH1n3oOnA?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=360 ) - let's see if the redis server listens on this port:

	netstat -an | grep 6379


#### useful links:

[Error: How to Fix ‘E: Could not get lock /var/lib/dpkg/lock’ Error in Ubuntu Linux]( https://itsfoss.com/could-not-get-lock-error/ )

[Error: cannot allocate memory - proc_open...]( https://www.nicesnippets.com/blog/proc-open-fork-failed-cannot-allocate-memory-laravel-ubuntu )

---	


[#1 Laravel queues create a task and send it to the queue | Laravel Queues | Laravel Jobs (5:16)]( https://www.youtube.com/watch?v=ZG1Gs6_7p28&list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&index=2 )
	
We go further and learn to create tasks and send them to the queue for execution.

Commands:

	php artisan make:job SendMessage

	php artisan serve
	php artisan queue:work	

[(00:30)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=30 )
We create our first task, use the command for this:
	
	php artisan make:job SendMessage

SendMessage - the name of the class for our job. ALL classes with tasks are located in the `app/Jobs` directory - if this folder is NOT present - it will be created after the first execution of the task creation command.
This is our first class that implements the queue interface. We can pass data for processing to the class constructor. If we are talking about user registration, then the data may be an e-mail to which you must send
a message about successful registration and the username to which the message will be addressed. The handle () method will just perform the task that we will write to it. For example, here you can write a user message sending code
using the data that came in the constructor of the class.

[(1:25)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=85 )
Let's complete our first task in line. - Create a class member with the identifier protected $ message; - assign a NAME - it should convey meaning as the name of a variable. Next, the constructor takes one parameter, the parameter is NOT
must be one. - Remember about the scope, in fact these are different variables. - Now in order to assign a variable to the class member that came to the constructor, use the link to the current object of the class $ this and the NAME of the class member WITHOUT the "$" sign (- property).
`$this->message->data`. To demonstrate the job, we use the Laravel helper function, `info()`. And we put the data that we transfer to her in the Laravel log file. -> `info ($this-> message);` - After we wrote our task class, we can send it
queued for execution using the `dispatch()` helper method;

***! DO NOT write logic in routes !***

- The arguments passed to the sending method will be passed to the job designer.
Actions:
- edit the code ...

[(2:50)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=170 )
Now we launch the development server:

1.
	`php artisan serve`

And run the queue handler:

2. In another terminal:

	`php artisan queue:work`
	
- This team may be familiar to you from our course on creating real-time applications.

3. Open this route in the browser:

	`127.0.0.1:8000`

[Error: LogicException : Please make sure the PHP Redis extension is installed and enabled.]( https://github.com/TypiCMS/Base/issues/158 )
 
	sudo nano /etc/php/7.2/cli/php.ini
		
//extension=redis.so
1. Locate your php.ini file for your server.
2. Add extension=redis.so to the extension section.
3. Save your changes.
4. Restart the server and your application

- check in `routes/web` phpinfo(); where is php.ini

- add library	

	`sudo apt-get install php-redis`	
	
[Error: Unable to install Redis.]( https://qna.habr.com/q/556049 )

- redis.so appeared in `usr/lib/php/20170718`

Restart:

1.
	
    `php artisan serve`

2. In another terminal:

	`php artisan queue:work`

3. In the browser:

	`127.0.0.1:8000`

4. We look at the message in `storage/logs/laravel.log`

[(3:21)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=201 )
Open the console and see that our task has been started and completed:

`...Processing...` 

`...Processed...`
 
Stop the queue handler `Ctrl + C` and see in what form the tasks are stored in the database.

- Created by dka_develop_queue utf8mb4_general_ci

	`php artisan queue:table`

	`php artisan migrate`

[Error: SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhost' (using password: NO). DB_HOST set to localhost](https://stackoverflow.com/questions/58233866/sqlstatehy000-1045-access-denied-for-user-rootlocalhost-using-password)
		
	php artisan config:cache 
	
[(3:24)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=204 ) 
As you can see, our task is serialized into a row with additional data and added to the table field. - When it comes time for execution, it is removed and runs ALL of the instructions that are indicated.

[(3:45)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=225 ) 
Start the queue back:

1.
	`php artisan serve`

2. In another terminal:

	`cd /var/www/LARAVEL/dka-develop_queue.loc`

	`php artisan queue:work`

3. In the browser:
	
	`127.0.0.1:8000`

- AND ALL that requires execution has been processed (- The entry in the table will disappear).

`.env`

`! QUEUE_DRIVER | QUEUE_CONNECTION = database !`

[(3:55)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=235 )

Let's add a delay to the job. Add 10 minutes to the current time:

- Add to `routes/web`:
 
```php 
App\Jobs\SendMessage::dispatch("TEST MESSAGE")->delay(now()->addMinutes(10));
```
	
We launch in the browser:

	127.0.0.1:8000 (- Refresh)

[(4:14)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=254 )

![screenshot of sample]( https://github.com/mslobodyanyuk/dka-develop_queue/blob/master/public/images/1(4.14).png )
                        
We look in the database `available_at` - this field is responsible for the moment when this task can already be completed. While the current time has NOT reached this mark - the task is skipped. If even the execution time has passed, let's say an hour has passed instead of 10 minutes.
and you turned off the queue handler, and then turned it on. - It will EVERYTHING be fulfilled, since from now on it is available for fulfillment.

+ time label conversion site `unixtimestamps.com`

[(4:35)]( https://youtu.be/ZG1Gs6_7p28?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=275 )
 _Example_ - it is now 8 o’clock, 8:00 - You have postponed the task for 10 minutes. + 0:10 - accordingly at 8:10 it can already be executed. - You turned off the handler and turned it on at 8:30. - The task EVERYTHING will be fulfilled equally.
8:30 - This is just the time when the task can be processed.
	
#### useful links:

[Error: LogicException : Please make sure the PHP Redis extension is installed and enabled.]( https://github.com/TypiCMS/Base/issues/158 )
	
[Error: Unable to install Redis.]( https://qna.habr.com/q/556049 )
	
[Error: SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhost' (using password: NO). DB_HOST set to localhost]( https://stackoverflow.com/questions/58233866/sqlstatehy000-1045-access-denied-for-user-rootlocalhost-using-password )

---

[#2 Laravel queues: cascade of tasks and the number of attempts to complete | Laravel Queues | Laravel Jobs (5:25)]( https://www.youtube.com/watch?v=2KGXg03ryPI&list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&index=3 )

We go further and learn to create tasks and send them to the queue for execution.

Commands:

	php artisan make:job Name

	php artisan serve
	php artisan queue:work
	php artisan queue:work --tries=3	

In this video we will create a cascade of tasks. In fact, one big task is divided into stages. - Let's try to imagine that your service allows you to upload images. - The user sent you a picture, you upload it,
overlay and publish. - ALL of these tasks can be divided into different tasks. It is of course thick enough for these purposes. - When you will create some kind of complex process - think about whether you can break it down into stages.
For example, YouTube - IF summarized, in fact there are much more operations performed on the video:
 - uploads your video,
 - then processes.
 - That's just what it can be divided into 3 stages. - IF the download was successful - The task is passed to the video handler. - IF EVERYTHING is normal - it is ready for publication. (Upload | Processing | Done)
- So in the Work on the Laravel queues, the task is carried out in a chain. IF some task is NOT executed in the chain, for example, when trying to connect to the server to send a message, further tasks in the chain are NOT executed.

[(1:22)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=82 ) 
Let’s create for our Assignment which in the last video we will create 2 more Assignments.

	//cd /var/www/LARAVEL/dka-develop_queue.loc

	php artisan make:job Name

	php artisan make:job PrepareJob
		
- Of course, you should NOT give such names for the classes of the assignment - this is too much level of abstraction. And 2nd Task:

	`php artisan make:job PublishJob`
	
- We do this by analogy with the 1st task, they will take one parameter in the constructor AND output a message to the log file (- `storage/logs/laravel.log` )

[(2:10)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=130 ) - Goes to the route - Delete the execution delay And before calling the Task we add a method that takes an array of events as a parameter.

***! DO NOT write logic in routes !***

IF the first Task is NOT completed, the subsequent such will NOT be completed.

[(2:48)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=168 ) We start the development server and the queue handler, open the root route:

1.
	
	`php artisan serve`

2. In another terminal:
	
	`//cd /var/www/LARAVEL/dka-develop_queue.loc`
	
	`php artisan queue:work`

3. In the browser:

	`127.0.0.1:8000`

- We look at the handler - we see in what sequence the tasks were completed. - The first task was completed that calls 2 others, according to the indices in the array ([0,1]):

![screenshot of sample]( https://github.com/mslobodyanyuk/blob/master/dka-develop_queue.loc/public/images/2_2(3.10).png )

`routes/web:`
```php	
Route::get('/', function () {
	//return view('welcome');
	//App\Jobs\SendMessage::dispatch("TEST MESSAGE")->delay(now()->addMinutes(10));
	App\Jobs\SendMessage::withChain([
		new App\Jobs\PrepareJob('Prepare...'),
		new App\Jobs\PublishJob('Publish!')
	])->dispatch("Start Job");
   // phpinfo();
});
```

[(3:25)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=205 ) 

Let's stop completing the quest in quest 2 in the chain.
  - To emulate the error, we will throw an exception, `Jobs/PrepareJob.php` - we look at the code in the `handle()` method:  

```php 
throw new \Exception("Our Error", 101);
info($this->message);
```		
	Ctrl + C
		
[(3:40)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=220 )

![screenshot of sample]( https://github.com/mslobodyanyuk/blob/master/dka-develop_queue.loc/public/images/2_3(3.40).png )

 - Again we start the task in the queue:

	`php artisan queue:work`

(- execution will run), stop the handler `Ctrl + C`. - You see, the first task is completed, the second one is trying to execute, and the 3rd one is NOT executed, because the 2nd one generates an error.

[(4:00)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=240 )
Let's open the database. attempts - this parameter indicates the number of attempts to complete tasks. - The number of execution attempts can be limited through an additional parameter when starting the handler OR directly by specifying
job code.

[(4:19)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=259 ) Let's indicate in the parameter, this parameter is responsible for the attempts. The number of attempts is indicated here.

	php artisan queue:work --tries=3
	
- Let's run it. - You see, he reports an ERROR and then does NOT try to execute it anymore. - Open the database and find that this Task is NOT. - He will try to execute it. And after the 3rd time, he just threw it out.


 - [(4:35)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=275 ) Now let's indicate the number of attempts in the code. - We go into the task and write a public member of the class, the name is the same as when called in the terminal.
`Jobs/PrepareJob.php`:
	
```php	
public $tries = 3;
```
	
- Now run WITHOUT the indication of attempts:
	
	`php artisan queue:work`
	
 - [(4:53)]( https://youtu.be/2KGXg03ryPI?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=293 ) Refreshing the page - see, exactly 3 attempts. Last time, it flew out right away, as there the counter of attempts had long exceeded 3.
---


[#3 Laravel Queues: Unsuccessful Jobs | Laravel Queues | Laravel Jobs (5:19)]( https://www.youtube.com/watch?v=bucNubYHQRY&list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&index=4 )

Commands:

	php artisan queue:failed-table
	php artisan migrate

View all missions that failed.
	
	php artisan queue:failed

Try to complete the task again with a specific id in the database.
	
	php artisan queue:retry 2

Try to complete all failed tasks.
	
	php artisan queue:retry all

Remove a failed task from the table.
	
	php artisan queue:forget 5

Or completely clear the task table.
	
	php artisan queue:flush

In this video, we will consider tasks that exceeded the number of attempts AND Failed to complete. Sometimes your tasks end in ERROR. Be it an inaccessible server to send a message OR a fallen off network interface.
- In the last lesson, we indicated how many attempts a task can make ( `php artisan queue: work --tries = 3` ) before leaving the queue. “BUT this is somehow WRONG.” - We must, for example, execute it later OR process it.
- DO NOT worry after this task has exceeded the number of attempts - it will be transferred to the database table ( `failed_jobs` ).

[(0:47)]( https://youtu.be/bucNubYHQRY?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=47 ) 
BUT for this you need to configure and create a table.
_- Already had a Jobs table, Laravel 7.7_

	//php artisan queue:failed-table 
	
- issued an ERROR:

_A CreateFailedJobsTable class already exists._
		
	//php artisan migrate
	
[(1:06)]( https://youtu.be/bucNubYHQRY?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=66 )
 - Open the database - we will see a table in which there will be tasks that could NOT be successfully completed after a certain number of attempts.

[(1:18)]( https://youtu.be/bucNubYHQRY?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=78 ) Let's run the task that we created in the last video again. We remind you that the task will take 3 attempts.

	php artisan queue:work

In the browser, refresh the page:
	
	127.0.0.1:8000

- We look at the Jobs table - it is NOT in the "queue" AND it is displayed in the table of tasks that failed to execute ( `failed_jobs` ) - This table is slightly different. The connection is indicated here. - Yes, for each task
various compounds may be used. - One, for example, can be in the database, and the other is a redis server ( `connection` is a column ), then our task ( `payload` ), message with ERROR ( `exception` ), date and time ( `failed_at` )
 - For unsuccessful ERRORS to be placed in a separate table, they should indicate the number of attempts to execute ( `public $ tries = 3;` OR, the handler should start with this parameter `php artisan queue: work --tries = 3` ).

![screenshot of sample]( https://github.com/mslobodyanyuk/blob/master/dka-develop_queue.loc/public/images/3(1.57).png ) 


[(2:18)]( https://youtu.be/bucNubYHQRY?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=138 )
You can use the method in this task( `Jobs\PrepareJob` ) to handle the Failed job completion. Connect the namespace use Exception; Remove " \ " from `handle()` before the connected Exception class;
We write a comment for the method, its name must be exactly the same, and as a parameter it accepts our exception. Here you can send a message to the client that the task has failed and the text ERRORS.
We will output the message to the log file. We use a predefined constant that outputs the NAME of our class + "." to combine with a string. Reduce the number of attempts to one ( `public $ tries = 1;` ).
	
```php	
	use Exception;

	throw new Exception("Our Error", 101);

		/**
		 * The job failed to process
		 * 
		 * @param Exception $exception
		 */
		public function failed(Exception $exception){
			info(__CLASS__ . ": ошибка выполнения");
		}
		
		public $tries = 1;		
```	
[(3:05)]( https://youtu.be/bucNubYHQRY?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=185 )
Run the handler:  
	
	php artisan queue:work

In the browser, refresh the page:
	
	127.0.0.1:8000

Open the log file. You see, our action is displayed with the class name and text.
```
[2020-04-29 18:19:16] local.INFO: Start Job  
[2020-04-29 18:19:18] local.INFO: App\Jobs\PrepareJob: ошибка выполнения 
```
Then an ERROR is output to the log file - this already works the exception handler in Laravel.
! By the way, IF YOU WILL NOT limit the number of attempts, and the task will try to be executed - this will cause the file with the log to increase and your disk space
may end unexpectedly.
! Be careful. Using artisan you can see ALL the tasks that failed:

View all missions that failed.
	
	php artisan queue:failed

Try to complete the task again with a specific id in the database.
	
	php artisan queue:retry 4

We start the queue again.
	
	php artisan queue:work

Remove failed task from tables.
	
	php artisan queue:forget 5

[(4:20)]( https://youtu.be/bucNubYHQRY?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=260 )
Attempt to complete all failed tasks.

	php artisan queue:retry all

Or completely clear the task table.
	
	php artisan queue:flush

Again. View all missions that failed.
	
	php artisan queue:failed

- There are no tasks with errors.

---


[#4 Laravel queues: autostart after server reboot, process crashes and on vps | Supervisor (6:54)]( https://www.youtube.com/watch?v=eqKEbJzkpGc&list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&index=5 )
	
If you don’t know how to start the queue on the battle server as a process or if the server reboots, we will offer you instructions for solving this problem.

Сommands:

With official documentation: 
<https://laravel.com/docs/5.6/queues>

Use commands with official documentation in case of changes it is updated.

In this video, we will learn how to start the queue handler after rebooting the server OR process crashes. This is especially useful on vps servers and on a combat server. just like a non-working app is one frustration!
This instruction is also suitable for launching the Realtime application ( `Laravel and Vue (Realtime App) `) that we created in this playlist. - Supervisor will help us with this - it is a `CLIENT-SERVER` system with the help of which
user can control connected processes on UNIX systems. The tool creates processes in the form of subprocesses on its behalf, so it has full control over them. This instruction is suitable for ALL UNIX-like
`Linux systems | Mac | FreeBSD` - the only difference is the Installation Manager and the Storage location for config files. For demonstration we use `UBUNTU`.

[(1:09)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=69 )
 Use the command to install Supervisor:
	
	sudo apt install supervisor
	
	- /etc/supervisor/supervisord.conf

We go under the superuser, otherwise we will NOT have access to edit directories:
	
	sudo -i

[(1:41)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=101 )
The configuration files are located on this path:
	
	/etc/supervisor/conf.d/*.conf
	
- Your own files will be located here. The official documentation says that the file must be created in this directory:
	
	`/etc/supervisor/conf.d/`
	
The NAME of the file must meet the requirements of NAME.conf.

[(2:12)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=132 )
Copy the example from the official documentation:
```
	[program:laravel-worker]
	#process_name=%(program_name)s_%(process_num)02d
	#command=php /home/forge/app.com/artisan queue:work --sleep=3 --tries=3
	command=php /var/www/LARAVEL/dka-develop_queue.loc/artisan queue:work --sleep=3 --tries=3
	autostart=true
	autorestart=true
	user=USER_NAME
	numprocs=1
	redirect_stderr=true
	stdout_logfile=/var/www/LARAVEL/dka-develop_queue.loc/worker.log
```
	
	sudo nano /etc/supervisor/conf.d/laravel-worker.conf

[(2:50)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=170 ) - Set the number of instances to 1, `numprocs = 1`, command = `php /var/www/LARAVEL/dka-develop_queue.loc/artisan queue: work --sleep = 3 --tries = 3` - specify the full path to artisan, `--sleep` parameter sets
time for which the handler will fall asleep if there are NO new tasks. `autostart = true` - running worker together with starting Supervisor in case your server reboots; your handler will be automatically launched.
`autorestart = true` - restart worker if it crashes for some reason. `user` - start the process under a specific user, specify it. `redirect_stderr = true` - this parameter will redirect errors to this
`log file` (- listed below, `stdout_logfile = /var/www/LARAVEL/dka-develop_queue.loc/worker.log` ). Let's put it in the project folder on Laravel.
	
	//cd /var/www/LARAVEL/dka-develop_queue.loc
	
[(4:25)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=265 ) 
Now we need to add our configuration file to the monitor. We execute the commands in order of priority.

	sudo supervisorctl reread
	
- [(4:38)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=278 ) The directory has been re-read and our configuration is available.

	`sudo supervisorctl update`

- Updating - the process has been added.

	`sudo supervisorctl start laravel-worker`

- Run our configuration - appeared worker.log, in the root.
[(5:00)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=300 )
Check running processes:

	`ps ax | grep artisan` 
	
![screenshot of sample]( https://github.com/mslobodyanyuk/blob/master/dka-develop_queue.loc/public/images/4(5.10).png )	

- Great, our queue handler is launched in the directory with our project.

[(5:15)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=315 )
 - Let's check how it works. We launch the development server for the project that we created in previous releases. - Separately, the handler does NOT start as it is already running using Supervisor (s)
In the new terminal:

	`//cd /var/www/LARAVEL/dka-develop_queue.loc`
	
	`php artisan serve`

[(5:30)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=330 )
To start the task, you must open the main page:

	 127.0.0.1:8000

- Let's open our project in an editor (- for example phpStorm), a file with routes - routes / web.php. - When you start this route, a cascade of tasks starts. And in this assignment (- PrepareJob)
an exception will be thrown which will try to be executed 1 time - the code takes precedence (- not 3 times), then it will be moved to the table with unsuccessful tasks.

[(6:10)]( https://youtu.be/eqKEbJzkpGc?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=370 )
How to stop a process that is running in Supervisor. We use this command:
	
	sudo supervisorctl stop laravel-worker
	
- stopped
Check:

	`ps ax | grep artisan` 
	
- there is NO process, ("artisan serve" is our development server)
Run again:
	
	`sudo supervisorctl start laravel-worker`

- started
We check:

	`ps ax | grep artisan` 

- the process is visible.

#### useful links:

With official documentation:
 
<https://laravel.com/docs/5.6/queues>

---


[#5 Laravel Horizon what it is and how to configure and use it | Laravel Queues | Laravel Jobs (5:10)]( https://www.youtube.com/watch?v=HhhzBtoVXR0&list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&index=6 )
	
Commands:
	
	composer require laravel/horizon
	php artisan vendor:publish --provider="Laravel\Horizon\HorizonServiceProvider"
	php artisan horizon

In this video we will get acquainted with the Horizon package, what it is for and what it has to offer. Horizon - allows you to track the key indicators of your queuing system.
Such as job bandwidth / lead time AND job failure. Essentially, it replaces the queue handler and provides a dashboard for tracking status of tasks and their management.

	//cd /var/www/LARAVEL/dka-develop_queue.loc
	
![screenshot of sample]( https://github.com/mslobodyanyuk/blob/master/dka-develop_queue.loc/public/images/5_1(0.27).png )	

[(0:41)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=41 )
The main thing to remember is that this package will work ONLY if you use a redis server as a driver and storage.

[(0:48)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=48 )
`laravel/horizon` - let's install this package. For demonstration, we will use the project that was created in this playlist.

	composer require laravel/horizon

[Error: - proc_open cannot allocate memory]( https://www.nicesnippets.com/blog/proc-open-fork-failed-cannot-allocate-memory-laravel-ubuntu )

[(1:04)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=64 )
We create a configuration file.

	php artisan vendor:publish --provider="Laravel\Horizon\HorizonServiceProvider"

[(1:15)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=75 )
`.env` -> Change the driver to redis ( `QUEUE_DRIVER = redis` ) - IF you DON'T know how to install and connect Laravel to the redis server, see this video in the queue playlist.
! - In Laravel 7.7 ( `QUEUE_CONNECTION = redis` (- instead of `database` ))
	
[(1:30)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=90 )

	php artisan config:cache

[(1:35)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=95 )
Let's open the file with the configuration of this package - `config/horizon.php`( `'use' => 'default'` ) - this is the NAME for connecting to the redis server, this package will store the information necessary for it
functioning - Unsuccessful tasks, performance indicators, and other information. Remember that we are not limited to one redis server. IF you set up queues especially without changing anything, then DO NOT touch either.
Next, prefix is ​​used to store the package data. IF he does NOT conflict with anything, also DO NOT change it. ( `'waits'` ) - this is the queue threshold in seconds for each type of connection before the event is triggered.
( `'trim'` ) - here you can configure how long in minutes you want the last Failed tasks to be stored. By default, the latest tasks are stored for `1 hour`. And ALL UNSUCCESSFUL - within a week ( `10080` )
( `'environments' + 'local'` ) - these are essentially options for working with queues in different states. For example, to work in production, that is, when your application is already used by the user. ( `'tries' => 3` ) - this option is responsible for the quantity
attempts to complete the task. ( `'processes'` ) - the number of processes. ( `'balance'` ) - is a load balancer. It has `3 states` - `simple | automatic | false`. `simple` - evenly distributes tasks between loads.
`auto` - adjusts the number of workflows in the queue based on the load. For example, IF the notification queue( `notifications: 1000 render: 0` ) has 1000 jobs waiting, while the rendering queue is empty, this package will allocate more.
worker in the notification queue until it becomes empty.

[(3:12)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=192 )
And what are these types of queues? ( `notifications, render` ) - by placing tasks in different queues, you can divide them into categories, as well as set priorities according to the number of processors of different queues. The default name is default.
IF this parameter is set to false, then the default behavior will be used which processes the queues in the order specified in your configuration.

[(3:38)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=218 )
Launch our panel and handler. This means that you do NOT need to run a separate handler.

	php artisan horizon

[(3:48)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=228 )
Launch the development server:

	php artisan serve

[(3:54)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=234 )
By default, the panel is available at the following address.

	127.0.0.1:8000/horizon

- Now run the task in the queue:
	
	`127.0.0.1:8000`

[(4:14)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=254 )
- Now we will comment out the event in `rotes/web.php` which will cause the ERROR -> // PrepareJob And run again:

	`127.0.0.1:8000`

- We look at the panel:

	`127.0.0.1:8000/horizon`

[(4:30)]( https://youtu.be/HhhzBtoVXR0?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH&t=270 )
In this panel, you can see the execution of the task ( `Metrics` ) - categories of queues, ( `Recent` ) - recent, and Failed ( `Failed` ) - which we can run again ( `Retry` ).

![screenshot of sample]( https://github.com/mslobodyanyuk/blob/master/dka-develop_queue.loc/public/images/5_6(4.48).png )

#### useful links:

[Error: - proc_open cannot allocate memory]( https://www.nicesnippets.com/blog/proc-open-fork-failed-cannot-allocate-memory-laravel-ubuntu )
				
Useful links:
=============
[LARAVEL - QUEUE]( https://www.youtube.com/playlist?list=PLD5U-C5KK50Xo5mG_JPzyjIv-d3R7gqGH )
