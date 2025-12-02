
# 6-Axis Industrial Manipulator Visualization

![Demo](demo.gif)

## Quick Start (Docker - Recommended)

> **⚠️ Platform Support**: This Docker setup has been tested on **Linux (x86_64)** only. It may work on macOS and Windows, but is currently untested on those platforms.

### Running the Application
```bash
# For Linux/Mac:
./launch.sh

# For Windows:
launch.bat
```

**Note**: Despite its name, `launch.sh` is the main script to run the application. It will:

1. Check for Docker and install it if needed (Linux only)
2. Verify Docker is running
3. Build and start the application in development mode
4. Open the app at http://localhost:4200 (with backend on :9000)

Run `./launch.sh` every time you want to start the application.

### Manual Installation

If you prefer to install manually:

#### Prerequisites

- **Docker**: Install from https://docs.docker.com/get-docker/
- **Docker Compose**: Usually included with Docker Desktop

#### Installation & Run
```bash
# 1. Clone/download this repository
# 2. Navigate to the project directory
cd <repo_root>

# 3. Build and run with Docker Compose (Development Mode)
docker compose -f docker-compose.dev.yml up --build

# Or if you have an older Docker version:
docker-compose -f docker-compose.dev.yml up --build
```

**Access the application at: http://localhost:4200**

That's it! No need to install Node.js, yarn, or any other dependencies.

## IDE Development Setup

**For IDE users (VSCode, Cursor, etc.)**: To avoid import errors and path resolution issues in your IDE, install packages locally even when using Docker:
```bash
# Install packages locally for IDE support
yarn install

# Then run Docker for development
./launch.sh
```

This gives you the best of both worlds: Docker for runtime isolation and local packages for IDE intellisense.

## Alternative: Manual Development Setup

If you prefer to run without Docker:

### Prerequisites

- Node.js 20.11.0+ (or use `nvm use` to automatically use the correct version)
- yarn package manager

### Installation
```bash
# Use the correct Node.js version (if using nvm)
nvm use

# Install dependencies
yarn install

# Run all services in development mode
yarn dev

# Or run individually:
yarn dev:client    # Client only (port 4200)
yarn dev:server    # Server only (port 9000)
```

## Overview

This application demonstrates a complete robotic visualization system with:

- **Real-time state synchronization** from server to client via WebSocket
- **Multiple control modes**: Direct joint control and forward kinematics via end-effector positioning
- **Physics-accurate motion modeling** with velocity and acceleration limits
- **Hardware-accelerated 3D rendering** using WebGL for fluid manipulator visualization

## Architecture

This is a TypeScript monorepo with three main packages optimized for performance and maintainability:

- **Client**: React 18 + Babylon.js for interactive UI and 3D rendering
- **Server**: High-throughput WebSocket server with manipulator control logic
- **Common**: Shared types, configurations, and protocol definitions

## Technologies & Design Decisions

### Client

- **React 18** with hooks for component state management
- **Babylon.js** for WebGL-based 3D manipulator rendering and scene management
- **TypeScript** for type safety and developer experience
- **Styled Components** for component-scoped styling
- **Ant Design** for production-ready UI components
- **Webpack** for optimized bundling
- **Custom hooks** for WebSocket communication and manipulator state management

### Server

- **Node.js** with TypeScript for consistent language across stack
- **ws library** chosen for reliable WebSocket communication with broad compatibility
- **Custom ManipulatorController** implementing realistic motion physics and constraints
- **Lightweight architecture** focusing on simplicity and performance

### Common Package

- **Centralized type definitions** for WebSocket protocol and manipulator state
- **Shared configuration** for scene parameters, manipulator dimensions, and constraints
- **Single source of truth** for domain logic to prevent duplication

## Troubleshooting

### Docker Issues
```bash
# If container won't start, check logs:
docker compose logs

# If port 9000 is in use:
docker compose down
# Or modify docker-compose.yml to use different port

# Clean build:
docker compose down --rmi all
docker compose up --build
```

### System Requirements

- **Docker**: Version 24.0 or higher
- **Available Memory**: 4GB minimum
- **Ports**: 4200 and 9000 must be available

### Known Issues

- **ws library compatibility**: This project uses standard ws library for cross-platform WebSocket support. Performance optimizations available through message batching and binary protocol encoding.

## Project Structure
```
├── client/            # React 18 + Babylon.js application
│   ├── src/
│   │   ├── components/    # Reusable UI components
│   │   ├── hooks/         # WebSocket and manipulator state logic
│   │   ├── pages/         # Main manipulator visualization page
│   │   └── utils/         # Babylon.js utilities and helpers
├── server/            # Node.js WebSocket server
│   ├── src/
│   │   ├── manipulator/   # ManipulatorController and physics simulation
│   │   ├── app.ts         # WebSocket server and protocol handling
│   │   └── utils/         # Server utilities
├── packages/common/   # Shared types and configurations
│   └── src/
│       ├── protocol/      # Message type definitions
│       ├── manipulator/   # Manipulator state and configuration types
│       └── config/        # Scene and physics constants
└── package.json       # Root workspace configuration
```


# PR Update: 2025-12-03 01:40:19
