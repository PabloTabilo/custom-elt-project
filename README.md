# A simple project for learning ELT for a Data Engineer Role
The structure of the projects is as follows:

```bash
C:.
│   docker-compose.yaml
│   README.md
│
├───elt
│       Dockerfile
│       elt_script.py
│
└───source_db_init
        init.sql
```
## Branch: `build-pipeline`
### Objective:
The purpose of this project is to understand how data engineers work with data and how to move this data using Docker, Python subprocesses, and other tools.
* The `docker-compose.yaml` file contains information about the different images we will use to move the data.
    * In this case, we will have three images and a network that allows them to communicate, along with a volume to persist the information.
    * In simple terms, 
        * A source Docker container running a PostgreSQL database.
        * A destination Docker container where we will transfer data from the source.
        * A third Docker container running a Python script that manages the entire ELT process.
* In `docker-compose.yaml`, we define the two PostgreSQL containers and configure the ELT container, where the `elt/Dockerfile` specifies the environment needed to run the ELT script.

## Branch: `dbt`
### Objective:
Ok, now we create the `models` directory and the `macros` directory.
* we provide a clean, tranformed data ready for analysis.
* Macros and jinja for build reusable and modular code.
```sql
{% macro generate_ratings() %}
CASE
    WHEN user_rating >= 4.5 THEN 'Excellent'
    WHEN user_rating >= 4.0 THEN 'Good'
    WHEN user_rating >= 3.0 THEN 'Average'
    ELSE 'Poor'
END as rating_category
{% endmacro %}
```
* Perform automatic testing
```yaml
version: 2

models:
  - name: films
    description: "This table contains details about films"
    columns:
      - name: film_id
        description: "Unique identifier for the film."
        tests:
          - unique
          - not_null
```

# References:
* https://github.com/justinbchau/custom-elt-project/tree/main