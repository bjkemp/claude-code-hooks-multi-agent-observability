# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

**Start the full system:**
```bash
./scripts/start-system.sh
```

**Reset/stop system:**
```bash
./scripts/reset-system.sh
```

**Test system connectivity:**
```bash
./scripts/test-system.sh
```

**Server commands (apps/server/):**
```bash
bun install          # Install dependencies
bun dev              # Start development server with hot reload
bun start            # Start production server
bun run typecheck    # Type checking only
```

**Client commands (apps/client/):**
```bash
bun install          # Install dependencies
bun dev              # Start Vite dev server (port 5173)
bun run build        # Build for production
bun run preview      # Preview production build
```

## Architecture Overview

This is a multi-agent observability system that provides real-time monitoring of Claude Code agent behavior through comprehensive hook event tracking.

### Key Components

1. **Hook System (/.claude/hooks/)** - Python scripts using `uv` that intercept Claude Code lifecycle events
2. **Server (apps/server/)** - Bun TypeScript server with SQLite database and WebSocket support
3. **Client (apps/client/)** - Vue 3 TypeScript frontend with real-time visualization

### Data Flow
Claude Agents ’ Hook Scripts ’ HTTP POST ’ Bun Server ’ SQLite ’ WebSocket ’ Vue Client

### Technology Stack
- **Server**: Bun runtime, TypeScript, SQLite with WAL mode
- **Client**: Vue 3, TypeScript, Vite, Tailwind CSS
- **Hooks**: Python 3.8+, Astral uv package manager
- **Communication**: HTTP REST API, WebSocket for real-time updates

## Hook Integration

The `.claude` directory contains the complete hook system that can be copied to other projects. Key files:

- `settings.json` - Hook configuration mapping events to scripts
- `hooks/send_event.py` - Universal event sender to observability server
- Event-specific hooks: `pre_tool_use.py`, `post_tool_use.py`, `notification.py`, etc.

When integrating into new projects, update the `--source-app` parameter in `settings.json` to identify the project.

## Server Architecture

**Database**: SQLite with automatic migrations and WAL mode for concurrent access
**Endpoints**:
- `POST /events` - Receive events from agents
- `GET /events/recent` - Paginated event retrieval with filtering
- `GET /events/filter-options` - Available filter values
- `WS /stream` - Real-time event broadcasting

## Environment Configuration

Required environment variables in root `.env`:
- `ANTHROPIC_API_KEY` - Required for Claude Code
- `ENGINEER_NAME` - For logging/identification

Optional APIs:
- `GEMINI_API_KEY`, `OPENAI_API_KEY`, `ELEVEN_API_KEY`

Client configuration in `apps/client/.env`:
- `VITE_MAX_EVENTS_TO_DISPLAY=100` - Maximum events displayed

## Security Features

The hook system includes validation to block dangerous commands and prevent access to sensitive files. All inputs are validated before execution.

## Ports

- Server: 4000 (HTTP/WebSocket)
- Client: 5173 (Vite dev server)

## Use Bun Runtime

This project uses Bun as the JavaScript runtime. Always use `bun` instead of `node`, `npm`, or other package managers. The server at `apps/server/` has specific Bun configuration documented in its local CLAUDE.md file.