# Arbitrary passthrough arguments should not change task hash of dependencies

Expected: For a `test => build` task graph, passthrough arguments passed to `test` should not affect the hash of the `build` task as these arguments are not passed to `build`.

Actual: any arguments alter the global task hash

## Reproduction

1. Clone this repository

   ```sh
   git clone https://github.com/maxpatiiuk/turbo-passthrough-repro
   cd turbo-passthrough-repro
   ```

2. Install turbo

   ```sh
   npm install
   ```

3. Run the test command once:

   ```sh
   npx turbo test -- --one
   ```

   `build` has cache miss as there is no cache yet (correct).

4. Run the test command again with a different argument:

   ```sh
   npx turbo test -- --two
   ```

   The `build` task has a cache miss (not expected) even though it didn't receive any arguments (expected):

   ```log
   build: cache miss, executing 38fce17028c43c18  👈
   build:
   build: > build
   build: > node -e 'console.log("[build] Received args:", JSON.stringify(process.argv.slice(1)))' --
   build:
   build: [build] Received args: []  👈
   ```
