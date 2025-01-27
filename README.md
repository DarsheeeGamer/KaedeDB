# KaedeDB - Lightweight File-Based Database with REST API - For Development and Prototyping

[Link to GitHub Repository: https://github.com/DarsheeeGamer/KaedeDB](https://github.com/DarsheeeGamer/KaedeDB)

**Warning: KaedeDB is intended for development, prototyping, and small-scale, non-critical applications. It is NOT recommended for production environments requiring high availability, scalability, or robust security guarantees. Consider using production-grade databases like MongoDB, PostgreSQL, or cloud-based database services for critical applications.**

KaedeDB is a simple, file-based database system that provides a MongoDB-like document store accessible through a RESTful API. It's designed to be easy to set up and use for development and experimentation. KaedeDB stores data in encrypted `.kdb` files and offers basic security features through API keys and file encryption.

## Features

*   **File-Based Storage:** Data is persisted in encrypted `.kdb` files for easy management and backup in development environments.
*   **MongoDB-like API:** Provides a familiar document-oriented data model (databases, collections, documents) for developers accustomed to NoSQL databases.
*   **RESTful API:**  HTTP-based API for Create, Read, Update, and Delete (CRUD) operations, facilitating integration with various applications and tools.
*   **Basic Security Features (Development Use):**
    *   `.kdb` database files are encrypted using `cryptography.fernet` (for basic protection of data at rest).
    *   Optional API Key authentication to control access to the API endpoints (suitable for development, NOT robust production security).
    *   API Keys are generated per IP address for basic access control.
*   **Command-Line Interface (CLI) for Server:**  `kaededb serve` command for easy server startup and configuration during development.
*   **Python Client Package:**  Convenient Python client library (`kaededb` package) to interact with the KaedeDB API from Python applications.
*   **Built-in API Documentation:**  Web-based API documentation endpoint (`/kapi/services/kdb/docs/`) with CSS styling for easy reference.

## Prerequisites

*   **Python 3.6 or higher** installed on your server and client machines.
*   **pip** (Python package installer) should be installed.

## Installation - KaedeDB Packages from PyPI (Recommended)

For the easiest setup, install the KaedeDB client and server packages directly from PyPI:

**Install KaedeDB Client:**

```bash
pip install kaededb
```

**Install KaedeDB Server:**

```bash
pip install KaedeDB-Server
```

After installation, the `kaededb serve` command will be available to start the server, and the `kaededb` Python client library can be imported in your Python projects.

## Running the KaedeDB Server in Development

1.  **Start the Server:**

    Open a terminal and use the `kaededb serve` command to start the server with default settings:

    ```bash
    kaededb serve
    ```

    This is suitable for development and testing. The server will run with:

    *   **Host:** `0.0.0.0` (accessible on all network interfaces)
    *   **Port:** `80`
    *   **Storage Path:** Current directory (`.`)
    *   **Authentication:** Enabled
    *   **Database File:** `my_database.kdb`

    Check the terminal output for the server address and configuration details.

2.  **Server Address:**

    The server will be accessible at `http://<your_server_ip>:<port>/kapi/services/kdb/`.  For local development, it's typically `http://localhost/kapi/services/kdb/` or `http://127.0.0.1/kapi/services/kdb/`.

### KaedeDB Server Command-Line Arguments (for Development Configuration)

The `kaededb serve` command provides arguments for development-time configuration:

*   `--host <host-ip>`:  Specify the host IP address to bind to (default: `0.0.0.0`).
*   `--port <port>`: Specify the port number for the server (default: `80`).
*   `--storage-path <location>`:  Set the directory for `.kdb` database files and API key tokens (default: current directory `.`).
*   `--set-authentication <true|false>`: Enable or disable API key authentication (default: `true`). Use `false` to disable API key checks for easier local testing.
*   `--default-db <dbname>.kdb`:  Specify the filename for the default database file (default: `my_database.kdb`).

**Example Commands:**

*   **Run server on port 8080, storing data in a custom directory, and disabling authentication (for open access during development):**

    ```bash
    kaededb serve --port=8080 --storage-path=/tmp/kaededb_data --set-authentication=false
    ```

*   **Run server on a specific IP address and port, using a different database filename:**

    ```bash
    kaededb serve --host=192.168.1.100 --port=9000 --default-db=my_dev_database.kdb
    ```

## Installation - KaedeDB Client (Python)

**Install from PyPI (Recommended):**

```bash
pip install kaededb
```

**Install from Source (for development):**

If you are contributing to the KaedeDB client library or want to install it directly from the source code:

1.  **Download or Clone the KaedeDB Repository:** (See Server Installation instructions for repository details)
2.  **Navigate to the root directory:** `cd path/to/your/KaedeDB`
3.  **Install the Client Package:** `pip install .`

## Using the KaedeDB Client (Python)

After installing the `kaededb` client package, you can use it in your Python scripts to interact with the KaedeDB API.

Refer to the example scripts `example_kdb_client_interactive.py` and `example_kdb_client.py` in the root directory of the repository for usage examples.

**Basic Client Usage:**

1.  **Import `KDBClient`:** `from kaededb import KDBClient`
2.  **Initialize `KDBClient`:**

    ```python
    base_url = "http://localhost/kapi/services/kdb"  # Replace with your server's address
    api_key = "YOUR_API_KEY"  # Get API key if authentication is enabled

    client = KDBClient(base_url, api_key)
    ```

3.  **Use Client Methods:**  Call methods like `client.list_databases()`, `client.insert_document()`, etc., to interact with the API.

## Accessing API Documentation

For detailed API endpoint information, run the KaedeDB server and open the built-in documentation in your browser:

```
http://<your_server_ip>:<port>/kapi/services/kdb/docs/
```

## Security Considerations (Development and Limited Use)

**KaedeDB's security features are basic and primarily intended for development and non-sensitive data scenarios.**

*   **No Production Security:**  **Do not use KaedeDB for production applications requiring robust security.** It lacks advanced security features like user authentication, authorization policies, input validation, and protection against various attack vectors.
*   **Basic Encryption:**  `.kdb` file encryption provides minimal protection against casual inspection of data at rest but is not designed for strong security guarantees.
*   **API Keys for Development Access Control:** API keys offer a simple way to control access during development but should not be relied upon for production-level authentication.
*   **HTTPS Not Implemented:**  HTTPS encryption for network communication is not implemented in this example. **Enabling HTTPS is crucial for production security** to protect data in transit and API keys.

**For Production Applications, Use Robust Database Systems:**

For any application handling sensitive data or requiring production-level reliability, scalability, and security, use established and robust database systems like:

*   **Cloud-based Database Services:** (e.g., AWS DynamoDB, Google Cloud Firestore, Azure Cosmos DB) - Offer scalability, managed security, and high availability.
*   **Production-Grade Databases:** (e.g., MongoDB, PostgreSQL, MySQL) - Mature, feature-rich databases designed for production workloads.

## Getting Help and Contributing

For issues, questions, or contributions, please open an issue on GitHub: [https://github.com/DarsheeeGamer/KaedeDB/issues](https://github.com/DarsheeeGamer/KaedeDB/issues).
