# Fluf

Fluf is a small wrapper for Community that allows you to configure a YAML file to predefine static properties by dynamically calling setters within your class.

  - PSR-0 compliant
  - Easy to use
  - Stable

## Dependencies
  - Outglow\Community
  - Symfony\Yaml

## Installation
Add `outglow/fluf` to your composer.json file

    {
        "require" : {
            "outglow/fluf" : "dev-master"
        }
    }
Then run: `php composer.phar install`
(NOTE: An update may be required first)

## Usage
Here we will autoload both Community and Fluf into our project:

    include('vendor/autoload.php');
    
    use Outglow\Component\Community\Community;
    use Outglow\Component\Fluf\Fluf;
    
    $community = new Fluf(new Community());
    
    $community->set('Facebook', function() {
        return new Facebook();
    });
    
Here, we have just used Community like it normally works, and for the example, we are loading some sort of Facebook SDK class. But for something like that, we'd need need predefined properties, such as an AppID or an App Secret. With Fluf, we can do that by using a YAML file for our configuration.

    app_id: YOURAPPID
    app_secret: YOURAPPSECRET

For the example, let's say we call this `facebook.yml` and store it in the root of our project. To load it in, we'd do the following:

    include('vendor/autoload.php');
    
    use Outglow\Component\Community\Community;
    use Outglow\Component\Fluf\Fluf;
    
    $community = new Fluf(new Community());
    
    $community->prepare(function()
        return array(
            'key'  => 'Facebook',
            'data' => function() {
                return new Facebook();
            },
            'configuration' => 'facebook.yml'
        );
    });

So providing that in our Facebook class we had the methods `setAppId` and `setAppSecret`, our YAML file can store the values for those, and when the object is created using Community, those methods will be run automaticly.

You can still use Community how you normally would, with the `set`, `get` and `remove` methods available for you.

`prepare` is the only new public method so far that Fluf allows us to use along-side Community.
