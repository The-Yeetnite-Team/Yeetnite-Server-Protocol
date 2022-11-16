# TCP/UDP Yeetnite Server/Client Packet Specification

**Format:** `<PacketTypeID>;<PacketArgs>` (Simple, right?)

## Data Types:
* `PacketTypeID`: `unsigned short`
* `PacketArgs`: *`any`* - Semicolon (`;`) separated parameters (unnamed, order matters)

## User Management

### `Login`
This packet type is sent to attempt to register the player into a match
* `PacketTypeID`: 1
* `PacketArgs`:
    * `Username | string` - Username of the player

### `LoginResponse`
This packet is sent back to the client after doing a login request using the `Login` packet
* `PacketTypeID`: 2
* `PacketArgs`:
    * **If Login Succeeded:**
        * `Success | bool (true)` - Whether the login succeeded or not. Always true in this case.
        * `Id | unsigned char` - Socket ID of the player in that match. Used for authentication during further packet sending.
    * **If Login Didn't Succeed:**
        * `Success | bool (false)` - Whether the login succeeded or not. Always false in this case.
        * `Error | string` - Human-readable login error

### `Location`
This packet is used to report location of a player to a server or for the server to report to other clients.
* `PacketTypeID`: 3
* `PacketArgs`:
    * `Id | unsigned char` - Socket ID of the player that location is reported for
    * `Position | string` - Comma-separated list of X, Y, and Z location coordinates
    * `Rotation | string` - Comma-separated list of X, Y, Z, and W (*FQuat*) rotation coordinates

### `Logout`
This packet is used to signal the server of a player leaving
* `PacketTypeID`: 4
* `PacketArgs`:
    * `Id | unsigned char` - Socket ID of the user logging out