# Crummy

[![Gem Version](https://badge.fury.io/rb/crummy.png)](http://badge.fury.io/rb/crummy)
[![Build Status](https://secure.travis-ci.org/zachinglis/crummy.png?branch=master)](http://travis-ci.org/zachinglis/crummy)
[![Code Climate](https://codeclimate.com/badge.png)](https://codeclimate.com/github/zachinglis/crummy)

Crummy is a simple and tasty way to add breadcrumbs to your Rails applications.

## Install

Simply add the dependency to your Gemfile:

      <code>
        gem "crummy", "~> 1.6.0"
      </code>

# Example

In your controllers you may add\_crumb either like a before\_filter or
within a method (It is also available to views).

      <code>
        class ApplicationController
          add_crumb "Home", '/'
        end

        class BusinessController < ApplicationController
          add_crumb("Businesses") { |instance| instance.send :businesses_path }
          add_crumb("Comments", :only => "comments") { |instance| instance.send :businesses_comments_path }
          before_filter :load_comment, :only => "show"
          add_crumb :comment, :only => "show"

          # Example for nested routes:
          add_crumb(:document) { [:account, :document] }

          def show
            add_crumb @business.display_name, @business
          end

          def load_comment
            @comment = Comment.find(params[:id])
          end
        end
      </code>

Then in your view:

      <code>
        <%= render_crumbs %>
      </code>

## Options for render\_crumbs

render\_crumbs renders the list of crumbs as either html or xml

It takes 3 options

The output format. Can either be :xml or :html or :html\_list. Defaults
to :html

<code>:format =\> (:html|:html\_list|:xml)</code>

The separator text. It does not assume you want spaces on either side so
you must specify. Defaults to <code>&raquo;</code> for :html and
<code><crumb></code> for :xml

<code>:separator =\> string</code>

Render links in the output. Defaults to *true*

<code>:links =\> boolean</code>

<code>:skip\_if\_blank =\> true</code>

Render
[Richsnipet](http:/support.google.com/webmasters/bin/answer.py?hl=en&answer=99170&topic=1088472&ctx=topic/)
Default to *false*

<code>:microdata =\> true</code>

With this option, output will be blank if there are no breadcrumbs.

### Examples

     <code>
      render_crumbs                        #=> <a href="/">Home</a> &raquo; <a href="/businesses">Businesses</a>
      render_crumbs :separator => ' | '    #=> <a href="/">Home</a> | <a href="/businesses">Businesses</a>
      render_crumbs :format => :xml        #=> <crumb href="/">Home</crumb><crumb href="/businesses">Businesses</crumb>
      render_crumbs :format => :html_list  #=> <ul class="" id=""><li class=""><a href="/">Home</a></li><li class=""><a href="/">Businesses</a></li></ul>
      render_crumbs :format => :html_list, :microdata => true
                                           #=> <ul class="" id=""><li class="" itemscope="itemscope" itemtype="http://data-vocabulary.org/Breadcrumb">
                                           #   <a href="/" itemprop="url"><span itemprop="title">Home</span></a></li></ul>
     </code>

A crumb with a nil argument for the link will output an unlinked crumb.

With :format =\> :html\_list you can specify additional params:
:active\_li\_class, :li\_class, :ul\_class, :ul\_id

### App-wide configuration

You have the option to pre-configure any of the Crummy options in an
application-wide configuration. The options above are available to
configure, with the exception of `:separator`, as well as many others.

The biggest difference is that `:separator` is not an option. Instead,
you have format-specific configuration options: `:html_separator`,
`:xml_separator`, and `:html_list_separator`. `:separator` can still be
overridden in the view.

Insert the following in a file named `config/initializers/crummy.rb`:

      <code>
        Crummy.configure do |config|
          config.format = :xml
        end
      </code>

Possible parameters for configuration are:

      <code>
        :format
        :links
        :skip_if_blank
        :html_separator
        :xml_separator
        :html_list_separator
        :first_class
        :last_class
        :ul_id
        :ul_class
        :li_class
        :active_li_class
        :microdata
      </code>

See `lib/crummy.rb` for a list of these parameters and their defaults.

## Notes

Test library is at [Crummy Test](https://github.com/zachinglis/crummy-test)

## Todo

-   Accept collections of models as a single argument
-   Accept instances of models as a single argument
-   Allow for variables in names. (The workaround is to do your own
    before\_filter for that currently)
-   Make a crumbs? type method

## Credits

-   [Zach Inglis](http://zachinglis.com) of [London
    Made](http://londonmade.co.uk)
-   [Rein Henrichs](http://reinh.com)
-   [Les Hill](http://blog.leshill.org/)
-   [Sandro Turriate](http://turriate.com/)
-   [Przemysław
    Kowalczyk](http://szeryf.wordpress.com/2008/06/13/easy-and-flexible-breadcrumbs-for-rails/)
    - feature ideas
-   [Sharad Jain](http://github.com/sjain)
-   [Max Riveiro](http://github.com/kavu)
-   [Kamil K. Lemański](http://kml.jogger.pl)
-   [Brian Cobb](http://bcobb.net/)
-   [Kir Shatrov](http://github.com/shatrov) ([Evrone
    company](http://evrone.com))
-   [sugilog](http://github.com/sugilog)
-   [Trond Arve Nordheim](http://github.com/tanordheim)
-   [Jan Szumiec](http://github.com/jasiek)
-   [Jeff Browning](http://github.com/jbrowning)
-   [Bill Turner](http://github.com/billturner)
-   [Andrew Nesbitt](http://github.com/andrew)

**Copyright 2008-2013 Zach Inglis, released under the MIT license**