# KaedeDB Client (Python)

[Link to GitHub Repository: https://github.com/DarsheeeGamer/KaedeDB](https://github.com/DarsheeeGamer/KaedeDB)

The `kaededb` Python client library provides a convenient way to interact with a KaedeDB server from your Python applications. KaedeDB is a lightweight, file-based database with a REST API, designed for development and prototyping.

**Warning:** This client library is designed to work with KaedeDB servers intended for development, prototyping, and small-scale, non-critical applications. Do NOT use KaedeDB or this client library for production applications requiring robust security or high reliability.

## Features

*   **Easy Interaction with KaedeDB API:** Provides Pythonic functions to call all KaedeDB API endpoints.
*   **Handles API Key Authentication:**  Automatically includes API keys in requests when interacting with a secured KaedeDB server.
*   **Simplified Data Handling:**  Handles JSON serialization and deserialization of requests and responses.
*   **Error Handling:**  Provides basic error handling for API requests, raising exceptions with informative error messages when API calls fail.

## Installation

**Install from PyPI (Recommended):**

```bash
pip install kaededb
```

**Install from Source (for development):**

If you are contributing to the KaedeDB client library or want to install it directly from the source code:

1.  **Download or Clone the KaedeDB Repository:**

    Get the KaedeDB code from [https://github.com/DarsheeeGamer/KaedeDB](https://github.com/DarsheeeGamer/KaedeDB).Clone it:

    ```bash
    git clone https://github.com/DarsheeeGamer/KaedeDB.git
    cd KaedeDB
    ```

2.  **Navigate to the root directory of the KaedeDB repository** (where `setup.py` is located).

3.  **Install the Client Package:**

    ```bash
    pip install .
    ```

## Usage

**Basic Usage Example:**

```python
from kaededb import KDBClient

# Replace with your KaedeDB server URL and API Key (if authentication is enabled)
base_url = "http://localhost/kapi/services/kdb"
api_key = "YOUR_API_KEY"  # Obtain API key from /tokens/generate endpoint if needed

client = KDBClient(base_url, api_key)

try:
    # List databases
    databases_response = client.list_databases()
    print("Databases:", databases_response['databases'])

    # Create a new database
    create_db_response = client.create_database(db_name="my_new_db")
    print("Create DB Response:", create_db_response['message'])

    # ... (Continue using other client methods to interact with collections, documents, etc.)

except Exception as e:
    print(f"API Error: {e}")
```

**Key `KDBClient` Methods:**

The `KDBClient` class provides methods that correspond to the KaedeDB REST API endpoints. Here are some of the main methods:

*   **Database Operations:**
    *   `list_databases()`: List all databases.
    *   `create_database(db_name)`: Create a new database.
    *   `delete_database(db_name)`: Delete a database.

*   **Collection Operations:**
    *   `list_collections(db_name)`: List collections in a database.
    *   `create_collection(db_name, collection_name)`: Create a new collection.
    *   `delete_collection(db_name, collection_name)`: Delete a collection.

*   **Document Operations:**
    *   `find_documents(db_name, collection_name, query=None)`: Find documents in a collection (supports query filters as a dictionary).
    *   `insert_document(db_name, collection_name, document)`: Insert a new document (as a dictionary).
    *   `get_document(db_name, collection_name, doc_id)`: Get a document by its ID.
    *   `update_document(db_name, collection_name, doc_id, update_data)`: Update a document.
    *   `delete_document(db_name, collection_name, doc_id)`: Delete a document.
    *   `generate_token()`: Generate a new API key (for token-based authentication).

Refer to the KaedeDB API documentation (accessible through the `/docs/` endpoint on your running KaedeDB server) for complete details on request parameters, response formats, and available API endpoints.

## Authentication

If the KaedeDB server you are connecting to has authentication enabled, you will need to obtain an API key.

1.  **Generate API Key:**
    *   Use the `client.generate_token()` method (or make a `POST` request to `/kapi/services/kdb/tokens/generate` directly) from the IP address where your client code is running.
    *   The API will return an API key specific to that IP address.

2.  **Initialize `KDBClient` with API Key:**
    *   When creating a `KDBClient` instance, provide the generated API key as the `api_key` argument:

        ```python
        client = KDBClient(base_url, api_key="YOUR_GENERATED_API_KEY")
        ```

    *   For API calls that require authentication, the `KDBClient` will automatically include the `X-API-Key` header with your API key in the requests.

If authentication is disabled on the KaedeDB server, you can initialize the `KDBClient` with `api_key=None`:

```python
client = KDBClient(base_url, api_key=None)
```

## Error Handling

The `KDBClient` methods include basic error handling. If an API request fails (e.g., due to network issues, server errors, or invalid requests), the client methods will raise a Python exception. You should wrap your API calls in `try...except` blocks to handle potential errors gracefully.

**Example Error Handling:**

```python
try:
    databases_response = client.list_databases()
    # ... process response ...
except Exception as e:
    print(f"API Error: {e}")
    # ... handle error ...
```

## Security Notes (Client-Side)

*   **Securely Store API Keys:** If you are using API key authentication, handle API keys securely in your client applications. Avoid hardcoding keys directly in your code if possible. Use environment variables, configuration files, or secure secret management mechanisms to store and retrieve API keys.
*   **HTTPS Communication:** For any real-world application, ensure that you are communicating with your KaedeDB server over HTTPS (`https://`) to encrypt communication and protect API keys and data in transit.

## Getting Help and Reporting Issues

For any issues, questions, or contributions related to the KaedeDB Python client library, please open an issue on GitHub: [https://github.com/DarsheeeGamer/KaedeDB/issues](https://github.com/DarsheeeGamer/KaedeDB/issues).
