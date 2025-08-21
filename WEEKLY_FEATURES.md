# Lunch Picker - Weekly Features Update

## ✅ New Features Implemented

### 1. Weekly Restriction System
- **Changed from**: 3-day cooldown 
- **Changed to**: No repeats within the same week (Monday-Sunday)
- **Toggle**: "🚫 No Repeats This Week" checkbox
- **Logic**: Items picked this week are excluded from eligible selections

### 2. Weekly History Table
- **Location**: New section above item management panel
- **Display**: Table showing Day, Lunch, and Time for current week
- **Grouping**: Entries grouped by day with proper day names
- **Today Indicator**: "Today" label for current day entries
- **Empty State**: Shows calendar icon when no lunches picked this week

### 3. Clear Week History
- **Button**: 🗗️ Clear button in weekly history header
- **Function**: Removes all current week entries and resets restrictions
- **Confirmation**: Asks user to confirm before clearing
- **Effect**: Allows re-selection of previously picked items this week

## 🔧 Technical Implementation

### Week Calculation Logic
```javascript
// Monday as week start (ISO standard)
function getWeekStart(date = new Date()) {
    const d = new Date(date);
    const day = d.getDay();
    const diff = d.getDate() - day + (day === 0 ? -6 : 1);
    d.setDate(diff);
    d.setHours(0, 0, 0, 0);
    return d;
}

// Sunday as week end  
function getWeekEnd(date = new Date()) {
    const weekStart = getWeekStart(date);
    const weekEnd = new Date(weekStart);
    weekEnd.setDate(weekStart.getDate() + 6);
    weekEnd.setHours(23, 59, 59, 999);
    return weekEnd;
}
```

### Updated Filtering Logic
```javascript
function getEligibleItems() {
    let eligible = filterItems(appState.items);
    
    if (document.getElementById('avoidRepeats').checked) {
        const thisWeekHistory = getThisWeekHistory();
        const pickedItemIds = thisWeekHistory.map(entry => entry.item_id);
        eligible = eligible.filter(item => !pickedItemIds.includes(item.id));
    }
    
    // Fallback: ignore restriction if no eligible items
    if (eligible.length === 0) {
        eligible = filterItems(appState.items);
    }
    
    return eligible;
}
```

### Data Structure Updates
- **Settings**: Added `weekly_restriction: true` flag
- **History Tracking**: Unchanged - still stores all picks with timestamps
- **Item Management**: `last_picked_at` field still updated for potential future features

## 🎯 User Experience Flow

### Normal Usage
1. **Week Start**: All items available for selection
2. **After Selection**: Picked item excluded from future spins this week
3. **Visual Feedback**: Weekly table shows what was eaten when
4. **Restriction**: Cannot pick same item again until next week

### Edge Cases Handled
1. **No Eligible Items**: If all items picked this week, restriction temporarily ignored
2. **Week Transition**: Sunday night → Monday morning resets all restrictions
3. **Manual Reset**: Clear button allows immediate reset of weekly restrictions
4. **Data Persistence**: Weekly history survives browser refresh/app restart

## 📱 UI Components Added

### Weekly History Section
```css
.weekly-history {
    background: var(--neutral-50);
    border-radius: var(--radius-lg);
    margin-bottom: var(--space-4);
}

.week-table {
    width: 100%;
    border-collapse: collapse;
    max-height: 300px;
    overflow-y: auto;
}
```

### Table Styling Features
- **Sticky Header**: Table header stays visible during scroll
- **Row Hover**: Visual feedback on row interaction
- **Day Grouping**: Multiple entries per day properly grouped
- **Today Highlighting**: Current day entries highlighted in primary color
- **Responsive**: Table scrolls horizontally on mobile if needed

## ✅ Testing Checklist

### Functional Tests
- ✅ Week boundary calculation (Monday start/Sunday end)
- ✅ Item exclusion after selection within same week
- ✅ History table population with correct day grouping
- ✅ Clear week functionality removes restrictions
- ✅ Fallback behavior when no eligible items
- ✅ Data persistence across browser sessions

### UI/UX Tests  
- ✅ Table displays correctly on mobile and desktop
- ✅ Empty state shows when no history
- ✅ Today indicator appears correctly
- ✅ Clear button confirmation dialog
- ✅ Smooth updates after selections
- ✅ Accessibility (table headers, ARIA labels)

## 🚀 Ready for Use

The weekly restriction system is now fully implemented and tested. Users can:

1. **Track Weekly Consumption**: See exactly what they ate each day
2. **Avoid Repetition**: Cannot select same item twice in one week
3. **Manual Override**: Clear week history if needed
4. **Visual Progress**: Table shows weekly eating patterns

The system maintains all existing functionality while adding the requested weekly restriction and history features.

### Usage Instructions
1. **Normal Operation**: Just spin as usual - system automatically prevents weekly repeats
2. **View History**: Check "This Week's Lunches" table to see daily selections
3. **Reset Week**: Click 🗗️ Clear button if you want to allow repeats again
4. **Week Rollover**: Every Monday, restrictions automatically reset

**Ready to use immediately!** 🍽️