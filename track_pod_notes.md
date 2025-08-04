Kait211: where is this done in this branch
Right now, in the developer friendly JCC, we do not have the track pod functionality. Let’s add that back. I think a good location to store that code would be src/web/openlayers/controls since the track pod functionality should not need any React specific data. The goal would be to clean up the existing track pod code, place in in src/web/openlayers/controls/track-pod.ts and then that could would eventually be called from the settings panel where the track pod toggle will live.

what does this in current 2.y

GitHub Copilot: I'll help you investigate the track pod functionality in the current 2.y branch and understand how it's implemented. Let me search for track pod related code in the codebase. Now let me search more specifically for track pod functionality: Great! Now let me look at the SettingsPanel to see the current track pod implementation: Now let me look at the CommandControl trackBot function to understand the full implementation: Let me also look at the zoomToPod function: Let me also check the getPodExtent function: Now let me check if there's an existing `src/web/openlayers/controls` directory: Let me check how the current track pod functionality is handled from a React component perspective: Let me see how the doTracking function is called: Now let me look at some of the existing controls or similar patterns. Let me check if there are any existing controls in the web folder: Let me check the interactions file to see the current patterns: Now let me examine the JCC more closely to understand the current architecture. Let me check what imports are used for OpenLayers functionality: Based on my analysis of the codebase, I can now provide you with a comprehensive overview of how track pod functionality is currently implemented in the 2.y branch and what needs to be done to clean it up and move it to `src/web/openlayers/controls`.

## Current Track Pod Implementation in 2.y Branch

The track pod functionality is currently implemented across several files:

### 1. **SettingsPanel.tsx** - UI Component
- Contains the "Track Pod" toggle in the settings panel
- `handleTrackPodToggleClick()` function handles the toggle logic
- Calls `props.trackBot("pod")` when enabled, `props.trackBot(null)` when disabled

### 2. **CommandControl.tsx** - Main Logic
- **State Management**: `trackingTarget: number | string | null` in component state
- **Core Functions**:
  - `trackBot(id: number | string)`: Main function that handles tracking logic
  - `doTracking()`: Called in `componentDidUpdate()` to perform continuous tracking
  - `zoomToPod(firstMove = false)`: Zooms map to show entire pod
  - `getPodExtent()`: Calculates bounding box of all bots
  - `centerOn()`: Centers map on specific coordinates

### 3. **Current Implementation Flow**:
1. User clicks "Track Pod" toggle in settings
2. `handleTrackPodToggleClick()` called
3. Calls `trackBot("pod")` which:
   - Sets `trackingTarget` state to "pod"
   - Calls `zoomToPod(true)` for initial zoom
   - Shows "Following pod" info message
4. `doTracking()` is called on every state update via `componentDidUpdate()`
5. When tracking pod, `doTracking()` calls `zoomToPod()` to keep pod in view

## Recommended Refactoring Plan

To clean up and move this functionality to `src/web/openlayers/controls/track-pod.ts`:

### 1. **Create the Control Module**
```typescript
// src/web/openlayers/controls/track-pod.ts
export class TrackPodControl {
    private map: Map;
    private trackingTarget: number | string | null = null;
    private isTracking = false;
    
    constructor(map: Map) {
        this.map = map;
    }
    
    startTrackingPod() { /* ... */ }
    startTrackingBot(botId: number) { /* ... */ }
    stopTracking() { /* ... */ }
    private getPodExtent() { /* ... */ }
    private zoomToPod() { /* ... */ }
    private centerOnBot() { /* ... */ }
}
```

### 2. **Extract Pure Functions**
Move these functions from CommandControl to the new control:
- `getPodExtent()` - Calculate bounding box of all bots
- `zoomToPod()` - Zoom to show entire pod 
- `zoomToBot()` - Zoom to specific bot
- Core tracking logic from `doTracking()`

### 3. **Create Clean Interface**
The control should expose simple methods that the React component can call:
```typescript
// Simple API for React components
trackPodControl.startTrackingPod();
trackPodControl.startTrackingBot(botId);
trackPodControl.stopTracking();
trackPodControl.isCurrentlyTracking(); // boolean
trackPodControl.getCurrentTarget(); // "pod" | number | null
```

### 4. **Integration Points**
- Initialize the control in `createMap()` or CommandControl constructor
- Update tracking when bot positions change (call from bot status updates)
- Connect settings panel toggle to control methods
- Remove tracking logic from CommandControl's `componentDidUpdate()`

This refactoring would separate the mapping/OpenLayers concerns from React state management, making the code more modular and reusable.