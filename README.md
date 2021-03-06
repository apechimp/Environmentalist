Environmentalist
================

[![Build Status](http://teamcity.codebetter.com/app/rest/builds/buildType:%28id:bt1100%29/statusIcon)](http://teamcity.codebetter.com/viewType.html?buildTypeId=bt1100&guest=1)

Configure your .Net application via environment variables in a manner where
you're not calling ``Environment.GetEnvironmentVariable`` all over the place.

Installation
============
Install via nuget
``
PM> Install-Package Environmentalist
``

Usage
=====

Define an interface
-------------------
Define your configuration via an interface. E.g.,
```c#
public interface ITestConfig
{
    [FromEnvironment("COOL_DICTIONARY")]
    IDictionary<string, string> CoolDictionary { get; }

    [FromEnvironment("COOL_PROPERTY")]
    string CoolProperty { get; }
}
```
Here, we're using the ``FromEnvironment`` attribute to tell Environmentalist
which environment variable to use with this property.

Create an instance
------------------
Create an instance of your configuration via
```c#
var config = Configurator.Create<ITestConfig>(new
    {
	CoolDictionary = new Dictionary<string, string>(),
        CoolProperty = "foo"
    }
);
```

The supplied object serves as a set of defaults if Environmentalist can't find
a given variable in the environment. Properties which aren't strings are
automatically deserialized from the enviroment as JSON.

???
---
Environmentalist doesn't assume anything about how you use your configuration.
You can just pop the returned object into your IoC container of choice or wrap
it in a singleton or static class if you want to use an API closer to the
standard way of doing config.
