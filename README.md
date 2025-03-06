# pretalx

pretalx is a conference planning and programme-management platform for handling the complete lifecycle of an event, from the initial Call for Participation to the published schedule and post-event speaker workflows.

It provides configurable submissions, collaborative reviews, scheduling, speaker communication, public agendas, feedback, APIs, and a plugin system for extending event-specific behavior.

## Overview

pretalx is designed for conferences, community events, meetups, festivals, and other multi-session events that need a structured process for collecting proposals and turning accepted submissions into a public programme.

The platform supports:

- Calls for Participation
- Speaker and submission management
- Review and selection workflows
- Multi-room and multi-day scheduling
- Public programme publishing
- Speaker communication
- Post-event feedback
- Extensible event-specific integrations

## Features

### Call for Participation

- Configurable submission forms
- Custom questions
- Tracks
- Session types
- Submission deadlines
- Speaker profiles
- Co-speaker support
- Draft submissions
- Confirmation workflows
- Invitation-based submissions
- Configurable public-facing content

### Review and Selection

- Collaborative review workflows
- Multiple reviewers
- Numeric scoring
- Text reviews
- Review phases
- Track-based review assignments
- Conflict handling
- Team permissions
- Submission acceptance and rejection
- Decision communication

### Scheduling

- Multi-day schedules
- Multiple rooms
- Session duration management
- Speaker availability
- Schedule versions
- Draft schedules
- Public schedule releases
- Per-speaker schedules
- Conflict detection
- Session movement and editing
- Public schedule frontend
- Schedule export formats

### Speaker Management

- Speaker accounts
- Speaker profiles
- Submission ownership
- Co-speaker invitations
- Confirmation requests
- Speaker communication
- Personal schedules
- Uploaded presentation files
- Post-event feedback

### Communication

- Templated emails
- Queued email delivery
- Recipient filtering
- Speaker notifications
- Submission notifications
- Schedule-related messages
- Review communication
- Event announcements

### Public Event Experience

- Published programme
- Session details
- Speaker profiles
- Track and room views
- Schedule filtering
- Time-zone-aware schedule display
- Public schedule widget
- Feedback forms
- Session exports
- Responsive event pages

### Platform Capabilities

- REST API
- API token authentication
- Internationalization
- Multi-event installations
- Role and team permissions
- Plugin framework
- Configurable event appearance
- QR code generation
- PDF generation
- Background task processing
- Redis-backed caching and locking
- Rate limiting
- Static and uploaded file handling
- Extensible event settings

## Tech Stack

| Area | Technologies |
| --- | --- |
| Language | Python 3.13 or later |
| Backend | Django 6 |
| API | Django REST Framework |
| API schema | drf-spectacular |
| Background processing | Celery 5.6 |
| Cache and locking | Redis |
| Production database | PostgreSQL 16 or later |
| Development database | SQLite |
| Templates | Django templates |
| Schedule frontend | Vue 3 |
| Frontend build | Vite 8 |
| Frontend localization | i18next |
| Styling | Stylus, Pug-based frontend templates |
| Markdown | Markdown, markdown-it |
| HTML sanitization | Bleach, DOMPurify |
| PDF generation | ReportLab |
| Images | Pillow |
| Static serving | WhiteNoise |
| Dependency management | uv |
| Task runner | just |
| Testing | Pytest, pytest-django, pytest-xdist |
| Quality tooling | Ruff, djangofmt, ESLint |
| Documentation | Sphinx |

## Installation

For local development, the repository provides a `just`-based setup workflow.

### Requirements

Install:

- Python 3.13 or later
- Git
- uv
- just
- Redis
- gettext
- A compiler and Python development headers where required
- npm for frontend development

PostgreSQL is optional for basic local development but recommended when testing production-style database behavior.

### Clone the Repository

```bash
git clone <repository>
cd pretalx
```

### Development Setup

The fastest development setup is:

```bash
just dev-setup
```

This workflow:

- Installs Python dependencies
- Installs frontend dependencies
- Collects static files
- Compiles translations
- Applies database migrations
- Creates an administrator
- Creates sample event data
- Starts the development server

### Manual Development Setup

Install all development dependencies:

```bash
just install-all
```

Or install only the main application and development extras:

```bash
just install --extra dev
```

Prepare static files:

```bash
just run collectstatic --noinput
```

Compile translations:

```bash
just run compilemessages
```

Apply migrations:

```bash
just run migrate
```

Create an administrator:

```bash
just run createsuperuser
```

Start the development server:

```bash
just run
```

The default `just run` command starts the pretalx development server.

### Start Redis

A Redis server should be available during development so caching, locking, and rate-limiting behavior matches production more closely.

For simple development without Celery configuration, long-running operations can execute synchronously.

## Usage

### Create an Event

1. Sign in with an administrator or organiser account.
2. Create a new event.
3. Configure the event name, dates, locale, and time zone.
4. Create tracks and session types.
5. Configure the Call for Participation.
6. Add submission questions.
7. Configure reviewer teams.
8. Open submissions.
9. Review and select proposals.
10. Build and release the schedule.

### Submission Workflow

A typical submission lifecycle is:

```text
Draft
  ↓
Submitted
  ↓
Review
  ↓
Accepted or Rejected
  ↓
Speaker Confirmation
  ↓
Scheduled
  ↓
Published
```

Organisers can adapt this workflow through event settings, review phases, permissions, and plugins.

### Review Workflow

1. Create a review team.
2. Assign tracks or submissions.
3. Configure review fields and scoring.
4. Allow reviewers to evaluate proposals.
5. Compare scores and written feedback.
6. Select accepted submissions.
7. Send decision emails to speakers.

### Schedule Workflow

1. Confirm accepted submissions.
2. Define rooms and event dates.
3. Review speaker availability.
4. Place sessions into schedule slots.
5. Resolve conflicts.
6. Save a schedule version.
7. Review the public programme.
8. Release the schedule.

Schedule versions allow organisers to continue editing without immediately changing the published programme.

## API

pretalx provides a REST API for integrations and event automation.

Typical API use cases include:

- Reading event information
- Reading schedules
- Accessing submissions
- Accessing speakers
- Managing rooms
- Reading tracks
- Exporting programme data
- Integrating event websites
- Synchronizing external tools
- Building mobile applications

Generate the local API schema during development with:

```bash
just api-docs
```

API credentials should be stored outside source-controlled files and limited to the permissions required by the integration.

## Plugins

pretalx provides a plugin system for extending event functionality without modifying core application code.

Plugins can add or modify:

- Event settings
- Submission workflows
- Schedule behavior
- Public pages
- Organiser views
- Navigation
- Forms
- API behavior
- Signals
- Email workflows
- Export formats
- Custom integrations

Install a local plugin in editable mode with:

```bash
just install-plugin /path/to/plugin
```

Restart the development server after enabling or changing plugin configuration.

## Configuration

Production pretalx installations use an INI-style configuration file.

A basic configuration contains filesystem, site, database, mail, Redis, and Celery settings.

### Filesystem

```ini
[filesystem]
data=/var/pretalx/data
static=/var/pretalx/static
```

The data directory stores uploaded and runtime files. The static directory contains collected application assets.

### Site

```ini
[site]
debug=False
url=your_public_application_origin
```

Production environments should disable debug mode and configure the correct externally accessible origin.

### Database

PostgreSQL is recommended for production:

```ini
[database]
backend=postgresql
name=pretalx
user=pretalx
password=replace_with_secure_password
host=localhost
port=5432
```

SQLite can be used for simple development installations.

### Redis

```ini
[redis]
location=redis://localhost:6379/1
```

Redis is used for:

- Caching
- Locking
- Rate limiting
- Shared application state

### Celery

```ini
[celery]
backend=redis://localhost:6379/2
broker=redis://localhost:6379/3
```

Celery should be configured only when workers are actually running.

Without a Celery configuration section, development environments can execute asynchronous work synchronously.

### Email

```ini
[mail]
from=conference@example.org
host=smtp.example.org
port=587
user=replace_with_smtp_user
password=replace_with_smtp_password
tls=True
ssl=False
```

Email delivery is required for normal speaker and organiser communication.

Keep SMTP credentials outside version control.

## Background Workers

Production installations should run Celery workers separately from the web application.

Start a development worker with:

```bash
just worker
```

Workers process tasks such as:

- Email delivery
- Image processing
- File cleanup
- Long-running event operations

A periodic task runner such as cron is also required for scheduled maintenance operations in production.

## Frontend Development

The repository contains Vue-based frontend applications for schedule management and public schedule functionality.

Install frontend dependencies:

```bash
just install-npm
```

Start the public schedule development environment:

```bash
just dev-schedule
```

Run a frontend npm script through the repository task runner:

```bash
just npm <script>
```

Build frontend assets:

```bash
just npm build
```

Run frontend linting:

```bash
just npm lint
```

Automatically fix supported frontend lint issues:

```bash
just npm lint:fix
```

## Common Development Commands

Start the development server:

```bash
just run
```

Run a Django management command:

```bash
just run <command>
```

Examples:

```bash
just run migrate
just run makemigrations
just run collectstatic --noinput
```

Open the pretalx-aware Django shell:

```bash
just shell
```

Start a Celery worker:

```bash
just worker
```

Update translation files:

```bash
just makemessages
```

Build API documentation:

```bash
just api-docs
```

Clean generated development artifacts:

```bash
just clean
```

## Testing

### Run Tests

```bash
just test
```

### Run Tests in Parallel

```bash
just test-parallel
```

### Run a Specific Test

```bash
just test path/to/test_file.py
```

### Run Tests with Coverage

```bash
just test-coverage
```

The project maintains strict automated test coverage requirements.

## Code Quality

Run repository formatting and automatic fixes:

```bash
just fmt
```

Check formatting and linting without modifying files:

```bash
just fmt-check
```

Run frontend linting:

```bash
just fmt-npm-check
```

Run most continuous-integration checks locally:

```bash
just ci
```

Quality checks include Python formatting, Python linting, Django-template formatting, frontend linting, translation compilation, package validation, and tests.

## Production Deployment

A production installation should include:

- Python 3.13 or newer
- PostgreSQL 16 or newer
- Redis
- Celery workers
- SMTP service
- Periodic task runner
- Reverse proxy
- HTTPS
- Persistent data storage
- Collected static files

Production deployments should:

- Run pretalx as an unprivileged user
- Disable debug mode
- Use PostgreSQL instead of SQLite
- Protect PostgreSQL and Redis from public access
- Use HTTPS through a reverse proxy
- Store uploaded files persistently
- Run Celery workers continuously
- Configure periodic tasks
- Back up the database and uploaded data
- Configure SMTP delivery
- Monitor worker failures
- Review plugin compatibility before upgrades
- Apply migrations during upgrades
- Rebuild static assets after package updates

## Contributing

Create a focused branch and follow the repository's backend, frontend, translation, testing, and plugin conventions.

Before submitting changes:

- Install the full development environment
- Add tests for new behavior and regression fixes
- Run targeted tests during development
- Run `just fmt-check`
- Run frontend linting when frontend code changes
- Keep user-facing text translatable
- Add migrations for database-model changes
- Preserve REST API compatibility where possible
- Update API schema output when contracts change
- Keep plugin extension points stable
- Avoid committing credentials or private event data
- Keep changes focused and clearly documented
