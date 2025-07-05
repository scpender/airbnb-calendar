# Airbnb Calendar

A GitHub Pages hosted calendar application for managing multiple Airbnb property bookings with persistent event storage.

## Features

### Calendar Display
- **Tabbed Interface**: Separate tabs for each property with anchor-based navigation
- **Month View**: Default calendar view showing current month
- **Partial Day Events**: Multi-day bookings display as partial days (3pm start, 11am end) rather than blocking entire days
- **Multiple Sources**: Supports both Direct calendar URLs and Airbnb iCal feeds
- **Real-time Loading**: Fetches calendar data from multiple CORS proxy services for reliability

### Event Management
- **Duplicate Prevention**: Advanced duplicate detection using both event IDs and content matching
- **Historical Preservation**: Past events are stored permanently even when iCal feeds drop them
- **Phone Number Filtering**: Automatically filters out partial phone numbers from event titles
- **Source Indication**: Events are color-coded by source (Airbnb vs Direct)

### Server-Side Storage
- **GitHub API Integration**: Uses GitHub's REST API for persistent server-side storage
- **Cross-User Persistence**: Events are stored server-side and accessible to all users
- **Token Authentication**: Secure GitHub Personal Access Token support for write operations
- **Fallback Strategy**: Graceful fallback to localStorage when GitHub API is unavailable
- **Historical Data**: Maintains event history across sessions and users

## Configuration

### Property Setup
Edit `airbnb-config.json` to configure your properties:

```json
{
  "properties": [
    {
      "name": "Property Name",
      "directUrl": "https://calendar.google.com/calendar/ical/...",
      "airbnbUrl": "https://www.airbnb.com/calendar/ical/..."
    }
  ]
}
```

- `directUrl` can be empty if not available
- Each property gets its own tab with automatic URL anchoring

### GitHub Token Setup
For persistent storage across all users:

1. Create a GitHub Personal Access Token with repository write permissions
2. Visit the calendar and click "Save Token" when prompted
3. Enter your token to enable server-side storage

Without a token, the calendar falls back to localStorage (user-specific storage).

## Usage

### Live Application
Visit: https://scpender.github.io/airbnb-calendar/

### Direct Tab Access
Access specific properties directly via URL anchors:
- `https://scpender.github.io/airbnb-calendar/#property-name`

### Storage Options
- **With GitHub Token**: Events persist server-side for all users
- **Without Token**: Events stored locally per user/browser

## Technical Implementation

### Storage Architecture
- **Primary**: Server-side JSON storage via GitHub API
- **Backup**: localStorage fallback for reliability
- **Structure**: Organized by property with event deduplication

### CORS Handling
Multiple proxy services for reliable iCal feed access:
- AllOrigins.win
- CorsProxy.io
- CORS-Anywhere (as fallback)

### Event Processing
- **iCal Parsing**: Uses ical.js library for standard iCal format support
- **Time Adjustment**: Converts all-day events to partial days (3pm-11am)
- **Duplicate Detection**: Multi-layered deduplication by ID and content
- **Phone Filtering**: Removes partial phone number patterns from titles

## Development

### Local Development
1. Clone the repository
2. Serve files with any HTTP server (e.g., `python -m http.server`)
3. Access via localhost

### Deployment
Automatically deploys to GitHub Pages on push to master branch.

## Dependencies
- FullCalendar v6.1.9 (calendar display)
- ical.js v1.5.0 (iCal parsing)
- Native Fetch API (HTTP requests)
- GitHub REST API (storage)