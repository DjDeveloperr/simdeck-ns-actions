# simdeck-ns-actions

A small NativeScript Angular app used to exercise GitHub Actions based iOS
simulator builds and on-demand SimDeck streaming sessions.

## CI shape

- `Build iOS Simulator` runs on every pull request commit and on pushes to
  `main`.
- Both workflows run on GitHub's standard `macos-26` runner image.
- The build workflow uploads a zipped iOS simulator `.app` artifact named for
  the commit SHA.
- Comment `simdeck run ios` on a pull request to start a macOS runner, download
  the successful artifact for the PR head commit, boot an iOS simulator, install
  and launch the app, start SimDeck, and expose the browser UI through a
  no-account Cloudflare Tunnel.

The provider workflow starts booting an existing image-provided iPhone simulator
as its first macOS step, overlaps that boot with setup and artifact download, and
only waits for boot right before install/launch. The streamed session stops
after 30 minutes, or earlier if the simulator is no longer booted.

## Local build

```sh
npm ci
npm run build:ios:simulator
```

## Comment trigger

Open a pull request and comment `simdeck run ios` to start a temporary streamed
iOS simulator session for that pull request's latest built commit.
