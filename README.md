Mailgun-PHP-SDK
===========
This is the Mailgun PHP SDK. This SDK contains methods for easily interacting with the Mailgun API. Below are examples for utilizing the SDK!

Installation
-----
To install the SDK, you need to be using Composer in your project. If you aren't using Composer yet, it's really simple! Here's how to install composer and the Mailgun SDK.

```PHP
# Install Composer
curl -sS https://getcomposer.org/installer | php

# Add Mailgun as a dependency
php composer.phar require mailgun/mailgun-php-sdk:~1.0
``` 
Next, require Composer's autoloader to automatically load the Mailgun SDK:
```PHP
require 'vendor/autoload.php';
```

Usage
-----
Using the SDK is rather simple, if you're already familiar with our API. If not, no problem... Just know that the classes follow the API endpoints. So when you're reviewing our documentation, the endpoints are expressed as a class. 

Here's an example for sending a message: 

```php
# First, instantiate the client with your API credentials and domain. 
$mgClient = new MailgunClient("key-3ax6xnjp29jd6fds4gc373sgvjxteol0", "samples.mailgun.org");

# Next, instantiate a Message object on the messages API endpoint.
$message = $mgClient->Messages();

# Now, compose your message.
$message->setMessage(array('from' => 'me@samples.mailgun.org', 
                           'to' => 'php-sdk@mailgun.net', 
                           'subject' => 'The PHP SDK is awesome!', 
                           'text' => 'It is so simple to send a message.'));

# Finally, send the message.
$message->sendMessage();
```

Advanced Usage
--------------
You've sent your first message, awesome! Let's move on to more advanced scenarios. 

#### Message Builder
Message Builder makes creating your messages really intuitive. If you despise arrays, or your workflow is better off defining each part of the MIME separately, use this!

```php
# First, instantiate the client with your API credentials and domain. 
$mgClient = new MailgunClient("key-3ax6xnjp29jd6fds4gc373sgvjxteol0", "samples.mailgun.org");

# Next, instantiate a Message Builder object on the Messages API endpoint.
$message = $mgClient->Messages()->MessageBuilder();

# Define the from address.
$message->setFromAddress("me@samples.mailgun.org", array("first"=>"PHP", "last" => "SDK"));
# Define a to recipient.
$message->addToRecipient("john.doe@samples.mailgun.org", array("first" => "John", "last" => "Doe"));
# Define a cc recipient.
$message->addCcRecipient("sally.doe@samples.mailgun.org", array("first" => "Sally", "last" => "Doe"));
# Define the subject. 
$message->setSubject("A message from the PHP SDK using Message Builder!");
# Define the body of the message.
$message->setTextBody("This is the text body of the message!");

# Finally, send the message.
$message->sendMessage();

```

#### Batch Sending
Batch sending allows you to submit up to 1,000 messages per API call. This is the best way to send a large amount of messages as quickly as possible. In the example below, we'll use the MessageBuilder object to create a message.

```php
# First, instantiate the client with your API credentials and domain. 
$mgClient = new MailgunClient("key-3ax6xnjp29jd6fds4gc373sgvjxteol0", "samples.mailgun.org");

# Next, instantiate a Batch Message object on the Messages API endpoint. 
# (note: The Batch Message object automatically includes a Message Builder object)
$batchMessage = $mgClient->Messages()->BatchMessage();

# Define the from address.
$batchMessage->setFromAddress("me@samples.mailgun.org", array("first"=>"PHP", "last" => "SDK"));
# Define the subject. 
$batchMessage->setSubject("A Batch Message from the PHP SDK!");
# Define the body of the message.
$batchMessage->setTextBody("This is the text body of the message!");

# Next, let's add a few recipients to the batch job.
$batchMessage->addBatchRecipient("john.doe@samples.mailgun.org", array("first" => "John", "last" => "Doe"));
$batchMessage->addBatchRecipient("sally.doe@samples.mailgun.org", array("first" => "Sally", "last" => "Doe"));
$batchMessage->addBatchRecipient("mike.jones@samples.mailgun.org", array("first" => "Mike", "last" => "Jones"));

# Finally, submit the batch job.
$batchMessage->sendMessage();
```