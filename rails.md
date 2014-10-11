# Ruby on Rails Cheatsheet
For Rails 4.1.6 /
_ by @francisbautista _

## Initialization

### Creating a Project

    # Creating the Project
    $ rails new <project-name>

    # Install Dependencies
    $ bundle install

    # Open the directory
    $ cd <project-name>

    # Run the project
    $ rails start

### Initialization Steps for Twitter Bootstrap 3
1. **Configurations for bootstrap-sass in Gemfile.** The gem `bootstrap-sass` must be added to the Gemfile. Also, make sure that the `sass-rails` is available on the Gemfile.

    Add the following code to the default.jade file, just below the `block styles` line.

        gem 'bootstrap-sass', '~> 3.2.0'
        gem 'sass-rails', '~> 4.0.3'


2. **Stylesheet Configuration** Rename the following file `app/assets/stylesheets/application.css` to `app/assets/stylesheets/application.css.scss`. This configuration is for the Sass CSS pre-processor, which is a dependency of the Bootstrap configuration.

        @import "bootstrap-sprockets";
        @import "bootstrap";

    These files must be included just under the `*=require` statements on the file.

3. **Javascript Configuration** Require the Bootstrap Javascript dependencies by adding the following lines to your `app/assets/javascripts/application.js` file.

        //= require jquery
        //= require bootstrap-sprockets

    **Note:** Coffeescript can be used interchangeably with Javascript for the application.


## Program Logic

### Generate Controllers and Models
Rails can create controllers and models automatically for your application.

    rails generate model <entityname>

This will create two new files in your project: `app/controllers/<EntityName>_controller.rb` and `app/models/<EntityName>.rb`.

### Define the Information Schema and Migrations
To define the attributes of the your models, you can run the following:

    # rails generate model <entityname> attribute_name:data_type
    # A sample of an actual model generation command for a Category model:
    rails g model Category category_name:string category_code:integer

After you've finished creating the models for your application, migrations must be run to configure the database for the model. To do so, run the following commands:

    rake db:migrate

TODO: Include manual migrations, recommended gems, routing, and model relations.
