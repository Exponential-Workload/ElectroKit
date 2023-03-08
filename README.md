# ElectroKit
A functional merge between [Electron](https://electronjs.org) [Forge](https://www.electronforge.io/) & [Svelte](https://svelte.dev)[Kit](https://kit.svelte.dev) & [TypeScript](https://typescriptlang.org) - Can easily be used as a base between Electron & almost any other framework.

This is so hacked together yet it somehow works. Don't ask how, or why. I don't know.

## Installation
***NOTICE: BUILD SCRIPTS HEAVILY DEPEND ON [PNPM](https://pnpm.io). HAVE IT INSTALLED. YOU WILL GET ERRORS IF IT'S NOT THERE.***

### 1. Dependencies

Thanks to build scripts that install dependencies in several directories for you, all you need to run is
```bash
$ pnpm i
```

### 2. Setting up a devserver

Surprisingly simple thanks to [concurrently](https://npm.im/concurrently) being an amazing utility (see package.json):
```bash
$ pnpm dev
```

### 3. Building a production build

```bash
$ pnpm build
```

*notice: this uses some hacky stuff to get sveltekit to work properly + prod builds DO bind to a [**random available port**](https://npm.im/random-port).*

> Outputs are in `electron/out/`
> 
> Outputs are only built for your current platform. Refer to [Electron Forge Documentation](https://www.electronforge.io/core-concepts/build-lifecycle#cross-platform-build-systems) for more information.

## Structure

`svelte/` contains a standard SvelteKit + Vite + TS Project. This is where your svelte app is built.

`electron/` contains a standard Electron Forge + Webpack + TS Project. This is where any nodejs code runs, and where electron fuckery happens.

`electron/app` is where your app is *finally* copied to right before electron build time.

`electron/renderer-app` is the temporary page shown for a split second during port searching & express static server creation. Users will rarely see it.

## Lifecycles

### Dev Lifecycle

1. We start the Vite development server
2. We start the Electron Forge Development stuff, passing an environment variable to tell the main process to load from port 5173 instead of prod behaviour.
3. Done!

### Build Lifecycle

1. We build the SvelteKit Application using Vite, SvelteKit & @sveltekit/adapter-static
2. We remove the existing electron src/app directory
3. We copy the output files from svelte/build to electron/src/app
4. We build the electron application using Electron Forge `make`
5. Done!

## Useful Resources

Here's a bunch: [Electron Documentation](https://www.electronjs.org/docs/latest/) | [Electron Forge Documentation](https://www.electronforge.io/) | [Learn Svelte](https://svelte.dev/tutorial/basics) (Svelte Beginners) | [Svelte API Refrence](https://svelte.dev/docs) (Svelte Advanced) | [SvelteKit Documentation](https://kit.svelte.dev/docs/introduction)

###### Footnotes

This can VERY easily be migrated to any framework, assuming `pnpm build` in the `svelte` directory results in a `build` output directory with static files, and `pnpm dev` there binds a devserver to port 5173.<br/>
That's all you need for this to work on any framework.
