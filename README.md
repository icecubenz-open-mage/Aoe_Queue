# Aoe_Queue

## How to run unit tests

    cd <MagentoDocRoot>

    # Get phpunit
    wget http://pear.phpunit.de/get/phpunit.phar
    chmod +x phpunit.phar

    # Run tests
    ./phpunit.phar tests/Aoe_Queue/QueueTestcase.php

## Adding a task to the queue

    $queue = Mage::getModel('aoe_queue/queue'); /* @var $queue Aoe_Queue_Model_Queue */
    $queue->addTask('aoe_queue/dummy::test', array('-+', '5'));

## Processing the queue

    $queue = Mage::getModel('aoe_queue/queue'); /* @var $queue Aoe_Queue_Model_Queue */
    $messages = $queue->receive(5); /* @var $messages Zend_Queue_Message_Iterator */
    foreach ($messages as $message) { /* @var $message Aoe_Queue_Model_Message */
        $message->execute();
    }

## Cron configuration

    In System > Configuration > Advanced > System > Queue

    Use Aoe_Scheduler to run a separate cronjob to process the aoe_queue task so this won't block the other tasks.


## Exceptions

In this fork exceptions are logged to cron and the job is discarded.
If you need to retry on exception use `Aoe_Queue_RecoverableException` - this will log to cron and be queued again after the timeout has expired.
