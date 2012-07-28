Using events and providers
==========================

CalendR can manage your events by adding them to the event manager, or by defining an event provider.

## Events

### How it works

You have to create an Event class that implements the CalendR\Event\EventInterface to use the event management.
You can use the built-in CalendR\Event\Event class, but in most of case you'd better to implement your own class.
You can also extend the CalendR\Event\Abstract event that provide base methods for your event class.

#### Define your event class

This class can be a Doctrine entity, for example.

```php

use CalendR\Event\AbstractEvent;

class Event extends AbstractEvent
{

    protected $begin;
    protected $end;
    protected $uid;

    public function __construct($uid, \DateTime $start, \DateTime $end)
    {
        $this->uid = $uid;
        $this->begin = clone $start;
        $this->end = clone $end;
    }

    function getUid()
    {
        return $this->uid;
    }

    public function getBegin()
    {
        return $this->begin;
    }

    public function getEnd()
    {
        return $this->end;
    }
}
```

## Providers

To retrieve your events via the CalendR event system, you need to create a provider

You have to implements the CalendR\Event\Provider\ProviderInterface to make your class become provider.

#### Define your providers

```php

use CalendR\Event\Provider\ProviderInterface;

class Provider implements ProviderInterface
{
    public function getEvents(\DateTime $start, \DateTime $end, array $options = array())
    {
        /*
            Your stuff, you have to return an event array here.
            In most of cases, it will be a database query, or a external
            service request (WebDAV ?)
        */
    }
}

```

#### Add your provider to the EventManager

```php

$factory->getEventManager()->addProvider(new Provider);

```

And that's all. You can now find your events from your provider via $factory->getEvents()


```php

$month = $factory->getMonth(2012, 01);
$events = $factory->getEvents($month);

```

### Extra : Bundled Providers

CalendR comes with 2 providers : Basic and Aggregate

The Basic provider is a simple array event storage.
The Aggregate provider allows you to use multiple providers
