# AirBnB_clone_v4 (HBnB)

The last stage of a student clone of the AirBnB website, written in Python. One codebase carries every layer: a command-line console, a persistent data model, a REST API, server-rendered pages and a jQuery front end that talks to the API.

## Layers

| Layer | Where | What it does |
|---|---|---|
| Console | `console.py` | `(hbnb)` command interpreter: `create`, `show`, `destroy`, `all`, `update`, `quit` |
| Models | `models/` | `BaseModel` plus `User`, `State`, `City`, `Amenity`, `Place`, `Review` |
| Storage | `models/engine/` | JSON file storage by default, or MySQL through SQLAlchemy |
| REST API | `api/v1/` | Flask API for every model, with CORS and Swagger documentation |
| Static pages | `web_static/` | Hand-written HTML and CSS for the site |
| Rendered pages | `web_flask/` | Flask routes and Jinja templates over the models |
| Dynamic pages | `web_dynamic/` | The same pages, with jQuery scripts that call the API |
| Deployment | `0-` to `3-*_web_static.*` | nginx setup, archive packing and Fabric deploy scripts |

## Run it

Python 3 with Flask, flask-cors, flasgger and SQLAlchemy 1.4. The default file storage needs no database.

```sh
HBNB_API_HOST=127.0.0.1 HBNB_API_PORT=5001 python3 -m api.v1.app
```

The API is then at `http://127.0.0.1:5001/api/v1/` and its Swagger UI at `/apidocs/`. The front-end scripts expect the API on port 5001.

```sh
curl http://127.0.0.1:5001/api/v1/status
curl -X POST -H "Content-Type: application/json" -d '{"name": "Kenya"}' http://127.0.0.1:5001/api/v1/states
curl http://127.0.0.1:5001/api/v1/stats
```

For the pages, in another terminal:

```sh
python3 -m web_dynamic.101-hbnb      # serves http://0.0.0.0:5000/101-hbnb
```

The console:

```sh
python3 console.py
(hbnb) create State name="Kenya"
(hbnb) all State
```

### MySQL storage

Create the development database and user with `setup_mysql_dev.sql` (or `setup_mysql_test.sql` for tests), install `mysqlclient`, and set:

```sh
export HBNB_TYPE_STORAGE=db HBNB_ENV=dev
export HBNB_MYSQL_USER=hbnb_dev HBNB_MYSQL_PWD=hbnb_dev_pwd
export HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_dev_db
```

## API

All under `/api/v1`: `/status`, `/stats`, and create, read, update and delete for `/states`, `/cities`, `/amenities`, `/users`, `/places` and `/reviews`, plus `/places_search` and the place-amenity links. Each endpoint has a YAML spec in `api/v1/views/documentation/` that feeds the Swagger UI.

## Front end

`web_dynamic/static/scripts/` holds the jQuery for each stage of the site: an API status indicator, places loaded from `POST /places_search`, and amenity checkboxes that filter the results.

## Known issues

Read from the code; the front-end scripts were not exercised in a browser.

- `4-hbnb.js`, `100-hbnb.js` and `101-hbnb.js` test checkboxes with `:checked_box`, which is not a valid jQuery selector, so the amenity checkbox handler in those stages needs checking.
- Script URLs are hard-coded to `0.0.0.0:5001`, and `2-do_deploy_web_static.py` has the course servers' addresses hard-coded. Change both for any other setup.

## Credits

Written by [Jones Mereu](https://github.com/meta-kore) as a project for ALX Africa Software Engineering. The project follows the AirBnB clone curriculum.
