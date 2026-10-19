#  Movie Browser: A Guided React Project for Beginners

Build your first multi-page React website, one small step at a time.

![React](https://img.shields.io/badge/React-18+-61DAFB?logo=react&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-build%20tool-646CFF?logo=vite&logoColor=white)
![Level](https://img.shields.io/badge/level-beginner-green)
![Time](https://img.shields.io/badge/time-about%201%20week-blue)

---

##  Project Description

**Movie Browser** is a small website where visitors can:

- See a grid of movies
- Search for a movie by name
- Click a movie to open its own detail page
- Add movies to a **Favorites** list

All movie data lives in **one file inside your project**. There is no internet connection, no API, and no account or key to set up. This lets you focus fully on learning React.

---

##  Purpose of This Project

This project is where you put everything you have learned about React into practice. Instead of doing small separate exercises, you will build **one real application** from start to finish.

By the end you will be able to:

| Skill | Where you practice it |
|---|---|
| Build and reuse **components** | Steps 2, 3 |
| Pass data with **props** | Steps 2, 3, 7 |
| Render lists with `map()` and **keys** | Step 3 |
| Create **pages** and navigate with React Router | Step 4 |
| Use **layouts** and `Outlet` | Step 4 |
| Read **URL parameters** with `useParams` | Step 5 |
| Manage data with **state** (`useState`) | Steps 6, 7 |
| Handle **events** and controlled inputs | Step 6 |
| **Lift state up** and share it between pages | Step 7 |
| **Deploy** a website online | Step 9 |

>  **Why a project?** Reading about React is not enough. You learn it by building, getting stuck, and fixing things. Getting stuck is part of the process.

---

##  Before You Start

You should know:

- Basic HTML and CSS
- Basic JavaScript: variables, functions, arrow functions, arrays, objects, and the array methods `map`, `filter`, and `find`
- The ideas of components, props, and state (from your lessons)

You need installed:

- [Node.js](https://nodejs.org) (LTS version)
- A code editor such as [VS Code](https://code.visualstudio.com)
- [Git](https://git-scm.com) and a [GitHub](https://github.com) account

---

##  How to Use This Guide

1. **Do the steps in order.** Each step builds on the last one.
2. **Read the goal first**, then try it yourself, then use the code given.
3. **Type the code yourself.** Do not copy and paste. Typing helps you remember.
4. **Check the "Done when" box** at the end of every step before moving on.
5. **Commit to Git after every step.**

```bash
git add .
git commit -m "Finish step 3"
```

6. When stuck, look at the ** Stuck?** tips and the [Troubleshooting](#-troubleshooting) section. Then ask a classmate or your teacher.

---

##  Final Project Structure

This is what your project will look like at the end:

```
movie-browser/
├── public/
│   └── _redirects            (only needed for Netlify, Step 9)
├── src/
│   ├── components/
│   │   ├── Layout.jsx
│   │   └── MovieCard.jsx
│   ├── data/
│   │   └── movies.js
│   ├── pages/
│   │   ├── Home.jsx
│   │   ├── Favorites.jsx
│   │   ├── MovieDetail.jsx
│   │   └── NotFound.jsx
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
└── package.json
```

---

##  Roadmap

- [ ] **Step 0:** Setup
- [ ] **Step 1:** Create the data
- [ ] **Step 2:** One movie card (components and props)
- [ ] **Step 3:** Show all movies (lists and keys)
- [ ] **Step 4:** Add pages (routing)
- [ ] **Step 5:** Movie detail page (URL parameters)
- [ ] **Step 6:** Search (state and events)
- [ ] **Step 7:** Favorites (lifting state up)
- [ ] **Step 8:** Finishing touches
- [ ] **Step 9:** Put it online

---

## Step 0: Setup

** Goal:** Create a working React project.

```bash
npm create vite@latest movie-browser -- --template react
cd movie-browser
npm install
npm install react-router-dom
npm run dev
```

Open the link shown in the terminal (usually `http://localhost:5173`).

Now clean up the starter files:

1. Replace everything in `src/App.jsx` with:

```jsx
export default function App() {
  return <h1>Movie Browser</h1>;
}
```

2. Delete everything inside `src/App.css` (or delete the file and remove its import).
3. Delete everything inside `src/index.css`. You will add your own styles later.

** Done when:** you see "Movie Browser" on the page.

---

## Step 1: Create the Data

** Goal:** Make a list of movies that your app will use. This is your "database".

Create the file `src/data/movies.js`:

```js
export const movies = [
  { id: 1, title: "Space Voyage", year: 2019, genre: "Sci-Fi", rating: 8.2, emoji: "🚀", description: "A crew travels to a distant planet to save humanity." },
  { id: 2, title: "The Lost Island", year: 2016, genre: "Adventure", rating: 7.4, emoji: "🏝️", description: "Friends crash on an island full of secrets." },
  { id: 3, title: "Laugh Out Loud", year: 2021, genre: "Comedy", rating: 6.9, emoji: "😂", description: "A shy man accidentally becomes a famous comedian." },
  { id: 4, title: "Midnight Mystery", year: 2018, genre: "Thriller", rating: 7.8, emoji: "🕵️", description: "A detective follows clues through a sleeping city." },
  { id: 5, title: "Dragon Kingdom", year: 2020, genre: "Fantasy", rating: 8.5, emoji: "🐉", description: "A young girl discovers she can talk to dragons." },
  { id: 6, title: "Robot Friends", year: 2022, genre: "Animation", rating: 7.6, emoji: "🤖", description: "Two robots learn what friendship really means." },
  { id: 7, title: "Ocean Deep", year: 2017, genre: "Documentary", rating: 8.0, emoji: "🌊", description: "A journey to the darkest parts of the sea." },
  { id: 8, title: "Race Day", year: 2023, genre: "Action", rating: 7.1, emoji: "🏎️", description: "A young driver gets one chance to win the big race." },
];
```

Test it. In `App.jsx`:

```jsx
import { movies } from "./data/movies";

export default function App() {
  console.log(movies);
  return <h1>Movie Browser</h1>;
}
```

** Done when:** you open the browser console (press `F12`) and see an array of 8 movies.

**Tip:** Later, replace these with your own favorite movies.

---

## Step 2: One Movie Card (Components and Props)

** Goal:** Build a reusable `MovieCard` component.

**Concept:** *Props* are data you pass **into** a component, like arguments passed into a function.

Create `src/components/MovieCard.jsx`:

```jsx
export default function MovieCard({ title, emoji, year, rating }) {
  return (
    <div className="card">
      <div className="poster">{emoji}</div>
      <h3>{title}</h3>
      <p>{year} · ⭐ {rating}</p>
    </div>
  );
}
```

Use it in `App.jsx`:

```jsx
import MovieCard from "./components/MovieCard";

export default function App() {
  return (
    <div>
      <h1>Movie Browser</h1>
      <MovieCard title="Space Voyage" emoji="🚀" year={2019} rating={8.2} />
    </div>
  );
}
```

Add styles to `src/index.css`:

```css
body { font-family: sans-serif; margin: 0; padding: 20px; }
.grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(180px, 1fr)); gap: 16px; }
.card { border: 1px solid #ddd; border-radius: 10px; padding: 12px; text-align: center; }
.poster { font-size: 64px; background: #f0f0f0; border-radius: 8px; padding: 20px 0; }
```

**✍️ Try it:** Add two more `MovieCard`s with different props.

** Done when:** you see 3 cards on the page.

**Think about:** Why is it better to have one `MovieCard` component than to copy the same HTML three times?

---

## Step 3: Show All Movies (Lists and Keys)

** Goal:** Show every movie from `movies.js` automatically.

**Concept:** `map()` turns an array of data into an array of components. Every item needs a unique `key` so React can keep track of it.

Update `App.jsx`:

```jsx
import { movies } from "./data/movies";
import MovieCard from "./components/MovieCard";

export default function App() {
  return (
    <div>
      <h1>Movie Browser</h1>
      <div className="grid">
        {movies.map((movie) => (
          <MovieCard
            key={movie.id}
            title={movie.title}
            emoji={movie.emoji}
            year={movie.year}
            rating={movie.rating}
          />
        ))}
      </div>
    </div>
  );
}
```

** Done when:** all 8 movies appear in a grid.

**✍️ Try it:** Add a 9th movie to `movies.js`. Does it appear without changing any other file?

** Stuck?** If you see a "unique key" warning in the console, check that `key={movie.id}` is on the outer component inside `map()`.

---

## Step 4: Add Pages (Routing)

** Goal:** Create a Home page, a Favorites page, and a 404 page with a menu that stays on screen.

**Concepts:** `BrowserRouter`, `Routes`, `Route`, `NavLink`, and `Outlet`.

### 4.1 Wrap the app in a router

`src/main.jsx`:

```jsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";
import "./index.css";

createRoot(document.getElementById("root")).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>
);
```

### 4.2 Create the Home page

Move the grid from `App.jsx` into `src/pages/Home.jsx`:

```jsx
import { movies } from "../data/movies";
import MovieCard from "../components/MovieCard";

export default function Home() {
  return (
    <div>
      <h1>All Movies</h1>
      <div className="grid">
        {movies.map((movie) => (
          <MovieCard
            key={movie.id}
            title={movie.title}
            emoji={movie.emoji}
            year={movie.year}
            rating={movie.rating}
          />
        ))}
      </div>
    </div>
  );
}
```

### 4.3 Create two simple pages

`src/pages/Favorites.jsx`:

```jsx
export default function Favorites() {
  return <h1>My Favorites</h1>;
}
```

`src/pages/NotFound.jsx`:

```jsx
export default function NotFound() {
  return <h1>Page not found 😕</h1>;
}
```

### 4.4 Create the layout

`src/components/Layout.jsx`:

```jsx
import { NavLink, Outlet } from "react-router-dom";

export default function Layout() {
  return (
    <div>
      <nav className="nav">
        <NavLink to="/">Home</NavLink>
        <NavLink to="/favorites">Favorites</NavLink>
      </nav>
      <Outlet />
    </div>
  );
}
```

> **What is `Outlet`?** It is a placeholder. The menu stays on screen, and the current page appears where `<Outlet />` is.

### 4.5 Set up the routes

`src/App.jsx`:

```jsx
import { Routes, Route } from "react-router-dom";
import Layout from "./components/Layout";
import Home from "./pages/Home";
import Favorites from "./pages/Favorites";
import NotFound from "./pages/NotFound";

export default function App() {
  return (
    <Routes>
      <Route element={<Layout />}>
        <Route path="/" element={<Home />} />
        <Route path="/favorites" element={<Favorites />} />
        <Route path="*" element={<NotFound />} />
      </Route>
    </Routes>
  );
}
```

### 4.6 Style the menu

Add to `index.css`:

```css
.nav { display: flex; gap: 16px; margin-bottom: 20px; }
.nav a { text-decoration: none; color: #555; }
.nav a.active { font-weight: bold; color: #4f46e5; }
```

`NavLink` automatically adds the class `active` to the link of the current page.

** Done when:**
- Clicking the menu links changes the page
- The menu does not disappear or reload
- A wrong address like `/xyz` shows the NotFound page

** Stuck?** If the page is blank, check that `<Outlet />` is inside `Layout`.

---

## Step 5: Movie Detail Page (URL Parameters)

** Goal:** Click a movie to open its own page, such as `/movie/3`.

**Concept:** A route like `/movie/:id` has a changing part. `useParams` reads it.

### 5.1 Make each card a link

`src/components/MovieCard.jsx`:

```jsx
import { Link } from "react-router-dom";

export default function MovieCard({ id, title, emoji, year, rating }) {
  return (
    <div className="card">
      <Link to={`/movie/${id}`} className="card-link">
        <div className="poster">{emoji}</div>
        <h3>{title}</h3>
      </Link>
      <p>{year} · ⭐ {rating}</p>
    </div>
  );
}
```

In `Home.jsx`, add `id={movie.id}` to each `MovieCard`. Add to `index.css`:

```css
.card-link { text-decoration: none; color: inherit; }
```

### 5.2 Add the route

In `App.jsx`, inside the Layout route:

```jsx
import MovieDetail from "./pages/MovieDetail";

<Route path="/movie/:id" element={<MovieDetail />} />
```

### 5.3 Create the detail page

`src/pages/MovieDetail.jsx`:

```jsx
import { useParams, Link } from "react-router-dom";
import { movies } from "../data/movies";

export default function MovieDetail() {
  const { id } = useParams();
  const movie = movies.find((m) => m.id === Number(id));

  if (!movie) return <p>Movie not found.</p>;

  return (
    <div>
      <Link to="/">← Back</Link>
      <div className="poster big">{movie.emoji}</div>
      <h1>{movie.title}</h1>
      <p>{movie.year} · {movie.genre} · ⭐ {movie.rating}</p>
      <p>{movie.description}</p>
    </div>
  );
}
```

** Done when:** clicking a movie opens its page, the Back link works, and refreshing the detail page still works.

**🤔 Think about:** `useParams` always returns a **string**, but the ids in `movies.js` are **numbers**. That is why we write `Number(id)`. Remove `Number()` and see what happens. Why does it break?

---

## Step 6: Search (State and Events)

** Goal:** Type in a box and show only matching movies.

**Concepts:** `useState`, controlled inputs, and `filter`.

In `Home.jsx`:

```jsx
import { useState } from "react";
import { movies } from "../data/movies";
import MovieCard from "../components/MovieCard";

export default function Home() {
  const [search, setSearch] = useState("");

  const filtered = movies.filter((movie) =>
    movie.title.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div>
      <h1>All Movies</h1>
      <input
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        placeholder="Search movies..."
      />

      {filtered.length === 0 && <p>No movies found.</p>}

      <div className="grid">
        {filtered.map((movie) => (
          <MovieCard
            key={movie.id}
            id={movie.id}
            title={movie.title}
            emoji={movie.emoji}
            year={movie.year}
            rating={movie.rating}
          />
        ))}
      </div>
    </div>
  );
}
```

**How it works:**
1. You type a letter, and `onChange` runs.
2. `setSearch` saves the new text in state.
3. React re-renders `Home`, and `filtered` is recalculated.

** Done when:** typing in the box shows only matching movies, and "No movies found." appears when nothing matches.

** Think about:** Why is `search` state and not a normal variable like `let search = ""`?

** Challenge:** Make the search match the `genre` as well as the title.

---

## Step 7: Favorites (Lifting State Up)

** Goal:** Add and remove favorites, and show them on the Favorites page.

**Concept:** Two pages (Home and Favorites) need the same data. When two components need the same state, put it in their **closest common parent**. Here that is `App`.

### 7.1 Add state to `App.jsx`

```jsx
import { useState } from "react";
import { Routes, Route } from "react-router-dom";
import Layout from "./components/Layout";
import Home from "./pages/Home";
import Favorites from "./pages/Favorites";
import MovieDetail from "./pages/MovieDetail";
import NotFound from "./pages/NotFound";

export default function App() {
  const [favorites, setFavorites] = useState([]); // a list of movie ids

  function toggleFavorite(id) {
    if (favorites.includes(id)) {
      setFavorites(favorites.filter((favId) => favId !== id)); // remove
    } else {
      setFavorites([...favorites, id]); // add
    }
  }

  return (
    <Routes>
      <Route element={<Layout />}>
        <Route
          path="/"
          element={<Home favorites={favorites} toggleFavorite={toggleFavorite} />}
        />
        <Route
          path="/favorites"
          element={<Favorites favorites={favorites} toggleFavorite={toggleFavorite} />}
        />
        <Route path="/movie/:id" element={<MovieDetail />} />
        <Route path="*" element={<NotFound />} />
      </Route>
    </Routes>
  );
}
```

>  **Never change state directly.** Do not use `favorites.push(id)`. Always create a **new** array with `filter` or `[...favorites, id]`.

### 7.2 Pass props in `Home.jsx`

```jsx
export default function Home({ favorites, toggleFavorite }) {
  // ...same code as before...

  <MovieCard
    key={movie.id}
    id={movie.id}
    title={movie.title}
    emoji={movie.emoji}
    year={movie.year}
    rating={movie.rating}
    isFavorite={favorites.includes(movie.id)}
    onToggleFavorite={() => toggleFavorite(movie.id)}
  />
}
```

### 7.3 Add a button to `MovieCard.jsx`

```jsx
export default function MovieCard({ id, title, emoji, year, rating, isFavorite, onToggleFavorite }) {
  return (
    <div className="card">
      {/* ...link, poster, title, year and rating as before... */}
      <button onClick={onToggleFavorite}>
        {isFavorite ? "♥ Remove" : "♡ Add"}
      </button>
    </div>
  );
}
```

### 7.4 Build the Favorites page

`src/pages/Favorites.jsx`:

```jsx
import { movies } from "../data/movies";
import MovieCard from "../components/MovieCard";

export default function Favorites({ favorites, toggleFavorite }) {
  const favoriteMovies = movies.filter((m) => favorites.includes(m.id));

  return (
    <div>
      <h1>My Favorites</h1>

      {favoriteMovies.length === 0 && <p>No favorites yet. Go add some!</p>}

      <div className="grid">
        {favoriteMovies.map((movie) => (
          <MovieCard
            key={movie.id}
            id={movie.id}
            title={movie.title}
            emoji={movie.emoji}
            year={movie.year}
            rating={movie.rating}
            isFavorite={true}
            onToggleFavorite={() => toggleFavorite(movie.id)}
          />
        ))}
      </div>
    </div>
  );
}
```

** Done when:**
- Clicking ♡ Add on the Home page adds the movie to Favorites
- Clicking ♥ Remove removes it (from both pages)
- An empty list shows "No favorites yet"

** Think about:** Refresh the page. What happened to your favorites? Why? (Bonus 1 fixes this.)

---

## Step 8: Finishing Touches

Complete this checklist:

- [ ] Show the number of favorites in the menu, like `Favorites (3)`
- [ ] Make it look good on a phone (resize your browser to test)
- [ ] Add a footer to `Layout`
- [ ] Add more of your own movies to `movies.js`
- [ ] No red errors or warnings in the browser console
- [ ] Remove unused imports and `console.log` lines

** Stuck on the favorites count?** `Layout` is the one that needs the number. It gets props from `App` just like `Home` does. Use `<Layout favoritesCount={favorites.length} />`.

---

## Step 9: Put It Online

** Goal:** Share your site with the world.

```bash
npm run build
```

**Option A: Netlify**
1. Push your project to GitHub.
2. On [netlify.com](https://www.netlify.com), choose *Add new site → Import from Git* and select your repository.
3. Build command: `npm run build`. Publish directory: `dist`.

**Option B: Vercel**
1. On [vercel.com](https://vercel.com), choose *Add New Project* and import your repository.
2. Accept the default Vite settings.

**Fix for refresh errors (Netlify):** If you see "Page not found" when you refresh on `/favorites`, create `public/_redirects` containing:

```
/* /index.html 200
```

** Done when:** your live site works, and refreshing on a movie page does not break it.

---

##  What to Hand In

1. Your **GitHub repository link**
2. Your **live website link**
3. A short text file `answers.md` with answers to these questions:
   1. What is the difference between **props** and **state**?
   2. Why does every item in `map()` need a **key**?
   3. What does **`Outlet`** do?
   4. Why does the favorites state live in `App` and not in `Home`?

---

##  Bonus Challenges

Try these in any order after finishing the main project.

### Bonus 1: Remember favorites after refresh

Save favorites in the browser with `localStorage`.

```jsx
import { useState, useEffect } from "react";

const [favorites, setFavorites] = useState(() => {
  const saved = localStorage.getItem("favorites");
  return saved ? JSON.parse(saved) : [];
});

useEffect(() => {
  localStorage.setItem("favorites", JSON.stringify(favorites));
}, [favorites]);
```

This is your first real use of `useEffect`: *"when `favorites` changes, save it."*

### Bonus 2: Genre filter
Add buttons or a dropdown to show only one genre.

### Bonus 3: Sort
Add a dropdown to sort movies by rating or year.

### Bonus 4: Dark mode
Use state to switch a CSS class on the page.

### Bonus 5: Use a real API
Replace the local `movies` array with data from the internet. You will need to learn four new things first: what an API is, what JSON is, `fetch` inside `useEffect`, and loading states. Try this only after your teacher has covered them.

---

## 🛠️ Troubleshooting

| Problem | Likely cause and fix |
|---|---|
| Blank white page | Open the console (`F12`) and read the red error. Often a wrong import path or a typo in a file name. |
| `X is not defined` | You forgot to `import` it at the top of the file. |
| "Each child should have a unique key" warning | Add `key={movie.id}` to the outer element inside `map()`. |
| Page content never appears inside the layout | `<Outlet />` is missing from `Layout.jsx`. |
| Detail page says "Movie not found" | Compare `movie.id === Number(id)`. Without `Number()`, `3 === "3"` is false. |
| Clicking a button does nothing | Pass the function, do not call it: `onClick={handle}`, not `onClick={handle()}`. |
| Favorites do not update on screen | You mutated the array with `push`. Create a new array instead. |
| Component not showing | Component names must start with a capital letter: `MovieCard`, not `movieCard`. |
| Everything works locally but refresh fails online | Add the `_redirects` file from Step 9. |
| Styles are missing | Check that `index.css` is imported in `main.jsx`. |

---

## 📊 Grading (100 points)

| Part | Points |
|---|---|
| Components and props (`MovieCard`, `Layout`, pages) | 20 |
| Lists with `map()` and keys | 15 |
| Routing (menu, `Outlet`, detail page with `useParams`, 404) | 25 |
| State and events (search and favorites) | 25 |
| Clean code, deployed site, answers to questions | 15 |

---

##  Reflect

After finishing, think about:

- Which step was hardest, and why?
- What bug took you the longest to fix? How did you find it?
- What would you add to this app if you had another week?

---


The project uses a local data file so students never fight with network errors or API keys. Upgrading to a real API afterwards makes a natural next lesson.

---

##  Useful Links

- [React documentation](https://react.dev)
- [React Router documentation](https://reactrouter.com)
- [Vite documentation](https://vite.dev)
- [MDN: JavaScript array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

---

Happy coding! 🚀
