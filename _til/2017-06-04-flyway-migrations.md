---
title:  "Flyway Migrations"
date:   2017-06-04 10:24:00
category: til
tags: [flyway, backend, database, migrations]
---

TIL how to configure the database migration framework [Flyway][flyway]{:target="_blank"}, as well as the difference between `Versioned` and `Repeatable` migrations.

### What is Flyway?

Flyway is a database migration framework by Boxfuse. Like your application has a version, migrations are used to "version" your database. A migration framework is useful to ensure your **database version is always in sync with your application version**, and that versions can be transitioned between in a deterministic way.

### How do Database Migrations Work?

Database migrations are just SQL scripts that are versioned, named, and run by the framework. Versioned migrations are run in the order in which they were created, and repeatable migrations are run after all versioned migrations have finished. Migrations are typically stored in the folder `src/main/resources/db/migration`. Migrations are run against a database with the `migrate` command.

When migrations are run, the migration framework creates a metadata table which tracks the migrations that have been applied: each row of the table represents a migration that has been run on the database. The migration framework checks this metadata table each time `migrate` is run to see which migrations, if any, need to be applied to the database.

Each migration script has a `checksum` value associated with it, which is used to detect if the contents of a migration script have changed. This is useful to detect inconsistencies in your migrations earlier on in the development process, before releasing to production.

### Integrating Flyway with Spring Boot

Before this week, I had only ever worked with [Django migrations][migration]{:target="_blank"}, which are baked in to the Django framework. Flyway is a 3rd party migration framework, so it requires a bit of configuration to get it working for an app.

There is a [Flyway gradle plugin][gradle]{:target="_blank"}, which is useful if you want to run your migrations at **build time.**

I realized after getting the plugin configured, though, that I was storing database configuration in multiple places: both in environment variables for the build step, and in application configuration for the JDBC connection in our app. I also realized that running migrations at build time would mean **re-building the app before deploying to each higher level environment** (Test, Integration, Production). This would increase the duration of each step in our release pipeline substantially.

Instead, I used the [Spring Boot Flyway integration][sb]{:target="_blank"} to have migrations run at runtime. This centralized all database and migration config to the `application-{envName}.yml` configuration files, and prevented us from needing to re-build the app multiple times during deploys. It is also one less step that developers to need to remember during development, since migrations just run automatically each time you boot the app!

### Versioned vs Repeatable Migrations

Flyway offers two different kinds of migrations: Versioned and Repeatable

[Versioned migrations][vers]{:target="_blank"} are prefixed with a `V` and a unique version number.
- They are run **once** by the migration framework, and are ignored on all subsequent migration runs.
  - If their checksum value ever differs from the value stored in the metadata table, the migration framework will throw an error. NOTE: the checksum value will change if a developer modifies the contents of a migration, even by adding comments!
- They usually contain SQL scripts that should be run once during the lifetime of a database, like schema definitions or user data corrections.

If you need to alter a schema applied by a migration, **create a new migration with the column updates** instead of modifying an old versioned migration (which would cause the checksum value to change).

[Repeatable migrations][repeat]{:target="_blank"} are prefixed with an `R`.
- They are meant to change, and can be applied multiple times by the migration framework on subsequent `migrate` runs.
- Since they can be run multiple times, it is up to the developer to ensure SQL statements will not throw errors on future runs.
  - e.g. use `ON CONFLICT DO NOTHING` clauses at the end of `INSERT` statements, to ensure future inserts of the same column will not throw errors.
- They are always applied **after** versioned migrations have finished running.
- We use repeatable migrations for seed data.

As stated previously, whenever the contents of a migration script are modified, the checksum value of the script changes. While this is undesirable in the case of versioned migrations, **flyway relies on the checksum value changing for repeatable migrations to detect when they need to be reapplied**. This is acceptable in this case.

### Environment Specific Migrations

Flyway can be configured to run migrations **by environment**. We separate our environment specific migrations out in to sub-folders in the `db/migration` directory, each named after the environment in which they should be run. We also put migrations that should be run in every environment in a folder called `all`.

Then, in `application-{envName}.yml`, we leverage the `flyway.locations` configuration parameter to point flyway to the correct folders (`all` and `envName`) in each environment.

An example of an environment specific migration, might be a versioned migration with a user-data correction that only needs to run in prod. Another example might be a repeatable migration that contains seed data that is only meant for the integration environment.

### Conclusion

A migration framework is critical to database development, to ensure database versions stay in sync with application versions in an automated way. I would never do database development without one!

[flyway]: https://flywaydb.org/
[migration]: https://docs.djangoproject.com/en/1.11/topics/migrations/
[gradle]: https://flywaydb.org/documentation/gradle/
[sb]: https://flywaydb.org/documentation/plugins/springboot
[vers]: https://flywaydb.org/documentation/migration/versioned
[repeat]: https://flywaydb.org/documentation/migration/repeatable
