# Hytopia Game Development Guidelines

## Getting Started

### Installation & Setup
1. Install dependencies:
   ```bash
   bun install
   ```
2. Configure SSL for local development:
   - Place self-signed certificates in `assets/certs/`
   - Required files:
     - `localhost.key`
     - `localhost.cert`
   - Enables https://localhost & wss://localhost support

3. Start local server:
   ```bash
   bun run index.ts
   ```

### Current Versions
```json
{
  "dependencies": {
    "@hytopia.com/assets": "^0.1.6",
    "hytopia": "^0.1.78"
  },
  "peerDependencies": {
    "typescript": "^5.0.0"
  }
}
```

### Basic Controls
- WASD: Movement
- Space: Jump
- Shift: Sprint
- \\: Toggle debug view

## Project Structure

### Directory Layout
```
project-root/
├── assets/                 # Default HYTOPIA game assets
│   ├── certs/             # SSL certificates for local dev
│   ├── models/            # GLTF model files
│   ├── textures/          # Texture files
│   ├── map.json          # World configuration
│   ├── package.json      # Assets package config
│   └── LICENSE.md        # HYTOPIA LIMITED USE LICENSE
└── src/
    └── index.ts          # Main entry point
```

### Naming Conventions
- **Files:** lowercase with descriptive names
  - Main entry: `index.ts`
  - Map configuration: `map.json`
  - Source files: `game-logic.ts`, `player-manager.ts`

- **Models:** lowercase with `.gltf` extension
- **Textures:** lowercase with `.png` extension
- **Block Types:** lowercase with underscores (e.g., `diamond_ore`, `stone_bricks`)

## Core Development

### Server Initialization
```typescript
import { startServer, Audio, GameServer, PlayerEntity } from 'hytopia';
import worldMap from './assets/map.json';

startServer(world => {
    // World initialization
    // Event handlers
    // Command registration
});
```

### Player Entity Setup
```typescript
const playerEntity = new PlayerEntity({
  player,
  name: 'Player',
  modelUri: 'models/player.gltf',
  modelLoopedAnimations: ['idle'],
  modelScale: 0.5,
});

playerEntity.spawn(world, { x: 0, y: 10, z: 0 });
```

### Event Handlers
```typescript
// Player Management
world.onPlayerJoin = player => {
    // Initial player setup
    // Spawn location
    // Starting inventory
};

world.onPlayerLeave = player => {
    // Save player data
    // Cleanup resources
};

// World Events
world.onBlockBreak = (player, block) => {
    // Block breaking logic
    // Drop items
    // Update world state
};
```

### Command System
```typescript
// Example: Rocket Command
world.chatManager.registerCommand('/rocket', player => {
  world.entityManager.getPlayerEntitiesByPlayer(player).forEach(entity => {
    entity.applyImpulse({ x: 0, y: 20, z: 0 });
  });
});
```

### Audio System
```typescript
new Audio({
  uri: 'audio/music/overworld.mp3',
  loop: true,
  volume: 0.1,
}).play(world);
```

## Available Assets

### Model Library
- Player Characters:
  - `player.gltf` (Default player model)
  
- Passive Creatures:
  - `horse.gltf`, `sheep.gltf`, `chicken.gltf`
  - `cow.gltf`, `pig.gltf`, `rabbit.gltf`
  - `donkey.gltf`
  
- Hostile Creatures:
  - `zombie.gltf`, `skeleton.gltf`, `spider.gltf`
  - `mindflayer.gltf`, `stalker.gltf`
  
- Ambient Creatures:
  - `bat.gltf`, `squid.gltf`

## Performance Optimization

### World Management
- Implement chunk loading similar to Minecraft
- Use occlusion culling for rendering optimization
- Batch similar block updates
- Cache frequently accessed world data

### Entity Management
- Pool frequently created/destroyed entities
- Implement proper bounds checking
- Use efficient collision detection algorithms
- Optimize entity update cycles

### Resource Management
- Preload essential assets
- Implement progressive loading for distant@ chunks
- Use texture atlases for blocks
- Optimize model complexity for performance

## Multiplayer Considerations

### Network Optimization
- Implement delta compression for position updates
- Use prediction for smooth movement
- Prioritize critical state synchronization
- Buffer non-essential updates

### State Management
- Maintain consistent world state across clients
- Implement proper conflict resolution
- Use efficient serialization for network packets
- Handle disconnections gracefully

## Development Tools

### Debug Features
```typescript
// Enable physics debug rendering (development only)
world.simulation.enableDebugRendering(true);

// Debug view toggle command (press \)
world.chatManager.sendPlayerMessage(player, 'Press \\ to enter or exit debug view.');
```

### TypeScript Configuration
```json
{
  "compilerOptions": {
    "lib": ["ESNext", "DOM"],
    "target": "ESNext",
    "module": "ESNext",
    "strict": true,
    "jsx": "react-jsx",
    "moduleResolution": "bundler"
  }
}
```

## Legal & Support

### License Information
- Assets are under HYTOPIA LIMITED USE LICENSE
- Requires maintaining active HYTOPIA registration
- Source code modifications allowed for HYTOPIA platform use
- Feedback submissions grant HYTOPIA unrestricted usage rights

### Support Resources
- Documentation: https://github.com/hytopiagg/sdk/blob/main/docs/server.md
- Examples: https://github.com/hytopiagg/sdk/tree/main/examples/payload-game
- Issue Tracking: https://github.com/hytopiagg/sdk/issues
- Discord Community: https://discord.gg/DXCXJbHSJX
