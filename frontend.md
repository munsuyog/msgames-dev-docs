# Frontend Documentation

## Project Overview
The frontend is built using React with TypeScript, utilizing Vite as the build tool. The project follows a modular architecture with clear separation of concerns.

## Core Technologies
- React 18.2.0
- TypeScript 5.0.2
- Vite 4.4.5
- TailwindCSS
- Redux Toolkit
- React Router

## Directory Structure

```
src/
├── apis/                  # API integration layer
│   ├── endpoints/         # API endpoint definitions
│   │   ├── auth.ts       # Authentication endpoints
│   │   ├── game.ts       # Game-related endpoints
│   │   └── user.ts       # User-related endpoints
│   ├── hooks/            # Custom API hooks
│   └── services/         # API service functions
│
├── assets/               # Static assets
│   ├── images/          # Image files
│   ├── icons/           # Icon files
│   └── fonts/           # Font files
│
├── components/           # Reusable components
│   ├── common/          # Generic components
│   │   ├── Button/
│   │   ├── Input/
│   │   └── Modal/
│   ├── layout/          # Layout components
│   │   ├── Header/
│   │   ├── Footer/
│   │   └── Sidebar/
│   ├── forms/           # Form components
│   │   ├── LoginForm/
│   │   └── SignupForm/
│   └── game/            # Game-specific components
│       ├── GameBoard/
│       └── GameControls/
│
├── helpers/             # Helper functions
│   ├── formatters/      # Data formatting utilities
│   ├── validators/      # Validation functions
│   └── utils/          # General utilities
│
├── pages/              # Page components
│   ├── auth/          # Authentication pages
│   │   ├── Login.tsx
│   │   └── Register.tsx
│   ├── game/          # Game pages
│   │   ├── GameList.tsx
│   │   └── GamePlay.tsx
│   └── dashboard/     # Dashboard pages
│
├── routes/            # Routing configuration
│   ├── PrivateRoute.tsx
│   ├── PublicRoute.tsx
│   └── AppRoutes.tsx
│
├── store/             # Redux store configuration
│   ├── slices/       # Redux slices
│   │   ├── authSlice.ts
│   │   ├── gameSlice.ts
│   │   └── uiSlice.ts
│   ├── hooks/        # Custom store hooks
│   └── store.ts      # Store configuration
│
├── styles/           # Global styles
│   ├── global.css   # Global CSS
│   └── theme.css    # Theme variables
│
├── types/           # TypeScript type definitions
│   ├── api/        # API types
│   ├── models/     # Data model types
│   └── store/      # Redux store types
│
└── utils/          # Utility functions
    ├── constants.ts
    ├── functions.ts
    └── validation.ts
```

## Directory Details

### `/apis`
Contains all API-related code:
- `endpoints/`: API endpoint definitions and configurations
- `hooks/`: Custom hooks for API calls
- `services/`: Service layer for API interactions

```typescript
// Example API endpoint definition
// apis/endpoints/game.ts
export const gameEndpoints = {
  getGames: '/api/games',
  getGame: (id: string) => `/api/games/${id}`,
  createGame: '/api/games',
};
```

### `/components`
Reusable UI components:
- `common/`: Generic, reusable components
- `layout/`: Page layout components
- `forms/`: Form-related components
- `game/`: Game-specific components

```typescript
// Example component structure
// components/common/Button/index.tsx
import React from 'react';
import type { ButtonProps } from './types';
import styles from './Button.module.css';

export const Button: React.FC<ButtonProps> = ({ 
  children, 
  variant = 'primary' 
}) => {
  return (
    <button className={styles[variant]}>
      {children}
    </button>
  );
};
```

### `/pages`
Page components that represent routes:
```typescript
// Example page structure
// pages/game/GamePlay.tsx
import React from 'react';
import { GameBoard, GameControls } from '@/components/game';
import { useGame } from '@/hooks/useGame';

export const GamePlay: React.FC = () => {
  const { gameState, actions } = useGame();

  return (
    <div>
      <GameBoard state={gameState} />
      <GameControls actions={actions} />
    </div>
  );
};
```

### `/store`
Redux store configuration:
```typescript
// Example store slice
// store/slices/gameSlice.ts
import { createSlice } from '@reduxjs/toolkit';

export const gameSlice = createSlice({
  name: 'game',
  initialState: {
    currentGame: null,
    games: [],
  },
  reducers: {
    // reducers
  },
});
```

### `/types`
TypeScript type definitions:
```typescript
// Example type definition
// types/models/game.ts
export interface Game {
  id: string;
  status: 'active' | 'completed';
  players: Player[];
  score: number;
}
```

## Best Practices

### Component Organization
1. One component per file
2. Co-locate related files
3. Use index.ts for exports

```
Button/
├── index.tsx
├── Button.tsx
├── Button.module.css
└── types.ts
```

### File Naming
- Components: PascalCase (Button.tsx)
- Utilities: camelCase (formatDate.ts)
- Types: PascalCase (UserType.ts)
- Constants: SCREAMING_SNAKE_CASE

### Import Organization
```typescript
// External imports
import React from 'react';
import { useDispatch } from 'react-redux';

// Internal imports
import { Button } from '@/components/common';
import { useGame } from '@/hooks';
import { formatDate } from '@/utils';
```

### State Management
- Use Redux for global state
- Use local state for component-specific state
- Use React Query for server state

### Styling
- Use CSS Modules for component styles
- Use Tailwind for utility classes
- Keep global styles minimal

## Code Standards

### TypeScript
- Use strict mode
- Define interfaces for props
- Use type inference where possible
- Document complex types

### Performance
- Lazy load routes
- Memoize expensive computations
- Optimize re-renders
- Use proper key props

### Accessibility
- Use semantic HTML
- Include ARIA labels
- Ensure keyboard navigation