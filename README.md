# Module 10

Muhammad Brian Subekti 2306256444




##  How to run

1. **Build the project**

   ```sh
   cargo build 
   ```

2. **Start the server**

   ```sh
   cargo run --bin server
   ```

   This will bind to `127.0.0.1:2000` and wait for incoming client connections.

3. **Start three clients**
   In *three* separate terminals, run:

   ```sh
   cargo run --bin client
   ```

   Each client will connect to the server on port 2000.

4. **Chat!**

    * Type a message in *any* client and press Enter.
    * The server logs the raw message it received and then broadcasts it back to *all* connected clients.
    * Each client prints incoming messages prefixed with `From server:`.

---

##  Demo

![Proof of Running](img/Proof_Of_Running.png)

In the screenshot above:

1. **Top-left** window is the **server**, showing:

   ```
   listening on port 2000
   New connection from 127.0.0.1:55312
   New connection from 127.0.0.1:55313
   New connection from 127.0.0.1:55314
   From client 127.0.0.1:55312: "halo"
   From client 127.0.0.1:55313: "keren banget"
   From client 127.0.0.1:55314: "cool"
   ```
2. **Other three** windows are **clients**. When you type:

   ```
   halo
   ```

   in client 1, *all* clients display:

   ```
   From server: halo
   ```

   and similarly for the other messages.

---

##  What happens under the hood

* **Server**

    * Listens on TCP port 2000.
    * Accepts each connection and spawns a lightweight task to read lines.
    * Whenever a line arrives, it pushes it onto a broadcast channel.

* **Clients**

    * Connect to the server’s socket.
    * Spawn two asynchronous tasks:

        1. **Reader**: listens for incoming broadcasts and prints them.
        2. **Writer**: reads your keyboard input and sends it to the server.

