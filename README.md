# React Glassmorphism Profile Card

A single-file React 18 application that renders a premium glassmorphism-styled student profile card. The entire app lives in one `index.html` file loaded entirely via CDN scripts, so there are no build tools, no npm install, and no package.json. Just open the file in a browser and it works.

The card displays a student profile (name, role, bio, skills, avatar, location, social links) with a dark/light mode toggle and a like button that triggers a heart animation. The glassmorphism design uses semi-transparent backgrounds, backdrop blur, rounded corners, and layered gradients for a frosted-glass aesthetic.

## Features

- **Glassmorphism design** — semi-transparent card with `backdrop-blur`, rounded corners, and gradient backgrounds visible through the glass surface
- **Dark/Light mode toggle** — switch between dark and light themes instantly without reloading the page
- **Like button with heart animation** — click the heart to increment the count and trigger a scale animation
- **Responsive layout** — card adapts from mobile (320px) to desktop without horizontal overflow
- **Accessible markup** — semantic HTML (`h1`, `h2`, `p`), `alt` text on the avatar, and `aria-label` on all interactive elements
- **Dynamic skills rendering** — skills are rendered from an array using `map()`, so the card adapts to any skill set
- **Dynamic social links rendering** — social links are rendered from an array using `map()`, so the card adapts to any set of links

## Technologies Used

| Technology | Purpose | Source |
|---|---|---|
| React 18 | Core React library (hooks, components) | CDN (`unpkg.com`) |
| ReactDOM 18 | DOM renderer | CDN (`unpkg.com`) |
| Tailwind CSS | Utility-first CSS framework | CDN (`cdn.tailwindcss.com`) |
| Babel Standalone | Transforms JSX into browser-compatible JavaScript at runtime | CDN (`unpkg.com`) |

No build tools or package managers are used. All libraries are loaded via CDN `<script>` tags inside `index.html`.

## React Concepts

This project demonstrates several core React concepts. Each one is explained below in beginner-friendly terms.

### Functional Components

A **functional component** is a plain JavaScript function that returns JSX, which describes what the UI should look like. In this project, `ProfileCard` and `App` are both functional components.

```jsx
function ProfileCard({ name, role, bio, skills, avatar, location, socialLinks }) {
  return (
    <div>
      <h1>{name}</h1>
      <p>{bio}</p>
    </div>
  );
}
```

Unlike class components, functional components have no `this` keyword, are simpler to write, and can use React Hooks (like `useState`) to manage state.

### Props

**Props** (short for "properties") are how a parent component passes data to a child component. They are read-only inside the receiving component.

In this project, the `App` component defines a `profileData` object and passes every field to `ProfileCard` as individual props using the spread operator:

```jsx
function App() {
  const profileData = {
    name: "Omkar Jagtap",
    role: "Blockchain Technology Student & Web Developer",
    bio: "...",
    skills: ["React", "JavaScript", "Blockchain Technology"],
    avatar: "https://via.placeholder.com/150",
    location: "Pune, Maharashtra, India",
    socialLinks: [...]
  };

  return <ProfileCard {...profileData} />;
}
```

The spread operator `{...profileData}` is shorthand for writing out each prop individually: `name={profileData.name}`, `role={profileData.role}`, and so on.

### State

**State** is data that a component holds and can change over time. When state changes, React automatically re-renders the component so the UI reflects the new value. In this project, state powers the dark/light toggle and the like button count.

### The `useState` Hook

**`useState`** is a React Hook that lets a functional component create and update local state. It returns an array with two elements:

1. The current state value
2. A setter function that updates that value and triggers a re-render

```jsx
const [isDark, setIsDark] = useState(false);
//  isDark     setIsDark        initial value
```

- `isDark` holds the current theme (`false` = light, `true` = dark).
- `setIsDark` is the function you call to change `isDark`.
- `false` is the initial value.

Each `useState` call creates one independent piece of state. This project uses three:

| State variable | Type | Initial value | Updater | Purpose |
|---|---|---|---|---|
| `isDark` | boolean | `false` | `setIsDark` | Tracks the current theme |
| `likes` | integer | `0` | `setLikes` | Tracks the like count |
| `isLiked` | boolean | `false` | `setIsLiked` | Triggers the heart animation |

### `map()` for List Rendering

The **`map()`** method iterates over an array and returns a new array of JSX elements, one per item. This is how React renders lists dynamically instead of hardcoding each element.

```jsx
{(skills || []).map((skill, index) => (
  <span key={skill || index} className="badge">
    {skill}
  </span>
))}
```

- `map()` runs once per skill in the array, producing one `<span>` badge per item.
- The `|| []` fallback ensures that if `skills` is missing or undefined, an empty array is used instead, preventing a runtime error.
- The `key` prop gives each element a stable identity so React can efficiently update the list on re-renders.

The same pattern is used for social links: each link in the `socialLinks` array becomes a clickable anchor element.

### Dark Mode Toggle Using State

The dark/light toggle works by flipping a boolean state variable. When the user clicks the toggle button, an event handler calls the state updater with the opposite value:

```jsx
const toggleDarkMode = () => {
  setIsDark(!isDark);
};
```

Because `isDark` is React state, updating it with `setIsDark` triggers a re-render. During that re-render, every conditional class expression that reads `isDark` re-evaluates, swapping the gradient background, card opacity, and text colors to the opposite theme. The whole switch happens without reloading the page.

### Like Button with State-Driven Animation

The like button uses two pieces of state together:

- **`likes`** (integer) — increments by 1 on each click and is displayed as a number next to the heart icon.
- **`isLiked`** (boolean) — set to `true` on click, which adds the `animate-heart-pop` CSS class to the heart icon, triggering a scale animation. After 600ms (matching the animation duration), it resets to `false` so the animation can be re-triggered on the next click.

```jsx
const handleLike = () => {
  setLikes(likes + 1);
  setIsLiked(true);
  setTimeout(() => setIsLiked(false), 600);
};
```

```jsx
<svg className={`w-6 h-6 ${isLiked ? "animate-heart-pop" : ""}`} />
<span>{likes}</span>
```

This pattern demonstrates how state drives both the displayed data (the count) and the visual effect (the animation) without directly manipulating the DOM.

## Getting Started

### Prerequisites

A modern web browser (Chrome, Firefox, Edge, or Safari). No other tools are needed.

### Running the Project Locally

1. Download or clone the repository:

   ```bash
   git clone https://github.com/omkarjagtap2105-design/apnoai-task-2-omkar.git
   ```

2. Open `index.html` directly in your browser.

   - **Option A:** Double-click the `index.html` file.
   - **Option B:** Right-click `index.html` and select "Open with" → your browser of choice.

That's it. The app loads React, ReactDOM, Tailwind CSS, and Babel from CDN, so no build step or `npm install` is required. An internet connection is needed on first load so the CDN scripts can download.

## Project Structure

```
apnoai-task-2-omkar/
├── index.html    # Entire application: HTML structure, Tailwind config, React components
├── README.md     # Project documentation (this file)
└── .gitignore    # Ignores OS and editor files
```

All application code (HTML, CSS via Tailwind, and React components) lives in the single `index.html` file. There are no separate JavaScript or CSS files.

## Props Explanation

Props are the mechanism React uses to pass data from a parent component to a child component. The parent sends data down, and the child receives it as read-only inputs.

In this project:

- **`App`** (parent) holds the `profileData` object and passes it to `ProfileCard` using the spread operator: `<ProfileCard {...profileData} />`.
- **`ProfileCard`** (child) receives each field as a named prop through destructuring in the function parameter: `function ProfileCard({ name, role, bio, skills, avatar, location, socialLinks })`.

| Prop | Type | Description |
|---|---|---|
| `name` | string | Displayed as the card heading (`h1`) |
| `role` | string | Displayed as the subheading (`h2`) |
| `bio` | string | Displayed as a paragraph (`p`) |
| `skills` | string[] | Array of skill labels rendered as badges via `map()` |
| `avatar` | string | Image URL for the avatar |
| `location` | string | Displayed as a text label with a location pin icon |
| `socialLinks` | object[] | Array of social link objects (`label`, `url`, `icon`) rendered as anchors via `map()` |

Props are **read-only** inside `ProfileCard`. If the component needs to change data over time (like the theme or like count), it uses **state**, not props.

## State and useState Explanation

**State** is data that a component owns and can update over time. When state changes, React re-renders the component so the UI reflects the new value. This is what makes the app interactive.

**`useState`** is the React Hook that creates a piece of state in a functional component. Each call returns a pair: the current value and a setter function.

```jsx
const [isDark, setIsDark] = useState(false);
```

- **`isDark`** — the current state value (starts at `false`, meaning light mode).
- **`setIsDark`** — the updater function. Calling `setIsDark(true)` changes `isDark` to `true` and triggers a re-render.
- **`useState(false)`** — the argument (`false`) is the initial value.

Key points:

- Calling a setter (e.g., `setLikes(5)`) updates the state and re-renders the component automatically.
- You never modify state directly (e.g., `isDark = true` does not work). You always use the setter.
- Each `useState` call creates an independent piece of state. A component can have multiple `useState` calls for different values.

This project uses three `useState` calls inside `ProfileCard`:

```jsx
const [isDark, setIsDark] = useState(false);   // theme toggle
const [likes, setLikes] = useState(0);          // like count
const [isLiked, setIsLiked] = useState(false);  // animation trigger
```

- The **dark mode toggle** calls `setIsDark(!isDark)` to flip the theme.
- The **like button** calls `setLikes(likes + 1)` to increment the count and `setIsLiked(true)` to trigger the heart animation.

## About the Developer

| Field | Details |
|---|---|
| **Name** | Omkar Jagtap |
| **Role** | Blockchain Technology Student & Web Developer |
| **Course** | B.Sc. Blockchain Technology |
| **College** | Savitribai Phule Pune University (SPPU) |
| **Location** | Pune, Maharashtra, India |

## Deployment

This project is a static single-page HTML application. No build step, no server, and no package manager are required. Below are two ways to deploy it online.

### Deploy to GitHub

1. Create a new repository on GitHub named `apnoai-task-2-omkar` at [https://github.com/omkarjagtap2105-design/apnoai-task-2-omkar](https://github.com/omkarjagtap2105-design/apnoai-task-2-omkar).

2. Initialize the local repository and push your files:

   ```bash
   git init
   git add index.html README.md .gitignore
   git commit -m "feat: build React profile card with props and state"
   git branch -M main
   git remote add origin https://github.com/omkarjagtap2105-design/apnoai-task-2-omkar.git
   git push -u origin main
   ```

3. Your `index.html` and `README.md` are now live on GitHub. Anyone can clone or download the repository and open `index.html` in a browser.

### Deploy to Vercel

Since this is a static single-page HTML project, Vercel can serve it directly with no build configuration.

1. Go to [vercel.com](https://vercel.com) and sign in with your GitHub account.
2. Click **Add New** → **Project**.
3. Import the GitHub repository `apnoai-task-2-omkar`.
4. Configure the project settings:
   - **Framework Preset:** Other
   - **Root Directory:** `./`
   - **Build Command:** *(leave blank)*
   - **Output Directory:** *(leave blank)*
   - **Install Command:** *(leave blank)*
5. Click **Deploy**.

Vercel will serve `index.html` as a static file. The deployment URL is available immediately after the import completes.

## Debugging Exercise

A common mistake when learning React hooks is misspelling the hook name. React hooks are **case-sensitive**. Let's look at what happens when `useState` is written in lowercase as `usestate`.

### Incorrect Code

```jsx
function ProfileCard({ name, role, bio, skills, avatar, location, socialLinks }) {
  // ❌ Wrong: 'usestate' is not a valid React hook
  const [isDark, setIsDark] = usestate(false);
  const [likes, setLikes] = usestate(0);
  const [isLiked, setIsLiked] = usestate(false);

  return (
    <div>
      <h1>{name}</h1>
      <p>{likes}</p>
    </div>
  );
}
```

### What Goes Wrong

React hooks are JavaScript functions exported by the React library, and JavaScript is case-sensitive. The correct hook name is `useState` (camelCase with a capital `S`). When you write `usestate` (all lowercase), JavaScript looks for a variable or function named `usestate` — which does not exist. This throws a **ReferenceError**:

```
ReferenceError: usestate is not defined
```

The app will fail to render, and the error will appear in the browser console. The dark mode toggle, like button, and heart animation will all stop working because none of the state variables can be initialized.

### Corrected Code

```jsx
function ProfileCard({ name, role, bio, skills, avatar, location, socialLinks }) {
  // ✅ Correct: 'useState' is the proper React hook name (capital S)
  const [isDark, setIsDark] = useState(false);
  const [likes, setLikes] = useState(0);
  const [isLiked, setIsLiked] = useState(false);

  return (
    <div>
      <h1>{name}</h1>
      <p>{likes}</p>
    </div>
  );
}
```

### The Fix

By changing `usestate` to `useState`, the hook is recognized as the function exported by React. The state variables (`isDark`, `likes`, `isLiked`) initialize correctly, the setters (`setIsDark`, `setLikes`, `setIsLiked`) work as expected, and the component renders properly. The dark mode toggle, like counter, and heart animation all function again.

**Key takeaway:** React hooks follow camelCase naming. `useState`, `useEffect`, `useRef` — always capitalize the first letter after `use`. JavaScript treats `usestate` and `useState` as two different identifiers.

## ApnoAI Submission Checklist

Use this checklist to confirm all required deliverables are complete before submitting.

- [ ] Starter HTML used or authorized equivalent created
- [ ] Profile information customized
- [ ] Personal photo added or replacement pending
- [ ] Props implemented
- [ ] useState implemented
- [ ] Dark/light mode working
- [ ] Like counter working
- [ ] Debugging exercise understood
- [ ] README completed
- [ ] GitHub repository created
- [ ] Code pushed to GitHub
- [ ] Vercel deployment verified
- [ ] Mobile testing completed
- [ ] ApnoAI submission completed

## AI Usage Disclosure

I used Kiro AI and ChatGPT to understand React components, props, state, and useState. I used AI assistance to understand and debug React errors, improve the UI, and test the interactive features. I reviewed the code and customized the profile card with my own information.
