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

## Program Logic

### Generate Controllers and Models
SailsJS can create controllers and models automatically for your application.

    sails generate api <entityname>

This will create two new files in your project: `api/controllers/<EntityName>Controller.js` and `api/models/<EntityName>.js`.

### Define the Information Schema
Open the generated model file and define the entity's data structure and how it will be validated.

*Sample Generated File with Data Attributes and Data Validations*

    /**
    * User.js
    *
    */

    module.exports = {

      attributes: {

        first_name: {
          type: 'string',
          required: true
        },

        last_name: {
          type: 'string',
          required: true
        },

        email: {
          type: 'string',
          required: true,
          unique: true,
          email: true
        },

        encryptedPassword: {
          type: 'string',
          required: false
        }

      }
    };

See [Official SailsJS Documentation](http://sailsjs.org/#/documentation/concepts/ORM/Validations.html) for data validation rules.

### Define Controller Actions and Associated Views (if needed)

The SailsJS Blueprint API would typically handle the default CRUD operations, but applications would also need custom screens to display or collect data. Follow these steps to create your own controller methods and views.

1. Open the controller file and define the action method.

        /**
         * UserController
         *
         * @description :: Server-side logic for managing users
         * @help        :: See http://links.sailsjs.org/docs/controllers
         */

        module.exports = {

            'new': function(req, res) {

                res.view();

            }

        };

  In this example, the action `new` would simply load the view file `views/user/new.jade`

2. Create the associated view file `views/<controller>/<action>.jade`

3. Define the action in the `config/routes.js` file. *REST and CRUD actions are automatically mapped by default to the routes using the SailsJS Blueprint API so no need to define them. Default CRUD shortcuts may be overrode in the controller.*

  Visiting `<site-url>/<controller>/<action>` would now display the corresponding view.


### Error Handling
SailsJS displays a default error screen when the model field validations are not met. Follow these steps to implement basic error handling in the controller and view:

1. Set the validation rules in the model (see previous sections)
2. Add the error catching logic in the controller method that triggers the error.

        'create': function(req, res, next) {

            ...

            User.create( req.params.all(), function userCreated (err, user) {

                // Do not proceed if there is an error
                if (err)
                {

                    // Show in console to help in debugging
                    console.log(err);

                    // Option 1: API Friendly -- default error handling (json)
                    // return next(err);

                    // Option 2: Error handling in view
                    // Store error to session
                    req.session.flash = {
                        err: err
                    };

                    // Redirect to the originating page
                    return res.redirect('/user/new');

                }

                ...

                // Clear the session flash variables when done
                req.session.flash = {};

            } );

         }

3. Allow the receiving page to access the session variable.

        'new': function(req, res) {

            // Set the flash variables for error display
            res.locals.flash = _.clone(req.session.flash);

            console.log(res.locals.flash);

            // Load the view
            res.view();

            // Clear the session flash variables
            req.session.flash = {};

        },

4. Display in the view

        - if( flash && flash.err )
          .alert.alert-danger
            strong= flash.err.summary
            ul.list-unstyled
                - each error in flash.err.invalidAttributes
                    li= JSON.stringify(error)


## Security Notes

+ **CSRF Protection** - Enable csrf protection in `config/csrf.js` then create a hidden field named `_csrf` with value of the variable `_csrf` in every form.

      // HTML Code
      <input type="hidden" name="_csrf" value="<%= _csrf %>">

      // Jade Code
      input(type='hidden', name='_csrf', value=_csrf)

+ **Data Model Schema** - add `schema: true` to the model to only allow explicitly specified attributes to be saved in the database.
