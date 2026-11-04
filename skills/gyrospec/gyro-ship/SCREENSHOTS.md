# Screenshots for a pull request

Capture only what a reviewer needs to see the change, with the least setup, time, and tokens that get it done.

1. Pick the shots from the diff: each changed screen or component state once, in light and dark mode when the change touches theming or the app supports both, and at a phone width when layout changed. Keep the set small.
2. Serve the UI the cheapest way that works, without a production build and without the backend:
   - Reuse what exists first: a running dev server, Storybook or another component workbench, a dev or mock mode, fixtures, or seed data.
   - Otherwise start the frontend's own dev command in the background. Render a non-web framework through its web target where it has one, such as `flutter run -d web-server --web-port <port>` for Flutter.
   - When a screen needs data that nothing provides, hardcode minimal sample data at the nearest UI or data boundary, for the screenshot only. Never build a mock API, start a mock server, run the backend, install dependencies the project does not need, refactor app logic, or fix unrelated problems. When a screen still does not render after two attempts, skip it and say why.
3. Capture with the Playwright CLI through `npx`, without adding it to the project, for example `npx -y playwright screenshot --viewport-size "1280, 800" --color-scheme dark --wait-for-timeout 1000 <url> <out>.png`. When its browser is missing, `npx -y playwright install chromium` is the one install allowed. Save the images outside the repository.
4. Clean up before attaching anything: restore every screenshot-only edit with `git restore <file>`, stop each server you started, and confirm `git status --porcelain` shows exactly what it showed before step 2. Screenshots and their sample data never land on the PR branch.
5. `gh pr comment` cannot upload images, so publish them on `gyro-screenshots`, a separate orphan branch that holds only images and is never merged. Use a temporary worktree outside the repository so the PR branch stays untouched:

   ```bash
   git fetch origin gyro-screenshots
   git worktree add --detach <tmp>
   cd <tmp>
   git switch gyro-screenshots || git switch --orphan gyro-screenshots
   ```

   The fetch fails harmlessly the first time, before the branch exists. Copy the images into `<slug>/pr-<number>/` there, commit them by the commit rules in the gyro reference, run `git push origin gyro-screenshots`, then `git worktree remove <tmp>` from the repository root.
6. Post one comment with `gh pr comment <number> --body-file <file>`, the file written outside the repository. Give each image one caption line and the link `![<what it shows>](https://github.com/<owner>/<repo>/blob/gyro-screenshots/<slug>/pr-<number>/<name>.png?raw=true)`. These links render for everyone who can read the repository, private ones included.
