# Changelog

## 2025-06-30

### New Features

- Added **real-time data support for BESS (Battery Energy Storage Systems)**, enhancing visibility and operational insights.

### Improvements

- Refactored PV equipment logic for improved performance and maintainability.

### Bug Fixes

- Fixed an issue with the API endpoint for updating user projects in the admin module.

## 2025-06-23

### New Features

- **Custom Theming**: Added extensive theming capabilities to provide a more customized user experience.
- **KPI Portfolio Comparison**: KPI reports can now include portfolio-level comparisons, enabling broader performance analysis across multiple projects.

### Improvements

- **GIS Map**: The GIS map on the project Home page now features a new layer selector, making it easier to switch between different map views.
- **Project Calendar**: A Project calendar is now available for users to add project-related reporting deadlines, site visits and other items.
- **Performance**: Optimized the loading of pages that display open event counts.
- **BESS Analysis**: The date range selector for BESS Equipment Analysis has been updated for better usability.

### Bug Fixes

- **Navigation**: Corrected navigation links from the Equipment Analysis page to Project Energy KPI and from Uptime metrics to Data Browsing.
- **Real-Time Page**: Added missing axis labels to heatmaps for improved clarity.
- **Data Visualization**: Resolved an issue where plots would not update correctly after changing filter selections.
- **UI**: Fixed a minor visual glitch in the calendar display.

## 2025-06-16

### New Features

- **Real Time Page**: Added a x-axis labels to graphs on the real time page

### Bug Fixes

- **GIS Maps**: Hovering over a met station no longer shows PCS data

## 2025-06-09

### New Features

- **Weather & Forecast**: Improved display and added appropriate attribution to OpenWeatherMap.

### Bug Fixes

- **DC Amperage Report**: Improved error handling to display feedback to user if the report fails to complete.
- **CMMS Tab**: Corrected error handling that would cause the page to crash.

## 2025-06-02

### New Features

- **Inverter Availability Report**: A new report page has been added to monitor inverter availability across projects. The report is downloadable as an Excel file, allowing users to modify input parameters directly within the spreadsheet to perform custom analyses or generate updated availability metrics.

- **Admin User Management**: Introduced a comprehensive admin dashboard for managing users, assigning projects, and more.

- **CMMS Tab for All Projects**: All projects now have access to the CMMS tab, streamlining asset ticket management.

### Other Improvements

- Cleaned up UI in several report pages to improve clarity and consistency.
