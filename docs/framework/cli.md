# Command-Line Interface

Running `app/console` lists all commands available. The following commands including **Doctrine Migrations** 
for creating and migrating database tables are supported out of the box:

Command                  | Description
-------------------------|----------------------------------------------------------------------------
migrations:execute       | Execute a single migration version up or down manually
migrations:generate      | Generate a blank migration class
migrations:migrate       | Execute a migration to a specified version or the latest available version
migrations:status        | View the status of a set of migrations
migrations:version       | Manually add and delete migration versions from the version table
database:create          | Create the database configured in app/config/parameters.yml
database:drop            | Drop the database configured in app/config/parameters.yml
database:insert-fixtures | Insert database fixtures for testing (see app/db/fixtures/)
user:create              | Create a new user
user:delete              | Delete a user
user:reset-password      | Send password reset email to a user
