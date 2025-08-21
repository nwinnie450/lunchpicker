# Lunch Picker - Design Specification

## Project Overview

**Product:** Lunch Picker - Random Lunch Decider  
**Design Goal:** Create a fast, fun, and intuitive interface that helps users decide what to eat for lunch in under 10 seconds  
**Design Principles:** Mobile-first, accessibility-focused, performance-optimized, delightfully simple

---

## Design System

### Color Palette

```css
/* Primary Colors */
--primary-500: #FF6B35;     /* Vibrant orange - main CTA */
--primary-400: #FF8A65;     /* Lighter orange - hover states */
--primary-600: #E53E3E;     /* Darker orange - pressed states */

/* Secondary Colors */
--secondary-500: #4299E1;   /* Blue - secondary actions */
--secondary-400: #63B3ED;   /* Light blue - hover */
--secondary-600: #3182CE;   /* Dark blue - pressed */

/* Neutral Colors */
--neutral-50: #F9FAFB;      /* Background */
--neutral-100: #F3F4F6;     /* Card backgrounds */
--neutral-200: #E5E7EB;     /* Borders */
--neutral-300: #D1D5DB;     /* Disabled states */
--neutral-400: #9CA3AF;     /* Placeholder text */
--neutral-500: #6B7280;     /* Secondary text */
--neutral-700: #374151;     /* Primary text */
--neutral-900: #111827;     /* Headers */

/* Status Colors */
--success-500: #10B981;     /* Success states */
--warning-500: #F59E0B;     /* Warning states */
--error-500: #EF4444;       /* Error states */

/* Semantic Colors */
--food-vegetarian: #22C55E; /* Vegetarian tag */
--food-spicy: #EF4444;      /* Spicy tag */
--food-healthy: #06B6D4;    /* Healthy tag */
--food-comfort: #8B5CF6;    /* Comfort food tag */
```

### Typography

```css
/* Font Stack */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;

/* Type Scale */
--text-xs: 0.75rem;    /* 12px - Captions */
--text-sm: 0.875rem;   /* 14px - Body small */
--text-base: 1rem;     /* 16px - Body */
--text-lg: 1.125rem;   /* 18px - Large body */
--text-xl: 1.25rem;    /* 20px - H4 */
--text-2xl: 1.5rem;    /* 24px - H3 */
--text-3xl: 1.875rem;  /* 30px - H2 */
--text-4xl: 2.25rem;   /* 36px - H1 */
--text-5xl: 3rem;      /* 48px - Display */

/* Font Weights */
--font-normal: 400;
--font-medium: 500;
--font-semibold: 600;
--font-bold: 700;

/* Line Heights */
--leading-tight: 1.25;
--leading-normal: 1.5;
--leading-relaxed: 1.625;
```

### Spacing System

```css
/* Spacing Scale (4px base) */
--space-1: 0.25rem;   /* 4px */
--space-2: 0.5rem;    /* 8px */
--space-3: 0.75rem;   /* 12px */
--space-4: 1rem;      /* 16px */
--space-5: 1.25rem;   /* 20px */
--space-6: 1.5rem;    /* 24px */
--space-8: 2rem;      /* 32px */
--space-10: 2.5rem;   /* 40px */
--space-12: 3rem;     /* 48px */
--space-16: 4rem;     /* 64px */
--space-20: 5rem;     /* 80px */
```

### Border Radius

```css
--radius-sm: 0.375rem;   /* 6px - Small elements */
--radius-md: 0.5rem;     /* 8px - Cards, buttons */
--radius-lg: 0.75rem;    /* 12px - Larger cards */
--radius-xl: 1rem;       /* 16px - Main spin button */
--radius-full: 9999px;   /* Circular elements */
```

### Shadows

```css
--shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
--shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
--shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
--shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
```

---

## Component Specifications

### 1. Header Component

**Purpose:** App branding and quick access to settings  
**Height:** 64px mobile, 72px desktop

```
┌─────────────────────────────────────────┐
│ 🍽️ Lunch Picker              ⚙️        │
└─────────────────────────────────────────┘
```

**Specifications:**
- Background: `--neutral-50` with bottom border `--neutral-200`
- Logo: Food emoji + "Lunch Picker" in `--text-2xl` `--font-semibold`
- Settings icon: 24px, `--neutral-500`, hover: `--neutral-700`
- Padding: `--space-4` horizontal, `--space-5` vertical
- Sticky positioning on scroll

### 2. Primary Spin Button

**Purpose:** Main CTA for generating lunch picks  
**Size:** 200px × 200px mobile, 240px × 240px desktop

```
        ┌─────────────────┐
        │                 │
        │      🎲         │
        │     SPIN        │
        │                 │
        └─────────────────┘
```

**Specifications:**
- Shape: Circle with `--radius-full`
- Background: Gradient `--primary-500` to `--primary-600`
- Text: "SPIN" in `--text-3xl` `--font-bold` white
- Icon: 48px dice emoji above text
- Shadow: `--shadow-xl` with orange glow
- Hover: Scale 1.05, brighter gradient
- Active: Scale 0.98, deeper shadow
- Loading: Rotation animation with pulse effect

**States:**
- Default: Orange gradient with subtle pulse
- Hover: Brighter, lifted shadow
- Spinning: Rotate 360° over 2s with elastic easing
- Disabled: `--neutral-300` background, no interaction

### 3. Result Card

**Purpose:** Display the chosen lunch item with actions  
**Dimensions:** Full width mobile, max 400px desktop

```
┌─────────────────────────────────────────┐
│ 🌮 Tacos from Corner Deli         $$$  │
│ #mexican #spicy #quick                  │
│ Great fish tacos, ask for extra lime!  │
│                                         │
│ [Pick Again] [Lock Filters] [Mark Eaten]│
└─────────────────────────────────────────┘
```

**Specifications:**
- Background: `--neutral-50` with `--radius-lg`
- Border: 2px solid `--primary-500` (winner highlight)
- Padding: `--space-6`
- Animation: Slide up from bottom with bounce

**Content Layout:**
- Title: `--text-2xl` `--font-semibold` `--neutral-900`
- Price: Right-aligned, `--text-lg` `--neutral-500`
- Tags: Chips with category colors, `--space-2` gap
- Notes: `--text-base` `--neutral-700` italic
- Actions: Button row with `--space-3` gap

### 4. Filter Bar

**Purpose:** Quick filtering without leaving main view  
**Height:** Auto-height with wrap

```
┌─────────────────────────────────────────┐
│ 🏷️ [Mexican] [Quick] [Healthy]          │
│ 💰 [$] [$$] [$$$]  🚫 Avoid Repeats    │
└─────────────────────────────────────────┘
```

**Specifications:**
- Background: `--neutral-100` with top border
- Padding: `--space-4`
- Sections: Tags, Price, Settings toggle

**Filter Chips:**
- Default: `--neutral-200` background, `--neutral-700` text
- Active: `--primary-500` background, white text
- Shape: `--radius-full` pills
- Size: `--text-sm` with `--space-2` `--space-3` padding

### 5. Item List Panel

**Purpose:** Manage lunch items with search and CRUD operations  
**Layout:** Collapsible panel below main interface

```
┌─────────────────────────────────────────┐
│ 🔍 Search items...               [+ Add]│
├─────────────────────────────────────────┤
│ 🌮 Tacos from Corner Deli         [⚙️] │
│    #mexican #spicy $$$                  │
├─────────────────────────────────────────┤
│ 🍕 Pizza Palace                   [⚙️] │
│    #italian #comfort $$                │
└─────────────────────────────────────────┘
```

**Specifications:**
- Expandable accordion starting collapsed
- Search: Full-width input with magnifying glass icon
- Add button: Primary style, top-right corner
- Items: List with hover states and edit access

**Item Row Layout:**
- Left: Emoji + Name (`--text-lg` `--font-medium`)
- Tags: Below name, smaller chips
- Right: Price + Edit button
- Hover: `--neutral-100` background
- Padding: `--space-4` vertical, `--space-5` horizontal

### 6. Item Edit Modal

**Purpose:** Add/edit lunch items with all properties  
**Size:** Mobile: Full screen, Desktop: 500px wide modal

```
┌─────────────────────────────────────────┐
│ ✏️ Edit Item                       [×] │
├─────────────────────────────────────────┤
│ Name: [Tacos from Corner Deli        ] │
│ Emoji: [🌮]  Price: [$$$ ▼]           │
│ Tags: [mexican] [spicy] [+ Add]        │
│ Notes: [Great fish tacos, ask for...] │
│ Weight: [●●●○○] Normal                 │
│                                         │
│           [Cancel] [Save]               │
└─────────────────────────────────────────┘
```

**Form Elements:**
- Text inputs: `--radius-md` border `--neutral-300`
- Dropdowns: Native select with custom arrow
- Tag chips: Removable with × icon
- Weight slider: 5-dot visual scale
- Buttons: Cancel (secondary), Save (primary)

### 7. History Drawer

**Purpose:** Show recent picks for reference  
**Access:** Footer link opens slide-up drawer

```
┌─────────────────────────────────────────┐
│ 📜 Recent Picks                    [×] │
├─────────────────────────────────────────┤
│ Today                                   │
│ 🌮 Tacos (Corner Deli)     12:30 PM   │
│                                         │
│ Yesterday                               │
│ 🍕 Pizza Palace            1:15 PM    │
│ 🥗 Garden Salad           12:45 PM    │
└─────────────────────────────────────────┘
```

**Specifications:**
- Grouped by day with date headers
- Items show name, source, and time
- Scrollable list, max 30 items
- Pull to refresh on mobile

---

## Responsive Layouts

### Mobile Layout (320px - 768px)

```
┌─────────────────────┐
│ 🍽️ Lunch Picker  ⚙️│
├─────────────────────┤
│                     │
│       ┌─────┐       │
│       │ 🎲  │       │
│       │SPIN │       │
│       └─────┘       │
│                     │
├─────────────────────┤
│ [Result Card]       │
├─────────────────────┤
│ [Filter Chips]      │
├─────────────────────┤
│ [Item List Toggle]  │
│                     │
│ History | Advanced  │
└─────────────────────┘
```

**Mobile Specifications:**
- Single column layout
- Spin button: 200px centered
- Full-width cards with `--space-4` margin
- Collapsible sections to save space
- Bottom sheet modals for editing

### Desktop Layout (1024px+)

```
┌─────────────────────────────────────────────────────┐
│ 🍽️ Lunch Picker                               ⚙️   │
├─────────────────────────────────────────────────────┤
│                    │                                │
│       ┌─────┐      │  [Filter Bar]                  │
│       │ 🎲  │      │  ┌─────────────────────────────┐│
│       │SPIN │      │  │ [Search] [+ Add]           ││
│       └─────┘      │  ├─────────────────────────────┤│
│                    │  │ Item List                  ││
│  [Result Card]     │  │ • Tacos                    ││
│                    │  │ • Pizza                    ││
│                    │  │ • Salad                    ││
│                    │  └─────────────────────────────┘│
│ History | Advanced │                                │
└─────────────────────────────────────────────────────┘
```

**Desktop Specifications:**
- Two-column layout (60/40 split)
- Left: Spin button + result
- Right: Filters + item management
- Sidebar panel always visible
- Modal dialogs for editing

---

## Animations & Micro-interactions

### Spin Animation Sequence

1. **Click Response** (0.1s)
   - Button scales to 0.98
   - Slight shadow increase

2. **Spin Phase** (2.0s)
   - 360° rotation with elastic easing
   - Gentle pulsing glow
   - Disable all interactions

3. **Result Reveal** (0.5s)
   - Result card slides up from bottom
   - Bounce animation on entry
   - Confetti effect (brief)

### Interactive Feedback

- **Button Hovers:** 0.2s scale + color transition
- **Card Interactions:** Gentle lift shadow on hover
- **Filter Toggles:** Smooth color transitions
- **List Items:** Fade backgrounds on hover
- **Form Inputs:** Focus ring with brand color

### Loading States

- **Spin Button:** Pulse animation during processing
- **List Loading:** Skeleton cards with shimmer
- **Search Results:** Fade transition between states

---

## Accessibility Features

### Color & Contrast

- All text meets WCAG AA contrast ratios (4.5:1+)
- Color is never the only indicator of state
- High contrast mode support

### Keyboard Navigation

- Tab order: Header → Spin → Filters → List → Footer
- Enter/Space activates spin button
- Arrow keys navigate filter chips
- Escape closes modals/drawers

### Screen Reader Support

- Semantic HTML structure
- ARIA labels for interactive elements
- Live regions for dynamic content
- Skip links for main content

### Visual Indicators

- Focus rings on all interactive elements
- Loading indicators for async operations
- Clear error messages with icons
- Tooltips for complex interactions

---

## Empty States & Onboarding

### First-Time User Experience

**Empty State (No Items):**
```
┌─────────────────────────────────────────┐
│               🍽️                       │
│         Welcome to                      │
│        Lunch Picker!                    │
│                                         │
│   Add some lunch options to get        │
│          started                        │
│                                         │
│        [+ Add Your First Item]         │
└─────────────────────────────────────────┘
```

**Guided First Steps:**
1. Prompt to add 3-5 initial items
2. Show example items with "Try these" button
3. Demonstrate first spin with tutorial overlay
4. Highlight filter features after first pick

### Progressive Enhancement

- Core functionality works without JavaScript
- Graceful degradation for older browsers
- Offline-first with service worker
- App shell loads instantly

---

## Performance Considerations

### Critical Rendering Path

1. **Above-fold priority:** Header + Spin button + basic styles
2. **Secondary loading:** Filter bar + item list
3. **Lazy loading:** History drawer + settings modal

### Bundle Optimization

- CSS-in-JS with critical CSS extraction
- Component code splitting
- Icon optimization (subset font or SVG sprites)
- Image optimization with WebP fallbacks

### Interaction Performance

- 60fps animations with CSS transforms
- Debounced search input (300ms)
- Virtual scrolling for large item lists
- Optimistic UI updates

---

## Implementation Notes for Developer

### CSS Architecture

```scss
// Recommended structure
src/
  styles/
    tokens.css       // Design system variables
    base.css         // Reset + base styles
    components/      // Component-specific styles
    utilities.css    // Utility classes
```

### Component Props Interface

```typescript
// Example component interfaces
interface SpinButtonProps {
  isSpinning: boolean;
  onSpin: () => void;
  disabled?: boolean;
}

interface FilterBarProps {
  tags: Tag[];
  selectedTags: string[];
  priceRange: PriceRange;
  avoidRepeats: boolean;
  onFilterChange: (filters: Filters) => void;
}
```

### State Management

- Context API for global state (items, filters)
- Local component state for UI interactions
- localStorage for persistence
- Service worker for offline support

### Testing Considerations

- Component isolation for visual testing
- Accessibility testing with axe-core
- Performance testing with Lighthouse
- Cross-device testing matrix

---

## Design Handoff Checklist

### Assets Delivered

- ✅ Complete design system tokens
- ✅ Component specifications with states
- ✅ Responsive layout guidelines
- ✅ Animation specifications
- ✅ Accessibility requirements
- ✅ Performance guidelines
- ✅ Empty state designs
- ✅ Error state handling

### Developer Resources

- **Figma File:** [Link to interactive prototypes]
- **Style Guide:** All design tokens in CSS format
- **Animation Demos:** Lottie files or CSS examples
- **Icon Library:** SVG sprite or font subset
- **Typography:** Font files and fallback stack
- **Testing Matrix:** Browser and device requirements

### Success Metrics

- **Performance:** <2s TTI on 3G networks
- **Accessibility:** 100% WCAG AA compliance
- **User Experience:** <10s from load to lunch pick
- **Visual Quality:** Pixel-perfect implementation
- **Interaction Design:** Smooth 60fps animations

---

## Next Steps

This design specification provides everything needed to implement the Lunch Picker app. The focus is on creating a delightful, fast, and accessible experience that makes lunch decisions fun rather than stressful.

**Ready for handoff to Frontend Developer!** 🚀

*Design Phase Complete - All specifications documented and ready for implementation*