#JWKS Server with Key Management and Token Issuance
This project is a simple implementation of a JSON Web Key Set (JWKS) server that manages RSA keys and issues JSON Web Tokens (JWTs). The server provides endpoints to issue JWTs and retrieve the JWKS, which contains the public keys for verifying the JWTs.
Files Overview
1. KeyManager.cs
•	Description: This file contains the KeyManager class, which is responsible for managing RSA keys. It handles key generation, storage, and retrieval from a SQLite database. The keys are stored in PEM format, and the class provides methods to export/import keys to/from this format.
•	Key Features:
o	Database Initialization: Creates a SQLite database and a keys table if they don't exist.
o	Key Generation: Generates 2048-bit RSA keys and stores them in the database with an expiry timestamp.
o	Key Retrieval: Retrieves valid (non-expired) keys and expired keys from the database.
o	PEM Format Handling: Exports and imports RSA keys in PEM format.
2. Program.cs
•	Description: This file sets up the ASP.NET Core application, initializes the KeyManager, and configures the middleware to handle routing and endpoints.
•	Key Features:
o	Dependency Injection: Registers KeyManager as a singleton service.
o	Key Initialization: Generates sample keys (one valid and one expired) when the application starts.
o	Middleware Configuration: Sets up routing and endpoints for the application.
3. AuthController.cs
•	Description: This file contains the AuthController class, which provides an endpoint to issue JWTs. The tokens can be issued with either a valid or an expired key, depending on the query parameter.
•	Key Features:
o	Token Issuance: Issues JWTs with claims for a user and an admin role.
o	Key Selection: Uses either a valid or expired key based on the expired query parameter.
o	JWT Creation: Creates JWTs with a header containing the Key ID (kid) and a payload with standard claims.
4. JwksController.cs
•	Description: This file contains the JwksController class, which provides an endpoint to retrieve the JWKS. The JWKS contains the public keys of all valid (non-expired) keys managed by the KeyManager.
•	Key Features:
o	JWKS Retrieval: Retrieves all valid keys and formats them in JWKS format.
o	Key Formatting: Formats the keys with their Key ID (kid), key type (kty), algorithm (alg), intended use (use), modulus (n), and exponent (e).
How It Works
1.	Database Initialization:
o	When the application starts, the KeyManager initializes the SQLite database and creates the keys table if it doesn't exist.
2.	Key Generation:
o	The KeyManager generates two RSA keys: one valid (expires in 1 hour) and one expired (expired 1 hour ago). These keys are stored in the database.
3.	Token Issuance:
o	The AuthController provides an endpoint (/auth) to issue JWTs. The expired query parameter determines whether the token should be signed with a valid or expired key.
o	The JWT includes claims for a user and an admin role, and the header contains the Key ID (kid).
4.	JWKS Retrieval:
o	The JwksController provides an endpoint (/.well-known/jwks.json) to retrieve the JWKS. The JWKS contains the public keys of all valid keys, formatted according to the JWKS specification.
Running the Application in Microsoft Visual Studio
1.	Prerequisites:
o	Microsoft Visual Studio (2022 or later recommended).
o	.NET 6 SDK or later (installed with Visual Studio).
o	SQLite (the database file will be created automatically).
2.	Steps:
o	Open the project in Visual Studio:
	Launch Visual Studio.
	Select File > Open > Project/Solution.
	Navigate to the project directory and select the .csproj file.
o	Build the project:
	In Visual Studio, click Build > Build Solution to compile the project.
o	Run the application:
	Press F5 or click Debug > Start Debugging to run the application.
	Alternatively, click Debug > Start Without Debugging (Ctrl + F5) to run without attaching the debugger.
o	The application will start and initialize the database, generate sample keys, and be ready to handle requests.
o	The output should be like this : 
 
3.	Endpoints:
o	Issue a JWT:
	Valid Token: POST /auth
	Expired Token: POST /auth?expired=true
o	Retrieve JWKS: GET /.well-known/jwks.json
4.	Accessing the Application:
o	Once the application is running, you can access the endpoints using a tool like Postman, curl, or a web browser.
o	The default URL for the application is http://localhost:5000 or https://localhost:5001 (if HTTPS is enabled).
Notes
•	The SQLite database file (totally_not_my_privateKeys.db) will be created in the project's output directory (e.g., bin/Debug/net6.0).
•	If you encounter any issues, ensure that the necessary NuGet packages (e.g., Microsoft.Data.Sqlite, Microsoft.AspNetCore) are installed and up to date.
Grade Bot issue :
I have included SQL insertion parameters in the KeyManager.cs source file:
 
 But it looks like what the grade bot is seeking for. So I got 0 on that. If possible, you can review my code to see the SQL insertion parameters.
