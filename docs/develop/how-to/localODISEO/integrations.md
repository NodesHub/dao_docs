# Integrations

You can integrate LocalODISEO in ODISEO Station, `achillesd`, and JavaScript and Python SDKs.

## ODISEO Station

ODISEO Station has built-in support for LocalODISEO for quick and easy interaction. [Open Station](https://station.ODISEO.money/) and switch to the `LocalODISEO` network.

## achillesd

1. Ensure the same version of `achillesd` and LocalODISEO are installed.

2. Use `achillesd` to talk to your LocalODISEO `achillesd` node:

    ```sh
    $ achillesd status
    ```

    This command automatically works because `achillesd` connects to `localhost:26657` by default.

    The following command is the explicit form:
    ```sh
    $ achillesd status --node=tcp://localhost:26657
    ```

3. Run any `achillesd` commands against your LocalODISEO network:

   ```sh
   $ achillesd query account ODISEO1dcegyrekltswvyy0xy69ydgxn9x8x32zdtapd8
   ```

## ODISEO Python SDK

Connect to the chain through LocalODISEO's LCD server:

```python
from ODISEO_sdk.client.lcd import LCDClient
ODISEO = LCDClient("localODISEO", "http://localhost:1317")
```

## ODISEO JavaScript SDK

Connect to the chain through LocalODISEO's LCD server:

```ts
import { LCDClient } from "@ODISEOmoney/ODISEO.js";

const ODISEO = new LCDClient({
  URL: "http://localhost:1317",
  chainID: "localODISEO",
});
```
