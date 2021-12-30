# UI API

The extension UI has access to an extension API, allowing:

## Common Functions

Listing running containers:

```typescript
window.ddClient.listContainers();
```

Listing images:

```typescript
window.ddClient.listImages();
```

Displaying an error in a red banner on the Dashboard:

```typescript
window.ddClient.toastError("Something went wrong");
```

## Running any docker command and getting results

```typescript
window.ddClient.backend
  .execDockerCmd("info", "--format", '"{{ json . }}"')
  .then((cmdResult) => console.log(cmdResult));
```

result will be of the form:

```json
{
  "stderr": "",
  "stdout": "{...}"
}
```

(In this example the docker command output is a json output)

For convenience, the command result object also has methods to easily parse it:

* `cmdResult.lines() : string[]` split output lines
* `cmdResult.parseJsonObject() : any` parse a well formed json output
* `cmdResult.parseJsonLines() : any[]` parse each output line as a json object

## Communication with the Extension Backend

Accessing a socket exposed by your extension VM service:

```typescript
window.ddClient.backend
  .get("/some/service")
  .then((value: any) => console.log(value));
```

Running a command in the container inside the VM:

```typescript
window.ddClient.backend
  .execInVMExtension(`cliShippedInTheVm xxx`)
  .then((cmdResult: any) => console.log(cmdResult));
```

Invoking an extension binary on your host:

```typescript
window.ddClient.execHostCmd(`cliShippedOnHost xxx`).then((cmdResult: any) => {
  console.log(cmdResult);
});
```