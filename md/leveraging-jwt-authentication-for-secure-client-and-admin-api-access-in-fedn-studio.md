# Leveraging JWT Authentication for Secure Client and Admin API Access in FEDn Studio

In the dynamic landscape of modern web development, security stands as a paramount concern. With the proliferation of APIs driving interactions between clients and servers, implementing robust authentication mechanisms is essential. JSON Web Tokens (JWT) have emerged as a popular choice, offering a secure and efficient method for authenticating and authorizing users. In this technical blog post, we'll explore how JWT’s can be leveraged for both client and admin API access to authenticate federated clients to FEDn Studio.

![Image 1](https://cdn.prod.website-files.com/65b2c538561625e62bd16a2a/663b67f86e39d1e35285ad37_Screenshot%202024-05-08%20at%2013.48.48.png)

### **Understanding JWT Authentication**

JWT is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. It consists of three parts: a header, a payload, and a signature. These parts are base64-encoded and concatenated with periods to form the token.

*   **Header**: Contains metadata about the token such as the type of token and the signing algorithm used.
*   **Payload**: Contains the claims, which are statements about an entity (typically, the user) and additional data.
*   **Signature**: Verifies that the sender of the JWT is who it says it is and ensures that the message wasn't changed along the way.

JWT authentication works by generating a token upon successful authentication, which is then included in subsequent requests to access protected resources. The server validates the token and grants access if the token is valid.

### **Implementing JWT Authentication for Federated FEDn Clients**

In FEDn, clients interact with the controller API (REST) and to gRPC endpoints (combiner). The Controller API acts as both a service discovery, so that training clients can ask for a particular combiner assignment (connect to aggregations gRPC server), and as admin CRUD interface for managing federated sessions. The gRPC server, the combiner, orchestrates and aggregates federated models to client and to backend services.  All of these different interfaces imply the necessity for an RBAC system. And for this we are using JWT’s based on different roles in custom claims of the generated token.

To secure these interactions, we'll implement JWT authentication as follows:

1.  **User Authentication**: When a user logs in to FEDn Studio, their credentials are verified, and upon successful authentication, they can generate JWT in a FEDn Project containing relevant user information (such as user ID and roles). This token can then be downloaded as a config file (client.yaml) and provided to the federated training client.
2.  **Token Storage**: The client stores the JWT securely (in the client.yaml), typically in local storage. The FEDn client includes the token in the Authorization header of subsequent API requests.
3.  **Server Validation**: Each API request from the client includes the JWT. The server validates the token's authenticity and checks whether the user has the necessary permissions to access the requested resource.
4.  **Token Expiry and Refresh**: JWTs can have an expiration time. Upon expiry, the client can request a new token using a refresh token, if provided, without requiring the user to log in again. The client.yaml configuration file includes the refresh token.

### **Enabling Admin API Access with JWT Authentication**

In addition to federated client access, FEDn Studio also provides an admin interface for managing the federated system settings and orchestrating the federations. Admin API access requires elevated privileges and is secured using JWT authentication:

1.  **Admin Authentication**: Admins of Projects authenticate themselves using their credentials, same to regular users. Upon successful authentication, an admin-specific JWT can be generated in the \`Settings\` section of the project. Here you need to copy the token and set the environment variable FEDN\_AUTH\_TOKEN or provide it directly as a class argument in the APIClient of the FEDn SDK. For details see documentation:  
    https://fedn.readthedocs.io/en/stable/quickstart.html#initialize-fedn-with-the-client-code-bundle-and-seed-model
2.  **Token Scope**: Admin JWTs contain additional claims indicating the admin role or specific permissions granted, enabling fine-grained access control.
3.  **Securing Admin Endpoints**: Endpoints accessible only to admins are protected by middleware that verifies the presence and validity of the admin JWT.
4.  **Revocation and Renewal**: Admin JWTs can be revoked if an admin's privileges are revoked or if there are security concerns. Refresh mechanisms ensure seamless continuation of administrative tasks without frequent reauthentication.

### **Conclusion**

JWT authentication provides a robust and scalable solution for securing both client and admin API access in FEDn Studio. By leveraging JWTs, developers can implement a flexible authentication mechanism that enhances security without compromising usability. With careful design and implementation, FEDn Studio ensures that only authenticated and authorized users, whether clients or administrators, can access its features, safeguarding sensitive data and maintaining trust in the federated system’s integrity.
