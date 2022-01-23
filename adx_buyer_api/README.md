# Google AdWords and DoubleClick Ad Exchange Buyer Client Library

Welcome to the next generation Google-developed Ruby client library for the
AdWords and DoubleClick Ad Exchange Buyer API!

It contains full support for v201502 and v201506, with full stubs, and
a simplified programming interface that lets you handle everything in native
Ruby collections.


# Docs for Users

## 1 - Installation:

`google-adwords-api` and `google-adx-buyer-api` are ruby gems. See:

> [http://docs.rubygems.org/read/book/1]()

Install them using the gem install command:

    $ gem install --remote google-adwords-api
    $ gem install --remote google-adx-buyer-api

Please note the `google-adx-buyer-api` gem contains only DoubleClick Ad Exchange
Buyer client library examples. You need the AdWords library in order to use it
which is installed automatically as a dependency.

The following gem libraries are required:

  - savon
  - google-ads-common


## 2 - Using the client library:

Include the library with 'require':

    require 'adwords_api'

Then create an API instance:

    adwords = AdwordsApi::Api.new

The created API object will grant you access to all the services for all of the
currently supported vesions of the APIs. It uses a config file in:

    ENV['HOME']/adwords_api.yml to read all of your configurations by default.

There is an example configuration file shipped with these libraries.

You can also pass API a manually constructed config hash like:

    adwords = AdwordsApi::Api.new({
      :authentication => {
          :method => 'OAuth2',
          :oauth2_client_id: 'INSERT_OAUTH2_CLIENT_ID_HERE'
          :oauth2_client_secret: 'INSERT_OAUTH2_CLIENT_SECRET_HERE'
          :developer_token => 'DEVELOPER_TOKEN',
          :client_customer_id => '012-345-6789',
          :user_agent => 'Ruby Sample'
      },
      :service => {
        :environment => 'PRODUCTION'
      }
    })

To obtain OAuth2 client credentials, follow instructions
[on the wiki](https://github.com/googleads/google-api-ads-ruby/wiki/OAuth2).

Once the library instance is create, specify which service you're looking to
use, and which version:

    campaign_srv = adwords.service(:CampaignService, :v201306)

You should now be able to just use the API methods in the object you were
returned:

    # Get 'Id', 'Name' and 'Status' fields of all campaigns.
    campaigns = campaign_srv.get({:fields => ['Id', 'Name', 'Status']})

See the code in the examples directory for working examples you can build from.

*Note*: if your setup requires you to send connections through a proxy server,
please set the appropriate options in the config file or config hash. E.g.:

    config[:connection] = {
      :proxy => 'http://user:password@proxy_hostname:8080'
    }

*Note*: if you are using Ruby 1.8 you may need to include RubyGems to be able
to require other gems code. There are several ways to do it, the easiest one is
to pass '-rubygems' parameter to the ruby interpreter:

    $ ruby -rubygems my_program_that_uses_gems

You can also set this up in the environment:

    $ export RUBYOPT="rubygems"

Or add it to the bash configuration file:

    $ echo 'export RUBYOPT="rubygems"' >> ~/.bashrc


### 2.1 - Ruby names for a Ruby library:

In order to make things more Ruby-like for our Ruby developers, we've renamed
API objects and methods to more closely match Ruby conventions. This means using
snake_case for methods and parameters, and UpperCamelCase for class names.

For example, the `startDate` field of the Campaign object is named `start_date`
in the client library. The `get` method, returns a CampaignPage object which has
an `entries` and a `total_num_entries` field. So, to access the return
values, you would do this:

    response = campaign_srv.get(selector)
    num_entries = response[:total_num_entries]

Essentially, all you have to do is follow Ruby conventions, and the library will
do the rest. All of the examples are written following this standard.


### 2.2 - Using the Test Account:

For testing purposes, obtain a Test Account in the production environment. Any
request against a Test Account inccurs no API units charge. See
[this guide](https://developers.google.com/adwords/api/docs/test-accounts) for
more details.

To use the library against a Test Account, set the `client_customer_id property`
in the configuration file to its client customer ID.


### 2.3 - Logging:

It is often useful to see a trace of the raw SOAP XML being sent and received.
The quickest way of achieving this when debugging your application is by setting
the library.log_level configuration variable to 'DEBUG':

    config[:library] = {
      :log_level => 'DEBUG'
    }

or via configuration file (see example).

This will output the SOAP XML to stderr, which will usually show up in your
terminal window.

There's also an option of logging requests and XML to a file. In order to enable
this, you should create a standard Logger object and pass it to the library:

    require 'logger'
    logger = Logger.new('path/to/log_filename')
    logger.level = Logger::DEBUG
    adwords = AdwordsApi::Api.new
    adwords.logger = logger

Request details and units spend are logged at the INFO log level, while raw HTTP
headers and XML dumps are logged at the DEBUG log level. For more details on
using Logger refer to the Ruby Logger documentation.

### 2.4 - Calculating operations usage

Each AdWords API operation performed consumes a certain number of operations as
described in the
[rate sheet](https://developers.google.com/adwords/api/docs/ratesheet).

The amount of operations consumed is returned in the header part of the SOAP
response. This information can be obtained by passing a user block during the
method call:

    response = campaign_srv.get(selector) do |header|
      puts "Operations consumed: %d" % header[:operations]
    end

You can also retrieve the response body as the second block parameter:

    campaign_srv.get(selector) {|header, body| ... }

### 2.5 - GZip compression

The library offers a transparent compression option which can be enabled in the
configuration file or by the following setting:

    config[:connection] = {
      :enable_gzip => true
    }

Enabling this option will set the headers required to request the server to
respond in gzipped format. All requests are sent uncompressed regardless.


# Docs for Developers

## Rake targets

    $ rake generate

to regenerate the stubs if needed

    $ gem build google-adwords-api.gemspec
    $ gem build google-adx-buyer-api.gemspec

to build the gems

    $ rake test

to run unit tests on the library


## Where do I submit bug reports and feature requests?

Bug reports and feature requests can be posted on the
[library page](https://github.com/googleads/google-api-ads-ruby/issues).

Questions can be asked on forum

> [http://groups.google.com/group/adwords-api]()

Make sure to subscribe to our Google Plus page for API change announcements and
other news:

> [https://plus.google.com/+GoogleAdsDevelopers]()


# Copyright/License Info

## Licence

Copyright 2010-2015, Google Inc. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

> [http://www.apache.org/licenses/LICENSE-2.0]()

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Authors

Authors:

 - Sérgio Gomes
 - Danial Klimkin
 - Michael Cloonan

Maintainer:

 - Michael Cloonan
