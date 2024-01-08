# TCP/UDP Yeetnite Server/Client Packet Specification

**Format:** `<PacketTypeID>;<PacketArgs>` (Simple, right?)

## Data Types:
* `PacketTypeID`: `uint8_t`
* `PacketArgs`: *`any`* - packed together as binary according to each parameter's data type size
    * NOTE: for data types with variable size (Ex: strings), a uint8_t is at the start of each parameter location indicating the length

## Packets

### `Login` | *tcp*
This packet type is sent to attempt to register the player into a match
* `PacketTypeID`: 0
* `PacketArgs`:
    * `Username | string` - Username of the player

### `LoginResponse` | *tcp*
This packet is sent back to the client after doing a login request using the `Login` packet
* `PacketTypeID`: 1
* `PacketArgs`:
    * **If Login Succeeded:**
        * `Success | bool (true)` - Whether the login succeeded or not. Always true in this case.
        * `Id | unsigned char` - Socket ID of the player. Used for authentication during further packet sending.
    * **If Login Didn't Succeed:**
        * `Success | bool (false)` - Whether the login succeeded or not. Always false in this case.
        * `Error | string` - Human-readable login error

### `Position` | *udp*
This packet is used to report location of a player to a server or for the server to report to other clients.
* `PacketTypeID`: 2
* `PacketArgs`:
    * `Id | unsigned char` - Socket ID of the player that location is reported for
    * `Location | string` - Comma-separated list of X, Y, and Z location coordinates
    * `Rotation | string` - Comma-separated list of X, Y, Z, and W (*FQuat*) rotation coordinates

### `Logout` | *tcp*
This packet is used to signal the server of a player leaving
* `PacketTypeID`: 3
* `PacketArgs`:
    * `Id | unsigned char` - Socket ID of the user

### `EquipItem` | *tcp*
This packet is used to signal a player changing their currently-equipped item
`PacketTypeID`: 4
* `PacketArgs`:
    * `Id | unsigned char` - Socket ID of the user
    * `SlotNum | char` - Slot number that is equipped (-1 if none equipped)

### `UseConsume` | *tcp*
This packet is used to signal a player using or consuming their currently-equipped item (weapon shoot, consumable use)
* `PacketTypeID`: 5
* `PacketArgs`:
    * `Id | unsigned char` - Socket ID of the user