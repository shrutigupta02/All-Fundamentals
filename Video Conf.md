This project is a **full-stack web application for real-time video and audio communication**, built using the MERN stack (MongoDB, Express, React, Node.js) along with WebRTC and Socket.IO. It includes user authentication, meeting history, and an in-call chat feature.

---
## Backend Server

The backend is built with Node.js and Express. 
It serves two primary functions: 
- handling user data via a REST API 
- managing real-time communication for video calls and chats via a Socket.IO signaling server.

### Core Setup (`app.js`)

- **Server Initialization**: An Express app is created, and then a standard Node.js `http` server is created from it. This `http` server is essential because Socket.IO needs to attach to it.
    
- **Database Connection**: It uses Mongoose to connect to a MongoDB Atlas cluster. The connection is established inside an `async` function `start()`, **which ensures the server only starts listening for requests after the database is connected.**
    
- **Middleware**:
    
    - `cors()`: Allows cross-origin requests, which is necessary for the React frontend (running on a different port/domain) to communicate with the backend.
        
    - `express.json()` and `express.urlencoded()`: These parse incoming JSON and URL-encoded request bodies, making them available in `req.body`.
        
- **Routing**: It registers the user-related API routes under the base path `/api/v1/users`.
    
- **Socket Integration**: It calls `connectToSocket(server)` from `socketManager.js`, passing the `http` server instance to initialize Socket.IO.
    

### Database (DB) Working & Schema

The application uses MongoDB as its database, managed by Mongoose. There are two main data models.

- **`User` Model (`user.model.js`)**:
    
    - **Schema**:
        
        - `name`: { String, required } - The user's full name.
            
        - `username`: { String, required, unique } - The user's unique identifier for login.
            
        - `password`: { String, required } - The user's password, which is **stored as a bcrypt hash**, not in plain text.
            
        - `token`: { String } - A simple, randomly generated string used for session management after a user logs in.
            
    - **Purpose**: To store user credentials and session information.
        
- **`Meeting` Model (`meeting.model.js`)**:
    
    - **Schema**:
        
        - `user_id`: { String } - Stores the `username` of the user who joined the meeting. This links a meeting record to a user.
            
        - `meetingCode`: { String, required } - The unique identifier for the meeting room (likely the URL path).
            
        - `date`: { Date, default: Date.now } - The timestamp of when the meeting was joined.
            
    - **Purpose**: To maintain a history of meetings joined by each user.
        

### Real-time Communication (`socketManager.js`)

This file is the **heart of the real-time functionality**. It doesn't handle the video/audio streams themselves (that's WebRTC's job), but it acts as a **signaling server** to coordinate connections between users.

- **In-Memory Storage**:
    
    - `connections`: An object where keys are room paths (e.g., the meeting URL) and values are arrays of `socket.id`s of users in that room.
        
    - `messages`: An object to store chat history for each room.
        
    - `timeOnline`: An object to track when each user connects.
        
    - **Note**: This data is volatile and will be lost if the server restarts.
        
- **Socket Events**:
    
    - `connection`: Fired when a new client connects to the server.
        
    - `join-call`: A client sends this event with a `path` (the meeting room ID). The server adds the client's `socket.id` to the corresponding room in the `connections` object. It then emits a `user-joined` event to all clients in that room, so they know a new peer has arrived and can initiate a connection. It also sends any existing chat history to the newly joined user.
        
    - `signal`: This is a crucial event for **WebRTC signaling**. When one user wants to connect to another, they exchange information like session descriptions (SDP) and network candidates (ICE). A client emits a `signal` event containing this information and the `toId` of the recipient. The server acts as a simple proxy, forwarding this message directly to the specified recipient.
        
    - `chat-message`: When a user sends a chat message, the server finds which room the user is in, stores the message in its in-memory `messages` object, and then broadcasts the message to every other user in the same room.
        
    - `disconnect`: When a user disconnects, the server finds the room they were in, removes their `socket.id`, and emits a `user-left` event to the remaining users in that room so they can tear down the corresponding peer connection and remove the user's video feed.
        

---

## Frontend (React)

The frontend is a single-page application built with React. It handles user authentication and the entire video call interface.

### Authentication (`AuthContext.js`, `auth.jsx`, `utility.js`)

- **`AuthContext`**: Provides a global context for authentication state and functions. It uses `axios` to create a client for making API calls to the backend. It exposes functions like `handleRegister`, `handleLogin`, `getHistoryOfUser`, etc.
    
- **Login/Register Flow**: The `Authentication.jsx` component provides the UI for login and registration. When a user submits the form, it calls the corresponding function from `AuthContext`.
    
    - On successful login, the `handleLogin` function receives a `token` from the backend, **stores it in the browser's `localStorage`**, and navigates the user to the `/home` page.
        
- **Protected Routes (`withAuth`)**: `withAuth` is a Higher-Order Component (HOC) that wraps other components. It checks if a `token` exists in `localStorage`. If not, it automatically redirects the user to the `/auth` page, effectively protecting routes from unauthenticated access.
    

### Video & Meeting Component (`video.jsx`)

This is the most complex component, responsible for the entire video call experience.

- **Permissions & Media**: On load (`useEffect`), it calls `getPermissions()`, which uses `navigator.mediaDevices.getUserMedia()` to request access to the user's camera and microphone. The resulting media stream is then attached to the local `<video>` element.
    
- **Socket Connection**: After the user enters a username and clicks "Connect", the `connectToSocketServer()` function is called. This establishes a connection to the backend Socket.IO server.
    
    - Once connected, it immediately emits a `join-call` event, sending the current page's URL as the room identifier.
        
- **WebRTC Peer Connections**: The core of the video streaming is **WebRTC**, which enables direct peer-to-peer (P2P) connections between browsers.
    
    - **Initiation**: When the client receives a `user-joined` event from the server, it knows a new peer is in the room. It creates a new `RTCPeerConnection` object for that peer.
        
    - **Signaling Handshake**:
        
        1. **Offer**: The existing user (the "offerer") creates an SDP offer using `peerConnection.createOffer()`. This offer describes their media capabilities.
            
        2. **Sending Offer**: The offer is sent to the new user via the server, using the `signal` socket event.
            
        3. **Answer**: The new user receives the offer, sets it as their remote description (`setRemoteDescription`), and then creates an answer with `peerConnection.createAnswer()`.
            
        4. **Sending Answer**: This answer is sent back to the original user, also via the `signal` socket event.
            
    - **ICE Candidates**: During this process, both browsers find potential network paths to connect to each other (ICE candidates). Whenever a candidate is found, it's sent to the other peer via the `signal` event.
        
    - **Stream Established**: Once the handshake is complete and a network path is established, the `onaddstream` event handler on the `RTCPeerConnection` fires. This handler receives the remote user's media stream, which is then attached to a new `<video>` element and displayed on the screen.
        
- **UI Controls**: The component includes buttons to:
    
    - **Toggle Video/Audio**: Enable or disable tracks on the local `MediaStream`.
        
    - **Screen Share**: Uses `navigator.mediaDevices.getDisplayMedia()` to capture the screen and replaces the camera track in the `MediaStream`.
        
    - **End Call**: Stops all media tracks and redirects the user.
        
    - **Chat**: Toggles a chat modal, sends messages using the `chat-message` socket event, and displays incoming messages.
        

---

## API Calls & Socket Events Summary

### REST API Calls (Client ➡️ Server)

|Method|Endpoint|Called When...|Purpose|
|---|---|---|---|
|`POST`|`/api/v1/users/register`|User submits the registration form.|Create a new user account.|
|`POST`|`/api/v1/users/login`|User submits the login form.|Authenticate a user and get a session token.|
|`POST`|`/api/v1/users/add_to_activity`|A logged-in user joins a meeting room.|Save the meeting code to the user's history.|
|`GET`|`/api/v1/users/get_all_activity`|A logged-in user views their history page.|Retrieve all past meetings joined by the user.|

### Socket Events

#### Client ➡️ Server

|Event Name|Emitted When...|Purpose|
|---|---|---|
|`join-call`|A user first connects to the meeting page.|To join a specific meeting room on the server.|
|`signal`|A WebRTC offer, answer, or ICE candidate is generated.|To send signaling data to another specific peer.|
|`chat-message`|A user sends a message in the chat.|To broadcast the chat message to everyone in the room.|

#### Server ➡️ Client

| Event Name     | Emitted When...                                                        | Purpose                                                                 |
| -------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `user-joined`  | A new user joins the room that the client is in.                       | To inform clients to create a new `RTCPeerConnection` for the new user. |
| `user-left`    | A user in the same room disconnects.                                   | To inform clients to close the connection and remove the user's video.  |
| `signal`       | The server receives a `signal` event intended for this client.         | To deliver WebRTC signaling data from another peer.                     |
| `chat-message` | The server receives a `chat-message` from anyone in the client's room. | To display a new chat message in the UI.                                |


## Important question:
"So, your resume says you deployed the project on AWS. Can you walk me through your deployment architecture?"

**Your Answer:** "That's a great question. For this project, my deployment strategy was centered on using **managed services** to ensure reliability and speed. The database is the most critical part, so I used **MongoDB Atlas and provisioned it on their AWS infrastructure**.

This Platform-as-a-Service (PaaS) approach meant I didn't have to worry about database administration like patching, backups, or scaling. It allowed me to focus purely on building the application's features.

### 1. MERN Stack & Platform Development

_"Developed enterprise-grade video conferencing platform using MERN stack and Socket.io"_

#### **Q1: Can you walk me through the architecture of your application? How did the different parts of the MERN stack interact?**

**A:** Sure! It was a standard MERN architecture.

- **MongoDB:** I used MongoDB Atlas as my database to store user data, like profiles, credentials, and meeting history.
    
- **Express.js:** This was the backbone of my backend. I built a RESTful API with Express to handle user authentication (register, login) and to fetch user data.
    
- **React.js:** I used React for the entire frontend. It handled the UI, from the login page to the video call interface itself. I managed the application's state, like user login status and meeting details, using React's Context API.
    
- **Node.js:** This was the runtime environment for my Express server and the Socket.io server. It allowed me to use JavaScript on the backend, which streamlined development since the frontend was also in JS.
    

The flow was simple: The React client made API calls to the Express server, which then queried the MongoDB database. For real-time features, the React client established a persistent connection with the Socket.io server, which was also running on Node.js.

#### **Q2: What do you mean by "enterprise-grade"? What features did you implement that support this claim?**

**A:** For this project, "enterprise-grade" referred to building a reliable and secure foundation. This included:

- **Secure Authentication:** Using JWT for stateless, secure user sessions and encrypting passwords with **bcrypt**.
    
- **Scalable Database:** Using MongoDB Atlas, which is a managed, cloud-native database that can scale easily.
    
- **Real-time Reliability:** Implementing features like presence detection (knowing who is in the call) and handling disconnects gracefully using Socket.io.
    
- **Cloud Deployment:** Deploying the application on AWS, which is the standard for enterprise applications, focusing on security best practices like using environment variables for secrets.
    

---

### ## 2. Socket.io & Real-time Communication

_"Implemented Socket.io for real-time bidirectional communication, handling video streaming and instant messaging"_

#### **Q1: Why did you choose Socket.io over plain WebSockets?**

**A:** I chose Socket.io mainly for its **reliability and feature set**. While WebSockets provide the basic bidirectional communication channel, Socket.io adds several crucial features on top of it, like:

- **Automatic Reconnection:** If a user's connection drops temporarily, Socket.io automatically tries to reconnect, which is essential for a good user experience in a video call.
    
- **Broadcasting & Rooms:** Socket.io's concept of "rooms" was perfect for this project. I could easily group all participants of a specific video call into a room and broadcast messages or signals to everyone in that call simultaneously.
    
- **Fallback Mechanisms:** If a user's browser or network doesn't support WebSockets, Socket.io can fall back to other methods like HTTP long-polling to ensure the connection still works.
    

#### **Q2: Your resume says Socket.io handled video streaming. Can you explain that process? Isn't video data very large for a server to handle?**

**A:** That's a great question, and I should clarify the wording. Socket.io was **not** used to stream the actual video/audio data. You're right, that would be incredibly inefficient and would overload the server.

Instead, I used Socket.io as a **signaling server** to facilitate **WebRTC** connections. Here's how it worked:

1. **Signaling:** When a user joins a call, Socket.io informs everyone else in the "room" that a new peer has arrived.
    
2. **Peer Discovery:** The users then exchange connection metadata (like SDP offers/answers and ICE candidates) through the Socket.io server. This is just small text data.
    
3. **Direct P2P Connection:** Once this signaling is complete, WebRTC establishes a direct **peer-to-peer (P2P)**connection between the users' browsers.
    
4. **Streaming:** The actual high-bandwidth video and audio data is then streamed directly between the users over this P2P connection, completely bypassing my server.
    

So, Socket.io's role was crucial for **coordinating** the connections, while **WebRTC** handled the heavy lifting of streaming.

---

### ## 3. JWT Authentication & Security

_"Developed JWT-based authentication system with React AuthContext for secure session management, password encryption, and user authorization controls"_

#### **Q1: Can you explain the structure of a JWT and walk me through your authentication flow?**

**A:** Absolutely. A **JSON Web Token (JWT)** has three parts separated by dots:

1. **Header:** Contains metadata like the algorithm used for signing (e.g., HMAC SHA256).
    
2. **Payload:** Contains the claims or user data. I included the `userId` and an expiration time in my payload.
    
3. **Signature:** A secure signature created by hashing the header, payload, and a secret key stored on the server. This verifies that the token hasn't been tampered with.
    

My authentication flow was:

1. **Login:** A user sends their `username` and `password` to my Express API.
    
2. **Verification:** The server validates the credentials. I used **bcrypt** to compare the submitted password with the hashed password stored in MongoDB.
    
3. **Token Creation:** On success, the server generates a JWT, signing it with a secret key.
    
4. **Token Sent to Client:** The server sends this JWT back to the React client.
    
5. **Token Storage:** The client stores the JWT securely, typically in `localStorage` or `sessionStorage`.
    
6. **Authenticated Requests:** For any future API requests that require authentication, the client includes the JWT in the `Authorization` header (e.g., `Authorization: Bearer <token>`). The server then verifies the token's signature before processing the request.
    

#### **Q2: How did you use React's Context API for this?**

**A:** I used the **Context API** to manage global authentication state across my entire React application. I created an `AuthContext` that provided:

- **State:** Information like `isAuthenticated` (a boolean), the user's data, and the JWT itself.
    
- **Functions:** Methods like `login()`, `register()`, and `logout()`.
    

By wrapping my main `App` component with an `AuthProvider`, any component in the tree could access the authentication state or call the login/logout functions without me having to pass props down through many levels. This made my code much cleaner and easier to manage. I also used it to create **protected routes** that would redirect unauthenticated users to the login page.

