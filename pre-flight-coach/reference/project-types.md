# Project Types

You do not ask the user which platform they are building for. You infer it from their answer to "what will they use it on" in the opening prompt (see `rules.md`). The parenthetical in that prompt (their phone, a web browser, or their computer) lets a new developer who lacks the words "mobile / web / desktop" still tell you. Once you know, you pull the matching section below for the rest of the conversation.

If the user did not answer the "what will they use it on" part, or it is genuinely ambiguous, ask one short clarifying question: "Will people open this in a web browser, install it on their phone, or run it on a computer?" If it fits none of these, use general scaffolding and do not force a category.

Project type never changes the four questions. It changes the examples and prompts you reach for inside them.

## Mobile App

- **CHECK examples:** "On your actual phone, walk the whole flow from the first screen to the end." Real-device verification, not a simulator alone.
- **CONTEXT prompts:** "Are you targeting iPhone, Android, or both? Any app store rules you already know you have to follow?"
- **OUT OF SCOPE common buckets:** Animations between screens, push notifications, app store metadata and screenshots, dark mode.
- **Tooling pointer** (for Path 1 and 2 who need it; Path 3 will likely skip): Expo with React Native is a common starting point, and it lets you preview on your real phone over a QR code. One pointer plus the official docs, never a tutorial.

## Web App

- **CHECK examples:** "In your browser, open it and walk the flow. Open the developer tools console and confirm there are no red errors."
- **CONTEXT prompts:** "Which browsers do your users use? Where will this live, your computer only or hosted somewhere? Is anyone logging in?"
- **OUT OF SCOPE common buckets:** Search engine optimization, analytics, looking right on every screen size, login and accounts.
- **Tooling pointer:** Next.js or Vite is a common starting point, and `npm run dev` gives you a local address you open in your browser. One pointer plus the official docs.

## Desktop App

- **CHECK examples:** "After you install it on your own machine, open it and walk the flow the way a user would, not from your code editor."
- **CONTEXT prompts:** "Which operating system are you on, and which do your users use? Does this need an installer, or do they run a file?"
- **OUT OF SCOPE common buckets:** Auto-update, code signing, supporting more than one operating system, file associations.
- **Tooling pointer:** Electron or Tauri is a common starting point for a desktop app you can install and run locally. One pointer plus the official docs.
