# Script session 2: DemoKart live build

AI Residency, Goa. Day 2, Sat 21 Nov, 10:00 to 11:15, Main hall. Run by Deepak Kasera. Participants watch, then rebuild the same shop from 11:30 to 13:00.

**The prompts:** this script uses the first four prompts in [`prompts.md`](./prompts.md). Copy each one from there, word for word. Where this script says "Type: Prompt N", paste Prompt N from `prompts.md`.

## What the room will see

In 75 minutes, Deepak builds an online shop called DemoKart from an empty folder, using only prompts in Claude Code. By the end the shop has a polished product page with search, a category filter and a price sort, Add to cart and a cart page that remembers items, and its code on GitHub in a private repo.

There are no slides. DNS, HTTP, APIs and system design were covered on Day 1, so this session only builds. Concepts are named in one or two lines at the moment the build needs them. Putting the shop on the internet (Vercel) and databases are not part of this session.

## Running order

| Time | Step | Prompt | The room sees at the end |
|------|------|--------|--------------------------|
| 10:00 | 1. Show the goal | none | The finished DemoKart shop on a live link |
| 10:05 | 2. Empty folder to product page | Prompt 1: Setup | 15 products with real photos on localhost |
| 10:22 | 3. Filter and sort | Prompt 2: Filters | Category filter, price sort and search working together |
| 10:35 | 4. Cart | Prompt 3: Cart | Add to cart, a cart badge and a cart page with a total |
| 10:50 | 5. Git and GitHub | Prompt 4: GitHub | The code in a new private GitHub repo |
| 11:05 | 6. Recap and hand-off | none | The full path, and the plan for 11:30 |

## Before the session (Deepak)

- Node.js LTS installed: `node -v` shows 20 or higher.
- Claude Code signed in and started from the VS Code terminal.
- GitHub CLI installed and signed in: `gh auth status` shows "Logged in".
- An empty folder called `demokart-shop`: lowercase, with nothing inside, not even a `.vscode` folder.
- The finished shop already deployed under a different name (for example `demokart-final`), so the name `demokart-shop` is free on GitHub for the live build. The reference deploy is https://demokart-masai.vercel.app/. It is only shown in step 1.
- The `demokart-shop` repo does not exist yet on your GitHub account, or Prompt 4 will stop and ask what to do.
- A copy of the finished project with one commit per step, to jump ahead if a step stalls.
- VS Code font and browser zoom large enough for the back row.
- Two browser windows ready: a normal one signed in to GitHub, and a private (incognito) one.
- `prompts.md` open in a second window, so each prompt is one copy and paste.

## Before 11:30 (every participant laptop, Mohit checks)

Post these links in Slack at 10:00 so people can install in the background while they watch.

- Node.js LTS from nodejs.org. Check with `node -v`.
- GitHub CLI from cli.github.com. Then run `gh auth login` and choose GitHub.com, HTTPS, Login with a web browser. Check with `gh auth status`.
- Free GitHub account.
- Claude Code working in the VS Code terminal.
- The link to `prompts.md` in the repo, so everyone can copy the prompts.

## Step by step

### Step 1. Show the goal (10:00, 5 min)

**Say**
- This is DemoKart. Today we build its shop from nothing.
- By 11:15 we will have built the shop and put its code on GitHub. At 11:30 you build it yourselves.

**Show**
- Open the finished live link. Search for a product, filter by Electronics, sort by price, add two items, open the cart.
- Open the empty `demokart-shop` folder in VS Code and start Claude Code in the terminal.

### Step 2. Empty folder to product page (10:05, 17 min)

**Say**
- The folder is empty. Everything from here is written by Claude Code from my prompts.
- Claude Code asks before it runs a command. I will read each command out before I approve it.
- The prompt is long on purpose. A good prompt says what to build, what the data looks like, how it should look, and how to check it is done.

**Type: Prompt 1 (Setup)**

**The room should see**
Claude Code creates the project, installs packages, writes `data/products.js`, builds the components (Header, Hero, CategoryFilter, ProductCard, ProductGrid, Footer), runs the build, and starts the server. `localhost:3000` shows a polished DemoKart home page with 15 products, photos, prices, discount tags and an Add to cart button.

**Say while it runs**
- Next.js is a toolkit for building websites. It gives us pages and server code in one project.
- `node_modules` is code other people wrote that our project uses. It is large, and it never goes to GitHub.
- Open `data/products.js`: the products are a list inside a code file. Changing a price means editing code. Real shops keep this in a database, which is a topic for another session.
- Point at the product card. Every detail in the card (the badge, the struck-through price, the green "% off") came from one line in the prompt.

**If it breaks**
- Setup stops on a question: tell Claude Code "Use the non-interactive options and the default answers."
- "Folder not empty" or a project name error: the folder must be empty and its name lowercase.
- Port 3000 is busy: use the link the terminal prints, often `localhost:3001`.
- A photo does not load: the prompt already adds a placeholder fallback, so show it working. Or ask "Check the image links and use the placehold.co fallback."

### Step 3. Filter and sort (10:22, 13 min)

**Type: Prompt 2 (Filters)**

**The room should see**
Category pills and a sort dropdown on one row. Pick Electronics and only electronics show. Sort Low to high and prices go up. Type in the search box and the result count updates ("Showing 3 products"). All three work at the same time. A search with no match shows "No products found" and a Clear filters button.

**Say**
- Filter, sort and search run in the browser, on products already on the page. Nothing goes back to a server, which is why it is instant.
- The same list is filtered first, then sorted. That is why the controls combine instead of fighting each other.
- Ask Claude Code: "In 3 lines, explain how the filter and sort work." Read the answer out.

**Show (2 min)**
- Run the combinations Claude Code reports: Books + High to low, All + Low to high, then Clear filters.

### Step 4. Cart (10:35, 15 min)

**Type: Prompt 3 (Cart)**

**The room should see**
Add to cart changes to "Added ✓" for a moment, a toast appears at the bottom with a View cart link, and the header badge counts up. The `/cart` page lists each item with its image, quantity buttons, line total and Remove, plus a summary with the total in ₹ and a disabled Checkout button.

**Show (3 min)**
- Add 2 items, then reload the page: the cart is still there.
- Open `localhost:3000` in a private window: the cart there is empty.

**Say**
- The cart is saved in this one browser (localStorage). Another browser or another laptop cannot see it, and if the customer switches phones, it is gone.
- Only the product id and quantity are saved. Names and prices are looked up when the page shows, so a price change never leaves old prices in a saved cart.
- A real shop keeps the cart on its server. That is the next step up and we leave it for a later session.

**If it breaks**
- A hydration warning or a flash of "Your cart is empty": ask "Fix the hydration warning. The cart must load from localStorage only after the first render."
- The badge does not update: ask Claude Code to check that the app is wrapped in the CartContext provider in `app/layout.js`.

### Step 5. Git and GitHub (10:50, 15 min)

**Say**
- Git saves versions of your code on your laptop. Each saved version is a commit, with a message that says what changed.
- GitHub keeps a copy of those versions online. Teams work from it, and later we can publish the shop from it.
- `.gitignore` lists what must never be saved: `node_modules`, and `.env` files that hold keys.

**Type: Prompt 4 (GitHub)**

**The room should see**
Claude Code explains Git, a commit and GitHub in 3 lines, checks `.gitignore`, makes a commit, creates a private repo with `gh`, pushes, and verifies it. Open the repo link: the files and the commit history are on GitHub. The reply ends with the "Pushed successfully" block and the repo link.

**Show**
- Open the repo in the browser: the files, the commit message and the commit history.
- Point out that `node_modules` and `.env` files are not in the list.

**Say**
- The prompt tells Claude Code what to do if something goes wrong: stop and ask, never force push. Good prompts include the rules for failure.
- The repo is private, so the link only opens for someone logged in to my GitHub account.

**If it breaks**
- "Not logged in" from `gh`: run `gh auth login` in the terminal, sign in through the browser, then ask Claude Code to try again.
- The repo name is taken: ask for `demokart-shop-2`.
- git user name or email is not set: Claude Code stops by design. Set them with `git config --global user.name` and `git config --global user.email`, then continue.

### Step 6. Recap and hand-off (11:05, 10 min)

**Draw on the whiteboard**
Prompt, then Claude Code writes the code on the laptop, then Git saves versions, then GitHub keeps the code online.

**Say**
- What we did today: from an empty folder to a finished shop with its code on GitHub, in four prompts.
- The cart lives in one browser for now. Putting the shop live and moving products and carts into a database are the next steps, and we leave them for later.
- At 11:30 you build the same shop from an empty folder with the same 4 prompts. They are in `prompts.md`, linked in Slack.
- Check now: `node -v` and `gh auth status`. If either fails, find Mohit during the break.

### If running late, cut in this order

1. The private-window cart demo in step 4.
2. The "explain how the filter works" question in step 3.
3. The GitHub repo walkthrough in step 5.
4. Shorten the recap to 2 minutes.

## 11:30 to 13:00: participant build

Everyone builds the same DemoKart shop from an empty folder, using the four prompts in `prompts.md`.

1. Create an empty folder called `demokart-shop`, open it in VS Code and start Claude Code.
2. Prompt 1: Setup. The product page with 15 products.
3. Prompt 2: Filters. Category filter, price sort and search.
4. Prompt 3: Cart. Add to cart and the cart page, saved in the browser.
5. Prompt 4: GitHub. Commit and push to a new private repo with the GitHub CLI.
6. Post the repo link in Slack.

**Done when:** the shop runs on localhost, Add to cart works and the cart survives a reload, the code is on GitHub in a private repo, and the link is in Slack.

**Finished early:** add one feature, then commit and push it to GitHub:
- A product detail page that opens when you click a card.
- Sort by rating.
- A Checkout button that shows an order summary, with no payment.
- A wishlist heart on each product card.
