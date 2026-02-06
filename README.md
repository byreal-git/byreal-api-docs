# Byreal API Docs

This repository is a collection of Byreal API documentation.
It is designed as an entry point for developers and integrators to quickly find each service's API spec, request/response examples, and usage notes.

## Router Service Documentation

- Router Service: [router.md](./router.md)

### How to Use

1. Open the service doc from the list above.
2. Follow endpoint/request/response definitions in each file.
3. Keep this README updated when new service docs are added.

### Contributing Docs

When adding a new service document:

1. Create a new Markdown file in the repository root (for example: `order.md`).
2. Add the link in `README.md` under `Service Documentation`.
3. Use clear sections like `Overview`, `Endpoint`, `Request`, and `Response` for consistency.



## Byreal API Documentation


Our API is fully documented using the **Swagger/OpenAPI** specification. You can explore and test the APIs using the provided `swagger.json` file.

---

### 1. Download `swagger.json`

Please download the `swagger.json` file from the project repository or the link provided:

```

swagger.json

```

This file contains all the API definitions, including endpoints, request parameters, and response formats.


### 2. Using Swagger UI

Swagger provides a visual interface to browse, understand, and test the APIs.

#### Method 1: Online Swagger Editor

1. Open [Swagger Editor](https://editor.swagger.io/).
2. Click **File → Import File**.
3. Select the downloaded `swagger.json`.
4. The editor will load all API endpoints with descriptions, parameters, and response examples.
5. You can explore and test the APIs directly in the browser.

#### Method 2: Local Swagger UI

1. Download the [Swagger UI static files](https://github.com/swagger-api/swagger-ui).
2. Place `swagger.json` in the `dist` directory.
3. Edit `index.html` to point to your local JSON file:

```html
url: "swagger.json",
````

4. Open `index.html` in your browser to view and interact with the APIs.


### 3. Using Postman

You can also import `swagger.json` into Postman for testing.

1. Open the Postman application.
2. Click **File → Import → Upload Files**.
3. Select the downloaded `swagger.json`.
4. Postman will generate a collection with all API endpoints, grouped by path and HTTP method.
5. You can select an API, fill in the request parameters, and send the request for testing.

> **Tip:** If authentication is required (e.g., API Key, Bearer Token), configure it under **Authorization** in Postman before testing.

### 4. Notes & Tips

* **API Grouping:** APIs are organized by modules for easier navigation.
* **Request Examples:** Each endpoint includes sample requests and responses.
* **Testing:** APIs can be tested directly in Swagger UI or Postman, provided the backend service is running and accessible.
* **Export:** The API definitions can be exported in JSON/YAML format for SDK generation or further development.

