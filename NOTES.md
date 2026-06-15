# Detective notes: Theme Context Lab

---

## Bug: Aurora Notes is stuck in Light Mode

**What I think is wrong (before fixing):**
The application had the CSS styling for dark mode ready in \globals.css\, but there was no global state mechanism or state machine to toggle the \.dark\ class on the root HTML element. The button was completely static and had no event handler.

**What did I ask the AI / what did I look up:**
I reviewed the React Context API documentation for implementing custom providers and consumer hooks, as well as the 'use client' boundaries in Next.js.

**What was the solution:**
I created a custom \ThemeProvider\ in \app/context/ThemeContext.js\ that tracks the local theme status ("light" or "dark"). Using a \useEffect\ hook inside the provider, I automatically injected or removed the \.dark\ class on \document.documentElement\ whenever the state changed. Finally, I wrapped the entire application tree inside \app/layout.js\ so any component can access \useTheme()\.

---

## Closing reflection

- **Where does the theme live?**
  The theme lives in the \ThemeProvider\ wrapper node high above the individual page layouts. This architecture prevents the state from being trapped inside a specific route file like \page.js\, ensuring future elements like navbars or preference modals can access the shared data.

- **What has to become a Client Component, and what can stay on the server?**
  The \ThemeProvider\ and the page holding the toggle button must become Client Components (\"use client"\) because they depend on hooks (\useState\, \useEffect\, \useContext\) and interactive native browser operations like \onClick\. Global layout components can remain Server Components to preserve optimal hydration metrics.

- **Where do you put the \.dark\ class?**
  I applied the \.dark\ class directly to the root \<html>\ element via \window.document.documentElement\. This choice leverages CSS custom variable cascading, meaning every child element using the Tailwind design system tokens anywhere in the document instantly adapts to the context shift.
