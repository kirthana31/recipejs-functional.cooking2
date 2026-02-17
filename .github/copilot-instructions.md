# RecipeJS - AI Coding Agent Instructions

## Project Overview
RecipeJS is a **functional programming recipe companion app**. It demonstrates pure functional patterns with immutable data transformations, filtering, and sorting—no build tools, no frameworks, vanilla JavaScript only.

## Architecture & Data Flow

**Core Pattern:** Data → Transform → Render
1. **Data Layer** (`recipes` array): Static recipe objects with `id`, `title`, `time`, `difficulty`, `description`, `category`
2. **Transform Layer**: Pure functions (`filterRecipes`, `sortRecipes`) that return new arrays without mutation
3. **Render Layer**: `createRecipeCard` generates HTML, `renderRecipes` updates DOM

**Key Principle:** Always copy arrays before mutation (e.g., `const sorted = [...recipes]`) to maintain functional purity.

## Critical Code Patterns

### Template Literals for Card Generation
```javascript
const createRecipeCard = (recipe) => {
  return `<div class="recipe-card" data-id="${recipe.id}">
    <h3>${recipe.title}</h3>
    <span class="difficulty ${recipe.difficulty}">${recipe.difficulty}</span>
  </div>`;
};
```

### Pure Filter Functions
```javascript
const filterRecipes = (recipes, filter) => {
  switch (filter) {
    case "EASY": return recipes.filter(r => r.difficulty === "easy");
    default: return recipes;
  }
};
```
Note: Filter values are UPPERCASE strings ("EASY", "MEDIUM", "HARD", "QUICK"), but difficulty in data is lowercase.

### Event Delegation Pattern
Buttons use `data-*` attributes; listeners attach to containers with `btn.dataset.*` access:
```javascript
document.querySelectorAll("#filters button").forEach(btn => {
  btn.addEventListener("click", () => {
    currentFilter = btn.dataset.filter;
    updateDisplay();
  });
});
```

## Known Issues & Gotchas
1. **Property Name Inconsistency**: Recipe objects use `title` and `time`, but some code references `name` (causes bugs)
2. **Duplicate `renderRecipes`**: Two function definitions exist—the second overrides the first
3. **Case Sensitivity Bug**: Filter switches check capitalized difficulty ("Easy") but data is lowercase ("easy")
4. **Missing Binding**: Button selectors expect `#filters` and `#sort` containers; verify HTML structure matches selectors

## Development Workflow
- **No build system**: Direct HTML → JavaScript execution in browser
- **Testing**: Open `index.html` in browser; inspect DevTools console for errors
- **Styling**: CSS Grid for card layout; `.difficulty.{easy|medium|hard}` classes drive badge colors

## When Implementing Features
- **New filters**: Add uppercase case to `filterRecipes` switch; update HTML with matching `data-filter` button
- **New sorts**: Add case to `sortRecipes`; ensure algorithm maintains immutability
- **New fields**: Add to recipe objects; update `createRecipeCard` template; watch for cascading selector issues
- **DOM changes**: Always verify selectors like `#recipe-container`, `#filters`, `#sort` match HTML

## Style & Conventions
- Section headers with comment borders: `// ========================== Section Name ==========================`
- Pure functions only—no direct DOM mutations outside render layer
- Spread operator for array copies: `[...array]`
- Kebab-case for HTML IDs/classes; camelCase for JS functions
