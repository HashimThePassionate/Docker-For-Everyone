# Migrating the Database 🚀

Database migrations are essential for managing changes to your database schema over time. They allow you to incrementally update the database as your application evolves, ensuring consistency and reliability. Each tech stack has its specific tools and methods for handling migrations. In this guide, we'll explore how to manage database migrations in three different stacks:

1. **Node.js with MongoDB** 🛠️
2. **Django with PostgreSQL** 🧩
3. **FastAPI with PostgreSQL** 🌟

## 1. Node.js with MongoDB 🌐

### Understanding the Migration Setup

In our Node.js project, we use `migrate-mongo` to handle migrations. This tool is responsible for applying changes to the MongoDB database in a versioned manner, ensuring that the database schema remains consistent across different environments.

### Key Components 📋

1. **package.json**: -> [Package.js](./backend/package.json)
    ```json
    "devDependencies": {
      "jest": "^26.6.3",
      "migrate-mongo": "^8.1.4",
      "nodemon": "^2.0.7",
      "supertest": "^6.1.3"
    }
    ```
    - **migrate-mongo**: This package manages migrations for MongoDB.

2. **docker-entrypoint.sh**: **-> [docker-entrypoint.sh](./backend/docker-entrypoint.sh)**
    ```bash
    #!/bin/sh
    
    echo "Waiting for MongoDB to start..."
    ./wait-for db:27017 
    
    echo "Migrating the database..."
    npm run db:up 
    
    echo "Starting the server..."
    npm start 
    ```
    - **Waiting for MongoDB to start**: Ensures MongoDB is ready before running migrations.
    - **Migrating the database**: Runs the migration scripts to update the database schema.
    - **Starting the server**: Starts the Node.js server after migrations are applied.

3. **Migration File (in the `migrations` directory)**: **-> [migration file](./backend/migrations/20210208213312-populate-movies.js)**
    ```javascript
    module.exports = {
      async up(db, client) {
        await db
          .collection("movies")
          .insertMany([
            { title: "Avatar" },
            { title: "Star Wars" },
            { title: "Terminator" },
            { title: "Titanic" },
          ]);
      },
    
      async down(db, client) {
        await db.collection("movies").deleteMany({
          title: {
            $in: ["Avatar", "Star Wars", "Terminator", "Titanic"],
          },
        });
      },
    };
    ```
    - **up function**: Adds movie titles to the `movies` collection.
    - **down function**: Removes these movie titles if the migration is rolled back.

### Docker Compose Integration 🛠️

In our `docker-compose.yml` file, we have: **-> [docker-compose file](./docker-compose.yml)**

```yaml
services:
  web:
    depends_on:
      - api
    build: ./frontend
    ports:
      - 3000:3000

  api:
    depends_on:
      - db
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
    command: ./docker-entrypoint.sh

  db:
    image: mongo:4.0-xenial
    ports:
      - 27017:27017
    volumes:
      - vidly:/data/db

volumes:
  vidly:
```

- **command: ./docker-entrypoint.sh**: This runs the `docker-entrypoint.sh` script, which handles waiting for MongoDB, running migrations, and starting the server.

### How It All Comes Together 🎯

1. **Starting the Containers**: Running `docker-compose up` starts the containers in the correct order.
2. **Migration Process**: The `api` service applies pending migrations using `migrate-mongo`.
3. **Database Schema Updated**: The `up` function in the migration script updates the MongoDB database. The `down` function can roll back these changes if needed.

### Additional Details 📚

- **Configuration**: `migrate-mongo` uses a configuration file (`migrate-mongo-config.js`) to define the MongoDB connection settings and other migration-related settings.
- **Running Migrations Manually**: Use `npx migrate-mongo up` to apply migrations or `npx migrate-mongo down` to revert them.
- **Best Practices**: Always version control your migration scripts and ensure that migrations are tested in a staging environment before applying them to production.

---

## 2. Django with PostgreSQL 🧩

### Django Migration Commands 🛠️

Django's ORM handles migrations, making it easy to manage database schema changes.

1. **Creating a Migration**:
    - Define your model in `models.py`:
      ```python
      from django.db import models

      class Movie(models.Model):
          title = models.CharField(max_length=100)
      ```
    - Create a migration:
      ```bash
      python manage.py makemigrations
      ```
    - Apply the migration:
      ```bash
      python manage.py migrate
      ```

2. **Migration File Example**:
    ```python
    from django.db import migrations, models

    class Migration(migrations.Migration):

        dependencies = [
            # Define dependencies on other migrations
        ]

        operations = [
            migrations.CreateModel(
                name='Movie',
                fields=[
                    ('id', models.AutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                    ('title', models.CharField(max_length=100)),
                ],
            ),
        ]
    ```
    - **CreateModel**: Defines the creation of the `Movie` model.

### Additional Details 📚

- **Show Migrations**: Use `python manage.py showmigrations` to see the status of migrations.
- **Squashing Migrations**: Use `python manage.py squashmigrations app_name start_migration_number` to reduce the number of migration files.
- **Data Migrations**: Django allows you to write custom Python code inside migrations to modify data during the migration process.

### Best Practices 🌟

- **Keep Migrations in Sync**: Ensure your migrations are always in sync with your models.
- **Database Constraints**: Utilize Django’s database constraints to enforce data integrity.

---

## 3. FastAPI with PostgreSQL 🌟

### Alembic in Depth 📚

Alembic, used with SQLAlchemy in FastAPI, handles migrations.

1. **Setting Up Alembic**:
    - Install Alembic:
      ```bash
      pip install alembic
      ```
    - Initialize Alembic:
      ```bash
      alembic init alembic
      ```

2. **Creating a Migration**:
    - Define your models in `models.py` and create a migration script:
      ```bash
      alembic revision --autogenerate -m "create movie table"
      ```
    - Apply the migration:
      ```bash
      alembic upgrade head
      ```

3. **Migration File Example**:
    ```python
    from alembic import op
    import sqlalchemy as sa

    def upgrade():
        op.create_table(
            'movie',
            sa.Column('id', sa.Integer, primary_key=True),
            sa.Column('title', sa.String(length=100), nullable=False)
        )

    def downgrade():
        op.drop_table('movie')
    ```
    - **upgrade**: Creates the `movie` table.
    - **downgrade**: Drops the `movie` table if the migration is reverted.

### Additional Details 📚

- **Alembic Configuration**: Configured via `alembic.ini` and `env.py`, which define the SQLAlchemy URL and target metadata.
- **Manual Migration Commands**: Use `alembic upgrade head` to apply migrations or `alembic downgrade -1` to revert them.
- **Best Practices**: Review autogenerate migrations carefully and ensure they reflect the intended changes.

---

## Additional Considerations for Database Migrations 🚀

- **Backward Compatibility**: Ensure migrations are backward compatible to avoid breaking changes during deployments.
- **Zero-Downtime Migrations**: Aim for zero-downtime migrations, especially in production environments. This involves creating new columns instead of altering existing ones and populating data in phases.
- **Monitoring Migrations**: Monitor the impact of migrations on performance, especially for large datasets. Use tools like `EXPLAIN` in PostgreSQL to understand how changes might affect query performance.

---

## Conclusion 🎉

Database migrations are a vital part of maintaining and evolving your application's database schema. Whether you're using Node.js with MongoDB, Django with PostgreSQL, or FastAPI with PostgreSQL, understanding and managing migrations effectively will ensure that your application remains stable and scalable as it grows.

By following the best practices outlined in this guide and leveraging the tools available in each stack, you can confidently manage database changes and minimize potential issues during deployments.
