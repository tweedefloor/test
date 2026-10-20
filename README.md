
# Use simple `paths` input only when the tree is small
Raw `paths` is the low-ceremony path for demos, tests, and small static trees.
It is not the recommended setup for repo-scale or workspace-scale trees.

# Move to prepared input before the tree gets expensive
---
Use this recommended scale path:
1. Load canonical paths outside the UI.
2. Prepare the tree input one time.
3. Construct `new FileTree({ preparedInput, ... })`.

Use `preparePresortedFileTreeInput(...)` when the server or indexer already
knows the final order. It is the better fit because the client can skip the
extra sorting work.

# Add SSR later when server rendering matters
Hydration builds on top of the same class-first runtime. The client still
creates `new FileTree(...)`. But the client does not render fresh markup.
Instead, it attaches that model to server-rendered tree output with
`hydrate({ fileTreeContainer })`.

Continue with [SSR](#ssr) and [SSR API](#ssr-api) when you need that flow.

#### Advanced note: wrapping the vanilla model in another framework

Keep the same ownership boundary when you wrap the vanilla model in another
framework:

- create and own the `FileTree` instance from that framework's lifecycle
- mount or unmount around the instance
- keep all tree reads and writes on the model

This approach keeps React as the only first-class wrapper surface. It does not
change how the model works.