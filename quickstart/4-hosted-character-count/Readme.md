# Character Counting
This example builds upon 2. Hosted Services and 3. Browser to Service in order to make a character counting method that is exectued on a remote peer and delivers the character counted message to both the sender and the targeted peer.
As you're now on part 4 of a series, it's assumed you have access to parts 2 and 3, if you have not already completed them.

## Directory Structure
### service_creation
Service Creation is taken from 2. Hosted Services and uses Rust to build the service artifact *character_count.wasm* and post it to the Fluence dashboard (in my case, this was https://dash.fluence.dev/blueprint/b734dcc489485189692203d32b82c569935bce2b51fd2a978f37239ce79489f0).
#### configuration changes
Due to the fact that we're making a different set of functionality than the hello app in 2. Hosted Services, all configuration files are updated in configs to refer to `character_count` vice `hello`.
build.sh also has been updated to refer to the `character_count.wasm` artifact.
#### aqua changes carried forward
As the new service has been created, don't forget to use `marine aqua artifacts/character_count.wasm` in order to generate the aqua content that will be used in `browser_to_service`.
#### publishing
`fldist` is again used to publish to a selected node. Make sure that you note which node this is so that you can make use of it in the browser_to_service step. The artifact and the name should be updated to refer to your character_count configuration and service.

Example used in development:
```
fldist --node-id 12D3KooWFEwNWcHqi9rtsmDhsYcDbRUCDXH84RC4FW6UfsFWaoHi \
       new_service \
       --ms artifacts/character_count.wasm:configs/character_count_cfg.json \
       --name character_count
```

Published services can be seen on [Fluence Developer Hub](https://dash.fluence.dev/); I found it easiest to find the service by searching for the service ID returned, as seen in response to your `fldist` call's success
```
service id: e7d88de8-ff47-4316-9b67-ad15565c16c2
service created successfully
```

### browser_to_service
Browser to Service is based upon 3. Browser to Service in the quickstart guide. As per the original, `npm install` is required, but before staring the application, many changes have to be made in the code.
1. `aqua/character_count.aqua` should be created, replacing the `aqua/getting_started.aqua` (though file name is not crucial so long as it's correctly imported after the TS code is generated).
2. The TS code must be generated using `npm run compile-aqua`
3. References to Hello in App.tsx have to be reviewed to make sure that CharacterCount references are made instead.

At this point, `npm start` should run, and you will be able to interact with the application to count your message's characters, and see the result returned to both the sender and receiver peers after the wordcount is calculated on the remote peer's machine.
