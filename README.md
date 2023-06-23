# ProjectWork
![](https://github.com/GioanZ/project-work/blob/main/mvp_sample.gif)

Install packages:
https://mops.one/docs/install

```bash
mops install
npm install
```

Run and stop:

```bash
dfx start --background
dfx deploy
dfx stop
```

Welcome to your new ProjectWork project and to the internet computer development community. By default, creating a new project adds this README and some template files to your project directory. You can edit these template files to customize your project and to include your own code to speed up the development cycle.

To get started, you might want to explore the project directory structure and the default configuration file. Working with this project in your development environment will not affect any production deployment or identity tokens.

To learn more before you start working with ProjectWork, see the following documentation available online:

- [Quick Start](https://internetcomputer.org/docs/current/developer-docs/quickstart/hello10mins)
- [SDK Developer Tools](https://internetcomputer.org/docs/current/developer-docs/build/install-upgrade-remove)
- [Motoko Programming Language Guide](https://internetcomputer.org/docs/current/developer-docs/build/cdks/motoko-dfinity/motoko/)
- [Motoko Language Quick Reference](https://internetcomputer.org/docs/current/references/motoko-ref/)
- [JavaScript API Reference](https://erxue-5aaaa-aaaab-qaagq-cai.raw.icp0.io)

If, on the other hand, you want to learn more about how HTTP calls work for motoko, check out @krpeacock's mo:Server library:

- [mo:Server](https://github.com/krpeacock/server)

If you want to start working on your project right away, you might want to try the following commands:

```bash
cd project-work/
dfx help
dfx canister --help
```

## Running the project locally

If you want to test your project locally, you can use the following commands:

```bash
# Starts the replica, running in the background
dfx start --background

# Deploys your canisters to the replica and generates your candid interface
dfx deploy
```

Once the job completes, your application will be available at `http://localhost:4943?canisterId={asset_canister_id}`.

If you have made changes to your backend canister, you can generate a new candid interface with

```bash
npm run generate
```

at any time. This is recommended before starting the frontend development server, and will be run automatically any time you run `dfx deploy`.

If you are making frontend changes, you can start a development server with

```bash
npm start
```

Which will start a server at `http://localhost:8080`, proxying API requests to the replica at port 4943.

The API for adding a row to the database is a POST call. Here's an example of a call through Postman.

```curl
curl --location 'http://127.0.0.1:4943/add-row?canisterId={asset_canister_id}' \
--header 'Content-Type: application/json' \
--data '{
    "id": 23,
    "companyName": "Company_X",
    "cityDestination": "City_A",
    "supplier": "Company_Y",
    "cityOrigin": "City_B",
    "productType": "leather",
    "quantity": 10
}'
```
There are two ways to get the database, one with the GET API and another with the X method. The following is an example for each of them.

The GET returns the entire DB (following the order of insertion, with relative insertion number):
```curl
curl --location 'http://127.0.0.1:4943/get-row-db?canisterId={asset_canister_id}'
```

Output /get-row-db example in JSON:
```json
{
  "0": {
    "id": 23,
    "companyName": "Company_X",
    "cityDestination": "City_A",
    "supplier": "Company_Y",
    "cityOrigin": "City_B",
    "productType": "leather",
    "quantity": 10
  },
  "1": {
    "id": 7,
    "companyName": "Company_Z",
    "cityDestination": "City_C",
    "supplier": "Company_W",
    "cityOrigin": "City_D",
    "productType": "leather",
    "quantity": 100
  }
}
```

On ICP, in the backend part, you can see getDB() to get the DB as an array.

Output getDB() example in JSON:
```json
[{"id":"23","qty":"10","cityDestination":"City_A","supplier":"Company_Y","productType":"leather","companyName":"Company_X","cityOrigin":"City_B"},{"id":"7","qty":"100","cityDestination":"City_C","supplier":"Company_W","productType":"leather","companyName":"Company_Z","cityOrigin":"City_D"}]
```

### Note on frontend environment variables

If you are hosting frontend code somewhere without using DFX, you may need to make one of the following adjustments to ensure your project does not fetch the root key in production:

- set`DFX_NETWORK` to `ic` if you are using Webpack
- use your own preferred method to replace `process.env.DFX_NETWORK` in the autogenerated declarations
  - Setting `canisters -> {asset_canister_id} -> declarations -> env_override to a string` in `dfx.json` will replace `process.env.DFX_NETWORK` with the string in the autogenerated declarations
- Write your own `createActor` constructor
