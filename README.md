# Hello3DMCP Frontend - 3D Model Visualization

A 3D interactive model visualization built with Three.js that connects to an MCP server via WebSocket for remote control by AI assistants and other MCP clients.

## Features

- **Interactive 3D Model**: Rotate with mouse/touch, zoom with mouse wheel/pinch
- **Real-time Updates**: Changes made through MCP tools are instantly visible in the browser
- **WebSocket Communication**: Bidirectional communication with MCP server
- **Responsive Design**: Works on desktop and mobile devices

## Quick Start

### Prerequisites

- Node.js (v18 or higher)
- npm
- MCP server running (see [hello3dmcp-server](https://github.com/aidenlab/hello3dmcp-server) repository)

### Installation & Running

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Start the development server:**
   ```bash
   npm run dev
   ```
   Open `http://localhost:5173` in your browser

3. **Connect to MCP server:**
   - The app automatically connects to the WebSocket server at `ws://localhost:3001` by default
   - For production deployments, set the `VITE_WS_URL` environment variable to your WebSocket server URL
   - Include a `sessionId` query parameter in the URL to connect to a specific MCP session (e.g., `http://localhost:5173?sessionId=abc-123`)

## Building for Production

```bash
npm run build
```

This creates a `dist/` folder with optimized production files.

### Preview Production Build

```bash
npm run preview
```

## WebSocket Configuration

The frontend connects to the MCP server's WebSocket server for real-time communication.

### Development (Default)

- **WebSocket URL**: `ws://localhost:3001`
- Automatically detected when running locally

### Production

Set the `VITE_WS_URL` environment variable during build:

```bash
VITE_WS_URL=wss://your-server.com npm run build
```

Or configure in your deployment platform (Netlify, Vercel, etc.):

- **Netlify**: Add `VITE_WS_URL` to Site Settings → Environment Variables
- **Vercel**: Add `VITE_WS_URL` to Project Settings → Environment Variables

**Note**: Use `wss://` (secure WebSocket) for HTTPS deployments.

## Deployment

### Netlify

1. **Build Configuration:**
   - Build command: `npm run build`
   - Publish directory: `dist`

2. **Environment Variables:**
   - `VITE_WS_URL`: Your WebSocket server URL (e.g., `wss://your-server.com`)

3. **Deploy:**
   - Connect your repository to Netlify
   - Configure build settings
   - Set the `VITE_WS_URL` environment variable
   - Deploy

### Vercel

1. **Build Configuration:**
   - Build command: `npm run build`
   - Output directory: `dist`

2. **Environment Variables:**
   - `VITE_WS_URL`: Your WebSocket server URL (e.g., `wss://your-server.com`)

3. **Deploy:**
   - Connect your repository to Vercel
   - Configure build settings
   - Set the `VITE_WS_URL` environment variable
   - Deploy

## Project Structure

```
hello3dmcp-frontend/
├── src/
│   ├── Application.js         # Main app with WebSocket integration
│   ├── SceneManager.js         # Scene management with model manipulation
│   ├── WebSocketClient.js      # WebSocket client for server communication
│   ├── Model.js                # Model class definition
│   ├── CameraController.js     # Camera controls
│   ├── RotationController.js   # Rotation handling
│   ├── AreaLight.js            # Area light implementation
│   ├── RayPicker.js            # Ray picking for interactions
│   ├── InteractionModeManager.js # Interaction mode management
│   ├── constants.js            # Application constants
│   ├── main.js                 # Entry point
│   ├── style.scss              # Styles
│   └── utils/
│       ├── color/              # Color utilities
│       └── coordinates/        # Coordinate system utilities
├── public/
│   └── models/                 # 3D model assets
├── index.html                 # HTML entry point
├── vite.config.js             # Vite configuration
└── package.json               # Dependencies and scripts
```

## WebSocket Protocol

The frontend communicates with the MCP server using a WebSocket protocol:

### Session Registration

On connection, the frontend sends:
```json
{
  "type": "registerSession",
  "sessionId": "<session-id>"
}
```

### Receiving Commands

The frontend receives commands from the server:
- `changeColor` - Change model color
- `changeSize` - Change model size
- `scaleModel` - Scale model dimensions
- `changeBackgroundColor` - Change scene background
- `setKeyLightIntensity` - Set key light intensity
- `setKeyLightColor` - Set key light color
- And more... (see MCP server documentation)

### State Queries

The server may request current state:
```json
{
  "type": "requestState",
  "requestId": "<unique-id>",
  "forceRefresh": false
}
```

The frontend responds with:
```json
{
  "type": "stateResponse",
  "requestId": "<unique-id>",
  "state": { /* current state object */ }
}
```

## Development

### Environment Variables

- `VITE_WS_URL`: WebSocket server URL (optional, defaults to `ws://localhost:3001` for development)

### Local Development

1. Start the MCP server (see [hello3dmcp-server](https://github.com/aidenlab/hello3dmcp-server))
2. Start this frontend: `npm run dev`
3. Open browser to `http://localhost:5173?sessionId=<your-session-id>`

## Troubleshooting

### WebSocket Connection Issues

- Ensure MCP server is running on port 3001 (or your configured port)
- Check browser console for connection errors
- Verify `VITE_WS_URL` is set correctly for production deployments
- Ensure browser is connected with correct session ID

### Model Not Loading

- Check browser console for asset loading errors
- Verify `public/models/` directory contains model files
- Check network tab for failed requests

### Changes Not Visible

- Verify WebSocket connection is established (check browser console)
- Ensure browser is connected with correct session ID
- Check that MCP server is sending commands correctly

## Related Projects

- [hello3dmcp-server](https://github.com/aidenlab/hello3dmcp-server) - MCP server that controls this frontend

## License

MIT

## Contributing

Contributions welcome! Please feel free to submit a Pull Request.

