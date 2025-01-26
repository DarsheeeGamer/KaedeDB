# KaedeDB - Your Lightweight File-Based Database with REST API

KaedeDB is a simple, file-based database system that provides a MongoDB-like document store accessible through a RESTful API. It's designed for development, prototyping, or small-scale applications where a full-fledged database server might be overkill. KaedeDB stores data in `.kdb` files, which are encrypted for basic security and are readable through the API.

## Features

*   **File-Based Storage:** Data is persisted in encrypted `.kdb` files, making it easy to manage and backup.
*   **MongoDB-like API:** Provides a familiar document-oriented data model with databases, collections, and documents.
*   **RESTful API:**  Accessible over HTTP with standard CRUD operations (Create, Read, Update, Delete).
*   **Basic Security:**
    *   `.kdb` database files are encrypted using `cryptography.fernet`.
    *   API Key authentication (optional, can be enabled/disabled when starting the server).
    *   API Keys are generated per IP address.
*   **Command-Line Interface (CLI) for Server:**  Start and configure the server using the `kaededb serve` command.
*   **Python Client Package:**  Easy-to-use Python client library to interact with the KaedeDB API, installable via `pip install kaededb`.
*   **API Documentation:**  Built-in API documentation accessible through a web endpoint, styled with CSS for readability.

## Prerequisites

*   **Python 3.6 or higher** installed on your server and client machines.
*   **pip** (Python package installer) should be installed.

## Installation and Setup - Server

1.  **Download or Clone the KaedeDB Repository:**

    Get the KaedeDB code from [https://github.com/DarsheeeGamer/KaedeDB](https://github.com/DarsheeeGamer/KaedeDB).  If you have a repository (e.g., on GitHub), clone it:

    ```bash
    git clone https://github.com/DarsheeeGamer/KaedeDB.git
    cd KaedeDB
    ```
    If you downloaded a zip file, extract it and navigate to the extracted directory.

2.  **Navigate to the `kaededb_server` directory:**

    ```bash
    cd kaededb_server
    ```

3.  **Install the KaedeDB Server Package:**

    From inside the `kaededb_server` directory, run:

    ```bash
    pip install .
    ```

    This will install the `KaedeDB-Server` package, its dependencies (Flask, cryptography), and make the `kaededb` command available in your environment.

## Running the KaedeDB Server

1.  **Start the Server:**

    Open a terminal and use the `kaededb serve` command to start the server.

    ```bash
    kaededb serve
    ```

    This will start the KaedeDB API server with default settings:

    *   **Host:** `0.0.0.0` (accessible on all network interfaces)
    *   **Port:** `80`
    *   **Storage Path:** Current directory (`.`)
    *   **Authentication:** Enabled
    *   **Database File:** `my_database.kdb`

    You will see output in the terminal indicating the server is running and the configuration being used.

2.  **Server Address:**

    The server will be accessible at `http://<your_server_ip>:<port>/kapi/services/kdb/`.  Replace `<your_server_ip>` with the actual IP address or hostname of your server and `<port>` with the port the server is running on (default is 80).  If running locally, it's usually `http://localhost/kapi/services/kdb/` or `http://127.0.0.1/kapi/services/kdb/`.

### KaedeDB Server Command-Line Arguments

The `kaededb serve` command accepts the following arguments to customize server behavior:

*   `--host <host-ip>`:  Specify the host IP address to bind to. Default: `0.0.0.0` (all interfaces).
*   `--port <port>`: Specify the port number for the server to listen on. Default: `80`.
*   `--storage-path <location>`:  Specify the directory where `.kdb` database files and API key tokens will be stored. Default: current directory (`.`).
*   `--set-authentication <true|false>`: Enable or disable API key authentication. Default: `true` (authentication enabled).
*   `--default-db <dbname>.kdb`: Specify the filename for the default database file. Must end with `.kdb` extension. Default: `my_database.kdb`.

**Example Commands:**

*   **Run server on port 8080, store data in `/opt/kaededb_data`, and disable authentication:**

    ```bash
    kaededb serve --port=8080 --storage-path=/opt/kaededb_data --set-authentication=false
    ```

*   **Run server on a specific IP (e.g., `192.168.1.100`), port 9000, and use a database file named `data.kdb`:**

    ```bash
    kaededb serve --host=192.168.1.100 --port=9000 --default-db=data.kdb
    ```

*   **Run server with default settings (host `0.0.0.0`, port `80`, current directory storage, authentication enabled, `my_database.kdb`):**

    ```bash
    kaededb serve
    ```

## Installation and Setup - Client (Python)

1.  **Install the KaedeDB Client Package:**

    You can install the KaedeDB client package directly using pip, assuming you have packaged it correctly and made it available (e.g., published to PyPI or using a local index):

    ```bash
    pip install kaededb
    ```

    **Alternatively (for local development or if you haven't published the package):**

    Navigate to the root directory of the KaedeDB repository (where the client `setup.py` is):

    ```bash
    cd path/to/your/KaedeDB # Go to the root directory (containing setup.py for client)
    ```

    And install it from the local directory:

    ```bash
    pip install .
    ```

    Both methods will install the `KaedeDB` client package, making it available for import in your Python projects.

## Using the KaedeDB Client (Python)

After installing the `KaedeDB` client package, you can use it in your Python scripts to interact with the KaedeDB API.

See the example scripts `example_kdb_client_interactive.py` and `example_kdb_client.py` for usage demonstrations in the root directory of the repository.

**Key Client Usage Steps:**

1.  **Import `KDBClient`:**

    ```python
    from kaededb import KDBClient
    ```

2.  **Initialize `KDBClient`:**

    ```python
    base_url = "http://localhost/kapi/services/kdb"  # Replace with your server's address
    api_key = "YOUR_API_KEY"  # Obtain API key if authentication is enabled

    client = KDBClient(base_url, api_key)
    ```
    *   **`base_url`**:  Set this to the URL of your running KaedeDB server (e.g., `http://your_server_ip/kapi/services/kdb`).
    *   **`api_key`**: If authentication is enabled on the server, you'll need to obtain an API key for your client's IP address by making a `POST` request to the `/kapi/services/kdb/tokens/generate` endpoint (without an API key initially) and then use the generated key for subsequent requests. If authentication is disabled, you can set `api_key` to `None`.

3.  **Use Client Methods:**

    Use the methods of the `KDBClient` object (e.g., `client.list_databases()`, `client.create_database()`, `client.insert_document()`, etc.) to interact with the KaedeDB API. Refer to the example scripts and the API documentation for available methods and usage.

## API Documentation

KaedeDB provides built-in API documentation. Once you have the server running, you can access the documentation in your web browser by navigating to:

```
http://<your_server_ip>:<port>/kapi/services/kdb/docs/
```

This endpoint serves a styled HTML page with detailed information about all available API endpoints, request methods, headers, request bodies, query parameters, and response formats.

## Security Notes

*   **API Keys:** KaedeDB uses API keys for basic authentication.  API keys are specific to the IP address that requests them. Treat API keys as secrets and store them securely in your client applications.
*   **Encryption:** Database files (`.kdb`) are encrypted at rest using `cryptography.fernet`. However, this provides basic protection and is not a substitute for comprehensive security measures.
*   **HTTPS:** For any production or sensitive data scenarios, **strongly recommend enabling HTTPS** on your server to encrypt communication between clients and the server. This is essential to protect API keys and data in transit.
*   **Production Readiness:** KaedeDB is a simplified, file-based database example. It is **not designed for high-load, production environments** requiring scalability, high availability, or advanced security features. For production use, consider robust database systems like MongoDB, PostgreSQL, etc.
*   **Storage Path Security:** Ensure the `storage-path` you configure for KaedeDB server has appropriate file system permissions to prevent unauthorized access to database files and API keys.

## Getting Help

If you encounter issues, have questions, or want to contribute, please open an issue on GitHub at [https://github.com/DarsheeeGamer/KaedeDB/issues](https://github.com/DarsheeeGamer/KaedeDB/issues).

---

**Enjoy using KaedeDB!**
