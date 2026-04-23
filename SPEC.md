# Flashcard App Specification

## Project Overview
- **Name**: FlashStudy
- **Type**: Mobile flashcard application
- **Core Functionality**: Interactive flashcard study with multiple decks, rich media support, and progress tracking

## Technology Stack
- React Native with Expo
- AsyncStorage for local data persistence
- expo-image-picker for images
- expo-av for audio playback
- React Navigation (stack)
- react-native-gesture-handler + react-native-reanimated for swipe gestures

## UI/UX Specification

### Color Palette
- **Primary**: #4A90D9 (blue)
- **Secondary**: #6C757D (gray)
- **Success**: #28A745 (green)
- **Warning**: #FFC107 (yellow)
- **Danger**: #DC3545 (red)
- **Background**: #F8F9FA (light gray)
- **Card**: #FFFFFF (white)
- **Text Primary**: #212529
- **Text Secondary**: #6C757D

### Typography
- **Font Family**: System default (San Francisco on iOS, Roboto on Android)
- **Heading Large**: 28px, bold
- **Heading Medium**: 22px, semi-bold
- **Body**: 16px, regular
- **Caption**: 14px, regular

### Spacing System (8pt grid)
- **xs**: 4px
- **sm**: 8px
- **md**: 16px
- **lg**: 24px
- **xl**: 32px

## Screen Structure

### 1. DecksScreen (Home)
- Header with app title
- List of decks showing: name, card count, progress percentage
- FAB to create new deck
- Tap deck → CardsScreen
- Long press → Edit/Delete options

### 2. CardsScreen
- Header with deck name and card count
- List of cards showing: front text preview, has media indicators
- FAB to add new card
- Tap card → Edit card modal
- Swipe left → Delete confirmation

### 3. StudyScreen
- Header with deck name and progress (X/Y)
- Large flashcard (centered)
- Tap to flip (front ↔ back)
- Swipe right = Correct (green flash)
- Swipe left = Skip (red flash, move to end)
- Card count shows remaining
- Exit button in header

### 4. StatsScreen
- Header "Progress"
- Per-deck stats cards showing:
  - Deck name
  - Cards studied (total)
  - Correct count
  - Skip count
  - Last studied date

### 5. Add/Edit Deck Modal
- Text input for deck name
- Number input for max cards (default 100)
- Save/Cancel buttons

### 6. Add/Edit Card Modal
- Text input for front (question)
- Text input for back (answer)
- Image picker button (optional)
- Audio record/pick button (optional)
- Preview of media
- Save/Cancel buttons

### 7. Export/Import (Settings accessible from DecksScreen)
- Export button (saves JSON to device)
- Import button (pick JSON file)
- Success/error feedback

## Functionality Specification

### Core Features
1. **Create/Edit/Delete Decks**
2. **Create/Edit/Delete Cards** (with optional image/audio)
3. **Study Mode** with swipe gestures
4. **Progress Tracking** per deck
5. **Export/Import** decks as JSON

### Data Models
```typescript
interface Deck {
  id: string;
  name: string;
  maxCards: number;
  createdAt: string;
}

interface Card {
  id: string;
  deckId: string;
  front: string;
  back: string;
  imageUri?: string;
  audioUri?: string;
  order: number;
}

interface StudyStats {
  deckId: string;
  cardsStudied: number;
  correctCount: number;
  skipCount: number;
  lastStudied: string;
}
```

### User Flows
1. **Create Deck**: Home → Tap FAB → Enter name/max → Save
2. **Add Card**: Deck → Cards → Tap FAB → Enter front/back → Add media (opt) → Save
3. **Study**: Deck → Study button → Flip card → Swipe right/left → Repeat → Done
4. **View Stats**: Home → Stats tab → View all decks
5. **Export/Import**: Home → Settings → Export/Import

### Edge Cases
- Empty deck (no cards) → Show "Add cards to study" message
- Max cards reached → Show warning, option to increase
- No media permissions → Show settings prompt
- Corrupt JSON import → Show error message