# stenoai reviewer notes

## Architecture
StenoAI is a cross-platform application designed to serve as a private AI-powered stenographer, primarily for macOS. It comprises a Python backend responsible for audio recording and processing, and an Electron-based frontend built with React and Vite. The project is neatly organized into directories for the app, backend, and resources, ensuring a clear separation of concerns.

## Conventions
- **File Structure**: The repository follows a structured layout with separate folders for the Electron app (`app/`), Python backend (`src/`), and utility scripts (`simple_recorder.py`). The `app/renderer/src/` directory contains all React components, while the app entry point is `main.js`.
- **Naming Conventions**: 
  - Python files use snake_case (e.g., `transcriber.py`, `summarizer.py`).
  - JavaScript/React files use PascalCase for component names (e.g., `App.tsx`, `MainToolbar.tsx`).
- **Code Style**: Python adheres to PEP 8 style guidelines with type hints and docstrings encouraged. JavaScript follows a consistent style with semicolons and prefers `const`/`let` over `var`. Linting is done using `ruff` for Python and `eslint` for JavaScript.
- **Electron Integration**: The IPC model is strict, ensuring the renderer only interacts with the main process through defined channels in `preload.js`, maintaining security and separation of concerns.
- **CSS Framework**: The frontend uses Tailwind CSS for styling, allowing responsive designs and utility-first classes, configured in `tailwind.config.cjs`.

## Intentional non-standard choices
- **Local AI Models**: The application prioritizes privacy by utilizing local AI models for transcription and summarization, indicating a conscious decision to minimize data leakage risks, which may seem unconventional for AI applications that often rely on cloud-based services.
- **Single-Instance Lock**: The app employs a single-instance lock with `app.requestSingleInstanceLock()` in `main.js`, which can seem limiting but ensures that only one instance of the app can run at a time, preventing unintended conflicts.

## Watch out for
- **Direct Manipulation of Environment Variables**: Be cautious with environment variables in the `main.js` file, especially the hard-coded API keys and URLs, which should be handled securely through a `.env` file or similar mechanism to avoid exposure in version control.
- **Uncaught Promise Rejections**: In multiple areas, such as while handling shortcut actions, check for uncaught promise rejections that could lead to application instability or unexpected behavior.
- **Failure to Handle Non-MacOS Systems**: While the application is targeted for macOS, be attentive to areas where it assumes macOS functionality, such as system audio capture or using Homebrew for dependencies, which should be clearly marked or handled gracefully in other environments.
- **Verbose Logging**: The application logs a large number of events and errors, which could clutter logs in production. A systematic approach to filtering log levels could improve maintainability and performance.